# Memory Leak and ORM
앱을 개발할 때는 제한된 메모리를 어떻게 사용해야 좋은 성능을 낼 수 있는지 항상 고민해야한다. <br>
GC를 통해 알아서 메모리를 관리해주지만 능사는 아니다. 아무리 관리를 해준다해도 개발자가 신경을 쓰지 않으면 메모리릭과 ORM이 발생하기 마련이다.

> Memory Leak 과 ORM
  메모리릭은 메모리 누수라고도 한다. 앱 사용이 끝난 메모리를 반환하지 않은 경우 메모리 릭이 발생한다. 이렇게 사용하지 않은 메모리 양이 계속 증가하게 되면 최악의 경우 Out Of Memory(ORM)을 발생시키고 앱이 강제 종료되면서 할당되었던 메모리가 시스템으로 회수된다.
  
## 메모리 릭의 원인
1. Use of Static view/context/activity
2. Register and unregister listeners
3. Non-Static inner class
4. Wrong use of getContext() and getApplicationContext()
***
출처: https://www.notion.so/Memory-Leak-and-ORM-0e37b1de8b0f49188db0b2897ca71470
