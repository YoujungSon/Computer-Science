# Session Layer 란?
세션 계층(Session Layer, 5계층)에서는 응용 프로그램 간의 통신을 하기 위한 세션을 운영체제를 통해 확립, 유지, 중단하는 작업을 수행한다.


Transport Layer에서 언급한 듯이, 통신을 위해서 양끝단 컴퓨터가 TCP Handshaking을 수행하게 된다.

하지만 매번 모든 전송마다 TCP Handshaking 과정을 수행하면 너무나 비효율 적이다.

그리하여 OS에서 어떠한 TCP 연결에 대한 연결 정보를 가지고 있다.

이 과정을 Session Layer에서 수행한다.

## Persistence Connection(지속적인 연결)

기본적으로 Persistence Connection을 사용하게되면 매 통신마다 3 handshake 작업이 필요가 없어진다.

대표적인 장점으로는 다음과 같다.

1. 네트워크 혼잡 감소 : 연결 정보를 저장함으로 인해서 handshake 과정이 필요없기 때문에 불필요한 네트워크 통신을 줄여줄 수 있다.

2. 네트워크 비용 감소 : handshake 과정을 하지 않기 때문에 네트워크로 인한 비용이 줄어든다.

3. latency 감소 : handshake 과정을 하지 않기 때문에 handshake를 하는 시간을 절약할 수 있다.

단점으로는 다음과 같다.

1. session 관리 자원 필요 : session 정보를 저장할 자원이 필요해진다. 너무나 많은 session을 저장할 경우에는 disk나 memory의 많은 자원을 사용하게 된다.

2. session 관리에 대한 추가적인 정책 및 기능 필요 : 임의의 시간이 지나면 session을 지우거나 특정 session을 저장하고 찾는 알고리즘을 추가적으로 개발하여야 한다.


## Keep-alive

대표적으로 session layer와 관련된 Http 필드는 "connection"이다.

persistence connection은 Http 1.1 version 이후부터 "connection" 필드로 조작된다.

아래와 같이 connection의 옵션 값을 "keep-alive"로 설정해주면 connection은 일정 시간동안 유지가 되며, 불필요한 연결 동작이 필요없어진다.

<img width="756" alt="image" src="https://user-images.githubusercontent.com/79268661/203757463-01826424-f76e-4ca5-9a6d-682f54c928ea.png">


## connection 사용법

```http
HTTP/1.1 OK
Connection : Keep-Alive
Keep-Alive : timeout = 5, max = 1000
```

* Connection은 Persistence Connection을 사용한다는 뜻으로 "Keep-Alive"를 사용하였다.
* timeout은 request의 최대 개수를 정한다.
* max 는 idle 상태로 connection을 유지하는 시간을 말한다.

## Connection 기타 규칙

1. 클라이언트는 모든 요청에 위에서 언급된 헤더를 포함하여 보내야 한다. 1개의 요청에 라도 헤더가 포함되지 않으면 연결이 close된다.

2. 서버또한 모든 요청에 언급된 헤더가 포함되어야 한다. 포함되어있지 않다면, 클라이언트는 서버가 Connection을 끊었다고 생각할 수 있다.









