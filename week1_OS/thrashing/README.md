## Thrashing

### Abstract

- CPU가 동작하는 상황 중에서, 
  - 프로세스의 명령어를 처리하는 비율이 낮으며
  - 스와핑과 관련된 작업을 처리하는 비율이 높은 상황
- 예시
  - 현재 시스템에 2GB의 메인 메모리를 사용할 수 있는 상황
    - 각 1GB의 메모리가 필요한 A, B, C 프로세스를 Round Robin 알고리즘을 사용하여 처리
    - 페이지 교체 알고리즘 - Least Recently Used 알고리즘을 사용

  - 동작
    1. A 프로세스 처리 
       - 메인 메모리에 1GB의 메모리 공간을 할당 (잔여 공간: 1GB)
    2. B 프로세스 처리 
       - 메인 메모리에 1GB의 메모리 공간을 할당 (잔여 공간: 0GB)
    3. C 프로세스 처리
       - A 프로세스를 swap-out (잔여 공간: 1GB)
       - C 프로세스를 swap-in (잔여 공간: 0GB)
    4. A 프로세스 처리
       - B 프로세스를 swap-out 
       - A 프로세스를 swap-in
    5. B 프로세스 처리
       - C 프로세스를 swap-out
       - B 프로세스를 swap-in
    6. 3, 4, 5번 반복


---

### Virtual Memory

- 메모리 할당 기법
  - 프로그램의 메모리 주소를 가상 메모리 공간에 할당
  - 가상 메모리 공간을 물리 메모리 공간에 할당
- 특징
  - 가상 메모리 공간을 메인 메모리 / 디스크에 분산하여 할당할 수 있다. 
    - 메모리에 접근할 때 추가적인 작업이 필요

---

### Page Fault / Swapping

- 개요
  - 접근하려는 페이지가, 메인 메모리에 존재하지 않는 상황
- OS의 동작
  1. 페이지 테이블 탐색
     - 메인 메모리에 존재하는 페이지에 대한 테이블
  2. Page Fault 발생
     - 메인 메모리에 존재하지 않는 페이지
  3. 현재 프로세스를 Blocked 상태로 변경
  4. 가상 메모리 공간을 탐색
  5. Swapping (부하가 큰 작업)
     - 접근하려는 페이지를 메인 메모리에 적재 (**파일 입출력**)
       - 메인 메모리의 공간이 부족할 경우, 페이지 교체 알고리즘 수행
       - 교체될 페이지는, 메인 메모리에서 디스크로 이동 (**파일 입출력**)
  6. 페이지 테이블 업데이트
  7. Blocked된 프로세스를, Ready 상태로 변경

---

### Thrashing

- 개요
  - CPU가 처리하는 작업의 대부분이, Swapping 작업인 상황
- 원인
  - High degree of multi-programming
    - 너무 많은 프로세스를 동시에 실행하는 경우
  - Lack of frames
    - 메인 메모리(RAM)의 용량이 적은 경우
- 요약
  - 물리 메모리 공간을 넘어서는, 가상 메모리 공간을 할당할 경우 발생

---

### Recovery(회복)

- 제한

  - Threshold를 넘어서는 메모리 공간이 필요한 경우, 더 이상의 프로세스를 적재하지 않도록 명령

- 유예(Suspend)

  - 우선 순위가 낮은 프로세스를 Ready 상태에서, Suspend 상태로 보내는 작업

    ![img](https://media.geeksforgeeks.org/wp-content/uploads/20190604122001/states_modified.png)

---

### Reference

- [Virtual Memory in Operating System - GeeksforGeeks](https://www.geeksforgeeks.org/virtual-memory-in-operating-system/)
- [Techniques to handle Thrashing  - GeeksforGeeks](https://www.geeksforgeeks.org/techniques-to-handle-thrashing/?ref=gcse)
- [memory management - What is thrashing? Why does it Occur? ](https://stackoverflow.com/questions/19031902/what-is-thrashing-why-does-it-occur)
- [States of a Process in Operating Systems - GeeksforGeeks](https://www.geeksforgeeks.org/states-of-a-process-in-operating-systems/)