사전적 원소는 토큰(token)과 공백(white space)으로 구성되며, 프로그램을 구성한다.

## 토큰
토큰은 컴파일러에게 의미있는 가장 작은 원소다.  
C++ 파서(parser)는 다음 종류의 토큰들을 인식한다.
- 키워드(keyword)
- 식별자(identifier)
- 리터럴(literal)
  - 수(numeric), 불(boolean), 포인터(pointer)
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
[문자 셋 링크][1]


[1]: https://docs.microsoft.com/en-us/cpp/cpp/character-sets?view=msvc-170
