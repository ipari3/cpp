# MSVC
MSVC는 Microsoft Visual C++의 약자로, C, C++, C++/CX를 위한 컴파일러다.
- 컴파일러: Common Object File Format(COFF)인 오브젝트 파일 **.obj**를 생성.
- 링커: 실행가능한(executalbe) 파일 **.exe**나 동적 링크 라이브러리(dynamic-link libraries) **.dll**을 생성.

# 컴파일러 옵션
모든 컴파일러 옵션은 대소문자를 구분한다.
- [카테고리 별 목록][1]
- [알파벳순 목록][2]

## /clr
**Common Language Runtime Option**
- clr은 Common Language Runtime을 의미한다. (닷넷 프레임워크의 CLR)  
- CLR과 C++/CLI 컴파일을 허용하여, managed 코드에 이용한다.

([더 자세한 설명 외부 링크][3], [제약 사항 외부 링크][4])

## /GF
**Eliminate Duplicate Strings**
- 컴파일러가 execution 중에 프로그램 이미지와 메모리에 있는 동일한 문자열들의 하나의 복사본을 만들게 한다.<br>
이로써 다수의 포인터가 다수의 각 버퍼를 가리키는 대신, 하나의 버퍼를 가리키게 한다.
- 이것은 프로그램을 더 작게 만드는 최적화이며, 문자열 풀링(pooling)이라고 불린다.
- 각 유니크 문자열을 위한 addressable 섹션을 만드는데, 최대 65,536개의 섹션을 만들 수 있다.<br>
따라서 프로그램이 65,536개 이상의 문자열을 포함하면, /bigobj 컴파일 옵션을 사용해야 한다.

> 65,536 = 2<sup>16</sup> = 16진수 ffff

#### /bigobj
**Increase Number of Sections in .Obj file**
- 목적 파일이 가질 수 있는 섹션의 개수를 늘린다.<br>
65,536<sup>2</sup> = 2<sup>32</sup> = 4,294,967,296개로 늘린다.
- [Universal Windows Platform][5] 프로젝트에서 default로 enable되어 있다.<br>
machine-generated XAML 코드가 매우 많은 헤더를 포함하기 때문이다.

#### /O1, /O2
**Minimize Size, Maximize Speed**
- 미리 정의된 크기 및 속도 최적화 옵션셋이다. (숫자가 0이 아니라 알파벳 대문자 O다.)

|옵션|설명|동등|
|---|---|---|
|/O1|크기 최소화|/Og /Os /Oy /Ob2 /GF /Gy|
|/O2|속도 최대화|/Og /Oi /Ot /Oy /Ob2 /GF /Gy|

## /J
**Default char Type Is unsigned**
- `char` 타입을 `signed char`에서 `unsigned char`로 바꾼다.
- `int`로 확장될 때 [영의 확장][6]이 적용된다.
- `char`에 `signed`가 명시적으로 선언된 경우, /J 옵션의 영향을 받지 않는다.<br>
이 경우 `int`로 확장될 때 [부호 확장][7]이 적용된다.

([더 자세한 설명 외부 링크][8])

## /Za (deprecated)
**Disable Language Extensions**  
- ANSI C89/ISO C90과 호환되지 않는 C에 대한 MS extensions를 disable한다.
#### /Ze
- MS extensions를 enable하는데, 이것은 dafulat이기 때문에 이 옵션은 deprecated되었다.

## /Zc
**Conformance(부합)**
- 컴파일러가 standard-confirming(표준에 부합) 혹은 Microsoft-specific(MS 특유)하게 행동하도록 한다.
- /Zc:option{,option ...}처럼 : 뒤에 옵션을 명시한다.
- Visual Studio에서 표준과 호환되지 않는 extension을 사용하는 경우, /Zc 옵션으로 표준에 부합하게 만들 수 있다.
- 어떤 옵션은 Microsoft-specific하여 breaking change를 최소화한다.  
어떤 옵션은 standard-confirming하여 보안, 성능, 호환성을 향상시키고, 이는 breaking change 비용보다 이득이 크다.
> breaking change: 한 부분의 변경이 잠재적으로 다른 컴포넌트의 실패를 야기하는 변경을 말한다.
#### /permissive-
**Standards conformance**
- 컴파일러에 standard conformance 모드를 명시한다.
- permissive(관대한) 뒤에 minus를 붙인 것으로, 관대한 행동을 disable하는 것이다.

([더 자세한 설명 외부 링크][9])

#### /Zc:strictStrings
**Disable string literal type conversion**
- 문자열 리터럴로 초기화되는 포인터는 엄격한 `const` 자격을 갖춰야 한다.
- `auto`로 문자열 포인터를 선언하면, 컴파일러가 리터럴 타입에 맞는 `const` 포인터 타입으로 선언한다.<br>
즉, `auto`에는 `const`를 붙이지 않아도 된다.

|코드|컴파일|
|--:|:-:|
|wchar_t* str = L"hello";|컴파일타임 에러|
|const wchar_t* str = L"hello";|컴파일 성공|
|auto str = L"hello";|컴파일 성공|

**/Zc:strictStrings-**
- 뒤에 minus를 붙인 옵션을 명시하면, 포인터가 엄격한 `const` 자격을 갖추지 않아도 된다.
- 그렇다고 포인터가 가리키는 리터럴이 상수가 아닌 것은 아니다.<br>
문자열을 변경하려고 하면 런타일 에러가 발생한다.

```
wchar_t* str = L"hello"; // /Zc:strictStrings-이면 컴파일 성공
str[1] = L'a'; // 런타임 에러
```

#### /Zc:wchar_t
**wchar_t Is Native Type**
- `wchar_t`를 built-in 타입으로 분석하며(parse), C++ 표준에 따라 2바이트 unsigned 값을 표현한다.
- C++ 컴파일의 default 옵션이며, C 컴파일에서는 무시된다.
- /permissive- 옵션에는 영향을 받지 않는다.

**/Zc:wchar_t-**
- 뒤에 minus를 붙인 옵션을 명시하면 `wchar_t`는 built-in 타입이 아니고, `unsigned short`에 대한 `typedef`로 정의된다. (stddef.h)<br>
이것은 C++ 표준이 아니기 때문에 권장되지 않으며, typedef 버전은 이식성(portability) 문제가 발생할 수 있다.

([더 자세한 설명 외부 링크][10])<br>
([/Zc 옵션 목록][11])


<!-- 외부 링크들 -->
[1]: https://docs.microsoft.com/en-us/cpp/build/reference/compiler-options-listed-by-category?view=msvc-170
[2]: https://docs.microsoft.com/en-us/cpp/build/reference/compiler-options-listed-alphabetically?view=msvc-170

<!-- /clr -->
[3]: https://docs.microsoft.com/en-us/cpp/build/reference/clr-common-language-runtime-compilation?view=msvc-170
[4]: https://docs.microsoft.com/en-us/cpp/build/reference/clr-restrictions?view=msvc-170

<!-- /GF -->
[5]: https://github.com/ipari3/cpp/blob/main/theoretical/C%20Language.md#api

<!-- /J -->
[6]: https://github.com/ipari3/cpp/blob/main/theoretical/Numeric%20Manipulation.md#zero-extension
[7]: https://github.com/ipari3/cpp/blob/main/theoretical/Numeric%20Manipulation.md#sign-extension
[8]: https://docs.microsoft.com/en-us/cpp/build/reference/j-default-char-type-is-unsigned?view=msvc-170#remarks
<!-- /Zc -->
[9]: https://docs.microsoft.com/en-us/cpp/build/reference/permissive-standards-conformance?view=msvc-170
[10]: https://docs.microsoft.com/en-us/cpp/build/reference/zc-wchar-t-wchar-t-is-native-type?view=msvc-170
[11]: https://docs.microsoft.com/en-us/cpp/build/reference/zc-conformance?view=msvc-170#remarks
