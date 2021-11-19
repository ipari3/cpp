# 빌트인(built-in) 타입
빌트인 타입은 컴파일러에 들어가 있는(built into) 타입으로, 어떠한 헤더 파일에도 정의되어 있지 않다.  
대부분의 빌트인 타입은 컴파일러에게 별개의(distict) 타입으로 취급되지만,  
어떤 것들은 동의어로써, 동일한 타입으로 취급되는 것도 있다.  
빌트인 타입은 크게 세 범주로 나뉜다.
- integral
- floating-point
- void
빌트인 타입은 fundamental 타입이라고도 한다.  
또한 데이터를 담는 빌트인 타입은 native 타입 혹은 native 데이터 타입이라고도 한다.  
문자열은 빌트인이나 native 타입이 아니며, specific 데이터 타입이라고 한다.

## 문자(character) 타입
#### char
C++ 컴파일러는 `char`, `signed char`, `unsigned char` 타입을 다른 타입으로 다룬다.
- `char` 타입은 기본적으로 `signed char` 처럼 `int`로 진급된다(promoted).
- [/J 컴파일 옵션][1]이면, `unsigned char` 처럼 부호 확장이 없는 `int`로 진급된다.
#### wchar_t
`wchar_t` 타입은 **wide-character** 혹은 **multibyte character** 타입이다.  
- `L` 접두사를 붙여 wide-character 타입으로 명시할 수 있다.
`wchar_t`는 기본적으로(by default) native 타입이다.
- 이것은 [`/Zc` 컴파일러 옵션][2]에서 [`/Zc:wchar_t`][3]인 상태이다.
- 뒤에 minus를 붙여서 `/Zc:wchar_t-`로 하면, `wchar_t`가 `unsigned short`의 typedef가 된다.  
하지만 이렇게 하면 `wchar_t`가 빌트인 타입이 아니게 되므로 권장되지 않는다.
- `__wchar_t` 타입은 `wchar_t`의 동의어(synonym)이다.
#### char8_t
UTF-8 문자 표현을 위해 사용된다.
- C++20부터 생겼기 때문에 [`/std:c++20`][4]이나 `/std:c++latest` 같은 그 이후의 컴파일러 옵션을 사용해야 한다.
- `unsigned char`과 동일한 표현을 갖지만, 컴파일러에게는 별개의 타입으로 취급된다.
#### char16_t
UTF-16 문자 표현을 위해 사용된다.  
이는 어떠한 UTF-16 코드 유닛이라도 표현할 수 있을 만큼 커야 한다.  
역시 컴파일러에게 별개의 타입으로 취급된다.
#### char32_t
UTF-32 문자 표현을 위해 사용된다.  
이는 어떠한 UTF-32 코드 유닛이라도 표현할 수 있을 만큼 커야 한다.  
역시 컴파일러에게 별개의 타입으로 취급된다.

## 정수(integer) 타입
`int` 타입은 기본 정수 타입이며, 구현에 따른 범위 내의 모든 수를 표현한다.  
- `signed` modifier 키워드가 있으면 양수와 음수를 표현할 수 있으며, default로 signed이다.
- `unsigned` modifier 키워드가 명시되면 음수가 아닌 값만 표현할 수 있다.
크기 modifier는 세 가지가 있다.
- short: 16비트보다 작아야 한다.
- long: 32비트보다 작아야 한다.
- long long: 64비트보다 작아야 한다.
`int` 키워드는 다른 modifier가 있을 때 생략될 수 있다.  
또한 `short unsigned`나 `unsigned int short` 처럼 순서는 상관이 없다.  
다음은 컴파일러에게 동의어인 것들의 예이다.
- short, short int, signed short, signed short int
- int, signed, signed int
- unsigned long long, unsigned long long int
또한 크기를 명시하는 형태도 있다.
- `__int8`: `char`과 동의어다.
- `__int16`: `short`과 동의어다.
- `__int32`: `int`와 동의어다.
- `__int64`: `long long`과 동의어다.

#### 빌트인 타입 크기 비교
- 1바이트: `bool`, `char`, `char8_t`, `unsigned char`, `signed char`, `__int8`
- 2바이트: `char16_t`, `__int16`, `short`, `unsigned short`, `wchar_t`, `__wchar_t`
- 4바이트: `char32_t`, `float`, `__int32`, `int`, `unsigned int`, `long`, `unsigned long`
- 8바이트: `double`, `__int64`, `long double`, `long long`, `unsigned long long`
[데이터 타입의 범위 외부 링크][] 

## 부동소수점(floating-point) 타입
C++ 컴파일러는 4바이트와 8바이트 IEEE-754 부동소수점 표현을 사용하며, 3개의 빌트인 타입이 있다.
- float
- double
- long double
`double`과 `long double`의 표현은 동일하지만, 컴파일러에게는 별개의 타입으로 취급된다.  
[부동소수점 표현 외부 링크][5]

## 기타 타입
#### bool
불 타입은 true나 false의 값을 가진다.  
크기(size)는 구현하기 나름이며, C++에서는 1바이트다.
#### void
보이드 타입은 값들의 공집합(empty set)이다.  

변수의 타입이 될 수는 없으며, 주된 쓰임은 다음과 같다.
- 반환을 하지 않는 함수 선언
- 타입되지 않거나(untyped) 임의 타입(arbitrarily typed)의 데이터를 가리키는 제네릭 포인터 선언

어떤 표현(expression)이라도 보이드 타입으로 명시적으로 변환되거나 캐스트될 수 있다.  
하지만 이러한 표현은 다음의 사용으로 제한된다.
- [표현문][1](expression statement)
- [컴마 연산자][2]의 왼쪽 피연산자
- [삼항 조건 연산자][3](conditional operator) `? :`의 두세번째 피연산자
> 표현(expression)은 연산자와 피연산자 시퀀스다.  
> 구문(statement)는 if문 같은 프로그램 요소이다.  
> 표현문은 표현이 평가되는 구문이다.
#### std::nullptr_t
[`nullptr`][4]는 `std::nullptr_t` 타입의 널포인터 상수다.  
이것은 모든 raw 포인터 타입으로 변환될 수 있다.



[1]: https://docs.microsoft.com/en-us/cpp/build/reference/j-default-char-type-is-unsigned?view=msvc-170
[2]: https://docs.microsoft.com/en-us/cpp/build/reference/zc-conformance?view=msvc-170
[3]: https://docs.microsoft.com/en-us/cpp/build/reference/zc-wchar-t-wchar-t-is-native-type?view=msvc-170
[4]: https://docs.microsoft.com/en-us/cpp/build/reference/std-specify-language-standard-version?view=msvc-170
[5]: https://docs.microsoft.com/en-us/cpp/build/ieee-floating-point-representation?view=msvc-170

[]: https://docs.microsoft.com/en-us/cpp/cpp/data-type-ranges?view=msvc-170

[1]: 표현문
[2]: 컴마 연산자
[3]: 삼항 조건 연산자
[4]: nullptr
