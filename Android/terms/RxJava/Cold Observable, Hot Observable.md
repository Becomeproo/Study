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

***
출처: https://www.notion.so/1-c756ff9a819346a38e1ebc2c8d665424
