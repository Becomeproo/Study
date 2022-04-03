# FragmentFactory
Fragment를 만들 때 보편적으로 사용하는 방식은
```
val fargment = FirstFragment()
```
위처럼 사용할 것이다. Fragment를 생성하는 쪽에서 데이터를 넘겨줘야 하는 경우가 종종 있는데 이럴때 보편적으로
```
val age = 1
val fragment = FirstFragment(age)
```
위처럼 사용했다. 이렇게 생성자로 바로 넘겨주는 방법을 사용했다가 앱이 강제 종료되는 현상을 한번쯤은 겪어보지 않았을까 싶다. Fragment의 공식 문서를 확인하면 주의사항이 적혀있다. <br>
![image](https://user-images.githubusercontent.com/91411447/161406896-1c949773-6a5d-4656-b757-84a5f80a37b3.png)
Fragment를 상속받을 때는 인자가 없는 기본 생성자를 반드시 포함해야 한다는 내용이다. Fragment는 자기만의 라이프사이클을 갖고 있지만 액티비티 라이프사이클에 종속적이다. 따라서 메모리가 부족하거나 화면회전, 디스플레이 변경 등의 다양한 이벤트로 액티비티가 재생성 될 때 프래그먼트도 재생성 된다. <br>
위와 같은 이유로 프래그먼트의 잦은 재생성이 일어난다. 이를 해결하는 방법으로 newInstance()를 활용하는 것이다.
```
companion object {
  fun newInstance(): ThirdFragment (
    val fragment = ThirdFragment()
    val args = Bundle()
    args.putInt("test", 1)
    fragment.argguments = args
    
    return fragment
  }
}
```
Fragment 내부에 newInstance() 이름의 static 메서드를 만들고, 빈 생성자로 Fragment 인스턴스를 생성한다. <br>
key-value 형식으로 bundle에 데이터를 저장한다.
arguments로 Fragment에 전달할 데이터를 설정한다.
Fragment에선 arguments로 전달된 데이터를 꺼내서 사용할 수 있다.
```
val age = 1
val fragment = FirstFragment.newInstance(age)
```
AndroidX 부터는 instance()는 deprecated 되었고 FragmentFactory의 instantiate를 사용해 구현하라고 나와있다. <br>
![image](https://user-images.githubusercontent.com/91411447/161407009-a394536a-edec-408e-aae1-5144467add36.png)

위 방법을 사용하는 이유는 기존에는 프래그먼트에 빈 생성자가 없다면 구성 변경 및 앱의 프로세스 재생성과 같은 특정 상황에서 시스템은 프래그먼트를 초기화하지 못했다. 이 점을 해결하기 위해 FragmentFactory를 사용하게 되었고 Fragment에 필요한 인수 및 종속성을 제고하여 시스템이 Fragment를 더욱 잘 초기화하는데 도움을 준다.
```
class FragmentFactoryImpl(private val age: Int) : FragmentFactory() {
  
  override fun instantiate(classLoader: ClassLoader, className: tring): Fragment {
    return when (className) {
      FirstFragment::class.java.name -> {
        FirstFragment()
      }
      SecondFragment::class.java.name -> {
        SecondFragment(age)
      }
      ThirdFragment::class.java.name -> {
        ThirdFragment().apply {
          arguments = Bundle().apply {
            putInt("test", age)
          }
        }
      }
      else -> super.instantiate(classLoader, className)
    }
  }
}
```
<br>
```
override fun onCreate(savedInstanceState Bundle?) {
  super.onCreate(savedInstanceState)
  supportFragmentManager.fragmentFactory = FragmentFactoryImpl(100)
  
  setContentView(R.layout.activity_factory)
  
  initView()
  initListener()
}

private fun initView() {
  frame = findView...
  btnFirst = find...
  btnSeconds = find...
  btnThird = find....
}

private fun initListener() {
  btnFirst.setOnClickListener {
    fragment = supportFragmentManager.fragmentFactory.instantiate(classLoader, FirstFragment::class.java.name)
    supportFragmentManager.beginTransaction()
      .replace(R.id.frame_layout, fragment)
      .commitNow()
  }
  btnSecon.setOnClickListener {
    fragment = supportFragmentManager.fragmentFactory.instantiate(classLoader, SecondFragment::class.java.name)
    supportFragmentManager.beginTransaction()
      .replace(R.id.frame_layout, fragment)
      .commitNow()
  }
  btnThird.setOnClickListener {
    fragment = supportFragmentManager.fragmentFactory.instantiate(classLoader, ThirdFragment::class.java.name)
    supportFragmentManager.beginTransaction()
      .replace(R.id.frame_layout, fragment)
      .commitNow()
  }
}
```
![image](https://user-images.githubusercontent.com/91411447/161407172-a6b7f019-1c7c-4ba1-a38c-29bc86c19784.png)

위와 같이 사용하면 Fragment.java에 있던 instantiate()를 사용하지 않고 FragmentFactory의 instantiate를 사용해 초기화하게 된다. 여기서 중요한 것은
```
supportFragmentManager.fragmentFactory = FragmentFactoryImpl()
```
위 코드를 Activity의 onCreate()보다 먼저 선언해주어야 한다. <br>
이렇게 하면 빈 생성자를 굳이 생성할 필요가 없으며 bundle에 데이터를 넣지 않아도 재생성 상황에서 데이터가 보존된다.

그렇다면 FragmentFactory를 꼭 사용해야 할까?
-> 기존 방식대로(newInstance) 사용해도 무방하다. 다만 인자가 있는 생성자를 상요할 시 반드시 빈 생성자 프래그먼트를 만들어 주어야한다. 만약 인자가 있는 생성자와 FragmentFactory를 사용한다면 위 방법을 권장한다. <br>
또한 koin을 사용하고 있다면 fragmentFactory()를 사용해 좀 더 쉽게 인자가 있는 프래그먼트를 주입할 수 있다.
***
출처: https://www.notion.so/FragmentFactory-8f3ad6874e9d43eba0af054b72b5a738
  
