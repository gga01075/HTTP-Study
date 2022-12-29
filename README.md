## 목차

[1. 인터넷 네트워크](#1-인터넷-네트워크)

[2. URI와 웹 브라우저 요청 흐름](#2-uri와-웹-브라우저-요청-흐름)

[3. HTTP 기본](#3-http-기본)

[4. HTTP 메서드](#4-http-메서드)

[5. HTTP 메서드 활용](#5-http-메서드-활용)

[6. HTTP 상태코드](#6-http-상태코드)

[7. HTTP 헤더1 - 일반 헤더](#7-http-헤더1---일반-헤더)

[8. HTTP 헤더2 - 캐시와 조건부 요청](#8-http-헤더2---캐시와-조건부-요청)



## 1. 인터넷 네트워크

인터넷에서 컴퓨터 둘은 어떻게 통신할까??

예를 들어, 아래와 그림과 같이 클라이언트와 서버 사이에 인터넷망이 있다. 

클라이언트로부터 보낸 메시지를 인터넷망의 중간 노드들을 거쳐서 서버로 보내야 하는데 도대체 어떤 규칙으로 넘어갈까?

![http-network1](https://user-images.githubusercontent.com/72422020/209826124-62184b40-f713-43ca-a67f-3051a76a35e0.PNG)

그러기 위해서는 <span style="color:blueviolet">**IP(인터넷 프로토콜)**</span>를 이해해야 한다.

메시지를 보내는 입장에서, IP 패킷 정보를 구성해서 IP주소를 따라서 메시지를 보낸다.

![image-20221229120716785](https://user-images.githubusercontent.com/72422020/209898725-c4723257-b5cd-46b2-8753-3468983aaf9c.PNG)


메시지를 보낼때 내 IP주소와 상대IP주소, 그리고 보낼 메시지 등을 같이 적어서 요청하면 인터넷망의 노드들이 최선의 루트를 찾아서 상대방에게 전송한다. 

![image-20221229120931234](https://user-images.githubusercontent.com/72422020/209898811-28f22152-c60d-4d57-ae23-969278727ebf.PNG)

그러면 상대방이 메시지를 받고 답장을 해주는데 답장도 마찬가지로 출발IP주소와 목적지 IP주소, 응답메시지를 담아서 전송한다.

![image-20221229121052030](https://user-images.githubusercontent.com/72422020/209898871-b2317148-54d0-4a2b-8d57-9092e9b3fdda.PNG)


### IP 프로토콜의 한계

1. **비연결성**
   - 패킷을 받을 대상이 없거나 서비스 불능 상태여도 패킷 전송
2. **비신뢰성**
   - 중간에 패킷이 사라지면?
   - 패킷이 순서대로 안오면?
3. **프로그램 구분**
   - 같은 IP를 사용하는 서버에서 통신하는 애플리케이션이 둘 이상이면?



이런 문제를 해결해주는 것이 **TCP,UDP** 이다.

우선 프로토콜 계층을 살펴보면 사용자가 데이터를 다른 곳에 보낼 때, 소켓을 통해 전달되는데 

TCP정보로 래핑되고, 그 다음 IP정보로 래핑되고, 다음 이더넷프레임으로 감싸져서 전송된다.



![image-20221229124122604](https://user-images.githubusercontent.com/72422020/209900784-a0f2f2fe-0876-4953-8066-f17008158edd.PNG)

이렇게 전송되는데 TCP/IP로 감싸진 패킷의 형태를 보면 아래와 같다. 

![image-20221229124412296](https://user-images.githubusercontent.com/72422020/209900829-0a2d6d74-037d-4d0a-8c97-f08395d4fcc7.PNG)

### TCP 특징

전송 제어 프로토콜(Transmission Control Protocol)

- 연결지향 - TCP 3 way handshake(가상 연결)

  - 일단 상대방이랑 연결이 됐는지 먼저 확인하고 메시지를 보냄. 연결이 안되면 안보냄

  - 물리적으로 연결된 것은 아니고, 논리적으로만 연결됨
  
    ![image-20221229123913423](https://user-images.githubusercontent.com/72422020/209900846-9b4ee3b8-e0f2-434c-8024-b429c677ee81.PNG)

​					

- 데이터 전달 보증

  - 패킷이 중간에 누락되면 어디서 못받았는지 알 수 있다.

    ![image-20221229125301846](https://user-images.githubusercontent.com/72422020/209902079-0c4ae3bf-5162-4a49-9dbf-d904f2b59979.PNG) 

- 순서 보장

  ![image-20221229125339434](https://user-images.githubusercontent.com/72422020/209902090-d3590cbb-4aed-40be-81e0-3f142b39de08.PNG)

- 신뢰할 수 있는 프로토콜

- 현재는 대부분 TCP 사용



### UDP 특징

사용자 데이터그램 프로토콜(User Datagram Protocol)

- 하얀 도화지에 비유(기능이 거의 없음)
- 연결지향 - TCP 3 way handshake X
- 데이터 전달 보증 X
- 순서 보장 X
- 데이터 전달 및 순서가 보장되지 않지만, 단순하고 빠름
- 정리
  - IP와 거의 같다 + PORT + 체크섬 정도만 추가
    - PORT : 한 IP에서 여러 애플리케이션과 통신할떄 필요
    - 체크섬 : 데이터 검증해주는 데이터 
  - 애플리케이션에서 추가 작업 필요
- 최근 HTTP3가 나오면서 TCP는 전달과정에서 3번의 승인과정이 필요하여 속도가 더뎌지고 용량도 커지면서 UDP가 다시 떠오르고 있다.













## 2. URI와 웹 브라우저 요청 흐름







## 3. HTTP 기본











## 4. HTTP 메서드











## 5. HTTP 메서드 활용













## 6. HTTP 상태코드

































## 7. HTTP 헤더1 - 일반 헤더

































## 8. HTTP 헤더2 - 캐시와 조건부 요청































