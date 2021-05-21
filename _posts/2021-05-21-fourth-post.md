---
title: "[리액트 프로그래밍 정석] React ES6 문법 액기스"
date: 2021-05-21 08:26:28 -0400
categories: front-end react
url: https://heeyeonkoo99.github.io/front-end/
---
# 02-1. 템플릿 문자열
- 기존 자바스크립트에선 문자열과 문자열 또는 문자열과 변수를 연결하려면 병합연산자(+)를 사용했어야 했는데 ES6에선 템플릿 문자열을 도입하여 이를 대체했다. 
> 템플릿 문자열: 작은따옴표('') 대신 백틱(`)으로 문자열을 표현한다. 템플릿 문자열에 특수 기호 $를 사용하여 변수 또는 식을 포함할수도 있다. 

```javascript
// ES5 문법
var string1 = '안녕하세요';
var string2 = '반갑습니다';
var greeting = string1 + ' ' + string2;
var product = { name: '도서', price: '4200원' };
var message = '제품' + product.name + '의 가격은' + product.price + '입니다';
var multiLine = '문자열1\n문자열2';
var value1 = 1;
var value2 = 2;
var boolValue = false;
var operator1 = '곱셈값은 ' + value1 * value2 + '입니다. ';
var operator2 = '불리언값은 ' + (boolValue ? '참' : '거짓') + '입니다. ';

//ES6 문법
var string1 = '안녕하세요';
var string2 = '반갑습니다';
var greeting = `${string1} ${string2}`;

var product = { name: '도서', price: '4200원' };
var message = `제품 ${product.name}의 가격은 ${product.price}입니다`;
var multiLine = `문자열1
문자열2`;
var value1 = 1;
var value2 = 2;
var boolValue = false;
var operator1 = `곱셈값은 ${value1 * value2}입니다.`;
var operator2 = `불리언값은 ${boolValue ? '참' : '거짓'}입니다.`;
```

# 02-2. 전개 연산자
- ES6의 전개 연산자(spread operator)는 독특하면서도 매우 유용한 문법으로 나열형 자료를 추출하거나 연결할때 사용한다. 사용방법은 배열이나 객체, 변수명 앞에 마침표 세 개(...)를 입력한다. 다만, 배열,객체,함수 인자 표현식([],{},()) 안에서만 사용해야 한다.
> ES6의 배열 전개 연산자 사용 방법 
```javascript
// ES5 문법
var array1 = ['one', 'two'];
var array2 = ['three', 'four'];
var combined = [array1[0], array1[1], array2[0], array2[1]];
var combined = array1.concat(array2);
var combined = [].concat(array1, array2);
var first = array1[0];
var second = array1[1];
var three = array1[2] || 'empty';

function func() {
  var args = Array.prototype.slice.call(this, arguments);
  var first = args[0];
  var others = args.slice(1);
}

// ES6 문법
var array1 = ['one', 'two'];
var array2 = ['three', 'four'];
var combined = [...array1, ...array2];
// combined = ['one', 'two', 'three', 'four'];
var [first, second, three = 'empty', ...others] = array1;
// first = 'one', second = 'two', three = 'empty', others = []

function func(...args) {
  var [first, ...others] = args;
}

function func(first, ...others) {
  var firstInES6 = first;
  var othersInES6 = others;
}

// 올바르지 못한 예
// var wrongArr = ...array1;
```
> ES6의 객체 전개 연산자 사용 방법 
```javascript
// ES5 예제
var objectOne = { one: 1, two: 2, other: 0 };
var objectTwo = { three: 3, four: 4, other: -1 };

var combined = {
  one: objectOne.one,
  two: objectOne.two,
  three: objectTwo.three,
  four: objectTwo.four,
};
var combined = Object.assign({}, objectOne, objectTwo);
// combined = { one: 1, two: 2, three: 3, four: 4, other: -1}
var combined = Object.assign({}, objectOne, objectTwo);
// combined = { one: 1, two: 2, three: 3, four: 4, other: 0}
var others = Object.assign({}, combined);
delete others.other;

// ES6 예제
var combined = {
  ...objectOne,
  ...objectTwo,
};
// combined = { one: 1, two: 2, three: 3, four: 4, other: -1} 
var combined = {
  ...objectTwo,
  ...objectOne,
};
// combined = { one: 1, two: 2, three: 3, four: 4, other: 0}
var { other, ...others } = combined;
// others = { one: 1, two: 2, three: 3, four: 4}
```
# 02-3. 가변 변수와 불변 변수
- 기존 자바스크립트 문법은 변수 선언에 var 키워드를 사용했지만 ES6에서는 값을 수정할수 있는 가변 변수를 위한 let키워드와, 값을 수정할 수 없는 불변 변수를 위한 const 키워드를 사용한다. 
```javascript
const num = 1;
num = 3; // 타입 에러가 발생합니다

const str = '문자';
str = '새 문자'; // 타입 에러가 발생합니다

const arr = [];
arr = [1, 2, 3]; // 타입 에러가 발생합니다

const obj = {};
obj = { name: '내 이름' }; // 타입 에러가 발생합니다

const arr2 = [];
arr2.push(1); // arr2 = [1]
arr2.splice(0, 0, 0); // arr2 = [0,1]
arr2.pop(); // arr2 = [1]

const obj2 = {};
obj2['name'] = '내이름'; // obj2 = { name: '내이름' }
Object.assign(obj2, { name: '새이름' }); //obj2 = { name: '새이름' }
delete obj2.name; //obj2 = {}

const num1 = 1;
const num2 = num1 * 3; // num2 = 3

const str1 = '문자';
const str2 = str1 + '추가'; // str2 = '문자추가'

const arr3 = [];
const arr4 = arr3.concat(1); // arr4 = [1]
const arr5 = [...arr4, 2, 3]; // arr5 = [1, 2, 3]
const arr6 = arr5.slice(0, 1); // arr6 = [1], arr5 = [1, 2, 3]
const [first, ...arr7] = arr5; // arr7 = [2, 3], first = 1

const obj3 = { name: '내이름', age: 20 };
const obj4 = { ...obj3, name: '새이름' }; // obj4 = { name: '새이름', age: 20}
const { name, ...obj5 } = obj4; // obj5 = { age: 20 }

const arr = [1, 2, 3];
// 가변 변수를 사용한 예
for (let i = 0; i < arr.length; i++) {
  console.log(arr[i]);
}
// iterator 방식의 for-in 루프와 함께 불변 변수를 사용한 예
for (const item in arr) {
  console.log(item);
}

// forEach 함수를 활용한 예
arr.forEach((item, index) => {
  console.log(item);
  console.log(index);
});
```

# 02-4. 클래스
- 기존 자바스크립트 문법은 클래스 표현식이 없어서 prototype으로 클래스를 표현했다. 하지만 ES6는 클래스를 정의하여 사용할수 있다. 
```javascript
// ES5 문법
function Shape(x, y) {
  this.name = 'Shape';
  this.move(x, y);
}
// static 타입 선언 예제
Shape.create = function(x, y) {
  return new Shape(x, y);
};
Shape.prototype.move = function(x, y) {
  this.x = x;
  this.y = y;
};
Shape.prototype.area = function() {
  return 0;
};

// 혹은
Shape.prototype = {
  move: function(x, y) {
    this.x = x;
    this.y = y;
  },
  area: function() {
    return 0;
  },
};

var s = new Shape(0, 0);
var s2 = Shape.create(0, 0);
s.area(); // 0

function Circle(x, y, radius) {
  Shape.call(this, x, y);
  this.name = 'Circle';
  this.radius = radius;
}
Object.assign(Circle.prototype, Shape.prototype, {
  area: function() {
    return this.radius * this.radius;
  },
});

var c = new Circle(0, 0, 10);
c.area(); // 100

// ES6 예제
class Shape {
  static create(x, y) {
    return new Shape(x, y);
  }
  name = 'Shape';

  constructor(x, y) {
    this.move(x, y);
  }
  move(x, y) {
    this.x = x;
    this.y = y;
  }
  area() {
    return 0;
  }
}

var s = new Shape(0, 0);
var s1 = Shape.create(0, 0);
s.area(); // 0

class Circle extends Shape {
  constructor(x, y, radius) {
    super(x, y);
    this.radius = radius;
  }
  area() {
    if (this.radius === 0) return super.area();
    return this.radius * this.radius;
  }
}

var c = new Circle(0, 0, 10);
c.area(); // 100
```

# 02-5. 화살표 함수
- 화살표 함수는 화살표 기호=>로 함수를 선언하고 화살표 기둥 =을 사용하므로 fat arrow function이라고 부르기도 한다. 
```javascript
function add(first, second) {
  return first + second;
}

var add = function(first, second) {
  return first + second;
};

var add = function add(first, second) {
  return first + second;
};

// this scope를 전달한 예
var self = this;
var addThis = function(first, second) {
  return self.value + first + second;
};

var addThis = (first, second) => first + second;

function addNumber(num) {
  return function(value) {
    return num + value;
  };
}
// 화살표 함수로 변환한 예
var addNumber = num => value => num + value;

var addTwo = addNumber(2);
var result = addTwo(4); // 6

// bind함수를 통해 this scope를 전달한 예
class MyClass {
  value = 10;
  constructor() {
    var addThis2 = function(first, second) {
      return this.value + first + second;
    }.bind(this);
    var addThis3 = (first, second) => this.value + first + second;
  }
}
```

# 02-6. 객체 확장 표현식과 구조 분해 할당
> 객체확장 표현식 사용 방법 알아보기
```javascript
// ES5의 예
var x = 0;
var y = 0;

var obj = { x: x, y: y};

var randomKeyString = 'other';
var combined = {};
combined['one' + randomKeyString] = 'some value';

var obj2 = {
  methodA: function() { console.log('A'); },
  methodB: function() { return 0; },
};

// ES6의 예
var x = 0;
var y = 0;
var obj = { x, y };

var randomKeyString = 'other';
var combined = {
  ['one' + randomKeyString]: 'some value',
};

var obj2 = {
  x,
  methodA() { console.log('A'); },
  methodB() { return 0; },
};
```
> 구조 분해 사용 방법 알아보기
```javascript
// ES5 예제
var list = [0,1];
var item1 = list[0];
var item2 = list[1];
var item3 = list[2] || -1;

var temp = item2;
item2= item1;
item1 = temp;

var obj = {
  key1: 'one',
  key2: 'two',
};

var key1 = obj.key1;
var key2 = obj.key2;
var key3 = obj.key3 || 'default key3 value';
var newKey1 = key1;

// ES6 예제
var list = [0, 1];
var [
  item1,
  item2,
  item3 = -1,
] = list;
[item2, item1] = [item1, item2];

var obj = {
  key1: 'one',
  key2: 'two',
};
var {
  key1: newKey1,
  key2,
  key3 = 'default key3 value',
} = obj;

var [item1, ...otherItems] = [0, 1, 2];
var { key1, ...others } = { key1: 'one', key2: 'two' };
// otherItems = [1, 2]
// others = { key2: 'two' }
```

# 02-7. 라이브러리 의존성 관리
- ES6는 import 구문을 사용하여 script엘리먼트 없이 연결된 파일 및 의존 파일을 먼저 모두 내려 받고 코드를 구동하도록 변경한다.
```javascript
// CommonJS 방식
var module = require('./MyModule');
function Func() {
  module();
}
module.exports = new Func();

// RequireJS 방식
// define(['./MyModule'], function(module) {
//   function Func() {
//     module();
//   }
//   return new Func();
// });

// ES6 방식
// import MyModule from './MyModule';
// import { ModuleName } from './MyModule';
// import { ModuleName as RenamedModuleName } from './MyModule';

// function Func() {
//   MyModule();
// }
export const CONST_VALUE = 0;
export default new Func();
```
# 02-8. 배열 함수
- ES6에선 배열 함수인 forEach(),map(),reduce()함수가 추가되었다.
```javascript
const qs = 'banana=10&apple=20&orange=30';

function parse(qs) {
  var queryString = qs.substr(1); // querystring = 'banana=10&apple=20&orange=30'
  var chunks = queryString.split('&'); // chunks = ['banana=10', 'apple=20', 'orange=30']
  var result = {};
  for(var i = 0; i < chunks.length; i++) {
    var parts = chunks[i].split('=');
    var key = parts[0];
    var value = Number.isNaN(Number(parts[1])) ? parts[1] : Number(parts[1]);
    result[key] = value;
  }
  return result;
}

const params = parse(qs); // params = { banana: 10, apple: 10, orage: 30};

function parse(qs) {
  const queryString = qs.substr(1); // querystring = 'banana=10&apple=20&orange=30'
  const chunks = queryString.split('&'); // chunks = ['banana=10', 'apple=20', 'orange=30']
  let result = {};
  chunks.forEach((chunk) => {
    const [ key, value ] = chunk.split('='); // key = 'banana', value = '10'
    result[key] = value; // result = { banana: 10 }
  });
  return result;
}

function parse(qs) {
  const queryString = qs.substr(1);
  const chunks = queryString.split('&');
  const result = chunks.map((chunk) => {
    const [ key, value ] = chunk.split('='); // key = 'banana', value = '10'
    return { key: key, value: value }; // { key: 'banana', value: '10' }
  });
  return result;
  // result = [
  //  { key: 'banana', value: '10'},
  //  { key: 'apple', value: '20' },
  //  { key: 'orange', value: '30'}
  // ];
}

function sum(numbers) {
  return numbers.reduce((total, num) => total + num, 0);
}
sum([1, 2, 3, 4, 5, 6, 7, 8, 9, 10]) // 55

function parse(qs) {
  const queryString = qs.substr(1);
  const chunks = queryString.split('&');
  return chunks
    .map((chunk) => {
      const [ key, value ] = chunk.split('='); // key = 'banana', value = '10'
      return { key, value }; // { key: 'banana', value: '10' }
    })
    .reduce((result, item) => ({
      ...result,
      [item.key]: item.value,
    }), {});
}

function parse(qs) {
  const queryString = qs.substr(1);
  const chunks = queryString.split('&');

  // return chunks
  //   .map((chunk) => chunk.split('='))
  //   .map(([key, value]) =>({ key, value }))
  //   .reduce((result, [ key, value ]) => ({
  //     ...result,
  //     [key]: value,
  //   }), {});
  return chunks
    .map((chunk) => chunk.split('='))
    .reduce((result, [ key, value ]) => ({
      ...result,
      [key]: value,
    }), {});
}
```


# 02-9. 비동기 함수
- 기존의 콜백지옥을 벗어나기 위해서 만들었다는 후문이다.~~\*\*비동기 처리방식이 중요하니 다시한번 복습하록 하자.\*\*~~

# 02-10. 디바운스와 스로틀
> 디바운스(debounce): 어떤 내용을 입력하다가 특정 시간동안 대기하고 있으면 마지막에 입력된 내용을 입력하다가 특정시간 동안 대기하고 있으면 마지막에 입력된 내용을 바탕으로 서버 요청을 하는 방법이다. ex)연관검색어

> 스로틀(throttle): 디바운스 개념과 비슷하지만 '입력되는 동안에도 바로 이전에 요청한 작업을 주기적으로 실행한다는 점'이 다르다. ex)페이스북의 타임라인의 무한 스크롤 기능



* 참고한 : https://github.com/justinpark/justin-do-it-react/blob/master/src/02



[jekyll-docs]: https://jekyllrb.com/docs/home
[jekyll-gh]:   https://github.com/jekyll/jekyll
[jekyll-talk]: https://talk.jekyllrb.com/
