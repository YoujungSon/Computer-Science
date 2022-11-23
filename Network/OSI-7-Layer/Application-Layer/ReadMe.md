# Application Layer 란?

**Application Layer**는 응용 프로그램 계층에서 응용 프로그램 간의 통신을 하기위한 계층이다.

# Protocol

Applicaiton Layer에는 다양한 프로토콜이 존재한다.

1. HTTP (Hyper Text Transfer Protocol)
2. FTP (File Transfer Protocol)
3. SMTP (Simple Mail Transfer Protocol)
4. POP3 (Post Office Protocol)

여러가지 프로토콜이 존재하지만 현재는 **HTTP** 가 가장 대중화된 프로토콜이다.

# HTTP

> HTTP 는 응용 프로그램 간의 통신 프로토콜이다.

인터넷 상에서 데이터를 주고 받기위한 약속이라고 할 수 있다.

HTTP는 많은 환경을 전제조건으로 사용한다.

## HTTP 특징

HTTP 특징은 여러가지가 존재한다.

1. Connectionless & Stateless

통신을 위해서 서버에 연결을 진행하고 서버와 클라이언트는 연결을 끊어버린다.

![image](https://user-images.githubusercontent.com/79268661/202673029-40d8488e-a18c-4234-bfbe-cc75fa43d5ce.png)

위의 그림과 같이 HTTP는 1 Request에는 1Response가 주어지고, 연결은 끊는다.

다음 Request를 받은 서버는 이전의 Request를 기억하고 있지 않다.

위와 같이 진행하는 이유는 서버 입장에서 불특정 다수에게 서비스를 하기 위한 방식이다. 매 Request를 서버가 기억하고 있다면 서버에는 클라이언트 정보를 저장해야한다. 이럴 경우 서버에는 엄청나게 큰 자원이 필요하다.

이 특징 때문에 요청 정보를 저장해야하는 로직에는 **"쿠키, 세션"**을 사용한다.


2. HTTP 는 TCP/IP 위에서 동작한다.

# HTTP 구조

HTTP 구조는 아래와 같은 구조를 가지고 있다.

![image](https://user-images.githubusercontent.com/79268661/202673303-25c8f4c4-4513-4585-b656-d71ef7db2ff5.png)

* start line은 method, target, version이 명시된다.
* HTTP Header는 HTTP에 담겨있는 전반적인 데이터에 대한 설명과 연결에 대한 정보를 나열한다.
* HTTp Body는 실질적으로 주고받는 데이터를 저장한다.


## HTTP Start Line

### Method

HTTP는 행위를 설명하는 **"Method"**가 존재한다.

Method는 여러가지 종류가 있다.

1. GET : READ 행위를 말한다.
2. POST : CREATE, UPDATE 행위를 말한다.
3. PUT : UPDATE 행위를 말한다. (기존에 수정하려는 데이터가 존재하지 않으면 데이터를 생성한다.)
4. PATCH : UPDATE 행위를 말한다. (기존에 수정하려는 데이터가 존재하지 않으면 데이터를 생성하지 않는다.)
5. DELETE : DELETE 행위를 말한다.

그 외 잘 알려지지 않은 메서드가 존재한다.

1. OPTIONAL : 본 요청을 날리기 전에 수행하는 Method이다. cors 및 요청 validation 작업을 진행하는 메서드이다.
2. HEAD : 본 요청에 대한 응답의 Header만 받는 요청이다.
3. TRACE : HTTP 요청이 어떠한 경로로 이동되는지 볼 수 있는 요청이다.

### Target

target은 목표 자원을 표기한다.

### Version

Http는 현재 1.1 버전을 사용하고 있다.

그리고 2.0, 3.0이 설계되고 개발되는 중이다.

## HTTP Header

<img width="500" alt="스크린샷 2022-11-18 오후 6 23 41" src="https://user-images.githubusercontent.com/79268661/202673094-79561130-a67b-4c72-bd37-139093cf839b.png">

Http Header는 다음과 같이 4가지가 있다.

1. General header : 요청과 응답 모두에 적용되지만 바디에서 최종적으로 전송되는 데이터와는 관련이 없는 헤더.

2. Request header : 페치될 리소스나 클라이언트 자체에 대한 자세한 정보를 포함하는 헤더. == 내가 보내는 메세지의 헤더

3. Response header : 위치 또는 서버 자체에 대한 정보(이름, 버전 등)와 같이 응답에 대한 부가적인 정보를 갖는 헤더. == 내가 받은 메세지의 헤더

4. Entity header: 컨텐츠 길이나 MIME 타입과 같이 엔티티 바디에 대한 자세한 정보를 포함하는 헤더.

### HTTP Request Header

> HTTP 요청 헤더에는 수 많은 속성이 존재한다.

1. Host : 요청자의 Host 명, Port 번호를 가지고 있다.

2. User-agent : 요청자의 소프트웨어 정보를 담는다. 요청 브라우저, OS, 기타 환경 정보를 가지고 있다.

> user-agent를 포함하지 않고 요청을 보내었을 때, 응답이 거절되는 서버도 존재한다. 이유는 크롤링, 스크래핑 등과 같이 봇이 웹사이트를 방문하여 요청을 날리는 경우도 있기 때문이다.

3. Accept : 요청자가 원하는 미디어 타입의 우선순위를 표기한다.

> 이하 생략 : 다른 Layer이기 때문에 생략하겠습니다.

### HTTP Response Header

> HTTP 응답 헤더에도 수 많은 속성이 존재한다.

1. Server : 서버 소프트웨어의 정보를 표현한다.

2. content-encoding : 응답하는 내용의 인코딩 포맷을 표현한다.

3. content-type: 응답하는 내용의 타입과 문자 포맷을 표현한다.

4. cache-control : 캐시 관리에 대한 정보를 표현한다.

