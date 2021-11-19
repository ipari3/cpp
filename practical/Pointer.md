
## nullptr
`nullptr` 키워드는 `std::nullptr_t` 타입의 널 포인터 상수를 명시하며, 이는 모든 raw 포인터 타입으로 변환가능하다.  
`nullptr` 키워드는 헤더를 include할 필요가 없지만, `std::nullptr_t` 타입을 쓰려면 `<cstddef>` 헤더를 include해야 한다.  

#### C++/CLI의 nullptr
`nullptr` 키워드는 C++/CLI에도 정의되어 있지만 ISO 표준 C++ 키워드와는 다르며 교체가능하지 않다.
[/clr][] 컴파일러 옵션을 사용하는 경우, 컴파일러가 네이티브 C++의 `nullptr`로 해석하게 하려면 `__nullptr`을 사용해야 한다.
[]: https://github.com/ipari3/cpp/blob/main/theoretical/Compiler%20Options.md#clr
[C++/CLI와 C++/CX의 nullptr 외부 링크][]
[]: https://docs.microsoft.com/en-us/cpp/extensions/nullptr-cpp-component-extensions?view=msvc-170

#### nullptr, NULL, 0
`NULL`과 `0`을 널 포인터 상수로 사용하는 것은 피해야 하며, `nullptr`을 사용하는 것이 낫다.
- `nullptr`: `std::nullptr_t` 타입
- `NULL`: 매크로
- `0`: integral 타입
[예시 외부 링크][]
[]: https://docs.microsoft.com/en-us/cpp/cpp/nullptr?view=msvc-170#remarks
