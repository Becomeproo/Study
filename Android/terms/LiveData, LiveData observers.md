# LiveData, LiveData observers
LiveData는 LifeCycle 내에서 관찰할 수 있는 데이터 홀더 클래스이다.
Observer, LifeCycleOwner와 쌍으로 추가할 수 있다. Observer에는 래핑된 데이터의 수정에 대해 알림을 받는다. 단, LifeCycleOwner상태가 LifeCycle.State.STARTED or LifeCycle.State.RESUMED일 때 받을 수 있다. observeForever는 항상 활성 상태로 간주하여 항상 알림을 받을 수 있으며 수동으로 removeObserver로 옵저버를 제거할 수 있다. LiveCycle에 추가된 관찰자는 LifeCycle.State.DESTROYED 상태로 이동되면 옵저버가 즉시 구독이 취소되므로 메모리 누수에 대해 걱정할 필요가 없다는 장점을 가지고 있다. LiveData는 ViewModel의 데이터 필드를 보유하도록 설계되어 있다.
![image](https://user-images.githubusercontent.com/91411447/160732864-f39e0893-a2b6-4a41-a45b-f6d5dd022a18.png)

## LiveData 사용 시 장점
* UI가 데이터 상태와 일치하는지 확인할 수 있다.
  LiveData는 옵저버 패턴을 따른다. LiveData Observer는 기본 데이터가 변경될 때 구독자에게 알린다. 구독자는 알림을 받으면 해당 객체의 UI를 업데이트 할 수 있다.
* 메모리 누수가 없다.
  LiveData는 LifeCycle을 알고 있기 때문에 LifeCycleOwner의 상태가 LifeCycle.State.DESTROYED가 되면 자동으로 구독 취소된다.
* UI의 상태가 활성 상태가 아닐 때 이벤트를 수신하지 않는다.
* 항상 최신 데이터를 유지
  수명주기가 비활성화되면 다시 활성화 된 후 최신 데이터를 받는다. 예를 들어, 백그라운드에 활성 중이던 데이터가 포그라운드로 오면 최신 데이터를 받는다.
* 화면 회전 시 구성 변경
  디바이스가 회전 시 데이터들이 다시 생성되면 최신 데이터를 즉시 수신한다.
  
## LiveData가 LifeCycle을 어떻게 아는가?
![image](https://user-images.githubusercontent.com/91411447/160733216-462e798f-058f-447e-a20a-c340d0cf79d4.png)
![image](https://user-images.githubusercontent.com/91411447/160733221-0e7f5a4a-38e8-4df8-bb78-a2143e7c9835.png)

LifeCycleOwner는 SAM이며, 이를 Activity 또는 Fragment에서 이를 상속하고 있다.
즉, LiveData의 observe함수를 이용해 owner와 observer를 등록해 Activity or Fragment의 변수로서 사용한다면 각 화면 별 생명주기에 따라 LiveData는 자신의 임무를 수행한다.
```
viewModel.message.observe(LifecycleOwner등록, Observer {
  /* ..수행.. */
  })
```

> LiveData의 캡슐화
외부에서 직접 필드 내부값을 노출시켜 변경하는 것 보다는 변경할 수 있는 함수를 만들어서 노출한다. UI 컨트롤러는 데이터를 읽어야하므로 데이터 필드가 완전히 비공개 일 수는 없다. LiveData를 캡슐화 하려면 MutableLiveData와 LiveData를 함께 사용해야 한다.
* MutableLiveData
  말그대로 변경이 가능한 LiveData이다. MutableLiveData 클래스는 setValue(T), postValue(T)의 메서드를 공개로 노출한다. 일반적으로 MutableLiveData는 ViewModel에서 사용되며 ViewModel은 변경이 불가능한 LiveData 객체만 관찰자에게 노출한다.
```
public class MutableLiveData<T> extends LiveData<T> {
  
  public MutableLiveData(T value) {
    super(value);
  }
  
  public MutableLiveData() {
    super();
  }
  
  @Override
  public void postValue(T value) {
    super.postValue(value);
  }
  
  @Override
  public void setValue(T value) {
    super.setValue(value);
  }
}

SetValue

@MainThread
protected void setValue(T value) {
  assertMainThread("setValue");
  mVersion++;
  mData = value;
  dispatchingValue(null);
}
```
LiveData를 구독하고 있는 옵저버가 있는 상태에서 setValue(T)를 통해 변경시킨다면 메인 스레드에서 그 즉시 값이 반영된다. 여기서 중요한 점은 메인 스레드에서 값을 dispatch 시킨다는 점이다.
  * postValue
  ```
  portected void postValue(T value) {
    boolean postTask;
    synchronized (mDataLock) {
      postTask = mPendingData == NOT_SET;
      mPendingData = value;
    }
    if (!postTask) {
      return;
    }
    ArchTaskExecutor.getInstance().postToMainThread(mPostValueRunnable);
  }
  ```
  postValue 역시 메인스레드에서 동작한다. 하지만 그 과정이 백그라운드에서 진행된다고 보면 된다. 내부적으로 Handler를 통해 Handler(Looper.mainLooper()).post(() -> setValue()) 로 변경된다.
  ```
  liveData.postValue("a");
  liveData.setValue("b");
  ```
  위 코드가 실행된다면 b가 mainThread에 의해 set되고 그 이후 mainThread가 override 되어서 a가 set된다. postValue가 여러번 실행되면 마지막 값이 set된다.
***
출처: https://www.notion.so/LiveData-and-LiveData-observers-f8e8f70f75264dc48f3812bd6875ee56
