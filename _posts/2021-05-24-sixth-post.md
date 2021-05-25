---
title: "[리액트 프로그래밍 정석] 모듈 내보내고 가져오기"
date: 2021-05-22 08:26:28 -0400
categories: front-end react
url: https://heeyeonkoo99.github.io/front-end/
---
export와 import 지시자는 다양한 방식으로 활용된다. export 문은 JavaScript 모듈에서 함수, 객체, 원시 값을 내보낼 때 사용되며 내보낸 값은 다른 프로그램에서 import 문으로 가져가 사용할 수 있다. 여기서 import와 require의 차이, 그리고 export,exports,export default, module.exports 등 여러가지가 있기에 나도 한번 정리해보려 한다.

# 선언부 앞에 export 붙이기
변수나 함수, 클래스를 선언할 때 맨 앞에 export를 붙이면 내보내기가 가능하며 아래 내보내기는 모두 유효하다.

```javascript
// 배열 내보내기
export let months = ['Jan', 'Feb', 'Mar','Apr', 'Aug', 'Sep', 'Oct', 'Nov', 'Dec'];

// 상수 내보내기
export const MODULES_BECAME_STANDARD_YEAR = 2015;

// 클래스 내보내기
export class User {
  constructor(name) {
    this.name = name;
  }
}
```
 > **클래스나 함수를 내보낼 땐 세미콜론을 붙이지 않는다!**    
클래스나 함수 선언 시, 선언부 앞에 export를 붙인다고 해서 함수 선언 방식이 함수 선언문에서 함수 표현식(function expression) 으로 바뀌지 않는다. 내보내 지긴 했지만 여전히 함수 선언문이다. 대부분의 자바스크립트 스타일 가이드는 함수나 클래스 선언 끝에 세미콜론을 붙이지 말라고 권유하며 같은 이유로 export class나 export function 끝에 세미콜론을 붙이지 않는다.

```javascript
export function sayHi(user) {
  alert(`Hello, ${user}!`);
}  // 끝에 ;(세미콜론)을 붙이지 않습니다.
```
     
# 선언부 떨어진 곳에 export 붙이기

```javascript
// 📁 say.js
function sayHi(user) {
  alert(`Hello, ${user}!`);
}

function sayBye(user) {
  alert(`Bye, ${user}!`);
}

export {sayHi, sayBye}; // 두 함수를 내보냄
```
     
# import *
무언갈 가져오고 싶다면 아래와 같이 이에 대한 목록을 만들어 import {...}안에 적어주면 되며, 가져올 것이 많으면 import * as <obj> 처럼 객체 형태로 원하는 것들을 가지고 올 수 있다. 
  
```javascript
// 📁 main.js
import {sayHi, sayBye} from './say.js';

sayHi('John'); // Hello, John!
sayBye('John'); // Bye, John!
```
```javascript
// 📁 main.js
import * as say from './say.js';

say.sayHi('John');
say.sayBye('John');
```
 > **어떤걸 가져올때 구체적으로 명시하는게 좋다!**  
물론 '한꺼번에 모든걸 가져오는 방식'을 사용하면 코드가 짧아져서 좋지만 구체적으로 명시한는게 좋은 이유는 다음과 같다.    
> 1. 웹팩(webpack)과 같은 모던 빌드 툴은 로딩 속도를 높이기 위해 모듈들을 한데 모으는 번들링과 최적화를 수행한다. 이 과정에서 사용하지 않는 리소스가 삭제되기도 한다.아래와 같이 프로젝트에 서드파티 라이브러리인 say.js를 도입하였다 가정해보자. 이 라이브러리엔 수 많은 함수가 있다.    
```javascript
// 📁 say.js
export function sayHi() { ... }
export function sayBye() { ... }
export function becomeSilent() { ... }
```
>   현재로선 say.js의 수 많은 함수 중 단 하나만 필요하기 때문에, 이 함수만 가져와 본다.
```javascript
// 📁 main.js
import {sayHi} from './say.js';
```
>   빌드 툴은 실제 사용되는 함수가 무엇인지 파악해, 그렇지 않은 함수는 최종 번들링 결과물에 포함하지 않는다. 이 과정에서 불필요한 코드가 제거되기 때문에 빌드 결과물의 크기가 작아지며 이런 최적화 과정은 ```'가지치기(tree-shaking)'``` 라고 불린다.    
>   2. 어떤 걸 가지고 올지 명시하면 이름을 간결하게 써줄 수 있다. 예를 들어 say.sayHi()보다 sayHi()가 더 간결하다.     
>   3. 어디서 어떤 게 쓰이는지 명확하기 때문에 코드 구조를 파악하기가 쉬워 리팩토링이나 유지보수에 도움이 된다.
    
# import ‘as’ 와 Export ‘as’    
as를 사용하면 이름을 바꿔서 모듈을 가져올 수 있다. 먼저 import에서  sayHi를 hi로, sayBye를 bye로 이름을 바꿔서 가져와 보자.
```javascript
// 📁 main.js
import {sayHi as hi, sayBye as bye} from './say.js';

hi('John'); // Hello, John!
bye('John'); // Bye, John!
```
마찬가지로 export에도 as를 사용할 수 있다. sayHi와 sayBye를 각각 hi와 bye로 이름을 바꿔 내보내 보자. 이제 다른 모듈에서 이 함수들을 가져올 때 이름은 hi와 bye가 된다.
```javascript
// 📁 say.js
...
export {sayHi as hi, sayBye as bye};
```
```javascript
// 📁 main.js
import * as say from './say.js';

say.hi('John'); // Hello, John!
say.bye('John'); // Bye, John!
```
    
# export default
모듈은 크게 두 종류로 나뉜다.  
```
1. 복수의 함수가 있는 라이브러리 형태의 모듈(위 예시의 say.js)    
2. 개체 하나만 선언되어있는 모듈(아래의 user.js. class User 하나만 내보내기 함)    
```

대개는 두 번째 방식으로 모듈을 만드는 걸 선호하기 때문에 함수, 클래스, 변수 등의 개체는 전용 모듈 안에 구현된다. 그런데 이렇게 모듈을 만들다 보면 자연스레 파일 개수가 많아질 수밖에 없다. 그렇더라도 모듈 이름을 잘 지어주고, 폴더에 파일을 잘 나눠 프로젝트를 구성하면 코드 탐색이 어렵지 않으므로 이는 전혀 문제가 되지 않는다.    
모듈은 export default라는 특별한 문법을 지원한다. export default를 사용하면 '해당 모듈엔 개체가 하나만 있다’는 사실을 명확히 나태낼 수 있다. 내보내고자 하는 개체 앞에 export default를 붙여준다.
```javascript
// 📁 user.js
export default class User { // export 옆에 'default'를 추가해보았습니다.
  constructor(name) {
    this.name = name;
  }
}
```
파일 하나엔 대개 export default가 하나만 있다. 이렇게 default를 붙여서 모듈을 내보내면 중괄호 {} 없이 모듈을 가져올 수 있다.
```javascript
// 📁 main.js
import User from './user.js'; // {User}가 아닌 User로 클래스를 가져왔습니다.

new User('John');
```
중괄호 없이 클래스를 가져오니 더 깔끔해 보인다.~~개발자들은 깔끔한 코드를 좋아하는게 확실하다..!~~ 모듈을 막 배우기 시작한 사람은 중괄호를 빼먹는 실수를 자주 하니까 named export 한 모듈을 가져오려면 중괄호가 필요하고, default export 한 모듈을 가져오려면 중괄호가 필요하지 않다는 걸 기억해놓자.
![image](https://user-images.githubusercontent.com/68431716/119430733-42c06000-bd4c-11eb-8b77-abd529d5158e.png)

사실 named export와 default export를 같은 모듈에서 동시에 사용해도 문제는 없다. 그런데 실무에선 이렇게 섞어 쓰는 사례가 흔치 않다. 한 파일엔 named export나 default export 둘 중 하나만 사용하는경우가 일반적으로 보여지는듯하다. 여기서 중요한 사실은 파일당 최대 하나의 default export가 있을 수 있으므로 내보낼 개체엔 이름이 없어도 괜찮다는 것이다.    
아래 예시의 개체엔 이름이 없지만 모두 에러 없이 잘 동작한다.
```javascript
export default class { // 클래스 이름이 없음
  constructor() { ... }
}
```
```javascript
export default function(user) { // 함수 이름이 없음
  alert(`Hello, ${user}!`);
}
```
```javascript
// 이름 없이 배열 형태의 값을 내보냄
export default ['Jan', 'Feb', 'Mar','Apr', 'Aug', 'Sep', 'Oct', 'Nov', 'Dec'];
```
export default는 파일당 하나만 있으므로 이 개체를 가져오기 하려는 모듈에선 중괄호 없이도 어떤 개체를 가지고 올지 정확히 알 수 있으므로 이름이 없어도 괜찮다. 그렇지만 default를 붙이지 않았다면 개체에 이름이 없는 경우 에러가 발생한다.
```javascript
export class { // 에러! (default export가 아닌 경우엔 이름이 꼭 필요합니다.)
  constructor() {}
}
```
> **'default' name에 대해서**     
> default 키워드는 기본 내보내기를 참조하는 용도로 종종 사용된다. 함수를 내보낼 때 아래와 같이 함수 선언부와 떨어진 곳에서 default 키워드를 사용하면, 해당 함수를 기본 내보내기 할 수 있다.    ```javascript
function sayHi(user) {
  alert(`Hello, ${user}!`);
}

// 함수 선언부 앞에 'export default'를 붙여준 것과 동일합니다.
export {sayHi as default};
```
>   흔치 않지만 user.js라는 모듈에 ‘default’ export 하나와 다수의 named export가 있다고 해보자.    
```javascript
// 📁 user.js
export default class User {
  constructor(name) {
    this.name = name;
  }
}

export function sayHi(user) {
  alert(`Hello, ${user}!`);
}
```
> 아래와 같은 방식을 사용하면 default export와 named export를 동시에 가져올 수 있다.       
```javascript
// 📁 main.js
import {default as User, sayHi} from './user.js';

new User('John');
```

> "*" 를 사용해 모든 것을 객체 형태로 가져오는 방법도 있는데, 이 경우엔 default 프로퍼티는 정확히 default export를 가리킨다.    
```javascript
// 📁 main.js
import * as user from './user.js';

let User = user.default; // default export
new User('John');
```    

> **default export의 이름에 관한 규칙**    
>  named export는 내보냈을 때 사용한 이름 그대로 가져오므로 관련 정보를 파악하기 쉽다.그런데 아래와 같이 내보내기 할 때 쓴 이름과 가져오기 할 때 쓸 이름이 동일해야 한다는 제약이 있다..그렇지만 named export와 다르게 default export는 가져오기 할 때 개발자가 원하는 대로 이름을 지정해 줄 수 있다. But 그런데 이렇게 자유롭게 이름을 짓다 보면 같은 걸 가져오는데도 이름이 달라 혼란의 여지가 생길 수 있기에 코드의 일관성을 유지하기 위해 default export 한 것을 가져올 땐 아래와 같이 파일 이름과 동일한 이름을 사용하도록 팀원끼리 내부 규칙을 정할 수 있다.
```javascript
import {User} from './user.js';
// import {MyUser}은 사용할 수 없습니다. 반드시 {User}이어야 합니다.
```
```javascript
import User from './user.js'; // 동작
import MyUser from './user.js'; // 동작
// 어떤 이름이든 에러 없이 동작합니다.
```
```javascript
import User from './user.js';
import LoginForm from './loginForm.js';
import func from '/path/to/func.js';
...
```
    
# 모듈 다시 내보내기
export ... from ... 문법을 사용하면 가져온 개체를 즉시 ‘다시 내보내기(re-export)’ 할 수 있다. 이름을 바꿔서 다시 내보낼 수 있는 것이다.

```javascript
export {sayHi} from './say.js'; // sayHi를 다시 내보내기 함

export {default as User} from './user.js'; // default export를 다시 내보내기 함
```
```javascript
auth/
    index.js
    user.js
    helpers.js
    tests/
        login.js
    providers/
        github.js
        facebook.js
        ...
```
```javascript
// 📁 auth/index.js

// login과 logout을 가지고 온 후 바로 내보냅니다.
import {login, logout} from './helpers.js';
export {login, logout};

// User를 가져온 후 바로 내보냅니다.
import User from './user.js';
export {User};
...
```
    
---------------------------------------
    
### 참고하여 알아둘것!    
-export는 내보낼 변수 하나하나의 값을 선언하여 내보내며 exports는 exports객체를 내보내므로 객체의 프로퍼티 형태로 내보낸다.     
-require와 import의 차이는 require는 내보낸 값이 module 객체에 담겨오며, import는 값 자체를 가져온다.

* 참고한 사이트:         
<https://ko.javascript.info/import-export>     
<https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Statements/export>    

* 참고한 블로그:         
<https://velog.io/@jch9537/Javascript-export-default>     
    
**~~역시 공식사이트를 가야지 명확한 답을 얻을 수 있다.~~**



[jekyll-docs]: https://jekyllrb.com/docs/home
[jekyll-gh]:   https://github.com/jekyll/jekyll
[jekyll-talk]: https://talk.jekyllrb.com/

