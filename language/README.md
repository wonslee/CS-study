<details>
<summary>Table of Contents</summary>

- [Java 와 JVM의 구성요소](#java-와-jvm)
- [Java Garbage Collection](#java-garbage-collection)
- [Java Collection](#java-collection)
- [Generic](#generic)
</details>


# **Java 와 JVM**

Java의 가장 큰 특징 중 하나가 어느 플랫폼, 어느 하드웨어(CPU)든, 어떤 운영체제(OS)이던 상관없이 컴파일된 코드가 플랫폼 독립적인 것이다.

다시 말해 어떤 필랫폼이든 작성한 소스를 변경할 필요 없이 다 실행할 수 있다는 것이다. 이 점이 웹 어플리케이션의 특성과 맞아 떨어져 폭발적인 인기와 함께 현재 웹 어플리케이션 개발에 가장 사용이 많이 되는 언어 중 하나가 되었다.

그리고 이 특징을 구현하기 위해 필요한 것이 바로 이 JVM이다.

![https://blog.kakaocdn.net/dn/EvIZw/btrXOScJVoi/h4TF2kiZ6KiloKFl4WGkk1/img.png](https://blog.kakaocdn.net/dn/EvIZw/btrXOScJVoi/h4TF2kiZ6KiloKFl4WGkk1/img.png)

예를 들어 C언어로 작성된 Test.c가 있다고 하면, 이 Test.c를 윈도우 컴파일러를 사용해서 컴파일하면 Test.exe가 만들어니다. 윈도우 컴파일러로 컴파일되었기에 Test.exe는 윈도우에서만 실행되는 실행 파일이다. 리눅스 운영체제에서는 실행할 수 없다. 즉 C / C++에서는 컴파일 플랫폼과 타겟 플랫폼이 다를 경우에는 프로그램이 동작하지 않는다. 만약 이 Test.exe 파일을 리눅스 운영체제에서 실행하려면 리눅스 환경을 타겟으로 크로스 컴파일을 해서 리눅스 운영체제에 맞는 실행 파일을 새로 만들어야 한다.

**반면**

![https://blog.kakaocdn.net/dn/my07N/btrXOwHNgJA/Sd3FPDh5MaGG8DlPGFod6k/img.png](https://blog.kakaocdn.net/dn/my07N/btrXOwHNgJA/Sd3FPDh5MaGG8DlPGFod6k/img.png)

Java의 경우에는 Java언어로 작성된 Test.java는 컴파일하면 Test.class 파일이 생성된다. 그리고 이렇게 생성된 바이트코드는 각자의 플랫폼에 설치되어 있는 자바 가상 머신(JVM)이 운영체제에 맞는 실행 파일로 바꿔준다. 즉 Java에서는 C언어와는 달리 JVM을 사용하기 때문에 각자의 플랫폼에 맞게끔 컴파일을 따로따로 해줘야 할 필요가 없다.

---

## **JVM(Java Virtual Machine)**

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

## **JVM 구성**

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

### PC Register

JVM의 PC Register는 CPU 내의 기억장치인 레지스터와 다르게 작동한다. PC Register는 각 쓰레드 별로 하나씩 존재하며 현재 수행 중인 JVM Instruction의 주소를 가지게 된다. 즉, 쓰레드가 어떤 명령을 실행할지 기록하는 부분이라고 할 수 있다.

※ 쓰레드 - 프로그램(프로세스) 실행의 단위이며 하나의 프로세스는 여러개의 쓰레드로 구성이 가능하다.


### Native Method Stack

자바 외의 언어로 작성된 네이티브 코드들을 위한 스택이다. Java Native Interface를 통해 호출되는 C/C++ 등의 코드를 수행한다.

### Method Area

모든 쓰레드가 공유하는 메모리 영역으로 클래스, 인터페이스, 메서드, 필드, Static 변수 등의 바이트 코드를 보관한다. Method Area에는 Runtime Constant Pool이라는 별도의 관리 영역도 존재한다. 이는 상수 자료형을 저장하고 참조하여 중복을 막는 역할을 수행한다.

### Stack

메소드(method)가 호출될 때 메서드와 메서드의 정보는 JVM Stack에 쌓이게 된다.   
즉 메서드의 매개변수(parameter), 지역 변수(local variable), return 주소, 임시 변수 등의 정보를 기록하는 스택이다. 각 쓰레드 별로 생성되기 때문에 다른 스레드에 접근할 수 없다. 메서드 호출이 종료되면 스택에서 정보들이 제거된다.

![https://s3.ap-northeast-2.amazonaws.com/yaboong-blog-static-resources/java/javamemory-stack-and-heap-dzone.jpg](https://s3.ap-northeast-2.amazonaws.com/yaboong-blog-static-resources/java/javamemory-stack-and-heap-dzone.jpg)

- **원시타입**(int, boolean, char 등)의 **데이터**가 값과 함께 할당된다.
- Heap 영역에 생성된 **객체 타입 변수의 참조값**이 할당된다.
- 지역변수들은 scope 에 따른 **visibility** 를 가진다.

  특정 함수 내부에 선언된 지역변수는 다른 함수에서 접근할 수 없다.  
  그리고 함수의 실행이 종료되면 그 지역변수들은 stack에서 pop되어 사라진다.

- 각 Thread 는 자신만의 stack 을 가진다. 각 스레드에서 다른 스레드의 stack 영역에는 접근할 수 없다.

실제 코드 예시:
```java
public class Main {
    public static void main(String[] args) {
        int x = 4;
        argument = divideByTwo(x);
    }

    private static int divideByTwo(int param){
        int result = param / 2;
        return result;
    }
}
```

![Untitled](./images/Untitled.png)




### Heap

![https://blog.kakaocdn.net/dn/YIKCC/btrXRUH2EN2/yIOS5RH8Ca25kCf2z2iON0/img.png](https://blog.kakaocdn.net/dn/YIKCC/btrXRUH2EN2/yIOS5RH8Ca25kCf2z2iON0/img.png)

Runtime 시점에 동적으로 할당하여 사용하는 영역이다. 클래스를 이용해 인스턴스를 생성하면 Heap에 저장된다. 즉, new 연산자를 이용해 생성된 객체를 저장하는 영역이다. Heap은 크게 New/Young 영역, Old 영역, Permanent Generation 3 영역으로 나뉜다.

※ 참고로 java8 이후에는 Permanent 영역이 Metaspace 영역으로 바뀌었다.

- 주로 긴 생명주기를 가지는 데이터들이 저장된다.
- 애플리케이션의 모든 **메모리 중 stack 에 있는 데이터를 제외한 부분**이라고 보면 된다.
- **모든 Object 타입**(Integer, String, ArrayList, ...)은 heap 영역에 생성된다.
- 몇개의 스레드가 존재하든 상관없이 단 하나의 heap 영역만 존재한다.
- Heap 영역에 있는 오브젝트들을 가리키는 레퍼런스 변수가 stack 에 올라가게 된다.

코드 예시 : 
```java
public class Main {
  public static void main(String[] args) {
    List<String> listArgument = new ArrayList<>();
    listArgument.add("yaboong");
    listArgument.add("github");
  }
}
```

![https://s3.ap-northeast-2.amazonaws.com/yaboong-blog-static-resources/java/java-memory-management_heap-5.png](https://s3.ap-northeast-2.amazonaws.com/yaboong-blog-static-resources/java/java-memory-management_heap-5.png)

---

참고

[https://code-lab1.tistory.com/92](https://code-lab1.tistory.com/92)

[https://steady-snail.tistory.com/67](https://steady-snail.tistory.com/67)

[https://aljjabaegi.tistory.com/387](https://aljjabaegi.tistory.com/387)


---


# **Java Garbage Collection**

Garbage Collection이란, 스택으로부터, Heap 영역 객체 중 도달 불가능한 객체들을 자동으로 메모리에서 제거해주는 개념이다. Java는 객체지향 언어인만큼, 힙을 사용하여 객체를 생성하는 경우가 굉장히 많다. 개발자들은 이렇게 힙을 자유롭게 사용하고, 더 이상 사용되지 않는 객체들은 가비지 컬렉션 과정에서 자동으로 메모리에서 제거된다.

그렇다면 다른 언어들은 어떠할까?

## **C언어**

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

## **가비지 컬렉션 대상**

가비지 컬렉션은 특정 객체가 Garbage인지 아닌지 판단하기 위해 도달성, 도달능력(Reachablily)이라는 개념을 적용한다.

객체에 레퍼런스가 있다면 Reachable로 구분되고, 객체의 유효한 레퍼런스가 없다면 Unreachable로 구분해 버리고 수거해버린다.

※ Reachable : 객체가 참조되고 있는 상태

※Unreachable : 객체가 참조되고 있지 않은 상태 즉 GC의 대상

![https://blog.kakaocdn.net/dn/bnAUxR/btrYjldFSCJ/plnKnfWdO0usRSk2fmg5L1/img.png](https://blog.kakaocdn.net/dn/bnAUxR/btrYjldFSCJ/plnKnfWdO0usRSk2fmg5L1/img.png)

JVM메모리에서는 객체들은 실질적으로 Heap영역에 생성되고 Method Area나 Stack Area에서는 Heap Area에 생성된 객체의 주소만 참조하는 형식으로 구성된다.

특정 이벤트 등으로 인하여 Heap Area 객체의 메모리 주소를 가지고 있는 참조 변수가 삭제되는 현상이 발생하면, 참조되고 있지 않은 객체 (Unreachable)이 발생한다. 그리고 GC는 이러한 것들을 주기적으로 제거하는 역할을 한다.

---

## **가비지 컬렉션 청소 방식**

**1. Stop The World**

Stop The World는 가비지 컬렉션을 실행하기 위해 JVM이 애플리케이션의 실행을 멈추는 작업이다. GC가 실행될 때는 GC를 실행하는 쓰레드를 제외한 모든 쓰레드들의 작업이 중단되고, GC가 완료되면 작업이 재개된다. 당연히 모든 쓰레드들의 작업이 중단되면 애플리케이션이 멈추기 때문에, GC의 성능 개선을 위해 튜닝을 한다고 하면 보통 stop-the-world의 시간을 줄이는 작업을 하는 것이다. 또한 JVM에서도 이러한 문제를 해결하기 위해 다양한 실행 옵션을 제공한다.

**2. Mark and Sweep 알고리즘**

- Mark : 사용되는 메모리와 사용되지 않는 메모리를 식별하는 작업
- Sweep : Mark 단계에서 사용되지 않음으로 식별된 메모리를 해제하는 작업
- Compaction : Sweep 후에 분산된 객체들을 Heap의 시작 주소로 모아 메모리가 할당된 부분과 그렇지 않은 부분으로 압축한다.

![https://blog.kakaocdn.net/dn/lDidk/btrX41aqZIq/uO17AnUY2ng9BWrnVZ84f0/img.png](https://blog.kakaocdn.net/dn/lDidk/btrX41aqZIq/uO17AnUY2ng9BWrnVZ84f0/img.png)

---

## **Minor GC와 Major GC**

JVM의 힙 영역은 동적으로 레퍼런스 데이터가 저장되는 공간으로, 가비지 컬렉션에 대상이 되는 공간이다.

![https://blog.kakaocdn.net/dn/bkOeuE/btrYiVGrNuk/EOwGG969FbKDa5EoKq4QN1/img.png](https://blog.kakaocdn.net/dn/bkOeuE/btrYiVGrNuk/EOwGG969FbKDa5EoKq4QN1/img.png)

## **Young 영역 (Young Generation)**

- 새롭게 생성된 객체가 할당되는 영역
- 대부분의 객체가 금방 Unreachable 상태가 되기 때문에, 많은 객체가 Young 영역에 생성되었다가 사라진다.
- Young 영역에 대한 가비지 컬렉션을 Minor GC라고 부른다.

## **Old 영역 (Old Generation)**

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

## **GC 발생 시나리오**

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



---

# **Generic**

```java
List<Integer> list = new ArrayList<>();
Map<String, Integer> map = new HashMap<>();
```

우리는 위의 예제와 같이 클래스 타입이 명시된 패턴을 자주 발견할 수 있다. 이걸 **제네릭(Generic)** 이라고 부르며, 제네릭 파라미터는 꺽쇠안에 포함하여 전달한다.

**JAVA에서 제네릭이란?**

- 파라미터 타입이나 리턴 타입에 대한 정의를 외부로 미룬다.
- 타입에 대해 유연성과 안정성을 확보한다.
- 런타임 환경에 아무런 영향이 없는 컴파일 시점의 전처리 기술이다.

**제네릭을 왜 사용할까?**

> 타입을 유연하게 처리하며, 잘못된 타입 사용으로 발생할 수 있는 런타임 타입 에러를 컴파일 과정에 검출한다.
>

제네릭을 사용하면 실수로 지정한 타입이 들어오는 경우 컴파일 시점에서 미리 예방할 수 있게 된다. 또한 클래스 외부에서 데이터 타입을 지정하기 때문에, 타입을 고려해서 이리저리 변환할 필요가 없다. 따라서 코드의 재사용성이 높아지고 전체 코드 관리가 용이해진다.

```java
List v = new ArrayList();
v.add("test");// A String that cannot be cast to an Integer
Integer i = (Integer)v.get(0);// Run time error
```

위와 같은 코드는 런타임 에러를 발생한다. List에 String을 저장하고 Integer로 타입 변환하는 오류가 런타임에 잡히는 것이다.

```java
List<String> v = new ArrayList<String>();
v.add("test");
Integer i = (Integer)v.get(0);// (type error)  compilation-time error
```

하지만 위와 같이 제네릭을 이용하면 컴파일 에러를 발생시켜 컴파일 시에 오류를 잡아낼 수 있다. 런타임 에러보다 컴파일 에러가 훨씬 디버깅이 쉽기 때문에 이점이 많다.

또한 제네릭에서는 <>안에서 데이터 타입을 자유롭게 설정할 수 있다. 특정 자료구조를 만들 때 Integer, String, Boolean 등 데이터 타입에 따라 다른 자료구조를 만든다면 매우 비효율적이다. 제네릭은 이러한 상황일 때 굉장히 효율적이다.

```java
ArrayList list = new ArrayList();//제네릭을 사용하지 않을경우
list.add("test");
String temp = (String) list.get(0);//타입변환이 필요함

ArrayList<String> list2 = new ArrayList();//제네릭을 사용할 경우
list2.add("test");
temp = list2.get(0);//타입변환이 필요없음
```

### **제네릭에서 많이 사용되는 문자열들**

이 문자열들은 개발자들이 암묵적으로 사용하는 코드일 뿐이지 사용자가 임의로 지정한 문자열을 넣어도 제네릭이 동작하는데에 아무런 영향을 미치지 않는다.

![https://blog.kakaocdn.net/dn/dNXr6S/btrZqTn9qHj/07kKXQizxwJlMTyTeTZeZK/img.png](https://blog.kakaocdn.net/dn/dNXr6S/btrZqTn9qHj/07kKXQizxwJlMTyTeTZeZK/img.png)

---

### **예시**

### **제네릭 클래스**

```java
class ExampleGeneric<T> {
    private T t;

    public void setT(T t) {
        this.t = t;
    }

    public T getT() {
        return t;
    }
}
```

클래스를 설계할 때 구체적인 타입을 명시하지 않고 타입 파라미터로 넣어두었다가 실제 설계한 클래스가 사용될 때 ExampleGeneric<String> YD = New ExampleGeneric<>(); 이런식으로 구체적인 타입을 지정하면서 사용하면 타입 변환을 최소화 할 수 있다.

---

### **제네릭 인터페이스**

```java
interface ExampleGeneric<T> {
    T example();
}

class ExGeneric implements ExampleGeneric<String> {

// ExampleGeneric<String> <- String으로 넘어왔기 때문에 example()의 반환값이 String이여야만 한다.@Override
    public String example() {
        return "YD";
    }
}
```

---

### **멀티 타입 파라미터 사용**

```java
class ExMultiTypeGeneric<K, V> implements Map.Entry<K,V>{

    private K key;
    private V value;

    @Override
    public K getKey() {
        return this.key;
    }

    @Override
    public V getValue() {
        return this.value;
    }

    @Override
    public V setValue(V value) {
        this.value = value;
        return value;
    }
}
```

타입은 두 개 이상의 멀티 타입 파라미터를 사용할 수 있고, 이 경우 각 타입 파라미터를 콤마로 구분한다.

---

### **제네릭 메소드**

**선언**

```java
// 제네릭 메소드 선언// 매개 변수 타입: T// 리턴 타입: Box<T>public <T> Box<T> boxing(T t) {
    Box<T> box = new Box<T>();
    box.set(t);
    return box;
}
```

먼저 리턴 타입(int, boolean, String, Box<T> 등등) 앞에 <T>와 같이 타입 파라미터를 기술한 다음,

매개 변수에 제네릭 타입 파라미터를 기입하면 된다.

**호출**

```java
public class BoxingMethodExample {
	public static void main(String[] args) {
		Box<Integer> box1 = <Integer>boxing(100);
		int intValue = box1.get();

		Box<String> box2 = boxing("YD");
		String strValue = box2.get();
	}
}
```

메서드를 호출 할 때, <Integer> boxing(100); 처럼 타입 파라미터를 구체적으로 명시하여도 되고, 단순히 box(100);으로 작성하여도 타입 파라미터를 Integer로 추정하여 대입하게 된다.

**제한된 타입 파라미터**

```java
// extends를 이용해 타입 제한public static <T extends Number> int compare(T t1, T t2) {
		double v1 = t1.doubleValue();
		double v2 = t2.doubleValue();

		return Double.compare(v1, v2);
}
```

위 제네릭 메소드에서 int 앞에 <T extends Number>라는 부분이 의미하는 바는

Number 클래스 또는 Number 클래스의 하위 타입 클래스만 사용할 수 있음을 의미한다.

그러므로 이 compare 제네릭 메소드에 String이나 기타 다른 타입이 삽입되는 것을 제한 할 수 있게 된다.

**주의할 점**

1. 상위 타입은 클래스뿐만 아니라 인터페이스도 가능하다. 하지만 그렇다고 extends를 implements라고 쓰진 않는다.

2. 제네릭 메소드의 중괄호 {} 안에서 타입 파라미터 변수로 가능한 것은 상위 타입의 멤버(필드, 메소드)로 제한한다.

- 즉 <T extends Number>라고 제네릭 메소드의 타입을 제한했다면, 메소드의 실행 부분에서 Number 타입의 필드와 메소드만 사용할 수 있다는 뜻이다. Integer에는 있고 Number에는 없는 필드나 메소드는 사용할 수 없다.

---

### **제네릭 와일드 카드**

와일드카드는 물음표 ?로 표시하며 Java에서 unknown type이다. 와일드카드는 매개변수, 필드 또는 지역 변수의 유형 때론 반환 유형으로 다양한 상황에서 사용할 수 있다.

와일드카드 타입에는 총 세가지의 형태가 있으며 물음표(?)라는 키워드로 표현된다.

제네릭타입<?> : 타입 파라미터를 대치하는 것으로 모든 클래스나 인터페이스 타입이 올 수 있다.

제네릭타입<? extends 상위타입> : 와일드카드의 범위를 특정 객체의 하위 클래스만 올 수 있다.

제네릭타입<? super 하위타입> : 와일드카드의 범위를 특정 객체의 상위 클래스만 올 수 있다.

---

**출처**

[https://coding-factory.tistory.com/573](https://coding-factory.tistory.com/573)

[https://hahahoho5915.tistory.com/69](https://hahahoho5915.tistory.com/69)

[https://jehuipark.github.io/java/java-generic](https://jehuipark.github.io/java/java-generic)