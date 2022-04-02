# Image format

### px
화면을 구성하는 최소 단위

### dpi
1인치에 몇 픽셀이 들어가는지 나타내는 단위
ex) 160 dpi -> 1인치에 160 x 160 개의 픽셀이 들어간다.

### dp
* 픽셀 독립 단위
* 화면의 크기가 달라도 동일한 비율로 보여주기 위해 안드로이드에서 정의한 단위

현재까지 엄청나게 많은 디바이스가 출시되었고 매년 새로운 스마트폰이 출시된다. 그에 따라 디바이스 해상도도 달라지고, dpi가 달라진다. 그에 맞게 동일한 비율로 보일 수 있도록 대응하기 위해 안드로이드에서는 dp 단위를 사용한다.

여기서 핵심은 안드로이드의 기준 dpi는 160dip(mdpi)이다.

160dip = mdip 기준으로 1px은 1dp이다.

```
mdpi    = 160dip = 1x = default
hdpi    = 240dpi = 1.5x
xhdpi   = 320dpi = 2x
xxhdpi  = 480dpi = 3x
xxxhdpi = 640dpi = 4x
```
만약, 아이콘 이미지가 mdpi에 24x24px 이었다면, xhdpi에 맞게 작업해주려면 디자이너는 48x48px 사이즈로 작업을 해줘야 한다.

본론으로 가서,

이미지 포맷은 크게 비트맵 방식과 벡터 방식으로 구분할 수 있다.
비트맵은 정사각형 모양의 아주 작은 px들이 집합해 이미지를 형성한 파일로, 화면을 강제로 확대할수록 이미지가 깨지는 특성을 보인다. 반면, 벡터는 수학적 연산에 의한 그래픽 이미지로 점과 선의 연결을 통해 이미지를 표현한다. 벡터의 가장 큰 장점은 사이즈를 아무리 크게 해도 그래픽이 깨지지 않는다는 특성이 있다.

비트맵 방식의 확장자는 대표적으로 png, jpeg, gif 등이 있고 <br>
벡터 방식의 확장자는 ai, svg 등이 있다.

## 안드로이드에서 이미지를 적용할 때 어떤 방식을 사용해야할까?
정답은 없다. 상황에 따라 다르고 어떤 이미지냐 등의 변수가 있다. <br>
기존에는 png 이미지를 export해서 해상도 별로 대응했으나 최근에는 svg로 변환해 drawable.xml로 관리하는 방법을 지향하고 있다. 그 이유는 이미지 파일이 많으면 많을수록 앱의 크기가 생각보다 많이 늘어난다. 앱의 크기를 줄이기 위해 svg를 사용하는 이유도 있고 파일이 하나만 생기므로 관리하기가 편했다. <br>
벡터이미지는 비트맵보다 품질면에서 뛰어나나 렌더링 속도의 경우 네이티브 크기의 비트맵을 만들고 캐시하지 않는 한 비트맵이 더 빠르다고 한다. <br>
***
출처: https://www.notion.so/Image-format-670b0442fda94b07a6c0bd9effbf5dd3