## MSVC
MSVC는 Microsoft Visual C++의 약자로, C, C++, C++/CX를 위한 컴파일러다.
- 컴파일러: Common Object File Format(COFF)인 오브젝트 파일 **.obj**를 생성.
- 링커: 실행가능한(executalbe) 파일 **.exe**나 동적 링크 라이브러리(dynamic-link libraries) **.dll**을 생성.

## 컴파일러 옵션
모든 컴파일러 옵션은 대소문자를 구분한다.  
[카테고리 별 목록][1], [알파벳순 목록][2]

#### /clr
**Common Language Runtime Option**
- clr은 Common Language Runtime을 의미한다. (닷넷 프레임워크의 CLR)  
- CLR과 C++/CLI 컴파일을 허용하여, managed 코드에 이용한다.

([더 자세한 설명 외부 링크][3], [제약 사항 외부 링크][4])

#### /J
**Default char Type Is unsigned**
- `char` 타입을 `signed char`에서 `unsigned char`로 바꾼다.
- `int`로 확장될 때 [영의 확장][3]이 적용된다.
- `char`에 `signed`가 명시적으로 선언된 경우, /J 옵션의 영향을 받지 않는다.  
이 경우 `int`로 확장될 때 [부호 확장][4]이 적용된다.

([더 자세한 설명 외부 링크][5])

[1]: https://docs.microsoft.com/en-us/cpp/build/reference/compiler-options-listed-by-category?view=msvc-170
[2]: https://docs.microsoft.com/en-us/cpp/build/reference/compiler-options-listed-alphabetically?view=msvc-170

<!-- /clr -->
[3]: https://docs.microsoft.com/en-us/cpp/build/reference/clr-common-language-runtime-compilation?view=msvc-170
[4]: https://docs.microsoft.com/en-us/cpp/build/reference/clr-restrictions?view=msvc-170
<!-- /J -->
[5]: https://github.com/ipari3/cpp/blob/main/theoretical/Numeric%20Manipulation.md#zero-extension
[6]: https://github.com/ipari3/cpp/blob/main/theoretical/Numeric%20Manipulation.md#sign-extension
[7]: https://docs.microsoft.com/en-us/cpp/build/reference/j-default-char-type-is-unsigned?view=msvc-170#remarks
