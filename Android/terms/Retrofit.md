# Retrofit
Retrofit은 REST API 통신을 위해 구현된 라이브러리이디. 백그라운드에서 실행되며 callback을 통해 메인스레드에서의 UI 업데이트를 간단하게 할 수 있도록 제공하고 있다.

## 안드로이드의 통신 라이브러리
초기에 안드로이드 통신은 HttpClient를 사용했다. HttpClient에는 몇 가지 버그가 있어서 HttpUrlConnection이 권장되고 나서 쭈욱 사용하다 복잡했던 사용법으로 인해 Volley 라는 녀석이 나와 표준 라이브러리로 사용되었다. (Square의 OkHttp도 인기가 높고 많이 사용됨)

하지만 안드로이드 5.1에서 HttpClient가 Deprecated된 후, HttpClient에 의존하던 Volley도 Deprecated 되었다. Sqaure에서 만들어지 라이브러리인 OkHttp와 그 래퍼인 Retrofit은 하나의 선택처럼 필수가 되어버렸다.

## Retrofit을 사용해야 하는 이유
서버와 통신을 하려면 Http 통신을 해야한다. 기본적으로 HttpUrlConnection을 이용하면 매번 connection 설정 등 반복적인 작업이 발생한다. 이것을 도와주는 라이브러리가 OkHttp이다.

Rtrofit의 장점은 속도, 편의성, 가독성이 있다. OkHttp는 사용시 대게 Asynctask를 통해 비동기로 실행하게 되는데 성능상 느리다는 이슈가 있었다. Retrofit은 Asyntask를 사용하지 않고 자체적인 비동기 실행과 스레드 관리를 통해 속도를 훨씬 빠르게 끌어올렸다.

(Retrofit은 request, response 설정 등 반복적인 작업을 라이브러리에서 넘겨서 처리하므로 작업량이 줄어들고 사용하기 굉장히 편리하다.) <br>
***
출처: https://www.notion.so/Retrofit-781f29d5be064f1aa373b9342e956381
