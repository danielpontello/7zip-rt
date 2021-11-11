# 7zip-rt
7zip source code slightly modified for running in the Surface RT. Tested only on Windows RT 10.

## Requirements

### Building
- [Visual Studio 2012 Express for Windows 8](https://visualstudio.microsoft.com/en-us/vs/older-downloads/)
- [Visual Studio 2012 Express for Windows Desktop](https://visualstudio.microsoft.com/en-us/vs/older-downloads/)
- The following libraries from an Windows RT 10 device:
  - advapi32.dll
  - comctl32.dll
  - comdlg32.dll
  - gdi32.dll
  - mpr.dll
  - ole32.dll
  - oleaut32.dll
  - shell32.dll
  - user32.dll
- [dll2lib](https://github.com/peterdn/dll2lib)

### Running (install on device)
- [Visual C++ Redistributable for Visual Studio 2012 Update 4](https://www.microsoft.com/en-us/download/details.aspx?id=30679) (vcredist_arm.exe)
- [Visual C++ Redistributable Packages for Visual Studio 2013](https://www.microsoft.com/en-us/download/details.aspx?id=40784) (vcredist_arm.exe)

## Building

### Compiler setup
- Install both Visual Studio 2012 Express for Windows 8 and Windows Desktop
- Edit the file `C:\Program Files (x86)\MSBuild\Microsoft.Cpp\v4.0\V110\Platforms\ARM\Microsoft.Cpp.ARM.Common.props` and add the following line after `<PropertyGroup>`:

```xml
<WindowsSDKDesktopARMSupport>true</WindowsSDKDesktopARMSupport>
```

- Comment the following lines in `C:\Program Files (x86)\Microsoft Visual Studio 11.0\VC\include\crtdefs.h`:

```cpp
#if defined(_M_ARM) 
 #if _CRT_BUILD_DESKTOP_APP && !_ARM_WINAPI_PARTITION_DESKTOP_SDK_AVAILABLE
  #error Compiling Desktop applications for the ARM platform is not supported.
 #endif
#endif
```

- Copy the DLL files to some folder
- Copy the dll2lib.exe to the same folder
- Open **VS2012 Developer Command Prompt** and run

```bat
for %i in (*.dll) do dll2lib.exe %i
```

- Copy the resulting .lib files to `C:\Program Files (x86)\Microsoft Visual Studio 11.0\VC\lib\arm`
  - **WARNING**: don't overwrite any files; this will break your VS installation!

### Compiling
- Clone the repository
- From **VS2012 Developer Command Prompt**, enter the `CPP/7zip` directory and run:

```
set PLATFORM=arm
nmake NEW_COMPILER=1 MY_STATIC_LINK=1
```

### Binaries
