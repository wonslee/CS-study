<details>
<summary>Table of Contents</summary>

- [array(배열)](#array-배열)
- [Linked List(연결리스트)](#linked-list-연결리스트)
- [Stack (스택)](#stack-스택)
- [Queue (큐)](#queue-큐)
- [Deque (데크)](#deque-데크)
- [Tree (트리)](#tree-트리)
- [Binary Tree (이진 트리)](#binary-tree-이진-트리)
- [Graph (그래프)]()

</details>

# Array (배열)

## 배열이란?

배열은 **연속적인(인접한) 메모리 공간**에 저장된, **같은 타입**의 데이터들의 모임이다. 가장 기초적인 자료구조.

![Untitled](https://miro.medium.com/max/497/1*-ImKrqrT14UlG6wMpAEIJQ.png)

## 배열의 연산들

- 접근 연산 :  
  배열의 접근 연산은 **index**를 이용하고, 이는 **random access**로, 빠른 속도를 보여준다.
  항상 `O(1)`.
- 탐색 연산 :  
  선형 탐색이라고 가정할 때 배열의 길이에 비례한다. `O(n)`.
    ```python
  # 선형 탐색
  def lenear_search(arr, n, key):
      for i in range(n):
                  # found
          if (arr[i] == key):
              return i
             
      # If the key is not found
      return -1
    ```

- 삽입 연산 :  
  위치에 따라 다르지만, 평균적으로 `O(n)` 시간이 걸린다.  
  대부분의 경우, 삽입하려는 원소 뒤의 원소들을 먼저 **shift(한칸씩 뒤로)**한 후에 원소를 넣어야 하기 때문이다.  
  예외 케이스로, 맨 끝자리에 대한 삽입은 해당 자리를 찾아서 넣기만 하면 끝이다.  즉 접근 연산과 할당 연산 두개이기 때문에 `O(1)`. 

- 삭제 연산 :   
  삽입 연산과 비슷하다. 맨 마지막 자리의 원소를 삭제할 때엔 O(1)이지만, 나머지 자리들은 없앤 후에 그 뒤의 원소들을 **shift(한칸씩 앞으로)** 해야한다. 이 **이동 과정** 때문에 일반적으로  `O(n)` 시간이 걸린다.

<details>
<summary>현대적인 언어들의 배열과 메소드 지원 (python) </summary>

파이썬을 비롯한 현대 언어들은 기본 지원되는 메소드를 통해 삽입, 삭제를 편하게 구현할 수 있다.
```python
arr= [1,2,3]
# 파이썬의 기본 배열 메소드들
arr.insert(0,10) # [10,1,2,3]
arr.remove(3) # [10,1,2]
```
그래도 여전히 시간 복잡도는 그대로다.
</details>

## 정적 배열의 문제점
정적 배열의 큰 문제 중 하나는 처음에 배열을 선언할 때 배열의 크기를 지정해야 하며, 그 이상의 데이터를 집어넣을 수 없다는 점이다. 즉 **원소의** **개수가 제한된다**.
```cpp
// c++의 정적 배열
int arr[3];
arr[0]=100;
arr[1]=100;
arr[2]=100;
arr[3]=100; // error!
```

## 동적 배열

배열의 문제점을 해결하기 위해 고안된 것이 자료의 개수가 변함에 따라 크기가 변경되는 동적 배열(dynamic array)이다.
동적 배열은 언어 차원에서 지원되는 기능이 아니라, 배열을 이용해 만들어 낸 별도의 자료구조다. (c++의 `vector`. python은 기본적으로 `list` 를 동적 배열로써 지원)
내부적으로 배열을 이용하기 때문에, 동적 배열은 배열이 갖는 다음 특성들을 그대로 이어받는다.
- 원소들은 메모리의 **연속된 위치에 저장됨**
- 특정 위치의 원소를 접근, 변경하는 연산을 `O(1)`에 할 수 있다.

추가적인 특성
- 배열의 크기를 변경하는 `resize()` 연산이 가능. `O(n)`
- 주어진 원소를 배열의 맨 끝에 추가함으로써 크기를 1 늘리는 `append()` 연산을 지원한다. `O(1)`

동적 배열은 크기가 바뀌어야 할 때 새 배열을 ‘동적으로 할당’받은 뒤 기존 원소들을 복사하고, 변수가 새 배열을 참조하도록 바꿔치기한다. 따라서 동적 배열 클래스는 현재 배열의 크기와 포인터를 저장하고 있을 것이다.
```cpp
class 동적배열 {
	int size; // 배열의 현재 크기
	element * array // 배열을 가리키는 포인터
	// ...
}
```

`append()` 가 `O(1)` 인 이유는, 처음에 메모리를 할당받을 때 배열의 크기가 커질 때를 대비해 **여유분**의 메모리를 미리 할당받아두기 때문이다. 즉, 실제로는 용량이 더 있으면서 배열의 크기(size)는 현재 가진 원소의 개수만큼만 인식하게 된다.

# Linked List (연결리스트)

정적이든 동적이든, 결국 배열의 단점은 삽입과 삭제시의 **이동 과정**이다.

연결 리스트는 이 문제를 해결하기 위해 고안된 자료구조로, 특정 위치에서의 **삽입과 삭제를 O(1) 시간에** 할 수 있게 해준다.

배열이 연속된 메모리 공간에 원소들을 저장하던 것과 달리, 연결 리스트는 원소들이 (일반적인 변수와 같이) 메모리 여기저기 흩어져있고 각 원소들이 이전과 다음 원소를 가리키는 **포인터**를 갖는다. 그리고 원소와 포인터의 한 묶음을 **노드**라고 부른다.

링크드 리스트는 보통 첫번째 노드와 마지막 노드에 대한 포인터를 갖고 있는데, 이를 각각 **head**와 tail이라고 부른다. (head를 중요하게 보고, tail은 선택사항이다.)

![Untitled](https://media.geeksforgeeks.org/wp-content/uploads/20220816144425/LLdrawio.png)

```cpp
// 일방향 링크드 리스트의 노드
struct node{
	int data; // 데이터
	struct node * next; // 다음 노드를 가리킬 포인터. 자기 참조
}
```

## 장점

**삽입, 삭제시의 과정이 아주 간단**하다. 이전,이후 노드의 포인터를 바꾸면 된다. (맨 처음, 맨 끝 노드에 대한 과정은 또 달라서 실제로 구현할 땐 보이는것보다 어렵긴 하다)

추가하고 싶은 위치의 노드 주소값을 알고 있다면 `O(1)` 에 가능하다. 그게 아니라 배열처럼 index로 찾아가려면 이동하는 만큼, 즉 `O(n)` 이 걸린다..

## 단점

배열과는 달리 메모리에 연속적으로 저장되어있지 않기 때문에, **특정 위치의 값을 찾기가 쉽지 않다**. i번째 노드를 찾아가려면 결국 첫번째 노드에서부터 하나씩 포인터를 따라가며 나올 때까지 찾는다. 이는 `O(n)` 시간이다.

또한 따로 포인터를 저장해야 하기 때문에 원소의 개수만큼 **포인터를 위한 메모리(overhead)**가 필요하다. 길이를 n이라고 할 때 `32bit` 컴퓨터라면 4 * n 바이트, `64bit` 컴퓨터라면 8 * n 바이트가 필요하게 되는 식이다.

##  배열 vs 링크드 리스트

|                       | 배열 | 링크드 리스트 |
|-----------------------| --- | --- |
| 메모리상의 배치              | 연속 | 불연속 |
| 임의 위치의 데이터 접근         | O(1) | O(n) |
| 임의 위치에 원소 추가,삭제       | O(n) | O(1) |
| 추가적으로 필요한 공간(overhead) |  | O(n) |

정리해보자. 임의 위치 원소에 대해 삽입이나 삭제를 할 일이 없거나, 하더라도 맨 끝에서만 이루어질 경우엔 배열이 항상 더 좋은 선택이다. 그리고 프로그램 특성상 크기가 변할 일이 없고 메모리가 중요하다면 정적배열이 좋다. 그게 아니라면 언제나 동적 배열이 안정적이라고 생각한다.

만약 임의의 원소를 접근하는게 아니라, 임의의 위치에 삽입과 삭제를 할 일이 많다면 연결리스트가 좋은 선택이다. 가령 브라우저의 히스토리 이동 기능, 음악 플레이어 등을 링크드리스트로 구현할 수 있다. 앞으로 가기, 뒤로 가기, 그리고 현재의 주소만 필요하다면, 링크드리스트만으로 충분하다.

## 연결리스트의  종류

**단일 연결리스트**(singly linked list)는 위에서 살펴봤듯이 헤드 포인터가 가장 앞의 노드를 가리키고, 각 노드는 데이터와 링크(포인터) 필드를 1개 담고 있어 노드끼리 한 방향으로 포인팅하며 이어져있는 형태다.

반면 **이중 연결리스트**(doubly linked list)는 노드가 링크 필드를 2개 담고 있어서 앞뒤의 노드들끼리 서로 포인팅하는 형태. 양방향으로 이동해야할 경우에 유용하고, 중간에서 출발하더라도 모든 노드를 방문할 수 있다.

**원형 연결리스트**(circular linked list)는 마지막 노드가 첫번째 노드를 포인팅해서, 중간에서 출발하더라도 맨 첫번째를 포함해 모든 노드를 방문할 수 있다.

이중 원형 연결리스트도 있다.

# 선형 자료구조

> Therefore, we can traverse all the elements in single run only. Linear data structures are easy to implement because **computer memory is arranged in a linear way**.

스택, 큐, 데크는 모두 [추상 자료형(ADT)](https://zorbathegeek.tistory.com/2)이다. 즉, 언어에서 기본적으로 제공되는게 아니라 데이터와 연산들에 의해 정의되는 추상적인 자료구조이다. 각각을 구현하는건 프로그래머의 몫이다.

스택, 큐, 데크 모두 선형 자료 구조이며 각자 **특정 방향**으로 **삽입(push), 삭제(pop)**가 된다는 특징을 갖고 있다. 그리고 `n`개의 원소들로 이뤄진 선형 리스트이다. 그렇기 때문에 보통

- `is_empty()` : 비어있는지 여부를 반환한다.
- `size()` : 사이즈를 반환한다.

등의 리스트 관련 연산을 갖는다. 그리고 특정 데이터(index가 아니라 데이터 값)를 찾는 경우에는 `O(n)` 이 걸린다.

이 세 자료 구조들간의 **차이점**은, 데이터를 **어느쪽 끝에서** 넣고 뺄 수 있는가이다.

# Stack (스택)

스택은 **한 방향에서만** 자료를 넣고 뺄 수 있다. 이 속성에 따라서, 가장 늦게 들어간 자료를 가장 먼저 꺼내게 된다(**후입선출, LIFO**).

![Untitled](https://media.geeksforgeeks.org/wp-content/uploads/20220714004311/Stack-660x566.png)

## 스택의 연산

스택의 주요 연산은 모두 맨 위에 있는 원소(`top`)에 관련되어있다.

- `push(e)` : 스택의 `top`에 e를 추가
- `pop()` **:** 스택의 `top`에 있는 원소를 삭제하는 동시에 반환
- `peek()` : 스택의 `top`에 있는 원소를 반환

위의 연산들은 모두 상수 시간, 즉 `O(1)` 에 이뤄져야 한다. push, pop, peek은 모두 맨 위를 가리키는 index(top)에만 접근하므로 배열의 삽입, 삭제처럼 원소들을 이동할 필요가 없기 때문이다.

## 스택의 활용

프로그램의 **콜 스택**이 대표적이다. 함수 호출은 가장 마지막에 호출된 함수가 가장 먼저 실행이 끝나고 복귀하는 후입선출 구조이다. 어떤 함수 one이 다른 함수 two를 호출했을 때, 함수 two의 실행이 끝나고 함수 one로 돌아갈 때, 컴퓨터는 내부적으로 스택을 사용해 함수 내에 선언된 변수 등 여러 정보들로 이뤄진 맥락(context)과 전반적인 **실행 순서를 관리**한다.

![Untitled](https://miro.medium.com/max/1400/1*rJ2sh-q1deQGGGVG5gYyIQ.png)

개발하면서 자주 보는 스택 오버플로우에서 ‘스택’이 바로 콜 스택이다! 😮

그 외에도…

- 실행 취소(undo, ctrl+Z)
- 수식의 괄호식 검사(백준 문제도 있다)
- DFS

등이 있다.

# Queue (큐)

큐에서는 **한 쪽 끝**에서 자료를 넣고 **반대 쪽 끝**에서 자료를 꺼낼 수 있다. 이 속성에 따라서, 가장 먼저 들어간 자료가 가장 먼저 나오게 된다(**선입선출, FIFO**).

`front` (`head`) 는 큐에서 데이터가 삭제될 위치, `rear` (`tail` )는 큐에서 마지막 데이터가 삽입된 위치이다.

![Untitled](https://media.geeksforgeeks.org/wp-content/cdn-uploads/20221213113312/Queue-Data-Structures.png)

## 큐의 연산

- `enQueue(e)` : 큐의 `rear` (맨 뒤)자리에 원소 e를 삽입
- `deQueue()` : 큐의 `front` (맨 앞)자리의 원소를 삭제하고 반환
- `get_front()` , `get_rear()` : 맨앞, 맨뒤의 원소 값을 반환

위 연산 모두 특정 index (혹은 pointer)를 사용해 빠르게 접근하기 때문에 `O(1)` 에 수행된다.

## 큐의 활용

- **프로세스** 레디 큐
- 스케줄링
- 캐시(Cache) 구현
- 네트워크 패킷 전송시의 **버퍼 대기 큐**
- 키보드 버퍼
- 프린터 큐 (스풀링)


# Deque (데크)
![https://www.geeksforgeeks.org/deque-in-python/](https://media.geeksforgeeks.org/wp-content/uploads/anod.png)

데크는 **양쪽 방향에서** 자료들을 넣고 뺄 수 있는 자료구조를 말한다.  
데크는 스택과 큐의 상위집합이라고 볼 수 있고, 데크를 이용해서 스택과 큐를 모두 구현할 수 있다.  
파이썬과 c++ 모두 `deque`를 지원하며, 메서드의 형태가 다를뿐 역할은 같다.

## 데크의 연산

- `add_front(e)` : 맨 앞에 원소 e 추가
- `add_rear(e)` : 맨 뒤에 원소 e 추가
- `delete_front(e)` : 맨 앞의 원소 삭제 및 반환
- `delete_rear(e)` : 맨 뒤의 원소 삭제 및 반환
- `get_front()` , `get_rear()` : 맨앞, 맨뒤의 원소 값을 반환

## 데크의 활용

데크는 스택과 큐의 연산을 모두 쓸 수 있기 때문에, 스택과 큐로 구현할 수 있는 것들을 모두 구현할 수 있다.

# Tree (트리)

## 트리란?
자료가 **계층적인 구조**를 가진다면 어떻게 할까?  
가계도, 모집합과 부분집합, 컴퓨터의 디렉토리 구조 등 계층적인 관계를 표현하려고 한다면 선형 자료구조(큐, 스택 )는 더 이상 적합하지 않다.  
트리는 이런 계층적인 구조를 표현할 때 적합하며, 원소들간에 **일대다(one-to-many)** 관계를 가진다.  
여기서 일대다라는건, 부모 노드쪽에선 다수의 자식 노드를 갖지만 자식 노드들은 오직 하나의 부모 노드를 갖는다는걸 의미.



![](https://miro.medium.com/max/828/1*PWJiwTxRdQy8A_Y0hAv5Eg.webp)
> 트리는 자료가 저장된 **노드**(node)들이 간선을 통해 서로 **계층적**으로 연결되어 있는 자료구조다.


## 트리 용어들
`루트 노드` : 트리의 최상위 노드. 1개만 존재한다(unique).  
`부모 노드` : 1개의 간선으로 이어져있는 윗노드는  
`자식 노드` : 1개의 간선으로 이어져있는 아랫노드  
`형제 노드` : 동일한 부모 노드를 공유하는 노드  
`조상 노드` : 해당 노드의 부모노드 ~ root노드까지 이르는 경로에 존재하는 모든 노드들  
`자손 노드` : 해당 노드의 자식노드 ~ leaf노드까지 이르는 경로에 존재하는 모든 노드들. 즉 해당 노드의 sub tree 노드들  

`내부 노드(internal/nonterminal node)` : 자식이 있는 노드  
`외부 노드(leaf/terminal/ node)` : 자식이 없는 노드  

`부분 트리(sub tree)` : 전체 트리 내부에서 특정 노드를 루ㅌ로 갖는 또 다른 트리  

`depth` : 루트에서 어떤 노드에 도달하기위해 거쳐야 하는 간선의 수(e.g. 루트노드의 깊이 = 0)  
`level` : 특정 깊이(depth)를 갖는 노드들의 층 혹은 집합  
`height` : 가장 깊숙히 있는 노드의 깊이. 전체 레벨의 수이기도 함.  
`degree` : 특정 노드에 Edge로 연결된 자식 노드의 수. '트리의 차수'라고 할 때는 트리에 있는 노드의 차수 중에서 가장 큰 값을 말한다.  


## 트리의 활용
- 컴퓨터의 파일 시스템.  
  ![https://informationtechnologyja.wordpress.com/2020/10/19/information-technology-grade-9-lesson-2-tree-directory-structure/](https://informationtechnologyja.files.wordpress.com/2019/09/presentation4.jpg)

- HTML, DOM(javascript) 트리.  
  ![](http://watershedcreative.com/naked/img/dom-tree.png)  
- DBMS의 indexing
- 결정 트리(머신러닝)
- 이진검색트리, AVL트리, B-트리 등의 트리 자료구조들

## 트리의 재귀적 성질
트리는 기본적으로 **재귀적(recursive)** 이다.   
> 하나의 트리는 0개의 루트 노드 혹은 1개의 루트 노드와 0 ~ k개의 서브 트리를 갖는다.  
그 서브 트리들도 마찬가지.  

![](https://www.101computing.net/wp/wp-content/uploads/recursive-tree-steps.png)  
위 그림은 서브 트리를 2개 갖는 트리의 재귀적 구조를 표현한 그림.  
이런 재귀적 성질 때문에, 트리를 순회할 땐 재귀 함수를 자주 쓰게 된다.  
이 성질은 `Binary Search Tree`를 구현할 때 중요하게 쓰인다.


# Binary Tree (이진 트리)
자식 노드의 개수가 고정되지 않은 트리에는 문제점이 있다.  
노드에 붙어있는 서브 트리의 개수에 다라 노드의 메모리 크기가 달라지기 때문에 구현하기 복잡하다는 것.  
e.g. 자식이 100개로 한정된 트리에서 101개가 되었을 때, 노드 객체의 포인터 수를 늘려야 한다.

2진 트리(Binary Tree)는 각 노드의 자식 노드 개수가 2개로 고정된 트리다.  
그래서 보통 노드의 자식을 얘기할 때 `왼쪽 자식`과 `오른쪽 자식`이라고 표현한다.  
![](https://www.geeksforgeeks.org/wp-content/uploads/binary-tree-to-DLL.png)

> 이진 트리는 하나의 루트 노드와 **2개의 서브트리**로 이뤄진다.    
즉 왼쪽 서브트리, 오른쪽 서브트리로 구성된 노드들의 유한집합이다.  
서브트리는 **공집합**일 수 있고, 이진 트리의 서브트리들은 모두 이진트리여야 한다.

## 이진 트리의 성질
> 높이 h인 이진 트리의 노드 갯수 n에 대해  
> $$h \leq  n  \leq  2^{h} -1$$

트리의 재귀적인 성질도 그대로 가지고 있다.  

## 이진 트리 종류

### 포화 이진 트리(full binary tree)
트리의 각 레벨에 노드가 꽉 차있는 이진 트리.  
높이 k인 포화 이진트리의 노드 개수 = 2^k-1  
![img_1.png](https://www.gatevidyalay.com/wp-content/uploads/2018/08/Time-Complexity-of-Binary-Search-Tree-Best-Case.png)

### 완전 이진 트리(complete binary tree)
높이가 k일 때, 1~k-1 레벨까지는 노드가 모두 채워져있고 마지막 레벨에선 왼쪽부터 오른쪽으로 노드가 순서대로 채워져 있는 이진 트리.  
포화 이진트리는 완전 이진 트리의 부분집합.  

### 편향 이진 트리(skewed binary tree)
높이가 k일 대, 최소 개수의 노드를 가지면서 한쪽 방향의 자식 노드만을 가진 이진 트리  
![img.png](https://www.gatevidyalay.com/wp-content/uploads/2018/08/Time-Complexity-of-Binary-Search-Tree-Worst-Case.png)

## 이진 트리의 구현
이진트리를 구현하는 2가지 방법이 있다.
<details>
<summary>이진트리 구현 방법(c++)</summary>

### 포인터로 구현하는 방법 (Linked)
```c++
struct Node{
   int data;
   struct Node *left_child;
   struct Node *right_child;
};

// addEdge: x,y 노드를 간선으로 연결
void addEdge(int x, int y, vector<vector<int>>& adj)
{
    adj[x].push_back(y);
    adj[y].push_back(x);
}
```  

### 배열로 구현하는 방법
이진트리의 부모-자식 노드간의 순서(index)에는 규칙이 생기는데, 이 성질을 활용하면 배열로 만들 수 있다.

> 부모노드의 순서가 p라고 할 때, 왼쪽 자식의 순서는 (2 * p) + 1, 오른쪽 자식의 순서는 (2 * p) + 2 이다.


```c++
char tree[14];
tree[0] = 'A'; // root
tree[1] = 'H'; 
tree[2] = 'B'; 
tree[3] = 'G'; 
tree[4] = 'I';
tree[6] = 'C';
tree[9] = 'F';
tree[10] = 'E';
tree[11] = 'D';
```
![](https://upload.wikimedia.org/wikipedia/commons/6/63/Binary_tree_%28letters%29.png?20060626010746)
</details>

# Binary Search Tree (이진 탐색 트리)
`이진 탐색 트리` : 이진 탐색 트리의 성질을 만족하는 이진트리

![](https://media.geeksforgeeks.org/wp-content/uploads/BSTSearch.png)
## 이진 탐색 트리의 성질
- 모든 원소는 유일한 키(primary key)를 가진다
- 왼쪽 서브트리 키들은 루트의 키보다 작고, 오른쪽 서브 트리의 키들은 루트의 키보다 크다
- 왼쪽과 오른쪽 서브트리 또한 이진 탐색 트리다.

## 순회
선형 자료 구조(연결 리스트, 스택, 큐 등)는 요소들을 순회하는 방법이 하나뿐이지만,  
트리는 다른 방식을 사용해야 한다.  
순회는 3단계로 이뤄진다.
- 왼쪽 서브트리로 이동
- 루트 노드 방문
- 오른쪽 서브트리로 이동

순회는 루트노드를 언제 방문하냐에 따라 순서대로 전위순회(preorder), 중위순회(inorder), 후위순회(postorder)로 나뉜다.

<details>
<summary>이진트리 순회 방법 (c++)</summary>

여기서 트리의 **재귀적인 성질**을 다시 떠올려보면 이해하기 쉽다.

```c++
// 전위순회
void preorder(struct Node* node){
if (node == NULL) return;
cout << node->data << " "; // 루트 노드 방문
preorder(node->left);
preorder(node->right);
}
// 중위순회
void inorder(struct Node* node){
    if (node == NULL) return;
    inorder(node->left);
    cout << node->data << " "; // 루트 노드 방문
    inorder(node->right);
}
// 위순회
void postorder(struct Node* node){
    if (node == NULL) return;
    inorder(node->left);
    inorder(node->right);
    cout << node->data << " "; // 루트 노드 방문
}
```  
순서의 차이가 사소해보일 수 있지만, 결과는 천차만별이다!  
![](https://miro.medium.com/max/640/0*YzOEfnGnWTPbsUkv)  
시간복잡도는 O(n)으로 모두 같다.
</details>

## 연산과 시간복잡도
`이진 탐색`처럼, 이진 탐색 트리는 각 분기(현재 노드보다 큰지 OR 작은지)마다 탐색할 후보의 수를 2분의 1로 줄여나간다.  
즉 트리의 높이 h만큼 **O(h)** 가 걸린다. 하지만 원소의 개수 N을 기준으로 하는 표준을 따르자 ^^.   

**이진트리의 종류**를 생각해보자. 왠만하면(완전트리일 때) h = log2 n이고 최악(편향이진트리)일 때 h = N이다.  
그리고 이진트리의 삽입과 삭제는 **탐색**이 주요 로직이다.  
따라서..

|       | 평균       | 최악   |
|-------|----------|------|
| 탐색    | O(log n) | O(n) |
| 삽입    | O(log n) | O(n) |
| 삭제    | O(log n) | O(n) |
| 순회    | O(n)     | O(n) |

# Graph (그래프)
## 그래프란?
>   현실 세계의 사물, 객체, 추상적인 개념 간의 **연결 관계**를 표현한 것

`그래프`는 정점(**V**ertex, node)과 **간선**(edge)으로 이뤄진 **비선형 자료구조**이며 정점과 간선들의 유한 집합이라고 할 수 있다.  
  e.g. 도시들을 연결하는 도로망, 사람들의 지인 관계, 네트워크, 웹 사이트 간의 링크 관계

그래프에는 **부모 자식 관계에 대한 제약이 없기** 때문에, 트리보다 훨씬 다양한 구조를 표현할 수 있고 그만큼 많은 문제를 해결하는데 쓰일 수 있다.
그래프는 정점들과 간선들로 정의되며, 정점의 위치 정보나 간선의 순서는 그래프를 정의하는 요소가 될 수 없다.  

![](https://cdncontribute.geeksforgeeks.org/wp-content/uploads/undirectedgraph.png)  

## 그래프의 용어
- `간선(u,v)` : 정점 `u`에서 정점 `v`를 잇는 선. 간선은 무게/값/비용 등의 가치를 가질 수 있다.
- `경로(path)` : 정점과 정점 사이에 연결된 간선들을 순서대로 나열한 것이다. (대부분 한 정점을 한번만 지나는 경로인 `단순 경로(simple path)`를 의미)
- `부분 그래프(subgraph)` : 어떤 그래프의 정점의 일부와 간선의 일부로 이뤄진 또 다른 그래프
- `인접 정점(adjacent vertex)`란 간선에 의해 한 정점과 직접 연결된 정점
- `정점의 차수(degree)` : 한 정점에 인접한 정점의 수(=정점에 그어진 간선의 수). 방향 그래프에서는 외부로 향하는 간선의 개수를 `진출 차수`, 외부로부터 오는 간선의 개수를 `진입 차수`라고 부른다.
- `사이클(cycle)` : 시작 정점과 종료 정점이 동일한 (단순)경로. 트리는 그래프의 특수한 형태로서, 사이클을 갖지 않는 그래프다.

## 그래프의 종류
그래프의 정점과 간선에는 추가적인 속성을 부여할 수 있고, 형태에 제약을 둘 수 있다. 그에 따라 종류가 나뉘어진다.
### 방향 그래프 (directed graph)
각 간선이 **방향** 속성을 갖는 그래프.  
짝사랑 관계, 도로망의 일방통행 등을 표현할 수 있다.  

![](https://media.geeksforgeeks.org/wp-content/cdn-uploads/digraph.png)  

방향은 경로에 있어서 중요한 속성이 된다.  
예를 들어 위 그림에서 V3에서 V2로 가려면, e2간선을 쓰지 못하고 e3, e5, e7을 거쳐야 한다. 
반대로 방향 속성을 갖지 않는 그래프를 `무향 그래프(undirected graph)`라고 부른다.  

### 가중치 그래프 (weighted graph, network)

각 간선이 **가중치** 속성을 갖는 그래프.  
이 속성은 두 도시 사이의 거리, 두 외화 사이의 환율, 두 사람 사이의 호감도 등을 표현할 수 있다.  
가중치 그래프는 네트워크라고도 부르며, 최소 신장 트리, 최단 경로 알고리즘에 쓰인다.  

![](https://media.geeksforgeeks.org/wp-content/uploads/graphhh.png)   

### 다중 그래프와 단순 그래프 (multi graph and simple graph)
두 정점 사이에 **두 개 이상의 간선**이 있을 수 있는 그래프.  

![](https://luisnatera.com/assets/img/Euler2.jpg)  
위 그림은 그래프 이론의 시초인 오일러의 `쾨니히스베르크 다리` 그림이다.   
각 지면을 정점, 다리를 간선이라고 보면 하나의 그래프가 생기고, 이는 다중 그래프다.  
반대로 두 정점 사이에 딱 하나의 간선만이 있는 그래프를 `단순 그래프`라고 부른다.  


## 그래프 구현
그래프는 트리와는 달리 새로운 정점이나 간선을 추가,삭제할 일이 별로 없다.  
따라서 대부분의 그래프는 구조의 변경이 어렵더라도 간단하고 메모리를 적게 차지하는 정적인 방법으로 구현된다.  
그래프의 정점들을 하나하나 객체 인스턴스로 표현하는 대신,  
각 정점에 0부터 시작하는 번호(index)를 붙이고 **배열에 각 정점의 정보를 저장**한다.  

그래프 표현은 간선의 정보를 어떤 식으로 저장하느냐에 따라 크게 두가지로 나뉜다.  

### 인접 리스트(adjacency list)
인접 리스트 표현 방법에서는 그래프의 각 정점마다 해당 정점에서 나가는 간선들의 목록을 저장해서 표현한다. (**array of linked list**)  
즉, **각 정점마다 하나의 연결 리스트**를 갖는다. 여기서 주의할 점은, 이건 어떤 경로를 나타내는게 아니라 말 그대로 각 정점의 인접 정점들을 나열한다는 것.

![](https://soliloquiess.github.io/assets/20210316_105058.png)
<details>
<summary>인접 리스트 구현 (c++)</summary>

```c++
vector<list<int>> adj; // adj[i]는 정점 i와 연결된 정점들의 번호를 저장하는 연결리스트.  

// add edge (u,v)
adj[u].insert(v);
adj[v].insert(u);
```
</details>

### 인접 행렬(adjacency matrix)
인접 리스트 표현의 단점 : 두 정점이 주어질 때 둘의 연결 여부를 알기 위해선 한쪽 정점에 달린 연결 리스트를 일일이 뒤져야 한다  
특정 간선의 존재 여부 혹은 가중치 값에 접근하기 빠르게 하기 위해 인접 행렬 표현을 쓴다.  
V * V 크기의 2차원 배열을 이용한다.  


![](https://soliloquiess.github.io/assets/20210316_103546.png)

<details>
<summary>인접 행렬 구현 (c++)</summary>

```c++
vector<vector<bool>> adj; // 가중치 없는 가장 간단한 형태의 그래프(간선이 있으면 1, 없으면 0)

// add edge(u,v) 
adj[u][v] = 1;
```
</details>

인접 행렬 표현의 단점 : 실제 간선의 개수와 상관없이 항상 O(V^2) 크기의 공간을 사용해야 한다.  
반면 인접 리스트 표현은 연결리스트에 실제 간선 수만큼만 원소가 들어있으므로 O(V + E) 공간만 쓰면 된다.  

- `희소 그래프(sparse graph)` : 간선의 수가 V^2에 비해 훨씬 적은 그래프  
- `밀집 그래프(dense graph)` : 간선의 수가 V^2에 거의 비례하는 그래프

인접 리스트 표현은 희소 그래프, 인접 행렬 표현은 밀집 그래프를 표현할 때 각각 유리하다. 

### 인접 리스트 vs 인접 행렬
V = 정점의 개수, E = 간선의 개수, degree(V) = 정점의 차수

|  | 인접 행렬(Adjacency Matrix)                         | 인접 리스트(Adjacency List)                                                |
| --- |-------------------------------------------------|-----------------------------------------------------------------------|
| 공간 | **V^2**                                         | **V+E**                                                               |
| 간선 (u,v) 검색 | **O(1)**  : 2차원 배열의 원소 접근 연산. `matrix[i][j]`      | **O(degree(V))** : 한 정점의 연결리스트를 탐색.                                   |
| 정점 추가 | **O(V^2)** : 배열 크기가 (V+1)^2로 증가시키는 중의 배열 복사 비용. | **O(1)**  원소 하나만 추가하면 끝                                               |
| 간선 추가 | **O(1)**  `matrix[i][j] = 1`                      | **O(1)** 리스트에 추가하면 끝                                                  |
| 정점 삭제 | **O(V^2)** 배열 크기가 (V-1)^2로 감소하기 때문에 배열 복사 비용 생김. | **O(V+E)** 삭제한 공간을 채우기 위해 나머지 원소들을 shift해야 하고, 다른 정점의 연결리스트에서 삭제된 정점을 찾아 삭제 |
| 간선 삭제 | **O(1)**  `matrix[i][j] = 0`                      | **O(E)**   정점의 연결리스트에서 인접 정점 검색하는 시간 및 삭제 시간                          |


인접 행렬은 간선 검색, 추가, 삭제에 강하기 때문에 정점의 갯수가 고정적이라면 인접행렬이 훨씬 빠르다.  
반면 인접 리스트는 정점 추가, 삭제에 강하기 때문에 간선의 갯수가 고정적이라면 인접 리스트가 빠르다.
둘 다 고정적이라고 한다면, 밀집 그래프엔 인접 행렬(검색 시간이 빠르기 때문에)을 쓰고 희소 그래프에는 인접 리스트(공간을 아끼기 위해)를 쓰는 것이 좋다.  

# 우선순위 큐와 힙(Binary Heap)

## 우선순위 큐란?
> 데이터들이 우선순위를 가지고 있어, **우선순위가 높은 데이터부터** 먼저 나가는 큐.

특정 순서대로 기다리고 있는 데이터들을 저장한다는 점에서 큐와 비슷하다.  
그러나 큐는 FIFO인 반면, 우선순위 큐는 PFO(Priority First Out)라는 점에서 차이가 있다. (PFO는 내가 만들어낸 단어다.)  

우선순위 큐가 쓰이는 컴퓨터의 **작업 스케줄링**(운영체제 내용)에서는 우선순위에 따라 작업을 처리한다.  
가령 windows OS에서의 `윈도우 키`는 왠만한 다른 작업보다도 우선순위에 있다.  

우선순위 큐를 구현하는 방법은 다양하다.  
단순히 배열에 원소를 모두 집어넣고, 원소를 꺼낼 때마다 모든 원소중에서 가장 우선순위가 높은 데이터를 찾는 방법이 있다.  
이렇게 하면 원소를 추가하는데는 O(1), 원소를 꺼내는데는 O(n)의 시간이 걸린다.(정렬되어있지 않으므로 선형탐색만 가능)  

혹은 [균형잡힌 이진검색트리(AVL tree)](https://en.wikipedia.org/wiki/AVL_tree)방법이 있다.  
삽입과 삭제 모두 O(log n) 시간에 가능하지만, 우선순위 큐를 구현하는데 쓰이기에는 과하게 복잡하다.  
이 방법보다 훨씬 단순한 구조가 있는데 바로 이진 **힙(binary heap)** 이다.  

## 힙이란?

![](https://media.geeksforgeeks.org/wp-content/cdn-uploads/20221220165711/MinHeapAndMaxHeap1.png)
> **가장 큰(작은) 원소를 찾는데 최적화**된 형태의 완전 이진 트리.  

'가장 큰 원소를 찾는데 최적화'라는 말을 구체적으로 풀어 쓰면,  
평소에 데이터들이 특정한 **규칙**을 만족하도록 한다는 뜻이다.  

- **대소 관계 규칙** : **부모** 노드가 가진 원소값은 항상 자식 노드가 가진 원소값 **이상**이어야 한다.  
  이진 검색 트리와는 달리 왼쪽 자식과 오른쪽 자식이 갖는 원소의 크기는 제한하지 않는다.  
이렇듯 힙의 대소 관계 조건은 **느슨하다**.  
그래서 엄격한 **완전이진트리** 조건을 쉽게 만족시킬 수 있고, 덕분에 힙은 **배열**로 쉽고 효율적으로 구현할 수 있다.(트리의 배열 구현을 떠올려보자)
- **완전이진트리** 규칙 : 말 그대로 완전이진트리로써 갖는 규칙.  
마지막 레벨을 제외한 모든 레벨에 노드가 꽉 차 있어야 하고, 마지막 레벨의 노드들은 항상 가장 왼쪽부터 차례대로 채워져 있어야 한다.  
힙의 **높이**는 언제나 **log2 n** 이다.
- **모양 규칙** :  부모노드의 순서가 p라고 할 때, 왼쪽 자식의 순서는 (2 * p) + 1, 오른쪽 자식의 순서는 (2 * p) + 2 이다.  
이 규칙을 이용해서 노드들간의 연결 관계를 명시적으로 저장하는 대신 **배열의 index**를 통해 힙의 정보를 표현한다.  

![](https://media.geeksforgeeks.org/wp-content/cdn-uploads/Leaf-starting-point-in-a-Binary-Heap-data-structure.png)  

## 힙의 연산들
힙 자체는 배열로 간단하게 구현된다.   
그러나 힙의 연산들은 **힙의 조건을 만족시키도록** 까다로운 과정을 거친다.  

[visualgo](https://visualgo.net/en/heap)

### 삽입
먼저 **모양 규칙**을 만족시키도록 `heap[]`의 맨 끝에 추가한다.  
그 다음, **대소 관계 규칙**을 만족시키는 과정을 반복한다.  
새 원소를 그 부모 노드 원소와 비교하고, 부모 노드의 원소가 작다면 두 원소의 위치를 바꾼다.  
그렇게 더 크거나 같은 부모 노드를 만나거나 루트에 도달할 때까지 **반복**한다.  

![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Ft1.daumcdn.net%2Fcfile%2Ftistory%2F22557639583849E438)

이 반복 과정은 힙(트리)의 높이만큼, 즉 **O(log n)** 시간이 걸린다.  


<details>
<summary>힙 원소 삽입 구현 (c++)</summary>

```c++
// push
void push(int num) {
    maxHeap[++heapSize] = num; // 힙 크기를 증가시키는 동시에 마지막 노드에 인자값 할당
    
    for(int i = heapSize; i > 1; i /= 2) { // i/=2, 즉 한 레벨씩 위로 올라가기를 반복 
        if(maxHeap[i/2] < maxHeap[i]) // 자신의 부모 노드보다 크면 swap
          swap(i/2, i);
        else  break;
    }
}
```

</details> 

### 최대 원소 찾기
(최대힙에서) 최대 원소를 찾는건 아주 간단하다.  
배열의 첫번째 원소에 접근하면 된다.(heap[0])

### 삭제
문제는 최대 원소(루트 노드)를 삭제한 다음이다.  

먼저 **모양 규칙**을 만족하도록, 힙의 마지막 노드를 삭제하고 그 노드를 루트에 덮어씌운다.   
그 다음, **대소 관계 조건**을 만족하도록 두 자식 중 더 큰 원소를 선택해 (새로 온)루트와 맞바꾼다.  
이 작업을 트리의 바닥에 도달하거나 두 자손이 모두 자기 자신 이하의 원소를 갖고 있을 때까지 **반복**한다.  

![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Ft1.daumcdn.net%2Fcfile%2Ftistory%2F270DE042583849E61F)
(그림 오류 : 4,5번째 그림에서 3번째 노드의 값은 15입니다)

삽입과 마찬가지로 **O(log n)** 시간이 걸린다.  

<details>
<summary>힙 원소 삭제 구현 (c++)</summary>

```c++
// pop
void pop() {
    heap[0] = heap.back();
    heap.pop_back();
    int here = 0;
    
    while(true){
      int left = here*2 + 1, right = here*2 + 2;
      if (left >= heap.size()) break;   // leaf에 도달한 경우
      
      int next = here;  // heap[here]가 내려갈 위치
      // 왼쪽, 오른쪽 자식 노드중에 큰 놈 고르기
      if (heap[next] < heap[left])
        next = left;
      if (right < heap.size() && heap[next] < heap[right])
        next = right;
      
      if (next == here) break;
      swap(heap[here], heap[next]);
      here = next;
    }
}
```
</details>


### 일반 배열을 힙으로 만들기(Heapify)

배열을 힙으로 만들려면, 결국 **힙의 규칙**을 만족하도록 노드들을 재배치하는 과정이 필요하다.  
이진트리의 재귀적 성질 덕분에, 재귀함수로 이 과정을 구현할 수 있다.  

1. 부모, 왼쪽 자식, 오른쪽 자식 중에서 가장 큰 값을 갖는 노드를 찾는다(largest)
2. 부모가 largest라면 상태를 유지하고, 아니라면 부모 노드와 해당 자식의 위치를 바꾼다.(swap)
3. 바꿨다면, 해당 서브트리에 대해서 1,2 과정을 반복한다.(재귀호출)

이 과정을 내부(non-leaf)노드들에 대해서만 실행하면 힙이 완성된다.  
(n / 2) - 1번째부터 1번째(루트)노드까지 heapify를 호출하면 된다.  

<details>
<summary>heapify 구현 (c++)</summary>

https://www.geeksforgeeks.org/building-heap-from-array/

```c++
vector<int> tree;

// i : 서브 트리 노드의 index. 
void heapify(int i){
    int largest = i; // largest : 자식들과 비교하면서 갱신시킬 값의 index
    int left = 2 * i + 1; 
    int right = 2 * i + 2; 
  
    // 왼쪽 자식이 루트보다 큰 경우
    if (l < tree.size() && tree[l] > tree[largest])
        largest = l;
  
    // 오른쪽 자식이 루트보다 큰 경우
    if (r < tree.size() && tree[r] > tree[largest])
        largest = r;
  
    // largest가 루트노드가 아닌 경우, swap
    if (largest != i) {
        swap(tree[i], tree[largest]);
  
        // Recursively heapify the affected sub-tree
        heapify(largest);
    }
}

void buildHeap(){
    // Index of last non-leaf node
    int startIdx = (tree.size() / 2) - 1;
  
    // Perform reverse level order traversal from last non-leaf node and heapify each node
    for (int i = startIdx; i >= 0; i--)
        heapify(i);
    
}
```
</details>

## 시간복잡도
| 연산            | 시간 복잡도 |
|---------------| --- |
| 원소 삽입         | O(log n) |
| 최대값 접근        | O(1) |
| 원소 삭제         | O(log n) |
| 힙 생성(heapify) | O(n) |


# B-tree와 B+tree

## B-tree란?


: **이진트리를 확장**한 자료구조 **탐색 성능**을 높이기 위해 평소에 데이터들의 높이를 균형있게 유지하는 **Balanced Tree**의 일종이다.  

![https://media.geeksforgeeks.org/wp-content/uploads/20200506235136/output253.png](https://media.geeksforgeeks.org/wp-content/uploads/20200506235136/output253.png)

- 노드에는 **2개 이상**의 데이터(key)가 들어갈 수 있다.
- 하나의 노드가 가질 수 있는 자식의 최대 숫자가 **2보다 크다.**
- 모든 단말(leaf) 노드는 같은 레벨에 있어야 한다. 항상 균형을 유지한다 (**균형 잡힌 트리**).
- 최대 M개의 자식을 가질 수 있는 B 트리를 M차(M-way) B트리라고 한다. 내부 노드는 **M/2 ~ M개의 자식**을 가질 수 있다.  (e.g. 3차 B트리 : 1~3개의 자식 노드 가능)
- 노드 내에 **데이터(key)**는 floor(M/2)-1개부터 **최대 M-1개까지** 포함될 수 있다
- 특정 노드의 **데이터(key)가 K개**라면, **자식 노드의 개수는 K+1개**여야 한다.

![https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FBLi5L%2FbtrdInhxyVP%2Febff3uYkmyoEty5lULR8kK%2Fimg.png](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FBLi5L%2FbtrdInhxyVP%2Febff3uYkmyoEty5lULR8kK%2Fimg.png)

위의 그림은 3차 B 트리로, 각 노드에 데이터(key)와 그 자식들을 가리키는 포인터가 있다.

- 노드 안에서 데이터(key)는 항상 정렬된 상태를 유지한다.
- 이진 탐색 트리(BST)와 마찬가지로, 노드의 각 key의 왼쪽 자식은 자신보다 작고 오른쪽 자식은 자신보다 크다.

## 그래서 왜 쓰는데?

일반적인 트리인 경우 탐색하는데 평균적인 시간 복잡도로 **O(log N)**을 갖는다.

트리가 **편향된 경우**가 문제인데, ****최악의 시간복잡도로 **O(N)**을 갖게 된다.

이러한 트리의 단점을 보완하기 위해 트리가 편향되지 않도록 항상 균형을 유지하는게 B 트리다. 자식들의 밸런스를 잘 유지하면 **최악의 경우에도** **O(logN)**의 시간이 보장된다.

## B 트리 연산들

### 탐색

[https://www.cs.usfca.edu/~galles/visualization/BTree.html](https://www.cs.usfca.edu/~galles/visualization/BTree.html)

1. 루트 노드부터 탐색 시작
2. **노드안의 key를 순회**하면서 K를 찾고, 존재하면 탐색을 종료
3. K가 존재하지 않는다면, key들과의 **값 비교**를 한 후 그에 맞는 포인터를 통해 자식 node로 내려간다.
4. leaf node까지 2~3을 반복한다.

e.g. 아래 트리에서 값이 14인 key를 찾아보자.

![https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2Fdbqer3%2FbtrduHVBXeZ%2FZJISmJgbgKJpp1k5UnFYM0%2Fimg.png](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2Fdbqer3%2FbtrduHVBXeZ%2FZJISmJgbgKJpp1k5UnFYM0%2Fimg.png)

![https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FcjW9kD%2FbtrdESnKFfM%2FWkRCeAitSffVPiKpBfxkbK%2Fimg.png](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FcjW9kD%2FbtrdESnKFfM%2FWkRCeAitSffVPiKpBfxkbK%2Fimg.png)

### 삽입

균형을 유지해야 하는 B 트리의 성질 때문에, key를 삽입하고 균형이 맞지 않는 경우엔 트리를 변형시켜야 한다.

1. 빈 트리인 경우, 루트 노드를 만들어 K를 삽입한다. root node가 가득 찬 경우, node를 분할하여 leaf node를 생성한다.
2. K가 들어갈 **leaf node를 탐색**한다.
3. 해당 leaf node에 자리가 남아있다면 **정렬을 유지**하도록 알맞은 위치에 삽입하고, leaf node가 **꽉 차 있다면** K를 삽입한 후 해당 node를 **중앙값을 기준으로 분할**한다. 중앙값은 부모 node로 합쳐지거나 새로운 node로 생성되고, 중앙값을 기준으로 왼쪽의 key는 왼쪽 자식, 오른쪽의 key는 오른쪽 자식으로 생성된다.

e.g. 아까 트리에다 13을 삽입하는 과정

![https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FsCUPI%2FbtrdBrrwcKW%2FBNlLhjIHg9THT2HgC3araK%2Fimg.png](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FsCUPI%2FbtrdBrrwcKW%2FBNlLhjIHg9THT2HgC3araK%2Fimg.png)

1. 13이 들어갈 leaf node 탐색
2. 정렬을 유지하는 위치에 삽입
3. 한 노드에 들어갈 수 있는 key 수보다 많이 삽입된 상태이므로, 중앙값(13)을 기준으로 분할하고, 13은 부모 노드로 합쳐준다. 이 과정이 반복된다

![https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FupFIB%2FbtrdzN9tLOL%2FxVKPr2jaSIh7pysy5k1Qr1%2Fimg.png](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FupFIB%2FbtrdzN9tLOL%2FxVKPr2jaSIh7pysy5k1Qr1%2Fimg.png)

![https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2Fl8h8w%2FbtrdBqNyhwS%2F3dNoY02TCFniaHdEy5y6sk%2Fimg.png](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2Fl8h8w%2FbtrdBqNyhwS%2F3dNoY02TCFniaHdEy5y6sk%2Fimg.png)

## 시간복잡도

결국 핵심 연산은 탐색이고, 이 과정이 **O(log n)** 이기 때문에 삽입과 삭제 또한 O(log n).

|  | 평균 | 최악 |
| --- | --- | --- |
| 탐색 | O(log n) | O(log n) |
| 삽입 | O(log n) | O(log n) |
| 삭제 | O(log n) | O(log n) |


## B+tree란?

- B-tree의 확장개념
- 브랜치(중간) 노드에 **key만** 담아두고, data는 담지 않는 자료구조. 오직 **리프 노드에만 key와 data를 저장**한다. DB관점에서 생각해볼 때 레코드들은 모두 리프 노드에만 들어간다. 
- 리프 노드끼리 **Linked list**로 연결되어 있다. 그래서 SQL에서 전체 조회를 할 때에도 속도가 빠르다.

![https://upload.wikimedia.org/wikipedia/commons/thumb/3/37/Bplustree.png/600px-Bplustree.png](https://upload.wikimedia.org/wikipedia/commons/thumb/3/37/Bplustree.png/600px-Bplustree.png)

- 데이터의 빠른 접근을 위한 **인덱스** **역할**만 하는 중간 노드(internal node, index node)가 추가로 있음. index노드가 탐색시간을 줄여서 조건부 검색을 빠르게 해준다.
- 리프 노드를 제외하고는 데이터를 담아두지 않기 때문에 메모리 효율적이다.
- 하나의 노드에 더 많은 key들을 담을 수 있기 때문에 **트리의 높이가 더 낮다**.(cache hit를 높일 수 있음)
- SQL로 테이블 전체 조회를 할 때, B+tree는 전체 리프 노드들에 대해서만 한 번 선형탐색하면 되기 때문에 B-tree에 비해 빠르다. 반면 B-tree의 경우에는 모든 노드를 확인해야 한다.
## InnoDB에서 사용된 B+tree

![https://blog.kakaocdn.net/dn/Cbs9b/btqBVf7DVW2/8JOOKlHiwkoTsqbvbTt7R1/img.png](https://blog.kakaocdn.net/dn/Cbs9b/btqBVf7DVW2/8JOOKlHiwkoTsqbvbTt7R1/img.png)

복잡하긴 하다. 같은 레벨의 노드들끼리는 Double Linked List를 사용했고, 자식 노드로는 Single Linked List로 연결되어있다.

key의 범위마다 찾아가야할 **페이지 넘버**(포인터)가 있는데, 해당 페이지 넘버를 통해 곧바로 다음 노드로 넘어간다.


# 더 배울 부분들
- 각 자료구조의 응용, **사례와 연결지어 설명**
- **구간 트리**(segment tree), **AVL 트리**
- 그래프 **DFS와 BFS**, 최단거리 알고리즘
- **heapify**와 heap sort
- 배열과 Cache hit rate
- 동적 배열과 overhead
- 포인터의 메모리 크기는 왜 32, 64비트 컴퓨터마다 다를까?
- cache hit란?

# 참조
- 종만북(알고리즘 문제해결전략)
- C언어로 쉽게 풀어쓴 자료구조
- https://www.geeksforgeeks.org/difference-between-general-tree-and-binary-tree/
- https://towardsdatascience.com/8-useful-tree-data-structures-worth-knowing-8532c7231e8c
- https://medium.com/@ajinkyajawale/inorder-preorder-postorder-traversal-of-binary-tree-58326119d8da
- https://www.geeksforgeeks.org/graph-and-its-representations/
- https://www.geeksforgeeks.org/comparison-between-adjacency-list-and-adjacency-matrix-representation-of-graph/
- https://luisnatera.com/posts/2020/04/Euler/
- https://soliloquiess.github.io/class/2021/03/16/algorithm_01.html
- https://www.geeksforgeeks.org/heap-data-structure/
- https://www.geeksforgeeks.org/applications-priority-queue/
- https://debugdaldal.tistory.com/28
- [**https://rebro.kr/169**](https://rebro.kr/169)
- [https://zorba91.tistory.com/293](https://zorba91.tistory.com/293)
- [https://blog.jcole.us/2013/01/10/btree-index-structures-in-innodb/](https://blog.jcole.us/2013/01/10/btree-index-structures-in-innodb/)
- [https://en.wikipedia.org/wiki/B%2B_tree](https://en.wikipedia.org/wiki/B%2B_tree)