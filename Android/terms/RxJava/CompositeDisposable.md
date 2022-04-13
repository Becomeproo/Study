# CompositeDisposable
여러 Disposable을 모아 CompostieDisposable의 disposable 메서드를 호출함으로써 가지고 있는 모든 Disposable의 dispose 메서드를 호출할 수 있는 클래스이다.

### clear()
등록된 스트림을 모두 구독 해제하는 메서드 + 이후에 등록된 스트림은 해제하지 않음(재사용 가능)
```
val compositedislostable = CompositeDisposable()

    Single.just("1 finish")
            .delay(100, TimeUnit.MILLISECONDS)
            .doOnDispose { println("1 dispose") }
            .subscribe { v -> println(v) }
            .let(compositedislostable::add)
            
    Single.just("2 finish")
            .delay(100, TimeUnit.MILLISECONDS)
            .doOnDispose { println("2 dispose") }
            .subscribe { v -> println(v) }
            .let(compositedislostable::add)
            
    Single.just("3 finish")
            .delay(100, TimeUnit.MILLISECONDS)
            .doOnDispose { println("3 dispose") }
            .subscribe { v -> println(v) }
            .let(compositedislostable::add)
            
    comspositdislostable.clear()
```
clear는 등록된 스트림의 구독을 해제한다. 하지만 위치가 중요하다. 현재 clear는 1,2,3 번 아래 위치해 있어 1,2,3 모두 구독을 해제한다. 하지만 2번과 3번 위치에 가게 되면 1, 2번만 구독을 해제하게 되고 1번과 2번 위치에 가게되면 1번만 구독을 해제하게 된다. 즉, 전에 있던 스트림의 구독을 해제한다.

### dispose()
등록된 스트림을 모두 구독 해제 + 이후에 등록된 스트림은 자동으로 해제
```
val compositeddislostable = CompositeDisposable()

    Single.just("1 finish")
            .delay(100, TimeUnit.MILLISECONDS)
            .doOnDispose { println("1 dispose") }
            .subscribe { v -> println(v) }
            .let(compositedislostable::add)
            
    Single.just("2 finish")
            .delay(100, TimeUnit.MILLISECONDS)
            .doOnDispose { println("2 dispose") }
            .subscribe { v -> println(v) }
            .let(compositedislostable::add)
            
    Single.just("3 finish")
            .delay(100, TimeUnit.MILLISECONDS)
            .doOnDispose { println("3 dispose") }
            .subscribe { v -> println(v) }
            .let(compositedislostable::add)
            
    comspotedlostable.dispose()
```

***
출처: https://www.notion.so/2-1e43be8260114fbcb78958bc7eae46b7
