# **3-Way-Handshake**

## 사전 지식

1. **TCP** : Transport Layer(전송 계층) 에 속해 있다. **전송 계층**은 두 호스트 간에 연결을 맺고 최종적인 통신 목적지 까지 데이터를 전달하는 기능을 담당.
2. **전송 계층**의 주요한 2가지 프로토콜 : TCP, UDP
3. **TCP의 특징** : 연결 지향적이며 오류제어, 흐름제어, 혼잡제어, 타이머 재전송 등의 기능을 함.
4. **연결 지향** : 데이터를 전송하는 측과 데이터를 전송 받는 측에서 전용의 데이터 전송 선로(Session)을 만든다는 의미
5. 즉, **TCP**는 <u>데이터의 신뢰도가 중요하다 판단될 때 사용</u> 



## 3-Way-HandShake 개념

- TCP 통신을 이용하여 데이터를 전송하기 위해 네트워크 **연결을 설정****(Connection Establish)** 하는 과정
- **양쪽 모두** 데이터를 전송할 준비가 되었다는 것을 **보장**하고, 실제로 **데이터 전달이 시작하기 전에** 한 쪽이 **다른 쪽이 준비되었다는 것을 알 수 있도록 한다**.
- 즉, TCP/IP 프로토콜을 이용해서 통신을 하는 응용 프로그램이 데이터를 전송하기 전에 먼저 정확한 전송을 보장하기 위해 상대방 컴퓨터와 사전에 세션을 수립하는 과정을 의미한다 (위 4, 5와 같은의미)



## 3-Way-HandShake 동작 과정

<img src="/Users/kit938639/Documents/study/CS-Study/week2_NETWORK/3_Way_HandShake/img/1.png" style="zoom:150%;" />

- TCP 통신은 **PAR**(Positive Acknowledgement with Re-transmission) 을 통해 신뢰적 통신 제공
- PAR란, 송신 측이 수신 측으로부터 데이터를 올바르게 받았다는 응답을 받지 않으면 응답을 받을 때 까지 데이터를 송신하는 행위를 말함.
- **수신 측**은 데이터 유닛(**세그먼트**)이 **손상**된것을 확인하면(Error Detection에 사용되는 transport layer의 checksum을 활용), **해당 세그먼트를 없앤다**. 그러면 **송신 측**은 **positive ack이 오지 않은 데이터 유닛을 <u>다시</u> 보내야한다**.
- 이 과정에서 송신 측과 수신 측 사이에서 **3개**의 Segment가 교환되는 것을 확인할 수 있다. 이것이 바로 3-way handshake의 기본 매커니즘



## 작동 방식

<img src="/Users/kit938639/Documents/study/CS-Study/week2_NETWORK/3_Way_HandShake/img/2.png" style="zoom:150%;" />

| 상태         | 설명                                        |
| ------------ | ------------------------------------------- |
| Closed       | 닫힌 상태                                   |
| Listen       | 포트가 열린 상태로 연결 요청 대기중         |
| SYN-sent     | SYN 요청을 한 상태                          |
| SYN-Received | SYN 요청을 받고 상대방의 응답을 기다리는 중 |
| Established  | 확인, 확립 된 상태, 즉 연결이 확인된 상태   |

![](/Users/kit938639/Documents/study/CS-Study/week2_NETWORK/3_Way_HandShake/img/3.png)

- Client는 Server와 연결하기 위해 **3-way handshake**를 통해 연결 요청
- Client와 Server는 모두 서로 연결 요청을 먼저 할 수 있기 때문에, 연결 요청을 **먼저 시도한 요청자를 Clien**t, **연결 요청을 받은 수신자를 Server**로 생각
- **SYN(Synchronization)** : 연결요청, 세션을 설정하는데 사용되며 초기에 시퀀스 번호를 보냄
- **ACK(Acknowledgement)** : 보낸 시퀀스 번호에 TCP 계층에서의 길이 또는 양을 더한 것과 같은 값을 ACK에 포함하여 전송

### 세부 동작 과정

1. **Step 1 (SYN)**

   - 요약 : 클라이언트는 서버와 커넥션을 연결하기 위해 SYN을 보낸다.

   - 헤더 상태 : (seq : x)

   - 내용 : 클라이언트가 최초로 데이터를 전송할 때 Sequence Number를 임의의 랜덤 숫자로 지정하고, SYN 플래그 비트를 1로 설정한 세그먼트를 전송한다.

   - PORT 상태

      Client : CLOSED 에서 SYN_SENT 로 변함

      Server : LISTEN

2. **Step 2 (SYN + ACK)**

   - 요약 : **서버가** SYN(x)을 받고,클라이언트로 받았다는 신호인 ACK와 SYN 패킷을 보냄 
   - 헤더 상태 : (seq : y, ACK : x + 1)

   - 내용 : 접속요청을 받은 서버가 요청을 수락했으며, 접속 요청 프로세스인 클라이언트도 포트를 열어달라는 메세지를 전송 (SYN-ACK signal bits set)

     ACK Number필드를 Sequence Number + 1 로 지정하고 SYN과 ACK 플래그 비트를 1로 설정한 새그먼트 전송 (Seq=y, Ack=x+1, SYN, ACK)

   - PORT 상태

      Client : CLOSED

      Server : SYN_RCV

3. **Step 3 (ACK)**

   - 요약 : 클라이언트는 서버의 응답은 ACK(x+1)와 SYN(y) 패킷을 받고, ACK(y+1)를 서버로 보냄

   - 내용 : 마지막으로 접속 요청 프로세스인 클라이언트가 수락 확인을 보내 연결을 맺음 (ACK)

     이때, 전송할 데이터가 있으면 이 단계에서 데이터를 전송할 수 있다.

   - PORT 상태

      Client : ESTABLISED

      Server : SYN_RCV ⇒ ACK ⇒ ESTABLISED



## 추가적으로 확인하면 좋을 내용

1. **왜 초기 Sequence Number를 ISN이라 하는데, 이는 0이 아닌 랜덤한 숫자가 보내질까?**
   - IP address spoofing, session hijacking과 같은 공격을 예방하기 위해

2. **단순히 응답을 주고받는데 2-way Handshake면 충분해보이지 않는가? 왜 3-way 일까?**
   - TCP/IP 통신은 **양방향성** connection 이다. A host가 B host에게 존재를 알리고 패킷을 받을수있다는 것을 증명하듯이, B **host도 패킷을 보낼수 있다는 신호를 보내야한다**. **이는 2-way handshaked에서는 성립될 수 없다**.
3. **연결을 할 때는 3-way-handshake를 하는데 연결을 끊을 때는 어떻게 할까?**
   - 4-way-handshak를 한다.

