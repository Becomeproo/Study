# Service와 Thread의 차이
## Thread
* 앱이 사용자와 상호작용하는 과정에서 UI Thread가 Block 되지 않기 위한 작업 등을 처리하기 위한 Foreground 작업에 적합하다.
* Thread를 메인 스레드에서 처리를 하면 ANR이 발생하거나, 속도가 느려진다.

## Service
* 서비스를 실행한다는 것은 메인 스레드에서 실행을 의미
* 앱이 사용자와 상호작용하지 않아도 계속 수행되어야 하는 Background 작업에 적합하다.
* 서비스에서 무거운 작업을 처리를 할 경우, 서비스 내에 스레드를 생성해서 동작하는 걸 권장한다.
***
출처: https://www.notion.so/Android-Service-Thread-eee684b22ae847878f1920193fd05907
