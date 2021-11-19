## API
- Windows API(Win32 API): C언어 기반
https://namu.wiki/w/Windows%20API
- Windows Runtime(WinRT) 윈도우 런타임: C++ 기반 (since 윈도우8)
https://namu.wiki/w/Windows%20Runtime
- Universal Windows Platform: .NET Core 기반 (since 윈도우10)
https://namu.wiki/w/Microsoft%20.NET#s-3

## .NET
- .NET Core: 크로스 플랫폼 https://namu.wiki/w/Microsoft%20.NET#s-3
- .NET Framework: FCL(Framework Class Library) + CLR(Common Language Runtime. 공용 언어 런타임) https://namu.wiki/w/Microsoft%20.NET#s-4

- '닷넷'이라고 읽는다.
- .NET Core와 .NET Framework는 서로 호환되지 않는다.
  - 다양한 OS에서 사용해야 하거나 새로 시작되는 프로젝트라면 .NET Core를 이용한다.
  - .NET Framework는 비교적 배우기 쉬우며, 이를 이용한 프로젝트가 많은데 그대로 이어나갈 수 있다.
https://clear-sky-sun.tistory.com/14
https://centbin-dev.tistory.com/42

## language specification
- ISO C++ standard: 표준 C++ 규격.
- C++/CX: WinRT 및 XAML 지원을 위해 고안된 C++ 확장 규격.
- C++/CLI: .NET Framework의 CLR에 맞물려 동작하는 C++ 규격.

C++/CLI는 네이티브 C++ 코드를 C#에서 사용할 수 있는 코드로 래핑할 수 있다.
- 네이티브(native) 코드는 비관리(unmanaged) 코드라고도 하며, 메모리가 관리되지 않는 코드다.  
C++/CLI는 이를 관리(managed) 코드로 래핑하여, GC 등을 통해 메모리를 관리해준다.
- C++/CLI는 관리와 비관리 코드를 모두 작성할 수 있지만, 성능이 떨어지므로 래핑 관련 모듈에만 사용하는 것이 좋다. 
https://namu.wiki/w/C%2B%2B#s-4.1
https://luckygg.tistory.com/220

> MFC(Microsoft Foundation Classes): C++ 기반의 GUI 라이브러리로, Win32 API의 C언어 함수들을 래핑하여 C++ 클래스화한 라이브러리다.  
> - MFC: C++ 기반, C에서 C++으로 래핑.
> - C++/CLI: C++ 기반, C++에서 C#으로 래핑.
