<details>
<summary>Table of Contents</summary>

- [Java 와 JVM의 구성요소](#java-와-jvm)
</details>


## **Java 와 JVM**

Java의 가장 큰 특징 중 하나가 어느 플랫폼, 어느 하드웨어(CPU)든, 어떤 운영체제(OS)이던 상관없이 컴파일된 코드가 플랫폼 독립적인 것이다.

다시 말해 어떤 필랫폼이든 작성한 소스를 변경할 필요 없이 다 실행할 수 있다는 것이다. 이 점이 웹 어플리케이션의 특성과 맞아 떨어져 폭발적인 인기와 함께 현재 웹 어플리케이션 개발에 가장 사용이 많이 되는 언어 중 하나가 되었다.

그리고 이 특징을 구현하기 위해 필요한 것이 바로 이 JVM이다.

![https://blog.kakaocdn.net/dn/EvIZw/btrXOScJVoi/h4TF2kiZ6KiloKFl4WGkk1/img.png](https://blog.kakaocdn.net/dn/EvIZw/btrXOScJVoi/h4TF2kiZ6KiloKFl4WGkk1/img.png)

예를 들어 C언어로 작성된 Test.c가 있다고 하면, 이 Test.c를 윈도우 컴파일러를 사용해서 컴파일하면 Test.exe가 만들어니다. 윈도우 컴파일러로 컴파일되었기에 Test.exe는 윈도우에서만 실행되는 실행 파일이다. 리눅스 운영체제에서는 실행할 수 없다. 즉 C / C++에서는 컴파일 플랫폼과 타겟 플랫폼이 다를 경우에는 프로그램이 동작하지 않는다. 만약 이 Test.exe 파일을 리눅스 운영체제에서 실행하려면 리눅스 환경을 타겟으로 크로스 컴파일을 해서 리눅스 운영체제에 맞는 실행 파일을 새로 만들어야 한다.

**반면**

![https://blog.kakaocdn.net/dn/my07N/btrXOwHNgJA/Sd3FPDh5MaGG8DlPGFod6k/img.png](https://blog.kakaocdn.net/dn/my07N/btrXOwHNgJA/Sd3FPDh5MaGG8DlPGFod6k/img.png)

Java의 경우에는 Java언어로 작성된 Test.java는 컴파일하면 Test.class 파일이 생성된다. 그리고 이렇게 생성된 바이트코드는 각자의 플랫폼에 설치되어 있는 자바 가상 머신(JVM)이 운영체제에 맞는 실행 파일로 바꿔준다. 즉 Java에서는 C언어와는 달리 JVM을 사용하기 때문에 각자의 플랫폼에 맞게끔 컴파일을 따로따로 해줘야 할 필요가 없다.

---

### **JVM(Java Virtual Machine)**

자바 가상 머신은 단순하게 말하면 컴파일 된 코드(바이트 코드)를 실행시켜주는 가상의 컴퓨터라고 생각하면 이해하기 쉬울 것이다.

※ JVM은 H/W와 OS 위에서 실행되어서 JVM 자체는 플랫폼에 종속적 즉, 플랫폼에 따라 호환되는 JVM을 실행시켜줘야함

![https://blog.kakaocdn.net/dn/ns3dT/btrXPwtBTvz/e1EHw5lZkjUkbcKgpd4k2k/img.png](https://blog.kakaocdn.net/dn/ns3dT/btrXPwtBTvz/e1EHw5lZkjUkbcKgpd4k2k/img.png)

**1. 우선 자바 개발자들이 IntelliJ 나 기타 개발툴을 사용해 .java 파일을 생성한다.**

**2. 그리고 Build 라는 작업을 하게 되면 Java Compiler의 javac 라는 명령어를 사용해 .class 파일을 생성하게 된다. 이 것은 아직 컴퓨터가 읽을 수 없는 자바 바이트코드(반기계어)이다**.

![https://blog.kakaocdn.net/dn/6QESA/btrXM4SNJN2/gcoL1JswG3gSFqHQ4kwNQ0/img.jpg](https://blog.kakaocdn.net/dn/6QESA/btrXM4SNJN2/gcoL1JswG3gSFqHQ4kwNQ0/img.jpg)

**3. 이렇게 컴파일된 바이트코드를 JVM의 클래스 로더로 전달한다.**

**4. 클래스로더는 동적로딩(Dynamic Loading)을 통해 필요한 클래스들을 로딩 및 링크하여 런타임 데이터 영역(Runtime Data area), 즉 JVM의 메모리에 올린다.**

- 동적 로딩이란?

-프로그램을 실행할 때, 필요할 때마다 동적으로 메모리를 생성하고, 필요없는 메모리는 자동으로 메모리에서 소멸시킨다.

1) 대표적인 예: Java (웹과 같은 유동적, 가변적인 프로그램)

2) 장점: 필요한 기능만 메모리에 불러와 사용하기 때문에, 큰 프로그램도 작은 메모리에서 실행이 가능하다.

3) 단점: 프로그램의 실행 속도가 느려질 수 있다.

ex)  -로드 타임 동적 로딩:하나의 클래스를 로딩하는 과정에서 필요한 다른 클래스를 동적으로 로딩하는 것

-런타임 동적 로딩 : 코드를 실행하는 순간에 필요한 클래스를 로딩하는 것

- 정적 로딩이란?

-프로그램을 실행할 때, 모든 실행파일이 메모리에 로드된다.

1) 대표적인 예: C언어 (자주 변하지 않는 소프트웨어 등)



**5. 실행엔진(Execution Engine)은 JVM메모리에 올라온 바이트 코드들을 명령어 단위로 하나씩 가져와서 실행한다.**

1. 인터프리터 : 바이트 코드 명령어를 하나씩 읽어서 해석하고 실행한다. 하나하나의 실행은 빠르나, 전체적인 실행 속도가 느리다는 단점을 가진다.

2. JIT 컴파일러(Just-In-Time Compiler) : 인터프리터의 단점을 보완하기 위해 도입된 방식으로 바이트 코드 전체를 컴파일하여 바이너리 코드로 변경하고 이후에는 해당 메서드를 더이상 인터프리팅 하지 않고, 바이너리 코드로 직접 실행하는 방식이다. 하니씩 인터프리팅하여 실행하는 것이 아니라 바이트 코드 전체가 컴파일 된 바이너리 코드를 실행하는 것이기 때문에 전체적인 실행속도는 인터프리팅 방식보다 빠르다.

※ 다만 처음 컴파일 할 때는 느리다.

![https://blog.kakaocdn.net/dn/OYTL1/btrXK8OC6e3/c3VWiaNtcBuVBr3FG5EOU0/img.png](https://blog.kakaocdn.net/dn/OYTL1/btrXK8OC6e3/c3VWiaNtcBuVBr3FG5EOU0/img.png)

---

### **JVM 구성**

![https://blog.kakaocdn.net/dn/b5bvZa/btrXQMpNSec/MAK5qmgZgy30ZWGqE1anK1/img.png](https://blog.kakaocdn.net/dn/b5bvZa/btrXQMpNSec/MAK5qmgZgy30ZWGqE1anK1/img.png)

> Class Loader(클래스 로더)
>
![https://blog.kakaocdn.net/dn/csb9tt/btrXOtKUVXV/gjPsuiH0KwGyKBcdasAKjK/img.png](https://blog.kakaocdn.net/dn/csb9tt/btrXOtKUVXV/gjPsuiH0KwGyKBcdasAKjK/img.png)

- 위에서 보이는 바와 같이, JVM은 자바 컴파일러에 의해 생성된 바이트코드(.class)를 클래스로더를 통해 불러온다.
- 클래스 로더는 바이트코드로부터 실제 객체를 생성한다. 위 그림과 같이 메모리에 인스턴스 올리는, 즉 클래스의 인스턴스화를 수행하는 것이 바로 클래스 로더이다.
-

클래스 로드 과정

1 - 로드 : 클래스 파일을 가져와서 JVM의 메모리에 로드한다.

2 - 검증 : 클래스 로드 전 과정 중에서 가장 복잡하고 시간이 많이 걸리는 과정으로 읽어들인 클래스가 자바 언어 명세 (JAVA Language Specification) 및 JVM 명세에 명시된 대로 구성되어 있는지 검사한다.

3 - 준비 : 클래스가 필요로 하는 메모리를 할당한다. 필요한 메모리란 클래스에서 정의된 필드, 메서드, 인터페이스들을 나타내는 데이터 구조들 등등을 말한다.

4 - 분석 : 클래스의 상수 풀 내 모든 심볼릭 레퍼런스를 다이렉트 레퍼런스로 변경한다.

※ symbolic references - 참조하는 클래스의 특정 메모리 주소를 참조 관계로 구성한 것이 아닌 참조하는 대상의 이름으로 참조하는 것

※ direct references - 참조하는 클래스의 특정 메모리 주소를 참조하는 것

5 - 초기화 : 클래스 변수들을 적절한 값으로 초기화한다. (static 필드들을 설정된 값으로 초기화 등)

> Execution Engine (실행 엔진)
>
로드된 클래스의 바이트코드를 실행하는 런타임 모듈이 바로 실행 엔진이다. 클래스 로더를 통해 JVM내의 Runtime Data Areas에 배친된 바이트코드는 실행 엔진에 의해 실행되며, 실행 엔진은 자바 바이트 코드를 명령어 단위로 읽어서 실행한다. 여기서 Interpreter(인터프리터) 방식과 JIT compiler 방식을 사용하게 된다.

> Gabage Collector (가비지 컬렉터)
>
가비지 컬렉터는 유효하지 않은 메모리인 가비지(Garbage)를 정리해주는 역할을 한다. 즉 Garbage Collection(GC)를 담당한다. 세부 내용은 다음 글에서 계속하겠다.

> Runtime Data Area
>
![https://blog.kakaocdn.net/dn/EAWcc/btrXOsy6yUs/xKPjsW7yVfmQuF0Je3mA7K/img.png](https://blog.kakaocdn.net/dn/EAWcc/btrXOsy6yUs/xKPjsW7yVfmQuF0Je3mA7K/img.png)

Runtime Data Area는 JVM이 프로그램을 수행하기 위해 OS로부터 별도로 할당받은 메모리 공간을 말한다. Runtime Data Area는 크게 5가지 영역으로 나뉜다.

- PC Register

JVM의 PC Register는 CPU 내의 기억장치인 레지스터와 다르게 작동한다. PC Register는 각 쓰레드 별로 하나씩 존재하며 현재 수행 중인 JVM Instruction의 주소를 가지게 된다. 즉, 쓰레드가 어떤 명령을 실행할지 기록하는 부분이라고 할 수 있다.

※ 쓰레드 - 프로그램(프로세스) 실행의 단위이며 하나의 프로세스는 여러개의 쓰레드로 구성이 가능하다.

- JVM Stack

메소드(method)가 호출될 때 메서드와 메서드의 정보는 JVM Stack에 쌓이게 된다. 즉 메서드의 매개변수(parameter), 지역 변수(local variable), return 주소, 임시 변수 등의 정보를 기록하는 스택이다. 각 쓰레드 별로 생성되기 때문에 다른 스레드에 접근할 수 없다. 메서드 호출이 종료되면 스택에서 정보들이 제거된다.

- Native Method Stack

자바 외의 언어로 작성된 네이티브 코드들을 위한 스택이다. Java Native Interface를 통해 호출되는 C/C++ 등의 코드를 수행한다.

- Method Area

모든 쓰레드가 공유하는 메모리 영역으로 클래스, 인터페이스, 메서드, 필드, Static 변수 등의 바이트 코드를 보관한다. Method Area에는 Runtime Constant Pool이라는 별도의 관리 영역도 존재한다. 이는 상수 자료형을 저장하고 참조하여 중복을 막는 역할을 수행한다.

- Heap

![https://blog.kakaocdn.net/dn/YIKCC/btrXRUH2EN2/yIOS5RH8Ca25kCf2z2iON0/img.png](https://blog.kakaocdn.net/dn/YIKCC/btrXRUH2EN2/yIOS5RH8Ca25kCf2z2iON0/img.png)

Runtime 시점에 동적으로 할당하여 사용하는 영역이다. 클래스를 이용해 인스턴스를 생성하면 Heap에 저장된다. 즉, new 연산자를 이용해 생성된 객체를 저장하는 영역이다. Heap은 크게 New/Young 영역, Old 영역, Permanent Generation 3 영역으로 나뉜다.

※ 참고로 java8 이후에는 Permanent 영역이 Metaspace 영역으로 바뀌었다.

---

참고

[https://code-lab1.tistory.com/92](https://code-lab1.tistory.com/92)

[https://steady-snail.tistory.com/67](https://steady-snail.tistory.com/67)

[https://aljjabaegi.tistory.com/387](https://aljjabaegi.tistory.com/387)