# Combinding Observables
# merge
N개의 Observable을 1개로 합침 <br>
![image](https://user-images.githubusercontent.com/91411447/163503158-6527eff4-e4ab-477f-b6ca-2cf4833fe3eb.png)
![image](https://user-images.githubusercontent.com/91411447/163503166-daa96a71-34e9-4de8-a738-6d621e92491f.png)
발행한 순서대로 값을 방출한다.

간단한 예제 앱을 만들어보자.
Tap 버튼을 누르면 카운터가 증가하는 예제
```
```kotlin
<androidx.constraintlayout.widget.ConstraintLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    android:layout_width="match_parent"
    android:layout_height="match_parent">

    <Button
        android:id="@+id/btn_left"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Tab"
        app:layout_constraintBottom_toBottomOf="parent"
        app:layout_constraintEnd_toStartOf="@id/btn_right"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toBottomOf="@id/tv_result" />

    <Button
        android:id="@+id/btn_right"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Tab"
        app:layout_constraintBottom_toBottomOf="parent"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintStart_toEndOf="@id/btn_left"
        app:layout_constraintTop_toBottomOf="@id/tv_result" />

    <TextView
        android:id="@+id/tv_result"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:hint="값"
        app:layout_constraintBottom_toTopOf="@id/btn_right"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintHorizontal_chainStyle="spread"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toTopOf="parent" />

</androidx.constraintlayout.widget.ConstraintLayout>
```
```
private val compositeDisposablee = CompositeDisposable()
private val _onButtonLeft: Subject<Unit> = PublishSubject.create()
private val _onButtonRight: Subject<Unit> = PublishSubject.create()
private var count = 0

btn_left.setOnClickListener { _onButtonLeft.onNext(Unit) }
btn_right.setOnClickListener { _onButtonRight.onNext(Unit) }

Observable.merge(_onButtonLeft, _onButtonRight_
    .subscribe { tv_result.text = (count++).toString() }
    .let(compositeDisposable::add)
```

### amb
N개의 Observable 중 가장 먼저 발행되기 시작한 Observable만 수신 <br>
![image](https://user-images.githubusercontent.com/91411447/163509149-3515f847-66ac-480a-99bb-488b4b9ead78.png)
![image](https://user-images.githubusercontent.com/91411447/163509159-93be68be-bf9b-4a52-9e1c-5781bc50c118.png)

위 예제에서 merge -> ambArray로 바꾸고 실행하면 차이점을 한 눈에 알 수 있다.

### merge
N개의 single을 하나로 합침(single -> Flowable) <br>
![image](https://user-images.githubusercontent.com/91411447/163509263-413371ce-810a-4f00-9078-c1174830a016.png)
```
Single.merge(
    Single.just("A")
    Single.just("B")
).subscribe(::println)
```
single은 1개의 값만 발행 가능하기 때문에 N개의 발행을 위해서 Flowable이 된다.

### merge
N개의 maybe를 하나로 합침 (Maybe -> Flowable)
```
Maybe.merge(
    Maybe.empty<String>(),
    Maybe.just(1)
).subscribe( { value ->
    println(value)
}, {}, {
    println("complete!")
})
```

### merge
N개의 Completable을 하나로 합침 (Completable -> Completable)
```
Completable.mergeArray(
    Completable.complete()
        .delay(1_000, TimeUnit.MILLISECONDS, Schedulers.trampoline()),
    Completable.complete()
        .delay(2_000, TimeUnit.MILLISECONDS, Schedulers.trampoline())
).subscribe{ println("complete!") }
```

### zip
N개의 Observable에서 발행 순서가 같은 값끼리 그룹으로 묶는다. <br>
![image](https://user-images.githubusercontent.com/91411447/163509887-457578eb-ad9c-42a7-8cf3-5dcf810851ac.png)
![image](https://user-images.githubusercontent.com/91411447/163509901-d669adcc-e52c-4b51-a0c9-2fcf92aaaf69.png)

짝이 없으면 발행하지 않는다. (버림)
```
val first = Observable.just(1, 2, 3, 4, 5)
val second = Observable.just("A", "B", "C", "D")

    Observable.zip(
            first,
            second,
            Bifunction { ti: Int, t2: String -> t1 to t2 }
    .map{ (t1, t2) ->
        "$t1 $t2"
    }.subscribe(::println)
```
BiFunction을 통해 첫번째, 두번째 들어온 Observable을 Pair로 묶어 map을 통해 string으로 변환 후 출력한다. <br>
zip은 9개까지 묶을 수 있으며 세 개일때는 Function3, 네개 일때는 Function4 함수로 묶는다. 여기서는 타입추론이 안돼서 타입을 적어줘야 한다. 또한 세 개 일때는 triple 함수를 써서 엮어야 하는데 네 개부터는 없어서 직접 만들어서 사용해야 한다.
```
val first = Observable.just(1, 2, 3, 4, 5)
val second = Observable.just("A", "B", "C", "D")
val three = Observable.just(100, 200, 300, 400, 500, 600, 700)

    Observable.zip(
            first,
            second,
            three,
            Function3 { t1: Int, t2: String, t3: Int -> Triple(t1, t2, t3) }
    ).map { (t1, t2, t3) ->
        "$t1 $t2 $t3"
    }.subscribe(::println)
```

### concat
N개의 Observable을 순서대로 이어줌 (첫 번째 Observable이 끝나면 두 번째 Observable이 시작) <br>
![image](https://user-images.githubusercontent.com/91411447/163510539-d412d25e-214b-4f17-bc2c-a708b7833e28.png)
![image](https://user-images.githubusercontent.com/91411447/163510548-ac985821-60b3-4eec-96e3-1e16120c4e25.png)
```
val first = Observable.just(1, 2, 3, 4, 5)
val second = Observable.just("A", "B", "C", "D")

Observable.concat(first, second)
    .subscribe(::println)
```

### combineLatest
N개의 Observable의 최신값을 모아 방출 <br>
![image](https://user-images.githubusercontent.com/91411447/163510639-95957205-cd59-4094-9983-e3f7e8ec9599.png)

간단한 예제를 만들어보자.
두 개의 editText 값을 입력 후 TextView에 출력한다.
```
<androidx.constraintlayout.widget.ConstraintLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    android:layout_width="match_parent"
    android:layout_height="match_parent">

    <EditText
        android:id="@+id/et_first"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        app:layout_constraintBottom_toBottomOf="parent"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toTopOf="parent"
        app:layout_constraintVertical_chainStyle="packed" />

    <EditText
        android:id="@+id/et_second"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toBottomOf="@id/et_first" />

    <TextView
        android:id="@+id/tv_result"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toBottomOf="@id/et_second" />


</androidx.constraintlayout.widget.ConstraintLayout>
```
```
private val compositeDisposable = CompositeDisposable()
private val _firstChanged: Subject<String> = PublishSubject.create()
private val _secondChanged: Subject<String> = PublishSubject.create()

et_first.addTextChangedListener { _firstChanged.onNext(it?.toString() ?: "") }
et_second.addTextChangedListener { _secondChanged.onNext(it?.toString() ?: "") }

Observable.combineLatest(
    _firstChanged,
    _secondChanged,
    BiFunction { x: String, y: String -> x to y }
).map { (x, y) ->
    "$x + $y"
}.subscribe {
    tv_result.text = it
).addTo(compositeDisposable)
```
정상적으로 된다. 하지만 여기서의 문제점은 Publish로 하게 되면 구독 이후 값만 기억할 수 있다. 사용자는 즉각적인 반응을 원하기 때문에 Publish로 하게되면 default값이 없기 때문에 잘못하면 에러가 걸리는 현상이 나타날 수 있고 사용자 경험상 생소할 것이다. 따라서 Behavior로 바꿔준 후 default 값을 설정해주는 것이 좋다.
```
private val _firstChanged: Subject<String> = BehaviorSubject.createDefault("")
private val _secondChanged: Subject<String> = BehaviorSubsect.createDefault("")
```

### startWith
파라미터의 값을 먼저 방출 후 Observable의 값을 방출 <br>
![image](https://user-images.githubusercontent.com/91411447/163511202-982a6c28-a148-44e9-9d17-8031541e29e5.png)
```
Observable.just(1, 2, 3)
    .startWith(0)
    .subScribe(::println)
```
Observable로 생산한 1, 2, 3 보다 0을 먼저 출력 후 Observable 값을 방출한다.

***
출처: https://www.notion.so/3-47374a13d8eb4480b4889909bfdbc98a
