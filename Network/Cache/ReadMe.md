<<<<<<< HEAD
## DNS

DNS는 Domain Name System의 약자

도메인 이름과 IP 주소에 대한 정보를 관리하는 시스템

인터넷 사용자는 IP 주소를 몰라도 된다.

의미있는 문자열로 IP 주소를 추상화!

## **DNS 동작 과정**

먼저 브라우저는 브라우저에 있는 캐시에서 혹시 IP가 있는지 찾아본다.

캐시에 IP가 없으면 컴퓨터에 저장되어 있는 호스트 파일과 캐시에서도 한번 IP를 찾아본다.

여기에도 없으면 DNS 서버에게 IP를 달라고 요청하게 된다.

그러면 DNS 서버는 실제 IP를 주게 된다.

## **DNS 계층 구조**

하나의 DNS 서버에서 전 세계 모든 DNS 정보를 관리하는 것이 아니라 DNS는 서버를 여러 서버로 분리하기 위해 도메인을 계층적으로 관리한다. 트래픽과 데이터를 분산함으로써 안정적인 서비스를 제공하고 있다.

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/d6377582-3fae-4d42-8f08-35ea43254327/Untitled.png)

www.boribori.com.

Third-Level Domain Second-Level Domain Top-Level Domain(TLD) Root Domain

이렇게 계층구조에 따라 서버들이 배치되어 있다. 그래서 상위 서버는 하위 서버의 위치를 알고 있다.

그래서 타고 타고 들어가서 원하는 도메인까지 도착을 하게 되고 이러한 서버들을 네임 서버라고 부른다. (NS, Name Server). Root Name Server, com Name server 등등..

이런 서버들은 다양한 기관에서 관리를 해주고 있는데

### 권한이 있는 네임서버 (DNS 정보O)

**Root NS** - 국제인터넷주소관리기구 ICANN

**TLD NS** - 도메인 등록 기관 Registry

**Sub Domain NS** - 도메인 판매 업체 Registry ex)가비아, Route53

### 권한이 없는 네임서버 (DNS 정보O)

**로컬 DNS 서버** - ISP 제공 ex) KT, SKT

클라이언트와 직접적으로 통신을 하고 네임서버와 통신을 하기 위해 존재한다. 

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/94ab502c-e90e-4dda-894c-dc4d2e26c2c8/Untitled.png)

**재귀적 질의(Recursive Query)**

브라우저에서 로컬 DNS 서버로 IP 요청을 하면 캐시확인을 하고 여기에 없으면 힌트파일이라고 해서 Root Name Server에 대해 알고 있는 정보가 있다. 

웹사이트에 접속하려고 하면 내 컴퓨터에 있는 브라우저가 그 사이트를 제공하는 서버에 요청을 해서 데이터들을 받아와야 한다. 

**반복적 질의(Iterative Query)**

Root Name Server에 IP를 요청한다. 

근데 여기에는 IP를 가지고 있지 않기 때문에 하위 도메인 서버를 확인한다. 그래도 없으면 Sub Domain을 확인하게 된다.

그런데 여기에는 실제적으로 주소값을 가지고 있어서 응답을 해주게 되고 최종적으로 브라우저에게 도착하게 됩니다. 

도메인을 구매하려면 네임서버를 직접 구축 할 수도 있겠지만 보통 도메인 판매 업체를 많이 이용하게 됩니다. 

## 도메인 적용해보기

1. 원하는 도메인을 구매한다.

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/ff355ecc-4414-416b-a612-7f6f734ec471/Untitled.png)

1. DNS 레코드를 추가해 도메인 이름에 IP 주소를 매핑한다.

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/c13bc468-e6de-45a1-8cd3-ed0a380f64f9/Untitled.png)

이 값을 DNS 레코드라고 부르는데 이 값이 가비아 네임 서버에 저장이 된다. 그러면 어느 정도 조금만 시간이 지나면  boribori.com으로 접속할 수 있다.

호스트 : 서브도메인 - @null, www, m, dev 등

ex) boribori.com, www.boribori.com, m.boribori.com, dev.boribori.com

타입: 레코드의 타입. 도메인 이름에 어떤 타입의 값이 매핑되는지 

타입이 IP 주소의 값이 될 수도 있지만 다른 값이 될 수도 있다.

그래서 이 값들을 가지고 어떤 값이 매핑되는지 설정해줄 수 있다.

루트 DNS 서버 자체는 전세계에 13군데가 있습니다. 한국에는 없지만 미러 서버가 이를 대신합니다. 

DNS 스푸핑: 사용자 컴퓨터가 DNS에 IP를 물어보는 것을 가로채 해커들이 만들어둔 가짜 사이트 IP를 알려줘서 거기로 접속하게 할 수도 있다.

로컬 DNS서버는 일반적으로 통신사 것으로 세팅되어 있는데 이걸 수정하는 것은 정부에서 막아놓은 사이트에 접속하거나 또는 특정 서비스를 보다 빠르게 이용하기 위해 로컬 DNS서버를 기본값이랑 다른것으로 설정할 수 있습니다.

예를 들어서 윈도우에서 기본 DNS를 구글 DNS 서버 주소인 8,8,8,8로 설정하면 유튜브 등 구글에서 제공하는 서비스를 보다 빠르고 쾌적하게 이용할 수 있습니다. 대신 다른 서비스들은 느려질 수 있으니 주의해야 한다.

**A Record**

도메인을 서버의 IP로 직접 연결하는 것. 이 방식은 IP로의 직통연결이라 접속이 빠르다는 장점이 있다.

도메인 이름을 IP 주소 (IPv4)로 매핑해준다.

**CNAME**

도메인을 별명과 연결하는 것. IP가 유동적으로 변하는 서버의 경우 바뀌는 IP들에 일정하게 연결된 다른 도메인 즉, canonical name을 적는다는 뜻. AWS나 Firebase같은 데서 서비스나 인스턴스를 만들면 주는 주소는 서버 IP가 수시로 바뀌기 때문이다. 그걸 도메인에 연결하는게 CNAME 방식이다. IP가 유동적으로 사용된다는 장점이 있지만 대신 한 군데를 더 거친다는 것이 단점이다.

도메인 이름에 대한 별칭을 매핑해준다.

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/981e5c8f-cb9b-49c5-97c5-b7fb21582058/Untitled.png)

[boribori.com](http://boribori.com) === www.boribori.com

A 타입에서 같은 주소를 입력해도 똑같은 효과를 낼 수 있지만 왜 CNAME 타입을 사용하면 좋은 이유는 만약 서버 주소가 변경될 일이 생기면 값들을 바꿔주어야 한다. 그런데 A, A의 경우에는 두 개를 바꿔야 하고 A, CNAME의 경우에는 하나만 바꾸면 된다. 그래서 CNAME을 이용하면 변경에 유연한 구조로 가져갈 수 있다.

**NS**

도메인 이름에 대한 권한이 있는 네임 서버를 매핑해준다. 권한이 있는 네임 서버는 실제로 DNS 정보를 가지고 있는 네임서버. 가비아에서 구매를 하면 자동으로 NS값이 가비아 네임서버로 등록되어 있다. 

다른 네임서버. 특정 도메인 이름에 대한 권한을 위임을 했을 때 사용을 한다.

이런 식으로 값을 저장했으면 그러면 이렇게 IP를 요청하면 [m.boribori.com](http://m.boribori.com) 정보는 m 네임 서버에 있어서 여기로 가라고 토스를 해주게 된다.

SOA(Start Of Authority): 도메인 영역에 대한 권한을 설정해준다.

- TTL, 네임서버, 관리자 정보 등을 등록한다.

MX(Main eXchange): 도메인 이름에 메일 서버를 매핑해준다.

PTR(Pointer): IP 주소를 도메인 이름에 매핑한다.

- IP 주소에 매핑되는 도메인 이름을 확인할 때 사용한다.

TXT(TeXT): 도메인 이름에 대한 설명과 간단한 텍스트를 정한다.

- Google Search Console과 같은 검색 엔진 등록 과정에서 사용되기도 한다.

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/a8b4f6ee-cc98-4698-a9c0-af66e1a4c3f2/Untitled.png)

먼저 브라우저에서 로컬 DNS 서버로 요청을 보낸다. 그리고 캐시를 확인한 후 루트 NS로 가서 근데 여기에는 A값이 없다(IP주소) 그래서 NS값이 com 네임 서버의 주소를 응답하게 된다.

그럼 여기서도 가비아 네임서버의 주소를 응답해주게 됩니다. 그런데 가비아 네임서버에는 CNAME 타입으로 boribori.com이 매핑되어 있다. 그리고 네임서버에서 내부에서 요청을 해
=======
# 캐시(Cache)

### 캐시

> 자주 사용하는 데이터나 값을 미리 복사해 놓는 임시 장소
> 

### 캐싱

> 캐시에 데이터나 계산된 결과 값의 복사본을 저장해 둠으로써 전체적인 처리속도를 향상시킨다.
> 

아래와 같은 저장공간 계층 구조에서 확인할 수 있듯이, 캐시는 저장 공간이 작고 비용이 비싼 대신 빠른 성능을 제공한다.

![https://velog.velcdn.com/images%2Ftyjk8997%2Fpost%2Feb21d273-c8c6-4aae-b1e2-7eec2c26d752%2Fimage.png](https://velog.velcdn.com/images%2Ftyjk8997%2Fpost%2Feb21d273-c8c6-4aae-b1e2-7eec2c26d752%2Fimage.png)

Cache는 아래와 같은 경우에 사용을 고려하면 좋다.

- 접근 시간에 비해 원래 데이터를 접근하는 시간이 오래 걸리는 경우(서버의 균일한 API 데이터)
- 필요한 값을 얻기 위해 계산하는 과정을 생략하고 싶을 때
- 반복적으로 동일한 결과를 돌려주는 경우(이미지나 썸네일 등)

# 캐시(Cache)의 등장

무어의 법칙(Moore’s law)에 의해 CPU의 처리 속도가 급격히 증가했지만, 메모리 접근 속도는 늘어나지 못했다.

메모리보다는 빠르고 CPU보다는 느린 cache를 메모리와 CPU사이에 위치 시키고, CPU의 데이터 접근성을 줄였다.

결과가 나올 때마다 메모리에 저장하는 것보다, cache에 저장, 한 번에 메모리를 최신화하는 것이 효율적이다.

*CPU -> 캐시-> 메인 메모리*

**캐시는 CPU 와 메인 메모리 사이의 속도 간극을 줄여주는 완충재**이며, 따라서 버퍼라고 불리기도 합니다.

(이 속도 간극 때문에 병목현상이 발생합니다. 추가적으로 버퍼는 완충재라는 의미를 가지고 있음)

캐시에는 보통 가격이 비싸고 속도가 빠르지만 용량 대비 크기가 큰 [SRAM](https://namu.wiki/w/RAM#s-4.1.1) 을 사용하고,

메인 메모리에는 가격이 싸고 속도가 느린 [DRAM](https://namu.wiki/w/RAM#s-4.1.2) 을 사용합니다.

# **파레토(Pareto) 법칙 / 롱테일(Long Tail) 법칙**

![https://velog.velcdn.com/images%2Ftyjk8997%2Fpost%2F32f0eaa6-d40e-4701-8746-d760b00ad50e%2Fimage.png](https://velog.velcdn.com/images%2Ftyjk8997%2Fpost%2F32f0eaa6-d40e-4701-8746-d760b00ad50e%2Fimage.png)

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/63611357-c35d-4d4d-9d3d-de6c3f7e6e3f/Untitled.png)

결국 Cache란 **반복적으로 데이터를 불러오는 경우에**, 지속적으로 DBMS(Database Management System) 혹은 서버에 요청하는 것이 아니라 **Memory에 데이터를 저장하였다가 불러다 쓰는 것**을 의미한다. 

즉, 이것은 Cache가 효율적일 수 있는 이유가 될 수 있습니다. 모든 결과를 캐싱할 필요는 없으며, 실제 서비스에서 자주 호출되는 20%의 리소스를 캐싱함으로써 리소스 사용량을 대폭 줄이고, 성능을 대폭 향상할 수 있게 됩니다.

# 데이터 지역성 (Locality)

캐시 메모리를 관통하는 핵심 개념은 데이터 지역성 (Locality) 입니다. 

캐시 메모리는 이 원리로 작동합니다.

*"미리 쓸만한 데이터를 메인 메모리에서 캐시에 옮겨놓자"*

데이터 지역성은 시간 지역성( Temporal Locality), 공간 지역성 ( Spatial Locality), 순차적 지역성( Sequential Locality) 로 분류됩니다.

### 시간적 지역성

특정 데이터가 한번 접근 되었을 경우, 가까운 미래에 또 한번 데이터에 접근할 가능성이 높은 것을 말한다. 메모리 상의 같은 주소에 여러 차례 쓰기를 수행할 경우 상대적으로 작은 크기의 캐시를 사용해도 효율성을 꾀할 수 있다.

### 공간적 지역성

A[0], A[1] 로 구성되는 배열과 같이 참조된 데이터 근처의 데이터가 잠시 후에 사용될 가능성이 높다는 것입니다. 해당 배열이 위치한 메모리 공간의 내용은 for-in 순회가 끝나기 전까지 계속 쓰일 확률이 높습니다.

### 순차 지역성

비순차적 실행이 아닌 이상 명령어들이 메모리에 저장된 순서대로 실행되는 특성을 고려하여 다음 순서의 데이터가 곧 사용될 가능성이 높다는 것입니다.

# **캐시 쓰기 정책**

### **수정하려고 하는 블록이 캐시 히트인 경우**

![https://k.kakaocdn.net/dn/bf74I3/btrfXzmpwCX/BpuhkYcM9Hz0VMA7CgOTT0/img.png](https://k.kakaocdn.net/dn/bf74I3/btrfXzmpwCX/BpuhkYcM9Hz0VMA7CgOTT0/img.png)

**Write-through 정책** : 캐시의 내용도 수정하고 곧바로 메인 메모리의 내용도 수정합니다. 캐시와 메인 메모리간의 일관성이 보장되지만 버스 병목현상이 존재합니다.

![https://k.kakaocdn.net/dn/2F6Do/btrfVt8iYiK/YKMzoZuK5GCXrh5TevvU2K/img.png](https://k.kakaocdn.net/dn/2F6Do/btrfVt8iYiK/YKMzoZuK5GCXrh5TevvU2K/img.png)

**Write-back (Write-behind) 정책** : 캐시의 내용만 수정하고, 해당 캐시라인이 Replacement 될 때 메인 메모리의 내용도 수정합니다. 메인 메모리에 쓰기 동작이 최소화 되기 때문에 속도 측면에서 이득이 있습니다. 캐시라인에서 내려올 때 메인 메모리의 수정이 필요하다는 것을 알려주기 위해 추가적인 비트 (VI bit) 가 필요합니다.

캐시와 메인 메모리간 일관성 (Cache Coherency) 이슈가 있기 때문에 캐시 일관성 프로토콜이 사용됩니다.

복사본을 이용해서 복사본과 원본이 달라지는 경우가 생길 수 있기 때문에 일관성 유지에 유의해야 한다.

### **수정하려고 하는 블록이 캐시 미스인 경우**

**Write allocate (Fetch on write) 정책 :** 메인 메모리의 블록을 수정해준 다음 캐시로 Fetch 해주는 방식.

해당 데이터가 자주 수정된다면 이 정책을 사용하여 캐시에 올려두는 것이 성능에서 이득일 것입니다.

**No-write allocate (Write-no-allocate 또는 Write around) 정책** : 메인 메모리의 블록만 수정만 하는 정책. 캐시에 따로 올리지 않습니다.
>>>>>>> 4816c2606332f653a08f1d0b1cbf0b0049524ed1
