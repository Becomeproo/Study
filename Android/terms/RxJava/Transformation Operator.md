# Transformation Operator
## Map
발행된 값을 다른 값으로 변환하는 연산자 <br>
![image](https://user-images.githubusercontent.com/91411447/162549733-f8d90bcf-5d70-4f1d-9e12-9409f8acfddf.png)
```
Observable.fromIterable(1..10)
            .map { data -> data * 10 }
            .subscribe { result ->
                println(result)
            }

Observable.fromIterable(1..10)
            .map { data -> data * 10 }
            .map { data -> NumberFormat.getCurrencyInstance().format(data) }
            .subscribe { strResult ->
                println(strResult)
            }
```
인자로 받는 함수형 인터페이스는 데이터를 받으면 반드시 무엇이든 null이 아닌 데이터 하나를 반환해야 한다. 변환한 데이터의 자료형은 받은 데이터의 자료형과 달라도 상관없다. 두 번째 코드를 봐도 int로 생성된 데이터를 string으로 변환된 걸 볼 수 없다.

## Scan
이전 결과값을 현재 값과 함께 전달하는 연산자 <br>
![image](https://user-images.githubusercontent.com/91411447/162549858-430758bd-eb29-47ff-aaf4-46ef0c438101.png)
```
Observable.fromIterable(1..5)
    .scan { x, y -> x + y }
    .subscribe { result ->
        println("result: ${result}")
    }
```

## Buffer
buffer의 사이즈만큼 값이 발행되면 모아서 컬렉션으로 통지하는 연산자 <br>
![image](https://user-images.githubusercontent.com/91411447/162549914-48845e29-b3ed-49d6-821c-ec791e2cb385.png)
```
Observable.fromIterable(1..5)
            .buffer(3)
            .subscribe{ result ->
                println(result)
            }
// 3개의 값이 모이면 List로 묶어서 발행한다.


Observable.fromIterable(1..5)
    .buffer(3, 1)
    .subscribe{ result ->
        println(result)
    }
// 동일하게 3개씩 값이 모이면 발행되나 값이 1씩 밀린다.


Observable.fromIterable(1..5)
    .buffer(3) {
        mutableSetOf<Int>()
    }
    .subscribe{ result ->
        println(result)
    }
// List가 아닌 Set으로 바꿀수도 있다.

발행되는 시간에 따라 buffer를 모아줄수도 있다.
Observable.interval(0, 100, TimeUnit.MILLISECONDS, Schedulers.trampoline())
            .buffer(1000, TimeUnit.MILLISECONDS)
            .subscribe{ result ->
                println(result)
            }
```
interval로 0.1초마다 발행되는 데이터 값을 버퍼로 모으고 있다가 1초 후 발행한다. 여기서 interval의 스케줄러를 지정하지 않으면 동작하지 않는다. (interval의 기본 스케줄러는 computation이기 때문)


## Window
기본적으로 buffer와 같으나 Collection이 아닌 Observable을 발행하는 연산자
![image](https://user-images.githubusercontent.com/91411447/162550110-c5be1bac-a204-4c92-8929-85ed3a03e04f.png)

## GroupBy
동일한 key 값을 모아 Observable로 발행하는 연산자
![image](https://user-images.githubusercontent.com/91411447/162550128-3df8c47b-ee27-412b-874f-1fbfb2e29355.png)
```
Observable.range(1, 5)
            .groupBy { x -> x % 2 }
            .concatMapSingle { group ->
                group.toList()
                    .map { value ->
                        if (group.key == 0) {
                            "짝수: ${value}"
                        } else {
                            "홀수: ${value}"
                        }
                    }
            }
            .subscribe { result ->
                println(result)
            }
```
***
출처: https://www.notion.so/2-1e43be8260114fbcb78958bc7eae46b7

