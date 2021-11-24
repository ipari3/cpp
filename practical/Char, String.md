# 타입
## 문자 타입
문자 [built-in 타입][1]에는 `char`, `wchar_t`, `char8_t`, `char16_t`, `char32_t`가 있다.
- `char`은 1바이트, `wchar_t`는 2바이트 크기다.
-  size-spcific 타입들은 비트 단위로 명시되어 있다.
## 문자열 타입
문자열 타입은 built-in 타입이 아니다.<br>
<br>
**배열 문자열** 타입에는 `const char*`, `const wchar_t*`, `const char8_t*` 등이 있다.
- 타입의 형식은 원소의 타입에 `const`와 포인터 연산자 `*`가 붙은 것이다.
- null-terminated 배열이다.

**표준 라이브러리 문자열** 타입에는 `std::string`, `std::wstring`, `std::u8string` 등이 있다.
- 타입의 형식은 원소의 타입에서 `char`이 `string`으로 바뀐 것이다.

# 리터럴
문자 리터럴은 작은따옴표(')로 감싸고, 문자열 리터럴은 큰따옴표(")로 감싼다.<br>
<br>
문자 및 문자열 리터럴은 뉴메릭 리터럴과 다른 점이 많다.
- **case-sensitive**: 대소문자를 구분.
- **prefix**: 접두사로 타입 명시. (접미사 `s`는 예외) 
- **truncation**: 타입의 크기를 넘는 리터럴은 에러를 발생시키지 않고, 앞(high order) 부분이 잘린다.<br>
(접미사가 `L` 혹은 타입이 `wchar_t`인 경우는 반대)

## 문자(character) 리터럴
작은 따옴표로 감싼다.

|타입|접두사|인코딩|
|---|:-:|---|
|`char`| `'A'` |ASCII-like|
|`char8_t`|`u8'A'`|UTF-8|
|`wchar_t`|`L'A'`|UTF-16|
|`char16_t`|`u'A'`|UTF-16|
|`char32_t`|`U'A'`|UTF-32|
- 접두사 항목은 'A'에 붙은 형태를 표기한 것.
- C++20 이전에는 접두사가 `u8`인 리터럴의 타입은 `char`이었다. (`char8_t`는 C++20부터 도입)

#### 다중문자(multicharacter) 리터럴
작은 따옴표 안에 여러 문자가 들어갈 수 있다.<br>
따라서 문자 리터럴은 이스케이프 시퀀스도 받을 수 있다.
```
auto m0 = 'abcd'; // int, value 0x61626364
char c1 = 'abcd'; // char, value 'd' (truncated. C4305, C4309)
wchar_t w0 = 'abcd'; // wchar_t, value '\x6364' (truncated. C4305, C4309)
```
- concat: 리터럴은 빅 엔디언 느낌의 16진수 값이 된다.
- truncation: 타입 크기를 초과하면 앞 부분이 잘린다. 즉, 마지막에 쓴 것이 우선이다.

## 이스케이프 시퀀스
표현된 그대로가 아닌, 다른 문자나 문자 시퀀스로 변하는 문자 시퀀스다.<br>
모든 이스케이프 시퀀스는 둘 이상의 문자로 구성되며, **이스케이프 문자**라고 불리는 **백슬래시(\\)** 로 시작한다.
```
char newline = '\n';
char backslash = '\\';
```
- 따옴표 안의 백슬래시는 '이스케이프 문자'가 된다.
- 백슬래시 표현은 앞에 백슬래시를 붙여 백슬래시 두 개인 이스케이프 시퀀스로 명시한다.  
따옴표 안의 동일한 따옴표 표현도 앞에 백슬래쉬를 붙인다.

이스케이프 시퀀스 [외부 링크1][2], [외부 링크2][3]

#### 8진수 이스케이프 시퀀스
```
char c0 = '\100';  // '@'
char c1 = '\1000'; // '0' (truncated. C4305, C4309)
char c2 = '\009';  // '9'
char c3 = '\089';  // '9' (truncated. C4305, C4309)
char c4 = '\qrs';  // 's' (truncated. C4129, C4305, C4309)
```
- **'\oooㅁ'**:  ㅇㄹ
- **'\ooo'**의 형태: 최대 3-digit을 담으며, 앞에서 자릿수를 채우는 0은 생략 가능.
- truncation due to **length**: 숫자가 3개를 넘으면 뒤의 3개만 가지고 나머지 앞의 숫자들은 잘린다. (경고)
- truncation due to **value**: 각 숫자가 0 ~ 7이 아닌 값을 가지면 맨 뒤의 문자만 가지며 앞의 숫자들은 잘린다. (경고)
- **limit**: 리터럴이 `\377` 보다 크면 안된다. (에러 C2022: 'value-in-decimal': too big for character)  

##### unsigned char 범위
8진수 377은 10진수로 255이며, 이것은 1바이트(8비트)가 모두 1인 값이다.  
따라서 8진수 이스케이프 시퀀스의 범위는 10진수로 0-255이며, 이것은 unsigned char의 범위와 동일하다.  
ASCII는 총 128개지만, 8진수 이스케이프 문자는 총 256개다.

#### 16진수 이스케이프 시퀀스
```
char c0 = 'x0050'; // 'p'
char c1 = 'x0pqr'; // 'r' (truncated. C4305, C4309)
```
- '\xhh'의 형태: 16진수 숫자 2개를 담는다.  
'\xhhhh'의 형태: 16진수 숫자 4개를 담는다.  
- 16진수 숫자는 \[0-9a-fA-F\] 사이의 값이며, 숫자이므로 대소문자를 구분하지 않는다. (16진수임을 나타내는 x도 마찬가지)
- truncation: 8진수 표기와 동일하다.  
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
접두사는 16진수의 'x'처럼 코드에 포함된 것이므로 따옴표 안에 쓴다.
- \u: 접두사 `\u`를 붙이고, 뒤에 4개의 숫자를 붙인다.
- \U: 접두사 `\U`를 붙이고, 뒤에 8개의 숫자를 붙인다.

이 접두사들은 char16_t나 char32_t 타입을 명시하는 접두사와 별개다.  
예를 들어, `char16_t c = u'\u00A0'` 처럼 쓸 수 있다.

> 문자 리터럴과 네이티브 문자열 리터럴, 그리고 식별자는 UCN으로 나타낼 수 있다.  
> 써로게이트 페어가 필요하면 컴파일러가 발생시킨다.  
> ([식별자에 UCN을 사용하는 예시])[4]

#### 접두사 L
다중문자 리터럴에 접두어 L을 붙이면 truncation을 반대로 한다. (뒤를 잘라낸다.)  
또한 위에서 봤듯이 L은 wchar_t 타입을 명시한다. L은 아마도 letter(글자)를 의미.
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
```
auto s0 = "hello";   // const char*
auto s1 = u8"hello"; // const char8_t* (before C++20: const char*)
auto s2 = L"hello";  // const wchar_t*
auto s3 = u"hello";  // const char16_t*
auto s4 = U"hello";  // const char32_t*
```
- 큰 따옴표로 감싼다.
- `const char[n]` 계열 타입의 null-terminated 배열이다.  
- 큰 따옴표("), 백슬래시(\\), newline 문자를 제외한 모든 문자를 포함할 수 있다.  
1바이트 크기면 이스케이프 시퀀스나 UCN도 포함할 수 있다.

s0과 s2과 같은 문자열을 각각 narrow와 wide 문자열이라고 한다.
- narrow string: non-prefixed (s0)
- wide string: L-prefixed (s2)

> s1 처럼 u8-prefixed인 문자열은 UTF-8 encoded string이라고 한다.

#### raw string
이스케이프 문자를 사용하지 않고 모든 타입의 문자열 리터럴을 표현할 수 있다.  
raw string이 아닌 기본 문자열은 native string 혹은 non-raw string이라고 한다.
```
auto R0 = R"(Hello \ world)"   // Hello \ world
auto R1 = R"("Hello \ world")" // "Hello \ world"
auto R2 = u8R"(Hello \ R"(world)")"; // nested raw string
```
- 큰 따옴표가 아닌, `"xxx(`, `)xxx"`로 감싸고, 접두사 `R`을 붙인다.
  - xxx는 구획문자(delimiter)로, 임의의 0 ~ 16 문자를 넣을 수 있다.
  - 타입을 명시하는 접두사가 있으면, `R`은 그 뒤에 붙인다.
- native string과 마찬가지로, `const char[n]` 계열 타입의 null-terminated 배열이다.  
- 큰 따옴표("), 백슬래시(\\), newline 문자도 포함할 수 있다.
- 중첩된(nested) 형태로 사용할 수 있다.

##### delimiter
다음은 delimiter를 이용하여 내용이 `)"`인 raw string을 표현하는 예시다.
```
// bad: 에러 발생
const char* s1 = R"()")"; // )"을 단순히 "(과 )"로 감쌌는데, 내용인 )"가 닫는 괄호가 되었다.

// good
const char* s2 = R"abc()")abc"; // 여는 괄호가 "abc(이기 때문에 )"는 닫는 괄호가 되지 않았다.
```
##### newline
raw string은 이스케이프 시퀀스가 아닌 newline 자체를 포함할 수 있다.
```
const wchar_t* s1 = LR"(hello
goodbye)";
```

#### 접미사 s
접미사 `s`를 붙이면 표준 라이브러리 문자열 리터럴을 생성한다.  
이 때, 추가적인 construction이나 conversion 단계없이 `std::string` 리터럴을 생성한다.
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

## 문자열 리터럴의 크기


[1]: https://github.com/ipari3/cpp/blob/main/theoretical/Built-in%20Types.md#built-in-%ED%83%80%EC%9E%85

[2]: https://docs.microsoft.com/en-us/cpp/cpp/string-and-character-literals-cpp?view=msvc-170#bkmk_Escape
[3]: https://docs.microsoft.com/en-us/cpp/c-language/escape-sequences?view=msvc-170
[4]: https://docs.microsoft.com/en-us/cpp/cpp/character-sets?view=msvc-170#universal-character-names
