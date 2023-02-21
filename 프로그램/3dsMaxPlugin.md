# 3dsMaxPlugin

## 자료

- 샘플 저장소 (포크) : [3dsMax-Learning-Path](https://github.com/nobodyoutside/3dsMax-Learning-Path.git)
  - 2020버전 기준임.
- 튜토리얼 : [getting_started_writing_plug-ins](https://help.autodesk.com/view/MAXDEV/2023/ENU/?guid=getting_started_writing_plug-ins)

2023버전 기반 버전 정보
| 3ds        |Max Target Operating Systems  | Compatible 3ds Max SDK |  C++ Compiler  |  Minimum .NET Framework  |  Qt  |
| ----       | ----                         | ----                   |      ----  |         ----  | ---- |
|2023 (25.0) |  Windows 10 x64              | Windows 11              | 2023 Microsoft Visual Studio 2019, version 16.10.4, C++ Platform Toolset v142, Windows Platform SDK 10.0.19041.0  | .NET 4.8 |  Qt 5.15.1 |

## vs 프로젝트

- 일반 속성
  - 출력 디렉토리 : <3ds Max>/plugins (관리자 권한 필요)
  - 플랫폼 도구 집합 (Project Defaults) : Visual Studio 2017 (v141)
  - 대상 확장자: .dlu
  - 구성 유형: 동적 라이브러리(.DLL)
  - 문자 집합: 유니코드 문자 집합 사
![General 속성 페이지](https://user-images.githubusercontent.com/19432509/220116751-e7087b30-1777-4674-b446-615857c059ba.png)

- C/C++
  - General : Additional Include Directories : $(ADSK_3DSMAX_SDK_2023)\include;
- Linker
  - General : Additional Library Directories : $(ADSK_3DSMAX_SDK_2023)\lib\x64\Release;
  - Additional Dependencies : bmm.lib;core.lib;geom.lib;gfx.lib;mesh.lib;maxutil.lib;maxscrpt.lib;gup.lib;paramblk2.lib;
  - Module Definition File : MyTest.def

## 필수요소

- DllMain() : Windows에서 호출하는 DLL의 진입점 함수
- Def파일 : DEF는 내보내는 DLL의 함수를 나열

### DllMain() dll 함수

필수 함수들

- `LibNumberClasses()` -> `int` : dll에서 사용하는 클래스의 수를 반환
- `LibClassDesc()` -> `ClassDesc2` : 를 반환
- `LibDescription()` -> `TCHAR` : dll을 설명하는 문자열 반환
- `LibVersion()` -> `ULONG`(`unsigned long`) :  컴파일된 시스템의 버전을 나타내는 미리 정의된 상수를 반환
- `LibInitialize()` -> `int`(TRUE 1/FALSE 0) : 일회성 플러그인 초기화를 수행 확인
- `LibShutdown()` -> `int` : 플러그인이 언로드되기 직전에 한 번 호출됩니다. 이 방법에서 일회성 플러그인 초기화 해제를 수행, 반환값 무시


