---
title: "Android 생명주기 정리"
date: 2021-05-15 08:26:28 -0400
categories: front-end android
url: https://heeyeonkoo99.github.io/front-end/
---
# 안드로이드 생명주기는 왜 있는걸까?
- 사용자가 앱을 탐색하고, 앱에서 나가고, 앱으로 다시 돌아가면, 앱의 Activity 인스턴스는 수명 주기 안에서 서로 다른 상태를 통해 전환된다. Activity 클래스는 활동이 상태 변화(시스템이 활동을 생성, 중단 또는 다시 시작하거나, 활동이 있는 프로세스를 종료하는 등)를 알아차릴 수 있는 여러 콜백을 제공한다.
- 사용자가 활동을 벗어났다가 다시 돌아왔을 때 활동이 작동하는 방식을 수명 주기 콜백 메서드에서 선언할 수 있는데 예를 들어 스트리밍 동영상 플레이어를 빌드하는 경우, 사용자가 다른 앱으로 전환할 때 동영상을 일시중지하고 네트워크 연결을 종료할 수 있다. 사용자가 돌아오면 네트워크를 다시 연결하고, 사용자가 일시중지한 지점에서 동영상을 다시 시작하도록 허용한다. 즉, 각 콜백은 상태 변화에 적합한 특정 작업을 실행할 수 있도록 한다. 적시에 알맞은 작업을 하고 적절하게 전환을 처리하면 앱이 더욱 안정적으로 기능할 수 있는데 예를 들어 수명 주기 콜백을 잘 구현하면 앱에서 다음과 같은 문제가 발생하지 않도록 예방하는 데 도움이 될 수 있다.

![image](https://user-images.githubusercontent.com/68431716/118345839-3aa93900-b572-11eb-9a5d-7b0890375d59.png)

# Activity 생명주기의 개념
- Activity 생명 주기 단계간에 전환하기 위해 활동 클래스는 6가지 콜백으로 구성된 핵심 집합의 onCreate(),onStart(),onResume(),onPause(),onStop(),onDestroy()를 제공한다.~~fragment도 생명주기가 있는데 좀있다 살펴보자.~~
- 
![image](https://user-images.githubusercontent.com/68431716/118346073-e1420980-b573-11eb-9c38-4840e72ca0ed.png)
- onCreate(): 이 콜백은 시스템이 먼저 활동을 생성할 때 실행되는 것으로, 필수적으로 구현해야 한다. 활동이 생성되면 생성됨 상태가 된다. onCreate() 메서드에서 활동의 전체 수명 주기 동안 한 번만 발생해야 하는 기본 애플리케이션 시작 로직을 실행하는데 예를 들어 onCreate()를 구현하면 데이터를 목록에 바인딩하고, 활동을 ViewModel과 연결하고, 일부 클래스 범위 변수를 인스턴스화할 수도 있다. 이 메서드는 savedInstanceState 매개변수를 수신하는데, 이는 활동의 이전 저장 상태가 포함된 Bundle 객체로, 이번에 처음 생성된 활동인 경우 Bundle 객체의 값은 null이다.

- onStart(): 활동이 시작됨 상태에 들어가면 시스템은 이 콜백을 호출한다. onStart()가 호출되면 활동이 사용자에게 표시되고, 앱은 활동을 포그라운데 보내 상호작용할수 있도록 준비하는데 예를들어 이 메서드에서 앱이 UI를 관리하는 코드를 초기화한다. onStart()메서드는 매우 빠르게 완료되고, onCreate()상태와 마찬가지로 이 상태에서 계속 머무르지 않고 이 콜백이 완료되면 onResume()상태로 들어가 메서드를 호출한다.

- onResume(): 이 상태에 들어갔을때 앱이 사용자와 상호작용하며, 어떤 이벤트가 발생하여 앱에서 포커스가 떠날때까지 앱이 이 상태에서 머무른다. 예를들어 전화가 오거나 사용자가 다른 활동으로 이동하거나, 기기 화면이 꺼지는 이벤트가 이에 해당한다. 방해되는 이벤트가 발생하면 활동은 일시중지됨 상태에 들어가고, 시스템이 onPause() 콜백을 호출한다. 활동이 일시중지됨 상태에서 재개됨 상태로 돌아오면 시스템이 onResume() 메서드를 다시 한번 호출하고, 따라서 onResume()을 구현하여 onPause() 중에 해제하는 구성요소를 초기화하고, 활동이 재개됨 상태로 전환될 때마다 필요한 다른 초기화 작업도 수행해야 한다.

- onPause(): 사용자가 활동을 떠나는것을 나타내는 첫 번째 신호로 이 메서드를 호출하며, 활동이 포그라운드에 있지 않게 됨을 나타낸다.~~(but, 사용자가 멀티 윈도우 모드에 있을 경우에는 여전히 표시될수 있음)~~ 이 상태로 들어가는 경우는 아래의 예시와 같다. onPause()는 아주 잠깐 실행되므로 저장 작업을 하기에는 시간이 부족할수 있기에 onPause()를 사용하여 애플리케이션 또는 사용자 데이터를 저장하거나, 네트워크 호출을 하거나, 데이터베이스 트랜잭션을 실행해서는 안 된다. 러한 작업은 메서드 실행이 끝나기 전에 완료되지 못할 수도 있습니다. 그 대신, 부하가 큰 종료 작업은 onStop() 상태일 때 실행해야 한다. 
    - 일부 이벤트가 앱 실행을 방해하는 경우, 가장 일반적인 사례다.
    - Android 7.0(API 수준 24) 이상에서는 여러 앱이 멀티 윈도우 모드에서 실행되는데 언제든지 그중 하나의 앱(창)만 포커스를 가질 수 있기 때문에 시스템이 그 외에 모든 다른 앱을 일시중지시킨다.
    - 새로운 반투명 활동(ex.대화상자)이 열리면 활동이 여전히 부분적으로 보이지만 포커스 상태가 아닌 경우에는 onPause()상태로 유지된다.
 
- onStop(): 활동이 사용자에게 더 이상 표시되지 않으면 중단됨 상태에 들어가고, 시스템은 onStop() 콜백을 호출한다. 이는 예를 들어 새로 시작된 활동이 화면 전체를 차지할 경우에 적용되며 시스템은 활동의 실행이 완료되어 종료될 시점에 onStop()을 호출할 수도 있다. 활동이 중단됨 상태로 전환하면 이 활동의 수명 주기와 연결된 모든 수명 주기 인식 구성요소는 ON_STOP 이벤트를 수신한다. 여기에서 수명 주기 구성요소는 구성요소가 화면에 보이지 않을 때 실행할 필요가 없는 기능을 모두 정지할 수 있다. onStop() 메서드에서는 앱이 사용자에게 보이지 않는 동안 앱은 필요하지 않은 리소스를 해제하거나 조정해야 한다. 예를 들어 앱은 애니메이션을 일시중지하거나, 세밀한 위치 업데이트에서 대략적인 위치 업데이트로 전환할 수 있다. onPause() 대신 onStop()을 사용하면 사용자가 멀티 윈도우 모드에서 활동을 보고 있더라도 UI 관련 작업이 계속 진행된다. 또한 onStop()을 사용하여 CPU를 비교적 많이 소모하는 종료 작업을 실행해야 한다. 예를 들어 정보를 데이터베이스에 저장할 적절한 시기를 찾지 못했다면 onStop() 상태일 때 저장할 수 있다.

- onDestroy(): 활동이 소멸되기 전에 호출한다. 아래와 같은 경우일때 이 상태로 들어간다.
    - (사용자가 활동을 완전히 닫거나 활동에서 finish()가 호출되어) 활동이 종료되는 경우
    - 구성 변경(예: 기기 회전 또는 멀티 윈도우 모드)으로 인해 시스템이 일시적으로 활동을 소멸시키는 경우

# Fragment 생명주기의 개념
- fragment는 고유 콜백 메서드가 더 있는데 바로 onAttach(), onCreateView(), onDestroyView(), onDetach() 가 바로 그것이다.

![image](https://user-images.githubusercontent.com/68431716/118346716-58799c80-b578-11eb-99ee-60d5ebe5fd6d.png)
- 

* 안드로이드 개발사이트: <https://developer.android.com/guide/components/activities/activity-lifecycle?hl=ko>
* 참고한 블로그: <https://ddangeun.tistory.com/50> <https://limkydev.tistory.com/41>



[jekyll-docs]: https://jekyllrb.com/docs/home
[jekyll-gh]:   https://github.com/jekyll/jekyll
[jekyll-talk]: https://talk.jekyllrb.com/
