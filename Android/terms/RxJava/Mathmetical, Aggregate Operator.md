# Mathmetical, Aggregate Operator

### Reduce

onComplete가 호출되면 누적된 연산 결과를 발행하고 종료
```
Observable.just(1,2,2,2,5)
    .reduce(0) { x, y ->
        x + y
    }
    .subscribe { result ->
        println(result)
    }
```
Scan과 유사하지만 누적된 최종 값만을 발행하는 점에서 다르다.

### Utility Operator
* Do~
특정 시점에 어떤 작업을 수행하고 싶을 때 사용한다. (Log를 남겨서 디버깅하는 용도로 많이 씀) <br>
```
Observable.fromIterable(0..10)
            .doOnNext { println(it) }
            .map { it * 10 }
            .doOnNext { println(it) }
            .doOnComplete { println("complete!") }
            .doOnSubscribe { println("subscribe!") }
            .doOnAfterTerminate { println("terminate!!") }
            .subscribe{ result ->
                println(result)
            }
```
***
출처: https://www.notion.so/2-1e43be8260114fbcb78958bc7eae46b7
            
