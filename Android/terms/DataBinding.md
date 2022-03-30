# DataBinding
UI 요소와 데이터를 프로그램적 방식으로 연결하지 않고, 선언적 형식으로 결합할 수 있게 도와주는 라이브러리이다.

build.gradle(app)
```
dataBinding {
  enabled = ture
}
```
안드로이드 스튜디오 4.0 부터는
```
buildFeatures {
  dataBinding true
}
```

## Two-way data binding
뷰의 데이터가 변경될 때 마다 데이터를 가져오는 방식
```
android:text="@{viewModel.name}"
android:text="@={viewModel.name}"
```

## Binding Adapters
뷰에 어떤 값들을 적용할 것인지를 조작하기 위한 메서드
```
@BindingAdapter("app:goneUnless")
fun goneUnless(view: View, visible: Boolean) {
  view.visibility = if (visible) View.VISIBLE else View.GONE
}
```
***
출처: https://www.notion.so/DataBinding-b69c56ac00e549bd8dbc36318790cb4b
