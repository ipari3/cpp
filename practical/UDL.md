## UDL(User-Defined Literal)
사용자 정의 리터럴은 편의를 위해 존재하며, 성능적인 면에서 이익이나 불이익이 없다.

#### 예시
예를 들면, `Distance d = 36.0_mi + 42.0_km;` 처럼 사용할 수 있을 것이다.  
이를 위해서는 연산자에 대해서도 정의해야 한다.
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
[UDL_Distance 코드 외부 링크][1]

#### 구현
구현은 다음과 같은 형식을 갖는다.
```
ReturnType operator"" _a(long double);
ReturnType operator"" _b(const char*, size_t);
ReturnType operator"" _r(const char*); // raw literal operator
template<char...> ReturnType operator"" _t(); // literal operator template
```
- ReturnType은 Distance 같은 연산의 반환형이다.  
그 앞에 [`constexpr`][2]을 붙여 상수 표현으로 만들 수 있다.
- operator""에서 ""는 연산자가 들어가는 부분이다. (""면 접미사를 만든다.)
- \_a 등은 \_km 같은 접미사다.  
UDL에서는 언더스코어로 시작해야 한다. 언더스코어로 시작하지 않는 것은 표준 라이브러리만 허용된다.
- 정수(integral) 타입은 long long이어야 하고, 부동소수점 타입은 long double이어야 한다.  
리터럴 수(number)는 10진수여야 한다. 그렇지 않으면 integer로 해석되어 연산자와 호환되지 않을 것이다.
- raw 리터럴 연산자는 \_r 같은 접미사 이름과는 상관없고, `size_t`가 없는 `const char*`로 명시하는 것이다.  
raw 리터럴 연산자는 문자나 문자열 리터럴에 적용할 수 없다.  
[raw 리터럴 코드 외부 링크][3]


[1]: https://docs.microsoft.com/en-us/cpp/cpp/user-defined-literals-cpp?view=msvc-170#cooked-literals
[2]: constexpr 깃허브(yet)
[3]: https://docs.microsoft.com/en-us/cpp/cpp/user-defined-literals-cpp?view=msvc-170#example-limitations-of-raw-literals
