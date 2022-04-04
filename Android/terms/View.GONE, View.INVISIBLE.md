# View.GONE, View.INVISIBLE
* INVISIBLE: 뷰를 그려놓고 보이지는 않지만 레이아웃에 공간을 차지하고 있음
* GONE: 어댑터에 getView()가 호출되지 않아 뷰를 그리지 않고 레이아웃에 공간을 차지하고 있지 않음

> View.GONE으로 초기화하면 뷰가 초기화되지 않을 수 있으므로 임의의 오류가 발생할 수 있다. 따라서, 뷰의 처리(이동, 크기, 애님 등)를 하기 전에 View.VISIBLE 또는 View.INVISIBLE로 초기화하고 화면에 렌더링 한 다음에 처리해야한다.
***
출처: https://www.notion.so/imwj/Android-Interview-3ce7ddf12ddb413a9d2213173654d52c
