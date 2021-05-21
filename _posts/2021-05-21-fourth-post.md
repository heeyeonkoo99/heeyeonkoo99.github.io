---
title: "React ES6 문법 액기스"
date: 2021-05-21 08:26:28 -0400
categories: front-end react
url: https://heeyeonkoo99.github.io/front-end/
---
# 02-1. 템플릿 문자열
- 사용자가 앱을 탐색하고, 앱에서 나가고, 앱으로 다시 돌아가면, 앱의 Activity 인스턴스는 수명 주기 안에서 서로 다른 상태를 통해 전환된다. Activity 클래스는 활동이 상태 변화(시스템이 활동을 생성, 중단 또는 다시 시작하거나, 활동이 있는 프로세스를 종료하는 등)를 알아차릴 수 있는 여러 콜백을 제공한다.
- 사용자가 활동을 벗어났다가 다시 돌아왔을 때 활동이 작동하는 방식을 수명 주기 콜백 메서드에서 선언할 수 있는데 예를 들어 스트리밍 동영상 플레이어를 빌드하는 경우, 사용자가 다른 앱으로 전환할 때 동영상을 일시중지하고 네트워크 연결을 종료할 수 있다. 사용자가 돌아오면 네트워크를 다시 연결하고, 사용자가 일시중지한 지점에서 동영상을 다시 시작하도록 허용한다. 즉, 각 콜백은 상태 변화에 적합한 특정 작업을 실행할 수 있도록 한다. 적시에 알맞은 작업을 하고 적절하게 전환을 처리하면 앱이 더욱 안정적으로 기능할 수 있는데 예를 들어 수명 주기 콜백을 잘 구현하면 앱에서 다음과 같은 문제가 발생하지 않도록 예방하는 데 도움이 될 수 있다.

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


# 02-3. 가변 변수와 불변 변수

# 02-4. 클래스

# 02-5. 화살표 함수

# 02-6. 객체 확장 표현식과 구조 분해 할당

# 02-7. 라이브러리 의존성 관리

# 02-8. 배열 함수

# 02-9. 비동기 함수

# 02-10. 디바운스와 스로틀



* 안드로이드 개발사이트: <https://developer.android.com/guide/components/activities/activity-lifecycle?hl=ko>
* 참고한 블로그: <https://ddangeun.tistory.com/50> <https://limkydev.tistory.com/41>



[jekyll-docs]: https://jekyllrb.com/docs/home
[jekyll-gh]:   https://github.com/jekyll/jekyll
[jekyll-talk]: https://talk.jekyllrb.com/
