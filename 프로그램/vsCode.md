# c++ 개발 환경 만들기
https://webnautes.tistory.com/1158

시스템 속성 열기 : 실행에서 sysdm.cpl 

## 확장

### Region Viewer

맥스스크립트에 적용

설정에 다음을 추가
```json
"maxscript": {
"start": "^\\s*--\\s*#?region\\b(?<name>.*)",
"end": "\\s&--\\s*#?endregion\\b"
}
```
