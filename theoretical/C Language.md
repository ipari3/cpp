## 파생 언어
- **C++**: C에 객체지향 프로그래밍 지원. (C with classes)
- **C#**: C++에 메모리 관리 지원. (닷넷 플랫폼 관련. 자바의 영향)  
자바가 자바 가상 머신(JVM)이 필요하듯, C#은 닷넷 프레임워크가 필요하다.
- ~~D~~: C++에 안전성과 동시성을 보완하기 위한 새로운 언어. 사실상 그 역할은 Rust나 Go에 밀렸다.
  - **Rust**: 메모리 안전성에 이점이 있으며 C++에 대등한 속도를 목표로 한다. 엄격하여 학습이 비교적 어렵다.
  - **Go**: GC를 사용하여 속도가 느리며 완성도가 떨어지는 언어다. 다만 비교적 단순하고 구글에서 발표한 언어이기 때문에 수요가 있다.
- ~~Objective-C~~: C에 객체지향 프로그래밍 지원. 애플의 표준 언어였지만 Swift로 넘어갔다.
  - **Swift**: Objective-C를 보완한 언어로, iOS, iPadOS, macOS를 대상으로 한다.
  - **Kotlin**: Java와 함께 안드로이드의 공식 언어 중 하나다.
- *V(개발 중)*: Go의 단순함과 Rust의 안전함을 접목하는 것을 목표로 한다. 상용화가 될 지는 미지수.

## .NET
Windows 프로그램 개발 및 실행 환경. .NET은 크로스 플랫폼을 지원한다.
- [.NET Framework][1]: FCL(Framework Class Library) + CLR(Common Language Runtime. 공용 언어 런타임)
- [.NET][2]: 크로스 플랫폼. .NET Core에서 그냥 .NET으로 명칭이 변경되었다.

'닷넷'이라고 읽으며, .NET과 .NET Framework는 서로 호환되지 않는다.
  - .NET Framework는 비교적 배우기 쉬우며, 이를 이용한 기존 프로젝트를 그대로 이어나갈 수 있다.
  - .NET은 다양한 OS를 지원하며 새로 시작되는 프로젝트라면 고려할 만하다.

## API
- [Windows API(**Win32 API**)][3]: C언어 기반 API.
- [Windows Runtime(**WinRT**)][4]: C++ 기반 API. (since 윈도우8)
- [Universal Windows Platform][5]: .NET 기반 API. (since 윈도우10)

## language specification
- ISO C++ standard: 표준 C++ 규격.
- **C++/CX**: WinRT 및 XAML 지원을 위해 고안된 C++ 확장 규격.
- **C++/CLI**: .NET Framework의 CLR에 맞물려 동작하는 C++ 규격.

C++/CLI는 네이티브 C++ 코드를 C#에서 사용할 수 있는 코드로 래핑할 수 있다.
- 네이티브(native) 코드는 비관리(unmanaged) 코드라고도 하며, 메모리가 관리되지 않는 코드다.  
- C++/CLI는 네이티브 코드를 관리(managed) 코드로 래핑하여, GC 등을 통해 메모리를 관리해준다.
- C++/CLI는 관리와 비관리 코드를 모두 작성할 수 있지만, 성능이 떨어지므로 래핑 관련 모듈에만 사용하는 것이 좋다. 

## MFC
[MFC(Microsoft Foundation Classes)][7]는 C++ 기반의 GUI 라이브러리로, Win32 API의 C언어 함수들을 래핑하여 C++ 클래스화한 라이브러리다.
- MFC: C++ 기반, C에서 C++으로 래핑.
- C++/CLI: C++ 기반, C++에서 C#으로 래핑.

[1]: https://namu.wiki/w/Microsoft%20.NET#s-3
[2]: https://namu.wiki/w/Microsoft%20.NET#s-4
[3]: https://namu.wiki/w/Windows%20API
[4]: https://namu.wiki/w/Windows%20Runtime
[5]: https://namu.wiki/w/Universal%20Windows%20Platform
[6]: https://namu.wiki/w/C%2B%2B#s-4.1
[7]: https://namu.wiki/w/MFC
