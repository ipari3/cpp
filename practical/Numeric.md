## 뉴메릭
뉴메릭(numeric) 혹은 수는 크게 정수(integer)와 부동 소수점(floating point)으로 나눌 수 있다.

#### 리터럴
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
> [`std::is_integral`][1]은 bool, char 등의 타입에도 true를 반환한다.
```
18.46 // double 타입 (defualt 타입)
38.   // decimal point으로 끝날 수 있으며, 이로 인해 부동소수점이 된다.
18.46e0 // 18.46
18.46E1 // 184.6 (e와 E는 동일)
18E0    // exponent가 있으므로 decimal point가 없어도 부동소수점이다.

18.46f // float 타입 (f나 F를 붙인다.)
18.46l // long double 타입 (l이나 L을 붙인다.)
```

#### 제한
integer와 floating-point 타입의 제한값들이 상수로 정의되어 있다.
- integer: \<climits\> ([목록 외부 링크][2])
- floating-point: \<float.h\> ([목록 외부 링크][3])

[1]: https://docs.microsoft.com/en-us/cpp/standard-library/is-integral-class?view=msvc-170

[2]: https://docs.microsoft.com/en-us/cpp/cpp/integer-limits?view=msvc-170
[3]: https://docs.microsoft.com/en-us/cpp/cpp/floating-limits?view=msvc-170
