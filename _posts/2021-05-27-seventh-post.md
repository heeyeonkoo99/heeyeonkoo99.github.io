---
title: "[리액트 프로그래밍 정석] 리액트 고급 기술 따라하기"
date: 2021-05-27 08:26:28 -0400
categories: front-end react
url: https://heeyeonkoo99.github.io/front-end/
---
5장에선 **커링**과 **하이어오더 컴포넌트** 같은 중요 개념들이 나와서 이를 정리해보고자 본 글을 포스팅해본다.🤣
(~~책을 미리 한번 읽고나서 재독할때 코드를 따라쓰면서 공부내용을 온전히 내껄로 만드는 연습을 꾸준히 하자~~)

# 커링이란?
쉽게 말하자면 '함수를 반환하는 함수'이다. 커링을 사용하는 이유는 '함수의 재활용'때문이며 당연하게도 커링의 장점은 중복된 코드를 반복적으로 입력하지 않고 원하는 기능을 조합하여 적재적소에 사용할수 있다는 것이다.
- 커링 함수를 조합할때 커링 함수를 순서대로 조합하는 compose()함수를 사용한다.(for 코드 가독성)
- 실무에서 compose()함수를 어떻게 사용하는지는 아래의 코드내용을 보면 알수 있다.

```javascript
const multiply = (a, b) => a * b;
const add = (a, b) => a + b;

const multiplyX = x => a => multiply(a, 2);
const addX = x => a => add(x, a);
const addFour = addX(4);
const multiplyTwo = multiplyX(2);
const multiplyThree = multiplyX(3);

const formulaWithoutCompose = addX(4)(multiplyX(3)(multiplyX(2)));

const formula = x => addFour(multiplyThree(multiplyTwo(x)));
const formulaByReduce = [multiplyTwo, multiplyThree, addFour].reduce(
  function (prevFunc, nextFunc) {
    return function(value) {
      return nextFunc(prevFunc(value));
    }
  },
  function(k) { return k; }
);
const formulaByReduceResult = function(value) {
  return addFour(
    function(value) {
      return multiplyThree(
        function(value) {
          return multiplyTwo(
          (k => k)(value)
          );
        }(value)
      );
    }(value)
  );
};


function _compose(funcs) {
  return funcs.reduce(
    function (prevFunc, nextFunc) {
      return function() {
        const args = Array.prototype.slice.call(arguments);
        return nextFunc(prevFunc.call(this, args));
      }
    },
    function(k) {
      return k;
    },
  );
}

//실무에서 사용하는 함수 조합 기법 알아보기
/*
1.arguments를 사용하여 배열 인자 대신 나열형 인자로 함수 구조를 유연하게 만들기
-arguments는 javascript의 특수 변수로 함수 안에서 전달된 모든 인자 목록을 배열과 유사한 나열형자료(배열과 흡사하지만 객체 형태의 자료)로 저장해둔다. 여기선 배열함수 slice()를 사용하여
나열형 자료를 배열 형태로 변환했다.
2.arguments를 활용하여 하나의 실행 인자 value를 다중 인자로 사용 가능하게 확장하기
-앞선 에제에선 multiplyTwo,multiplyThree,addFour 모두 1개의 인자를 받아서 여러 인자를 처리하려면 이와 같은 방법을 쓴다.
=>아래코드는 1,2번 둘다 포함된것이다.
*/
function _composeWithArgs() {
  const funcs = Array.prototype.slice.call(arguments);
  return funcs.reduce(
    function (prevFunc, nextFunc) {
      return function() {
        const args = Array.prototype.slice.call(arguments);
        return nextFunc(prevFunc.call(this, args));
      }
    },
    function(k) {
      return k;
    },
  );
}

// 화살표 함수 표현식
export function compose(...funcs) {
  return funcs.reduce(
    (prevFunc, nextFunc) => (...args) => nextFunc(prevFunc(...args)),
    k => k
  );
}

// x
// => x * 2
// => (x * 2) * 3
// => ((x * 2) * 3) + 4
const formula = compose(
  multiplyX(2),
  multiplyX(3),
  addX(4),
);
```

# 하이어오더 컴포넌트
디자인패턴은 코드 중 활용도가 높은 구현방식ㅇ르 모아둔 비밀레시피와 같다. 앞서 공부한 커링도 디자인 패턴의 일종이며 리액트 컴포넌트에도 디자인 패턴을 적용할수 있다. 이번에 알아볼 디자인 패턴은 데코레이터 패턴이며 데코레이터 패턴을 적용하여 하이어오더 컴포넌트까지 개념을 확장해본다.    
- 데코레이터 패턴: 상속구조에서의 '기존구조를 해치지 않으면서 원하는 기능만 상속받는 것을 해결못함'이라는 단점을 보완하기 위해 데코레이터 패턴이 제안되었다. 클래스 간의 종속성 없이 기능만을 공유한다.    
-하이어오더 컴포넌트의 특징:
  - 함수나 클래스 형태의 컴포넌트를 모두 반환할수 있다.
  - 하이어오더 컴포넌트는 기존 컴포넌트를 확장한 컴포넌트이므로 기존 컴포넌트로 모든 프로퍼티를 전달해줘야 한다.
  
  ```javascript
  
function higherOrderComponent(Component){
    return function Enhanced(props){
        return <Component{...props}/>;
    }
}

function higherOrderComponent(Component){
    return class Enhanced extends React.Component{
        render(){
            return <Component{...this.props}/>;
        }
    }
}
  ```

### 참고하여 알아둘것!



* 참고한 사이트:         


* 참고한 블로그: <https://jwprogramming.tistory.com/68>    
        
     
    
**~~역시 공식사이트를 가야지 명확한 답을 얻을 수 있다.~~**



[jekyll-docs]: https://jekyllrb.com/docs/home
[jekyll-gh]:   https://github.com/jekyll/jekyll
[jekyll-talk]: https://talk.jekyllrb.com/

