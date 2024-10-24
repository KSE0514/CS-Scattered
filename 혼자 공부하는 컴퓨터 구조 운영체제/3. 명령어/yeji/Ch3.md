# 3회차

## Title: 혼자 공부하는 컴퓨터 구조 운영체제

### Subject: 명령어

### Date: 240728

<aside>
<img src="https://www.notion.so/icons/drafts_purple.svg" alt="https://www.notion.so/icons/drafts_purple.svg" width="40px" /> **NOTES**

# 소스 코드와 명령어

### 모든 소스 코드는 컴퓨터 내부에서 명령어로 변환

## 고급언어와 저급언어

1. 고급언어 : 사람을 위한 언어
2. 저급언어 : 컴퓨터가 이해하고 실행할 수 있는 언어

⇒ 고급 언어로 작성된 소스 코드는 반드시 실행위해서 저급 언어(명령어)로 변환되어야 함

![Untitled](./image/1.png)

- 기계어 : `0과 1` 의 명령어 비트로 이뤄진 언어
    - 기계어를 이진수로 나열하면 너무 길어져서 가독성을 위해 십육진수로 표현하기도
    - 기계어는 오로지 컴퓨터만을 위해 만들어진 언어라 사람 이해X
- 어셈블리어 : 0과 1로 표현된 명령어(기계어)를 읽기 편한 형태로 번역한 언어
    - 어셈블리어 한 줄 한 줄 == 명령어
    - 0과 1로 이뤄진 기계어를 읽기 편하게 만든 저급 언어일 뿐 → 개발자가 어셈블리어로 프로그래밍하기는 어려워
        - 그래서 `고급언어` 필요
    - 작성의 대상 & 관찰의 대상 - 어떤 과정으로 실행하는지 관찰 가능

## 컴파일 언어와 인터프리터 언어

### 컴파일 언어

컴파일러에 의해 소스 코드 `전체` 가 저급 언어로 변환되어 실행되는 고급 언어

ex) C

- Compile(컴파일) : 컴파일 언어로 작성된 소스 코드의 코드 전체가 저급 언어로 변환되는 과정
- Compiler(컴파일러) : 컴파일을 수행해 주는 도구
- 컴파일러가 소스 코드 내에서 `하나라도 오류` 발견하면 `컴파일 실패`
- 목적 코드 : 컴파일러 통해 저급 언어로 변환된 코드

![Untitled](./image/2.png)

### 인터프리터 언어

소스 코드가 인터프리터에 의해 `한 줄씩` 실행되는 고급 언어

ex) Python

- Interpreter(인터프리터) : 소스 코드를 한 줄씩 저급 언어로 변환하여 실행해 주는 도구
- 소스 코드를 한 줄씩 실행 → `N번째 줄 오류` ⇒ `N-1번째 줄까지는 올바르게 수행`

⇒ 일반적으로 인터프리터 언어가 컴파일 언어보다 느림.

➕ 목적 파일  vs 실행 파일

- 목적 파일 : 목적 코드로 이뤄진 파일
- 실행 파일 : 실행 코드로 이뤄진 파일 ex) .exe 파일

→ 목적 코드가 실행 파일이 되기 위해서는 `링킹(linking)` 이라는 작업 거쳐야 함

---

# 명령어의 구조

## 연산 코드와 오퍼랜드

### 명령어 = 연산 코드 + 오퍼랜드

### 연산 코드(operation code) (= 연산자)

명령어가 수행할 연산

- 일반적으로 명령어의 첫 부분에 위치하며, 고정된 비트 수를 차지
- 예시: 산술 연산(덧셈, 뺄셈 등), 논리 연산(AND, OR 등), 데이터 이동, 제어 흐름(분기, 점프) 등을 포함
- 각 프로세서 아키텍처마다 고유한 연산 코드 집합을 가지고 있음

### 오퍼랜드(operand) (= 피연산자)

연산에 사용할 데이터 or 연산에 사용할 데이터가 저장된 위치(주소 필드)

- 둘 중에 `연산에 사용될 데이터가 저장된 위치` 가 훨씬 더 자주 사용
- `연산 코드 필드` : 연산 코드가 담기는 영역
- `오퍼랜드 필드` : 오퍼랜드가 담기는 영역
- 명령어에 따라 오퍼랜드의 수가 다를 수 있음 (0~3개 또는 그 이상)
- 오퍼랜드는 상수, 메모리 주소, 레지스터 등 다양한 형태로 표현
- 주소 지정 방식에 따라 오퍼랜드의 해석 방법이 결정

## 주소 지정 방식

오퍼랜드의 실제 값이나 위치를 어떻게 결정할지 지정하는 방법

### 즉시 주소 지정 방식 (Immediate Addressing Mode)

- 오퍼랜드가 실제 데이터 값 자체
- 예: ADD R1, #5 (R1에 5를 더함)
- 장점: 빠른 실행 속도
- 단점: 표현할 수 있는 값의 범위가 제한적

### 직접 주소 지정 방식 (Direct Addressing Mode)

- 오퍼랜드가 메모리 주소를 직접 나타냅니다.
- 예: LOAD R1, [1000] (메모리 주소 1000의 내용을 R1에 로드)
- 장점: 간단하고 이해하기 쉬움
- 단점: 접근할 수 있는 메모리 범위가 제한적일 수 있음

### 간접 주소 지정 방식 (Indirect Addressing Mode)

- 오퍼랜드가 실제 데이터의 주소를 담고 있는 메모리 주소를 나타냄
- 예: LOAD R1, [[1000]] (주소 1000에 저장된 값을 주소로 사용하여 그 주소의 내용을 R1에 로드)
- 장점: 유연한 메모리 접근 가능
- 단점: 메모리 접근이 두 번 필요하여 속도가 느릴 수 있음

### 레지스터 주소 지정 방식 (Register Addressing Mode)

- 오퍼랜드가 레지스터를 직접 지정
- 예: ADD R1, R2 (R2의 값을 R1에 더함)
- 장점: 매우 빠른 실행 속도
- 단점: 레지스터의 수가 제한적

### 레지스터 간접 주소 지정 방식 (Register Indirect Addressing Mode)

- 오퍼랜드가 메모리 주소를 담고 있는 레지스터를 지정
- 예: LOAD R1, [R2] (R2가 가리키는 메모리 주소의 내용을 R1에 로드)
- 장점: 유연한 메모리 접근과 빠른 주소 계산
- 단점: 직접 주소 지정보다는 약간 느림

</aside>

---

<aside>
<img src="https://www.notion.so/icons/question-mark_purple.svg" alt="https://www.notion.so/icons/question-mark_purple.svg" width="40px" /> **QUESTIONS**

1. **고급 언어와 저급 언어의 차이점을 설명하고, 컴파일 언어와 인터프리터 언어를 비교해 주세요. 각각의 장단점은 무엇인가요?**

고급 언어는 사람이 이해하기 쉬운 형태로 작성된 프로그래밍 언어입니다. 반면 저급 언어는 컴퓨터가 직접 실행할 수 있는 기계어나 어셈블리어를 말합니다.

컴파일 언어는 소스 코드 전체를 한 번에 기계어로 번역합니다. 장점은 실행 속도가 빠르고 한 번 컴파일하면 다시 번역할 필요가 없다는 것입니다. 단점은 컴파일 시간이 필요하고, 다른 플랫폼에서 실행하려면 재컴파일해야 한다는 점입니다.

인터프리터 언어는 소스 코드를 한 줄씩 해석하여 실행합니다. 장점은 즉시 실행 가능하고 디버깅이 쉽다는 것입니다. 단점은 실행 속도가 상대적으로 느리다는 점입니다.

2. **명령어의 기본 구조에 대해 설명해 주세요. 연산 코드와 오퍼랜드의 역할은 무엇이며, 왜 이러한 구조가 필요한지 설명해 주세요.**

명령어는 기본적으로 연산 코드(operation code)와 오퍼랜드(operand)로 구성됩니다.

연산 코드는 수행할 연산의 종류를 지정합니다. 예를 들어 덧셈, 뺄셈, 데이터 이동 등의 연산을 나타냅니다.

오퍼랜드는 연산에 사용될 데이터나 데이터의 위치를 지정합니다. 오퍼랜드는 실제 데이터 값이거나 메모리 주소, 레지스터 등일 수 있습니다.

이러한 구조가 필요한 이유는 컴퓨터가 수행해야 할 작업(연산 코드)과 그 작업에 필요한 데이터(오퍼랜드)를 명확히 구분하여 효율적으로 처리할 수 있게 하기 위함입니다. 이를 통해 다양한 연산을 수행하고 데이터를 조작할 수 있는 유연성을 제공합니다.

3.  **주소 지정 방식 중 직접 주소 지정 방식과 간접 주소 지정 방식의 차이점을 설명하고, 각각의 장단점을 비교해 주세요. 어떤 상황에서 어떤 방식이 더 유용할 수 있을까요?**

직접 주소 지정 방식: 오퍼랜드에 직접 메모리 주소를 지정합니다.

- 장점: 간단하고 빠름
- 단점: 접근 가능한 메모리 범위가 제한적

간접 주소 지정 방식: 오퍼랜드에 실제 데이터 주소를 담고 있는 메모리 주소를 지정합니다.

- 장점: 유연한 메모리 접근, 복잡한 데이터 구조 처리 가능
- 단점: 메모리 접근이 두 번 필요해 상대적으로 느림

직접 방식은 단순한 데이터 접근에, 간접 방식은 동적 메모리 할당이나 포인터를 사용하는 복잡한 구조에 유용합니다.
</aside>