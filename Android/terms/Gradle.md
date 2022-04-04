# Gradle
유연성과 성능에 중점을 둔 오픈소스 빌드 자동화 도구이다. <br>
간단히 말해서 애플리케이션 빌드를 생성하는 자동화 도구이다.

## 안드로이드에서 Gradle의 역할
코드를 USB를 통해 에뮬레이터 또는 디바이스로 실행하려고 할 때마다 IDE에서 "Gradle Builld Running" 프로세스를 볼 수 있다. <br>
![image](https://user-images.githubusercontent.com/91411447/161459837-4e1ee5fe-e9a9-40ec-af99-b1e0f1bcd1dd.png)

Gradle이 자바 및 코틀린 코드를 APK로 컴파일하는데 도움이 되는 경우이다.

## 안드로이드 프로젝트의 여러 build.gradle 파일
new Android Project를 시작할 때마다 두 개의 서로 다른 build.gradle 파일이 있는 것을 볼 수 있다. <br>
하나는 프로젝트 수준이고 다른 하나는 앱 수준이다. 여기서 "앱"은 프로젝트의 모듈이다. 그렇다면 모듈이란 무엇일까?
> 모듈은 소스 파일 및 기능의 개발 단위로 프로젝트를 분할할 수 있도록 하는 빌드 설정의 모음이다.
> 프로젝트는 하나 이상의 모듈을 가질 수 있으며 하나의 모듈은 다른 모듈을 종속성으로 사용할 수 있다.
> 각 모듈은 독립적으로 구축, 테스트 및 디버깅할 수 있다.

모듈은 기능을 기반으로 하기 때문에 각 모듈에는 종속성 또는 타사 라이브러리가 있으므로 각 모듈에는 자체 gradle 파일이 있다.

### 프로젝트 레벨
![image](https://user-images.githubusercontent.com/91411447/161460050-3fabeaa7-193d-474c-aeab-1a81e9a68cdc.png)

1. 프로젝트의 종속성을 다운로드할 장소
2. 프로젝트 수준에서 필요한 종속성(모든 하위 프로젝트 또는 모듈에 유용), Gradle 종속성과 kotlin 종속성이 프로젝트 전체에서 사용되므로 여기로 선언
3. 모든 하위 프로젝트/모듈을 다운로드할 장소

### 앱 레벨
![image](https://user-images.githubusercontent.com/91411447/161460099-548cdff0-101c-4ae7-8077-be5edb4af0ab.png)

1. 필요한 플러그인을 추가한다. (코틀린 플러그인, 안드로이드 플러그인)
2. 모든 요구사항이 나열된 안드로이드 블록이다.
3. 요구사항에 따라 포함될 다양한 빌드 유형이다.
4. 모듈 또는 하위 프로젝트에 필요한 모든 타사 종속성

### Android Studio에서 빌드 프로세스가 시작되면 어떻게 될까?
![image](https://user-images.githubusercontent.com/91411447/161460192-c20926a2-2bbd-44d1-9eeb-4bb5f457dfa0.png)

* 컴파일러는 소스코드를 DEX(Dalvik Executable) 파일로 변환한다. 여기에는 안드로이드 기기에서 실행되는 바이트 코드와 그 밖의 모든 것이 컴파일 된 리소스도 포함된다.
* APK Packager는 DEX 파일과 컴파일 된 리소스를 단일 APK로 결합한다. 앱을 Android 기기에 설치하고 배포하려면 먼저 APK에 서명해야 한다.
* APK Packager는 디버그 또는 릴리즈 키 저장소를 사용하여 apk에 서명한다.
* 빌드 프로세스가 끝나면 외부 사용자에게 배포, 테스트 또는 출시하는데 사용할 수 있는 디버그/릴리즈 앱의 출리 apk가 있다.
***
출처: https://www.notion.so/Gradle-121a3b1d8f6849d3b9509a6463e90002
