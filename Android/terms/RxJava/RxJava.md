# RxJava란?
* Reactive Extensions의 JVM 구현체
* 상태 변화를 관찰하고 있다가 능동적으로 동작하는 프로그램을 작성할 수 있게 해줌 -> 반응형 프로그래밍
* 복잡한 비동기 에러 처리를 간단히 처리할 수 있게 해줌
* 다양한 플랫폼과 언어를 지원함 -> 크로스 플랫폼

RxJava는 Observer Pattern + Iterator Pattern + Funtional Programming을 합쳐서 만들어졌다.

## Observer Pattern 이란?
* 상태 변화가 발생했을 때 관찰자(Observer)에게 변경 사항을 전달하는 패턴이다.

코드를 보면,
```
class Observable<E>(
  private var value: E? = null
)  {
  private val observers: MutableList<Observer<E>> = mutableListOf()
  
  fun setValue(value: E?) {
    this.value = value
    this.notifyChanges()
  }
  
  fun notifyChanges() {
    val valueToNotify = this.value
    val iterator = this.observers.iterator()
    
    while (iterator.hasNext()) {
      val observer = iterator.next()
      observer.onChanged(valueToNotify)
    }
  }
  
  fun addObserver(observer: Observer<E>) {
    this.observers.add(observer)
  }
  
  fun removeObserver(observer: Observer<E>) {
    this.observers.remove(observer)
  }
  
  fun removeAllObservers() {
    this.observers.clear()
  }
}

interface Observer<E> {
  fun onChanged(changedValue: E?)
}
```
Observable이란 클래스는 제네릭을 E를 가지고 초기값은 null이다. <br>
observers 변수는 MutableList 옵저버를 가질 수 있다. <br>
setValue 함수는 파라미터로 들어온 value 값을 가져 Observable의 파라미터 value 에 들어온 value를 할당해주고 notifyChanges 함수를 호출한다. <br>
notifyChanges 함수는 현재 value를 valueToNotify로 할당하고 observers가 가지고 있는 value값들을 iterator하여 변수에 할당한다. while문으로 hasNext의 값을 받아 false가 나올 때까지 반복한다. 값들을 반복문을 통해 꺼내서 onChanged 함수에 들어온 값(value)을 알린다.
addObserver 함수는 파라미터로 들어온 observer를 변수 observers에 추가하고, removeObserver는 observers에서 제거, removeAllObservers는 observers를 clear 시킨다.

사용 예제
```
fun main() {
  val stringObservable = Observable<String>)
  stringObservable.addObserver(object : Observer<String> {
    override fun onChanged(changedValue: String?) {
      println("string: $changedValue")
    }
  })
  stringObservable.setValue("hello")
  stringObservable.setValue("hi")
  stringObservable.setValue("bye")
  
  val intObservable = Observabl<Int>()
  intObservable.addObserver(object : Observer<Int> {
    override fun onChanged(changedValue: Int?) {
      println("int: $changedValue")
    }
  })
  
  intObservable.setValue(100)
  intObservable.setValue(200)
  intObservable.setValue(300)
  
  val map = mapOf("key map" to "value map")
  
  val mapObservable = Observable<Map<String, String>>()
  mapObservable.addObserver(object : Observer<Map<String, String>> {
    override fun onChanged(changedValue: Map<String, String>?) {
      println("map: $changedValue")
    }
  })
  
  mapObservable.setValue(map)
}
```
Observable을 만들어 사용하고 싶은 제네릭 타입으로 생성해 addObserver 함수를 불러 무명객체에 interface Observer를 상속해 onChanged 함수를 override 해서 구현하면 된다. setValue를 통해 들어온 값이 onChanged 파라미터로 들어온다.

## Iterator Pattern 이란?
* Collection의 구현과 무관하게 순차 접근이 가능하도록 하는 패턴
```
val stringIterator = listOf(
  "Kotlin",
  "RxJava",
  "RxKotlin",
  "RxAndroid"
  ).iterator()
  
  while(stringIterator.hasNext()) {
    val string = stringIterator.next()
    println(string)
  }
```

## Functional Programming이란?
* 순수 함수: Side Effect가 없는 함수(immutable한 상태 관리 추구)
* 익명 함수: 이름 없는 함수
* 고차 함수: 함수를 처리하는 함수(함수도 객체로 취급)
```
listOf(
  "Java","Kotlin","Scala","Groovy")
    .filter{ it != "Groovy" }
    .map{ "Language Name: $it" }
    .joinToString{","}
    .let(::println)
```
