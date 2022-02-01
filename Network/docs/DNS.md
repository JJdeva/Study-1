# 1. DNS (Domain Name System)

> 도메인을 입력하면 거기에 해당하는 IP를 매핑해준다.

- Internet에서의 ID (기계)
	- IP
		- IPv4
			- 32bit (xxx.xxx.xxx.xxx)
				- 8bit \* 4
				- 10진수
			- 약 40억개
		- IPv6
			- 128bit (xxxx:xxxx:xxxx:xxxx:xxxx:xxxx:xxxx:xxxx)
				- 16bit \* 8
				- 16진수
	- Domain Name
		- 순수하게 인간을 위한 시스템
		- 응용 계층(인터넷 5계층, OSI 7계층)에서만 관여
		- 예
			- www.naver.com

## A. 서비스

- Domain Name을 IP 주소로 바꿔주는 서비스.
- Aliasing 관리
	- 같은 곳(동일 IP)에 대해서 여러개의 주소(도메인)를 사용하게 하는 것.
	
	![](/bin/Network_image/network_4_1.png)

### a. 도메인 네임 서비스가 다른 응용 layer와 다른점

- 전우주적으로 중앙관리함.
- 계층적으로 분산 관리함.

## B. Domain Name Server의 계층

### a. 최상위 (Root Name Server)

- Top level domain name server 관리
- 각 Top level domain에서 하위 도메인을 관리한다.
- Top level Domain server의 IP를 유효기간 (TTL)과 함께 반환한다
	- 유효기간 동안은 동일한 top-level domain server IP를 요청하지 않는다.
		
### b. Top level domain

- `.kr`, `.com`, `.edu`, `.org`, `.net` 등

### c. authorized dns (공인된 dns)

- 각 Domain Name Server(Authorized DNS, Local DNS)는 cache를 가지고 있음.
	- Root NS나 Top Level Domain NS는 cache를 가지고 있지 않음
- 많이 물어보는 것에 대해서는 상위 계층으로 올라가지 않고도 알려줄 수 있도록함.

## C. 도메인 네임 서비스를 받는 방법

> 찾고자하는 도메인이 dns cache에 없다고 가정한다. (Time out으로 사라졌다고 생각할때.)

![](/bin/Network_image/network_4_4.png)

### a. recursive Domain Name resolution

![](/bin/Network_image/network_4_2.png)

- 최근에는 동작하지 않음.
- Root NS가 친절하다면,
	- 이 방식은 Root NS에 과부하가 발생할 수 있다.

#### ㄱ) 실습

![](/bin/Network_image/network_4_10.png)

### b. Iterative

![](/bin/Network_image/network_4_3.png)

- 하나하나 물어보면, 각 단계에 해당하는 domain name server를 알려준다.

#### ㄱ) 실습

![](/bin/Network_image/network_4_6.png)

#### ㄴ) 계층형 DNS별로 요청

![](/bin/Network_image/network_4_7.png)

![](/bin/Network_image/network_4_8.png)

![](/bin/Network_image/network_4_9.png)
