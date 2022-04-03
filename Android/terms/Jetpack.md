# Jetpack
## JetPack이란
안드로이드 앱을 구축하는데 도움이 되는 도구 모음이다. <br>
즉, Jetpack은 안드로이드 개발자들이 더욱 쉽게 높은 퀄리티의 앱을 개발할 수 있도록 도와주는 라이브러리들의 모음집이다. <br>
Jetpack은 크게 4가지로 분류할 수 있다.
* Foundation Components
* Architecture Components
* Behavior Components
* UI Components

### Foundation Components
기본 컴포넌트들 다음을 제공한다.
* Backward compatibility
* Testing
* Kotlin language support

전체 컴포넌트
* App Compat
  안드로이드의 오래된 버전(support)과 함께 머터리얼 디자인을 함께 지원한다.
* Android KTX
  코틀린의 extensions을 간결하게 쓰고, 직관적으로 보여지게 한다.
* Multidex
  앱에 대한 여러 dex files를 지원
* Test
  단위 및 런타임 UI테스트를 위한 프레임워크
  
### Architecture Components
아키텍처 컴포넌트는 앱을 구축하는데 도움을 준다.
* Robust Apps
* Testable Apps
* Maintainable Apps

모든 구성요소의 아키텍처
* Data Binding
  레이아웃의 UI요소를 앱의 데이터 소스에 선언적으로 바인딩
* Lifecycles
  액티비티, 프래그먼트의 Lifecycles을 관리
* LiveData
  변경사항이 있으면 view에게 알린다.
* Navigation
  앱 네비게이션의 필요한 모든것을 핸들링 한다.
* Paging
  데이터소스의 필요에 따라 순차적으로 정보를 불러온다.
* Room
  SQLite 데이터베이스를 접근
* ViewModel
  Lifecycle에 관련된 UI 데이터를 관리한다.
* WorkManager
  안드로이드 백그라운드 작업을 관리한다.
  
***
출처: https://www.notion.so/Android-Jetpack-ba7fc5af7422453e87a1f8354fdba7b2
