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
