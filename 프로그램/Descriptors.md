# Class Descriptors(클래스 설명자)

[help](https://help.autodesk.com/view/MAXDEV/2023/ENU/?guid=class_descriptors)

```cpp
class WidgetClassDesc : public ClassDesc2 {
public:
   int          IsPublic()     { return TRUE; }
   void*        Create(BOOL loading = FALSE) { return new Widget(); }
   const TCHAR* ClassName()    { return GetResourceString(IDS_CLASS_NAME); }
   SClass_ID    SuperClassID() { return GEOMOBJECT_CLASS_ID; }
   Class_ID     ClassID()      { return Widget_CLASS_ID; }
   const TCHAR* Category()     { return GetResourceString(IDS_CATEGORY); }

   // 고정 구문 분석 가능 이름(스크립터-벡터 이름)을 반환합니다
   const TCHAR* InternalName() { return _T("Widget"); }   

   // returns owning module handle 소유 모듈 핸들을 반환합니다
   HINSTANCE    HInstance()    { return hInstance; }     
};
```

| 요소 | 클래스 내용|
| ---- | ---- |
|`IsPublic()` | `int IsPublic() { return TRUE; }`|
| `Create` | `void* Create(BOOL loading = FALSE) { return new Widget(); }`|
| `ClassName()` | `const TCHAR* ClassName() { return GetResourceString(IDS_CLASS_NAME); }`|
| `ClassID()` | `Class_ID ClassID() { return Widget_CLASS_ID; }`|
| `Category()` | `const TCHAR* Category()     { return GetResourceString(IDS_CATEGORY); }`|
| `InternalName()` | `const TCHAR* InternalName() { return _T("Widget"); }` |
| `HInstance()` | `HINSTANCE    HInstance()    { return hInstance; }` |

## IsPublic()

멤버 함수는 3ds Max 사용자 인터페이스를 통해 플러그인이 사용자에게 노출되어야 하는지 여부를 나타내는 부울 값을 반환

## Create()

3ds Max는 ClassDesc::Create플러그인 클래스의 새 인스턴스에 대한 포인터가 필요할 때 () 멤버 함수를 호출

예시)

`virtual void* Create(BOOL /*loading = FALSE*/) override { return UtilitySample::GetInstance(); } }`

클래스`UtilitySample`에서 `GetInstance()`가 싱글톤으로 `static UtilitySample theSample`를 반환한다.

```cpp
class UtilitySample : public UtilityObj, public QObject 
{
public:
    // Constructor/Destructor
    UtilitySample();
    virtual ~UtilitySample();
    virtual void DeleteThis() override {}
    
    virtual void BeginEditParams(Interface *ip,IUtil *iu) override;
    virtual void EndEditParams(Interface *ip,IUtil *iu) override;
    virtual void Init(HWND hWnd);
    virtual void Destroy(HWND hWnd);
    
    // Singleton access
    static UtilitySample* GetInstance() { 
        static UtilitySample theSample;
        return &theSample;
    }

private:
    void DoSomething();
    QWidget *widget;
    Ui::PluginRollup ui;
    IUtil* iu;
};
```

## ClassName()

```cpp
virtual const TCHAR * ClassName() override { return GetString(IDS_CLASS_NAME); }
```

- 멤버 `ClassDesc::ClassName()`함수는 잠재적으로 지역화된 클래스 이름을 반환
- 이름은 3ds Max 사용자 인터페이스의 플러그인 버튼에 나타납니다.
- 이름은 MAXScript에서 클래스의 이름으로도 사용
- 그러나 이 메서드에서 반환된 이름은 지역화된 이름일 수 있으므로 한 **로케일로 작성된 스크립트는 다른 로케일에서 제대로 작동하지 않을 수 있습니다.**
- `ClassDesc::UseOnlyInternalNameForMAXScriptExposure()true`를 구현하고 반환

## SuperClassID()

`ClassDesc::SuperClassID` 멤버 함수는 이 플러그인 클래스가 파생된 클래스를 설명하는 시스템 정의 상수를 반환

```cpp
SClass_ID             SuperClassID()          { return GEOMOBJECT_CLASS_ID; }
virtual SClass_ID     SuperClassID() override { return UTILITY_CLASS_ID; }
```

## ClassID()

`ClassDesc::ClassID()` 멤버 함수는 플러그인 개체의 고유 ID를 반환 해야 합니다.

```cpp
Class_ID     ClassID()      { return Widget_CLASS_ID; }
virtual Class_ID ClassID() override { return UTILITYSAMPLE_CLASS_ID; }
```

> ***기존 플러그인과의 충돌을 피하기 위해 이러한 ID를 생성하는 Generating Class IDs 라는 프로그램이 SDK와 함께 제공됩니다 . 이 프로그램을 사용하여 플러그인에 대한 클래스 ID를 만드는 것이 매우 중요합니다.***

## Category()

```cpp
const TCHAR* Category()     { return GetResourceString(IDS_CATEGORY); }
virtual const TCHAR*  Category() override { return GetString(IDS_CATEGORY); }
```

ClassDesc::Category는 명령 패널의 만들기 분기에 있는 맨 아래 드롭다운 목록에서 선택

- 예: "Standard Primitives", "Particle Systems" 등

## InternalName()

`ClassDesc::InternalName`는 플러그인에 대한 고정된 구문 분석 가능한 내부 이름을 반환

```cpp
// Returns fixed parsable name (scripter-visible name)
virtual const TCHAR*  InternalName() override { return _T("lesson1a"); } 

// 고정 구문 분석 가능 이름(스크립터-벡터 이름)을 반환합니다
const TCHAR* InternalName() { return _T("Widget"); }
```
