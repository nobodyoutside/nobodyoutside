# 3dsMaxPlugin

## 자료

- 샘플 저장소 (포크) : https://github.com/nobodyoutside/3dsMax-Learning-Path.git
  - 2020버전 기준임.
- 튜토리얼 : https://help.autodesk.com/view/MAXDEV/2023/ENU/?guid=getting_started_writing_plug-ins

2023버전 기반 버전 정보
| 3ds        |Max	Target Operating Systems	| Compatible 3ds Max SDK |	C++ Compiler  | 	Minimum .NET Framework	|  Qt  |
| ----       | ----                         | ----                   |      ----  |         ----  | ---- |
|2023 (25.0) |	Windows 10 x64              | Windows 11              | 2023	Microsoft Visual Studio 2019, version 16.10.4, C++ Platform Toolset v142, Windows Platform SDK 10.0.19041.0	| .NET 4.8 |	Qt 5.15.1 |


## vs 프로젝트

- 일반 속성
  -  출력 디렉토리 : <3ds Max>/plugins
  -  플랫폼 도구 집합 : v141
  -  대상 확장자: .dlu
  -  구성 유형: 동적 라이브러리(.DLL)
  -  문자 집합: 유니코드 문자 집합 사
