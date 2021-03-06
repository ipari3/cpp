# Lexical Elements
어휘 요소는 토큰과 여백으로 구성되며, 프로그램을 구성한다.
> lexical: 어휘의, 사전의, 사전적인

## 토큰
토큰은 컴파일러에게 의미있는 가장 작은 요소다.  
C++ 파서(parser)가 인식하는 토큰에는 다음의 종류가 있다.
- 식별자
- 키워드
- 리터럴
- 연산자
- punctuator
#### 식별자(identifier)
식별자는 다음을 표시하는 문자 시퀀스다.
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
- 언더스코어 두 개나 하나와 대문자로 시작하는 식별자는 예약되어 있다.  
언더스코어 하나와 소문자로 시작하는 식별자는 충돌의 가능성이 있으므로 피하는 것이 좋다.
#### 키워드
키워드는 특별한 의미를 가진 예약된 식별자다.  
예약된 식별자는 식별자로 사용될 수 없으며, 따라서 키워드도 식별자로 사용할 수 없다.  
[키워드 목록 외부 링크][2]
#### 리터럴
리터럴은 값을 직접 표현하는 요소다.
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
- [사용자 정의 리터럴][5]
##### 불리언 리터럴
`true`와 `false`가 있다.
##### 포인터 리터럴
포인터 리터럴은 [`nullptr`][6]을 말하며, 이는 0으로 초기화된(zero-initialized) 포인터를 명시한다.  
널 포인터 상수로 `NULL`이나 `0` 대신 `nullptr`을 사용하는 것이 좋다.
#### puctuator
C++ 어휘 요소에서의 문장 부호를 puctuator라고 부른다.  
`! % ^ & * ( ) - + = { } | ~ [ ] \ ; ' : " < > ? , . #`는 문장 부호로 간주된다.
> 문장 부호의 영단어는 puctuation 혹은 puctuation marks다.

## 여백(white space)
토큰을 구분해주는 요소다.  
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


[1]: https://docs.microsoft.com/en-us/cpp/cpp/identifiers-cpp?view=msvc-170
[2]: https://docs.microsoft.com/en-us/cpp/cpp/keywords-cpp?view=msvc-170
[3]: 문자 및 문자열 리터럴
[4]: 뉴메릭 리터럴
[5]: https://github.com/ipari3/cpp/blob/main/practical/UDL.md
[6]: nullptr 깃허브(yet)
