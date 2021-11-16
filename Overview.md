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
