# SSL

### SSL을 소개하기에 앞서 HTTP와 HTTPS를 간단히 알아보면,

- HTTP : 통신 프로토콜, 데이터 암호화가 되어 있지 않음
- HTTPS : HTTP에 SSL 프로토콜을 통해 암호화해 사용하는 프로토콜

### 그렇다면 이런 암호화된 프로토콜을 사용하는 이유는 무엇이 있을까?

클라이언트와 서버 간의 통신 과정을 보면 중간에 여러 경로를 거친다. 암호화를 하지 않을 경우, 

1. 경로 상에 있는 것은 별다른 큰 노력없이 통신 내용을 볼 수 있다.
2. 통신하려는 상대가 진짜 그 대상인지 확실하게 알 수 없다.
3. 통신 과정 중간에 내용이 변조될 수 있다.

### 그렇다면 이 암호화를 가능하게 해주는 SSL은 무엇일까?

## 1. SSL 개념

- SSL : Secure Sockets Layer(보안 소켓 계층)의 약자
- Netscape사에서 웹서버와 브라우저 사이의 보안을 위해 만듬 [1.png]
- 인터넷과 같이 TCP/IP 네트워크를 사용하는 통신에 적용됨. [2.png]

![1.png](/Users/kit938639/Documents/study/CS-Study/week3_NETWORK/SSL/img/1.png)           ![2.png](/Users/kit938639/Documents/study/CS-Study/week3_NETWORK/SSL/img/2.png)

- SSL과 TLS

  : SSL과 TLS(Transprot Layer Security Protocol)는 같은 의미 TLS는 Netscape 사에서 만든 SSL이 표준화 기구인 ITEF의 관리로 변경되면서 TLS라는 이름으로 바뀜 TLS 1.0은 SSL 3.0을 계승함.

## 2. SSL의 암호화

SSL의 핵심은 암호화이다. SSL은 보안과 성능상의 이유로 두가지 암호화 기법을 혼용해서 사용. SSL의 동작방법을 이해하기 위해서는 이 암호화 기법들을 이해해야 함.

### 1. 대칭키 방식

- 한 개의 키를 사용해 암호화와 복호화를 수행하는 암호화 방법
- 키 생성이 빠르지만 키를 탈취 당할 경우 보안에 취약함
- 때문에 키 교환이 어려움

![3.png](/Users/kit938639/Documents/study/CS-Study/week3_NETWORK/SSL/img/3.png)

### 2. 공개키 방식

- 대칭키 방식과 다르게 2개의 키를 가지고 암호화 복호화를 수행, 그래서 비대칭키 방식으로도 부름
- 하나는 공개키(public key), 나머지 하나는 비밀키(private key, 또는 개인키)
- 공개키는 어디든지 배포 가능, 비밀키는 본인만 소지
- 공개키로 암호화를 하면 비밀키로 복호화를 한다.
  - 공개키가 유출되더라도 비밀키가 유출되지 않았다면 복호화를 할 수 없기에 안전함
- 비밀키로 암호화를 하면 공개키로 복호화를 한다. ???
  - 공개키는 배포를 하는데 이러면 보안상 취약할 텐데 이렇게 하는 이유는?
  - 데이터의 보호가 목적이 아님
  - 암호화된 데이터를 공개키를 가지고 복호화 할 수 있다는 것은 그 데이터가 공개키와 쌍을 이루는 비공개키에 의해서 암호화 되었다는 것을 의미
  - = 공개키가 데이터를 제공한 사람의 신원을 보장해줌. 이것을 전자 서명이라 부른다.
- 공개키를 교환하는 것은 매우 쉬움
- 하지만 암호화 속도가 느림

![4.png](/Users/kit938639/Documents/study/CS-Study/week3_NETWORK/SSL/img/4.png)



> SSL은 이 두 방식의 장점을 취해 암호화를 진행

## 3. SSL 통신과정

- SSL은 공개키 방식으로 대칭키를 전달함.

### 공개키 방식으로 대칭키를 전달하는 과정

1. A에서 B로 접속 요청을 보냄

![5.png](/Users/kit938639/Documents/study/CS-Study/week3_NETWORK/SSL/img/5.png)

2. B는 A에게 자신의 공개키를 전송

![6.png](/Users/kit938639/Documents/study/CS-Study/week3_NETWORK/SSL/img/6.png)

3. A는 자신의 대칭키를 B에서 전달받은 B의 공개키로 암호화

![7.png](/Users/kit938639/Documents/study/CS-Study/week3_NETWORK/SSL/img/7.png)

4. 암호화한 자신의 대칭키를 B에게 전달

![8.png](/Users/kit938639/Documents/study/CS-Study/week3_NETWORK/SSL/img/8.png)

5. B는 A의 대칭키를 자신의 비밀키로 복호화, 이 결과로 A의 대칭키를 얻어냄.

![9.png](/Users/kit938639/Documents/study/CS-Study/week3_NETWORK/SSL/img/9.png)

6. 얻은 대칭키를 통해 A와 B는 안전하게 통신

![10.png](/Users/kit938639/Documents/study/CS-Study/week3_NETWORK/SSL/img/10.png)

정리하면 데이터 암호화와 복호화를 위한 한 쪽의 대칭키를 다른 쪽의 공개키로 암호화하여 전송하면 반대편에서 자신의 비밀키로 복호화하여 그 반대편의 대칭키를 알아내고 이 대칭키를 바탕으로 통신을 함.

### 그렇다면 사용자가 접속한 사이트가 유효한 사이트인지 확인은 어떻게 하는가?

이 과정에는 제 3의 인증기관(CA)가 포함된다.

* CA(Certificate authority) : SSL 인증서(클라이언트가 접속한 서버가 클라이언트가 의도한 서버가 맞는지 보장)를 발급하는 기관

1. 우선, 사이트는 사이트 인증서가 필요하다. 사이트 인증서는 인증기관에서 사이트에게 발급하는 문서, 이를 위해 사이트에서 인증기관에게 사이트 정보와 사이트 공개키를 전달

![11.png](/Users/kit938639/Documents/study/CS-Study/week3_NETWORK/SSL/img/11.png)

2. 인증기관에서는 사이트 인증서를 발급하기 전, 먼저 전달받은 데이터를 검증

![12.png](/Users/kit938639/Documents/study/CS-Study/week3_NETWORK/SSL/img/12.png)

3. 인증기관에서 성공적으로 검증을 완료하면, 인증기관은 사이트 인증서를 생성하기 위해 데이터를 자신의 비밀키로 서명

![13.png](/Users/kit938639/Documents/study/CS-Study/week3_NETWORK/SSL/img/13.png)

4. 서명 이 후 사이트 인증서가 생성되고, 인증기관은 이 인증서를 사이트에 전달

![14.png](/Users/kit938639/Documents/study/CS-Study/week3_NETWORK/SSL/img/14.png)

5. 인증기관은 사용자에게 자신의 공개키 전달

![15.png](/Users/kit938639/Documents/study/CS-Study/week3_NETWORK/SSL/img/15.png)

6. 사용자가 인증기관으로부터 전달받은 인증기관 공개키는 사용자 브라우저에 자동으로 내장

![16.png](/Users/kit938639/Documents/study/CS-Study/week3_NETWORK/SSL/img/16.png)

7. 사용자가 사이트에 접속요청을 보내면 사이트는 자신이 신뢰할 수 있는 사이트 임을 증명하기 위해 사용자에게 자신의 인증서 전달

![17.png](/Users/kit938639/Documents/study/CS-Study/week3_NETWORK/SSL/img/17.png)

8. 사용자는 브라우저에 내장되어 있는 인증기관 공개키로 사이트 인증서를 복호화하여 검증, 사이트 인증서를 해독하면 사이트 정보와 사이트 공개키를 얻을 수 있다.

![18.png](/Users/kit938639/Documents/study/CS-Study/week3_NETWORK/SSL/img/18.png)

9. 이렇게 얻은 사이트 공개키로, 사용자는 자신의 대칭키를 암호화 한 후 사이트에 전달

![19.png](/Users/kit938639/Documents/study/CS-Study/week3_NETWORK/SSL/img/19.png)

10. 사이트는 자신의 비밀키로 사용자로부터 전달받은 암호문을 해독하여 사용자의 대칭키를 얻음

![20.png](/Users/kit938639/Documents/study/CS-Study/week3_NETWORK/SSL/img/20.png)

11. 이렇게 얻은 대칭키를 활용하여 사용자와 사이트는 암호문을 주고 받을 수 있게 된다. SSL은 사이트 외에 인증기관과 사용자도 협력하기에 안전한 접속 방법이 되며 사용자는 접속하는 사이트가 믿을 수 있는 사이트인지 확인이 가능해진다.

![21.png](/Users/kit938639/Documents/study/CS-Study/week3_NETWORK/SSL/img/21.png)



#### 참고자료

1. [SSL(Secure Sockets Layer) 개념 및 동작 원리 알아보기](https://goodgid.github.io/TLS-SSL/)
2. [[정보보안] SSL(Secure Socket Layer) 이란](https://12bme.tistory.com/80)
3. [[암호] 대칭키 암호 vs 공개키 암호](https://gaeko-security-hack.tistory.com/123)
4. [[10분 테코톡] 다니의 HTTPS](https://www.youtube.com/watch?v=wPdH7lJ8jf0&t=613s)

추천자료

1. [JWT Token 서버 보안](https://github.com/goodGid/NodeSeminar/blob/master/Seminar_8th/SOPT21th_8%EC%B0%A8%EC%84%B8%EB%AF%B8%EB%82%98.pdf)
2. [HTTPS는 어떻게 다를까?](https://parksb.github.io/article/24.html)