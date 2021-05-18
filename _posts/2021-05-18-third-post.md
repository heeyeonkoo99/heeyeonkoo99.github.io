---
title: "Android 기본개념_면접준비1"
date: 2021-05-18 08:26:28 -0400
categories: jekyll update
---
#### 안드로이드의 실행환경에 대해 간단히 설명하시오.
> 안드로이드는 크게 4가지 실행환경으로 구성되어있습니다. 가장 하단부터 리눅스 커널, 라이브러리, 어플리케이션 프레임워크, 어플리케이션 순서입니다. 리눅스 커널은 OS로 안드로이드 스마트폰의 다양한 하드웨어(화면, 카메라, 블루투스, GPS, 메모리 등)를 관리합니다. 라이브러리는 안드로이드에 있는 다양한 기능(UI 처리, 미디어 프레임워크, 데이터베이스, 그래픽 처리, 웹 킷 등)을 소프트웨어적으로 구현해 놓은 환경 뿐만 아니라 안드로이드 앱을 구동해주는 dalvik 가상머신과 코어 라이브러리까지 포함하는 영역입니다. 어플리케이션 프레임워크는 사용자의 입력(액티비티, 윈도우, 컨텐츠, 뷰, 노티피케이션 등) 또는 특정한 이벤트에 따라 출력을 담당하는 환경을 말합니다. 마지막으로 실제로 동작하는 앱이 설치되는 환경인 어플리케이션이 있습니다.

#### 안드로이드는 다른 플랫폼에 비해 어떤 장점이 있는가?
> 첫째로 안드로이드를 구성하는 모든 소스가 오픈소스로 무료로 개방되어있어 비용적인 부담이 없습니다. 또한 전 세계의 수많은 개발자로부터 피드백을 받아 수정되기 때문에 안정성과 버그 수정이 빠릅니다. 그 밖에도 원한다면 소스를 다운 받아 수정해서 쓰기 편리합니다. 둘째로 자바를 주 언어로 사용하고 있기 때문에 많은 세계적으로 점유율이 높은 자바 개발자들이 쉽게 개발할 수 있습니다. 셋째로 리눅스 커널을 OS로 채택했기 때문에 다양한 하드웨어에 대한 드라이버 소스가 풍부합니다. 마지막으로 구글의 다양한 앱과 연동이 매우 편리하며 다른 플랫폼에 비해 앱간 연동에 너그러운편입니다.

#### 인플레이션(inflation)이란 무엇인가?
> xml 레이아웃 파일로 정의한 정보를 런타임에 setContentView 메소드가 호출됨에 따라 메모리 상에 객체로 만들어주는 과정을 말합니다. 이 과정에서 xml 레이아웃 파일에서 뷰에 id를 설정하고 해당 id가 R.java 파일에 주소 값으로 환원되며 findViewById 메소드와 id를 이용하여 코드 상으로 뷰 객체를 가져와 제어할 수 있습니다. ~~~인플레이션개념이 되게 중요해보이니까 잘알아두자.~~~

> 다시말해, 인플레이션이란 xml layout file에 비치된 resources들이 setContentView()나 LayoutInflator 객체등을 통해 메모리상에 실제로 객체화되어 어플리케이션에 보여지는 과정을 말합니다.  setContentView()호출 전에 R.id.button과 같이 xml 파일의 리소스를 참조하려하면 당연히 null exception이 뜬다. 왜냐면 아직 메모리에 xml 리소스가 객체화가 되어있지 않기때문에 참조할수 없기때문이다. LayoutInflater 객체는 setContentView가 xml파일의 모든 뷰들을 객체화시켜준다면, 일부 뷰만을 객체화시켜주는 것이다. LayoutInflater 클래스는 시스템 서비스로 제공되므로 사용하려면 시스템 서비스를 사용해야한다.

 ```
LayoutInflater inflater=(LayoutInflater)getSystemService(Context.LAYOUT_INFLATER_SERVICE);
inflater.inflate(R.layout.sub1,container,true);
```

```
@Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
         // activity_main 전체 인플레이션
        setContentView(R.layout.activity_main);
        // 컨텐츠를 추가할 레이아웃
        LinearLayout linearLayout = (LinearLayout) findViewById(R.id.linearLayout);
        // 레이아웃 인플레이터 객체
        LayoutInflater inflater = (LayoutInflater) getSystemService(Context.LAYOUT_INFLATER_SERVICE);
        // 인플레이터를 통해 button_layout 부분 인플레이션
        inflater.inflate(R.layout.button_layout, linearLayout, true);
    }
```



* 참고한 블로그: <https://inspire12.tistory.com/52> <https://bryant.tistory.com/80> <https://youngest-programming.tistory.com/96> <https://secuinfo.tistory.com/entry/Android-Develop-%EC%9D%B8%ED%94%8C%EB%A0%88%EC%9D%B4%EC%85%98Inflation>



[jekyll-docs]: https://jekyllrb.com/docs/home
[jekyll-gh]:   https://github.com/jekyll/jekyll
[jekyll-talk]: https://talk.jekyllrb.com/
