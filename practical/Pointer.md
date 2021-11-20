## __ptr32, __ptr64
- `__ptr32`: 32비트 시스템에서 네이티브 포인터를 표현.
- `__ptr64`: 64비트 시스템에서 네이티브 포인터를 표현.
```
int * __ptr32 p32; // 64비트 시스템에서는 64비트 포인터로 강압된다(coerced).
int * __ptr64 p64; // 32비트 시스템에서는 32비트 포인터로 잘린다(truncated).
```
- `__ptr32`와 `__ptr64`는 /clr:pure 컴파일 옵션에서는 사용할 수 없다. (에러 발생)  
그러나 /clr:pure과 /clr:safe는 VS2015에서 deprecated되었고, VS2017에서는 지원되지 않는다.
- 언더스코어가 하나인 이전 버전의 `_ptr32`, `_ptr64`과는 각각 동의어이다.
  - [/Za 컴파일 옵션][1]이 명시된 경우는 동의어가 아니다.
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
[예시 외부 링크][2]

[1]: https://github.com/ipari3/cpp/blob/main/theoretical/Compiler%20Options.md#za
[2]: https://docs.microsoft.com/en-us/cpp/cpp/nullptr?view=msvc-170#remarks
