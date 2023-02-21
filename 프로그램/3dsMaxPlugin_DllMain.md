# 3dsMaxPlugin_DllMain

## LibNumberClasses()

이 함수는 DLL이 사용하는 플러그 인 클래스의 수를 반환합니다.

```cpp
__declspec( dllexport ) int LibNumberClasses()
{
  return 1;
}
```

## LibClassDesc()

이 함수는 DLL에서 내보낸 n번째 클래스에 대한 클래스 `descriptor()`를 반환

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

### [Class Descriptors]

[3ds Max help](https://help.autodesk.com/view/MAXDEV/2023/ENU/?guid=class_descriptors), [Descriptors.md](.//Descriptors.md)

- `ClassDesc2`클래스는 iparamb2.h 에 정의 되어있고 (C:\Program Files\Autodesk\3ds Max 2023 SDK\maxsdk\include\iparamb2.h) `ClassDesc(plugapi.h):MaxHeapOperators(MaxHeap.h)`을 상속
- DLL에 구현된 각 플러그인에 대한 정보를 3ds Max에 제공
- `ClassDesc2` 클래스 설명자에는 플러그인에 사용되는 모든 매개 변수 블록에 대한 `ParamBlockDesc2`s 표와 블록 설명자, UI 관리, 자동 매개 변수 블록2 구성 및 `ParamMap2`에 대한 액세스를 위한 다양한 방법이 포함되어 있습니다.

| 요소 | 값 |클래스 내용|
| ---- | ---- | --- | 
|`IsPublic()`| int | `int IsPublic() { return TRUE; }`|
| `Create` | void |`void* Create(BOOL loading = FALSE) { return new Widget(); }`|
| `ClassName()`| TCHAR | `const TCHAR* ClassName() { return GetResourceString(IDS_CLASS_NAME); }`|
| `ClassID()` | SClass_ID <- ulong <- unsigned long (maxtypes.h) | `Class_ID ClassID() { return Widget_CLASS_ID; }`|
| `Category()` | Class_ID <- MaxHeapOperators (maxtypes.h) |`const TCHAR* Category()     { return GetResourceString(IDS_CATEGORY); }`|
| `InternalName()`| TCHAR | `const TCHAR* InternalName() { return _T("Widget"); }` |
| `HInstance()` | HINSTANCE (minwindef.h) |`HINSTANCE    HInstance()    { return hInstance; }` |

```cpp
class UtilitySampleClassDesc : public ClassDesc2 
{
public:
	virtual int           IsPublic() override                       { return TRUE; }
	virtual void*         Create(BOOL /*loading = FALSE*/) override { return UtilitySample::GetInstance(); }
	virtual const TCHAR * ClassName() override                      { return GetString(IDS_CLASS_NAME); }
	virtual SClass_ID     SuperClassID() override                   { return UTILITY_CLASS_ID; }
	virtual Class_ID      ClassID() override                        { return UTILITYSAMPLE_CLASS_ID; }
	virtual const TCHAR*  Category() override                       { return GetString(IDS_CATEGORY); }

	virtual const TCHAR*  InternalName() override                   { return _T("lesson1a"); } // Returns fixed parsable name (scripter-visible name)
	virtual HINSTANCE     HInstance() override                      { return hInstance; } // Returns owning module handle
};

ClassDesc2* GetUtilitySampleDesc()
{
	static UtilitySampleClassDesc UtilitySampleDesc;
	return &UtilitySampleDesc; 
}
```

## LibDescription()

이 함수는 DLL을 설명하는 문자열을 반환합니다. 이것은 구현 DLL을 사용할 수 없을 때 장면에서 참조되는 엔티티를 식별하기 위해 3ds Max에서 사용됩니다. 예를 들어 시스템에 적절한 DLL이 없는 다른 3ds Max 사용자와 공유되는 장면에서 특정 재질이 사용되는 경우입니다. 따라서 문자열은 사용자가 DLL을 얻을 수 있는 위치를 언급해야 합니다.

```cpp
// 이 함수는 DLL과 사용자 위치를 설명하는 문자열을 반환합니다
// DLL이 없으면 획득할 수 있습니다.
__declspec( dllexport ) const TCHAR* LibDescription()
{
	return GetString(IDS_LIBDESCRIPTION);
}
```

## LibVersion()

이 함수는 컴파일된 시스템의 버전을 나타내는 미리 정의된 상수를 반환합니다. 3ds Max에서 사용되지 않는 DLL을 식별할 수 있도록 하는 데 사용됩니다. 이 함수는 3ds Max에서 플러그인을 로드한 직후에 한 번 호출됩니다. 일반적으로 `Get3DSMAXVersion()`.

```cpp
// This function returns a pre-defined constant indicating the version of 
// the system under which it was compiled.  It is used to allow the system
// to catch obsolete DLLs.
// 이 함수는 다음을 나타내는 미리 정의된 상수를 반환합니다.컴파일된 시스템의 버전입니다.
//그것이 시스템이 사용되지 않는 DLL을 catch할 수 있도록 하는 데 사용됩니다.
__declspec( dllexport ) ULONG LibVersion()
{
	return VERSION_3DSMAX;
}
```

## LibInitialize()

이 방법으로 일회성 플러그인 초기화를 수행합니다. 플러그인의 초기화가 성공적이면(예: 플러그인이 올바르게 로드된 경우) TRUE를 반환하고 그렇지 않으면 FALSE를 반환합니다. 함수가 FALSE를 반환하면 시스템은 플러그인을 로드하지 않고 FreeLibrary()DLL을 호출하고 메시지를 보냅니다.

```cpp
// This function is called once, right after your plugin has been loaded by 3ds Max. 
// 이 기능은 플러그인이 3ds Max로 로드된 직후에 한 번 호출됩니다.
// Perform one-time plugin initialization in this method.
// 이 방법으로 일회성 플러그인 초기화를 수행합니다.
// Return TRUE if you deem your plugin successfully loaded, or FALSE otherwise. If 
// 플러그인이 성공적으로 로드되었다고 판단되면 TRUE를 반환하고, 그렇지 않으면 FALSE를 반환합니다. 한다면
// the function returns FALSE, the system will NOT load the plugin, it will then call FreeLibrary
// 함수가 FALSE를 반환합니다. 시스템이 플러그인을 로드하지 않고 Free Library를 호출합니다
// on your DLL, and send you a message.
// DLL에서 메시지를 보냅니다.
__declspec( dllexport ) int LibInitialize(void)
{
	#pragma message(TODO("Perform initialization here."))
	return TRUE;
}
```

## LibShutdown()

플러그인이 언로드되기 직전에 한 번 호출됩니다. 이 방법에서 일회성 플러그인 초기화 해제를 수행합니다. 시스템은 반환 값을 무시합니다.

```cpp
// This function is called once, just before the plugin is unloaded. 
// 이 함수는 플러그인이 언로드되기 직전에 한 번 호출됩니다.
// Perform one-time plugin un-initialization in this method."
// 이 방법으로 일회성 플러그인 초기화 해제를 수행합니다."
// The system doesn't pay attention to a return value.
// 시스템은 반환 값에 주의를 기울이지 않습니다.
__declspec( dllexport ) int LibShutdown(void)
{
	#pragma message(TODO("Perform un-initialization here."))
	return TRUE;
}
```