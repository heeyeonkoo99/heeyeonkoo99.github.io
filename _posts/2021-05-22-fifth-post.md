---
title: "[리액트 프로그래밍 정석] React 중요개념 정리"
date: 2021-05-22 08:26:28 -0400
categories: front-end react
url: https://heeyeonkoo99.github.io/front-end/
---
# JSX는 무엇인가?
jsx는 Javascript를 확장한 문법으로 javascript의 모든 기능이 포함되어 있으며 React "엘리먼트(element)"를 생성한다. 언뜻보면 HTML같이 생겼지만 실제로는 JavaScript이다. 

```javascript
return <div>안녕하세요</div>;
```
리액트 컴포넌트 파일에서 XML형태로 코드를 작성하면 babel이 이 JSX를 JavaScript로 변환을 해준다.

> Babel 은 자바스크립트의 문법을 확장해주는 도구다. 아직 지원되지 않는 최신 문법이나, 편의상 사용하거나 실험적인 자바스크립트 문법들을 정식 자바스크립트 형태로 변환해줌으로서 구형 브라우저같은 환경에서도 제대로 실행 할 수 있게 해주는 역할을 한다.

-------
_JSX가 JavaScript로 제대로 변환되려면 지켜주어야 하는 몇가지 규칙이 있다. 다음 문법들을 준수하면 된다!_

- 태그는 꼭 닫혀있어야 한다.
- 태그와 태그 사이에 내용이 들어가지 않을 때에는 *self closing* 태그 라는 것을 사용해야 한다. 현재 Hello 컴포넌트를 사용할때도 self closing태그를 사용해줬다.

```javascript
import React from 'react';
import Hello from './Hello';

function App() {
  return (
    <div>
      <Hello />
      <Hello />
      <Hello />
      <input />
      <br />
    </div>
  );
}

export default App;
```

- 두 개 이상의 태그는 무조건 하나의 태그로 감싸져있어야 한다.=> ```<div></div>```로 하거나 리액트의 Fragment를 사용해서 해결한다.

```javascript
import React from 'react';
import Hello from './Hello';

function App() {
  return (
    <>
      <Hello />
      <div>안녕히계세요</div>
    </>
  );
}

export default App;
```
- JSX 내부에 자바스크립트 변수를 보여줘야 할때는 {}으로 감싸서 보여준다.

```javascript
import React from 'react';
import Hello from './Hello';

function App() {
  const name = 'react';
  return (
    <>
      <Hello />
      <div>{name}</div>
    </>
  );
}

export default App;

```

- JSX 에서 태그에 style 과 CSS class 를 설정하는 방법은 HTML 에서 설정하는 방법과 다르다. 우선, 인라인 스타일은 객체 형태로 작성을 해야 하며, background-color 처럼 - 로 구분되어 있는 이름들은 backgroundColor 처럼 camelCase 형태로 네이밍 해주어야 한다. 그리고, CSS class 를 설정 할 때에는 class= 가 아닌 className= 으로 설정을 해주어야 한다.
- 주석같은 경우는 ```{/* 이런 형태로 */}``` 작성한다. 아니면 ```//이런형태로``` 하면된다.


# XML은 무엇인가?
> XML의 개념

- Extensible Markup Language의 약자로 W3C 권고 확장성 있는 마크업 언어
- W3C가 인간과 응용프로그램간, 혹은 응용프로그램 간에 정보를 쉽게 교환하기 위해 만든 데이터 교환 포맷
- eXtensible: 데이터를 설명하는 태그(Tag)를 사용자 마음대로 정의할수 있다.

> XML과 HTML의 차이

- XML은 data를 전달하는 데에 포커스를 맞춘 언어
- HTML은 data를 표현하는 데에 포커스를 맞춘 언어
- XML은 HTML과 달리 tag가 미리 정의되어 있지 않다. 

# Virtual Dom은 무엇인가?
> Virtual Dom의 개념

 Virtual DOM (VDOM)은 UI의 이상적인 또는 “가상”적인 표현을 메모리에 저장하고 ReactDOM과 같은 라이브러리에 의해 “실제” DOM과 동기화하는 프로그래밍 개념입니다. 이 과정을 재조정이라고 합니다. 이 접근방식이 React의 선언적 API를 가능하게 합니다. React에게 원하는 UI의 상태를 알려주면 DOM이 그 상태와 일치하도록 합니다. 이러한 방식은 앱 구축에 사용해야 하는 어트리뷰트 조작, 이벤트 처리, 수동 DOM 업데이트를 추상화합니다. 

 “virtual DOM”은 특정 기술이라기보다는 패턴에 가깝기 때문에 사람들마다 의미하는 바가 다릅니다. React의 세계에서 “virtual DOM”이라는 용어는 보통 사용자 인터페이스를 나타내는 객체이기 때문에 React elements와 연관됩니다. 그러나 React는 컴포넌트 트리에 대한 추가 정보를 포함하기 위해 “fibers”라는 내부 객체를 사용합니다. 또한 React에서 “virtual DOM” 구현의 일부로 간주할 수 있습니다.


> 왜 쓰이는 거지?

 자 이제 DOM 을 조작했을 때 어떤 작업이 이뤄지는지 알겠죠? DOM에 변화생기면, 렌더트리를 재생성하고 (그러면 모든 요소들의 스타일이 다시 계산됩니다) 레이아웃을 만들고 페인팅을 하는 과정이 다시 반복되는거죠.복잡한 SPA(싱글 페이지 어플리케이션) 에서는 DOM 조작이 많이 발생해요. 그 뜻은 그 변화를 적용하기 위해 브라우저가 많이 연산을 해야한단 소리고, 전체적인 프로세스를 비효율적으로 만듭니다. 자, 이 이부분에서 Virtual DOM 이 빛을 발합니다! 만약에 뷰에 변화가 있다면, 그 변화는 실제 DOM 에 적용되기전에 가상의 DOM 에 먼저 적용시키고 그 최종적인 결과를 실제 DOM 으로 전달해줍니다. 이로써, 브라우저 내에서 발생하는 연산의 양을 줄이면서 성능이 개선되는 것 이지요.

 DOM 조작의 실제 문제는 각 조작이 레이아웃 변화, 트리 변화와 렌더링을 일으킨다는겁니다. 그래서, 예를 들어 여러분이 30개의 노드를 하나 하나 수정하면, 그 뜻은 30번의 (잠재적인) 레이아웃 재계산과 30번의 (잠재적인) 리렌더링을 초래한다는 것이죠.  Virtual DOM 은 그냥 뭐 엄청 새로운것도 아니고, 그냥 DOM 차원에서의 더블 버퍼링이랑 다름이 없는거에요. 변화가 일어나면 그걸 오프라인 DOM 트리에 적용시키죠. 이 DOM 트리는 렌더링도 되지 않기때문에 연산 비용이 적어요. 연산이 끝나고나면 그 최종적인 변화를 실제 DOM 에 던져주는거에요. 딱 한번만 한는거에요. 모든 변화를 하나로 묶어서. 

그러면, 레이아웃 계산과 리렌더링의 규모는 커지겠지만, 다시 한번 강조하자면 딱 한번만 하는거에요. 바로 이렇게, 하나로 묶어서 적용시키는것이, 연산의 횟수를 줄이는거구요. 사실, 이 과정은 Virtual DOM 이 없이도 이뤄질수 있어요. 그냥, 변화가 있을 때, 그 변화를 묶어서 DOM fragment 에 적용한 다음에 기존 DOM 에 던져주면 돼요.

그러면, Virtual DOM 이 해결 하려고 하는건 무엇이냐? 그 DOM fragment를 관리하는 과정을 수동으로 하나하나 작업 할 필요 없이, 자동화하고 추상화하는거에요. 그 뿐만 아니라, 만약에 이 작업을 여러분들이 직접 한다면, 기존 값 중 어떤게 바뀌었고 어떤게 바뀌지 않았는지 계속 파악하고 있어야하는데 (그렇지 않으면 수정 할 필요가 없는 DOM 트리도 업데이트를 하게 될 수도 있으니까요), 이것도 Virtual DOM 이 이걸 자동으로 해주는거에요. 어떤게 바뀌었는지 , 어떤게 바뀌지 않았는지 알아내주죠. 마지막으로, DOM 관리를 Virtual DOM 이 하도록 함으로써, 컴포넌트가 DOM 조작 요청을 할 때 다른 컴포넌트들과 상호작용을 하지 않아도 되고, 특정 DOM 을 조작할 것 이라던지, 이미 조작했다던지에 대한 정보를 공유 할 필요가 없습니다. 즉, 각 변화들의 동기화 작업을 거치지 않으면서도 모든 작업을 하나로 묶어줄 수 있다는거죠.


* 참고한 사이트: <https://ko.reactjs.org/docs/introducing-jsx.html> <https://facebook.github.io/jsx/> <https://react.vlpt.us/basic/04-jsx.html> <https://velopert.com/3236>
* 참고한 블로그: <https://helloworld-88.tistory.com/67> <https://ryublock.tistory.com/41>
**~~역시 공식사이트를 가야지 명확한 답을 얻을 수 있다.~~**



[jekyll-docs]: https://jekyllrb.com/docs/home
[jekyll-gh]:   https://github.com/jekyll/jekyll
[jekyll-talk]: https://talk.jekyllrb.com/
