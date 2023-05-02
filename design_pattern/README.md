<details>
<summary>Table of Contents</summary>

- [SOLID](#solid)

</details>

# SOLID   
## 객체지향 설계과정   
1. 요구사항 (제공해야 할 기능) 을 찾고 세분화 한다. 그리고 그 기능을 알맞은 객체로 할당한다.
2. 기능을 구현하는 데에 필요한 데이터를 객체에 추가한다.
3. 해당 데이터를 이용하는 기능을 구현한다. (기능은 최대한 캡슐화)
4. 객체 간에 어떻게 메소드 호출을 주고받을 지 결정한다.   

## 객체지향 설계원칙   
## SRP (Single Responsibility) 단일 책임 원칙   
- 클래스는 단 한개의 책임을 가져야 함
- 클래스를 변경하는 이유는 단 하나여야 함
- 이를 지키지 않으면, 한 책임의 변경에 의해 다른 책임과 관련된 코드에 영향을 미칠 수 있음.   

> SRP 에서 이야기하는 책임이란, '기능' 정도로 생각하면 된다. 만약 한 클래스가 수행할 수 있는 기능 (책임) 이 여러 개라면, 클래스 내부의 함수끼리 강한 결합을 발생할 가능성이 높아진다. 응집도는 높고 결합도는 낮은 프로그램을 설계하는 것이 비로소 객체지향 설계의 핵심인데, 이것이 위반되는 것이다. 새로운 요구사항이나 프로그램 변경에 의해 클래스 내부의 동작들이 연쇄적으로 변경되어야 할 수도 있다. 이는 유지보수가 비효율적이므로, 책임을 잘게 쪼개어 분리시킬 필요가 있다.  
 
### SRP 원칙 위반 예제와 수정하기   
1. 회계팀에서 급여를 계산하는 메서드
2. 인사팀에서 근무시간을 계산하는 메서드
3. 기술팀에서 변경된 정보를 DB에 저장하는 메서드
4. 초과 근무 시간을 계산하는 메서드 (회계팀과 인사팀에서 공유하며 사용)   

를 가지고 있는 `Employee` 클래스가 있다.   

```Java
class Employee {
    int totalOverTimeHours; // 초과 근무 시간

    Employee(String name, String position) {
        this.totalOverTimeHours = 0;
    }

	int calculatePay() {
        // 재무팀에서 사용
        // 초과 근무 수당 계산
        // 기존 수당에 overtimeHours의 3만원을 곱한 값을 리턴한다.
        return defaultPay + totalOverTimeHours * 30000;
    }
    
    int reportHours(int overTimeHours) {
        // 인사팀에서 사용
        // 초과 근무 시간 보고
        totalOverTimeHours += overTimeHours;
        return totalOverTimeHours;
    }
}
```   

인사팀에서 초과 근무 시간을 수정해야하는 일이 생겼다. 할 일은 많은데 CEO는 계속 초과 근무 시간을 줄이라고 하는 것이다. `reportHours()` 함수의 `return totalOverTimeHours;`를 `return totalOverTimeHours - 3;` 처럼 수정하면 괜찮지만 실수로 `Employee`생성자의 `this.totalOverTimeHours = 0`를 `this.totalOverTimeHours = -3;`으로 수정했다. 여기서 문제는 `totalOverTimeHours`는 재무팀에서도 사용하기 때문에 `calculatePay()`에도 영향이 미치게된다. 따라서 근로자는 실제로 초과 근무한 시간에서 3시간을 제외한 부분을 인정받게 된다.   

물론 바보 같은 실수라고 생각할 수 있지만, 실제 코딩을 하게되면 이런 실수가 종종 나온다. 이런 실수가 나오는 이유는 `Employee` 클래스가 재무팀, 인사팀 2개의 액터에 대한 책임을 가지고 있기 때문이다.   

> 액터는 시스템을 수행하는 역할을 하는 요소로써, 시스템을 이용하는 사용자, 하드웨어 혹은 외부 시스템이 될 수 있다.
회계팀, 인사팀, 기술팀에서 데이터를 얻기위해 하나의 Employee 클래스를 사용하기 때문에, 3개의 액터가 하나의 클래스를 변경할 수 있는 요인이 되어 SRP 원칙을 어긴 구조라고 하는 것이다.   

이를 해결하기 위해서는 인사팀과 재무팀 `totalOverTimeHours` 변수를 수정할 수 없게 만들면 된다.   

```java
// * 통합 사용 클래스
class EmployeeFacade {
    int totalOverTimeHours; // 초과 근무 시간

    Employee(String name, String position) {
        this.totalOverTimeHours = 0;
    }
    
    // * 급여를 계산하는 메서드 (재무팀 클래스를 불러와 에서 사용)
    int calculatePay() {
        // ...
        new PayCalculator().calculatePay(totalOverTimeHours);
        // ...
    }

    // * 근무시간을 계산하는 메서드 (인사팀 클래스를 불러와 에서 사용)
    int reportHours(int overTimes) {
        // ...
        new HourReporter().reportHours(overTimes, totalOverTimeHours);
        // ...
    }
}

// * 재무팀에서 사용되는 전용 클래스
class PayCalculator {
    
    int calculatePay(int totalOverTimeHours) {
        // ...
        return defaultPay + totalOverTimeHours * 30000;
    }
}

// * 인사팀에서 사용되는 전용 클래스
class HourReporter {

    int reportHours(int overTimeHours, int totalOverTimeHours) {
        // 인사팀에서 사용
        // 초과 근무 시간 보고
        totalOverTimeHours += overTimeHours;
        return totalOverTimeHours;
    }
}
```   

재무팀에서 사용하는 전용 클래스인 `PayCalculator`와 인사팀에서 사용하는 전용 클래스 `HourReporter`를 만들었다. 이제 재무팀과 인사팀은 `EmployeeFacade` 클래스를 사용하지 않으므로 `totalOverTimeHours`가 변경될 일이 없다.   

> 위와 같은 구성을 디자인 패턴중 하나인 Facade 패턴이라고 말한다.
Facade란 건물의 정면을 의미한다. Facade Pattern은 말 그대로 건물의 뒷부분이 어떻게 생겼는지는 보여주지 않고 건물의 정면만 보여주는 패턴이다.
예를들어 EmployeeFacade 클래스는 메서드의 구현이 어떻게 되어있는지는(건물의 뒷부분) 보여주지 않고
어떤 메서드가 있는지(건물의 정면)만 보여준다.
구체적인 메서드의 구현은 각각 PayCalculator, HourReporter, EmployeeSaver 클래스에 위임하기 때문이다.   
 

## OCP (Open-Closed Principle) 개방 폐쇄 원칙   
- 기존의 코드를 변경하지 않으면서, 기능을 추가할 수 있도록 설계가 되어야 한다는 원칙
- 확장(새로운 기능 추가)에 대해서는 개방적(open)이고 수정에 대해서는 폐쇄적(closed)이어야 한다는 의미
   
어렵게 생각할 필요 없이, OCP 원칙은 **추상화**를 의미한다고 생각하면 된다.   

### OCP 원칙 위반 예제와 수정하기   
```java
class Animal {
	String type;
    
    Animal(String type) {
    	this.type = type;
    }
}

// 동물 타입을 받아 각 동물에 맞춰 울음소리를 내게 하는 클래스 모듈
class HelloAnimal {
    void hello(Animal animal) {
        if(animal.type.equals("Cat")) {
            System.out.println("냐옹");
        } else if(animal.type.equals("Dog")) {
            System.out.println("멍멍");
        }
    }
}

public class Main {
    public static void main(String[] args) {
        HelloAnimal hello = new HelloAnimal();
        
        Animal cat = new Animal("Cat");
        Animal dog = new Animal("Dog");

        hello.hello(cat); // 냐옹
        hello.hello(dog); // 멍멍
    }
}
```   

만약 '양'이나 '사자'를 추가하게 된다면 어떻게 될까?
HelloAnimal 클래스를 수정해줘야 한다.   

```java
public class Main {
    public static void main(String[] args) {
        HelloAnimal hello = new HelloAnimal();

        Animal cat = new Animal("Cat");
        Animal dog = new Animal("Dog");

        Animal sheep = new Animal("Sheep");
        Animal lion = new Animal("Lion");

        hello.hello(cat); // 냐옹
        hello.hello(dog); // 멍멍
        hello.hello(sheep); 
        hello.hello(lion);
    }
}

class HelloAnimal {
    // 기능을 확장하기 위해서는 클래스 내부 구성을 일일히 수정해야 하는 번거로움이 생긴다.
    void hello(Animal animal) {
        if (animal.type.equals("Cat")) {
            System.out.println("냐옹");
        } else if (animal.type.equals("Dog")) {
            System.out.println("멍멍");
        } else if (animal.type.equals("Sheep")) {
            System.out.println("메에에");
        } else if (animal.type.equals("Lion")) {
            System.out.println("어흥");
        }
        // ...
    }
}
```   

이런식으로 코드를 구성한다면, 동물이 추가될 때마다 계속 코드를 일일이 변경해줘야 한다.   

OCP 설계 원칙에 따라 적절한 추상화 클래스를 구성하고 이를 상속하여 확장시키는 관계로 구성하면 변경에는 닫히고 추가에는 열려있는 프로그램을 만들 수 있다.   

어떤 식으로 OCP대로 추상화 설계를 할 것인가에 대해서는 다음 규칙대로 이행하면 된다.   
1. 먼저 변경(확장)될 것과 변하지 않을 것을 엄격히 구분한다.
2. 이 두 모듈이 만나는 지점에 추상화(추상클래스 or 인터페이스)를 정의한다.
3. 구현체에 의존하기보다 정의된 추상화에 의존하도록 코드를 작성한다.   
   

```java
// 추상화
abstract class Animal {
    abstract void speak();
}

class Cat extends Animal { // 상속
    void speak() {
        System.out.println("냐옹");
    }
}

class Dog extends Animal { // 상속
    void speak() {
        System.out.println("멍멍");
    }
}

class HelloAnimal {
    void hello(Animal animal) {
        animal.speak();
    }
}

public class Main {
    public static void main(String[] args) {
        HelloAnimal hello = new HelloAnimal();

        Animal cat = new Cat();
        Animal dog = new Dog();

        hello.hello(cat); // 냐옹
        hello.hello(dog); // 멍멍
    }
}
```   

위와 같이 구성하게 되면 기능 추가가 되었을때도 코드 수정 없이 확장이 가능하게 된다.   

따라서 다음과 같이 양 클래스와 사자 클래스를 추가할 때 HelloAnimal 클래스의 코드 수정 없이 정상적으로 기능 확장이 되는 것을 보여주게 된다.   
```java
// 추상클래스를 상속만 하면 메소드 강제 구현 규칙으로 규격화만 하면 확장에 제한 없다 (opened)
class Sheep extends Animal {
    void speak() {
        System.out.println("매에에");
    }
}

class Lion extends Animal {
    void speak() {
        System.out.println("어흥");
    }
}

// 기능 확장으로 인한 클래스가 추가되어도, 더이상 수정할 필요가 없어진다 (closed)
class HelloAnimal {
    void hello(Animal animal) {
        animal.speak();
    }
}

public class Main {
    public static void main(String[] args) {
        HelloAnimal hello = new HelloAnimal();

        Animal cat = new Cat();
        Animal dog = new Dog();

        Animal sheep = new Sheep();
        Animal lion = new Lion();

        hello.hello(cat); // 냐옹
        hello.hello(dog); // 멍멍
        hello.hello(sheep); // 매에에
        hello.hello(lion); // 어흥
    }
}
```

## 참고 자료   
[https://velog.io/@haero_kim/SOLID-%EC%9B%90%EC%B9%99-%EC%96%B4%EB%A0%B5%EC%A7%80-%EC%95%8A%EB%8B%A4](https://velog.io/@haero_kim/SOLID-%EC%9B%90%EC%B9%99-%EC%96%B4%EB%A0%B5%EC%A7%80-%EC%95%8A%EB%8B%A4)   

[https://inpa.tistory.com/entry/OOP-%F0%9F%92%A0-%EC%95%84%EC%A3%BC-%EC%89%BD%EA%B2%8C-%EC%9D%B4%ED%95%B4%ED%95%98%EB%8A%94-SRP-%EB%8B%A8%EC%9D%BC-%EC%B1%85%EC%9E%84-%EC%9B%90%EC%B9%99](https://inpa.tistory.com/entry/OOP-%F0%9F%92%A0-%EC%95%84%EC%A3%BC-%EC%89%BD%EA%B2%8C-%EC%9D%B4%ED%95%B4%ED%95%98%EB%8A%94-SRP-%EB%8B%A8%EC%9D%BC-%EC%B1%85%EC%9E%84-%EC%9B%90%EC%B9%99)   

[https://selfish-developer.com/entry/SRP-Single-Responsibility-Principle](https://selfish-developer.com/entry/SRP-Single-Responsibility-Principle)   

[https://inpa.tistory.com/entry/OOP-%F0%9F%92%A0-%EC%95%84%EC%A3%BC-%EC%89%BD%EA%B2%8C-%EC%9D%B4%ED%95%B4%ED%95%98%EB%8A%94-OCP-%EA%B0%9C%EB%B0%A9-%ED%8F%90%EC%87%84-%EC%9B%90%EC%B9%99](https://inpa.tistory.com/entry/OOP-%F0%9F%92%A0-%EC%95%84%EC%A3%BC-%EC%89%BD%EA%B2%8C-%EC%9D%B4%ED%95%B4%ED%95%98%EB%8A%94-OCP-%EA%B0%9C%EB%B0%A9-%ED%8F%90%EC%87%84-%EC%9B%90%EC%B9%99)
