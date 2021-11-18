사전적 원소는 토큰과 여백으로 구성되며, 프로그램을 구성한다.
> lexical: 어휘의, 사전의, 사전적인

## 토큰
토큰은 컴파일러에게 의미있는 가장 작은 원소다.  
C++ 파서(parser)가 인식하는 토큰에는 식별자, 키워드, 리터럴, 연산자, punctuator가 있다.  
리터럴에는 수, 불, 포인터, 문자, 문자열, 사용자 정의 리터럴이 있다.
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
- 유니버설 문자 [관련 외부 링크][1]  
또한 다음의 규칙을 갖는다.
- 대소문자를 구분한다. (case sensitive)
- 키워드는 식별자로 사용할 수 없다.
- 언더스코어 두 개(\_\_)나 하나와 대문자(\_X-)로 시작하는 식별자는 예약되어 있다.  
언더스코어 하나와 소문자(\_x-)로 시작하는 식별자는 충돌의 가능성이 있으므로 피하는 것이 좋다.
#### 키워드
키워드는 특별한 의미를 가진 예약된 식별자다.  
예약된 식별자는 식별자로 사용될 수 없으며, 따라서 키워드도 식별자로 사용할 수 없다.  
[키워드 목록 외부 링크][2]
#### 리터럴
리터럴은 값을 직접 표현하는 원소다.
리터럴은 보통 변수 등에 담기는 값으로 쓰이며, 마법의 상수(magic constant)로 사용하는 것은 좋지 않다.  
예를 들어, 아래의 코드는 문자열 리터럴 "Success"를 상수처럼 반환값으로 사용했는데,  
MAXIMUM_ERROR_THRESHOLD라 이름지은 문자열 상수(named string constant)에 담아 사용하는 것이 좋다.
```
// bad
if (num < 100)
    return "Success";
```
리터럴은 크게 다음의 종류로 나눌 수 있다.
- [문자 및 문자열 리터럴][3]
- [뉴메릭][4], 불리언, 포인터 리터럴
- 사용자 정의 리터럴
##### 불리언 리터럴
`true`와 `false`가 있다.
##### 포인터 리터럴
포인터 리터럴은 [`nullptr`][5]을 말하며, 키워드이기도 하다.
nullptr는 0으로 초기화된(zero-initialized) 포인터를 명시한다.  
포터블 코드에서는 integral 타입의 `0`이나 `NULL` 같은 매크로 대신 `nullptr`를 써야 한다.
> 포터블 코드(portable code): 특정 플랫폼에 밀착 결합되지 않은(not tightly coupled) 코드.
##### UDL(User-Defined Literal. 사용자 정의 리터럴)
UDL은 편의를 위해 존재하며, 성능적인 면에서 이익이나 불이익이 없다.  
예를 들면, `Distance d = 36.0_mi + 42.0_km;` 처럼 사용할 수 있을 것이다.  
이를 위해서는 연산자에 대해서도 정의해야 한다.
```
ReturnType operator"" _a(long double);
ReturnType operator"" _b(const char*, size_t);
ReturnType operator"" _r(const char*); // raw literal operator
template<char...> ReturnType operator"" _t(); // literal operator template
```
- ReturnType은 Distance 같은 연산의 반환형이다.  
그 앞에 [`constexpr`][6]을 붙여 상수 표현으로 만들 수 있다.
- operator""에서 ""는 연산자가 들어가는 부분이다. (""면 접미사를 만든다.)
- \_a 등은 \_km 같은 접미사다.  
UDL에서는 언더스코어로 시작해야 한다. 언더스코어로 시작하지 않는 것은 표준 라이브러리만 허용된다.
```
// operator""는 접미사를 만든다. (Distance의 단위가 km인 경우다.)
Distance operator"" _km(long doulbe val)
{
    return Distance(val);
}

// operator+는 덧셈 연산자를 만든다.
Distance operator+(Distance other)
{
    return Distance(get_kilometers() + other.get_kilometers());
}
```
[UDL_Distance 코드 외부 링크][7]


- 연산자(operator)
- puctuator

## 여백(white space)
토큰은 주로 여백으로 구분된다.  
빈칸(blank), 수평 및 수직 탭(tab), new line, form feed, 주석 등이 있다.
#### new line
화면에서 다음 행으로 줄을 바꾼다.  
이스케이프 시퀀스 `\n`나 `0x0a`로 지시하며, 런타임에 OS에 따라 LF나 CRLF로 변환된다.
#### form feed
프린터가 현재 페이지를 끝내고 다음 페이지의 처음으로 이동하게 한다.
#### 주석(comment)
컴파일러는 주석을 공백처럼 다루어 무시하므로 의미가 없다.  
하지만 프로그래머에게는 유용하며, 보통 훗날의 참고(future reference)를 위해 주석을 단다(annotate).
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
[3]: 문자 및 문자열 리터럴
[4]: 뉴메릭 리터럴
[5]: nullptr
[6]: constexpr
[7]: https://docs.microsoft.com/en-us/cpp/cpp/user-defined-literals-cpp?view=msvc-170#cooked-literals
[]: https://docs.microsoft.com/en-us/cpp/cpp/character-sets?view=msvc-170
