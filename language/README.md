<details>
<summary>Table of Contents</summary>

- [Java 와 JVM의 구성요소](#java-와-jvm)
- [Java Garbage Collection](#java-garbage-collection)
- [Java Collection](#java-collection)
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


---


## **Java Garbage Collection**

Garbage Collection이란, 스택으로부터, Heap 영역 객체 중 도달 불가능한 객체들을 자동으로 메모리에서 제거해주는 개념이다. Java는 객체지향 언어인만큼, 힙을 사용하여 객체를 생성하는 경우가 굉장히 많다. 개발자들은 이렇게 힙을 자유롭게 사용하고, 더 이상 사용되지 않는 객체들은 가비지 컬렉션 과정에서 자동으로 메모리에서 제거된다.

그렇다면 다른 언어들은 어떠할까?

### **C언어**

```bash
#include <stdio.h>#include <stdlib.h>

void main()
{
    int* pPoint;
    pPoint = (int*)malloc(sizeof(int)*5);

    pPoint[0] = 25;
    pPoint[1] = 45;
    pPoint[2] = 50;
    pPoint[3] = 70;
    pPoint[4] = 99;

    int i = 0;
    for ( i = 0; i < 5; i++ )
        printf("pPoint[%d] : %d\n", i, pPoint[i]);

    free(pPoint);
}
```

C & C++ 에서는 Java와 달리 메모리에 직접 접근한다. 그렇기 때문에 free() 메소드를 명시적으로 사용하여 할당 받았던 메모리를 해제해주는 과정이 필요하다. free() 메소드 호출을 하지 않게 된다면 당장은 직접적인 영향이 없다고 생각될 수 있어도 이후 메모리 누수 (memory leak)가 발생할 수 있고 향후의 프로그램을 보장할 수 없다.

Java에서는 메모리 문제를 어떻게 해결을 할까? 그 이유는 C++과 달리 메모리 영역에 직접 접근하지 않고, JVM이라는 가상머신을 사용해서 간접적으로 접근하기 때문이다.

※ free 함수 - 힙 영역에 할당된 메모리를 해제하는 함수

---

### **가비지 컬렉션 대상**

가비지 컬렉션은 특정 객체가 Garbage인지 아닌지 판단하기 위해 도달성, 도달능력(Reachablily)이라는 개념을 적용한다.

객체에 레퍼런스가 있다면 Reachable로 구분되고, 객체의 유효한 레퍼런스가 없다면 Unreachable로 구분해 버리고 수거해버린다.

※ Reachable : 객체가 참조되고 있는 상태

※Unreachable : 객체가 참조되고 있지 않은 상태 즉 GC의 대상

![https://blog.kakaocdn.net/dn/bnAUxR/btrYjldFSCJ/plnKnfWdO0usRSk2fmg5L1/img.png](https://blog.kakaocdn.net/dn/bnAUxR/btrYjldFSCJ/plnKnfWdO0usRSk2fmg5L1/img.png)

JVM메모리에서는 객체들은 실질적으로 Heap영역에 생성되고 Method Area나 Stack Area에서는 Heap Area에 생성된 객체의 주소만 참조하는 형식으로 구성된다.

특정 이벤트 등으로 인하여 Heap Area 객체의 메모리 주소를 가지고 있는 참조 변수가 삭제되는 현상이 발생하면, 참조되고 있지 않은 객체 (Unreachable)이 발생한다. 그리고 GC는 이러한 것들을 주기적으로 제거하는 역할을 한다.

---

### **가비지 컬렉션 청소 방식**

**1. Stop The World**

Stop The World는 가비지 컬렉션을 실행하기 위해 JVM이 애플리케이션의 실행을 멈추는 작업이다. GC가 실행될 때는 GC를 실행하는 쓰레드를 제외한 모든 쓰레드들의 작업이 중단되고, GC가 완료되면 작업이 재개된다. 당연히 모든 쓰레드들의 작업이 중단되면 애플리케이션이 멈추기 때문에, GC의 성능 개선을 위해 튜닝을 한다고 하면 보통 stop-the-world의 시간을 줄이는 작업을 하는 것이다. 또한 JVM에서도 이러한 문제를 해결하기 위해 다양한 실행 옵션을 제공한다.

**2. Mark and Sweep 알고리즘**

- Mark : 사용되는 메모리와 사용되지 않는 메모리를 식별하는 작업
- Sweep : Mark 단계에서 사용되지 않음으로 식별된 메모리를 해제하는 작업
- Compaction : Sweep 후에 분산된 객체들을 Heap의 시작 주소로 모아 메모리가 할당된 부분과 그렇지 않은 부분으로 압축한다.

![https://blog.kakaocdn.net/dn/lDidk/btrX41aqZIq/uO17AnUY2ng9BWrnVZ84f0/img.png](https://blog.kakaocdn.net/dn/lDidk/btrX41aqZIq/uO17AnUY2ng9BWrnVZ84f0/img.png)

---

### **Minor GC와 Major GC**

JVM의 힙 영역은 동적으로 레퍼런스 데이터가 저장되는 공간으로, 가비지 컬렉션에 대상이 되는 공간이다.

![https://blog.kakaocdn.net/dn/bkOeuE/btrYiVGrNuk/EOwGG969FbKDa5EoKq4QN1/img.png](https://blog.kakaocdn.net/dn/bkOeuE/btrYiVGrNuk/EOwGG969FbKDa5EoKq4QN1/img.png)

### **Young 영역 (Young Generation)**

- 새롭게 생성된 객체가 할당되는 영역
- 대부분의 객체가 금방 Unreachable 상태가 되기 때문에, 많은 객체가 Young 영역에 생성되었다가 사라진다.
- Young 영역에 대한 가비지 컬렉션을 Minor GC라고 부른다.

### **Old 영역 (Old Generation)**

- Young 영역에서 Reachable 상태를 유지하여 살아남은 객체가 복사되는 영역
- Young 영역보다 크게 할당되며, 영역의 크기가 큰 만큼 가비지는 적게 발생한다.
- Old 영역에 대한 가비지 컬렉션을 Major GC 또는 Full GC라고 부른다.

힙 영역은 효율적인 GC를 위해 Young 영역을 3가지 영역(Eden, survivor 0, survivor 1)으로 나눈다.

**Eden**

- new를 통해 새로 생성된 객체가 위치한다.
- 정기적인 쓰레기 수집 후 살아남은 객체들은 Survivor 영역으로 보낸다.

**Survivor 0 & Survivor 1**

- 최소 1번의 GC로부터 살아남은 객체가 존재하는 영역
- Survivor 영역에는 특별한 규칙이 있는데, Survivor 0 또는 Survivor 1 둘 중 하나에는 꼭 비어 있어야 한다.

---

### **GC 발생 시나리오**

![https://blog.kakaocdn.net/dn/JmsT7/btrYjfLz6Jd/61h5a5lEgY43lewlao3j00/img.png](https://blog.kakaocdn.net/dn/JmsT7/btrYjfLz6Jd/61h5a5lEgY43lewlao3j00/img.png)

- 객체가 생성되면 Eden 영역에 위치하게 된다.

![https://blog.kakaocdn.net/dn/2LWt4/btrYaoQlWqr/TsPJak4VQ0WWIkAakEEy80/img.png](https://blog.kakaocdn.net/dn/2LWt4/btrYaoQlWqr/TsPJak4VQ0WWIkAakEEy80/img.png)

- Eden 영역이 가득차게 되면 Minor GC가 발생하여 참조가 없는 객체는 삭제되고, 참조 중인 객체는 Survivor 영역으로 이동한다.

![https://blog.kakaocdn.net/dn/ckgcF2/btrX6pvvyYf/ls4lgBkGgB4t2sEsUnDBk1/img.png](https://blog.kakaocdn.net/dn/ckgcF2/btrX6pvvyYf/ls4lgBkGgB4t2sEsUnDBk1/img.png)

- Survivor 영역이 가득차게 되면 minor GC가 발생하고 참조가 없는 객체는 삭제되고,
참조 중인 객체는 다른 Survivor 영역으로 이동한다.

- 살아남은 객체들은 age의 값이 1씩 증가

※age 값 : Survivor 영역에서 객체가 살아남은 횟수를 의미하는 값이며, Object Header에 기록된다.

![https://blog.kakaocdn.net/dn/dp3gT1/btrYjfkweed/uI4CIm3QT8IV6IckM8cip1/img.png](https://blog.kakaocdn.net/dn/dp3gT1/btrYjfkweed/uI4CIm3QT8IV6IckM8cip1/img.png)

Survivor 영역에서의 GC과정을 반복하며, 계속 참조 중인 객체는 Old 영역으로 이동한다.

※객체의 age가 임계값에 도달하게 되면 이동한다. / 이를 promotion이라 부른다

![https://blog.kakaocdn.net/dn/HhnYB/btrYjXjl0Jx/B4EloOVZfBEf4iu37M49k1/img.png](https://blog.kakaocdn.net/dn/HhnYB/btrYjXjl0Jx/B4EloOVZfBEf4iu37M49k1/img.png)

- Eden 영역에서 Survivor 영역으로 이동 할 때 객체가 남아있는 영역보다 큰 경우 Old 영역으로 이동한다.

Major GC는 Old 영역의 데이터가 가득 차면 GC를 실행하는 단순한 방식이다.

---

참조

[https://deveric.tistory.com/64](https://deveric.tistory.com/64)

[https://inpa.tistory.com/entry/JAVA-%E2%98%95-%EA%B0%80%EB%B9%84%EC%A7%80-%EC%BB%AC%EB%A0%89%EC%85%98GC-%EB%8F%99%EC%9E%91-%EC%9B%90%EB%A6%AC-%EC%95%8C%EA%B3%A0%EB%A6%AC%EC%A6%98-%F0%9F%92%AF-%EC%B4%9D%EC%A0%95%EB%A6%AC](https://inpa.tistory.com/entry/JAVA-%E2%98%95-%EA%B0%80%EB%B9%84%EC%A7%80-%EC%BB%AC%EB%A0%89%EC%85%98GC-%EB%8F%99%EC%9E%91-%EC%9B%90%EB%A6%AC-%EC%95%8C%EA%B3%A0%EB%A6%AC%EC%A6%98-%F0%9F%92%AF-%EC%B4%9D%EC%A0%95%EB%A6%AC)

[https://blog.ycpark.net/entry/JAVA%EC%9D%98-GC%EC%9D%98-%EC%A2%85%EB%A5%98-%EB%B0%8F-%ED%8A%B9%EC%A7%95](https://blog.ycpark.net/entry/JAVA%EC%9D%98-GC%EC%9D%98-%EC%A2%85%EB%A5%98-%EB%B0%8F-%ED%8A%B9%EC%A7%95)

---

# **Java Collection**

Java collection에는 List, Map, Set 인터페이스를 기준으로 여러 구현체가 존재한다. 이에 더해 Stack과 Queue 인터페이스도 존재한다. 왜 이러한 Collection을 사용할까?

그 이유는 다수의 Data를 다루는데 표준화된 클래스들을 제공해 주기 때문에 DataStructure를 직접 구현하지 않고 편하게 사용할 수 있기 때문이다. 또한 배열과 다르게 객체를 보관하기 위한 공간을 미리 정하지 않아도 되므로, 상황에 따라 객체의 수를 동적으로 정할 수 있다. 이는 프로그램의 공간적인 효율성 또한 높여준다.

---

### **Java Collections Framework(JCF)**

Java Collections Framwork(JCF)는 이러한 데이터, 자료구조인 컬렉션과 이를 구현하는 클래스를 정의하는 인터페이스를 제공한다.

다음은 JCF의 상속 구조를 나타낸다.

![https://blog.kakaocdn.net/dn/bsVerP/btrYCinsGXb/8KbpPDk2vl6KERnxPweQv0/img.png](https://blog.kakaocdn.net/dn/bsVerP/btrYCinsGXb/8KbpPDk2vl6KERnxPweQv0/img.png)

Collection 인터페이스는 List, Set, Queue로 크게 3가지 상위 인터페이스로 분류할 수 있다. 그리고 Map의 경우 Collection 인터페이스로 상속받고 있지 않지만 Collection으로 분류된다.

---

**1. List Interface**

> 순서가 있는 데이터의 집합으로 데이터의 중복을 허용한다.
>
- LinkedList

![https://blog.kakaocdn.net/dn/b2t9d9/btrYz0OEi4t/EiIspwaGiQzSLoagKgtPzK/img.png](https://blog.kakaocdn.net/dn/b2t9d9/btrYz0OEi4t/EiIspwaGiQzSLoagKgtPzK/img.png)

- 양방향 포인터 구조로 데이터의 삽입, 삭제가 빈번할 경우 데이터의 위치정보만 수정하면 되기에 유용하다.
- 스택, 큐, 양방향 큐 등을 만들기 위한 용도로 쓰인다.
- Vector

![https://blog.kakaocdn.net/dn/bI4SUG/btrYy9ZRLdU/nx0lP0Ge8Hy7Cs1RImipkK/img.png](https://blog.kakaocdn.net/dn/bI4SUG/btrYy9ZRLdU/nx0lP0Ge8Hy7Cs1RImipkK/img.png)

- 과거에 대용량 처리를 위해 사용했으며, 내부에서 자동으로 동기화 처리가 일어나 비교적 성능이 좋지 않고 무거워 잘 쓰이지 않는다.
- ArrayList와 동이한 내부구조를 보이나, 동기화에서의 차이가 존재할 뿐이다.

※ 동기화 - 프로세스(스레드)가 수행되는 시점을 조절하여 서로가 알고 있는 정보가 일치하는 것인데, 쉽게 말해 프로세스 간 데이터가 일치하도록 하는 것

- ArrayList

![https://blog.kakaocdn.net/dn/bv8uir/btrYBdzZryS/rfainQTAGxaERbMnIKRNs1/img.png](https://blog.kakaocdn.net/dn/bv8uir/btrYBdzZryS/rfainQTAGxaERbMnIKRNs1/img.png)

- 단방향 포인터 구조로 각 데이터에 대한 인덱스를 가지고 있어 조회 기능에 있어서 성능이 뛰어나다.
- ArrayList Vs Vector

Vector는 동기화된 메소드로 구성되어 있기 때문에 멀티 쓰레드가 동시에 이 메소드들을 실행할 수 없고, 하나의 쓰레드가 실행을 완료해야만 다른 쓰레드들이 실행할 수 있다. 드래서 멀티 스레드 환경에서 안전하게 객체를 추가하고 삭제할 수 있다.

하지만 벡터의 동기화는 장점이자 단점이 될 수 있다. 스레드가 1개일때도 동기화를 하기 때문에 ArrayList보다 성능이 떨어진다. ArrayList는 기본적인 기능은 벡터와 동일하나 자동 동기화 기능이 빠져있고, 동기화 옵션이 존재한다. 따라서 벡터의 비해 속도가 빠르기 때문에 벡터에 비해 많이 쓰이고 있다.

---

**2. Set Interface**

> 순서를 유지하지 않는 데이터의 집합으로 데이터의 중복을 허용하지 않는다.
>
- HashSet

<img src="https://blog.kakaocdn.net/dn/bXJsCb/btrYydIe11p/nLk0LsuwZ7SGuCFuyrmV40/img.png" width="400" height="300">

- 가장 빠른 임의 접근 속도
- 순서를 예측할 수 없다.
- null 값을 허용한다.
- TreeSet

![https://blog.kakaocdn.net/dn/buSb6Y/btrYy9etrTt/groDPumJZ9GREmPhQlx2E1/img.png](https://blog.kakaocdn.net/dn/buSb6Y/btrYy9etrTt/groDPumJZ9GREmPhQlx2E1/img.png)

- 정렬방법을 지정할 수 있다.
- 이진 탐색 트리 구조로 되어있다.(레드-블랙 트리)

※ 레드-블랙 트리(Red-Black Tree) - 부모노드보다 작은 값을 가지는 노드는 왼쪽 자식으로, 큰 값을 가지는 노드는 오른쪽 자식으로 배치하여 데이터의 추가나 삭제 시 트리가 한쪽으로 치우쳐지지 않도록 균형을 맞춘 이진 탐색 트리 구조 중 하나

---

**3. Map Interface**

> 키(Key), 값(Value)의 쌍으로 이루어진 데이터의 집합으로,
>
- HashMap

![https://blog.kakaocdn.net/dn/cdy1jE/btrYwmZxeRK/s3HOr2q2KYAEicXhPKZsR0/img.png](https://blog.kakaocdn.net/dn/cdy1jE/btrYwmZxeRK/s3HOr2q2KYAEicXhPKZsR0/img.png)

- 중복과 순서가 허용되지 않으며, null값이 올 수 있다.
- 만약 기존에 저장된 키와 동일한 키로 값을 저장하면 기존의 값은 없어지고 새로운 값으로 대치된다.
- HashMap은 많은 양의 데이터를 검색하는 데 있어서 뛰어난 성능을 보인다.
- Hashtable

![https://blog.kakaocdn.net/dn/brEVcr/btrYtRMcDlx/COSQNYDAMLPosgl7TSkfck/img.png](https://blog.kakaocdn.net/dn/brEVcr/btrYtRMcDlx/COSQNYDAMLPosgl7TSkfck/img.png)

- HashMap보다는 느리지만 동기화 지원
- null 불가
- TreeMap

![https://blog.kakaocdn.net/dn/TRkfr/btrYByD2E91/0BM5iMHE9RPGD7ovtjavv1/img.png](https://blog.kakaocdn.net/dn/TRkfr/btrYByD2E91/0BM5iMHE9RPGD7ovtjavv1/img.png)

- 정렬된 순서대로 키(Key)와 값(Value)을 저장하여 검색이 빠르다.

---

+

**4. Queue**

![https://blog.kakaocdn.net/dn/B3O5p/btrYzkNGITN/0kKx0mDDc09naSKDYBxpLK/img.png](https://blog.kakaocdn.net/dn/B3O5p/btrYzkNGITN/0kKx0mDDc09naSKDYBxpLK/img.png)

- 줄을 지어 순서대로 처리되는 것 (First In First Out)
- 즉, 가장 먼저 들어온 데이터가 가장 먼저 나가는 구조

Queue 선언

```java
import java.util.LinkedList;//importimport java.util.Queue;//import
Queue<Integer> queue = new LinkedList<>();//int형 queue 선언, linkedlist 이용
Queue<String> queue = new LinkedList<>();//String형 queue 선언, linkedlist 이용
```

자바에서 큐는 LinkedList를 활용해서 생성해야 한다. 그렇기에 Queue와 LinkedList가 다 import되어 있어야 사용이 가능하다. Queue<Element> queue = new LinkedList<>()와 같이 선언하면 된다.

- Priority Queue
- 높은 우선순위의 요소를 먼저 꺼내서 처리하는 구조이다.
- 내부 요소는 힙으로 구성되어 이진트리 구조로 이루어져 있다.
- 운선순위를 중요시해야 하는 상황에서 주로 쓰인다.

Priority Queue 선언

```java
import java.util.PriorityQueue;
import java.util.Collections;

//낮은 숫자가 우선 순위인 int 형 우선순위 큐 선언
PriorityQueue<Integer> priorityQueueLowest = new PriorityQueue<>();

//높은 숫자가 우선 순위인 int 형 우선순위 큐 선언
PriorityQueue<Integer> priorityQueueHighest = new PriorityQueue<>(Collections.reverseOrder());
```

---

참고

[https://coding-factory.tistory.com/550](https://coding-factory.tistory.com/550)

[https://gangnam-americano.tistory.com/41](https://gangnam-americano.tistory.com/41)

[https://velog.io/@gillog/Java-Priority-Queue%EC%9A%B0%EC%84%A0-%EC%88%9C%EC%9C%84-%ED%81%90](https://velog.io/@gillog/Java-Priority-Queue%EC%9A%B0%EC%84%A0-%EC%88%9C%EC%9C%84-%ED%81%90)