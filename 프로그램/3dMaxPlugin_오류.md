# 3dsMaxPlugin_오류


## 시스탬 경로가 잘못되어있는경우

- .vcxproj 파일을 직접 열어서 수정
- ![적용된 파일을 찾지 못해서 새로 추가](https://user-images.githubusercontent.com/19432509/219955983-bc08e61c-b651-46fe-aee7-1293073f4fd5.png)

> [기본적으로 SDK의 `$(QTDIR)`은 `<maxsdk>\Qt\<QtVersion>`로 정의됩니다. 따라서 Qt 설치 내용을이 위치에 복사 할 수 있습니다. 또는:
`<maxsdk>\ProjectSettings\propertySheets\3dsmax.general.project.settings.props`에서 `$(QTDIR)`의 정의를 Qt의 설치된 버전을 가리키도록 변경합니다.](https://forums.autodesk.com/t5/3ds-max-programming/qt-installation/m-p/10025579)
  
- `ui_plugin_form.h`파일은 `plugin_form.ui`을 컴파일해야 
