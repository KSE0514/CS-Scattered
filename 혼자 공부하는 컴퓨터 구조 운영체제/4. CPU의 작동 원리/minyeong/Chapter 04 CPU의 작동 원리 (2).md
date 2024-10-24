# Chapter 04. CPU의 작동 원리 (2)
- [Chapter 04. CPU의 작동 원리 (2)](#chapter-04-cpu의-작동-원리-2)
- [04-3. 명령어 사이클과 인터럽트](#04-3-명령어-사이클과-인터럽트)
  - [명령어 사이클](#명령어-사이클)
  - [인터럽트](#인터럽트)
    - [**동기 인터럽트**](#동기-인터럽트)
    - [**비동기 인터럽트**](#비동기-인터럽트)
- [Q\&A](#qa)

# 04-3. 명령어 사이클과 인터럽트

![image.png](./Chapter%2004%20CPU의%20작동%20원리%20(2)/image.png)
## 명령어 사이클

- 하나의 명령어를 처리하는 정형화된 흐름
1. **인출 사이클 (fetch cycle)** : 메모리에 있는 명령어를 CPU로 가지고 오는 단계
2. **실행 사이클 (execution cycle)** : CPU로 가져온 명령어를 실행하는 단계
    - 제어 장치가 명령어 레지스터에 담긴 값 해석
    - 제어 신호 발생시키는 단계
3. **간접 사이클 (indirect cycle)** : 명령어를 실행하기 위해 메모리 접근을 더 해야하는 경우

## 인터럽트

- CPU의 작업을 방해하는 신호

### **동기 인터럽트**

![image.png](./Chapter%2004%20CPU의%20작동%20원리%20(2)/image%201.png)

- 예외
- CPU에 의해 발생하는 인터럽트
- CPU가 명령어들을 수행하다가 예상치 못한 상황에 마주쳤을 때 발생하는 동기 인터럽트
- **참고 : 예외의 종류**
    
    <aside>
    🆚 폴트 (fault) VS 트랩 (trap)
    
    - **예외 처리 직후** CPU가 본래 작업으로 돌아왔을 때 **예외가 발생한 명령어부터??** **예외가 발생한 명령어의 다음 명령어부터??**
    - **폴트 (fault)**
        
        : 예외 발생한 명령어부터 실행
        
    - **트랩 (trap)**
        
        : 예외 발생한 다음 명령어부터 실행
        
    </aside>
    
    <aside>
    🆚 중단 (abort)
    
    : CPU가 실행 중인 프로그램 강제 중단할 수 밖에 없는 심각한 오류 발견했을 때
    
    </aside>
    
    <aside>
    🆚 소프트웨어 인터럽트 (software interrupt)
    
    : 시스템 호출 발생했을 때
    
    </aside>
    

### **비동기 인터럽트**

![image.png](./Chapter%2004%20CPU의%20작동%20원리%20(2)/image%202.png)

- 하드웨어 인터럽트
- 입출력장치에 의해 발생하는 인터럽트
- CPU가 입출력 장치에 입출력 작업 부탁하면 작업 끝낸 **입출력장치가 CPU에 완료 알림 보냄**
- 즉, 입출력장치가 어떠한 입력 받아들였을 때 처리하기 위해 인터럽트 보냄
- 처리 순서
    - 입출력 장치가 CPU에 `인터럽트 요청 신호` 보냄 ⇒ CPU가 실행 사이클 후 명령어 인출 전 인터럽트 여부 확인 ⇒ 인터럽트 요청 확인 후 `인터럽트 플래그` 통해 현재 받아들일 수 있는지 확인 ⇒ 받아들일 수 있으면 CPU는 작업한 내용 스택에 백업 ⇒ `인터럽트 백터` 참조 해 프로그램 카운터 값 갱신 후 `인터럽트 서비스 루틴` 실행 ⇒ 끝나면 백업해 둔 작업 복구해 실행 재개
    
    <aside>
    1️⃣ 인터럽트 요청 신호
    
    : CPU의 작업을 방해하는 인터럽트에 대한 요청
    
    </aside>
    
    <aside>
    2️⃣ 인터럽트 플래그
    
    : 인터럽트 받아들일 지(가능), 무시할 지(불가능) 결정하는 플래그
    
    - 가장 우선 순위 높은 인터럽트 : 정전이나 하드웨어 고장 → 불가능이어도 무시 불가
    </aside>
    
    <aside>
    3️⃣ 인터럽트 벡터
    
    : 인터럽트 서비스 루틴을 식별하기 위한 정보
    
    - 인터럽트 서비스 루틴의 시작 주소 알 수 있음
    - 데이터 버스를 통해 전달 받음
    </aside>
    
    <aside>
    4️⃣ 인터럽트 서비스 루틴
    
    : 인터럽트 핸들러
    
    : 인터럽트 처리하기 위한 동작들로 이루어진 프로그램
    
    - 루틴 처리 후 본래 수행하던 작업으로 되돌아 옴
    - 명령어 + 데이터로 이루어져 레지스터를 사용하며 실행
    </aside>
    

# Q&A

1. **명령어 사이클의 유형에 대해 간략히 설명하시오.**
2. **동기 인터럽트와 비동기 인터럽트의 차이에 대해 설명하시오.**
3. **비동기 인터럽트의 처리 순서에 대해 설명하시오.** 

- **1번 답**
    
    명령어 사이클은 하나의 명령어가 처리되는 주기로, 인출, 실행, 간접, 인터럽트 사이클로 구성되어 있습니다. 
    
    인출 사이클은 메모리에서 명령어를 CPU로 가져오는 단계, 실행 사이클은 명령어를 실행하는 단계이며, 간접 사이클은 해당 명령어를 실행하기 위해 메모리 접근을 더 해야하는 경우 발생합니다. 또 인터럽트 사이클은 CPU의 작업을 방해하는 신호가 나타났을 때 발생합니다.
    
- **2번 답**
    
    동기 인터럽트는 CPU에 발생하는 인터럽트이며, 비동기 인터럽트는 입출력 장치에 의해 발생하는 인터럽트입니다.
    
- **3번 답**
    
    입출력 장치가 CPU에 `인터럽트 요청 신호` 보내면 CPU가 실행 사이클 후 명령어 인출 하기 전에 인터럽트 여부 확인합니다. 확인이 끝나면 `인터럽트 플래그` 통해 현재 상황을 받아들일 수 있는지 확인하고, 받아들일 수 있으면 작업한 내용을 스택에 백업합니다. 이후 `인터럽트 백터` 를 참조 해 프로그램 카운터 값 갱신 후 `인터럽트 서비스 루틴` 을 실행하게 되고,  인터럽트 서비스 루틴이 끝나면 백업해 둔 작업 복구해 실행을 재개합니다.