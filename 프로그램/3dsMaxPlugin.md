# 3dsMaxPlugin

## 자료

- 샘플 저장소 (포크) : https://github.com/nobodyoutside/3dsMax-Learning-Path.git
  - 2020버전 기준임.
- 튜토리얼 : https://help.autodesk.com/view/MAXDEV/2023/ENU/?guid=getting_started_writing_plug-ins

2023버전 기반 버전 정보
| 3ds        |Max	Target Operating Systems	| Compatible 3ds Max SDK |	C++ Compiler  | 	Minimum .NET Framework	|  Qt  |
| ----       | ----                         | ----                   |      ----  |         ----  | ---- |
|2023 (25.0) |	Windows 10 x64              | Windows 11              | 2023	Microsoft Visual Studio 2019, version 16.10.4, C++ Platform Toolset v142, Windows Platform SDK 10.0.19041.0	| .NET 4.8 |	Qt 5.15.1 |

## 시스탬 경로가 잘못되어있는경우

- .vcxproj 파일을 직접 열어서 수정
- ![적용된 파일을 찾지 못해서 새로 추가](https://user-images.githubusercontent.com/19432509/219955983-bc08e61c-b651-46fe-aee7-1293073f4fd5.png)
- 
> [기본적으로 SDK의 $(QTDIR)은 <maxsdk>\Qt\<QtVersion>로 정의됩니다. 따라서 Qt 설치 내용을이 위치에 복사 할 수 있습니다. 또는:
<maxsdk>\ProjectSettings\propertySheets\3dsmax.general.project.settings.props에서 $(QTDIR)의 정의를 Qt의 설치된 버전을 가리키도록 변경합니다.](https://forums.autodesk.com/t5/3ds-max-programming/qt-installation/m-p/10025579)
  
- `ui_plugin_form.h`파일은 `plugin_form.ui`을 컴파일해야 

## vs 프로젝트

- 일반 속성
  -  출력 디렉토리 : <3ds Max>/plugins (관리자 권한 필요)
  -  플랫폼 도구 집합 (Project Defaults) : Visual Studio 2017 (v141)
  -  대상 확장자: .dlu
  -  구성 유형: 동적 라이브러리(.DLL)
  -  문자 집합: 유니코드 문자 집합 사
  ![General 속성 페이지](https://user-images.githubusercontent.com/19432509/220116751-e7087b30-1777-4674-b446-615857c059ba.png)
  
- C/C++
  - General : Additional Include Directories : $(ADSK_3DSMAX_SDK_2023)\include;
- Linker
  - General : Additional Library Directories : $(ADSK_3DSMAX_SDK_2023)\lib\x64\Release;
  - Additional Dependencies : bmm.lib;core.lib;geom.lib;gfx.lib;mesh.lib;maxutil.lib;maxscrpt.lib;gup.lib;paramblk2.lib;
  - Module Definition File : MyTest.def
  

## 필요한 DLL 함수
 
### DllMain()
  
