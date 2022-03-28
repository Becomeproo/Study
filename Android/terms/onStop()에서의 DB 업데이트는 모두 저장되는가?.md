# onStop()에서의 DB 업데이트는 모두 저장되는가?
몇몇 경우엔 onStop()이 호출되지 않을 수 있다. 메모리 부족이나 configuration changes인 경우, onStop()에 도달하기 전에 android가 강제로 어플리케이션을 종료할 수 있다.
(사용자가 back버튼을 누를 경우, onSaveInstatnceState가 호출되지 않기 때문에 모든 DB 테이블을 onSaveInstanceState에 저장할 수 없다.)
***
출처: https://www.notion.so/imwj/Android-Interview-3ce7ddf12ddb413a9d2213173654d52c
