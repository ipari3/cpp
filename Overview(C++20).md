## 특징
C++은 빠르고 효율적이며 유연하다.
- 최적화된 표준 라이브러리 제공
- low-level HW feature 접근 가능
- 다양한 분야에 대해 프로그래밍 가능

C스타일을 허용하여 C와의 하위 호환성(backward compatibility)을 챙겼다.  
하지만 C스타일은 좋은 성능을 낼 수 있지만, 복잡하고 버그의 원인이 된다.  
따라서 모던 C++은 C스타일을 최소화하여 간단, 안전, 명쾌하면서도 여전히 빠르다.
> C스타일: raw 포인터, 배열, null-종결 문자열 등

## RAII
메모리 누수(leak)는 C스타일의 주요 문제 중 하나이다.  
누수는 보통 `new`로 할당된 메모리를 `delete`하지 않아 발생한다.  
  
모던 C++은 RAII 원칙으로 메모리 누수에 대비한다.  
자원을 객체(object)가 소유해야 한다는 원칙이다.
- 생성자(constructor): 새로 할당된 자원을 생성하거나 받는다.
- 소멸자(destructor): 이를 삭제한다(delete).

RAII 원칙은 객체가 범위(scope)를 벗어날 때 소유한 모든 자원이 OS로 반환됨을 보장한다.
> RAII: Resource Acquisition Is Initialization. 자원 획득은 초기화다.  

> 자원: 힙 메모리, 파일 핸들, 소켓 등

## 스마트 포인터
RAII를 위해 C++ 표준 라이브러리는 세 가지 스마트 포인터 타입을 지원한다.
- [`std::unique_ptr`][1]
- [`std::shared_ptr`][2]
- [`std::weak_ptr`][3]

스마트 포인터는 자신이 가진 메모리의 할당(allocation)과 삭제(deletion)을 다룬다.  
```
#include <memory>
class widget
{
private:
    std::unique_ptr<int> data;
public:
    widget(const int size) { data = std::make_unique<int>(size); }
    void do_something() {}
};

void functionUsingWidget() {
    // w의 lifetime은 자동적으로 둘러싼 범위(enclosing scope)에 묶인다.
    widget w(1000000); // construct and allocate
} // 자동적으로 destruct and deallocate
```
- widget 클래스는 unique_ptr 타입의 data를 갖는다.
- data는 make_unique 함수를 통해 힙에 할당되는 배열이다.

힙 메모리 관리는 가능하면 스마트 포인트를 사용하는 것이 좋다.  
new와 delete 연산자로 명시해야하면, RAII 원칙을 따라야 한다.
- [RAII 링크][4]
- [스마트 포인터 링크][5]

## 문자열
C스타일 문자열도 주요 문제 중 하나다.  
`std::string`과 `std::wstring`을 사용하여 C스타일 문자열과 관련된 에러를 제거할 수 있다.  
읽기전용 접근만 필요하면 `std::string_view`를 사용하여 성능을 더 향상시킬 수 있다.  
안정성 뿐 아니라, 탐색이나 첨부 등의 멤버 함수를 이용할 수도 있다.
- [basic_string 링크][6]
- [basic_string_view 링크][7]

## 컨테이너
표준 라이브러리 컨테이너는 모두 RAII 원칙을 따른다.  
- 순차(sequential) 컨테이너로 raw [array][8] 대신 [`vector`][9]를 사용한다.
- 연관(associative) 컨테이너로 [unordered_map][10] 대신 [`map`][11]을 사용한다.  
또한 용도에 따라 [`set`][12], [`multiset`][13], [`multimap`][14]을 사용한다.
```
vector<string> apples;
apples.push_back("Granny Smith");

map<string, string> apple_color;
apple_color["Granny Smith"] = "Green";
```
성능 최적화가 필요하면 다음을 고려할 수 있다.
- 클래스 멤버 같이 임베딩이 중요한 경우, array 타입을 사용한다.
- unordered_map 같은 비정렬 연관 컨테이너는 원소당 오버헤드가 작고 룩업이 상수시간이다.
- sorted vector
- [관련 알고리즘 링크][15]

C스타일 배열은 사용하지 않도록 한다.  
오래된 API에서 직접 접근이 요구되면, `f(vec.data(), vec.size());` 같은 accessor method를 이용한다.  
[표준 라이브러리 컨테이너 더 보기][16]

## 표준 라이브러리 알고리즘
커스텀 알고리즘을 작성하기 전에 표준 라이브러리 알고리즘을 살펴보는 것이 좋다.  
- for_each(순회), find_if(탐색), transform(not-in-place 수정), sort(정렬), lower_bound 등
- 탐색, 정렬, 필터링, 무작위화(randomizing), 수학 알고리즘 등
```
auto comp = [](const widget& w1, const widget& w2)
    { return w1.weight() < w2.weight(); }

sort( v.begin(), v.end(), comp );

auto i = lower_bound( v.begin(), v.end(), comp );
```

## auto
변수, 함수, 템플릿 선언에 `auto` 키워드를 사용한다.  
auto는 객체의 타입을 컴파일러가 추론하도록(deduce)하여, 명시적으로 나타내지 않도록 한다.  
추론되는 타입이 중첩된(nested) 템플릿인 경우 특히 유용하다.
```
map<int,list<string>>::iterator i = m.begin(); // C스타일
auto i = m.begin(); // 모던 C++
```

## range-based for
```
# include <iostream>
# include <vector>

int main()
{
    std::vector<int> v {1,2,3};
    
    // C스타일
    for(int i = 0; i < v.size(); ++i)
    {
        std::cout << v[i];
    }
    
    // 모던C++
    for(auto& num : v)
    {
        std::cout << num;
    }
}
```
[범위 기반 for 링크][17]

## constexpr
매크로 대신 [`constexpr`][18]를 사용한다.  
매크로는 컴파일 전, 전처리기(preprocessor)를 통해 가공되는 토큰이다.  
C스타일에서는 컴파일-시간 상수(constant)를 정의하기 위해 자주 사용되었는데, 매크로는 디버깅이 어렵다.  
따라서 모던 C++에서는 컴파일-시간 상수로 constexpr 변수를 사용한다.
```
#define SIZE 10 // C스타일
constexpr int size = 10; // 모던 C++
```

## uniform 초기화
모던 C++에서는 어떤 타입이라도 괄호(brace) 초기화가 가능하다.  
이는 배열, 벡터나 다른 컨테이너를 초기화할 때 특히 편리하다.
```
#include <vector>

struct S
{
    std::string name;
    float num;
    S(std::string s, float f) : name(s), num(f) {}
};

int main()
{
    // C스타일
    std::vector<S> v;
    S s1("Norah", 2.7);
    S s2("Frank", 3.5);
    
    v.push_back(s1);
    v.push_back(s2);
    
    // 모던 C++
    std::vector<S> v2 {s1, s2}; // 위에 정의된 변수 이용
    std::vector<S> v3{ {"Norah", 2.7}, {"Frank", 3.5} }; // 직접 값 입력
}
```
[괄호 초기화 링크][19]

## move sematics
모던 C++은 move semantics를 제공하여, 불필요한 메모리 복사의 가능성을 제거한다.  
move 연산은 복사하지 않고 자원의 소유를 다른 객체로 옮길 수 있다.  
자원을 소유하는 클래스를 구현할 때, [move constructor와 move assignmet 연산자][20]를 정의할 수 있다.

## 람다식
C스타일은 함수 포인터를 이용하여 함수를 넘겼다.  
이는 유지와 인해가 힘들고 함수의 정의와 적용이 소스 코드 상에서 멀리 존재할 수 있으며 type-safe하지 않다.  
  
C++에서는 함수 객체를 제공하는데, 이는 operator() 연산자를 오버라이드하는 클래스로, 함수처럼 호출될 수 있다.  
함수 객체는 인라인 람다식에서 매우 유용하다.
```
std::vector<int> v {1,2,3,4,5};
int x = 2;
int y = 4;
auto result = find_if(begin(v), end(v), [=](int i) { return i > x && i < y; });
```
람다식 `[=](int i) { return i > x && i < y; }`은 `int i`를 인자로 받고 boolean을 반환하는 함수로 볼 수 있다.

## 예외
모던 C++은 에러 코드가 아닌 예외(exception)을 강조한다.  
[예외와 에러 핸들리 링크][21]

## atomic
inter-thread 통신(communication)을 위해 [`std::atomic`][22] struct와 관련된 타입을 사용한다.

## variant
C스타일에서는 union을 사용하여 서로 다른 타입의 멤버가 동일한 메모리 위치를 점유하게 하도록 하여 메모리를 보존한다.  
이는 type-safe하지 않고 에러를 유발한다.  
따라서 모던 C++에서는 [`std::variant`][23] 클래스를 사용하여 강인하고 안전하게 union을 대체한다.  
[`std::visit`][24] 함수를 이용하여 variant 타입의 멤버에 type-safe하게 접근할 수 있다.


[1]: https://docs.microsoft.com/en-us/cpp/standard-library/unique-ptr-class?view=msvc-170
[2]: https://docs.microsoft.com/en-us/cpp/standard-library/shared-ptr-class?view=msvc-170
[3]: https://docs.microsoft.com/en-us/cpp/standard-library/weak-ptr-class?view=msvc-170
[4]: https://docs.microsoft.com/en-us/cpp/cpp/object-lifetime-and-resource-management-modern-cpp?view=msvc-170
[5]: https://docs.microsoft.com/en-us/cpp/cpp/smart-pointers-modern-cpp?view=msvc-170
[6]: https://docs.microsoft.com/en-us/cpp/standard-library/basic-string-class?view=msvc-170
[7]: https://docs.microsoft.com/en-us/cpp/standard-library/basic-string-view-class?view=msvc-170
[8]: https://docs.microsoft.com/en-us/cpp/standard-library/array-class-stl?view=msvc-170
[9]: https://docs.microsoft.com/en-us/cpp/standard-library/vector-class?view=msvc-170
[10]: https://docs.microsoft.com/en-us/cpp/standard-library/unordered-map-class?view=msvc-170
[11]: https://docs.microsoft.com/en-us/cpp/standard-library/map-class?view=msvc-170
[12]: https://docs.microsoft.com/en-us/cpp/standard-library/set-class?view=msvc-170
[13]: https://docs.microsoft.com/en-us/cpp/standard-library/multiset-class?view=msvc-170
[14]: https://docs.microsoft.com/en-us/cpp/standard-library/multimap-class?view=msvc-170
[15]: https://docs.microsoft.com/en-us/cpp/standard-library/algorithms?view=msvc-170
[16]: https://docs.microsoft.com/en-us/cpp/standard-library/stl-containers?view=msvc-170
[17]: https://docs.microsoft.com/en-us/cpp/cpp/range-based-for-statement-cpp?view=msvc-170
[18]: https://docs.microsoft.com/en-us/cpp/cpp/constexpr-cpp?view=msvc-170
[19]: https://docs.microsoft.com/en-us/cpp/cpp/initializing-classes-and-structs-without-constructors-cpp?view=msvc-170
[20]: https://docs.microsoft.com/en-us/cpp/cpp/move-constructors-and-move-assignment-operators-cpp?view=msvc-170
[21]: https://docs.microsoft.com/en-us/cpp/cpp/errors-and-exception-handling-modern-cpp?view=msvc-170
[22]: https://docs.microsoft.com/en-us/cpp/standard-library/atomic-structure?view=msvc-170
[23]: https://docs.microsoft.com/en-us/cpp/standard-library/variant-class?view=msvc-170
[24]: https://docs.microsoft.com/en-us/cpp/standard-library/variant-functions?view=msvc-170#visit
