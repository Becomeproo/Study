# Filtering Operator
### Skip
특정 횟수 또는 시간만큼 발행을 건너뜀 <br>
![image](https://user-images.githubusercontent.com/91411447/162867879-ea5d0dfa-ed2c-4bf6-9345-545ce97dbf05.png)
```
Observable.range(1, 4)
    .skip(2)
    .subscribe {
        println(it.toString())
    }
```

### Filter
조건에 만족(true)하는 경우에만 값을 발행 (Rx에선 if문 같은 역할) <br>
![image](https://user-images.githubusercontent.com/91411447/162867992-722dc7fc-4c27-4420-890b-edde715b65db.png)
```
Observable.fromIterable(0..10)
    .filter{ x -> x % 2 == 0 }
    .subscribe {
        println(it)
    }
```

### debounce
처음 값이 발행된 이후 정해진 시간동안 값이 발행되지 않으면 최근 값을 방출 <br>
(자동 완성과 같은 짧은 시간에 많은 쿼리가 이뤄지는 작업에 이용) <br>
![image](https://user-images.githubusercontent.com/91411447/163091819-adfb3fbb-558d-49dc-8ccf-43c008d36586.png)
![image](https://user-images.githubusercontent.com/91411447/163091830-6e30a54c-1a7d-4bb7-9db1-e0898aae5719.png)

간단한 예제를 만들어 보자.
키보드 입력을 받고 텍스트 뷰에 입력 결과를 띄워보자
```
<androidx.constraintlayout.widget.ConstraintLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    android:layout_width="match_parent"
    android:layout_height="match_parent">

    <EditText
        android:id="@+id/et"
        android:layout_width="0dp"
        android:layout_height="wrap_content"
        android:background="@null"
        android:hint="입력창"
        app:layout_constraintBottom_toTopOf="@id/tv"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toTopOf="parent"
        app:layout_constraintVertical_chainStyle="packed" />

    <TextView
        android:id="@+id/tv"
        android:layout_width="0dp"
        android:layout_height="wrap_content"
        android:hint="결과창"
        app:layout_constraintBottom_toBottomOf="parent"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toBottomOf="@id/et" />
</androidx.constraintlayout.widget.ConstraintLayout>
```
```
private val compositeDisposable = CompositeDisposable()
private val _onTextChanged: Subject<String> = PublishSubject.create()

et.addTextChangedListener { text ->
            _onTextChanged.onNext(text?.toString() ?: "")
        }
        
        _onTextChanged
            .debounce(700, TimeUnit.MILISECONDS)
            .observeOn(AndroidSchedulers.mainThread())
            .subscribe(tv::setText)
            .let(compositeDisposable::add)
```
![image](https://user-images.githubusercontent.com/91411447/163092217-9285d10a-ed59-45a7-a37d-d794f126eaac.png)

타이핑을 종료하면 일정 시간 후 반영되는 것을 알 수 있다.

### throttleFirst
발행된 값을 정해진 시간 간격으로 묶고, 그 중 1번 째 값만을 방출 <br>
(주로 버튼 클릭 이벤트가 동시에 여러 번 발생하는 것을 방지하기 위해 사용) <br>
![image](https://user-images.githubusercontent.com/91411447/163092306-58ee2def-cebd-4185-a31c-1d291046a0f4.png)
![image](https://user-images.githubusercontent.com/91411447/163092324-4676785d-43fa-43ca-baf0-e27d3564dd8b.png)

간단한 예제를 만들어보자
```
<androidx.constraintlayout.widget.ConstraintLayout
    xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    android:layout_width="match_parent"
    android:layout_height="match_parent">

    <CheckBox
        android:id="@+id/chk"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        app:layout_constraintBottom_toTopOf="@id/btn"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintHorizontal_chainStyle="packed"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toTopOf="parent" />

    <Button
        android:id="@+id/btn"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Toggle!"
        app:layout_constraintBottom_toBottomOf="parent"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toBottomOf="@id/chk" />


</androidx.constraintlayout.widget.ConstraintLayout>
```
```
private val compositeDisposable = CompositeDisposable()
private val _onClick: Subject<Unit> = PublishSubject.create()

btn.setOnClickListener { _onClick.onNext(Unit) }

        _onClick
            .throttleFirst(1_000, TimeUnit.MILLISECONDS)
            .observeOn(AndroidSchedulers.mainThread())
            .subscribe { chk.isChecked = !chk.isChecked }
            .let(compositeDisposable::add)
```
.throttleFirst를 제거한 코드와 비교를 해보면 한 번에 와닿을 수 있을 것이다.

### sample
발행된 값을 정해진 시간 간격으로 묶고, 그 중 마지막 값을 방출
(emitLast가 true이면 onComplete 호출 시 남아있는 값을 발행하고 종료) <br>
![image](https://user-images.githubusercontent.com/91411447/163304643-933978ac-0b60-4a3a-877e-0707ec1a5de5.png)
![image](https://user-images.githubusercontent.com/91411447/163304658-7f9e37e0-0e96-468b-84ab-6fe7ce22bf33.png)

throttleLast의 리턴 값은 sample이다. emitLast가 있고 없고의 차이이다.

### dintinct
중복된 값 발행을 방지 <br>
![image](https://user-images.githubusercontent.com/91411447/163304806-0bd10694-d400-4cc6-aa49-ba7f102d4562.png)
```
Observable.just(1, 2, 2, 1, 3)
    .distinct()
    .subscribe(::println)
```
1, 2가 중복이 되어 있기 때문에 1, 2, 3이 출력된다.

### distinctUntilChanged
이전 값과 다른 경우에만 방출 <br>
![image](https://user-images.githubusercontent.com/91411447/163305100-35a617e3-ddee-40ab-81e6-54a521fe9a49.png)
```
Observable.just(1, 2, 2, 1, 3)
    .distinctUntilChanged()
    .subscribe(::println)
```
이전 값과 비교하기 때문에 1, 2, 1, 3 이 출력된다.

### take
발행된 값 중 n번째까지만 방출 <br>
![image](https://user-images.githubusercontent.com/91411447/163305211-91d862a1-473e-41f2-870a-73a5dd51d232.png)

### takeLast
발행된 값 중 뒤에서 n번째까지만 방출 <br>
(단, onComplete가 호출되지 않으면 무한대가 되므로 onComplete이후에 발행된다.)
![image](https://user-images.githubusercontent.com/91411447/163305289-c1394b00-773e-4566-8976-26b19f2968f0.png)

### first(Single)
발행된 값 중 1개만 가져옴 <br>
(Flowable -> Single, Observable -> Single 로 변환) <br>
![image](https://user-images.githubusercontent.com/91411447/163306330-c88c113b-ed8f-427e-9f71-96ede91a844b.png)
```
Observable.fromArray(1, 2, 3)
    .first(0)
    .subscribe { value -> println(value) }

Observable.empty<Int>()
    .first(0)
    .subscribe { value -> println(value) }
```
첫번째 Observable은 fromArray로 생성한 인자 중 첫번째 값이 1이 방출이 되고 두번째 Observable은 empty를 생성했기 때문에 first의 default item인 0이 방출된다.

### firstOrError(Single)
first랑 동일하나 error를 방출할 때 사용
```
Observable.fromArray(1, 2, 3)
    .firstOrError
    .subscribe(::println, Throwable::printStackTrace)
    
Observable.empty<Int>()
    .firstOrError()
    .subscribe(::println, Throwable::printStackTrace)
```
첫번째 Observable은 1이 방출되고, 두번째 Observable은 생성된 값과 default item이 없기 때문에 error를 방출한다. single은 1개의 값 또는 에러라는 것을 다시 한 번 기억하자

### firstElement(Maybe)
발행된 값 중 1개의 값만 가져옴
(Flowable -> Maybe, Observable -> Maybe)
```
Observable.formArray(1, 2, 3)
    .firstElement()
    .subscribe(::println, {}, {
        println("complete 1")
    })

Observable.empty<Int>()
    .firstElement()
    .subscribee(::println, {}, {
        println("complete 2")
    })
```
첫번째 Observable은 1이 방출되고, 두번째 Observable은 complete가 호출된다.(Maybe는 1개의 값 또는 방출하지 않거나, 완료 통지를 하기 때문)

### ignoreElements
발행된 값 중 1개의 값만 가져옴
(Flowable -> Completable, Observable -> Completabe, Single -> Completable, Maybe -> Completable)
```
Observable.just(1, 2, 3)
    .ignoreElements()
    .subscribe{ println("complete!") }
```
Completable이기 때문에 완료 통지를 한다.

***
출처: https://www.notion.so/3-47374a13d8eb4480b4889909bfdbc98a
