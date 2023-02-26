<details>
<summary>Table of Contents</summary>

- [프로세스와 스레드](#프로세스와-스레드)
- [동기와 비동기 vs 블락킹과 논블락킹](#동기와-비동기-vs-블락킹과-논블락킹)
- [뮤텍스](#뮤텍스와-세마포어)
</details>

## 프로세스와-스레드
### 참고 자료
[자료 1](https://velog.io/@aeong98/%EC%9A%B4%EC%98%81%EC%B2%B4%EC%A0%9COS-%ED%94%84%EB%A1%9C%EC%84%B8%EC%8A%A4%EC%99%80-%EC%8A%A4%EB%A0%88%EB%93%9C) [자료 2](https://zangzangs.tistory.com/108) [자료 3](https://m.blog.naver.com/merongvvvv/222038241661) [자료 4](https://m.blog.naver.com/merongvvvv/222039850144) [자료 5](https://velog.io/@raejoonee/%ED%94%84%EB%A1%9C%EC%84%B8%EC%8A%A4%EC%99%80-%EC%8A%A4%EB%A0%88%EB%93%9C%EC%9D%98-%EC%B0%A8%EC%9D%B4)

### 프로세스(Process)
#### 프로세스가 도대체 뭐야?
> * 실행 중에 있는 프로그램   
> * 하드디스크에 있는 프로그램을 실행하면, 실행을 위해서 메모리 할당이 이루어지고, 할당된 메모리 공간으로 바이너리 코드가 올라가게 된다. 이 순간부터 프로세스라 불린다.   

#### 프로세스의 문맥(context)
> 프로세스가 현재 어떤 상태에서 수행되고 있는지 정확히 규명하기 위해 필요한 정보   
 
**왜 필요한건데?**   
현대의 운영체제는 여러 프로세스가 함께 수행되는 시분할 시스템 환경이다. 시분할 시스템 환경에서는 타이머 인터럽트에 의해 짧은 시간동안 CPU를 점유하고 다른 프로세스에게 넘겨주고 다시 차례가 되면 CPU를 점유하여 명령을 수행합니다. 다시 명령을 수행하기 위해서 이전에 어디까지 명령을 수행했는지 정확한 수행 시점과 상태를 재현할 수 있는 정보가 필요했습니다. 그 필요한 정보가 바로 프로세스 문맥(process context)입니다.   

    ex) 카카오톡을 켜놓고 유튜브로 노래를 들으면서 웹서핑을 하는 것은 사용자 입장에서 동시에 일어나는 일처럼 보이지만 실제로는 그렇지 않음.   

![플래시](https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcS2ewSMyMQMz9DtaG4gw6kIbfo9Cejte_QCuA&usqp=CAU)   
(CPU는 마치 ... 플래시 같은 느낌 ...)


**문맥 종류**   
1.  CPU 수행상태를 나타내는 하드웨어 문맥 : 현시점에서 이 프로세스가 'instruction(기계어)을 어디까지 수행했는가?'를 알기 위한 요소. (PC, 각종 register:주로 register가 현재 어떤 값을 갖고 있었는가 를 나타냄)   
2. 프로세스 주소공간 : 현 시점의 'code, data, stack에 어떤 내용이 들어 있는가?' 를 알아야 이 프로세스의 현재 상태를 정확히 알 수 있다.   
3. 프로세스 관련 커널 자료구조 : 프로세스 하나 생길 때마다 OS는 그 프로세스를 관리하기 위해 자신의 data 구조에 자료구조(PCB)를 두고있다. 즉, PCB에 얼마나 CPU와 메모리를 줘야 할지, 나쁜 짓을 하고있진 않은지 관리하는 역할. (OS는 지금 돌아가는 프로그램을 관리하는 역할)   

#### 프로세스의 상태(State)
* 프로세스는 상태(state)가 변경되며 수행된다.
    * Running
        * CPU 를 잡고 instruction 을 수행중인 상태
    * Ready
        * CPU 를 기다리는 상태
    * Blocked (waiting, sleep)
        * CPU를 주어도 당장 instruction 을 수행할 수 없는 상태
        * Process 자신이 요청한 event(예: I/O) 가 즉시 만족되지 않아, 이를 기다리는 상태
        * (예) 디스크에서 file 을 읽어와야 하는 경우
    * New : 디스크에서 메모리로 프로그램이 올라가 실행준비를 하는 상태
    * Terminated : 수행 (execution)이 끝난 상태

#### 문맥 교환 (Context Switch)
> 한 프로세스에서 다른 프로세스로 CPU의 제어권을 넘겨주는 과정(단, OS로 넘어가는 것은 문맥 교환이 아니다. - 하나의 프로세스가 사용자 모드에서 실행되다가, 커널 모드로 실행 모드만 바뀌는 것일 뿐 CPU 를 점유하는 프로세스가 다른 사용자 프로세스로 변경되는 과정이 아니기 떄문)   

     어플리케이션 프로그램이 수행되는 모드는 유저모드입니다. 
     프로그램이 수행되다가 인터럽트 걸려서 운영체제가 호출돼 수행되는 모드는 커널모드

     
커널모드 특징

- 시스템의 모든 메모리에 접근 할 수 있고 모든 CPU 명령을 실행 할 수 있다.
- 운영체제 코드나 디바이스 드라이버 같은 커널 모드 코드를 실행한다.
- CPU는 커널 모드 특권 수준에서 코드를 실행한다.


유저모드 특징

- 사용자 애플리케이션 코드가 실행한다.
- 시스템 데이터에 제한된 접근만이 허용되며 하드웨어를 직접 접근 할 수 없다.
- 유저 애플리케이션은 시스템 서비스 호출을 하면 유저모드에서 커널 모드로 전환된다.
- CPU 유저 모드 특권 수준으로 코드를 실행한다
- 유저모드에서 실행하는 스레드는 자신만의 유저모드 스택을 가진다. 

> 모드 변경이 필요한 이유 : 시스템 메모리에 함부로 접근하지 못하도록 하기 위함. 시스템 메모리를 잘못 건드리면 오류가 발생할 수 있다.

<br>

![](https://mblogthumb-phinf.pstatic.net/MjAyMDA3MjJfMTA2/MDAxNTk1MzkxNTI3NjU5.HIrzeAJLacDUQDPnP_vhoNuYTzqO8ubuVMKxg5Ee1_Qg.sYkTesnJoAkCMgJcCeQrWdm5BWpYbqgTtFZWAJGiaTYg.PNG.merongvvvv/image.png?type=w800)   

현재 프로세스 A 가 CPU 를 사용하고 있는 상황에서 CPU 사용시간이 끝나, 다음 프로세스에게 CPU 를 넘겨주어야 합니다. 스케줄링 알고리즘에 의해 다음 CPU 를 받을 프로세스B 가 선택되었고, 타이머 인터럽트가 발생해 CPU 제어권이 운영체제 커널에 넘어가게 됩니다.   

이 과정에서 운영체제는 타이머 인터럽트 처리 루틴으로 가서 직전까지 수행중이던 프로세스 A 의 문맥을 자신의 PCB 에 저장하고, 프로세스 B 는 예전에 저장했던 자신의 문맥을 PCB로부터 실제 하드웨어로 복원 시키는 과정을 거치게 된다.   

CPU가 동시에 여러개의 프로세스를 실행시키는 것처럼 보이지만, 사실은 CPU가 재빠르게 여러 프로세스를 번갈아가며 실행하고 관리하고 있는것. 이때 프로세스를 번갈아가면서 처리하는 것을 Context Switching(문맥교환)이라고 한다.   

### 스레드
> 프로세스 내에서 실행되는 여러 흐름의 단위. 프로세스의 처리속도를 높이기 위해 프로세스가 수행해야 할 여러 작업들을 나누어 수행 할 수 있도록 설계한 것   

### 프로세스 VS 스레드
운영체제는 프로세스마다 각각 독립된 메모리 영역을, Code/Data/Stack/Heap의 형식으로 할당해 준다. 각각 독립된 메모리 영역을 할당해 주기 때문에 프로세스는 다른 프로세스의 변수나 자료에 접근할 수 없다.

![](https://velog.velcdn.com/images%2Fraejoonee%2Fpost%2Fb7939911-f3e8-48ac-abb8-63d8a17d0444%2F103.png)   

이와 다르게 스레드는 메모리를 서로 공유할 수 있다고 언급했었다. 이에 대해 더 자세히 설명하자면, 프로세스가 할당받은 메모리 영역 내에서 Stack 형식으로 할당된 메모리 영역은 따로 할당받고, 나머지 Code/Data/Heap 형식으로 할당된 메모리 영역을 공유한다. 따라서 각각의 스레드는 별도의 스택을 가지고 있지만 힙 메모리는 서로 읽고 쓸 수 있게 된다.   

![](https://velog.velcdn.com/images%2Fraejoonee%2Fpost%2Fb91490ed-c67b-407d-8fea-a8d6fdb22559%2F104.png)   

여기서 프로세스와 스레드의 중요한 차이를 하나 더 알 수 있게 된다. 만약 한 프로세스를 실행하다가 오류가 발생해서 프로세스가 강제로 종료된다면, 다른 프로세스에게 어떤 영향이 있을까? 공유하고 있는 파일을 손상시키는 경우가 아니라면 아무런 영향을 주지 않는다.

그런데 스레드의 경우는 다르다. 스레드는 Code/Data/Heap 메모리 영역의 내용을 공유하기 때문에 어떤 스레드 하나에서 오류가 발생한다면 같은 프로세스 내의 다른 스레드 모두가 강제로 종료된다.   

#### 프로세스가 다른 프로세스의 정보에 접근하는 방법   
프로세스 간 정보를 공유하는 방법에는 다음과 같은 방법들이 있다. 다만 이 경우에는 단순히 CPU 레지스터 교체뿐만이 아니라 RAM과 CPU 사이의 캐시 메모리까지 초기화되기 때문에 앞서 말했듯 자원 부담이 크다.

1. IPC(Inter-Process Communication)을 사용한다.
2. LPC(Local inter-Process Communication)을 사용한다.
3. 별도로 공유 메모리를 만들어서 정보를 주고받도록 설정해주면 된다.   

#### 멀티 스레드
> 하나의 프로세스가 여러 작업을 여러 스레드를 사용하여 동시에 처리하는 것   

> **멀티스레드의 장점**
> 
> 1. Context-Switching할 때 공유하고 있는 메모리만큼의 메모리 자원을 아낄 수 있다.
> 2. 스레드는 프로세스 내의 Stack 영역을 제외한 모든 메모리를 공유하기 때문에 통신의 부담이 적어서 응답 시간이 빠르다.   
>
> **멀티스레드의 단점**
>
> 1. 스레드 하나가 프로세스 내 자원을 망쳐버린다면 모든 프로세스가 종료될 수 있다.
> 2. 자원을 공유하기 때문에 필연적으로 ***동기화 문제***가 발생할 수밖에 없다.

## 동기와 비동기 vs 블락킹과 논블락킹
[참고 자료 1](https://inpa.tistory.com/entry/%F0%9F%91%A9%E2%80%8D%F0%9F%92%BB-%EB%8F%99%EA%B8%B0%EB%B9%84%EB%8F%99%EA%B8%B0-%EB%B8%94%EB%A1%9C%ED%82%B9%EB%85%BC%EB%B8%94%EB%A1%9C%ED%82%B9-%EA%B0%9C%EB%85%90-%EC%A0%95%EB%A6%AC) [참고 자료 2](https://evan-moon.github.io/2019/09/19/sync-async-blocking-non-blocking/#%EB%8F%99%EA%B8%B0-%EB%B0%A9%EC%8B%9D--%EB%85%BC%EB%B8%94%EB%A1%9D%ED%82%B9-%EB%B0%A9%EC%8B%9D) [참고 자료 3](http://homoefficio.github.io/2017/02/19/Blocking-NonBlocking-Synchronous-Asynchronous/) [참고 자료 4](https://velog.io/@codemcd/Sync-VS-Async-Blocking-VS-Non-Blocking-sak6d01fhx)   

### 동기 & 비동기
여러 개의 함수들이 ***시간을 맞춰서 실행되느냐***에 따라 구분

> * **동기**   
>  함수가 두 개 이상 존재할 때, 이 함수들이 작업을 동시에 시작하거나, 끝나는 타이밍을 맞추거나, 하나가 끝나고 다른 하나를 차례로 실행하는 것
>
> * **비동기**   
> 두 함수는 서로가 언제 시작하고, 언제 일을 마치는지 전혀 신경쓰지 않는 것   
 
### 블로킹 & 논블로킹
> * **제어권**  
> 자신(함수)의 코드를 실행할 권리 같은 것. 제어권을 가진 함수는 자신의 코드를 끝까지 실행한 후, 자신을 호출한 함수에게 돌려준다.   
> * **블로킹**   
> 다른 함수를 호출할 때, 제어권도 아예 함께 넘겨주고 작업이 끝난 후에 돌려받는 방식   
> * **논블로킹**   
> 호출할 때 제어권을 넘겨주기는 하지만, 바로 돌려받는다.   

### 요약
> 동기/비동기 : 시간을 맞추냐, 안맞추냐   
> 블로킹/논블로킹 : 제어권을 넘겨주냐, 안넘겨주냐   
 
백문이 불여일견! 코드로 직접 한번 봐보자!   
```javascript
function employee() {
  for (let i = 1; i < 101; i++) {
    console.log(`직원: 인형 눈알 붙히기 ${i}번 수행`);
  }
}
 
function boss() {
  console.log('사장: 출근');
  employee();
  console.log('사장: 퇴근');
}
 
boss();
```
    사장: 출근
    직원: 인형 눈알 붙히기 1번 수행
    직원: 인형 눈알 붙히기 2번 수행
    ...
    직원: 인형 눈알 붙히기 100번 수행
    사장: 퇴근   

* 블락킹일까? 논블락킹일까?
1. `boss()`함수 내에서 `employee()`함수가 실행되면 `boss()`함수의 *제어권*이 `employee()`로 넘어간다.
2. `employee()`함수는 종료된 이후에 제어권을 다시 `boss()`함수로 넘겨준다.   
-> 음! 제어권을 넘겨주고 다시 받았으니 *블락킹* 이구나!

* 동기일까? 비동기일까?
1. `boss()`함수가 실행되다가 `employee()`함수가 실행된다.
2. `employee()`함수가 종료된 이후에 `boss()`함수가 마저 실행되고 종료된다.
-> 음! 두 개의 함수가 순차적으로 실행되니 *동기* 이구나!   

즉 위 코드는 **동기-블락킹**이다.

다음 문제~!   
```c
int main() {
    Future ft = asyncFileChannel.read(~~~); //논블락킹 함수 - 대략 60분 동안 실행된다고 가정

    while(!ft.isDone()) {
        // isDone()은 asyncChannle.read() 작업이 완료되지 않았다면 false를 바로 리턴해준다.
        // isDone()은 물어보면 대답을 해줄 뿐 작업 완료를 스스로 신경쓰지 않고,
        // isDone()을 호출하는 쪽에서 계속 isDone()을 호출하면서 작업 완료를 신경쓴다.
        // asyncChannle.read()이 완료되지 않아도 여기에서 다른 작업 수행 가능 
    }
}
// 작업이 완료되면 작업 결과에 따른 다른 작업 처리
```   
그냥 블로그에서 퍼온 코드이다...! 주석을 자세히 읽으면 동기/비동기, 블락킹/논블락킹을 판단할 수 있다.   

`asyncFileChannel.read()`함수는 *논블락킹* 함수이다. 즉 `main()` 함수는 제어권을 `asyncFileChannel.read()`함수로 넘겨준 뒤 바로 돌려받는다.   

`isDone()`은 단순히 `asyncFileChannel.read()`함수의 작업이 완료되었는지 파악하는 함수이다. `isDone()`이 `true`일 때, 즉 `asyncFileChannel.read()`가 종료된 이후에 `main()`함수의 나머지 부분이 실행되므로 *동기*이다.   

-> 정답! **논블락킹-동기**    



선생님, 다음 문제는 **블락킹-비동기** 예제 인가요?? ㅋㅋ    
아쉽게도 **블락킹-비동기**에 대한 예제 코드는 ... 없다 ... ! 대신 언제 블락킹-비동기가 발생하는지 서술하겠다.    

> Asynchronous Non-Blocking 모델 중에서 Blocking 으로 동작하는 작업이 있는 경우 의도와 다르게 Asynchronous Blocking으로 동작할 때가 있다고 한다.    
> 
> 대표적인 예로는 Node.js와 MySQL을 함께 사용하는 경우이다. Node.js는 비동기로 작업하려 하지만 MySQL 드라이버가 Blocking 방식으로 동작하므로 어쩔 수 없이 Asynchronous Blocking 방식으로 동작하게 된다.

<br>

후후 ... 분명 블락킹/논블락킹, 동기/비동기에 대한 설명을 들으면서 블락킹 == 동기, 비동기 == 논블락킹 아냐? 라는 생각을 했을 것이다 ... ! 하지만 위 예시를 보면 전혀 아니라는 것을 알 수 있다!   


```c
int main() {
    Future ft1 = asyncFileChannel.read(A); //논블락킹 함수 - 5분 동안 실행
    Future ft2 = asyncFileChannel.read(B); //논블락킹 함수 - 10분 동안 실행
}
```   

위 코드는 ... 어디보자 `asyncFileChannel.read(A)`, `asyncFileChannel.read(B)` 모두 논블락킹 함수이고 2개의 함수는 시간에 맞춰 실행되지 않으므로 비동기 ... !    
-> 정답! **논블락킹-비동기**!   

아래는 실제 프로젝트에서 동기 처리를 해주지 않아 생긴 문제이다.
### Access Token 갱신 요청이 동시에 2번 발생하면서 생긴 문제

### 🚨 ***Issue***

`okhttp3.Authenticator`를 사용하여 서버에서 내려온 `HTTP Status Code`가 `401`인 경우 자동으로 `Access Token` 갱신 요청을 하도록 만들어놨습니다.

1. 유저의 `Access Token`이 만료된 상태에서 `Access Token`이 필요한 api 2개를 비동기 호출
2. `okhttp3.Authenticator`에 의해 `Access Token Refresh Api`가 2번 비동기 호출
3. 2개의 `Access Token Refresh Api(ㄱ, ㄴ)`의 `Body`에는 `A Refresh Token` 값이 들어감
4. `ㄱ Api` 호출에서 `A Refresh Token`이 갱신되어 `B Refresh Token`으로 변경됨
1. `ㄴ Api`가 호출되지만 이미 `ㄱ  Api` 호출에서 서버 DB에 저장된 유저의 `Refresh Token` 값이 바뀌었기 때문에 토큰 갱신 실패

### ✅ ***Solution***
Api 호출을 동기 처리하여 문제를 해결했습니다.
```kotlin
//ViewModel.kt

// 서버에서 강의 평가의 통합 정보를 불러온다. 요청이 성공했을 경우 onSuccess()를 실행한다.
suspend fun fetchLectureIntegratedInfo(onSuccess: () -> Unit) {
        val response = lectureInfoRepository.getLectureDetailInfo(pageViewModel.lectureId)
        when {
            response.isSuccessful -> {
                response.body()?.data?.let { data ->
                    _lectureDetailInfoData.value = data
                    onSuccess()
                }
            }
            else -> {
                handleError(response.code())
            }
        }
    }

// 서버에서 강의평가 또는 시험정보 리스트를 불러온다.
fun fetchLectureList() {
        if(pageViewModel.page.value!! == LAST_PAGE)
            return
        when (_writeBtnText.value) {
            R.string.write_evaluation -> getEvaluationList()
            else -> getExamList()
        }
    }
```
```kotlin
//Fragment.kt
lifecycleScope.launch {
	lectureInfoViewModel.fetchLectureIntegratedInfo {
        lectureInfoViewModel.fetchLectureList()
   }
}
```   

## 뮤텍스와 세마포어
[참고 자료 1](https://hoyeonkim795.github.io/posts/mutex-semaphore/)   
[참고 자료 2](https://velog.io/@heetaeheo/%EB%AE%A4%ED%85%8D%EC%8A%A4Mutex%EC%99%80-%EC%84%B8%EB%A7%88%ED%8F%AC%EC%96%B4Semaphore%EC%9D%98-%EC%B0%A8%EC%9D%B4)   
[참고 자료 3](https://suintodev.tistory.com/4)   

***

여러 프로세스가 동시에 공유 데이터에 접근할 때 접근 순서에 따라 실행 결과가 달라지는 상황이 있다. 이런 상황에 놓인 프로세스들을 **경쟁 상태(race condition)**에 있다고 한다. 이러한 경쟁 상태를 예방하려면 병행 프로세스들을 동기화해야 하는데, 이는 임계 영역을 이용한 **상호배제**로 구현할 수 있다.   

> * 임계 영역   
> 여러 프로세스가 데이터를 공유하며 수행될 때, 각 프로세스에서 공유 데이터에 접근하는 프로그램 코드 부분   


다만, 임계 영역만으로 경쟁 조건이 만들어지지 않을 때가 있다. 공유 메모리를 사용하는 병렬 프로세스가 올바르고 효율적으로 수행되려면 다음과 같은 추가 조건이 필요하다.   

* Mutual exclusion
  * 두 개 이상의 프로세스들이 동시에 임계 영역에 있어서는 안 된다.
* Progress
  * 임계 구역 바깥에 있는 프로세스가 다른 프로세스의 임계 구역 진입을 막아서는 안 된다.
* Bounded Waiting
  * 어떤 프로세스도 임계 구역으로 들어가는 것이 무한정 연기되어서는 안된다.
* 프로세스들의 상대적인 속도에 대해서 어떤 가정도 해서는 안된다.
  * CPU의 성능과 속도에 영향을 받지 않아야 한다.


### 뮤텍스
동시 프로그래밍에서 공유 불가능한 자원의 동시 사용을 피하기 위해 사용하는 알고리즘 중 하나로 특징은 다음과 같다.

* 임계구역을 가진 스레드들의 실행시간이 서로 겹치지 않고 각각 단독으로 실행(상호배제 Mutual Exclution)되도록 하는 기술
* 한 프로세스에 의해 소유될 수 있는 Key를 기반으로 한 상호배제 기법. Key에 해당하는 어떤 객체(Object)가 있으며, 이 객체를 소유한 스레드/프로세스만 이 공유자원에 접근할 수 있다.

* 다중 프로세스들의 공유 리소스에 대한 접근을 조율하기 위해 동기화 또는 락을 사용합니다.

* 즉 뮤텍스 객체를 두 스레드가 동시에 사용할 수 없습니다.   

![](https://velog.velcdn.com/images/heetaeheo/post/993d9d0a-bd05-40a3-a4ed-ed45fb84d8ca/image.png)   

### 데커(Dekker) 알고리즘
최초의 뮤텍스 알고리즘   

```C
Flag1 = true;
  while(Flag2) {
    if(turn!=1) {
      Flag1 = false;

      while(turn!=1) {
         // busy wait
      }

      Flag1 = true;
    }
  }

  // Start of Critical Section
  ...
  ...
  
  turn = 2;
  Flag1 = false;
  // End of Critical Section
```

```C
Flag2 = true;

  while(Flag1) {
    if(turn!=2) {
      Flag2 = false;
      while(turn!=2) {
         // busy wait
      }
      Flag2 = true;
    }
  }
  
  // Start of Critical Section
  ...
  ...

  turn = 1;
  Flag2 = false;
  // End of Critical Section
```   

turn은 Flag1과 Flag2가 모두 True 일 때 우선 순위를 구분하기 위해 사용한다. turn이 1이면 1번 스레드가 먼저 진입, 2이면 2번 스레드가 먼저 진입   

* 단점
  * 속도가 느림
  * 구현이 복잡함
  * Busy waiting -> 기다리는 동안에도 while문을 계속 수행

### 피터슨 알고리즘
```C
//A 프로세스의 구조

while(1){
    flag[A] = true;
    turn = B;
    while(flag[B] && turn == B);
    V++; //critical section
    flag[A] = false;
}
```
```C
//B 프로세스의 구조

while(1){
    flag[B] = true;
    turn = A;
    while(flag[A] && turn == A);
    V++; //critical section
    flag[B] = false;
}
```

* A 프로세스를 동작시키고 싶다면 임계 영역에 들어가고 싶다는 표시로 flag [A]를 true로 만든다.
* turn 변수를 B로 설정하고 내부 while문을 수행한다.
* 이때 B 프로세스가 들어갈 의사표시를 하지 않았다면, 즉 flag [B]가 false라면 A 프로세스는 임계 영역에 바로 들어갈 수 있다.
* 그러나 만약 두 프로세스가 동시에 동작하려고 하면 turn 변수가 늦게 수행된 프로세스가 내부 while문에서 기다리며 순서를 양보한다.
* 먼저 임계 영역에 들어갔던 프로세스는 나오면서 자신의 flag를 false로 만들고 다른 프로세스가 임계 영역에 들어가는 것을 허용한다.
* 따라서 두 프로세스가 동시에 수행될 때 내부 while문의 turn값에 따라서 어떤 프로세스가 임계 영역에 들어갈지가 결정된다. turn이 A이면 B 프로세스가 수행되고 turn이 B이면 A 프로세스가 먼저 수행된다.

데커 알고리즘과는 상대방(다른 프로세스 혹은 스레드)에게 진입기회를 양보한다는 차이가 있다.

### 제과점 알고리즘
제과점 알고리즘은 위의 2개와 달리, 여러개의 프로세스(혹은 스레드)에 대한 처리가 가능하다. 가장 작은 수의 번호표를 가지고 있는 프로세스가 임계영역에 진입한다.   

```c
while(1) {
...
  isReady[i] = true;                 // 번호표를 받을 준비
  number[i] = max(number[0~n-1]) +1  // 현재 실행 중인 프로세스 중 가장 큰 번호로 배정
  isReady[i] = false;                // 번호표 수령 완료
 
  for( j =0; j < n; j++ ) {          // 모든 프로세스에 대해 번호표를 비교한다.
    while( isReady[j] );               // 비교할 프로세스가 번호표를 받을 때까지 대기
    while(number[j] && number[j] < number[i] && j<i);
    // 프로세스 j가 번호표를 가지고 있고,
    // 프로세스 j의 번호표가 프로세스i의 번호표보다 작거나 같을 경우
    // j가 i보다 작다면(프로세스 j가 i보다 먼저 온 프로세스이면)
    // 프로세스 j의 종료(number[j]=0)까지 대기
    }

/* 이곳은 임계영역이다. */
...
number[i] = 0;                     // 임계영역 사용 완료
...

}
```










