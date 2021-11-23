# 타입
다음은 뉴메릭 [built-in 타입][1]들이며, 괄호는 바이트 크기다.
- size-specific: `__int8`, `__int16`, `__int32`, `__int64`
- integer: `short`(2), `int`(4), `long`(4), `long long`(8)
- floating-point: `float`(4), `double`(8), `long double`(8)
## integral
integral은 '적분'이라는 뜻도 있지만, '정수'나 '전체'라는 뜻도 있다.  
여기서는 정수형 전체라는 느낌으로 사용되며, integer 타입은 integral 타입에 속한다.
- integral: `short`, `int`, `long`, `long long`, `bool`, `char`, `wchar_t`, `__wchar_t`
- integer:  `short`, `int`, `long`, `long long`  
integer는 numeric과 integral의 교집합이다.

# 리터럴
뉴메릭 리터럴에는 정수(integer)와 부동 소수점(floating point) 리터럴이 있다.
- case-insensitive: 문자들의 대소문자를 구별하지 않는다. (타입, 진법, 16진수 수, 지수 등)
- suffix: 접미사로 타입을 명시한다. (진법 표시는 접두사를 사용한다.)
- overflow: 타입보다 큰 값은 오버플로우가 발생한다. (에러가 아니기 때문에 예상치 못한 버그가 발생할 수 있다.)
- base-n: n 보다 크거나 같은 수가 존재하면 에러가 발생한다.
## 정수 리터럴
#### base-10
정수 리터럴은 default로 int타입이며 10진수다.
```
int a = 1234;
int b = 36'000'000;

unsigned c = 123u; // 접미사 u로 unsigned임을 명시.
long d = 0x7FFFFL; // 접미사 l로 long임을 명시.
auto e = 108LL;    // 접미사 ll로 long long임을 명시.
unsigned long f = 776784ul;
```
- 가독성을 위해 `'`를 임의의 위치에 쓸 수 있다. 컴파일에 영향을 주지 않는다.
- 부호와 크기 접미사를 붙여서 동시에 명시할 수 있다.
- `i8`, `ui8`, `i64` 등의 MS-specific 접미사도 있지만, 이식성 때문에 권장되지 않는다.
#### base-n
2, 8, 16진수 리터럴이 있다.
```
int a = 0b0011010;
int b = 0123;
int c = 0x3fff;

int d = 0987;   // 에러. (7보다 큰 수)
int e = 0x3fgh; // 에러. (F 보다 큰 수)
```
- 2, 8, 16진수 리터럴은 각각 `0b`, `0`, `0x`로 시작. (대소문자 상관없음)
- 각 진수에서 사용가능한 수 외의 문자를 사용하면 에러 발생.

## 부동소수점 리터럴
부동소수점 리터럴은 default 타입은 double이다.  
decimal point를 포함하거나, 지수 표기법을 사용하면 부동소수점 리터럴이다.
```
18.46 // double 타입 (defualt)
18.46f // float 타입
18.46l // long double 타입

38.

18.46e0 // 18.46
18.46E1 // 184.6
18E0
```
- 타입이나 지수 접미사는 대소문자를 가리지 않는다.  
`long double`과 `long`은 같은 접미사 `l`로 표기하는데, 실제 값으로 구분이 가능하다.
- decimal point으로 끝나도 된다.
- 지수로 표기하면 decimal point가 없어도 된다.

## 제한값
integer와 floating-point 타입의 제한값들이 상수로 정의되어 있다.
- integer: \<climits\> ([목록 외부 링크][2])
- floating-point: \<float.h\> ([목록 외부 링크][3])


[1]: https://github.com/ipari3/cpp/blob/main/theoretical/Built-in%20Types.md#built-in-%ED%83%80%EC%9E%85
[2]: https://docs.microsoft.com/en-us/cpp/cpp/integer-limits?view=msvc-170
[3]: https://docs.microsoft.com/en-us/cpp/cpp/floating-limits?view=msvc-170
