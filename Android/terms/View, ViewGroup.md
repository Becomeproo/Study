# View, ViewGroup
## View
안드로이드 화면의 구성요소이다. 화면에 보이는 모든 것은 View이다.
![image](https://user-images.githubusercontent.com/91411447/160523451-a73bb931-4949-4cd2-9bab-d33f960043d7.png)
View는 자신이 화면 어디에 배치되어야 하는지에 대한 정보가 없다. View만으로 화면에 나타날 수 없다. View를 화면에 배치하기 위해서는 반드시 무언가가 필요하다. 그것이 바로 ViewGroup 혹은 ViewContainer이다.
## ViewGroup
n개의 View를 담을 수 있는 Container이다. ViewGroup 또한 View를 상속받아 만든 클래스 또 다른 말로는 Layout 이라고 한다.
![image](https://user-images.githubusercontent.com/91411447/160524013-16ebb212-e3d5-4f91-af8b-e63e74282792.png)
ViewGroup은 View만 배치가능하며 ViewGroup조차 View로 다룬다. 그래서 자식 ViewGroup을 배치할 수 있다.
***
출처: https://www.notion.so/View-and-ViewGroup-b60fe74ebae744708aa3785b037917d7
