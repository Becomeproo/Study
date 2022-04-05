# View Lifecycle
Activity가 라이프사이클을 가지고 있는 것처럼 뷰도 라이프사이클을 가지고 있다. <br>
여기서 뷰란? 우리가 앱을 실해앟면 보이는 것 전부가 View라고 할 수 있다. View는 Button, TextView, ImageView 등의 위젯을 작성하는데 사용되는 기본 클래스이다. View의 또 다른 서브클래스인 ViewGroup은 보이지 않는 컨테이너로서 다른 View들을, 다른 View를 포함할 수 있다. <br>
![image](https://user-images.githubusercontent.com/91411447/161700313-374e0bae-a575-4540-8dea-2312d769be11.png)

## View Lifecycle
화면에 렌더링 된 View는 아래 그림과 같은 Lifecycle을 거쳐 화면에 그려진다. <br>
![image](https://user-images.githubusercontent.com/91411447/161700454-199976fb-463e-4261-a713-c3533519d3ae.png)

### Constructors
우리는 Custom View를 만들 때 생성자를 사용하는데 어떤 생성자를 구현 해야하는지 혼란스러울 때가 있다.
```
View (Context context)
View (Context context, @Nullable AttributeSet attrs)
View (Context context, @Nullable AttributeSet attrs, int defStyleAttr)
View (Context context, @NUllable AttributeSet attrs, int defStyleAttr, int defStyleRes)
```

* View(Context context)
  * 코드에서 View를 동적으로 만들 때 사용하는 간단한 Constructors이다. 여기서 매개변수 context는 View가 실행될 때 현재 테마, 리소스 등을 구성하는데 사용된다.
* View(Context context, @Nullable AttributeSet attrs)
  * XML에서 View를 inflation할 때 호출되는 생성자로, XML 파일에서 지정된 속성을 제공하여 XML 파일에서 View를 구성할 때 호출된다. 이 생성자는 기본 스타일인 0을 사용하므로 context의 테마 및 지정된 AttributeSet의 속성값만 적용된다.
* View(Context context, @Nullable AttributeSet attrs, int defStyleAttr)
  * XML을 통해 전개를 하고 테마 속성에서 클래스별 기본 스타일을 적용한다. 이 View 생성자는 서브 클래스가 전개할 때 자체 기본 스타일을 사용할 수 있도록 한다. 예를 들어, Button 클래스의 생성자는 수퍼 클래스 생성자를 호출하고 defStyleAttr에 R.attr.buttonStyle을 제공한다. 이를 통해 테마의 버튼 스타일은 모든 기본 View 속성(특히 배경)과 Button 클래스의 속성을 수정할 수 있다. defStyleAttr 매개변수는 View의 기본값을 제공하는 Style 리소스에 대한 참조를 포함하는 현재 테마의 속성이다. 기본값을 찾지 않으려면 0으로 지정할 수 있다.
* View(Context context, @NUllable AttributeSet attrs, int defStyleAttr, int defStyleRes)
  * XML을 전개하고 테마 속성 또는 Style 리소스에서 클래스 별 기본 스타일을 적용한다. 이 생성자는 서브 클래스가 전개할 때 자체 기본 스타일을 사용할 수 있도록 한다. 위와 유사하다. 매개변수 defStyleRes는 View의 defStyleAttr가 0이거나 테마에서 찾을 수 없는 경우에만 기본값을 제공하는 Style 리소스 ID이다. 기본값을 찾지 않으려면 0으로 지정한다.

* Attachment / Detachment
  * View가 Window에서 연결(Attachment)되거나 분리(Detachment)될 때의 단계
  * 이 단계에서는 적절한 작업을 수행하기 위해 콜백을 받는 몇 가지 방법이 있다.
* onAttachedToWindow()
  * View가 Window에 연결되면 호출된다. View가 활성화 될 수 있고, 드로잉할 표면이 있음을 알고있는 단계이다. 따라서 리소스 할당을 시작하거나 리스너를 설정할 수 있다.
* onDetachedFromWindow()
  * View가 Window에서 분리될 때 호출된다. 이 시점에서 더 이상 드로잉을 할 표면이 없다. 예약된 자원을 정리하거나 정리하는 모든 종류의 작업을 중지해야하는 곳이다. 이 메서드는 ViewGroup에서 View 제거를 호출하거나 액티비티가 Destroyed될 때 호출한다.
* onFinishInflate()
  * 이 메서드는 View가 전개가 끝날 때 호출된다. 레이아웃의 경우 모든 ChildView가 추가된 후에 호출된다.

### Traversals(순회)
View 계층 구조는 부모노드(ViewGroup)에서 분기가 있는 리프노드(Child Views)의 트리 구조와 같기 때문에 순회 단계라고 한다.

Animate -> Measure -> Layout -> Draw

* onMeasure
  * View의 크기를 확인하기 위해 호출된다. ViewGroup의 경우 계속해서 각 Child View에 대한 측정을 하고, 그에 대한 결과로 자신의 사이즈를 결정한다.
  ```
  onMeasure(int widthMeasureSpec, int heightMeasureSpec)
  @param widthMeasureSpec 부모 뷰에 의해 적용된 수평 공간 요구사항
  @param heightMeasureSpec 부모 뷰에 의해 적용된 수직 공간 요구사항
  ```
  onMeasure()는 값을 반환하지 않고, setMeasuredDimension()을 호출하여 너비와 높이를 명시적으로 설정한다.
  
  * MeasureSpec
    * MeasureSpec은 부모에서 자식으로 전달되는 레이아웃 요구사항을 캡슐화한다. 각 MeasureSpec은 너비 또는 높이에 대한 요구사항을 나타낸다. MeasureSpec은 크기와 모드로 구성되며, 세 가지 모드가 있다.
      * MeasureSpec.EXACTLY
        부모 뷰가 자식 뷰의 정확한 크기를 결정한다. 자식 뷰의 사이즈와 관계없이 주어진 경계내에서 사이즈가 결정된다.
      * MeasureSpec.AT_MOST
        자식 뷰는 지정된 크기까지 원하는 만큼 커질 수 있다.
      * MeasureSpec.UNSPECIFIED
        부모 뷰가 자식 뷰에 제한을 두지 않기 때문에, 자식 뷰는 원하는 크기가 될 수 있다.
* onLayout()
  뷰를 측정하여 화면에 배치한 후에 호출된다.
  
* onDraw()
  크기와 위치는 이전 단계에서 계산되므로 View는 그것들을 기준으로 그릴 수 있다. onDraw(Canvas)메서드에서 생성된 캔버스 객체에는 GPU로 보낼 OpenGL-ES 명령목록(displayList)이 있다. onDraw()는 여러번 호출되므로 이곳에서 객체를 만들면 안된다. <br>
  
특정 뷰의 속성이 변경되었을 때 실행되는 두 가지 메서드가 있다
* inValidate()
  변경사항을 보여주고자 하는 특정뷰에 대해 강제로 다시 그리기를 요구하는 메서드이다. 뷰 모양이 변경되면 invalidate()를 호출해야한다고 말할 수 있다.
* requestLayout()
  어떤 시점에서 뷰의 경계가 변경되었다면, View를 다시 측정하기 위해 이 메서드를 호출하여 Measure 및 Layout 단계를 다시 거칠 수 있다.
  > View에서 메서드를 호출할 때는 항상 UI 스레드 내에서 수행해야 한다. 다른 스레드에서 작업하고 있고, 해당 스레드에서 View의 상태를 업데이트 하려는 경우 Handler를 사용해야 한다.

## 상태의 저장과 복구
* onSaveInstanceState()
  먼저 상태를 저장하려면 ID를 제공해야 한다. View 계층에 동일한 ID를 가진 여러개의 뷰가 있는 경우 고유한 ID를 지정하여 모든 상태를 저장할 수 있도록 한다. <br>
  View.BaseSavedState를 확장하여 속성값을 저장하는 클래스가 필요하다.
* onRestoreInstaceState(Parcelable state)
  여기서는 이 메서드를 재정의하고 Parcelable에서 데이터를 읽은 다음 Parcelable에서 사용 가능한 데이터를 기반으로 로직을 작성해야 한다.
  ```
  @Override
  public Parcelable onSaveInstanceState() {
      Bundle bundle = new Bundle();
      // The vars you want to save - in this instance a string and a boolean
      String someString = "something";
      boolean someBoolean = true;
      State state = new State(super.onSaveInstanceState(), someString, someBoolean);
      bundle.putParcelable(State.STATE, state);
      return bundle;
  }

  @Override
  public void onRestoreInstanceState(Parcelable state) {
      if (state instanceof Bundle) {
          Bundle bundle = (Bundle) state;
          State customViewState = (State) bundle.getParcelable(State.STATE);
          // The vars you saved - do whatever you want with them
          String someString = customViewState.getText();
          boolean someBoolean = customViewState.isSomethingShowing());
          super.onRestoreInstanceState(customViewState.getSuperState());
          return;
      }
      // Stops a bug with the wrong state being passed to the super
      super.onRestoreInstanceState(BaseSavedState.EMPTY_STATE); 
  }

  protected static class State extends BaseSavedState {
      protected static final String STATE = "YourCustomView.STATE";

      private final String someText;
      private final boolean somethingShowing;

      public State(Parcelable superState, String someText, boolean somethingShowing) {
          super(superState);
          this.someText = someText;
          this.somethingShowing = somethingShowing;
      }

      public String getText(){
          return this.someText;
      }

      public boolean isSomethingShowing(){
          return this.somethingShowing;
      }
  }
  ```
  
## Activity Lifecycle <-> View Lifecycle Log
* 뷰가 그려지는 과정 <br>
  ![image](https://user-images.githubusercontent.com/91411447/161705246-e2468b6f-9950-4966-808a-d4d7a3ba2e05.png)

* 백그라운드로 갔을 때 <br>
  ![image](https://user-images.githubusercontent.com/91411447/161705289-c9845a3c-ba58-4a09-9ec8-82a096a82234.png)

* 백그라운드에서 포그라운드로 돌아올 때 <br>
  ![image](https://user-images.githubusercontent.com/91411447/161705345-1b1a2e23-c301-4217-a89b-4d0a4b0637d4.png)

* 액티비티 종료 <br>
  ![image](https://user-images.githubusercontent.com/91411447/161705432-412422f6-2e1d-4054-adb2-0c8255ac2646.png)

* 화면 회전 <br>
  ![image](https://user-images.githubusercontent.com/91411447/161705480-993bba27-4f73-4095-bb72-10729631b642.png)
  
<br>

![image](https://user-images.githubusercontent.com/91411447/161705539-bbde9f65-14b9-4306-b92d-1e5e321552b6.png)
***
출처: https://www.notion.so/View-Lifecycle-7cf2f8d8de714c05b3560e88968452fb
  
