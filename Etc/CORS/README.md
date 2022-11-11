# 1️⃣ SOP (Same-Origin Policy)

- **'동일 출처 정책'** 이라 부르며, 한 출처에서 불러온 문서, 스크립트 등의 리소스가 다른 출처에서 가져온 리소스와 상호작용하는 것을 **브라우저**가 **제한**하는 보안 방식
- 즉, same-origin 서버로부터만 리소스를 주고받을 수 있음
- 리소스란, JavaScript 파일, css, img. video, audio, @font-face, iframe 등을 의미
- 리소스를 받아올 때, origin이라는 꼬리표를 달고오는데, 같은 origin의 리소스끼리만 상호작용할 수 있도록 제한하는 것이 SOP (document.location.origin으로 origin 확인 가능!)
- 잠재적으로 해로울 수 있는 문서를 분리하여 공격받을 수 있는 경로를 줄이는 역할을 해줌.
- same-origin이란 프로토콜(Protocol), 호스트(Host), 포트(Port)가 같아야 함을 의미함. 포트는 드러나 있을 때만 같아야하고, 드러나있지 않다면 같지 않아도 상관없음.
  - https://computer.com:443 이라고 했을 때, https가 protocol, computer.com이 , :443이 port

```
다음 중 same-origin 인 것들은?

https://www.one.com/path/index.html
https://www.one.com:443/path2/index.html
http://www.one.com/path/index.html
http://blog.one.com/page.html
http://blog.one.com:808/pgae.html
```

## 1. SOP는 언제 적용될까?

- https://www.boribori.com 에서 https://www.user.boribori.com 으로 사용자 정보 json을 요청했을 때, document의 origin인 https://www.boribori.com와 json의 origin인 https://www.user.boribori.com 가 다르기 때문에 SOP 가 적용되어서 JSON 데이터를 읽을 수 없다.
- 웹에서 로컬스토리지, 세션스토리지 같은 웹 데이터베이스는 origin 마다 하나씩 생성되어, same-origin을 갖는 document의 script 에서 접근이 가능하다.

## 2. SOP의 중요성

### if(!SOP)

<img src="https://user-images.githubusercontent.com/89509857/201300708-8d9671e8-ef6d-4407-84dd-a0af4485e19a.png" width="800px">

<br>

### if(SOP)

<img src="https://user-images.githubusercontent.com/89509857/201300787-51d6fa1b-db0c-471a-b871-40ab955901c9.png" width="800px">


# 2️⃣ CORS (Cross Origin Resource Sharing)

- **'교차 출처 리소스 공유'** 로 '동일 출처 정책(SOP)과 반대되는 개념
- 본래는 SOP 정책에 의해서, 같은 출처의 리소스에만 접근할 수 있었는데, CORS 정책에서는 추가 HTTP 헤더를 사용하여서, 다른 도메인에서도 해당 리소스에 접근이 가능하게 할 수 있도록 한다.
- 즉, 다른 도메인의 리소스라도 상호작용을 할 수 있도록 하는 것이 CORS이다.
- CORS 에러가 뜨는 것은, SOP때문에 접근이 막혔을 때, CORS를 허용해달라는 뜻의 에러이다.

## CORS 동작 방식

- header의 origin 에 요청하는 쪽의 protocol, 도메인, port가 담김.
- 이 요청을 받은 서버는 응답의 헤더에 access-control-allow-origin 헤더에 이 데이터를 읽을 수 있는 origin을 따로 넣어준다. (즉, 허용할 origin만을 추가)
- 브라우저는 origin에서 보낸 origin 값과 서버의 응답에 담긴 access-control-allow-origin에 똑같은 출처가 담겨있으면 안전한 요청으로 간주하고 응답 요청을 받음
- 그렇지 않으면 SOP 에 근거하여 브라우저는 CORS 에러를 띄움

- CORS 시나리오 예제
  - 단순 요청(Simple Request)
  - 프리플라이트 요청(Preflight Request)
  - 인증정보 포함 요청(Credentialed Request)

## 1. Preflight Request (프리플라이트 요청)

- 사전 확인 작업으로 예비 요청인 preflight를 먼저 보내는 방식
- options 메서드를 통해 다른 도메인의 리소스에 요청이 가능한지 확인 작업을 한다
- client에서 server로 preflight 요청을 보내고 허용 응답이 온다면 actual 요청을 보낸다. 거부응답이 온다면 요청이 온다면 보내지 않는다.
- 가장 자주 만나는 CORS 방식

**preflight request** 요청 헤더

- origin: 요청 출처 (http://one.com)
- access-control-request-method : 실제 요청의 메서드 (POST..)
- access-control-request-headers : 실제 요청에서 사용할 http 헤더

```
OPTIONS /doc HTTP/1.1
Origin: http://one.com
Access-Control-Request-Method: POST
Access-Control-Request-Headers: Content-Type, X-PINGOTHER
```

**preflight response** 응답 헤더

- access-control-allow-origin : 서버 측 허가 출처 (허가된 origin을 알려주는 것)
- access-control-allow-methods : 서버 측 허가 http 메서드 (POST, GET, OPTIONS ...)
- access-control-allow-headers : 서버 측 허가 http 헤더 (Content-Type...)
- access-control-allow-max-age : Preflight 응답 캐시 기간

```
Access-Control-Allow-Origin: http://one.com
Access-Control-Allow-Methods: POST, GET, OPTIONS
Access-Control-Allow-Headers: Content-Type, X-PINGOTHER
Access-Control-Max-Age: 86400
```

<br>
<img src="https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS/preflight_correct.png" width="700px"/>

## 2. simple request (단순요청)

prefilght 요청 없이 본 요청을 날리면서, 이것이 Cross origin인지 바로 확인하는 절차로 다음 조건을 만족하는 경우에만 가능하다.

```
- GET, POST, HEAD 중 1
- Content-Type
  - application/x-www-form-urlencoded
  - multipart/form-data
  - text/plain
- 헤더는 Accept, Accept-Language, Content-Language, Content-Type 만 허용
```

- 조건이 까다로워 생각보다 자주 마주치지 않는 방식

## 3. Credentialed Request (인증정보 포함 요청)

- 인증된 요청을 사용하는 방법
- 다른 출처 간 통신에서 보안을 좀 더 강화하고 싶을 때 사용한다.
- 기존 브라우저가 제공하는 비동기 요청 API 인 XMLHttpRequest 나 fetch API는 별도의 옵션 없이 브라우저의 쿠키 정보나 인증과 관련된 헤더를 함부로 요청에 담지 않는다.
- 요청에 인증과 관련된 정보를 담을 수 있게 해주는 옵션이 바로 credentials 옵션 이다.

- 옵션
  - same-origin : (기본값) 같은 출처 간 요청에만 인증 정보를 담을 수 있다
  - include : 모든 요청에 인증 정보를 담을 수 있다
  - omit : 모든 요청에 인증 정보를 담지 않는다

# CORS 에러 해결하기

## 1. 서버에서 해결하기

- 서버에서 Access-Control-Allow-Origin 헤더에 유효한 값을 포함하여 응답을 브라우저로 보내면 CORS 에러를 해결할 수 있다.
- 프론트단에서 CORS 에러를 발견했다면 서버에게 Access-Control-Allow-Origin에 유효한 값을 포함해서 달라고 요청해야 한다.
- 와일드 카드 격인 \* 를 사용하여 Access-Control-Allow-Origin에 헤더를 세팅하면 모든 출처에서 오는 요청을 받겠다는 의미이므로 당장은 편하겠지만 보안적으로 심각한 이슈가 발생할 수 있다.
- Access-Control-Allow-Origin:특정주소 와 같이 출처를 명시해주어야 한다.
  <img src="https://junhyunny.github.io/images/react-proxy-1.JPG" width="700px"/>

## 2. 프론트에서 해결하기

- 백엔드 개발자와 협업을 통해 만들어진 api 에서 CORS 에러가 났다면 백엔드 개발자에게 부탁해서 해결할 수 있지만 백엔드 개발자와 소통이 불가능한 상황이라면 프론트 혼자 해결해야 한다.

- **프록시 서버** 를 사용한다.
  프록시 서버는 클라이언트가 프록시 서버 자신을 통해서 다른 네트워크 서비스에 간접적으로 접속할 수 있게 해준다.
- 쉽게 말해 브라우저와 서버 간의 통신을 도와주는 중계서버
- 백엔드 서버에 직접적으로 요청을 하지 않고, 프록시 서버에 요청
- 프록시 서버는 해당 요청을 받아서 그대로 백엔드 서버에 전달하고, 백엔드 서버에서 응답한 내용을 다시 브라우저 쪽으로 반환한다.

<img src="https://junhyunny.github.io/images/react-proxy-2.JPG" width="700px"/>
