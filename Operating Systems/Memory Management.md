# 1. Logical vs. Physical Address

- 주소 바인딩
	- 주소를 결정하는 것

Symbolic Address -> Logical Address --(이 시점이 언제인가?(next page))--> Physical address

- Symbolic Address
	- 프로그래머가 프로그램을 만들때는 메모리 주소(숫자 주소)를 가지고 프로그래밍을 하지는 않는다. 메모리의 특정 위치에 변수값을 저장하지만 메모리 주소를 지정하는 것이 아닌, 변수 이름을 주고 저장한다. 이때, 숫자로된 주소를 사용하지 않고 심볼로 된 주소를 사용한다.

## A. Logical address (=virtual address)

- 프로세스마다 독립적으로 가지는 주소 공간
- 각 프로세스마다 0번지부터 시작
- CPU가 보는 주소는 logical address임
	- 메모리에 올라갈때, 시작 위치는 바뀌지만 그 안에 있는 코드 상의 주소는 logical address로 남아있다.

> CPU가 메모리 몇 번지에 있는 내용을 달라고 요청하면, 그때 주소변환을 해서 물리적인 메모리 위치를 찾은 다음에 그 내용을 읽어서 CPU한테 전달한다.

## B. Physical address

- 메모리에 실제 올라가는 위치
- 아래부분에는 운영체제 커널, 상위 주소에는 여러 프로그램들이 섞여서 올라가있다.
 
# 2. 주소 바인딩 (Address Binding)

## A. Compile time Binding

> 컴파일시 바인딩

- 물리적 메모리 주소(physical address)가 컴파일 시 알려짐
- 시작 위치 변경시 재컴파일
- 컴파일러는 절대 코드(absolute cod) 생성

## B. Load time binding

> 실행이 시작될 때 바인딩

- Loader의 책임하에 물리적 메모리 주소 부여
- 컴파일러가 재배치가능코드(relocatable code)를 생성한 경우 가능

## C. Execution time binding (=Run time binding)

> 프로그램이 시작된 이후에도 실행하다가 중간에 물리적인 메모리 주소가 바뀔 수 있는 방법

- 수행이 시작된 이후에도 프로세스의 메모리 상 위치를 옮길 수 있음
- CPU가 주소를 참조할 때마다 binding을 점검 (address mapping table)
- 하드웨어적인 지원이 필요
	- e.g. base and limit registers, MMU

---

![](/bin/OS_image/os_8_1.png)

> CPU가 바라보는 주소도 Logical Address일 수밖에 없다.

- Symbolic address가 숫자로된 주소로 바뀐다.
	- 그 주소는 프로그램마다 가지는 주소이기 때문에 Logical address가 되는 것이다.
- 실행이 되려면 물리적 메모리에 올라가야한다.
	- 물리적인 메모리의 주소가 결정되는 것을 주소 바인딩이라고 부른다.
		- 물리적인 주소가 결정되는 시점
			- Compile time binding
				- 컴파일 시점에 이미 물리적인 주소가 결정되는 것
				- 프로그램을 물리적인 메모리에 올릴 때에는 이미 결정되어 있는 Logical address로 올려야한다.
				- **비효율적**, **지금의 컴퓨터 시스템에서는 사용하지 않는다.**
			- Load time binding
				- 프로그램이 시작되서 메모리에 올라갈 때, 물리적인 메모리 주소가 결정된다.
				- compile time에는 논리적인 주소까지만 결정됨, 
					- 실행시키게되면 물리적인 주소의 빈 공간에 올린다.
			- Run time binding
				- Load time binding처럼 실행시에 주소가 결정된다.
				- 주소가 실행 도중에 바뀔 수 있다.
				- **요즘의 컴퓨터**
				- CPU가 메모리를 요청할때마다 바인딩을 체크해야함.

### a. Memory-Management Unit (MMU)

> 주소변환을 지원해주는 하드웨어

- logical address를 physical address로 매핑해주는 Hardware device

#### ㄱ) MMU Scheme

- 사용자 프로세스가 CPU에서 수행되며 생성해내는 모든 주소값에 대해 base register (=relocation register)의 값을 더한다.
- 레지스터 두개를 이용한 간단한 MMU Scheme

##### 1) Dynamic Relocation

![](/bin/OS_image/os_8_2.png)

- 왼쪽 아래는 P1의 Logical Address
- CPU가 346번지를 달라고 했다면, process P1의 주소공간에서 0번지부터 346번지 떨어져있는 내용을 CPU가 요청한 상황이다.
- process P1이 physical memory 상에는 14000번지부터 올라가있는 상황임.

> 주소변환은 어떻게 해주는가?

- 프로그램이 물리적인 주소에 올라가있는 시작위치(14000)와 logical address(346)을 더해주면 된다.
	- 14000번지가 논리적인 주소 0번지이기 때문.
- 그래서, MMU Scheme에서는 base register(relocation register)에다가 프로그램(process P1)의 시작위치(14000)를 저장한다.
	- 주소변환을 할때는 논리주소(logical address)에 시작위치(base register)를 더해서 물리주소(physical address)인 14346을 얻게된다.

> 한가지 더 체크하는 방법 (limit register를 사용하는 방법)

- limit register는 프로그램(process P1)의 최대 크기(3000)를 가진다.
- 만약, 이 프로그램이 악성 프로그램이어서 최대 크기가 3000임에도 불구하고 중간에 4000번지에 있는 내용을 달라고 할 수 있기 때문.
	- 그러면 프로그램(process P1) 바깥에 존재하는 다른 프로그램의 메모리 위치를 요청할 수 있게된다.

##### 2) Hardware Support for Address Translation

![](/bin/OS_image/os_8_3.png)

```
- CPU가 메모리 몇 번지의 주소를 달라고 요청하면, 혹시 이 논리 주소가 프로그램의 크기보다 큰 논리 주소를 요청한 것은 아닌지 확인한다.
	- 만약, 크다면 trap이 걸린다.
		- trap이 걸리면, 이 프로그램이 CPU를 잡고 있었지만, 하던일을 잠시 멈추고 CPU제어권이 운영체제한테 넘어가게된다.
		- 이 경우 프로그램을 강제로 abort시킨다.
	- 작다면, base register의 값을 더해서 주소변환을 한 다음 physical address(물리적 메모리) 어딘가에 있는 내용을 읽어다가 CPU한테 전달해준다.

```

운영체제 및 사용자 프로세스 간의 메모리 보호를 위해 사용하는 레지스터

- Relocation register (=base register)
	- 접근할 수 있는 물리적 메모리 주소의 최소값
- Limit register
	- 논리적 주소의 범위

#### ㄴ) user program

- logical address만을 다룬다
- 실제 physical address를 볼 수 없으며 알 필요가 없다.

# 3. Some Terminologies

## A. Dynamic Loading

> Loading : 메모리로 올리는 것

> 원래 dynamic loading
> > 프로그래머가 명시적으로 dynamic loading해서 이루어지는 것
>
> 요즘 dynamic loading
> > 프로그래머가 명시하지 않고 운영체제가 알아서 올려놓고 쫒아내는 것도 dynamic loading이라고 섞어서 쓰기도 한다.

- 프로세스 전체를 메모리에 미리 다 올리는 것이 아니라 해당 루틴이 불려질 떄 메로리에 load하는 것
- memory utilization의 향상
- 가끔씩 사용되는 많은 양의 코드의 경우 유용
	- 예) 오류 처리 루틴
		- 이런 상황이 생기면, 그때 메모리에 올려서 처리한다.
- 운영체제의 특별한 지원없이 프로그램 자체에서 구현 가능
	- **OS는 라이브러리를 통해 지원 가능**

## B. Dynamic Linking

- Linking을 실행 시간(execution time)까지 미루는 기법

> 프로그램을 작성한 다음에 컴파일하고 링크해서 실행파일을 만든다.
> 링킹이라는 것은 여러군데 존재하는 컴파일된 파일들을 묶어서 하나의 실해파일을 만드는 과정
> > 소스 코드 파일을 따로 코딩을 해서 링킹하기도하고 또는 내가 작성하지 않은 코드(라이브러리)를 불러서 사용할 때 링킹을 통해 실행파일이 만들어진다. -> 내 코드 안에 라이브러리 코드가 포함이 되는 개념.

- Static linking
	- 라이브러리가 프로그램의 실행 파일 코드에 포함됨
	- 실행 파일의 크기가 커짐
	- 동일한 라이브러리를 각각의 프로세스가 메모리에 올리므로 메모리 낭비
		- e.g. printf 함수의 라이브러리 코드
- Dynamic linking
	- 라이브러리가 실행시 연결(link)됨
		- 내 코드 안에 실행파일을 만들 때 라이브러리를 포함시키는 것이 아닌, 실행 파일에는 라이브러리가 별도의 파일로 존재하고 그 라이브러리가 어디에 있는지 찾을 수 있는 작은 코드(stub)만 내 실행 파일에 두고 라이브러리 자체는 포함을 시키지 않는다.
	- 라이브러리 호출 부분에 라이브러리 루틴의 위치를 찾기 위한 stub이라는 작은 코드를 둠
	- 라이브러리가 이미 메모리에 있으면 그 루틴의 주소로 가고 없으면 디스크에서 읽어옴
	- 운영체제의 도움이 필요
	- dynamic linking을 해주는 라이브러리를 shared library(리눅스에서는 shared object, 윈도우에서는 DLL)라고 부른다.

## C. Overlays

- 메모리에 프로세스의 부분 중 실제 필요한 정보만을 올림
- 프로세스의 크기가 메모리보다 클 때 유용
- **운영체제의 지원없이 사용자에 의해 구현**
- 작은 공간의 메모리를 사용하던 초창기 시스템에서 수작업으로 프로그래머가 구현
	- **Manual Overlay**
	- 프로그래밍이 매우 복잡

## D. Swapping

### a. Swapping

> 프로세스를 메모리에서 하드디스크로 통째로 쫒아내는 것

- 프로세스를 일시적으로 메모리에서 **backing store**로 쫓아내는 것

> backing store

- 하드디스크같이 메모리에서 쫒겨난 것을 저장하는 곳을 backing store(=swap area)라고 한다. 

### b. Backing store (=swap area)

- 디스크
	- 많은 사용자의 프로세스 이미지를 담을 만큼 충분히 빠르고 큰 저장 공간

### c. Swap in / Swap out

> 메모리에 너무 많은 프로그램이 올라와있으면, 시스템이 굉장히 비효율적이 되기 때문에 중기 스케줄러가 일부 프로그램을 골라서 통째로 메모리에서 디스크로 쫓아내는 일을 한다.

- 일반적으로 중기 스케줄러(swapper)에 의해 swap out 시킬 프로세스 선정
- priority-based CPU scheduling algorithm
	- priority가 낮은 프로세스를 swapped out 시킴
	- priority가 높은 프로세스를 메모리에 올려 놓음
- Compile time 혹은 load time binding에서는 원래 메모리 위치로 swap in해야 함
- Execution time binding에서는 추후 빈 메모리 영역 아무 곳에나 올릴 수 있음
- swap time은 대부분 transfer time (swap되는 데이터의 양에 비례하는 시간, 전송시간)임

![](/bin/OS_image/os_8_4.png)

- Swap Out
	- 메모리에서 통째로 쫒겨나서 backing store(하드디스크)로 내려가는 것
- Swap in
	- backing store로 쫒겨났던 것이 메모리로 다시 올라오는 것

> Swapping이 효율적으로 진행되려면?

- Compile time binding이나 Load time binding일때는 이전에 할당받은 주소로 다시 올라가야하기 때문에 swapping의 효과를 발휘하기 어렵다.
- swapping이 효율적으로 동작하려면 run time binding이 지원되어야 한다.
	- 300번지부터 올라와있던 프로그램이 swap out을 당해서 쫓겨났으면, 나중에 다시 메모리에 올라올 때 다른 위치(700번지)로도 비어있다면 올라갈 수 있게 해준다.

![](/bin/OS_image/os_8_5.png)

# 4. Allocation of Physical Memory

- 메모리는 일반적으로 두 영역으로 나뉘어 사용
	- OS 상주 영역
		- interrupt vector와 함께 낮은 주소 영역 사용
	- 사용자 프로세스 영역
		- 높은 주소 영역 사용

![](/bin/OS_image/os_8_6.png)

- 사용자 프로세스 영역의 할당 방법
	- Contiguous allocation (연속 할당)
		- 각각의 프로세스가 메모리의 연속적인 공간에 적재되도록 하는 것
		- 프로그램이 메모리에 올라갈때, 통째로 올라가는 방법
			- Fixed partition allocation
			- Variable partition allocation
	- Noncontiguous allocation (불연속 할당)
		- 하나의 프로세스가 메모리의 여러 영역에 분산되어 올라갈 수 있음
		- 프로그램을 구성하는 주소 공간을 잘게 쪼개서 나눠서 올라가있는 방법
			- Paging
			- Segmentation
			- Paged Segmentation

## A. Contiguous Allocation (연속 할당)

![](/bin/OS_image/os_8_7.png)

### a. Fixed partition (고정분할) 방식

> 프로그램이 들어갈 사용자 메모리 공간을 미리 partition으로 나눠놓는 것

> 위에서는 사용자 프로그램이 들어갈 물리적인 메모리를 분할 4개로 미리 나눠놓음. (분할의 크기는 균일하거나 가변적으로 만들 수 있다.)

- 물리적 메모리를 몇 개의 영구적 분할(partition)로 나눔
- 분할의 크기가 모두 동일한 방식과 서로 다른 방식이 존재
- 분할당 하나의 프로그램 적재
- 융통성이 없음
	- 동시에 메모리에 load되는 프로그램의 수가 고정됨
	- 최대 수행 가능 프로그램 크기 제한
- Internal fragmentation 발생 (external fragmentation도 발생)
	- 외부 조각(외부 단편화, External fragmentation)
		- 프로그램의 크기보다 분할의 크기가 작아서 발생
	- 내부 조각(내부 단편화, Internal fragmentation)
		- 분할의 크기보다 프로그램의 크기가 작아서 발생

### b.  Variable partition (가변분할) 방식

> 사용자 프로그램이 들어갈 영역을 미리 나눠놓지 않는 것

- 프로그램의 크기를 고려해서 할당
- 분할의 크기, 개수가 동적으로 변함
- 기술적 관리 기법 필요
- External fragmentation 발생

#### 1) External fragmentation (외부 조각)

- **프로그램 크기보다 분할의 크기가 작은 경우**
- 프로그램의 크기가 분할의 크기보다 클때
- 아무 프로그램에도 배정되지 않은 빈 곳인데도 프로그램이 올라갈 수 없는 작은 분할

#### 2) Internal fragmentation (내부 조각)

- **프로그램 크기보다 분할의 크기가 큰 경우**
- 프로그램의 크기가 분할의 크기보다 작을때
- 하나의 분할 내부에서 발생하는 사용되지 않는 메모리 조각
- 특정 프로그램에 배정되었지만 사용되지 않는 공간

### c. Hole

- 가용 메모리 공간
- 다양한 크기의 hole들이 메모리 여러 곳에 흩어져 있음
- 프로세스가 도착하면 수용가능한 hole을 할당
- 운영체제는 다음의 정보를 유지
	1. 할당 공간
	2. 가용 공간 (hole)

![](/bin/OS_image/os_8_8.png)

#### 1) Dynamic Storage-Allocation Problem (Dynamic Memory-Allocation Problem)

> 가변 분할 방식에서 size n인 요청을 만족하는 가장 적절한 hole을 찾는 문제

> First-fit과 best-fit이 worst-fit보다 속도와 공간 이용률 측면에서 효과적인 것으로 알려짐 (실험적인 결과)

##### ㄱ) First-fit

- Size가 n 이상인 것 중 최초로 찾아지는 hole에 할당

##### ㄴ) Best-fit

- Size가 n 이상인 가장 작은 hole을 찾아서 할당
- Hole들의 리스트가 크기순으로 정렬되지 않은 경우 모든 hole의 리스트를 탐색해야함.
- 많은 수의 아주 작은 hole들이 생성됨

##### ㄷ) Worst-fit

- 가장 큰 hole에 할당
- 모든 리스트를 탐색해야 함
- 상대적으로 아주 큰 hole들이 생성됨

#### 2) compaction (압축)

> 실행중인 메모리를 한군데로 모는 방법이기 때문에 쉽지 않은 작업임

> 한쪽으로 모두 모는 방법
> 최소 비용만큼만 한쪽으로 모는 방법

- external fragmentation 문제를 해결하는 한 가지 방법
- 사용 중인 메모리 영역을 한군데로 몰고 hole들을 다른 한 곳으로 몰아 큰 block을 만드는 것
- 매우 비용이 많이 드는 방법임
- 최소한의 메모리 이동으로 compaction하는 방법 (매우 복잡한 문제)
- Compaction은 프로세스의 주소가 실행 시간에 동적으로 재배치 가능한 경우에만 수행될 수 있다. (run time binding)

## B. Noncontiguous Allocation (불연속 할당)

> 메모리에 통째로 올리지 않고, 분할하여 올린다.

### a. Paging

> 하나의 프로그램을 구성하는 주소 공간(virtual memory)을 같은 크기의 page로 자르는 것. page단위로 물리적인 공간(physical memory)에 올려놓거나, backing store에 내려놓는다.
> 
> 물리적 공간(physical memory)도 page 하나가 들어갈 수 있는 크기로 미리 잘라놓는다. 이것을 page **frame**이라고 부른다.
> 
> page frame 하나하나에는 page들이 올라갈 수 있음.

> 프로그램을 구성하는 address space를 같은 크기의 페이지 단위로 쪼갠 것

> 프로그램을 동일 크기의 페이지로 자르다보면, 코드 영역과 데이터 영역이 같이 들어갈 수도 있다. -> (read-only로 해야할지, read/write 권한을 다 줘야할지.. 어려움)

```
장점

- hole들의 크기가 균일하지 않아서 발생하는 문제나, compation(hole들을 한군데로 몰아놓는)이 발생하지 않는다.

단점

- 주소 변환이 복잡해진다.
	- MMU에 의해 주소변할을 할때, 단지 시작주소만 더해서 주소변환을 하는게 아니라. 잘려진 각각의 page가 물리적인 memory에 어디에 올라가 있는지를 확인해야한다. 
	- 주소 변환을 page별로 해야하기 때문에 address binding이 더 복잡해진다.

```

- Process의 virtual memory를 동일한 사이즈의 page 단위로 나눔
- Virtual memory의 내용이 page 단위로 noncontiguous하게 저장됨
- 일부는 backing storage에, 일부는 physical memory에 저장

#### 1) Basic Method

- Physical memory를 동일한 크기의 frame으로 나눔
- logical memory를 동일 크기의 page로 나눔 (frame과 같은 크기)
- 모든 가용 frame들을 관리
- page table을 사용하여 logical address를 physical address로 변환
- External fragmentation 발생 안함
- Internal fragmentation 발생 가능
	- 프로그램을 page단위로 나누다보면, page단위보다 작은 조각이 생길 수 있다. 이것으로인해 내부 단편화(프로그램의 크기가 분할의 크기보다 작은 경우)가 발생할 수 있다.

#### 2) Paging Example

![](/bin/OS_image/os_8_9.png)

프로그램을 구성하는 논리적인 메모리를 동일한 크기의 page로 잘라서 각각의 page별로 물리적인 메모리의 적당한 위치(어디든지 비어있는 위치가 있으면)에 올라갈 수 있게 해준다.

페이징 기법에서 주소변환을 위해서는 page table이 사용된다.

- Page table
	- 각각의 논리적인 page들이 물리적인 메모리 어디에 올라가 있는가 
		- 논리적인 page도 page 번호를 0번 ~ 3번으로 매겨놓음.
		- page frame도 0번부터 동일한 크기로 잘라서 번호를 매김
			- 물리적인 메모리에서 페이지가 들어갈 수 있는 공간을 page frame이라고 함.
	- 그럼, **논리적인 page 0번이 물리적인 page frame 어디에 올라가 있는가**와 같이 각각의 논리적인 page별로 주소변환을 하기 위한 table이 page table이다.
	- page table은 논리적인 메모리의 page 개수만큼 page table entry가 존재하게 된다.
		- 각각의 page table entry에는 그 page가 몇번 물리적인 frame에 올라가있는지를 나타낸다.
	- 인덱스를 이용하여 곧바로 접근할 수 있다.

#### 3) Address Translation Architecture

![](/bin/OS_image/os_8_10.png)

- logical address
	- p (앞부분)
		- page 번호
	- d (뒷부분)
		- page내에서 얼마나 떨어져있는지를 나타내는 offset
- page table
	- 논리적인 page 번호에 해당하는 entry를 page table의 위에서 p번째 찾아가면 f라는 page frame 번호가 나온다.
- physical address
	- f
		- 위 과정을 통해, 논리적인 주소를 물리적인 주소로 바꾼다.
- logical address -> physical address로 바꾸는 과정
	- 논리적인 page 번호를 물리적인 frame 번호(위에서 몇번째 frame인지를 나타내는)로 바꿔준다.
	- 위 과정이 주소변환.
	- page 내의 offset부분은 주소변환에서 영향이 없다.

#### 4) Implementation of Page Table

- Page table은 **main memory**에 상주
- **Page-table base register (PTBR)** 가 **page table**을 가리킴
	- 메모리상의 page table이 어디 있는지, 시작 위치를 이 레지스터가 가지고 있는다.
	- MMU의 base register
- **Page-table length register (PTLR)** 가 테이블 크기를 보관
	- page table의 길이를 이 레지스터가 가지고 있는다.
	- MMU의 limit register
- 모든 메모리 접근 연산에는 **2번의 memory access** 필요
	- **page table** 접근 1번, 실제 **data/instruction** 접근 1번
	- 메모리에 접근하기 위해서는 주소변환을 하고 접근해야하는데, 주소변환을 하려면 page table에 접근해야하고, page table이 메모리에 존재하기 때문에 2번의 메모리 접근이 필요하다.
- 속도 향상을 위해 **associative register** 혹은 **translation look-aside buffer (TLB)** 라 불리는 고속의 lookup hardware cache 사용
	- 메모리 접근을 위해 메모리에 2번 접근하는 것은 비효율적이기 때문에, 별도의 하드웨어를 사용한다.
	- 메인 메모리와 CPU 사이에 존재하는 주소변환을 해주는 계층
- page table은 프로세스마다 존재한다.
	- 프로세스별로 논리적인 주소 체계가 다르기 때문에 주소 변환을 위한 페이지 테이블은 프로세스마다 존재해야한다.
	- TLB도 프로세스마다 다른 정보를 갖는다.

##### ㄱ) Paging Hardware with TLB

###### cache

메인메모리에서 빈번하게 사용되는 데이터를 캐시메모리에 저장하여 CPU로부터 더 빨리 접근할 수 있게 해준다.

---

![](/bin/OS_image/os_8_11.png)

> 메모리 주소변환을 위한 별도의 캐쉬

> 주소변환을 전담해주는 일종의 캐쉬메모리

> 병렬적으로 동시에 접근

- page table에서 빈번히 참조되는 일부 entry를 caching하고 있다.
- 메인메모리보다 접근속도가 빠른 하드웨어로 구성되어 있다.

CPU가 논리적인 주소를 주면, 메인메모리상에 있는 page table에 접근하기 전에 TLB를 먼저 검색한다. TLB에 저장되어 있는 정보를 이용해서 주소변환이 가능한지를 체크한다. 

logical address의 p에 해당하는 entry가 TLB에 저장이 되어 있다면, TLB를 통해서 주소변환이 이루어진다. 그럼, 바로 주소변환을 해서 물리적인 메모리에 접근하니까. 메모리를 1번만 접근하면 된다. TLB에 없는 경우에는 page table을 통해서 일반적은 주소변환을 진행하기 때문에 2번의 메모리 접근이 필요하다. 

TLB는 page table의 정보 전체를 담고 있는게 아니라 일부만(빈번히 참조되는 entry 몇개만) 담고 있다.

논리적인 page 번호 p와 p에 대한 주소변환된 frame번호 f를 가지고 있어야 한다. (이게 일반적인 page table과의 차이점이다.)

주소변환을 위해서 TLB의 특정 항목을 검색하는 것이 아니라, 전체를 다 검색해야한다. 그래서 전체를 검색하는 시간이 오래 걸린다.

###### Associative Register

- Associative registers (TLB)
	- parallel search(병렬 탐색)가 가능
	- TLB에는 page table 중 일부만 존재
- Address translation
	- page table 중 일부가 associative register에 보관되어 있음
	- 만약 해당 page \#(번호)가 associative register에 있는 경우 곧바로 frame \#를 얻음
	- 그렇지 않은 경우 main memory에 있는 page table로부터 frame \#를 얻음
	- TLB는 context switch 때 flush (remove old entries)
		- 프로세스마다 주소변환 정보가 다르기 때문.

###### Effective Access Time

- Associative register lookup time = $\epsilon$
	- TLB에 접근하는 시간
	- 메인메모리에 접근하는 시간인 `1`보다 훨씬 작다.
- memory cycle time = 1
- Hit ratio = $\alpha$
	- associative register에서 찾아지는 비율
- Effective Access Time (EAT)

![](/bin/OS_image/os_8_12.png)

- hit
	- TLB에 접근하는 시간 $\epsilon$만큼과 주소변환이 끝났기 때문에 실제 데이터를 접근하는 메모리 시간 $1$이 걸린다. 여기에 $\alpha$만큼 곱한다.
	- TLB에 없는 경우($1-\alpha$)의 비율만큼 TLB에 접근하는 시간 $\epsilon$과 page table에 접근하는 시간(메모리 접근 시간 $1$, 주소변환을 한 다음 실제 메모리에 접근하는 시간 $1$) 총 $2$이 걸린다.

#### 5) Two-Level Page Table

![](/bin/OS_image/os_8_13.png)

> 시간은 더 걸리지만, 페이지 테이블을 위한 공간을 줄이기 때문에 사용한다.
> 사실, 시간도 2번 걸쳐서 주소변환을 해야하고, 바깥쪽 페이지 테이블이 하나 더 만들어져서 1단계 페이지 테이블을 사용하는 것보다 시간적, 공간적으로 손해이다.
> > 사용되지 않는 주소 공간(프로그램을 구성하는 공간 중에서 상당 부분은 사용되지 않는다.)에 대한 outer page table의 엔트리 값은 NULL (대응하는 inner page table이 없음)

> 페이지 테이블을 만들때는 사용되지 않는 공간도 entry를 만들어야 한다.
> > 페이지 테이블은 k번째 페이지를 주소변환하려면 위에서 k번째로 가서 주소변환을 해야한다. 그래서 사용되지 않는 페이지가 있더라도, 맥시멈 logical memory의 크기만큼 page table의 entry 수가 만들어져야한다.
> > > 2단계 페이지 테이블을 사용하면 그걸 해소할 수 있다.
> > > 바깥쪽 페이지 테이블은 전체 logical memory의 크기만큼 만들어지지만, 실제로 사용이 되지 않는 주소에 대해서 안쪽 페이지 테이블은 만들어지지 않고 포인터만 NULL 상태로 되어있는다.
> > > 실제로 사용되는 메모리 영역에 대해서만 안쪽 테이블이 만들어져서 주소를 가리킨다. 중간에 사용되지 않는 테이블은 NULL이 되어 있고, 안쪽 페이지 테이블이 만들어지지 않는다.

- 현대의 컴퓨터는 address space가 매우 큰 프로그램 지원
	- 32 bit address 사용시 $2^32$ (4G)의 주소 공간 ($2^10$ = K, $2^20$ = M, $2^30$ = G)
		- page size가 4K시 1M개의 page table entry 필요
			- 1M = 4G / 4K
		- 각 page entry가 4B시 프로세스당 4M의 page table 필요
			- 4M = 4B \* 1M
		- 그러나, 대부분의 프로그램은 4G의 주소 공간 중 지극히 일부분만 사용하므로 page table 공간이 심하게 낭비됨

> page table 자체를 page로 구성

> offset의 크기가 page의 크기에 따라 결정됨.

###### Two-Level Paging Example

- logical address (on 32-bit machine with 4K page size)의 구성
	- 20bit의 page number
	- 12bit의 page offset
		- 4K -> $2^12$
- page table 자체가 page로 구성되기 때문에 page number는 다음과 같이 나뉜다. (각 page table entry가 4B)
	- 10-bit의 page number
	- 10-bit의 page offset
		- 안쪽 page table이 페이지화되서 메모리에 들어간다.
			- 즉, 안쪽 페이지 테이블의 크기는 4Kbyte다.
			- 각 entry가 4byte이기 때문에, 하나의 entry수가 1K개이다.
			- 1K개의 entry 위치를 구분하기 위해서 P2는 10bit가 되어야한다. (1K = $2^10$)
- 따라서, logical address는 다음과 같다.

![](/bin/OS_image/os_8_14.png)

- P1은 outer page table의 index이고
- P2는 outer page table의 page에서의 변위(displacement)

###### Address-Translation Scheme

2단계 페이징에서의 Address-translation scheme

![](/bin/OS_image/os_8_15.png)

> outer-page table은 32bit일때 (10bit), 64bit일때 (42bit)
> page of page table에서는 1K개를 구분해야한다. (10bit)
> memory에서는 서로 다른 4K개를 구분해야한다. (12bit)

P1을 통해 outer page table의 index를 찾을 페이지 번호를 가지고 outer-page table의 위에서부터 P1번째 entry에 가서 주소변환정보를 얻는다.

위에서 얻은 결과는 page of page table(안쪽 페이지 테이블) 중에 어떤 페이지 테이블인지를 지정해줌. 안쪽 페이지 테이블은 여러개가 있다. 각각의 outer-page table 하나당 안쪽 페이지 테이블이 하나씩 있다.

즉, 바깥쪽 테이블이 가리키는 것은 안쪽 페이지 테이블이 어떤 것인지를 가리킨다.

그것이 정해지면, 안쪽 페이지 테이블의 번호(P2)를 이용해서 위에서부터 P2번째 entry를 가면 물리적인 페이지 프레임 번호를 얻게 된다.

최종적으로 페이지 프레임 번호를 logical address에 덮어씌우면 그 페이지 프레임이 나오고, 거기서 d번째 떨어진 위치에서 원하는 정보를 찾을 수 있다.

> 안쪽 페이지 테이블의 크기가 페이지 크기와 같다.

> 안쪽 페이지 테이블은 테이블 자체가 페이지화되서 페이지 어딘가에 들어가있게 된다. (page의 크기가 4K라고 하였기에, 안쪽 페이지 테이블의 크기는 4K다.)

#### 6) Multilevel Paging and Performance

- Address space가 더 커지면 다단계 페이지 테이블 필요
- 각 단계의 페이지 테이블이 메모리에 존재하므로 logical address의 physical address 변환에 더 많은 메모리 접근 필요
- TLB를 통해 메모리 접근 시간을 줄일 수 있음
- 4단계 페이지 테이블을 사용하는 경우
	- 메모리 접근 시간이 100ns, TLB 접근 시간이 20ns이고
	- TLB hit ratio가 98%인 경우
		- effective memory access time = 0.98 \* 120 + 0.02 \* 520 = 128 nanoseconds.
			- TLB에 hit하면,
				- 20ns만에 주소변환
				- 100ns동안 메모리 접근
			- TLB에서 주소변환이 되지 않는 경우, 
				- TLB에 없는 것을 확인하는 과정에서 20ns
				- 400ns 동안 주소변환
				- 100ns동안 데이터에 접근
		- 결과적으로 주소변환을 위해 28ns만 소요 (메모리 접근에 100ns를 사용하기 때문)

> 대부분 TLB를 통해 주소변환을 한다고 가정하면, 다단계 페이지 테이블이 크게 오버헤드가 되지 않는다.

페이지 테이블이 메모리에 있기 때문에 주소변환을 위해 메모리에 4번 접근해야함. 주소변환 후에는 실제 원하는 데이터에 접근하기 위해서 또 메모리 접근을 해야한다. 4단계 페이지이면 메모리에 한번 접근하기 위해 메모리에 4번 접근(4번은 주소변환, 1번은 실제 데이터 접근)해야한다.

#### 7) Valid (v) / Invalid (i) Bit in a Page Table

![](/bin/OS_image/os_8_16.png)

(왼쪽, Logical memory / 오른쪽, Physical memory)

- valid - invalid bit
	- 페이지에 주소변환 정보만 들어있는 것이 아닌, 부가적인 비트가 엔트리마다 같이 저장되어 있다.
	- 페이지 테이블은 위에서부터 몇번째인지를 지정하기 때문에 사용하지 않는 페이지도 엔트리로 할당해주어야한다. (테이블이라는 자료구조 특성상 인덱스로 접근해야하기 때문.)
		- 프로그램의 주소공간이 가질 수 있는 맥시멈 사이즈만큼 페이지 테이블 엔트리는 생겨야한다.
	- 사용하지 않는 페이지의 엔트리는 invalid bit로 표시한다.
		- 위 그림에서는 frame number가 0이지만, 이게 0번을 말하는 것인지, 사용하지 않는다는 것인지는 모르기 때문.

##### ㄱ) Memory Protection

Page table의 각 entry마다 아래의 bit를 둔다.

- Protection bit
	- page에 대한 접근 권한 (read/write/read-only)
		- 어떤 연산에 대한 접근 권한이 있는지를 나타냄
			- 코드같은 경우는 중간에 데이터가 바뀌면 안되기 때문에 read-only로 셋팅한다.
			- 데이터/스택 영역은 데이터를 쓰거나 업데이트할 수 있기 때문에 read/write 권한을 준다.
	- 페이지 테이블이 프로세스마다 독립적으로 존재하기 때문에 어떤 프로세스의 페이지 테이블에 접근했으면, 본인의 페이지에 대해서만 주소변환을 할 수 있다. 그래서 다른 프로세스가 내 페이지에 접근하는 것은 애초에 불가능함.
		- protection bit은 다른 프로세스가 이 페이지를 못보게하는 용도는 아님
			- 자기 자신이 자기 페이지만 접근하는 것이기 때문
- Valid-invalid bit
	- valid
		- 이 페이지 테이블 엔트리에 가면 0번 페이지가 2번 frame에, 5번 page가 9번 frame에 올라가있다는 의미.
		- 해당 주소의 frame에 그 프로세스를 구성하는 유효한 내용이 있음을 뜻함 (접근 허용)
	- invalid
		- 해당 주소의 frame에 유효한 내용이 없음을 뜻함 (접근 불허)
			- 프로세스가 그 주소 부분을 사용하지 않는 경우
			- 해당 페이지가 메모리에 올라와 있지 않고 swap area에 있는 경우

#### 8) Inverted Page Table

![](/bin/OS_image/os_8_17.png)

> physical address를 보고 logical address로 바꿀 수 있는 테이블

> page table에 대한 공간을 줄이려는 목적
> > 대신, 시간적인 오버헤드가 발생한다. (주소 변환을 위해 페이지 테이블을 전부 탐색해야하기 때문)

> 논리적인 page 번호(p)와 page id(어떤 프로세스의 p번째 페이지인지를 확인하기 위해) 를 저장해야 함. (논리적인 page번호는 프로세스마다 별도로 존재하기 때문)

- Page table이 매우 큰 경우
	- 모든 process별로 그 logical address에 대응하는 모든 page에 대해 page table entry가 존재
	- 대응하는 page가 메모리에 있든 아니든 간에 page table에는 entry로 존재

##### ㄱ) Inverted page table

> 각 프로세스마다 대응되는 page table이 있는 것이 아니라, 시스템마다 하나씩 존재

- Page frame 하나당 page table에 하나의 entry를 둔 것 (System-wide)
- 각 page table entry는 각각의 물리적 메모리의 page frame이 담고 있는 내용 표시 (process-id, process의 logical address)
	- page table의 entry가 physical memory의 페이지 frame 개수만큼 존재.
		- 첫 번째 entry는 물리적 메모리의 첫 번째 frame을 나타내는 entry

#####

-  단점
	- logical address인 P가 물리적인 메모리 어디에 올라가있는지를 찾으려면 page table의 entry를 다 찾아봐야 한다.
		- 테이블 전체를 탐색해야 함
	- 해당하는 p가 나오면, 위에서 부터 몇 번째 entry인지(f번째) 확인하고 페이지 p는  physical memory의  f번째 frame에 올라가있다는 것을 알 수 있음.
	- 테이블의 장점은 인덱스를 이용해서 위에서부터 p번째 떨어진 곳을 바로 검색하는 것인데, inverted page table은 그런 장점이 없다.
- 조치
	- associative register 사용 (expensive)

#### 9) Shared Page

> 다른 프로세스들과 공유할 수 있는 페이지

> 여러 프로세스가 공유할 수 있는 코드 부분을 같은 물리적인 메모리 프레임으로 매핑해주는 기법

> IPC에서 shared memory와는 다르다 !.
> > 프로세스간의 통신을 목적으로 여러 프로세스의 주소 공간에 같이 매핑하는 것은 같다.
> > 다만, 여기서는 read/write가 가능하다.

> Shared Page
> > 여기서는 read-only (코드를 공유한다)
> > 프로세스가 내용을 접근하다가 다른 프로세스한테 영향을 미치지 않게함.

- Shared code
	- Re-entrant Code (=Pure code) (=재진입 가능한 코드)
		1. read-only로 하여 프로세스 간에 하나의 code만 메모리에 올림
			```ad-example

			text editors, compilers, window systems

			```
		2. Shared code는 모든 프로세스의 **logical address space**에서 동일한 위치에 있어야 함.
			- 동일한 physical address를 가지는건 당연함.
			- 코드 안에는 logical address가 적혀있기 때문.
- Private code and data
	- 각 프로세스들은 독자적으로 메모리에 올림
	- Private data는 logical address space의 아무 곳에 와도 무방

##### ㄱ) Shared Page Example

세개의 프로세스가 같은 코드를 가지고 프로그램을 돌린다고 하면,  프로그램의 코드 부분은 세 개의 프로세스가 똑같은걸 써도 된다. 프로그램이 실행되면서 중간중간에 데이터 부분만 바뀐다. 

이런 것을 shared code라고 한다. 이런 것들에 대해서 각각을 물리적인 메모리에 별도로 올리는 것이 아니라, 하나만 물리적인 메모리에 올리는 것.

![](/bin/OS_image/os_8_18.png)

위 프로그램(P1, P2, P3)의 첫 번째, 두 번째, 세 번째 페이지가 물리적 메모리의 3번, 4번, 6번 프레임에 올라가있다. 

주소변환을 위한 page table은 따로 있겠지만, shared code 부분은 동일한 physical memory에 매핑이 되어 있다.

### b. segmentation

> 프로그램을 구성하는 address space(주소공간)를 의미 단위로 쪼갠 것.

> original segmentation을 쓰는 메모리 시스템은 현실적으론 없다.
> > segmentation을 쓰더라도 내부에는 paging을 같이 써야만 관리가 수월한 방식이 된다.

---

프로그램의 주소공간을 같은 크기로 자르는게 아니라, 어떤 의미있는 단위로 자르는 것
프로그램의 주소공간은 코드, 데이터, 스택. 이런 크게 3가지의 의미있는 공간으로 구성된다. 코드 segment, 데이터 segment, 스택 segment로 잘라서 각각의 segment를 필요시에 물리적인 메모리의 다른 위치에 올려놓을 수 있다.

segment는 프로그램의 주소공간을 구성하는 의미있는 단위라고 했기에 코드, 데이터, 스택이 될 수도 있고 더 잘게 자를수도 있다(코드 중에서도 함수가 여러개 있기에, 각각의 함수들을 다른 segment로 나누면 segment 수가 많아진다.)

segment는 의미 단위로 자른것이기 때문에 크기가 균일하지않다.

크기가 균일하지 않기 때문에 dynamic storage-allocation problem이 발생할 수 있음.
Hole이 발생할 수 있음

---

- 프로그램은 의미 단위인 여러 개의 segment로 구성
	- 작게는 프로그램을 구성하는 함수 하나하나를 세그먼트로 정의
	- 크게는 프로그램 전체를 하나의 세그먼트로 정의 가능
	- 일반적으로는 code, data, stack 부분이 하나씩의 세그먼트로 정의됨
- Segment는 다음과 같은 logical unit들임
	- main()
	- function,
	- global variables
	- stack
	- symbol table arrays

#### 1) Segmentation Architecture

- Logical address는 다음의 두 가지로 구성

$$<segment-number, offset>$$

- segment-number
	- segment 번호
- offset
	- segment 안에서 얼마나 떨어져있는지를 나타내는 offset

---

- Segment table (segment별로 주소변환을 위해 존재)
	- each table entry has:
		- base
			- **starting physical address** of the segment
		- limit
			- **length** of the segment
- Segment-table base register (STBR)
	- 물리적 메모리에서의 **segment table의 시작 위치**
- Segment-table length register (STLR)
	- 프로그램이 사용하는 **segment의 수(entry의 수)**

$$segment number s is legal if s < STLR$$

#### 2) Segmentation Hardware

![](/bin/OS_image/os_8_19.png)

segment table은 physical memory의 위치(시작위치)와 limit(segment의 길이)를 가진다.

paging 기법에서는 page의 크기가 전부 동일했기 때문에 limit이 필요없었던 것.

segment는 의미 단위로 짜르는 것이기 때문에 segment의 길이가 균일하지 않을 수 있어서 segment의 길이를 segment table entry에 같이 가지고 있게 된다.

##### ㄱ) segment 주소변환시 체크해야하는 것

1. logical address의 segment 번호가 STLR보다 작아야한다.
	- STLR에 segment의 개수가 들어 있음.
	- 만약, STLR보다 큰 segment를 요청하면 이것은 잘못된 시도이기 때문에 trap이 발생한다.
2. segment의 길이보다 segment 안에서 떨어진 offset이 작아야한다.
	- segment를 통해서 주소변환을 하는데, segment의 길이가 1000byte인데 segment 안에서 떨어진 위치(d, offset)를  2000byte 요청하면, trap 발생

정상적인 요청이라면 주소변환이 이루어지는데, 주소변환은 segment의 시작 위치에 offset을 더해서 주소변환을 한다.

physical memory의 위에서부터 base위치만큼 떨어진 곳에서 segment가 시작되고, 거기서부터 d만큼 떨어진 위치에 가면 원하는 주소에 내용이 들어있게 된다.

#### 3) Example of Segmentation

![](/bin/OS_image/os_8_20.png)

- 세그먼트 테이블
	- base
		- 물리적인 주소(Physical memory)의 시작 위치가 주어진다.
	- limit
		- 세그먼트의 길이도 주어진다.

#### 4) Segmentation Architecture (Cont.)

> segmentation은 의미 단위로 쪼개는 것이기 때문에 의미단위로 일하는 것은 segmentation이 효과적이다. 

- Protection
	- 각 세그먼트 별로 protection bit가 있음
	- Each entry
		- **Valid** bit = 0 => illegal segment
		- **Read/Write/Execution** 권한 bit
	- 의미단위로 자르기 때문에 하나의 page에 코드 영역과 데이터 영역이 함께 있을 수 없음
- Sharing
	- shared segment
	- same segment number
	
> segment는 **의미 단위**이기 때문에 **공유(sharing)와 보안(protection)에 있어 paging보다 훨씬 효과적**이다.
> > 메모리의 일부분을 공유하거나 메모리의 일부분에 대해서 읽기 혹은 쓰기 권한을 주는 등 동일한 크기 단위가 아닌 의미 단위로 하는 일에서는 segmentation이 유리하다.

- Allocation
	- first fit / best fit
	- external fragmentation 발생

> segment의 길이가 동일하지 않으므로 가변분할 방식에서와 동일한 문제점들이 발생
> > paging은 동일한 크기 단위로 자르기 때문에 중간중간에 물리적인 메모리의 조각이 발생할일이 없다.

#### 5) Segmentation과 Paging의 비교

- Paging
	- 개수가 많다.
	- 테이블을 위한 메모리 낭비가 심하다.
- Segmentation
	- 개수가 몇 개 없다.
	- 테이블을 위한 메모리 낭비가 적다.

#### 6) Sharing of Segments

> 세그먼트를 서로다른 두 프로세스가 공유하는 모습

![](/bin/OS_image/os_8_21.png)

- 0번 segment
	- 코드를 담고 있다.
	- 두 프로세스에서 같은 역할을 하고 있기 때문에 sharing을 하고 있다.
	- segment table을 통해 주소변환을 하면, 43062(base)에 위치하게 된다.

Shared segment는 같은 논리상의 주소에 있어야 한다.

### c. Segmentation with Paging (Paged Segmentation)

> Segment를 여러개의 Page로 구성하는 기법
> > Segment 하나가 여러 개의 Page로 구성된다.

- pure segmentation과의 차이점
	- **segment-table entry**가 segment의 **base address**를 가지고 있는 것이 아니라 segment를 구성하는 **page table**의 **base address**를 가지고 있음

![](/bin/OS_image/os_8_22.png)

Segment 하나가 여러 개의 Page로 구성되기 때문에, 먼저 Segment에 대한 주소 변환을 하게 된다.

각 프로그램이 가지고 있는 논리 주소는 (세그먼트 번호, offset(세그먼트 안에서 얼마나 떨어져 있는지를 나타냄))로 구성되어 있다.

1. segment table을 찾는다.
	- STBR에 segment의 시작 위치가 들어 있고, 위에서부터 s번째 entry에 가면. 이 segment에 대한 주소 변환 정보가 있었다.
2. segmentation에 대한 주소 변환을 해주면, 이 segment를 구성하는 page table의 시작 위치가 나온다.
	- segment 하나가 여러 개의 page로 구성되면, 각각의 page별로 주소변환을 해야한다. 그렇기 때문에 segment 당 page table이 존재하는 것이다.

---

- logical address
	- s
		- segment가 몇 번째 위치에서 시작되는지를 나타냄
	- d
		- segment offset (세그먼트 안에서 얼마나 떨어져 있는가를 나타냄)
		- segment 안에서 떨어진 offset의 길이가 segment의 길이 이내인 경우에 segment 안의 offset d를 아래처럼 자른다.

		```ad-note

		- p
			- 페이지 번호
		- d' (offset)
			- 페이지 안에서 얼마나 떨어져 있는지를 나타냄

		```

page table의 시작 위치로부터 segment offset 안의 페이지 번호(p)만큼 떨어진 entry에 가면, 이 page에 대한 주소변환 결과 (physical memory의 몇 번째 frame인지 frame 번호가 나온다.)

그 frame 번호를 앞에 붙이고, 페이지안에서의 offset을 뒤에 붙이면, physical memory의 주소가 된다.

---

#### 1) 장점

- 물리적인 메모리에 올라갈때는 page단위로 쪼개져서 올라가기 때문에, Allocation 문제가 발생하지 않는다. (외부 단편화 문제)
- 의미 단위로 해야하는 공유(sharing)나 보안(protection)같은 업무는 segment table level에서 한다.
	- 특정 segment의 protection이 read-only인지, read/write가 가능한지는 segment table의 해당 segment마다 해주면 된다.

---

운영체제도 하나의 프로그램이기 때문에, CPU를 잡으면 인스트럭션을 실행하는 것은 다른 프로그램과 다르지 않다.

# 참고자료

[1] 반효경, [이화여자대학교 :: CORE Campus (ewha.ac.kr)](https://core.ewha.ac.kr/publicview/C0101020140425151219100144?vmode=f). (accessed Dec 3, 2021)