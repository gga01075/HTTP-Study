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


<img src="https://user-images.githubusercontent.com/72422020/209826124-62184b40-f713-43ca-a67f-3051a76a35e0.PNG"  width="450" height="100%"/>

그러기 위해서는 <span style="color:blueviolet">**IP(인터넷 프로토콜)**</span>를 이해해야 한다.

메시지를 보내는 입장에서, IP 패킷 정보를 구성해서 IP주소를 따라서 메시지를 보낸다.
<img src="https://user-images.githubusercontent.com/72422020/209898725-c4723257-b5cd-46b2-8753-3468983aaf9c.PNG"  width="450" height="100%"/>

메시지를 보낼때 내 IP주소와 상대IP주소, 그리고 보낼 메시지 등을 같이 적어서 요청하면 인터넷망의 노드들이 최선의 루트를 찾아서 상대방에게 전송한다. 

<img src="https://user-images.githubusercontent.com/72422020/209898811-28f22152-c60d-4d57-ae23-969278727ebf.PNG"  width="450" height="100%"/>


그러면 상대방이 메시지를 받고 답장을 해주는데 답장도 마찬가지로 출발IP주소와 목적지 IP주소, 응답메시지를 담아서 전송한다.
<img src="https://user-images.githubusercontent.com/72422020/209898871-b2317148-54d0-4a2b-8d57-9092e9b3fdda.PNG"  width="450" height="100%"/>

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


<img src="https://user-images.githubusercontent.com/72422020/209900784-a0f2f2fe-0876-4953-8066-f17008158edd.PNG"  width="450" height="100%"/>


이렇게 전송되는데 TCP/IP로 감싸진 패킷의 형태를 보면 아래와 같다. 

<img src="https://user-images.githubusercontent.com/72422020/209900829-0a2d6d74-037d-4d0a-8c97-f08395d4fcc7.PNG"  width="450" height="100%"/>

### TCP 특징

전송 제어 프로토콜(Transmission Control Protocol)

- 연결지향 - TCP 3 way handshake(가상 연결)

  - 일단 상대방이랑 연결이 됐는지 먼저 확인하고 메시지를 보냄. 연결이 안되면 안보냄

  - 물리적으로 연결된 것은 아니고, 논리적으로만 연결됨
  <img src="https://user-images.githubusercontent.com/72422020/209900846-9b4ee3b8-e0f2-434c-8024-b429c677ee81.PNG"  width="450" height="100%"/>

​					

- 데이터 전달 보증

  - 패킷이 중간에 누락되면 어디서 못받았는지 알 수 있다.
<img src="https://user-images.githubusercontent.com/72422020/209902079-0c4ae3bf-5162-4a49-9dbf-d904f2b59979.PNG"  width="450" height="100%"/>

- 순서 보장
<img src="https://user-images.githubusercontent.com/72422020/209902090-d3590cbb-4aed-40be-81e0-3f142b39de08.PNG"  width="450" height="100%"/>

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
    - PORT : 한 IP에서 여러 애플리케이션과 통신할 때 필요
    - 체크섬 : 데이터 검증해주는 데이터 
  - 애플리케이션에서 추가 작업 필요
- 최근 HTTP3가 나오면서 TCP는 전달과정에서 3번의 승인과정이 필요하여 속도가 더뎌지고 용량도 커지면서 UDP가 다시 떠오르고 있다.



### PORT
<img src="https://user-images.githubusercontent.com/72422020/209903113-338c5923-0cba-4673-8c16-4edc8197529f.PNG"  width="450" height="100%"/>

- 0 ~ 65 할당 가능

- 0 ~ 1023 : 잘 알려진 포트, 사용하지 않는 것이 좋음

  - FTP : 20,21
  - TELNET  : 23
  - HTTP : 80
  - HTTPS - 443

  

### DNS

**IP의 문제점**

1. IP는 기억하기 어렵다

2. IP는 변경될 수 있다. (과거IP: 200.200.200.2 / 신규IP : 200.200.200.3)
<img src="https://user-images.githubusercontent.com/72422020/209903138-513b336f-109e-4943-8ed4-53922076e835.PNG"  width="450" height="100%"/>



이러한 IP의 문제점을 해결하기 위해  DNS를 사용한다. 

**DNS (도메인 네임 시스템, Domain Name System)**

- 전화번호부 같은 서버를 제공
- 도메인 명을 IP주소로 변환 
<img src="https://user-images.githubusercontent.com/72422020/209903158-727ff4c3-6bc0-4167-a075-5e0469ca5cfc.PNG"  width="450" height="100%"/>




## 2. URI와 웹 브라우저 요청 흐름

**URI(Uniform Resource Identifier)**

- **U**niform : 리소스 식별하는 통일된 방식
- **R**esource : 자원, URI로 식별할 수 있는 모든 것(제한 없음)
- **I**dentifier : 다른 항목과 구분하는데 필요한 정보



> "URI는 로케이터(locator), 이름(name) 또는 둘 다 추가로 분류될 수 있다." 
<img src="https://user-images.githubusercontent.com/72422020/210140516-b94dfaf4-4fdb-4ea3-9f2a-c40e5efc4a69.PNG"  width="450" height="100%"/>

- URL - Locator: 리소스가 있는 위치를 지정

- URN - Name : 리소스에 이름을 부여

- 위치는 변할 수 있지만 이름은 변하지 않는다. 

- URN 이름만으로 실제 리소스를 찾을 수 있는 방법이 보편화 되지않음

  

### URL 분석

**scheme://[userinfo@]host[:port][/path][?query][#fragment]**

ex )   https://www.google.com/search?q=hello&hl=ko



#### URL scheme

- 주로 프로토콜 사용
- 프로토콜 : 어떤 방식으로 자원에 접근할 것인가 하는 약속 규칙
  - 예) http, https, ftp 등등
- http는 80포트, https는 443 포트를 주로 사용, 포트는 생략 가능
- https는 http에 보안 추가(HTTP Secure)













## 3. HTTP 기본











## 4. HTTP 메서드











## 5. HTTP 메서드 활용













## 6. HTTP 상태코드

































## 7. HTTP 헤더1 - 일반 헤더

































## 8. HTTP 헤더2 - 캐시와 조건부 요청































