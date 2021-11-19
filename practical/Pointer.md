`nullptr` 키워드는 `std::nullptr_t` 타입의 널 포인터 상수를 명시하며, 이는 모든 raw 포인터 타입으로 변환가능하다.  
`nullptr` 키워드는 헤더를 include할 필요가 없지만, `std::nullptr_t` 타입을 쓰려면 `<cstddef>` 헤더를 include해야 한다.  

#### C++/CLI의 nullptr
`nullptr` 키워드는 C++/CLI에도 정의되어 있지만 ISO 표준 C++ 키워드와는 다르며 교체가능하지 않다.
> C++/CLI는 C++ 코드를 C#에서 사용할 수 있는 코드로 만들어준다.
> - ISO 표준 C++로 작성한 C++코드는 native 코드 혹은 unmanaged 코드라고 한다.
> - C++/CLI로 native 코드를 wrapping한 것을 managed 코드라고 한다. 
