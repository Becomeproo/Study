# Intent와 Bundle의 용도
인텐트는 저장이 아닌 전달하는 수단으로의 객체이고, 번들은 상태/값 등을 저장하기 위한 객체이다. 번들의 보관은 shut down 후 다시 초기화할 때 보관할 수 있다. 예를 들어, 메모리 부족으로 shut down 되는 경우, 디바이스를 가로, 세로 모드로 바꿀 때 shut down되는 경우, create에서 bundle saveinstancestate를 보면 처음 oncreate()일 때 bundle 값인 saveinstancestate는 null 값이 되고 저장했을 때 저장값을 갖게 된다.
***
출처: https://www.notion.so/imwj/Android-Interview-3ce7ddf12ddb413a9d2213173654d52c
