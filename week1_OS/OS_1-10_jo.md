## OS for interview

### Abstract

- 운영체제론(OS)의 주된 관심사에 대해 알아보고, 그에 대한 답변을 준비
  - 참조: https://github.com/devham76/tech-interview-study#3-operating-system

---

### Questions & Answers

1. **OS**에 대하여, 그리고 **핵심 기능**에 대하여 설명하시오

   > - OS는 컴퓨터의 시스템 레벨에 위치한 소프트웨어로써, 컴퓨터의 자원을 효율적으로 관리하고 / 프로그램에 필요한 공통적인 기능들을 제공합니다. 
   > - 핵심 기능
   >   - 자원 관리 측면
   >     - 프로세스, 메모리, 디스크 입출력 및 파일 시스템 관리
   >   - 공통 기능 제공 측면
   >     - 네트워크, 보안, UI에 관련된 기능을 제공

2. **부팅의 과정**을 설명하시오

   > 1. Startup; 시작 단계
   >    - BIOS와 CPU와 같은 컴퓨터 시스템의 주 자원에 전력을 공급
   > 2. BIOS: Power On Self Test
   >    - Basic Input/Output System이 수행하는 자가 진단
   >    - 컴퓨터의 메인 메모리, 디스크, 등이 제대로 동작하는지 진단
   > 3. OS 적재
   >    - 디스크에 존재하는 운영체제가, 메인 메모리에 적재
   >    - 운영체제의 초기화 파일 및 명령이 실행
   > 4. 시스템 설정
   >    - 메인 메모리에, 드라이버들이 적재
   >    - 드라이버는, 주변기기들이 동작할 수 있도록 도와주는 프로그램
   > 5. 시스템 유틸리티
   >    - 유용한 프로그램들이 메모리에 적재
   > 6. 사용자 인증
   >    - 비밀번호가 설정된 시스템의 경우, 사용자 인증을 통해 시스템을 시작

3. **프로세스의 5가지 상태**에 대하여 설명하시오

   > - New
   >   - 생성 중인 프로세스
   >   - Process Control Block(PCB)는 메모리에 존재하지만, 아직 프로그램이 메모리에 존재하지 않는 상태
   >   - 다수의 프로세스를 메인 메모리 올려둘 경우 Thrashing이 발생한다. 
   >     - OS 차원에서 이를 방지하기 위해, 프로세스를 New 상태에 유지하여 관리한다. 
   > - Ready
   >   - 실행(Execution)을 기다리는 프로세스의 상태
   >   - PCB와 프로그램 모두 메인 메모리에 존재
   > - Running
   >   - 현재 CPU에 의해 자원을 할당받아 실행 중인 프로세스의 상태
   > - Blocked
   >   - 특정한 이벤트가 완료될 때까지 기다리는 프로세스의 상태
   >     - I/O, 프로세스의 종료, 동기화 신호 등
   > - Terminated
   >   - 종료되었거나, 특정 사유로 인해 중단된 프로세스의 상태
   >   - OS에 의해 바로 자원이 해제되는 것이 아닌, 특정 시간 동안 자원을 유지한다. 
   > - 5 State Process Model의 단점
   >   - 다수의 작업이 Blocked 상태에 존재할 경우
   >     - 메인 메모리의 작업 공간을 차지하여 시스템의 전체적인 성능을 감소시킨다. 
   >   - 해결책
   >     - Suspended(유예) 상태를 추가한 6, 7 State Process Model
   >     - 모든 프로세스가 Blocked 상태일 경우, 이를 Suspended 상태로 변환
   >     - Suspended 상태의 프로세스는 메인 메모리에서, 2차 메모리로 옮겨진다. 

4. **메모리의 계층 구조**를 설명하시오

   > - 설계
   >   - CPU에서 가까운 메모리
   >     - 접근 속도는 빠르게, 저장 가능한 데이터는 적게, 비트 당 가격은 많게
   >   - CPU에서 먼 메모리
   >     - 접근 속도는 느리게, 저장 가능한 데이터는 많게, 비트 당 가격은 적게
   >   - 레지스터 - 캐시 - 메인 메모리 - 디스크의 계층 구조를 주로 차용
   > - 효용
   >   - 참조 지역성의 원칙에 따라서, 현재 참조하고자 하는 메모리의 주소는 가까운 시간 안에, 가까운 공간 안에 위치할 확률이 높다. 
   >     - 접근 속도가 빠른 메모리를 CPU에 가까이 위치시켜, 접근 속도를 향상
   >   - 접근 속도가 빠른 메모리를 적게 가져감으로써, 경제적으로도 효율적인 시스템을 구축

5. **캐시와 버퍼의 차이점**을 설명하시오

   > - 버퍼
   >   - 메인 메모리에 존재하는 임시 저장 공간 - DRAM
   >   - 프로세스 간 데이터를 통신함에 있어서, 서로 다른 통신 속도의 간극을 메꾸기 위해 사용된다. 
   > - 캐시
   >   - 컴퓨터의 저장 공간으로써, 메모리 계층 구조 상에서 CPU의 레지스터와 메인 메모리의 사이에 위치
   >   - 메인 메모리보다 더 빠른 접근 속도를 갖는다. - SRAM
   >   - 캐시는 메인 메모리 / 디스크에 모두 적용 가능하다. 

6. **세마포어**와 **뮤텍스**에 대하여, 그리고 그 **차이점**에 대하여

   > - 공통점
   >   - 커널 자원으로써, 동기화 서비스를 제공
   >   - 동기화 서비스
   >     - 임계 영역에 하나의 스레드만 접근 가능하게 한다. 
   >     - 그렇게 함으로써, 데이터의 원자성을 유지한다. 
   > - 차이점
   >   - 뮤텍스는 '잠금'에 초점이 맞추어져 있는 동기화 서비스
   >     - 임계 영역을 접근하려 하는, 여타 스레드의 접근이 제한된다. 
   >   - 세마포어는 '신호'에 초점이 맞추어져 있는 동기화 서비스
   >     - 임계 영역에서의 작업이 끝난 스레드는, Blocked / Waiting 상태의 스레드에 신호를 보낸다. 

7. **메모리 단편화** 및 **페이징**과 **세그멘테이션**에 대하여

   > - 메모리 단편화
   >   - 프로세스가 메모리에 할당 및 해제가 반복적으로 일어날 경우, 메모리에 중간중간에 자그마한 빈 메모리 공간이 생기는 것
   >   - Internal / External 단편화가 존재
   > - 페이징
   >   - 메모리 단편화를 해결하기 위해, 물리적 메모리를 여러 구간으로 분리하여 관리
   >     - 각 구간은, 동일한 크기를 갖는다. 
   >   - 각 프로세스는, 가상의 메모리 주소를 가지며, 이는 물리적 메모리 주소와 매핑
   >   - 가상 메모리 공간을 페이지, 물리 메모리 공간을 프레임이라고 한다. 
   > - 세그멘테이션
   >   - 프로세스를 여러 세그먼트로 분리하여, 세그먼트 테이블에 이에 해당하는 물리 메모리 주소로 매핑
   >   - 각 세그먼트들은, 크기가 달라도 무방하다. 

8. **선점 스케줄링**과 **비선점 스케줄링**, 그에 해당하는 **알고리즘**에 대하여

   > - CPU가 자원을 선점할 프로세스를 결정하는 결정권을 갖고 있는 것이, 선점 스케줄링
   >   - Round Robin
   > - 프로세스가 자원을 선점하는 결정권을 갖고 있는 것이, 비선점 스케줄링
   >   - FCFS (First Come First Served)

9. **문맥 교환**에 대하여

   > - 현재 프로세스의 문맥 정보를 저장한 이후, 실행할 다음 프로세스의 문맥 정보를 가져오는 것
   >   - Running 상태에서 CPU 자원을 빼앗길 경우, 문맥 정보가 저장된다. 
   >   - Ready 상태에서 CPU 자원을 할당받을 경우, 문맥 정보가 로딩된다. 

10. **PCB**에 대하여

   > - 프로세스에 관한 정보를 포함하는 데이터
   >   - 프로세스의 상태, ID, 레지스터 정보, 프로그램 카운터, 등의 정보를 포함

---

### Reference

- [Medium - Five State Process Model](https://medium.com/@sohailk1999/five-state-process-model-6e83d7428c8c)
- [SlayStudy - Process State Models in Operating System](https://slaystudy.com/process-state-models-in-operating-system/)
- [GeeksforGeeks - Memory Hierarchy Design and its Characteristics](https://www.geeksforgeeks.org/memory-hierarchy-design-and-its-characteristics/)
- [Difference between Buffer and Cache](https://www.geeksforgeeks.org/difference-between-buffer-and-cache/?ref=gcse)
- [Toppr - Concept of Booting](https://www.toppr.com/guides/computer-science/computer-fundamentals/classification-of-computers/concept-of-booting/)
- [Mutex vs Semaphore - GeeksforGeeks](https://www.geeksforgeeks.org/mutex-vs-semaphore/)
- [Memory Management in Operating System - GeeksforGeeks](https://www.geeksforgeeks.org/memory-management-in-operating-system/?ref=gcse)
- [Segmentation in Operating System - GeeksforGeeks](https://www.geeksforgeeks.org/segmentation-in-operating-system/?ref=gcse)
- [Introduction of Process Management - GeeksforGeeks](https://www.geeksforgeeks.org/introduction-of-process-management/?ref=gcse)