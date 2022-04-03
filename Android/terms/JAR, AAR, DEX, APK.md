# JAR, AAR, DEX, APK
### JAR(Java Archive)
JAR는 해당 플랫폼에서 JAVA 응용 프로그램을 배포하기 위해 고안된 패키지 파일 형식이다. <br>
컴파일 된 Java 클래스 파일과 매니페스트와 같은 파일들이 포함된다. 기본적으로 .zip 아카이브 형태이다.

### AAR (Android Archive)
Android 라이브러리 프로젝트의 바이너리 배포판이다. <br>
주로 Java 클래스 파일들만 포함하는 Jar과 달리 리소스 파일들도 포함하고 있다.

### DEX(Dalvik Excutable)
DVM(Dalvik Virtual Machine)을 위한 실행 파일이다. JVM을 위한 .class 파일들과 같은 역할이다. <br>
Android SDK의 Dex 컴파일러에 의해 JVM 바이트코드를 DVM 바이트코드로 변환하고 모든 클래스 파일들을 DEX 파일에 넣는다. DEX는 바이너리 파일 형식으로 컴파일 된다.

### APK(Android Application Package)
APK는 Android 플랫폼에 배포할 수 있도록 설계된 파일 형식이다. <br>
컴파일 된 클래스를 Dex 파일형태로 포함시키고 AndroidManifest.xml 등 리소스 파일들도 포함된다.
***
출처: https://www.notion.so/JAR-AAR-DEX-APK-abe038e0307c40818392e0bebc182d4d
