- 👋 Hi, I’m @nobodyoutside
- 👀 - 🌱 - 💞️ - 📫  Gool Luck!

iphone

# [비주얼 스튜디오 Express 에서 afxres.h 에러가 났을 때!](https://comso.tistory.com/entry/%EB%B9%84%EC%A3%BC%EC%96%BC-%EC%8A%A4%ED%8A%9C%EB%94%94%EC%98%A4-Express-%EC%97%90%EC%84%9C)
```
비주얼 스튜디오 Express 에서 afxres.h 에러가 났을 때!
인간신 2014. 11. 14. 19:28
1>------ 빌드 시작: 프로젝트: first test, 구성: Debug Win32 ------

1>  first test.cpp

1>resource.rc(10): fatal error RC1015: cannot open include file 'afxres.h'.

1>  

========== 빌드: 성공 0, 실패 1, 최신 0, 생략 0 ==========


이런 오류가 날 경우



#include "afxres.h"

를

// #include "afxres.h"

#include <winresrc.h>

#define IDC_STATIC (-1)



로 수정해 주세요.



실행되기도 하더군요.



afxres.h 헤더가 MFC 라이브러리의 들어가 있는데 Express 에선 MFC 라이브러리를 지원안한다네요.

대체로 winresrc.h를 사용한다네요.
```
