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
***
출처: https://www.notion.so/2-1e43be8260114fbcb78958bc7eae46b7
