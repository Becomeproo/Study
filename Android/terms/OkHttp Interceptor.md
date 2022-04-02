# OkHttp Interceptor
![image](https://user-images.githubusercontent.com/91411447/161375493-0cc66fce-fe0b-41b4-b612-13618b1f6a7f.png)

Retrofit은 내부적으로 OkHttp라는 Http 통신 라이브러리를 사용하고 있다.
윗 부분이 우리가 사용하고 있는 어플리케이션(Application), 아랫 부분이 서버라고 보면 된다.
Interceptor는 어플리케이션 내에서 OkHttp 코어 사이에서 요청/응답을 가로채는 역할을 하고 NetworkInterceptos는 실제 통신에서 서버와 OkHttp 코어 사이에서 요청/응답을 가로채는 역할을 한다.
***
출처: https://www.notion.so/OkHttp-Interceptor-33a164de43654a83b5ff0ab15b810865
