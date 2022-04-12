# Dispose, Disposable
Disposable은 구독을 해지하는 메서드를 포함한 인터페이스이다. Disposable은 Observale과 Observer 간 구독에서 Observable이 구독 준비가 되면 onSubscribe 메서드를 통해 Observer에 전달되는 객체이다. <br>

예를 들어, A화면에서 아이템을 클릭 후 B화면으로 넘어가는 상상을 떠올려보자 <br>
B화면으로 넘어왔을 때 데이터를 불러오게 되면 로딩이 나오게 된다. 이때 사용자가 데이터를 다 불러오지 않았는데 뒤로가기를 누르게되면 데이터를 불러오는 도중 다시 A화면으로 바뀌게 된다. B화면으로 구독이 되어있는 상태에서 onSuccess or onError를 기다리고 있는 상태에서 여전히 계속 기다리게 되는 현상이 나타나게 된다. 이렇게 된다면 메모리릭이 발생하게 된다. 메모리 누수를 방지하기 위해 사용자가 뒤로 갔을 때 subscribe -> unsubscribe로 구동을 해지시켜줘야 한다. <br>

```
if (!disposable.isDisposed) {
    disposable.dispose() // onComplete나 onError가 호출되지 않았더라도 구독을 해제한다.
}
```
***
출처: https://www.notion.so/2-1e43be8260114fbcb78958bc7eae46b7
