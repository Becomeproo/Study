# String과 StringBuffer
## String
불변, 문자를 수정하려면 지우고 다시 생성(new) -> 문자열 연산이 많으면 기능이 떨어진다.

## StringBuffer
가변, 한번 만들고 필요할 때 크기를 변경하여 문자를 변경(append()와 같이)

## StringBuilder
동기화 지원x, 멀티스레드 환경에 부적합 -> 싱글 스레드에서 StringBuffer보다 좋음
***
출처: https://www.notion.so/imwj/Android-Interview-3ce7ddf12ddb413a9d2213173654d52c
