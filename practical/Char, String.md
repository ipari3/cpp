# 리터럴
문자 리터럴은 작은따옴표(')로 감싸고, 문자열 리터럴은 큰따옴표(")로 감싼다.  
문자 및 문자열 리터럴은 뉴메릭 리터럴과 다른 점이 많다.
- case-sensitive: 대소문자를 구분.
- prefix: 접두사로 타입 명시.
- truncation: 타입의 크기를 넘는 리터럴은 에러를 발생시키지 않고, 앞 부분이 잘린다.

## 문자(character) 리터럴
작은 따옴표로 감싼다.
```
auto c0 = 'A';   // char
auto c1 = u8'A'; // char8_t (before C++20: char)
auto c2 = L'A';  // wchar_t
auto c3 = u'A';  // char16_t
auto c4 = U'A';  // char32_t
```
- char 타입: 기본형
- wchar_t 타입:  접두사 L (UTF-16)
- char8_t  타입: 접두사 u8 (UTF-8)
- char16_t 타입: 접두사 u (UTF-16)
- char32_t 타입: 접두사 U (UTF-32)

#### 다중문자(multicharacter) 리터럴
작은 따옴표 안에 여러 문자가 들어갈 수 있다.  
따라서 문자 리터럴은 이스케이프 시퀀스도 받을 수 있다.
```
auto m0 = 'abcd'; // int, value 0x61626364
char c1 = 'abcd'; // char, value 'd' (truncated. C4305, C4309)
wchar_t w0 = 'abcd'; // wchar_t, value '\x6364' (truncated. C4305, C4309)
```
- concat: 리터럴은 빅 엔디언 느낌의 16진수 값이 다.
- truncation: 타입 크기를 초과하면 앞 부분이 잘린다.

## 이스케이프 시퀀스
이스케이프 시퀀스는 문자나 문자열 리터럴에서 표현된 그대로가 아닌, 다른 문자나 문자 시퀀스로 변하는 문자 시퀀스다.  
모든 이스케이프 시퀀스는 둘 이상의 문자로 구성되며, 이스케이프 문자라고 불리는 백슬래쉬(\\)로 시작한다.
```
char newline = '\n';
char backslash = '\\';
```
- 따옴표 안의 백슬래쉬는 '이스케이프 문자'가 된다.
- 백슬래쉬를 표현하고 싶으면 두 개를 쓴다. 이것은 백슬래쉬로 변환되는 이스케이프 시퀀스다.  
따옴표 안의 동일한 따옴표를 표현하고 싶으면 앞에 백슬래쉬를 붙이다.

이스케이프 시퀀스 [외부 링크1][1], [외부 링크2][2]

#### 8진수 이스케이프 시퀀스
```
char c0 = '\100';  // '@'
char c1 = '\1000'; // '0' (truncated. C4305, C4309)
char c2 = '\009';  // '9'
char c3 = '\089';  // '9' (truncated. C4305, C4309)
char c4 = '\qrs';  // 's' (truncated. C4129, C4305, C4309)
```
- '\ooo'의 형태: 최대 3-digit을 담으며, 앞에서 자릿수를 채우는 0은 생략 가능.
- truncation due to length: 숫자가 3개를 넘으면 뒤의 3개만 가지고 나머지 앞의 숫자들은 잘린다. (경고)
- truncation due to value: 각 숫자가 0 ~ 7이 아닌 값을 가지면 맨 뒤의 문자만 가지며 앞의 숫자들은 잘린다. (경고)
- limit: 리터럴이 `\377` 보다 크면 안된다. (에러 C2022: 'value-in-decimal': too big for character)  

##### unsigned char 범위
8진수 377은 10진수로 255이며, 이것은 1바이트(8비트)가 모두 1인 값이다.  
따라서 8진수 이스케이프 시퀀스의 범위는 10진수로 0-255이며, 이것은 unsigned char의 범위와 동일하다.  
ASCII는 총 128개지만, 8진수 이스케이프 문자는 총 256개다.

#### 16진수 표기
```
char c0 = 'x0050'; // 'p'
char c1 = 'x0pqr'; // 'r' (truncated. C4305, C4309)
```
- '\xhh'의 형태: 16진수 숫자 2개를 담는다. 
'\xhhhh'의 형태: 16진수 숫자 4개를 담는다.  
16진수 숫자는 \[0-9a-fA-F\] 사이의 값이며, 숫자이므로 대소문자를 구분하지 않는다. (16진수임을 나타내는 x도 마찬가지)
- truncation: 8진수 표기와 마찬가지다.  
단, 16진수 숫자를 <ins>적어도 한 개</ins>는 포함해야 한다. (에러 C2153: "hex literals must have at least one hex digit")  
(8진수의 경우, 에러는 아니고 truncation에서 끝난다.)
- range: 0000-FFFF

#### UCL(Universal Character Name)
유니코드로 이스케이프 시퀀스를 표현한다.
```
char c2 = '\u0041'; // UCN 'A'
char c3 = '\U00000041'; // UCN 'A'
```
접두사 '\u'나 '\U'를 붙이고 뒤에 16진수 숫자를 붙인다.  
접두사는 16진수의 'x'처럼 코드에 포함된 것이므로 따옴표 안에 붙인다. (char16_t나 char32_t 타입을 명시하는 접두사와는 상관이 없다.)
- \u: 접두사 `\u`를 붙이고, 뒤에 4개의 숫자를 붙인다.
- \U: 접두사 `\U`를 붙이고, 뒤에 8개의 숫자를 붙인다.

> 문자 리터럴과 네이티브 문자열 리터럴, 그리고 식별자는 UCN으로 나타낼 수 있다.  
> 써로게이트 페어가 필요하면 컴파일러가 발생시킨다.  
> ([식별자에 UCN을 사용하는 예시][3]

#### 접두사 L
다중문자 리터럴에 접두어 L을 붙이면 truncation을 반대로 한다. (뒤를 잘라낸다.)  
또한 위에서 봤듯이 L은 wchar_t 타입을 명시한다. (L은 아마도 letter(글자)를 의미)
```
wchar_t w1 = L'\100'; // L'@'
wchar_t w2 = L'\1000'; // L'@' (truncated. C4066)
wchar_t w3 = L'\009'; // L'\0' (truncated. C4066)
wchar_t w4 = L'\089'; // L'\0' (truncated. C4066)
wchar_t w5 = L'\qrs'; // L'q' (truncated. C4066, C4129)

wchar_t w6 = L'\x0050'; // L'P'
wchar_t w7 = L'\x0pqr'; // L'\0' (truncated. C4066)
```

## 문자열(string) 리터럴
큰 따옴표로 감싼다.  
리터럴의 타입은 동일 종류의 문자 리터럴에 `const`와 `*`(포인터 연산자)가 붙는다.
```
auto s0 = "hello";   // const char*
auto s1 = u8"hello"; // const char8_t* (before C++20: const char*)
auto s2 = L"hello";  // const wchar_t*
auto s3 = u"hello";  // const char16_t*
auto s4 = U"hello";  // const char32_t*
```
#### raw string
이스케이프 문자를 사용하지 않고 모든 타입의 문자열 리터럴을 표현할 수 있다.  
raw string이 아닌 기본 문자열은 native string 혹은 non-raw string이라고 한다.
```
auto R0 = R"(Hello \ world)"   // Hello \ world
auto R1 = R"("Hello \ world")" // "Hello \ world"
auto R2 = u8R"(Hello \ R"(world)")"; // nested raw string
```
- 접두사 `R`을 붙이고 문자열을 `"`가 아닌, `"(`, `)"`로 감싼다.
  - 다른 문자 접두사와 마찬가지로 case-sensitive하므로, 대문자로 써야 한다.
  - 타입 명시 접두사가 있으면, `R`은 그 뒤에 붙인다.
- 중첩된(nested) 형태로 사용할 수 있다.
#### 표준 라이브러리 문자열
접미사 `s`를 붙여 추가적인 construction이나 conversion 단계없이 `std::string` 리터럴을 생성할 수 있다.
```
auto S0 = "hello"s;   // std::string
auto S1 = u8"hello"s; // std::u8string (before C++20: std::string)
auto S2 = L"hello"s;  // std::wstring
auto S3 = u"hello"s;  // std::u16string
auto S4 = U"hello"s;  // std::u32string

auto S5 = R"(Hello \ world)"s; // std::string from a raw const char*
```
- `s` suffix: 다른 문자 관련 접사와 달리 접미사다.
- 타입 prefix: 타입 명시 접두사를 사용하면 타입도 앞에 비슷한 접두사가 붙은 것을 사용한다.
- raw string과 결합하여 사용할 수 있다. 타입은 표준 라이브러리 문자열이다.


[1]: https://docs.microsoft.com/en-us/cpp/cpp/string-and-character-literals-cpp?view=msvc-170#bkmk_Escape
[2]: https://docs.microsoft.com/en-us/cpp/c-language/escape-sequences?view=msvc-170
[3]: https://docs.microsoft.com/en-us/cpp/cpp/character-sets?view=msvc-170#universal-character-names
