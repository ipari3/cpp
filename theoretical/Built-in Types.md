# built-in 타입
built-in 타입은 컴파일러에 들어가 있는(built into) 타입으로, 어떠한 헤더 파일에도 정의되어 있지 않다.  
동의어(synonym)인 타입들은 컴파일러에게 동일한 타입으로 취급된다.
## 빌트인 타입 크기 비교
- 1바이트: `__int8`, &nbsp; `char8_t`, &nbsp; `char`, `bool`, `unsigned char`, `signed char`, 
- 2바이트: `__int16`, `char16_t`, `short`, `wchar_t`, `__wchar_t`, `unsigned short`
- 4바이트: `__int32`, `char32_t`, `int`, `long`, `float`, `unsigned int`, `unsigned long`
- 8바이트: `__int64`, `long long`, `double`, `long double`, `unsigned long long`  
[데이터 타입의 범위 외부 링크][1]  
  
> 타입의 크기는 구현 방식에 따라 다르다.  
> C++에서는 `int`와 `long` 모두 4바이트다.  
> 다만 일반적으로 다음의 크기 비교가 성립한다.  
> 1바이트 = `char` <= `short` <= `int` <= `long` <= `long long`
#### 동의어
- `__int8`과 `char`
- `__int16`과 `short`
- `__int32`와 `int`
- `__int64`와 `long long`
## 정수(integer) 타입
정수 타입은 `int`에 modifier 키워드를 조합하여 사용한다.
#### sign modifier 키워드
- `signed`: 양수와 음수 표현 (default)
- `unsigned`: 음수가 아닌 값만 표현
#### size modifier 키워드
- short: 16비트(2바이트) 이하
- long: 32비트(4바이트) 이하
- long long: 64비트(8바이트) 이하
#### int 키워드와 modifier
`int` 키워드는 다른 modifier가 있을 때 생략될 수 있으며, modifier의 순서는 상관없다.  
`short unsigned`와 `unsigned int short`는 동의어다.  
##### 동의어 예시들
- short, short int, signed short, signed short int
- int, signed, signed int
- unsigned long long, unsigned long long int


`int` 타입은 기본 정수 타입이며, 구현에 따른 범위 내의 모든 수를 표현한다.  

## 부동소수점(floating-point) 타입
C++ 컴파일러는 4바이트와 8바이트 IEEE-754 부동소수점 표현을 사용하며, 3개의 빌트인 타입이 있다.
- float
- double
- long double
`double`과 `long double`의 표현은 동일하지만, 컴파일러에게는 별개의 타입으로 취급된다.  
[부동소수점 표현 외부 링크][5]

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
- [표현문][1](expression statement)
- [컴마 연산자][2]의 왼쪽 피연산자
- [삼항 조건 연산자][3](conditional operator) `? :`의 두세번째 피연산자
> 표현(expression)은 연산자와 피연산자 시퀀스다.  
> 구문(statement)는 if문 같은 프로그램 요소이다.  
> 표현문은 표현이 평가되는 구문이다.


[1]: https://docs.microsoft.com/en-us/cpp/cpp/data-type-ranges?view=msvc-170




[1]: https://docs.microsoft.com/en-us/cpp/build/reference/j-default-char-type-is-unsigned?view=msvc-170
[2]: https://docs.microsoft.com/en-us/cpp/build/reference/zc-conformance?view=msvc-170
[3]: https://docs.microsoft.com/en-us/cpp/build/reference/zc-wchar-t-wchar-t-is-native-type?view=msvc-170
[4]: https://docs.microsoft.com/en-us/cpp/build/reference/std-specify-language-standard-version?view=msvc-170
[5]: https://docs.microsoft.com/en-us/cpp/build/ieee-floating-point-representation?view=msvc-170



[1]: 표현문
[2]: 컴마 연산자
[3]: 삼항 조건 연산자

