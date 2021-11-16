# 1. JVM과 JAVA 프로그램 실행 과정을 설명하라.

JVM이란 JAVA Virtual Machine (자바 가상 머신)의 약자로, 자바 프로그램을 자바 API를 기반으로 실행하는 역할을 한다.

JVM은 자바 바이트코드를 실행하는 실행기이다.

## A. JAVA 프로그램 실행 과정

- 프로그램이 실행되면 JVM이 OS로부터 해당 프로그램이 필요로 하는 메모리를 할당받고,
- 자바 바이트코드로 변환된 (.class) 파일을 class 로더를 통해 JVM에 로딩한다.
- 로딩된 class 파일은 execution engine을 통해 해석되고, 실행된다.
- 필요시 garbage collection을 수행해서, 불필요하게 할당된 메모리를 해제한다.

> 자바 컴파일러에 의해 자바 소스 파일이 바이트 코드(.class)로 변환된다. 이 코드를 JVM에서 실행한다.

# 2. Garbage Collection이 필요한 이유

- JAVA 프로그램은 메모리를 명시적으로 지정해서 해제하지 않기 때문에, Garbage Collection Mechanism을 통해, 경우에 따라 더 이상 필요없는 객체를 찾아 지우는 작업을 수행한다.

## A. Garbage Collection 구조

- JVM 메모리 영역
	- JVM은 운영체제로부터 할당받은 메모리 영역을 세 영역으로 분리함.
		- 메소드 영역, JVM 스택, 힙 영역
		- 이 중에서 힙 영역에 생성된 객체가 저장되며, 사용하지 않는 객체를 GC를 통해  삭제함
			- JVM 힙 영역은 다음과 같이 나뉨
			- YOUNG, OLD, Permanent Generation
				- YOUNG generation
					- eden, S0, S1 (Survivor space)

## B. Garbage Collection 동작 방식

- 새롭게 생성된 객체는 YOUNG의 eden 영역에 들어가고, eden 영역이 다 차면 minor GC가 발생
- GC가 실행되면, GC를 실행하는 스레드 외에 나머지 스레드는 멈춘다.
- 불필요한 객체는 삭제되고 아직 필요한 객체는 S0으로 이동, S0에 있었던 객체는 S1로 이동, S1이 다 차면 S1에 아직 필요한 객체는 OLD generation으로 이동
- OLD generation은 크기가 크므로, 이 영역이 다 차는 경우는 자주 발생하지 않음.
	- 이 영역을 삭제할 떄 major GC (혹은 full GC) 발생
- minor GC는 자주 발생하지만, YOUNG 영역은 OLD 영역보다 적기 때문에, 프로그램 중지 시간(stop-the-world)은 짧아짐
- YOUNG 영역을 다 비우므로, YOUNG 영역에서는 연속된 여유 공간이 만들어짐.

# 3. Overriding VS Overloading

## A. Overriding

상위 클래스에 존재하는 메서드를 하위 클래스에 맞게 재정의하는 것

매서드 이름 및 파라미터 수 동일

## B. Overloading

두 메서드가 같은 이름을 가지고 있으나, 파라미터 수나 자료형이 다른 경우.

중복정의

# 4. abstract VS interface

## A. 추상 클래스

is - a이다. (상속도 마찬가지)

클래스 내부에 내용이 없는 abstract 메소드가 한 개 이상 있는 클래스.

abstract 메소드는 내용이 없다. 따라서 자식 클래스에서 재정의(오버라이딩)해야한다.

extends를 통해 상속한다.

추상 메서드를 1개 이상 가진 클래스는 객체 생성이 안되므로, 추상 클래스를 상속받은 클래스의 객체 생성을 위해서 추상 메소드를 구현해야함.

**미완성 설계도**

### a. 사용이유

- 공통된 필드와 메서드를 통일하기 위한 목적
- 실체 클래스 구현시, 시간절약
- 규격에 맞는 실체클래스 구현

### b. 사용시기

상속 관계를 쭉 타고 올라갔을때 같은 조상클래스를 상속하는데 기능까지 완벽히 똑같은 기능이 필요한 경우

## B. interface

has - a일 수 있다.

클래스 내부가 내용이 없는 abstract 메소드(오버라이딩해야함), static 메소드(변경할 수 없음), default 메소드(원하면 변경 가능)로 구성된 클래스.

implements를 통해 2개 이상 상속(다중 상속)받을 수 있음.

정의된 메소드를 implements 받은 곳에서 구현을 강제함.

**기본 설계도**

> 인터페이스를 implements한 클래스가 동일한 동작을 수행하도록 보장한다.

### a. 사용시기

상속 관계를 쭉 타고 올라갔을때 다른 조상클래스를 상속하는데 같은 기능이 필요할 경우 인터페이스 사용

# 5. Call by value

자바는 call by reference가 없다.

> 매개변수를 넘기는 과정에서 직접적인 참조를 넘긴 게 아닌, 주소 값을 복사해서 넘기기 때문에 이는 call by value이다. 복사된 주소 값으로 참조가 가능하니 주소 값이 가리키는 객체의 내용이 변경되는 것이다.

[[Java] Java는 Call by reference가 없다 (tistory.com)](https://deveric.tistory.com/92)

# 6. 객체와 클래스의 차이점

## A. 클래스(class)

- 개념
	- 객체를 만들어내기 위한 틀(설계도)
	- 연관되어 있는 변수와 메서드의 집합

ex) 붕어빵 틀

## B. 객체(object)

- 개념
	- 소프트웨어 세계에 구현할 대상
	- 클래스에 선언된 모양 그대로 생성된 실체
- 특징
	- **클래스의 인스턴스**라고도 부른다.
	- 객체는 모든 인스턴스를 대표하는 포괄적인 의미를 갖는다.
	- oop의 관점에서 클래스의 타입으로 선언되었을 때 **객체**라고 부른다.

ex) 붕어빵

## C. 인스턴스(Instance)

- 개념
	- 설계도를 바탕으로 소프트웨어 세계에 구현된 구체적인 실체
		- 즉, 객체를 소프트웨어에 실체화하면 그것을 **인스턴스**라고 부른다.
		- 실체화된 인스턴스는 메모리에 할당된다.
- 특징
	- 인스턴스는 객체에 포함된다고 할 수 있다.
	- oop의 관점에서 객체가 메모리에 할당되어 실제 사용될 때 **인스턴스**라고 부른다.
	- 추상적인 개념과 구체적인 객체 사이의 **관계**에 초점을 맞출 경우에 사용한다.
		- **~의 인스턴스**의 형태로 사용된다.
		- 객체는 클래스의 인스턴스다.
		- 객체 간의 링크는 클래스 간의 연관 관계의 인스턴스다
		- 실행 프로세스는 프로그램의 인스턴스다
	- 즉, 인스턴스라는 용어는 반드시 클래스와 객체 사이의 관계로 한정지어서 사용할 필요는 없다.
	- 인스턴스는 어떤 원본(추상적인 개념)으로부터 **생성된 복제본**을 의미한다.

```java

/* 클래스 */
public class Animal {
  ...
}
/* 객체와 인스턴스 */
public class Main {
  public static void main(String[] args) {
    Animal cat, dog; // '객체'

    // 인스턴스화
    cat = new Animal(); // cat은 Animal 클래스의 '인스턴스'(객체를 메모리에 할당)
    dog = new Animal(); // dog은 Animal 클래스의 '인스턴스'(객체를 메모리에 할당)
  }
}
https://gmlwjd9405.github.io/2018/09/17/class-object-instance.html

```

# 7. JAVA 메모리 영역

## A. Class Area (Method Area)

- Method Area, Class Area, Code Area, Static Area
- 클래스 정보(멤버변수의 이름), 변수정보(데이터타입, 접근제어자정보), 메소드정보(메소드이름, 리턴타입, 파라미터, 접근제어자 정보), static변수, final class 변수, constant pool(상수 풀: 문자상수, 타입, 필드, 객체참조가 저장됨) 등을 분류해서 저장한다.
- JVM이 동작해서 클래스가 로딩될 때 생성

## B. Heap Area

- new 키워드로 생성된 객체와 배열이 저장되는 영역
- Method Area에 로드된 클래스만 생성이 가능하다.
- GC의 주요 대상이 된다. (Stack, Class Area도 대상이 된다.)
- 효율적인 GC를 위해 메모리 영역이 분리되어 있다. (Eden, Survivor1, Survivor2, Old)
- 런타임시 할당된다.

## C. Stack Area

- 지역변수, 파라미터, 리턴값, 연산에 사용되는 임시값등이 생성되는 영역
- 메소드를 호출할 때마다 개별적으로 스택이 생성되며 종료 시 영역에서 해제된다.
- 컴파일 타임 시 할당된다.

### a 런타임과 컴파일타임

- 컴파일 타임
	- 소스코드가 기계어로 변환되어 실행가능한 프로그램이 되는 과정
	- Syntax Error, 파일참조오류, 타입체크오류 등
- 런타임
	- 컴파일타임 이후 프로그램이 실행되는 때
	- 0 나누기 오류, Null 참조 오류, 메모리 부족 오류

## D. PC register

- 스레드가 생성될때마다 생성되며 현재 스레드가 실행되는 부분의 주소와 명령을 저장하는 영역
- 이를 이용해서 스레드를 돌아가면서 수행할 수 있게 한다.

## E. Native Method Stack

- 자바 외 언어로 작성된 네이티브 코드를 위한 메모리 영역 (JNI)

## F. 멀티스레드시 공유되는 메모리 영역

- 1, 2번인 Method Area와 Heap Area는 모든 스레드가 공유한다.
- 3, 4, 5번은 각각의 스레드마다 생성되고 공유되지 않는다.

# 8. 접근 지정자

## A. public

- 모든 접근을 허용한다.
- 어떤 클래스가 접근을 하더라도 모두 허용한다.

## B. protected

- 상속받은 클래스 또는 같은 패키지에서만 접근이 가능하다.

## C. default

- 기본 접근제한자로써 아무것도 붙지 않는다.
- 같은 패키지에서만 접근 가능하다.

## D. private

- 같은 클래스에서만 접근이 가능하다.
- 외부에서는 접근이 불가능하다.

> 접근 영역 : public > protected > default > private

# 9. 객체지향 5대 원칙 (객체지향 설계)

## A. SRP (단일 책임 원칙, Single responsibility, principle)

- 한 클래스는 하나의 책임만 가져야 한다.

## B. OCP (개방-폐쇄 원칙, Open/Closed principle)

- 소프트웨어 요소는 확장에는 열려 있으나 변경에는 닫혀 있어야 한다.

## C. LSP (리스코프 치환 원칙, Liskov substitution principle)

- 프로그램의 객체는 프로그램의 정확성을 깨뜨리지 않으면서 하위 타입의 인스턴스로 바꿀 수 있어야 한다.

## D. ISP (인터페이스 분리 원칙, Interface segregation principle)

- 특정 클라이언트를 위한 인터페이스 여러 개가 범용 인터페이스 하나보다 낫다

## E. DIP (의존관계 역전 원칙, Dependency inversion principle)

- 프로그래머는 추상화에 의존해야지, 구체화에 의존하면 안된다.

# 10. 상수

final을 사용한다.

값을 수정하지 못하게 고정한다.

# 11. static (정적 변수, 메소드)

static은 보통 변수나 메소드 앞에 static 키워드를 붙여서 사용한다.

Static 키워드를 통해 static 영역에 할당된 메모리는 모든 객체가 공유하는 메모리이다. 하지만 garbage collector의 관리 영역 밖에 존재하므로 static을 자주 사용하면 프로그램의 종료시까지 메모리가 할당된 채로 존재하게 되어 시스템에 악영향을 준다.

메모리 할당이 딱 한 번만 하게 되어 여러 객체가 해당 메모리를 공유한다. 그리고 프로그램이 종료될 때 해제된다. 따라서 메모리 사용에 이점이 있다.

## A. static 변수

static 변수는 클래스 변수이다.

객체를 생성하지 않고도 static 자원에 접근이 가능하다.

## B. static 메소드

객체의 생성 없이 호출이 가능하다.

스태틱 메소드 안에는 인스턴스 변수 접근이 불가능하다. static 변수만 접근할 수 있다.

# 12. 객체지향

## a. 4가지 특징

- 추상화
	- 추상적인 것을 구체적으로 모델링하는 모든 과정
	- 사물들 간의 공통점은 취하고 차이점은 버린다.
- 캡슐화
	- 객체를 캡슐로 싸서 내부를 보호하고 볼 수 없게 하는 것으로 객체의 가장 본질적인 특징이다.
		- 데이터 캡슐화
		- 은닉화
- 상속성
	- class의 데이터와 메소드를 다른 class에 물려주거나 물려받는것.
	- 상속받은 메소드를 추가적으로 데이터와 메소드내용 재사용 가능하다.
- 다형성
	- 같은 이름의 메소드가 클래스 혹은 객체에 따라 다르게 동작하도록 구현되는 것을 말한다.
		- 오버라이딩
		- 오버로딩

# 13. Casting (업캐스팅 & 다운캐스팅)

## A. 캐스팅이란?

변수가 원하는 정보를 다 갖고 있는 것

```
int a = 0.1; // (1) 에러 발생 X
int b = (int) true; // (2) 에러 발생 O, boolean은 int로 캐스트 불가
```

(1)은 0.1이 double형이지만, int로 될 정보 또한 가지고 있음
(2)는 true는 int형이 될 정보를 가지고 있지 않음

### a. 캐스팅이 필요한 이유는?

1. 다형성
	- 오버라이딩된 함수를 분리해서 활용할 수 있다.
2. 상속
	- 캐스팅을 통해 범용적인 프로그래밍이 가능하다.

## B. 형변환의 종류

1. 묵시적 형변환 (업캐스팅)
	- 캐스팅이 자동으로 발생
		```
		Parent p = new Child(); // (Parent) new Child()할 필요가 없음
		```

		> Parent를 상속받은 Child는 Parent의 속성을 포함하고 있기 때문
2. 명시적 형변환 (다운캐스팅)
	- 캐스팅할 내용을 적어줘야 하는 경우
		```
		Parent p = new Child();
		Child c = (Child) p;
		```

		> 다운캐스팅은 업캐스팅이 발생한 이후에 작용한다.

### a. 예시 문제

```
class Parent {
	int age;

	Parent() {}

	Parent(int age) {
		this.age = age;
	}

	void printInfo() {
		System.out.println("Parent Call!!!!");
	}
}

class Child extends Parent {
	String name;

	Child() {}

	Child(int age, String name) {
		super(age);
		this.name = name;
	}

	@Override 
	void printInfo() {
		System.out.println("Child Call!!!!");
	}

}

public class test {
    public static void main(String[] args) {
        Parent p = new Child();
        
        p.printInfo(); // 문제1 : 출력 결과는?
        Child c = (Child) new Parent(); //문제2 : 에러 종류는?
    }
}
```

#### 1) 문제1 : `Child Call!!!!`

자바에서는 오버라이딩된 함수를 동적 바인딩하기 때문에, Parent에 담겼어도 Child의 printInfo() 함수를 불러오게 된다.

#### 2) 문제2 : `Runtime Error`

컴파일 과정에서는 데이터형의 일치만 따진다. 프로그래머가 따로 (Child)로 형변환을 해줬기 때문에 컴파일러는 문법이 맞다고 생각해서 넘어간다. 하지만 런타임 과정에서 Child 클래스에 Parent 클래스를 넣을 수 없다는 것을 알게 되고, 런타임 에러가 나오게 되는것!

# 14. 스레드 생성 방식

Runnable (인터페이스)이나 Thread 클래스 (상속)를 상속받아서 run() 메소드를 구현해준다.

- 장점
	- 빠른 프로세스 생성, 메모리를 적게 사용 가능, 정보 공유가 쉬움
- 단점
	- 데드락에 빠질 위험이 존재