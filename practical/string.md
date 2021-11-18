#### 문자(character)
문자 리터럴은 작은따옴표(')로 감싼다.
```
auto c0 = 'A';   // char
auto c1 = u8'A'; // char
auto c2 = L'A';  // wchar_t
auto c3 = u'A';  // char16_t
auto c4 = U'A';  // char32_t
```
- numeric과 다르게 접두사(prefix)를 사용하여 타입을 명시하며, 대소문자를 구분한다.
- char 타입(접두가사 없거나 접두사 u8)은 UTF-8 유니코드 인코딩을 사용한다. (가변 길이, 단일 인코딩)
- wchar_t 타입(접두사 L)은 wide 문자인 UCS-2나 UTF-16 인코딩을 사용한다.
- char16_t(접두사 u)와 char32_t(접두사 U) 타입은 각각 UTF-16, UTF-32 인코딩을 사용한다.
##### 다중문자(multicharacter)
자바와 달리 다중문자가 가능하다.
```
auto m0 = 'abcd'; // int, value 0x61626364
char c1 = 'abcd'; // char, value 'd' (truncated)
wchar_t w0 = 'abcd'; // wchar_t, value '\x6364' (truncated)
```
- 빅 엔디언 느낌의 16진수 값으로 표현할 수 있다. (from high-order to low-order)
- 타입을 명시한 경우 타입 크기를 초과하면 앞(high-order) 부분이 잘린다(truncated).  
그리고 컴파일러가 경고를 표시한다. (C4305, C4309)
##### 이스케이프 시퀀스
이스케이프 문자라고도 하며 백슬래쉬(\\)와 문자 하나로 구성된다.  
백슬래쉬(\\), 작은따옴표(')를 문자로 사용하려면 이스케이프 시퀀스를 사용해야 한다.  
반대로 `\b`처럼 이스케이프 시퀀스가 되면 \\b가 아닌 backspace가 된다.  
이스케이프 시퀀스 [외부 링크1][1], [외부 링크2][2]
```
char newline = '\n';
char backslash = '\\';
char backspace = '\b';

char c0 = '\100';  // '@'
char c1 = '\1000'; // '0' (truncated. C4305, C4309)
char c2 = '\009';  // '9'
char c3 = '\089';  // '9' (truncated. C4305, C4309)
char c4 = '\qrs';  // 's' (truncated. C4129, C4305, C4309)

char c5 = 'x0050'; // 'p'
char c6 = 'x0pqr'; // 'r' (truncated. C4305, C4309)

char c7 = '\u0041'; // UCN 'A'
char c8 = '\U00000041'; // UCN 'A'
```
###### 8진수 표기
- ASCII 형식을 따르며, 최대 3-digit을 담아 '\ooo'의 형태를 가진다. (앞에서 자릿수를 채우는 0은 생략 가능)
- 3개를 넘으면 뒤의 3개만 가지고 나머지 앞의 숫자들은 잘린다. (경고)
- 0-7가 아닌 숫자를 가지면 맨 뒤의 문자만 가지며 앞의 숫자들은 잘린다. (경고)
- `\377` 보다 크면 안된다. (에러 C2022: 'value-in-decimal': too big for character)  
8진수 0377은 10진수 255이다. 이것은 1바이트(8비트)가 모두 1인 값이다.  
따라서 범위는 10진수로 0-255이며, 이것은 unsigned char의 범위와 동일하다.  
ASCII는 총 128개지만, 8진수 이스케이프 문자는 총 256개다.
###### 16진수 표기
- 유니코드를 나타내며, 최대 4개의 숫자를 담아 '\xhhhh'의 형태를 가진다.
- truncation은 8진수 표기와 동일하게 앞의 숫자를 버린다.
- 숫자(digit)를 적어도 한 개는 포함해야 한다. (에러 C2153: "hex literals must have at least one hex digit")
###### Universal character
- 작은따옴표 내부에서 다중문자에 접두사 `\u`를 붙이고 뒤에 4개의 숫자를 붙인다.
- 작은따옴표 내부에서 다중문자에 접두사 `\U`를 붙이고 뒤에 8개의 숫자를 붙인다.
##### 접두사 L
다중문자에 접두어 L을 붙이면 truncation을 반대로 한다. (앞의 숫자를 가진다.)  
위에서 봤듯이 L은 wchar_t 타입을 명시하는 것이다.  
wchar은 wide-character를 줄인 것이며 이는 16비트(2바이트) 크기를 갖는다. (L은 아마도 letter(글자)를 의미)
```
wchar_t w1 = L'\100'; // L'@'
wchar_t w2 = L'\1000'; // L'@' (truncated. C4066)
wchar_t w3 = L'\009'; // L'\0' (truncated. C4066)
wchar_t w4 = L'\089'; // L'\0' (truncated. C4066)
wchar_t w5 = L'\qrs'; // L'q' (truncated. C4066, C4129)

wchar_t w6 = L'\x0050'; // L'P'
wchar_t w7 = L'\x0pqr'; // L'\0' (truncated. C4066)
```
#### 문자열(string)
문자열 리터럴은 큰따옴표(")로 감싼다.
```
auto s0 = "hello";   // const char*
auto s1 = u8"hello"; // const char*(before C++20), const char8_t*(in C++20)
auto s2 = L"hello";  // const wchar_t*
auto s3 = u"hello";  // const char16_t*
auto s4 = U"hello";  // const char32_t*
```
- char8_t 타입은 C++20부터 추가되었으며, 8비트 고정의 UTF-8 문자 표현법이다.
###### raw string
이스케이프 문자를 사용하지 않고 모든 타입의 문자열 리터럴을 표현할 수 있게 만든다.  
즉, C++에서는 문자열에 unescaped된 \와 "를 포함할 수 있다.  
raw string이 아닌 원래 문자열은 native string 혹은 non-raw string이라고도 한다.
```
auto R0 = R"(Hello \ world)"   // Hello \ world
auto R1 = R"("Hello \ world")" // "Hello \ world"
auto R2 = u8R"(Hello \ R"(world)")"; // nested raw string
```
- 접두사 바로 뒤에 `R`을 붙이고 문자열을 `"`, `"` 대신 `"(`, `)"`로 감싼다.
- 중첩된(nested) 형태로 사용할 수 있다.
- 일반 문자열과 마찬가지로 접두어 없음, 접두어 u8, L, u, U를 붙여서 타입을 명시하며, 리터럴 타입도 각각 동일하다.
##### std::string
추가적인 construction이나 conversion 단계없이 `std::string` 리터럴을 생성할 수 있다.
```
auto S0 = "hello"s;   // std::string
auto S1 = u8"hello"s; // std::string(before C++20), std::u8string(in C++20)
auto S2 = L"hello"s;  // std::wstring
auto S3 = u"hello"s;  // std::u16string
auto S4 = U"hello"s;  // std::u32string

auto S5 = R"(Hello \ world)"s; // std::string from a raw const char*
```
- 접미사 `s`를 붙인다.
- 동일한 접두어들을 사용하여 타입을 명시한다. 타입은 `std::string` 등이 된다.
- raw string과 결합하여 사용할 수 있다. 타입은 `std::string` 등이다.


[1]: https://docs.microsoft.com/en-us/cpp/cpp/string-and-character-literals-cpp?view=msvc-170#bkmk_Escape
[2]: https://docs.microsoft.com/en-us/cpp/c-language/escape-sequences?view=msvc-170
