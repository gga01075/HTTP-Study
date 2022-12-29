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

클라리언트로부터 보낸 메시지를 인터넷망의 중간 노드들을 거쳐서 서버로 보내야하는데 도대체 어떤 규칙으로 넘어갈까?

![http-network1](https://user-images.githubusercontent.com/72422020/209826124-62184b40-f713-43ca-a67f-3051a76a35e0.PNG)

그러기 위해서는 <span style="color:blueviolet">**IP(인터넷 프로토콜)**</span>를 이해해야 한다.

메시지를 보내는 입장에서, IP 패킷 정보를 구성해서 IP주소를 따라서 메시지를 보낸다.

![image-20221229120716785](https://user-images.githubusercontent.com/72422020/209898725-c4723257-b5cd-46b2-8753-3468983aaf9c.PNG)


 메시지를 보낼때 내 IP주소와 상대IP주소, 그리고 보낼 메시지 등을 같이 적어서 요청하면 인터넷망의 노드들이 최선의 루트를 찾아서 상대방에게 전송한다. 

![image-20221229120931234](C:\Users\User\AppData\Roaming\Typora\typora-user-images\image-20221229120931234.png)

그러면 상대방이 메시지를 받고 답장을 해주는데 답장도 마찬가지로 출발IP주소와 목적지 IP주소, 응답메시지를 담아서 전송한다.

![image-20221229121052030](C:\Users\User\AppData\Roaming\Typora\typora-user-images\image-20221229121052030.png)





## 2. URI와 웹 브라우저 요청 흐름







## 3. HTTP 기본











## 4. HTTP 메서드











## 5. HTTP 메서드 활용













## 6. HTTP 상태코드

































## 7. HTTP 헤더1 - 일반 헤더

































## 8. HTTP 헤더2 - 캐시와 조건부 요청































