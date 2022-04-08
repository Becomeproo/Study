# Cold Observable, Hot Observable
## Cold Observable
* Observer가 구독하는 동안 이벤트 방출 <br>
  ex) DB 조회, Rest API 통신, 메일링 서비스 <br>
  ![image](https://user-images.githubusercontent.com/91411447/162342333-4fbe711d-1a76-4021-8d2a-6587cce85bc8.png)
* Cold Observable의 종류
  Observable, Flowable, Single, Maybe, Completable

## Hot Observable
* 구독 여부와 관계 없이 이벤트 방출 <br>
  ex) 버튼 클릭, 키보드 입력 등 <br>
  ![image](https://user-images.githubusercontent.com/91411447/162342534-9f1ed3aa-a3a2-4296-90ab-bdd850c3ecdb.png)
* Hot Observable의 종류 <br>
  Subject, Processor
  
## Observable의 구성
* OnSubscribe: 구독 발생
* OnNext: 값 발행
* OnComplete: 발행 완료
* OnError: 에러 발생

## 마블 다이어그램
![image](https://user-images.githubusercontent.com/91411447/162342668-69784412-6744-4d95-8e85-c58b3eecdf20.png)

왼쪽에서 오른쪽 방향으로 데이터의 흐름을 나타내고 데이터를 생산하는 쪽을 upstream, 데이터를 방출하는 쪽을 downstream 이라고 부른다. <br>
이제 Cold Observable에 대해 알아보자 (Observable, Flowable, Maybe, Completable)

## 생산자 별 사용과 차이점
### Observable
* 0개 ~ n개의 값을 발행할 수 있다. 모두 발행되면 onComplete, 에러가 발생하면 onError <br>
  ![image](https://user-images.githubusercontent.com/91411447/162342844-72d9794c-3336-457c-820e-3bcc8b5149ac.png)
  
Observable은 주로 변화하는 값을 관찰하기 위해 사용한다. 예를 들어, 룸에 저장된 값들을 관찰하고 있다가 변화하면 알리는 용도
```
@Dao
interface TodoDao{
  @Query("SELECT * FROM todo ORDER BY updated_at DESC ")
  fun findAll(): Observable<List<Todo>>
}
```

### Single
* 1개의 값을 발행하거나 에러를 발행한다. <br>
  명확하게 값이 1개만 발행되는 Rest API 통신에서 사용된다.
  ![image](https://user-images.githubusercontent.com/91411447/162343043-be4f8f77-e74b-4b41-909d-bc363fbae308.png)

### Maybe
* 1개의 값을 발행하거나 아무것도 발행하지 않고 종료하거나 에러를 발행한다.
  성공, 취소, 에러 같은 3개의 상태를 가질 때 주로 사용한다. (예를 들어, activity for result 코드에서 사용자가 돌아왔을 때, 혹은 그냥 종료 했을 때나 취소했을 때) <br>
  ![image](https://user-images.githubusercontent.com/91411447/162343150-2873db73-b5d8-441b-aa3b-ca1c3f82c616.png)
  ![image](https://user-images.githubusercontent.com/91411447/162343167-0ef84ba9-738e-4427-ac2e-11d467d38d22.png)

### Completable
* 종료하거나 에러를 발행한다. (값을 발행하지 않는다.)
  완료되었다는 결과만 알고 싶을 때 사용한다.
  ![image](https://user-images.githubusercontent.com/91411447/162343227-1bfc90a0-ebc5-43e4-9afb-4492c6e649d7.png)

### Flowable
* 기본적으로 Observable과 구성이 같다. Observable과 다른 점은 배압기능이 있어 구독자의 처리속도에 따라 발행 속도를 조정할 수 있다. (BackPressure)
> Back Pressure Issue
  ![image](https://user-images.githubusercontent.com/91411447/162343291-e1112e35-da61-4fb5-a786-ba869535cfc9.png)
  ![image](https://user-images.githubusercontent.com/91411447/162343300-bc7e0028-0ef2-4eaa-91c1-0eff950e5aef.png)

데이터의 발행 속도 보다 소비 속도가 느려서 발생하는 문제이다. <br>
예를 들어, 1초마다 데이터를 통제하는데 소비하는 측에서 무거운 작업을 하게 되면 1초마다 데이터를 받지 못하게 된다. 이럴 때 배압 기능을 사용할 수 있다.

  * BackPressureStrategy의 종류
    * Drop
      ![image](https://user-images.githubusercontent.com/91411447/162343399-c698bbff-4efd-4d6d-a4ff-3a9c54d87a64.png)
      처리 속도가 느려지게 되면 발행된 값을 버린다. 즉, 통지할 수 있을 때까지 생성한 데이터를 삭제한다.
    * Buffer
      ![image](https://user-images.githubusercontent.com/91411447/162343468-1441106b-716d-4319-9b43-94e3e62540c7.png)
      통지할 수 있을 때까지 모든 데이터를 버퍼링한다. 하지만 용량을 초과하게 되면 문제가 발생한다.
    * Latest
      ![image](https://user-images.githubusercontent.com/91411447/162343520-576a39d2-93da-45d9-b8dd-2269e9959116.png)
      생성한 최신 데이터만 버퍼링하고 생성할 때마다 버퍼링하는 데이터를 교환한다. 즉, 최근에 발행된 값만 유지한다.
    * Error
      BackPressure가 발생하면 Error를 발생시킨다.
    * Missing
      BackPressure에 대한 처리를 하지 않는다.
    
## Flowable과 Observable의 차이
* Flowable: Flowable은 Reactive Streams의 생산자인 Publisher를 구현한 클래스이고, Subscriber는 Reactive Streams의 클래스
* Observable: Reactive Streams를 구현하지 않음

### Observable의 생성
* just
  ```
  Observable.just("Hello, RxJava")
              .subscribe { message ->
                println(message)
              }
  ```
"Hello, RxJava"를 한번 출력한다.
* fromIterable
  ```
  Observable.fromIterable(listOf("Hello", "Kotlin", "RxJava"))
              .subscribe { language ->
                 println(language) 
               }
  ```
리스트의 아이템을 가져와 1개씩 출력한다.
* error
  ```
  Observable.error<String>(IllegalStateException())
              .subscribe( { message ->
                println(message)
              }, { exception ->
                  println("error!: $exception")
              })
  ```
  에러를 출력한다.
* create
  ```
  Observable.create<String> { emitter ->
          emitter.onNext("Hello")
          emitter.onNext("RxAndroid")
          emitter.onNext("RxKotlin")
          emitter.onNext("RxJava")
          emitter.onComplete()
      }.subscribe { language ->
          println(language)
      }
  ```
  emitter를 통해 직접 값을 발행할 수 있다. (just는 함수 안에 onNext를 호출시킨다.)

![image](https://user-images.githubusercontent.com/91411447/162344373-c66da631-286e-42cf-8ef1-b7f2212ae590.png)
setCancellable을 구독 취소 시, emiiter를 취소시켜주는건데 그냥 넘어가면 메모리 누수가 발생하게 된다.

* fromArray
  ```
  val observable = Observable.fromArray("Kotlin", "Java", "Scala")
  
      observable.subscribe { println("subscribe1: $it")  }
      observable.subscribe { println("subscribe2: $it")  } 
      observable.subscribe { println("subscribe3: $it")  }
  ```
  각 Observer가 구독할 때마다 발행되는 것을 확인할 수 있다.

## Subject
> Subject = Observable + Observer

* Subject의 종류
  PublishSubject, BehaviorSubject, ReplaySubject, AsyncSubject, SerializedSubject
  
  * PublishSubject
    Observer가 구독한 이후 발행된 값만 수신한다. (클릭 이벤트에 주로 사용된다.)
    ```
    val publishSubject = PublishSubject.create<String>()
        publishSubject.onNext("Android")
        publishSubject.subscribe {
            println("1: $it")
        }
        
        publishSubject.onNext("Kotlin")
        publishSubject.subscribe {
            println("2: $it")
        }
        
        publishSubject.onNext("RxJava")
        publishSubject.subscribe {
            println("3: $it")
        }
    ```
    ![image](https://user-images.githubusercontent.com/91411447/162344994-815467e7-1afe-4985-bbda-18503c09df7e.png)
    로그에서 보이는 것처럼 구독한 이후 데이터만 받고 구독 전 발행된 값을 수신하지 않는다.

  * BehaviorSubject
    Observer가 구독하기 바로 이전 값을 기억하고 있다가 구독하면 캐시된 값을 발행한다. <br>
    (최신 상태를 기억해야 할 때, 주로 키보드 입력에 사용한다.)
    ```
    val behaviorSubject = BehaviorSubject.create<String>()
        behaviorSubject.onNext("Android")
        behaviorSubject.subscribe {
            println("1: $it")
        }
        
        behaviorSubject.onNext("Kotlin")
        behaviorSubject.subscribe {
            println("2: $it")
        }
        
        behaviorSubject.onNext("RxJava")
        behaviorSubject.subscribe {
            println("3: $it")
        }
    ```
    ![image](https://user-images.githubusercontent.com/91411447/162345317-65b40e00-0fc6-4cec-9d32-6b42a9ad5679.png)
    
    1번을 보면 구독 전 데이터를(Android), 2번은 구독 전 데이터(Kotlin), 3번은 (RxJava) 데이터를 받은 걸 볼 수 있다.

  * ReplaySubject
    Observer가 구독하기 전 값을 캐시하고 있다가 구독하면 캐시된 값을 준다. (전체)
    ```
    val replaySubject = ReplaySubject.create<String>()
        replaySubject.onNext("Android")
        replaySubject.subscribe {
            println("1: $it")
        }
        
        replaySubject.onNext("Kotlin")
        replaySubject.subscribe {
            println("2: $it")
        }
        
        replaySubject.onNext("RxJava")
        replaySubject.subscribe {
            println("3: $it")
        }
    ```
    ![image](https://user-images.githubusercontent.com/91411447/162345719-b2688609-5db5-403e-816a-f70a2bc9f08a.png)

  * AsyncSubject
    데이터를 아무것도 발행하지 않는다(onComplete를 호출하기 전까지) <br>
    onComplete를 호출하게 되면 가장 마지막에 발행된 데이터를 발행한다. (가장 최근에 생성한 데이터)
    ```
    val asyncSubject = AsyncSubject.create<String> ()
        asyncSubject.onNext("Android")
        aysncSubject.subscribe {
            println("1: $it")
        }
        
        asyncSubject.onNext("Kotlin")
        aysncSubject.subscribe {
            println("2: $it")
        }
        
        asyncSubject.onNext("RxJava")
        aysncSubject.subscribe {
            println("2: $it")
        }
        
        // onComplete를 호출해야 가장 최근에 발행한 값(RxJava)를 발행한다. (1, 2, 3, 모두 RxJava)
        aysncSubject.onComplete()
    ```
    
  * SerializedSubject
    단독으로 사용할 수 없고 뒤에 .toSerialized()를 붙여야 사용할 수 있다. (멀티 스레드 환경에서 동시성 문제를 해결하는데 주로 사용한다.)
    ```
    val subject = PublishSubject.create<String>()
                .toSerialized()
        subject.onNext("Java")
        subject.subscribe { println("subscribe1: $it") }
        
        subject.onNext("Kotlin")
        subject.subscribe { println("subscribe2: $it") }
        
        subject.onNext("Scala")
        subject.onComplete()
    ```

## Processor
> Flowable + Subscriber

* Processor의 종류
  PublishProcessor, BehaviorProcessor, ReplayProcessor, AysncProcessor, SerializedProcessor
  
Processor는 Subject와 동일하다. 마찬가지로 배압 기능이 차이가 있다. Processor는 잘 사용하지 않는다. <br>
주로 사용되는 것은 PublishSubject, BehaviorSubject를 사용하고 있고 SerializedSubject는 헤더에 토큰을 요청할 때 주로 사용된다.

***
출처: https://www.notion.so/1-c756ff9a819346a38e1ebc2c8d665424
