## MSVC
MSVC는 Microsoft Visual C++의 약자로, C, C++, C++/CX를 위한 컴파일러다.
- 컴파일러: Common Object File Format(COFF)인 오브젝트 파일 **.obj**를 생성.
- 링커: 실행가능한(executalbe) 파일 **.exe**나 동적 링크 라이브러리(dynamic-link libraries) **.dll**을 생성.

## 컴파일러 옵션
모든 컴파일러 옵션은 대소문자를 구분한다.  
링킹하지 않고 컴파일하려면 `/c` 옵션을 사용한다.  
[카테고리 별 목록][1], [알파벳순 목록][2]

#### /clr
clr은 Common Language Runtime을 의미한다. (닷넷 프레임워크의 CLR)  
CLR과 C++/CLI 컴파일을 허용한다.  
managed 코드에 이용하는 옵션이다.

[1]: https://docs.microsoft.com/en-us/cpp/build/reference/compiler-options-listed-by-category?view=msvc-170
[2]: https://docs.microsoft.com/en-us/cpp/build/reference/compiler-options-listed-alphabetically?view=msvc-170
