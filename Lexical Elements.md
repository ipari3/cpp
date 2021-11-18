사전적 원소는 토큰(token)과 공백(white space)으로 구성되며, 프로그램을 구성한다.

## 토큰
토큰은 컴파일러에게 의미있는 가장 작은 원소다.  
C++ 파서(parser)가 인식하는 토큰에는 식별자, 키워드, 리터럴, 연산자, punctuator가 있다.  
리터럴에는 수, 불, 포인터, 문자, 문자열, 사용자정의 리터럴이 있다.
#### 식별자(identifier)
식별자는 다음을 표시하는(denote) 문자 시퀀스다.
- 객체, 변수 이름
- 클래스, 구조체(structure), 공용체(union), 열거형(enumerated type) 이름
- 클래스, 구조체, 공용체, 열거(enumeration)의 멤버
- 함수, 클래스 멤버 함수
- typedef, label, 매크로 이름
- 매크로 파라미터

C++에서 식별자에 사용가능한 문자는 다음과 같다.
- 영어 대소문자, 언더스코어(\_)
- 숫자: 첫문자로는 쓸 수 없다.
- 달러 기호($): 유효하지만 특별한 기능이나 컨벤션은 없으며 특별히 사용할 이유는 없다.
- 유니버설 문자 [관련 링크][1]  
또한 다음의 규칙을 갖는다.
- 대소문자를 구분한다. (case sensitive)
- 키워드는 식별자로 사용할 수 없다.
- 언더스코어 두 개(\_\_)나 하나와 대문자(\_X-)로 시작하는 식별자는 예약되어 있다.  
언더스코어 하나와 소문자(\_x-)로 시작하는 식별자는 충돌의 가능성이 있으므로 피하는 것이 좋다.
#### 키워드
키워드는 특별한 의미를 가진 예약된 식별자다.  
예약된 식별자는 식별자로 사용될 수 없으며, 따라서 키워드도 식별자로 사용할 수 없다.  
[키워드 목록 링크][2]
#### 리터럴
리터럴은 값을 직접 표현하는 원소다.  
##### 수(numeric)
정수(integer)와 부동 소수점(floating point) 리터럴이 너머릭에 속한다.
```
int i = 1234;       // 10진수(decimal)
int j = 36'000'000; // digit separator(')를 사용할 수 있다. 컴파일에 영향을 주지 않는다.
auto k = 0b0011010; // 2진수(binary). 0b나 0B로 시작. int 타입이다.

int m = 0123; // 8진수(octal). 0으로 시작.
int n = 0987; // 에러. 8진수는 0-7 사이의 숫자만 사용할 수 있다.

int p = 0x3fff; // 16진수(hexadecimal). 0x나 0X로 시작.
int q = 0X3FFF; // 대소문자 모두 가능
int r = 0x3fgh; // 에러. 16진수는 0-9A-F 사이의 숫자나 문자만 사용할 수 있다.

unsigned a = 123u; // unsigned 값에 u나 U를 붙여 명시할 수 있다.
long b = 0x7FFFFL; // long 값에 l이나 L을 붙여 명시할 수 있다.
auto c = 108LL;    // long long 값에 ll이나 LL을 붙여 명시할 수 있다.
unsigned long c = 0776784ul; // 여러 개를 붙일 수 있다.
```
long long 타입은 64-bit integral(정수) 타입이다.  
LL 대신 i64 접미사(suffix)를 사용할 수 있지만 권장되지 않는다.
> integral은 '적분의'라는 뜻도 있지만, '정수의'라는 뜻도 있다.  
> C++에는 일반 int(integer) 타입과 구분되는 integral 타입이 있으며, int, short, unsigned 등이 포함된다.  
> [`std::is_integral`][3]은 bool, char 등의 타입에도 true를 반환한다.
```
18.46 // double 타입 (defualt 타입)
38.   // decimal point으로 끝날 수 있으며, 이로 인해 부동소수점이 된다.
18.46e0 // 18.46
18.46E1 // 184.6 (e와 E는 동일)
18E0    // exponent가 있으므로 decimal point가 없어도 부동소수점이다.

18.46f // float 타입 (f나 F를 붙인다.)
18.46l // long double 타입 (l이나 L을 붙인다.)
```
##### 불리언(boolean)
`true`와 `false`가 있다.
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
이스케이프 문자라고도 하며 백슬래쉬(\)와 문자 하나로 구성된다.  
문자 리터럴은 백슬래쉬(\), 작은따옴표('), newline을 제외한 모든 문자를 사용할 수 있다.  
이들을 문자로 사용하려면 이스케이프 시퀀스로 명시한다.  
반대로 `\b`처럼 이스케이프 시퀀스가 되면 b가 아닌 backspace가 된다.  
이스케이프 시퀀스 [링크1][4], [링크2][5]
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
```

- 8진수 표기: ASCII 형식을 따르며, 최대 3-digit을 담아 '\ooo'의 형태를 가진다. (앞에서 자릿수를 채우는 0은 생략 가능)
  - 3개를 넘으면 뒤의 3개만 가지고 나머지 앞의 숫자들은 잘린다. (경고)
  - 0-7가 아닌 숫자를 가지면 맨 뒤의 문자만 가지며 앞의 숫자들은 잘린다. (경고)
  - `\377` 보다 크면 안된다. (에러 C2022: 'value-in-decimal': too big for character)  
  8진수 0377은 10진수 255이다. 이것은 1바이트(8비트)가 모두 1인 값이다.  
  따라서 범위는 10진수로 0-255이며, 이것은 unsigned char의 범위와 동일하다.  
  ASCII는 총 128개지만, 8진수 이스케이프 문자는 총 256개다.
- 16진수 표기: 유니코드를 나타내며, 최대 4개의 숫자를 담아 '\xhhhh'의 형태를 가진다.
  - truncation은 8진수 표기와 동일하게 앞의 숫자를 버린다.
  - 숫자(digit)를 적어도 한 개는 포함해야 한다. (에러 C2153: "hex literals must have at least one hex digit")
##### 접두사 L
다중문자에 접두어 L을 붙이면 truncation을 반대로 한다. (앞의 숫자를 가진다.)  
위에서 봤듯이 L은 wchar_t 타입을 명시하는 것이다.  
wchar은 wide-character를 줄인 것이며 이는 16비트(2바이트) 크기를 갖는다. (L은 아마도 letter(글자)를 의미)
```
wchar_t w1 = L'\100'; // L'@' (이스케이프 시퀀스)
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


, 포인터(pointer)
  - 문자(character), 문자열(string)
  - 사용자정의(User-Defined)
- 연산자(operator)
- puctuator

## 공백
토큰은 주로 공백으로 구분된다.  
빈칸(blank), 수평 및 수직 탭(tab), new line, form feed, 주석 등이 있다.
#### new line
화면에서 다음 행으로 줄을 바꾼다.  
이스케이프 문자 `\n` 혹은 `0x0a`로 지시하며, 런타임에 OS에 따라 LF나 CRLF로 변환된다.
#### form feed
프린터가 현재 페이지를 끝내고 다음 페이지의 처음으로 이동하게 한다.
#### 주석(comment)
컴파일러는 주석을 공백처럼 다루어 무시한다.  
하지만 프로그래머에게 유용하며, 보통 훗날의 참고(future reference)를 위해 주석을 단다(annotate).
- `//`: 한 줄 주석
- `/*`/`*/`: 여러 줄 주석
##### \#if/\#endif 전처리기
테스트를 위해 코드를 비활성(inactive)으로 만들기 위해 주석으로 만들 수도 있다.  
하지만 이것은 `#if`/`#endif` 전처리기를 이용하는 것이 낫다.  
이것은 주석까지 둘러쌀 수 있기 때문이다. 반면 주석은 중첩(nest)되지 않는다.


소스 코드에 사용되는 문자 셋(set)은 영어 대소문자, 숫자, 특수문자, 공백으로 구성된다.  
[문자 셋 링크][]


[1]: https://docs.microsoft.com/en-us/cpp/cpp/identifiers-cpp?view=msvc-170
[2]: https://docs.microsoft.com/en-us/cpp/cpp/keywords-cpp?view=msvc-170
[3]: https://docs.microsoft.com/en-us/cpp/standard-library/is-integral-class?view=msvc-170
[4]: https://docs.microsoft.com/en-us/cpp/cpp/string-and-character-literals-cpp?view=msvc-170#bkmk_Escape
[5]: https://docs.microsoft.com/en-us/cpp/c-language/escape-sequences?view=msvc-170
[]: https://docs.microsoft.com/en-us/cpp/cpp/character-sets?view=msvc-170
