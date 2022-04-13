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



***
출처: https://www.notion.so/2-1e43be8260114fbcb78958bc7eae46b7
