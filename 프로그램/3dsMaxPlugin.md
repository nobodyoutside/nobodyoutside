# 3dsMaxPlugin

## 자료

- 샘플 저장소 (포크) : [3dsMax-Learning-Path](https://github.com/nobodyoutside/3dsMax-Learning-Path.git)
  - 2020버전 기준임.
- 튜토리얼 : [getting_started_writing_plug-ins](https://help.autodesk.com/view/MAXDEV/2023/ENU/?guid=getting_started_writing_plug-ins)

2023버전 기반 버전 정보
| 3ds        |Max Target Operating Systems  | Compatible 3ds Max SDK |  C++ Compiler  |  Minimum .NET Framework  |  Qt  |
| ----       | ----                         | ----                   |      ----  |         ----  | ---- |
|2023 (25.0) |  Windows 10 x64              | Windows 11              | 2023 Microsoft Visual Studio 2019, version 16.10.4, C++ Platform Toolset v142, Windows Platform SDK 10.0.19041.0  | .NET 4.8 |  Qt 5.15.1 |

## 시스탬 경로가 잘못되어있는경우

- .vcxproj 파일을 직접 열어서 수정
- ![적용된 파일을 찾지 못해서 새로 추가](https://user-images.githubusercontent.com/19432509/219955983-bc08e61c-b651-46fe-aee7-1293073f4fd5.png)

> [기본적으로 SDK의 `$(QTDIR)`은 `<maxsdk>\Qt\<QtVersion>`로 정의됩니다. 따라서 Qt 설치 내용을이 위치에 복사 할 수 있습니다. 또는:
`<maxsdk>\ProjectSettings\propertySheets\3dsmax.general.project.settings.props`에서 `$(QTDIR)`의 정의를 Qt의 설치된 버전을 가리키도록 변경합니다.](https://forums.autodesk.com/t5/3ds-max-programming/qt-installation/m-p/10025579)
  
- `ui_plugin_form.h`파일은 `plugin_form.ui`을 컴파일해야 

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

## 필요한 DLL 함수

### DllMain()

Windows에서 호출하는 DLL의 진입점 함수

- DllMain()
- HINSTANCE
- DisableThreadLibraryCalls()
- DLL_THREAD_ATTACH
- DLL_THREAD_DETACH
- DllMain()
- LibInitializeLibShutdown

### LibNumberClasses()

이 함수는 DLL이 사용하는 플러그 인 클래스의 수를 반환합니다.

```cpp
__declspec( dllexport ) int LibNumberClasses()
{
  return 1;
}
```

### LibClassDesc()

이 함수는 DLL에서 내보낸 n번째 클래스에 대한 클래스 `descriptor()`를 반환

### [Class Descriptors](https://help.autodesk.com/view/MAXDEV/2023/ENU/?guid=class_descriptors)

- DLL에 구현된 각 플러그인에 대한 정보를 3ds Max에 제공
- `ClassDesc2` 클래스 설명자에는 플러그인에 사용되는 모든 매개 변수 블록에 대한 `ParamBlockDesc2`s 표와 블록 설명자, UI 관리, 자동 매개 변수 블록2 구성 및 `ParamMap2`에 대한 액세스를 위한 다양한 방법이 포함되어 있습니다.

| 요소 | 클래스 내용|
| ---- | ---- |
|`IsPublic()` | `int IsPublic() { return TRUE; }`|
| `Create` | `void* Create(BOOL loading = FALSE) { return new Widget(); }`|
| `ClassName()` | `const TCHAR* ClassName() { return GetResourceString(IDS_CLASS_NAME); }`|
| `ClassID()` | `Class_ID ClassID() { return Widget_CLASS_ID; }`|
| `Category()` | `const TCHAR* Category()     { return GetResourceString(IDS_CATEGORY); }`|
| `InternalName()` | `const TCHAR* InternalName() { return _T("Widget"); }` |
| `HInstance()` | `HINSTANCE    HInstance()    { return hInstance; }` |

```cpp
__declspec( dllexport ) ClassDesc* LibClassDesc(int i)
{
  switch(i)
  {
    case 0:
       return GetSimpleWidgetDesc();
  }

  return 0;
}
```
