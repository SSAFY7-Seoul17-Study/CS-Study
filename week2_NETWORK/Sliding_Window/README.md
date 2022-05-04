## Sliding Window 기법

### Stop and Wait

- 개요

  - 데이터를 전송하는 측 / 수신하는 측 간 연결을 유지한 채 진행되는 통신 방식
  - Error / Flow에 대한 제어를 지원한다. 
  - Data Link 계층(2), Transport 계층(4)에서 사용된다. 

- 이미지

  ![Lightbox](https://media.geeksforgeeks.org/wp-content/uploads/Stop-and-Wait-ARQ.png)

  - Sender
    - 데이터 패킷을 하나씩 전송한다. 
    - ACK 신호를 수신한 이후에, 다음 데이터 패킷을 전송한다. 
  - Receiver
    - 데이터 패킷을 수신 / 소모한 이후 ACK 신호를 보낸다. 
    - 흐름 제어(Flow control)를 위해, 패킷을 소모한 이후에 ACK 신호를 보낸다. 
    - ACK: Acknowledgement(승인)

- 문제점

  1. 패킷 유실(Lost)
     - 데이터 패킷이 유실될 경우, 연결 상태를 회복할 수 없다. 
  2. 패킷 지연(Delayed)
     - 지연된 데이터 패킷이 도착할 경우, 통신의 흐름(Flow)이 끊긴다. 

---

### Stop and Wait ARQ(Automatic Repeat Request)

- 개요
  - Stop and Wait 프로토콜의 문제점을 해결하기 위해 나온 기법
- 해결책
  - Time Out
    - 일정 시간 동안 ACK 신호를 받지 못한 경우, 데이터 패킷을 재전송
    - 패킷 유실에 대한 해결책
  - Sequence Number (bit)
    - 데이터 패킷, ACK 신호에 색인을 추가
    - 동작
      1. Data 0 전송
      2. Data 0 수신, ACK 1 전송
      3. ACK 1 수신, Data 1 전송
      4. ...
- 한계
  - 자원의 낭비가 존재할 수 있다. 
    - 네트워크의 대역폭(Bandwidth)이 큰 경우
      - 고정된 데이터 패킷을 송수신하므로, Throughput에 한계가 있다. 
    - Round Trip Time(RTT)가 큰 경우
      - ACK 신호가 오기 전까지 다음 신호를 전송할 수 없으므로, RTT 이상의 시간 동안 기다려야 한다. 

---

### Sliding Window Protocol

- **이유**

  - Stop and Wait ARQ 프로토콜의 경우, 중간에 낭비되는 시간이 존재한다. 
  - Pipelining 기법을 적용하여, 낭비되는 시간을 줄이는 목적을 갖는다. 

- **알아야 하는 개념**

  - Transmission delay (T~t~)
    - 하나의 패킷를 처리하는 데 걸리는 시간
  - Propagation delay (T~p~)
    - 패킷을 네트워크의 현재 노드에서, 목적 노드까지 전송하는 데 걸리는 시간
  - Stop and Wait 프로토콜
    - 통신 사이클에 걸리는 시간
      - (packet) T~t~ + T~p~ + (ack) T~t~ + T~p~
      - (ack) T~t~는 무시해도 무방하다. (ACK 신호는 데이터의 크기가 매우 작다. )
      - = T~t~ + 2*T~p~
  - Pipelining
    - 특정한 작업이 완료되기를 기다릴 때, 다른 작업을 처리하는 기법

- **전송하는 단(Sender)**

  - Window

    - 전송했으나, 아직 ACK 신호를 받지 못한 패킷의 순번을 포함한다. 

  - 최소 Window의 크기

    - 하나의 통신 사이클에 걸리는 시간 / 하나의 패킷을 처리하는 데 걸리는 시간
    - = (T~t~ + 2*T~p~) / T~t~

  - 패킷의 순번 (Sequence number)

    - 각 패킷에 번호를 매겨서 관리한다. 
    - 최소 Window의 크기만큼의 번호가 필요하다. 

  - 이미지

    ![Lightbox](https://media.geeksforgeeks.org/wp-content/cdn-uploads/Sliding_Window_Protocol_1.jpg)

    ![Lightbox](https://media.geeksforgeeks.org/wp-content/cdn-uploads/Sliding_Window_Protocol_2.jpg)

- **Go Back N (GBN)**

  - 수신하는 단 (Receiver)

    - N개의 패킷을 수신한 이후에, ACK 신호를 보낸다. 
    - N개의 패킷을 수신하는 중간에 패킷을 수신하지 못한 경우
      - 수신하지 못한 패킷의 번호를 ACK 신호에 담아서 전송한다. 
    - Receiver 단의 Window 크기는 1이면 충분하다. 

  - 전송하는 단 (Sender)

    - N개의 패킷을 연속적으로 전송한다. 
    - 이후, 첫 번째 패킷의 순번(i)에 대한 ACK 번호가 도착하지 않는다면 재전송한다. 
    - (N+1) 크기의 Window를 필요로 한다. 

  - 이미지

    ![Lightbox](https://www.geeksforgeeks.org/wp-content/uploads/Sliding_SET_2-4.jpg)

- **Selective Repeat (SR); Selective Repeat Protocol(SRP)**

  - 이유

    - Go Back N (GBN) 프로토콜의 경우, 네트워크 상황이 좋지 않은 경우 성능이 하락한다. 
    - 이에 대한 성능 개선을 위해, Selective Repeat Protocol(SRP)를 사용할 수 있다. 

  - 차이점

    - 수신하는 단(Receiver)
      - 패킷에 대한 버퍼링이 가능해야 한다. (다수의 패킷을 저장한다. )
      - 순서가 잘못된 패킷 또한 받아들인다. 
      - 받아들인 패킷에 대해, 전송하는 단(Sender)에 ACK 신호를 송신한다. 
      - 제대로 전송되었음을 알려주는 신호(ACK) 뿐만이 아니라, 제대로 전송되지 않은 패킷에 대한 신호(NAK) 또한 전송할 수 있다. 
    - 전송하는 단(Sender)
      - 제대로 전송되지 않은 패킷만을 재전송할 수 있어야 한다. 
      - TimeOut 이후, 모든 제대로 전송되지 않은 패킷을 전송한다. 

  - 이미지

    ![Lightbox](https://media.geeksforgeeks.org/wp-content/uploads/Sliding-Window-Protocol.jpg)

---

### Reference

- [Stop and Wait ARQ](https://www.geeksforgeeks.org/stop-and-wait-arq/)
- [Sliding Window Protocol | Set 1 (Sender Side)](https://www.geeksforgeeks.org/sliding-window-protocol-set-1/)
- [Sliding Window Protocol | Set 2 (Receiver Side)](https://www.geeksforgeeks.org/sliding-window-protocol-set-2-receiver-side/)