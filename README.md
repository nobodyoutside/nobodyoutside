- 👋 Hi, I’m @nobodyoutside
- 👀 - 🌱 - 💞️ - 📫  Gool Luck!

<!---
nobodyoutside/nobodyoutside is a ✨ special ✨ repository because its `README.md` (this file) appears on your GitHub profile.
You can click the Preview link to take a look at your changes.
--->

https://developer-talk.tistory.com/128
StringComparison 열거형
StringComparison 열거형은 문자열 검색 및 비교하는 메소드에서 검색 규칙을 설정하는 기능
입니다.
열거형 필드
필드 설명
CurrentCulture
문화권 구분 정렬 규칙 및 현재 문화권을 사용하여 문자열을 비교합니
다.
CurrentCultureIgnore
Case
문화권 구분 정렬 규칙 및 현재 문화권을 사용하고 비교되는 문자열의
대/소문자를 무시하여 문자열을 비교합니다.
InvariantCulture
문화권 구분 정렬 규칙 및 고정 문화권을 사용하여 문자열을 비교합니
다.
InvariantCultureIgno
reCase
문화권 구분 정렬 규칙 및 고정 문화권을 사용하고 비교되는 문자열의
대/소문자를 무시하여 문자열을 비교합니다.
Ordinal
서수(이진) 정렬 규칙을 사용하여 문자열을 비교합니다.
암화와 같은 대소문자를 구분하는 리소스를 비교할 때 적합합니다.
OrdinalIgnoreCase
서수(이진) 정렬 규칙을 사용하고 비교되는 문자열의 대/소문자를 무
시하여 문자열을 비교합니다.
경로 및 파일 이름과 같은 리소스를 비교할 때 적합합니다.
설명은 공식 사이트를 참고하여 작성하였는데, 설명에서 말하는 현재 문화권, 고정 문화권등..
이해하기 어려운 용어들이 나옵니다. 위 설명에서 작성한 용어들에 대해 정리하였습니다.
목차
열거형
용어 정
사용 방
메소드
참고
마무리

 String






[C#]StringComparison 열거형

     
평범한 직장인의 … 구독하기
22. 5. 9. 오전 11:52 [C#]StringComparison 열거형
https://developer-talk.tistory.com/128 3/7
용어 정리
현재 문화권
- 현재 지역에 따라 문자열을 설정하여 비교합니다.
- 소프트웨어가 실행되는 위치에 따라 다르게 비교될 수 있습니다.
고정 문화권
- 기본적으로 영어로 설정되어 있습니다.
- 소프트웨어가 실행 되는 위치와 무관하게 영어로 비교합니다.
서수(이진)
- 유니코드 값을 기반으로 비교합니다.
사용 방법 및 예시
독일어 문자 ß 는 ss로 사용할 수 있습니다. 아래 Wikipedia에 들어가셔서 Ctrl + F 단축키로 s
s 를 검색하면 ß 도 검색됩니다.
https://en.wikipedia.org/wiki/%C3%9F

