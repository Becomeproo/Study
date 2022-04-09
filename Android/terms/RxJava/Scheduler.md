# Scheduler

## Scheduler란
RxJava에서는 개발자가 직접 스레드를 관리하지 않게 각 처리 목적에 맞춰 스레드를 관리하는 스케줄러(Scheduler)를 제공한다. 이 스케줄러를 설정하면 어떤 스레드에서 무엇을 처리할지 제어할 수 있다. <br>
![image](https://user-images.githubusercontent.com/91411447/162549213-b84d3de7-034f-4ba7-8125-c68fd8c915f2.png)
![image](https://user-images.githubusercontent.com/91411447/162549214-8e2e7108-183f-4b9c-b649-39cd555c8bf2.png)
![image](https://user-images.githubusercontent.com/91411447/162549219-409df120-cfd0-4448-99a1-8b62b2b8539f.png)

## RxJava의 Scheduler의 종류
### NewThread Scheduler
매번 새로운 스레드를 생성하는 스케줄러 <br>
![image](https://user-images.githubusercontent.com/91411447/162549350-646e7cbb-52a6-4d49-8317-8dcd02352250.png)


### SingleThread Scheduler
싱글 스레드에서 처리 작업을 할 때 사용하는 스케줄러 <br>
![image](https://user-images.githubusercontent.com/91411447/162549354-8e0ce58e-8bc9-434a-a75e-3836fb54473d.png)


### Computation Scheduler
연산 처리를 할 때 사용하는 스케줄러, 논리 프로세서 수와 같은 수만큼 스레드를 캐시한다. I/O 처리 작업에서는 사용할 수 없다. <br>
![image](https://user-images.githubusercontent.com/91411447/162549359-89b528ee-4bd2-4a56-a643-35f9cbd10f3d.png)


### I/O Scheduler
스레드 풀에서 스레드를 가져오며 필요에 따라 새로운 스레드를 만든다. <br>
![image](https://user-images.githubusercontent.com/91411447/162549362-0d433b14-41fe-4b38-84bd-cb0c2c7b53c3.png)


### Trampoline Scheduler
현재 스레드의 큐에 처리 작업을 넣는 스케줄러로, 이미 다른 처리 작업이 큐에 들어있다면 큐에 들어있는 작업의 처리가 끝난 뒤에 새로 등록한 처리 작업을 수행한다. <br>
![image](https://user-images.githubusercontent.com/91411447/162549375-1a76b233-afad-47e5-bdea-eba91f48baff.png)


### MainThread Scheduler(RxAndroid) <br>
![image](https://user-images.githubusercontent.com/91411447/162549378-5dac85ec-7b85-4c8f-9855-69bee3508227.png)

이 중 Computation 메서드와 I/O 메서드로 얻은 스케줄러는 거의 같은 역할을 하며, 호출할 때 스레드 풀에서 서로 다른 스레드를 가져온다. 다만, 사용 용도가 연산 처리 작업과 I/O 처리 작업으로 나뉜다. I/O 메서드로 가져온 스케줄러는 스레드 풀에 더 이상 가져올 스레드가 없을 때 새로운 스레드를 만든다. I/O 처리 작업은 처리 중에 대기 시간이 발생할 가능성이 커서 논리 프로세서 수를 초과하는 스레드를 생성해 동시에 처리 작업을 해도 효율적으로 처리 작업을 할 수 있기 때문이다. 하지만 Computation 메서드로 가져온 스케줄러는 논리 프로세서 수를 넘지 않는 범위에서 스레드를 주고 받는다. 이는 연산 처리 작업에는 대기하는 일이 없어서 논리 프로세서 수를 초과하는 스레드로 처리 작업을 하게 되면 실행 스레드를 전환하는 일이 발생하게 도 스레드 전환 비용에 의해 오히려 성능이 저할될 수 있다. 단, io 메서드로 가져온 스케줄러는 서로 다른 스레드가 동신에 접근하는 공유 I/O 처리 작업에서 사용하기에는 적절지 않다. 이런 i/o 처리 작업을 해야할 때는 스레드 안전을 보장하게 구현하거나 single 메서드로 가져온 스케줄러가 제공하는 공통 스레드에서만 처리 작업을 하게 해야 한다.
***
출처: https://www.notion.so/2-1e43be8260114fbcb78958bc7eae46b7

