# built-in 타입
built-in 타입은 컴파일러에 들어가 있는(built into) 타입으로, 어떠한 헤더 파일에도 정의되어 있지 않다.  
동의어(synonym)인 타입들은 컴파일러에게 동일한 타입으로 취급된다.
## 빌트인 타입 크기 비교
- 1바이트: `__int8`, &nbsp; `char8_t`, &nbsp; `char`, `bool`, `unsigned char`, `signed char`
- 2바이트: `__int16`, `char16_t`, `short`, `wchar_t`, `__wchar_t`, `unsigned short`
- 4바이트: `__int32`, `char32_t`, `int`, `long`, `float`, `unsigned int`, `unsigned long`
- 8바이트: `__int64`, `long long`, `double`, `long double`, `unsigned long long`

([데이터 타입의 범위 외부 링크][1])

> 타입의 크기는 구현 방식에 따라 다르며, 보통 다음의 크기 비교가 성립한다.  
> 1바이트 = `char` <= `short` <= `int` <= `long` <= `long long`  
> C++에서는 `int`와 `long`이 4바이트로 같으며, 위의 크기 비교에 부합한다.  
> 또한 `double`과 `long double`도 8바이트로 같다.
#### 동의어
언더스코어 두 개로 시작하고 크기가 명시된 타입은 다른 타입의 동의어다.
- `__int8`과 &nbsp; `char`
- `__int16`과 `short`
- `__int32`와 `int`
- `__int64`와 `long long`

다른 타입들과의 동의 관계
- `long`: `long`과 `__int32`는 동의어가 아니가 아니다. (`int`와 `long`이 동의어가 아니기 때문)
- ANSI 타입: `__int8`, `__int16`, `__int32`는 같은 크기의 ANSI 타입들과 동의어기 때문에 포터블 코드에 사용하기 좋다.
- 언더스코어 하나: 이전 버전의 `_int8`, `_int16`, `_int32`, `_int64`과는 각각 동의어이다.
  - [/Za 컴파일 옵션][2]이 명시된 경우는 동의어가 아니다.

## 정수(integer) 타입
`int`가 기본 정수 타입이며 modifier 키워드를 추가로 명시할 수 있다.
#### sign modifier 키워드
- `signed`: 양수와 음수 표현 (default)
- `unsigned`: 음수가 아닌 값만 표현
#### size modifier 키워드
- `short`: 16비트(2바이트) 이하
- `long`: 32비트(4바이트) 이하
- `long long`: 64비트(8바이트) 이하
C++에서 `long`의 표현 및 크기는 `int`와 동일하지만, 컴파일러에게 별개의 타입으로 취급된다.
#### int 키워드와 modifier
`int` 키워드는 다른 modifier가 있을 때 생략될 수 있으며, modifier의 순서는 상관없다.  
`short unsigned`와 `unsigned int short`는 동의어다.  
##### 동의어 예시들
- `short`, `short int`, `signed short`, `signed short int`
- `int`, `signed`, `signed int`
- `unsigned long long`, `unsigned long long int`

## 부동소수점(floating-point) 타입
C++ 컴파일러는 4바이트와 8바이트 IEEE-754 부동소수점 표현을 사용한다.  
3개의 built-in 타입이 있다: `float`, `double`, `long double`  
`double`과 `long double`의 표현은 동일하지만, 컴파일러에게는 별개의 타입으로 취급된다.  
([부동소수점 표현 외부 링크][3])

## 문자(character) 타입
built-in 타입으로 `char`, `wchar_t`, `char8_t`, `char16_t`, `char32_t`가 있다.  
#### char
- 1바이트(8비트)
- ASCII 코드나 multi-byte 문자의 개별 바이트를 담을 수 있다.
  - multi-byte 문자로는 Shift-JIS나 UTF-8 인코딩된 유니코드가 있다.
- `signed char`이나 `unsigned char`와는 별개의 타입이다.
  - `char` 타입은 기본적으로 `signed char` 처럼 `int`로 진급된다(promoted).
  - [/J 컴파일 옵션][4]이면, `unsigned char` 처럼 [부호 확장][5]이 없는 `int`로 진급된다.  
  `unsigned char`은 built-in 타입이 아니며, 바이트를 표현하는데 종종 사용된다.
#### wchar_t
- 2바이트(16비트)이며, unsigned 표현에 사용한다.
- [UTF-16LE][6]로 인코딩된 유니코드를 담을 수 있으며, 이것은 윈도우 OS의 네이티브 문자 타입이다.
- default로는 built-in 타입(혹은 네이티브 타입)이다.  
  - 이 경우, `__wchar_t`와 동의어다. (`__wchar_t`는 Microsoft-specific 네이티브 타입이다.)
  - 컴파일러 옵션을 [`/Zc:wchar_t-`][7]로 하면, `wchar_t`는 `unsigned short`의 typedef가 된다.  
  이 경우, `wchar_t`가 built-in 타입이 아니므로 권장되지 않는다.
- wchar_t라는 이름은 wide-character 타입을 나타낸다.
- 리터럴에 `L` 접두사를 붙여 wide-character 타입으로 명시할 수 있다.
#### char8_t, char16_t, char32_t
- char8_t: UTF-8 문자 표현.
  - `unsigned char`과 표현은 같지만 별개의 타입이다.
  - C++20부터 생겼기 때문에 `/std:c++20`이나 `/std:c++latest` 같은 그 이후의 컴파일러 옵션을 사용해야 한다.
- char16_t: UTF-16 문자 표현.
- char32_t: UTF-32 문자 표현.
#### 문자열
문자열은 built-in 혹은 native 타입이 아니고 specific 타입이다.  
- narrow string: `char`, `char8_t` 타입의 문자열.
- wide string: `wchar_t`, `char16_t`, `char32_t` 타입의 문자열.  
일반적으로 wide string은 `wchar_t` 타입의 스트링을 말한다.

C++ 표준 라이브러리의 `basic_string` 타입은 narrow와 wide 문자열 모두에 특화되어 있다.
- `std::string`: `char` 타입 문자들
- `std::wstring`: `wchar_t` 타입의 문자들
- `std::u8string`: `char8_t` 타입의 문자들
- `std::u16string`: `char16_t` 타입의 문자들
- `std::u32string`: `char32_t` 타입의 문자들
- 기타 `std::stringstream`과 `std::cout` 타입

## 기타 타입
#### bool
`bool` 타입은 true나 false의 값을 가진다.  
크기(size)는 구현하기 나름이며, C++에서는 1바이트다.  
C++17부터 postfix/prefix increment/decrement가 허용되지 않는다.  
```
bool b = false;
b++; // 에러
```

#### void
`void` 타입은 공집합(empty set)을 나타낸다.  
- **void 함수**: 값을 반환하지 않는다.
- **void 매개변수**: 함수가 매개변수를 갖지 않는다.
  - 매개변수가 void인 경우: 함수가 인자를 받으면 컴파일 에러가 발생한다.
  - 매개변수를 비워놓은 경우: 함수가 인자를 받아도 작동한다.
- ***void 포인터*** (void* 변수): 포인터가 universal이다.
  - `const`나 `volatile`이 아닌 모든 변수를 가리킬 수 있다.  
  free 함수(클래스 멤버가 아닌 함수)나 static 멤버 함수를 가리킬 수 있지만, non-static 멤버 함수는 안된다.
  - 역참조될(dereferenced) 수 없다. 다른 타입으로 캐스트되면 가능하다.
  - 어떤 종류의 데이터 타입으로도 변환될 수 있다.
- ~~void 변수~~: 선언할 수 없다.

어떤 표현(expression)이라도 보이드 타입으로 명시적으로 변환되거나 캐스트될 수 있다.  
하지만 이러한 표현은 다음의 사용으로 제한된다.
- [표현문][7](expression statement)
- [컴마 연산자][8]의 왼쪽 피연산자
- [삼항 조건 연산자][9](conditional operator) `? :`의 두세번째 피연산자
> 표현(expression)은 연산자와 피연산자 시퀀스다.  
> 구문(statement)는 if문 같은 프로그램 요소이다.  
> 표현문은 표현이 평가되는 구문이다.


[1]: https://docs.microsoft.com/en-us/cpp/cpp/data-type-ranges?view=msvc-170
[2]: https://github.com/ipari3/cpp/blob/main/theoretical/Compiler%20Options.md#za
[3]: https://docs.microsoft.com/en-us/cpp/build/ieee-floating-point-representation?view=msvc-170
[4]: https://github.com/ipari3/cpp/blob/main/theoretical/Compiler%20Options.md#j
[5]: https://github.com/ipari3/cpp/blob/main/theoretical/Numeric%20Manipulation.md#sign-extension
[6]: https://github.com/ipari3/cpp/blob/main/theoretical/Character%20Encoding.md#utf-16
[7]: https://github.com/ipari3/cpp/blob/main/theoretical/Compiler%20Options.md#zcwchar_t

[]: 표현문
[]: 컴마 연산자
[]: 삼항 조건 연산자
