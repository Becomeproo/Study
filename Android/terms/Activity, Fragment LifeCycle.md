# Activity, Fragment LifeCycle

## Activity LifeCycle
Activity LifeCycle은 Activity가 시작되고 종료되는 시점까지의 상태를 Activity LifeCycle이라 한다. <br>
Activity LifeCycle에는 onCreate(), onStart(), onPause(), onStop(), onDestroy(), onRestart()가 있다.
![Activity LifeCycle](https://user-images.githubusercontent.com/91411447/159403140-87f0e6af-842b-4cc1-94d0-a3d47985d3d9.png)
### onCreate()
- 이 함수는 필수적으로 구현해야한다. <br>
- 전체 LifeCycle 동안 "한 번"만 발생한다. <br>
- 이 메서드에는 xml, 멤버 변수 정의, 일부 UI 구성 등 설정을 한다.

### onStart()
- 활성 상태에 들어가면 이 함수가 호출된다. (사용자에게 보여지기 직전)
- 호출되면 포그라운드에 보내 상호작용을 할 수 있도록 준비한다.
- 주로 UI를 관리하는 코드를 초기화 한다. 이 메서드는 매우 빠르게 완료되고 바로 onResume()을 호출한다.

### onResume()
- 사용자한테 화면에 보여지고 상호 작용하는 메서드이다.
- 어떤 이벤트가 발생하여 앱에서 포커스가 떠날 때까지 이 상태에 머무른다.
- 이 상태에서는 생명주기 구성요소가 포그라운드에서 사용자에게 보이는 동안 실행해야 하는 모든 기능을 활성화한다. (예: 카메라 미리보기)
- 방해가 되는 이벤트가 발생하면 일시중지 상태에 들어가고, 시스템이 onPause()를 호출한다. (예: 전화가 오거나, 사용자가 다른화면을 이동하거나, 기기 화면이 off될 때)

### onPause()
- 사용자가 화면을 떠날 때, 시스템이 첫 번째로 이 메서드를 호출한다. (상태가 포그라운드에 있지 않게 되었따는 것을 나타냄)
- 포그라운드에 있지 않을 때 실행할 필요가 없는 기능을 모두 정지할 수 있다. (예: 카메라 미리보기 정지)
- 시스템 리소스, 센서 핸들(GPS), 사용자가 필요로 하지 않을 때 배터리 수명에 영향을 미칠 수 있는 모든 리소스를 해제할 수도 있다. (UI 관련 리소스와 작업을 완전히 해제하거나 조정할 때는 onPuase보다 onStop을 사용하는 것이 좋다. 멀티윈도우 모드 or 화면분할 때문)
- 이 메서드는 아주 잠깐 실행되므로 저장 작업을 하기에는 시작이 부족할 수 있다. 사용자 데이터를 저장하거나, 네트워크를 호출하거나, 데이터베이스 트랜잭션을 실행해서는 안된다. 이 메서드가 끝나기 전에 완료하지 못할 수 있다. 무거운 작업을 onStop에서 하고 저장은 viewModel, onSaveInstanceState()를 참조.
> 일반적인 다이얼로그는 activity가 아니기 때문에 onPause()를 호출하지 않는다. 권한 요청은 다이얼로그처럼 보이는 UI일 뿐 실제로 동작은 권한을 허용할 것인지, 거절할 것인지에 대한 UI를 띄우므로 onPause()를 호출한다.

### onStop()
- 포커스가 완전히 빠졌을 때 시스템은 이 콜백 메서드를 호출한다. (화면 전체가 가려졌을 때 또는 백그라운드로 갔을 때)
- 이 메서드에서는 앱이 사용자에게 보이지 않는 동안 앱이 필요하지 않는 리소스를 해제하거나 조정해야 한다. (예: 애니메이션 중지, 세밀한 위치 업데이트에서 대략적인 위치 업데이트로 전환할 수 있다.)
- onStop()에서 무거운 작업을 실행해야 한다고 했는데 예를 들어, 정보를 데이터베이스에 저장해야하는데 적절한 위치를 찾지 못했다면 이 메서드에서 저장할 수 있다. (하지만 이 함수는 항상 호출되는 것은 아니며 메모리가 부족할 경우 호출이 안될수도 있다.

### onDestroy()
- Activity가 소멸되기 전에 호출된다. 시스템은 다음 중 하나에 해당할 때 이 콜백을 호출한다.
1. 활동이 종료되는 경우 (스택에서 날리거나 finish()를 호출)
2. 구성 변경 (예: 기기회전 또는 멀티 윈도우모드)으로 인해 시스템이 일시적으로 활동을 소멸시키는 경우
- onStop()에서 해제하지 않은 모든 리소스를 해제해야 한다.
- 이 메서드가 호출되는 경우 시스템이 즉시 인스턴스를 생성한 다음, 새로운 구성에서 인스턴스에 관해 onCreate()를 호출한다.

### onRestart()
- onStop()상태에 있던 화면이 다시 접근했을 때 호출되는 콜백함수

### Activity에서 Activity로 이동할 때 LifeCycle 순서
1. 액티비티에서 문자가 왔을 경우 (화면이 일부 가려졌을 때) <br>
  - onCreate() ~ onResume() -> 문자옴 -> onPause() -> 문자사라짐 -> onResume()
2. A 액티비티에서 B 액티비티로 이동
  - A oncreate() ~ onResume() -> 화면이동 클릭 -> A onPause() -> B onCreate() ~ onResume() -> A onStop() ~ onDestroy() (상황에 따라)
3. 액티비티에서 백그라운드로 갔다 다시 포그라운드로 복귀 시
  - onCreate() ~ onResume() -> 홈버튼(백그라운드) -> onPause() -> onStop() -> 앱 복귀 -> onRestart() -> onStart() -> onResume()

### Activity에서 configuration changes가 될 때 LifeCycle 순서
![Activity에서 configuration changes가 될 때 LifeCycle 순서](https://user-images.githubusercontent.com/91411447/159406270-45602d34-a00b-4c33-8b37-c62c0c21eee5.png)

# Fragment LifeCycle
Fragment LifeCycle은 Fragment가 시작되고 종료될 때까지 상태를 Fragment LifeCycle이라고 한다.
Fragment LifeCycle에는 onAttach(), onCreate(), onCreateView(), onActivityCreated(), onStart(), onResume(), onPause(), onStop(), onDestroyView(), onDestroy(), onDetach() 가 있다.
![Fragment LifeCycle](https://user-images.githubusercontent.com/91411447/159406563-4f6de61e-f2e1-42c5-8398-20c7724199f5.png)

### onAttach(Activity)
- 액티비티에서 프래그먼트가 호출될 때 최초 한번 호출되는 함수
### onCreate(Bundle)
- 프래그먼트가 생성될 때 호출되는 함수
### onCreateView(LayoutInflater, ViewGroup, Bundle)
- 프래그먼트의 뷰를 생성하는 함수
### onActivityCreated(Bundle)
- 액티비티에서 onCreate()가 호출된 프래그먼트에서 호출되는 함수
### onStart()
- 프래그먼트가 사용자한테 보여지기 직전 호출되는 함수
### onResume()
- 프래그먼트가 사용자와 상호작용할 수 있는 상태
### onPause()
- 화면이 일부 가려졌을 때 호출
### onStop()
- 프래그먼트가 화면에 사라졌을 때 호출
### onDestroyView()
- 프래그먼트의 View가 사라질 때 호출되는 함수
### onDestroy()
- 프래그먼트가 제거될 때 호출되는 함수
### onDetach()
- 프래그먼트가 액티비티와 연결이 종료될 때 호출되는 함수

<br> 

### FragmentManager
- 프래그먼트를 추가, 삭제 또는 교체하고 백스택에 추가하는 등의 작업을 실행하는 클래스
- 프래그먼트의 변경사항 집합을 트랜잭션이라고 한다.

### FragmentTransaction
- 각 트랜잭션은 수행하고자 하는 변경사항의 집합이다. 변경사항을 설정하려면 add(), remove(), replace()와 같은 메서드를 사용해야 한다.

![image](https://user-images.githubusercontent.com/91411447/159407119-c9d717c5-400f-4eb3-9069-ea94b6569971.png)
![image](https://user-images.githubusercontent.com/91411447/159407132-ffa05ed1-ea29-4256-8e05-af1c50db9e21.png)

***
출처 : https://www.notion.so/Activity-Fragment-LifeCycle-a5ba57dc06d1487da622358989f5a724
