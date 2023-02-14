# 알고리즘 시간 복잡도 분석

##  시간 복잡도란?

> 시간 복잡도(time complexity)란 가장 널리 사용되는 알고리즘의 수행 시간 **기준**으로,  
알고리즘이 실행되는 동안 수행하는 **기본적인 연산의 수**를 **입력의 크기 n**에 대한 **함수**로 표현한 것입니다.

시간 복잡도를 왜 쓸까? **실제 실행시간 측정**을 하는게 훨씬 직관적이지 않나?  

그러나, 이는 알고리즘의 속도를 일반적으로 논하는 기준이 되기는 어렵다.  
프로그램의 수행 시간은 하드웨어, 운영체제, 언어, 라이브러리 등 **수많은  요소에 의해 바뀔 수 있기 때문**이다.

즉, 알고리즘 수행 시간의 기준은 **보편성**을 가져야 한다. 그래서 수학 개념들이 등장한다.    

### 빅오표기법(Big-O Notation)

> 한 가지 항목이 전체의 대소를 좌지우지하는 것을 **지배한다**고 표현합니다.

$$3n^{10} +  5n^{3} +  n^{2} + ...$$

위의 다항식에서 **전체 값을 지배**하는 항은 뭘까? 바로 3n^10이다.  
n이 증가하면 증가할수록 **가장 빠르게 증가하는 항**은 전체값에서 가장 큰 비중을 차지하게 된다.

그래서 `빅오표기법`에서는 **최고차항**만을 표기한다.  
그리고 최고차항의 계수 또한 전체를 '지배'하지는 않기 때문에 **상수는 제외**한다.  

$$O(n^{10})$$

### 반복문이 지배한다(dominate)

알고리즘의 수행 시간을 지배하는 것은 무엇일까? 바로 **반복문**이다.  

짧은 거리를 달릴 때는 자전거가 빠를 수 있는 것처럼, 입력의 크기가 작을 때는 반복 외의 다른 부분들이 갖는 비중이 클 수 있지만, 
**입력의 크기가 커지면 커질수록** 반복문이 알고리즘의 수행 시간을 지배하게 된다.


## 시간복잡도 분류

![Untitled](https://upload.wikimedia.org/wikipedia/commons/thumb/7/7e/Comparison_computational_complexity.svg/1200px-Comparison_computational_complexity.svg.png)  

### 선형 시간: O(n)

**연산횟수가 n에 정비례**하는 알고리즘을 말한다. 그래프로 치면 1차 함수 그래프.  

<details>
<summary>선형 탐색 구현 (python)</summary>

```python
# 선형 탐색 (일일이 다 확인하기)
def linearSearch(my_list, target):
	for element in my_list:
		if element == target:
			return True
	return False
```

리스트의 길이가 N이고 저 함수의 실행 시간은 N(에 정비례)이라고 할 수 있다.

</details>

보통 **반복문이 하나**이고 **길이 n인 배열 전체의 원소에 대한** **연산**(접근, 더하기 등)을 할 때 선형 시간을 필요로 한다.  

### 선형 이하 시간

**선형 시간 복잡도보다 상승률이 낮은** 알고리즘.  
O(log(n)), O(root(n)) 등이 있다.

배열을 절반씩 쪼개서 수를 찾는 `이진 탐색`은 `log2 N` 의 시간 복잡도를 갖는다.

<details>
<summary>이진 탐색 구현 (python)</summary>

```python
# 이진 탐색 : O(logN)
def binary_search(arr, x):
    low = 0
    mid = 0
    high = len(arr) - 1

    while low <= high:
        # 중간 index
        mid = (high + low) // 2
        # If x is greater, ignore left half
        if arr[mid] < x:
            low = mid + 1
        # If x is smaller, ignore right half
        elif arr[mid] > x:
            high = mid - 1
        # x is present at mid
        else:
            return mid
 
    # element not present
    return -1
```

</details>


### 상수 시간: O(1)

입력 크기 n에 상관없이 언제나 **고정적인 시간**이 걸리는 알고리즘.  
즉 기본적인 연산이다. 데이터가 얼마나 증가하든 성능에 영향을 거의 미치지 않는다.

<details>
<summary>배열의 원소 접근 (python)</summary>

index를 통해 해당 주솟값을 바로 찾아가는 **random access**이기 때문에 이는 O(1)이다.

```python
arr = [1,2,3]

print(arr[0])  # index 접근 연산 
arr.append(10)
arr.pop()
```

</details>

### 다항 시간

반복문의 수행 횟수를 **N의 다항식**으로 표현할 수 있는 알고리즘.  

같은 다항시간 알고리즘이라도 차수에 따라 큰 시간 차이가 난다.   

코드 관점에서는 중첩 반복문의 개수가 N의 차수가 된다.  

<details>
<summary>선택 정렬: O(n^2) (python)</summary>

```python
// 선택 정렬
def selection_sort(arr):
    for i in range(len(arr) - 1): // 첫번째 반복문
        min_idx = i
        for j in range(i + 1, len(arr)): // 두번째 반복문
            if arr[j] < arr[min_idx]:
                min_idx = j
        arr[i], arr[min_idx] = arr[min_idx], arr[i] // swap
```

</details>

### 지수 시간(exponential time)

<details>
<summary>재귀 피보나치: O(2^n)</summary>

재귀 피보나치의 경우, **n이 1 증가할 때마다** 실행시간은 약 **2배 증가**한다.  
즉 `2^n` 시간 복잡도를 갖는다.


```python
# 재귀 피보나치
def fib(n):
   if n <= 1:
       return n
   else:
       return(fib(n-1) + fib(n-2))
```

왜 `2^n` 복잡도를 갖는지 생각해보자.  
첫 실행 때 n이 5라면 fib(4)와 fib(3), 총 2번의 재귀호출을 하게 된다.  
그리고 호출된 함수들 fib(4)와 fib(3)도 또 다시 2번의 재귀호출을 한다.

![](https://i.stack.imgur.com/2dxLl.png)

</details>


### 시간복잡도별 코드 형태
<details>
<summary>시간복잡도별 코드</summary>

```cpp
// 1. O(log(n))
while(n)
	n/=2;

// 2. O(root(n))
for(int i=0; i*i < n; i++);

// 3. O(n)
for(int i=0; i<n; i++);

// 4. O(n log(n))
for(int i=0; i<n ;i++)
	j=i
	while (j)
		j/=2

// 5. O (n root(n))
for(int i=0;i<n;i++)
	for(int j=0; j*j<=i; j++);

// 6. O(n^2)
for(int i=0;i<n;i++)
	for(int j=0; j<n; j++);

// 7. O(n^3)
for(int i=0;i<n;i++)
	for(int j=0;j<n;j++)
		for(int k=0;k<n;k++);

// 8. O(2^n)
int function(int n){
	if(n==0||n==1)
		return 1;
	return function(n-1)+function(n-2);
}

// 9.O(n!)
// n개의 데이터를 나열하는 방법의 수
void function(int x, vector<int> pick, vector<bool> picked){
	if(x==n){
		for(int i=0;i<pick.size();i++)
			printf("%d\n", pick[i]);
		return;
	}
	for(int i=0;i<n;i++){
		if(picked[i])
			continue;
		pick.push_back(i);
		picked[i]=true;
		function(x+1, pick, picked);
		pick.pop_back();
		picked[i]=false;
	}
	return;
}
```

</details>

## 수행 시간 어림짐작하기

> 입력의 크기를 시간 복잡도에 대입해서 얻은 반복문 수행 횟수에 대해,  
> **1초**당 반복문 수행 횟수가 **1억**(10^8)을 넘어가면 시간 제한을 초과할 가능성이 있다.
>

그러나 알고리즘마다 추가적인 연산들이 있으니까 문제 풀이에 있어 위험하다고 할 수 있다.  
언제나 충분한 **여유**(10배)를 두자.

![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FNt5cW%2FbtqBZqoc97E%2FKYrwTbo8rbfk6EX8FJzrTK%2Fimg.png)

# BFS DFS

그래프에서 모든 정점을 방문해야는 방법 중 가장 대표적인 것은

너비 우선 탐색(BFS) 와 깊이 우선 탐색(DFS) 이다.

다음의 사진들은 트리를 대상으로 각각 BFS, DFS 를 적용한 것이다.

**너비 우선 탐색**
---------

![https://velog.velcdn.com/images/hs1430/post/85f28d8d-22a7-4e87-93b1-add232b85cc8/image.PNG](https://velog.velcdn.com/images/hs1430/post/85f28d8d-22a7-4e87-93b1-add232b85cc8/image.PNG)

BFS는 루트를 시작으로 탐색을 시작하면 먼저 루트의 자식을 차례대로 반복한뒤
루트 자식의 자식으로, 즉 루트에서 두 개의 간선을 거쳐서 도달할 수 있는 정점을 방문한다.
1 -> (2,3) -> (4,5,6,7,8)

BFS는 루트에서 거리 순으로 방문한다.

**깊이 우선 탐색**
---------

![https://velog.velcdn.com/images/hs1430/post/39d00912-01af-44e9-a5d4-12b2162a9623/image.PNG](https://velog.velcdn.com/images/hs1430/post/39d00912-01af-44e9-a5d4-12b2162a9623/image.PNG)

DFS는 루트의 자식 정점 하나를 방문한 다음 아래로 내려갈 수 있는 곳까지 내려간다.
더 이상 내려갈 수가 없으면 위로 되돌아오다가 내려갈 곳이 있으면 즉각 내려간다.

1 -> 2 -> 3 -> 4 -> 5.....

일반적인 그래프 G=(V,E)에 대한 BFS, DFS는 다음과 같다.

**너비 우선 탐색**
---------

![https://velog.velcdn.com/images/hs1430/post/379a6880-390a-4790-9361-2f50737f6740/image.png](https://velog.velcdn.com/images/hs1430/post/379a6880-390a-4790-9361-2f50737f6740/image.png)

BFS는 인접한 정점에 해당하는 정점을 모두 탐색한뒤
탐색한 정점과 인접하지만 탐색하지 않았던 정점을 탐색하는것을 반복한다.

**깊이 우선 탐색**
---------

![https://velog.velcdn.com/images/hs1430/post/3c616dd6-0e51-4247-8b95-88eb990b7693/image.png](https://velog.velcdn.com/images/hs1430/post/3c616dd6-0e51-4247-8b95-88eb990b7693/image.png)

DFS는 인접한 정점 중 하나를 선택해서 해당하는 정점의 끝까지 탐색하고 되돌아오는것을 반복한다.

그림 (b),(c),(d)에서 각각 인접한 다른 정점들이 존재하는데 이런 경우에는 자유롭게 원하는 정점을 선택해 방문해 주면 된다.

**BFS 알고리즘**
---------

BFS(G,s)
{

```
  for each v ∈ V-{s}
      visited[v] <- NO;
  visited[s] <- Yes;                 // s: 시작 정점
  enqueue(Q,s);(처음엔 1값만 가져옴)   // Q:큐, Q에서 s값을 읽어옴
  while (Q!=ø) {
      u <- dequeue(Q);              // Q값을 출력하여 u에 저장함
      for each v ∈ L(u)            // L(u) : 정점 u의 인접 정점 집합
          if (visited[v] = NO) then{
              visited[v] <- YES;
              enqueue(Q,v);
          }
  }

```

}

1. V-{s}에 해당하는 모든 정점에 visited[v] <- NO;
   시작 정점으로 쓰일 첫 정점값만 visited[s] <- Yes;
2. Q가 공집합이 아닐때까지 즉 모든 정점이 visited[v] <- Yes;가 될때까지
   while문 반복
3. q값 출력해서 u에 담고 u의 인접 정점 집합인 L(u)의 원소인 v에대해
   visited[v] = NO 라면 visited[v] <- YES; 로 바꾸고
   Q에서 v값을 가져온다.

BFS의 수행 시간은 O(V+E)이다. V = 정점, E = 간선

**DFS 알고리즘**
---------

DFS(v)
{

```
 visited[v] <- YES;
 for each x ∈ L(v) //L(v): 정점 v의 인접 정점 집합
      if (visited[x] = NO) then DFS(x);

```

}

1. 정점 v를 visited[v] <- YES; 처리한다
2. 이와 인접한 정점(for each x ∈ L(v))중 visited= NO 인 정점이 있다면
3. 다시 DFS 함수를 호출한다.

DFS의 수행 시간은 O(V+E)이다. V = 정점, E = 간선

위의 BFS,DFS는 모두 연결 그래프를 가정한 것이다.
만일 분리된 두 개 이상의 부분 그래프가 있다면
BFS나 DFS를 부분 그래프의 수만큼 수행해야 모든 정점을 방문할 수 있다.

**연결 그래프가 아닌 DFS의 알고리즘**
---------

DFS(G)
{

```
 for each v ∈ V
 visited[v] <- NO;

 for each v ∈ V
 if(visited[v] <- NO) then aDFS(v);

```

}

aDFS(v)
{

```
 visited[v] <- YES;
 for each x ∈ L(v) //L(v): 정점 v의 인접 정점 집합
      if (visited[x] = NO) then aDFS(x);

```

}

**연결 그래프가 아닌 그래프**

![https://velog.velcdn.com/images/hs1430/post/43c3d890-d8e6-4465-92ff-8423bb7ee9f6/image.png](https://velog.velcdn.com/images/hs1430/post/43c3d890-d8e6-4465-92ff-8423bb7ee9f6/image.png)

# 최단 경로 알고리즘

최단 경로 문제에서 입력 그래프의 유형은 크게 두 가지로

모든 간선의 가중치가 음이 아닌 **다익스트라 알고리즘**
음의 가중치가 존재하는 **벨만-포드 알고리즘**이 있다.

오늘은 이 중에서 다익스트라 알고리즘만 알아보려고 한다.

## Dijstra

다익스트라 알고리즘은 사이클이 없는 그래프이기 때문에,

하나의 시작 정점에서 다른 모든 정점까지 최단 경로를 구한다.

따라서 (n-1)개의 경로를 구하게 된다.

이와 같은 유형을 단일 시작점 최단 경로 문제 라고 한다.

해당 알고리즘에서는

임의의 두 정점간의 경로가 존재하지 않으면 해당 경로의 길이는 ∞로 정의하고

두 정점 사이에 간선이 없으면 가중치가 ∞인 간선이 있다 간주한다.

## **다익스트라 그래프**

![https://velog.velcdn.com/images/hs1430/post/b398a154-3405-4491-96bc-a66b41a109b4/image.png](https://velog.velcdn.com/images/hs1430/post/b398a154-3405-4491-96bc-a66b41a109b4/image.png)

1. 시작 정점 **r만 최단 거리 0으로 초기화하고** 다른 정점들의 최단 거리는 **모두 ∞로 초기화 한다**.


2. 정점 r을 집합 S의 첫번째 원소로 포함시키고 집합 S 즉 현재 r과 인접한 3개의 정점에 해당하는 곳을 각각 ∞에서 해당 정점에 도달할때의 **최단거리값
   즉 각각 (8,9,10)**으로 값을 바꿔준다.
   하지만 회색 상태는 값만 바뀐것이고 S집합에 포함시키는것은 아니다.
   이들중 가장 거리가 가까운 8을 집합 S에 포함시켜준다.


3. 집합 S에 8을 포함시키자, 8과 인접한 정점의 최단거리 역시 ∞에서 18로 바뀌었다.
   이 상태에서 S와 인접한 정점들의 **최단거리값은 각각 (9,10,18)이 된다**.
   따라서 9를 집합 S에 포함시켜준다.


4. 집합 S에 9가 추가되면서 최단거리값이 18에서 10으로 바뀌었고
   이 상태에서 **최단거리값은 (10, 11)이 된다**.
   따라서 10을 집합 S에 포합시켜준다.


5. 집합 S에 10이 추가되면서 새로운 정점이 ∞에서 12로 최단거리값이 변경되었다.
   이 상태에서 **최단거리값은 (11,12)이므로 11을 집합 S에 포함시킨다**.


6. 집합 S에 11이 추가되면서 새로운 2개의 정점이 각각 ∞에서 19로 최단거리값이 변경되었다.
   이 상태에서 **최단거리값은 (12,19,19)이므로 12를 집합 S에 포함시킨다**.


7. 집합 S에 12가 추가되면서 한개의 정점이 19에서 16으로 최단거리 값이 변경되었다.
   **집합 S에 16을 포함시키고 그뒤에 19을 포함시키며 마무리된다**.

## **다익스트라 알고리즘**

Dijstra(G,R)
G=(V,E): 주어진 그래프
r: 시작으로 삼을 정점
{

```
  S <- ∅;                 // S: 정점 집합
  for each u∈V           // 정점 r에서부터 정점 u에 이르는 최단거리
      d[u] <- ∞;         // 최단이기에 ∞사용, 최장거리 계산시에는 -∞
  d[r] <- 0;
  while (S!=V){
      u <- extractMin (V-S,d); //제일작은 값을 주는데, d[r]<-0;이므로 r을 주게된다
      S <- S∪{u};
      for each v∈L(u)   //L(u): 정점 u로부터 연결된 정점들의 집합
          if( v ∈ V-S and d[u] + w(u,v) < d[v])then{ //최단거리 교체용 if문 d[u] = 최단경로(**그림설명**)
              d[v] <- d[u] + w(u,v); //최단경로 수정
              prev[v] <- u; //삽입
          }
  }

```

}

extractMin(Q,d[])
{

```
  집합 Q에서 d값이 가장 작은 정점 u를 리턴한다; //d=최단거리

```

}

![https://velog.velcdn.com/images/hs1430/post/ad405b7d-81fe-4a21-8120-eb6b0a46c965/image.png](https://velog.velcdn.com/images/hs1430/post/ad405b7d-81fe-4a21-8120-eb6b0a46c965/image.png)

## **추가설명**

if( v ∈ V-S and d[u] + w(u,v) < d[v])then

v ∈ V-S 를 만족할때
d[u] + w(u,v) < d[v]이면 밑줄의 최단경로 수정으로 이동

r에서 v라는 정점으로 바로 이동할때 거리가 d[v]인데

r에서 u라는 다른 정점으로 이동할때 거리 d[u] + u에서 v로 이동할때의 거리를 합한 거리 w(u,v)가

d[v]보다 짧다면 d[v]값을 d[u] + w(u,v)로 변경한다는 내용이다.

위의 다익스트라 그래프에서 (c) -> (d) 과정이 이 과정을 통해 바뀐 것이다.

**수행시간**

다익스트라 알고리즘의 수행시간은 힙을 이용하면 O(ElogV)이다.

---

# 기본적인 정렬 알고리즘

- 기본적인 정렬 알고리즘

**선택정렬**

**버블정렬**

**삽입정렬**

## **선택 정렬**

선택 정렬은 원리가 간단한 정렬 알고리즘이다.

배열 A[1 . . . n]에서 가장 큰 원소를 찾아 이 원소와 배열의 끝자리에 있는
A[n]과 자리를 바꾼다.
해당 원소는 정렬이 완료되었으므로 다음 작업에서는 해당 원소를 제외한
나머지 원소들로 같은 동작을 반복한다.

선택정렬의 수행 시간은 (n-1)+(n-2)+...+2+1 = n(n-1)/2 이므로 항상 **O(n^2)** 이다.

---

![https://velog.velcdn.com/images/hs1430/post/a719000e-76f6-45c8-bb0b-2837c636092d/image.png](https://velog.velcdn.com/images/hs1430/post/a719000e-76f6-45c8-bb0b-2837c636092d/image.png)

---

## **선택 정렬 알고리즘**

selectionSort(A[],n)
{

```
 for last <- n downto 2{   // downto 는 to의 반대 4 downto 1 = 1234
      k <- theLargest(A, last);
      A[k] ↔ A[last];    // A[k]와 A[last]의 값 교환

  }

```

}

theLargest(A[],last)
{

```
 largest <- 1;

  for i <- 2 to last
     if (A[i] > A[largest]) then largest <- 1;
  return largest;

```

}

---

##  **파이썬을 이용한 선택 정렬 구현**

 ```
  def selection_sort(arr):
    for i in range(len(arr) - 1):
        min_idx = i
        for j in range(i + 1, len(arr)):
            if arr[j] < arr[min_idx]:
                min_idx = j
        arr[i], arr[min_idx] = arr[min_idx], arr[i]
 ```
 
---

##  **자바를 이용한 선택 정렬 구현**

 ```
 public class Selection {
    public static void sort(int[] arr) {
        for (int i = 0; i < arr.length - 1; i++) {
            int minIdx = i;
            for (int j = i + 1; j < arr.length; j++) {
                if (arr[j] < arr[minIdx])
                    minIdx = j;
            }
            swap(arr, i, minIdx);
        }
    }

    private static void swap(int[] arr, int a, int b) {
        int tmp = arr[a];
        arr[a] = arr[b];
        arr[b] = tmp;
    }
}
 ```

---

## **버블 정렬**

버블 정렬의 원리도 선택 정렬과 크게 다르지않다.

제일 큰 원소를 끝자리로 옮기는 작업을 반복하는 것이다.
차이점은 제일 큰 원소를 오른쪽으로 옮기는 방식이다.

선택정렬은 last와 제일 큰 원소의 위치를 바꾸는 방식이지만
버블 정렬은 왼쪽부터 오름차순으로 비교해가며 순서에 맞지않으면 서로 위치를 바꾼다.
이를 반복하면 처음 오른쪽 끝에 도달했을때 제일 큰 숫자가 last위치에 오게된다.

버블 정렬의 수행시간도 선택 정렬과 마찬가지로 (n-1)+(n-2)+...+2+1 = n(n-1)/2 이므로 항상 **O(n^2)**이다.
하지만 개선된 버블 정렬을 이용하면 최적의 경우 수행시간이 **O(n)** 이 된다.
이때 최적의 경우는 정렬이 완료되어있는 경우이다.

---

![https://velog.velcdn.com/images/hs1430/post/6b0de2f4-e074-4aba-8a96-bb1b078a1221/image.png](https://velog.velcdn.com/images/hs1430/post/6b0de2f4-e074-4aba-8a96-bb1b078a1221/image.png)

---

## **버블 정렬 알고리즘**

bubbleSort(A[], n)
{

```
  for last <- n downto 2
      for i to last-1
          if (A[i] > A[i+1]) then A[i] ↔ A[i+1];

```

}

- **개선된 버블 정렬 알고리즘**

bubbleSort(A[], n)
{

```
  for last <- n downto 2
      sorted <- TRUE;
      for i to last-1{
          if (A[i] > A[i+1]) then A[i] ↔ A[i+1];
          sorted <- FALSE;
      }
      if(sorted = TRUE) then return;
  }

```

}

---

## **파이썬을 이용한 버블 정렬 구현**

```
def bubble_sort(arr):
    for i in range(len(arr) - 1, 0, -1):
        for j in range(i):
            if arr[j] > arr[j + 1]:
                arr[j], arr[j + 1] = arr[j + 1], arr[j]

```

---

## **자바를 이용한 버블 정렬 구현**

```
public class Bubble {
    public static void sort(int[] arr) {
        for (int i = arr.length - 1; i > 0; i--) {
            boolean swapped = false;
            for (int j = 0; j < i; j++) {
                if (arr[j] > arr[j + 1]) {
                    swap(arr, j, j + 1);
                    swapped = true;
                }
            }
            if (!swapped) break;
        }
    }

    private static void swap(int[] arr, int a, int b) {
        int tmp = arr[a];
        arr[a] = arr[b];
        arr[b] = tmp;
    }
}

```

---

## **삽입 정렬**

삽입 정렬은 이미 정렬되어 있는 i개짜리 배열에 하나의 원소를 더 더하여
정렬된 i+1개짜리 배열을 만드는 과정을 반복한다.
선택 정렬과 버블 정렬이 n개 짜리 배열 에서 아직 **정렬되지 않은 배열**의 크기를 하나씩 줄이는 데 반해
삽입 정렬은 한 개 짜리 배열에서 시작해서 **이미 정렬된 배열**의 크기를 하나씩 늘리는 정렬이다.

삽입 정렬의 수행시간은 1+2+3+...+(n-1) = n(n-1)/2 이므로 **O(n^2)** 이지만
배열이 완전히 정렬되어 있다면 while 루프를 한 번도 수행하지 않아 최적의 수행시간은 **O(n)** 이된다.

---

![](https://velog.velcdn.com/images/hs1430/post/1468a15c-2529-4273-aa79-887d34f4076c/image.png)

---

## **삽입 정렬 알고리즘**

insertionSort(A[],n)
{

     for i <- 2 to n {
         loc <- i-1;
         newItem <- A[i]; // 이 지점에서 A[1~i-1]은 정렬되어있는 상태
     
         while ( loc >= 1 and newItem < A[loc]) {
            A[loc+1] <- A[loc];
            loc--; // newItem과 loc를 하나씩 비교하며 마지막엔 탈출
     
         }
         
         A[loc+1] <- newItem; //while문을 거치지 않는 경우 맨앞에 추가
         
     }


}


---

##  **파이썬을 이용한 삽입 정렬 구현**

 ```
  def insertion_sort(arr):
    for end in range(1, len(arr)):
        for i in range(end, 0, -1):
            if arr[i - 1] > arr[i]:
                arr[i - 1], arr[i] = arr[i], arr[i - 1]
 ```
 
---

##  **자바를 이용한 삽입 정렬 구현**

 ```
 public class Insertion {
    public static void sort(int[] arr) {
        for (int end = 1; end < arr.length; end++) {
            int toInsert = arr[end];
            int i = end;
            while (i > 0 && arr[i - 1] > toInsert) {
                arr[i] = arr[i - 1];
                i--;
            }
            arr[i] = toInsert;
        }
    }
}
 ```
 
---

# 고급 정렬 알고리즘

- 고급 정렬 알고리즘

**병합 정렬**

**퀵 정렬**

**힙 정렬**

## 병합 정렬(Merge Sort)

병합 정렬은 입력을 반으로 나누고, 이렇게 나눈 전반부와 후반부를 각각 독립적으로 정렬한다.
마지막으로 정렬된 두 부분을 합쳐서,즉 병합하여 정렬된 배열을 얻는다.
여기서 전반부,후반부를 정렬할 때도 반으로 나눈 다음 정렬해서 병합한다.
이를 재귀적으로 반복하는것이 병합 정렬이다.

병합 정렬의 수행시간은 항상 **O(nlogn)** 이다.

---

![](https://velog.velcdn.com/images/hs1430/post/711043b9-8313-423e-ae68-5b0425dfd05a/image.png)
전체적인 부분

![](https://velog.velcdn.com/images/hs1430/post/2893f75f-7412-4bb4-8e48-1abb5c4318b8/image.png)

전반부 후반부 정렬 완료후

---

## 병합 정렬 알고리즘

MergeSort(A[],p,r)  // A[p ... r]
{

     if(p<r) then {
        q <-⌊(p+r)/2⌋;         //p,r의 중간 지점 계산
        mergeSort(A,p,q);     // 전반부 정렬
        mergeSort(A,q+1,r);   // 후반부 정렬
        merge(A,p,q,r);       // 병합
     }


}

merge(A[]p,q,r)
{

     i <- p; j<-q+1; t<-1;
     
     while (i <= q and j <= r) {
        if ( A[i] <= A[j])
        then tmp[t++] <- A[i++]; //조건에 따라 tmp에 값저장
        else tmp[t++] <- A[j++];
    }                           //밑부분은 한쪽 배열이 모두 tmp에 저장되고 남은 부분 정렬
    while(i<=q)                //왼쪽 부분 배열이 남은 경우
       tmp[t++] <- A[i++];
       
    while(j<=r)               //오른쪽 부분 배열이 남은 경우
       tmp[t++] <- A[j++];
       
       i <- p; t <- 1;
       while(i<=r)               // 결과를 A[p ... r]에 저장
       A[i++] <- tmp[t++];



}
 
---

## **파이썬을 이용한 병합 정렬 구현**

``` 
def merge_sort(arr):
    if len(arr) < 2:
        return arr

    mid = len(arr) // 2
    low_arr = merge_sort(arr[:mid])
    high_arr = merge_sort(arr[mid:])

    merged_arr = []
    l = h = 0
    while l < len(low_arr) and h < len(high_arr):
        if low_arr[l] < high_arr[h]:
            merged_arr.append(low_arr[l])
            l += 1
        else:
            merged_arr.append(high_arr[h])
            h += 1
    merged_arr += low_arr[l:]
    merged_arr += high_arr[h:]
    return merged_arr
```
---

## **자바를 이용한 병합 정렬 구현**

```
public class MergeSorter {
    public static int[] sort(int[] arr) {
        if (arr.length < 2) return arr;

        int mid = arr.length / 2;
        int[] low_arr = sort(Arrays.copyOfRange(arr, 0, mid));
        int[] high_arr = sort(Arrays.copyOfRange(arr, mid, arr.length));

        int[] mergedArr = new int[arr.length];
        int m = 0, l = 0, h = 0;
        while (l < low_arr.length && h < high_arr.length) {
            if (low_arr[l] < high_arr[h])
                mergedArr[m++] = low_arr[l++];
            else
                mergedArr[m++] = high_arr[h++];
        }
        while (l < low_arr.length) {
            mergedArr[m++] = low_arr[l++];
        }
        while (h < high_arr.length) {
            mergedArr[m++] = high_arr[h++];
        }
        return mergedArr;
    }
}
```
---

## 퀵 정렬

퀵 정렬은 평균적으로 가장 좋은 성능을 가져 현장에서 가장 많이 쓰는 정렬 알고리즘이다.
**정렬 후 병합** 을 하는 **병합 정렬** 과는 달리 **퀵정렬** 은 **병합을 한뒤에 정렬** 을 한다.

퀵 정렬의 수행시간은 분할할때 한쪽에만 다 몰리는 최악의 경우에만 **O(n^2)** 이고
나머지 경우에는 **O(nlogn)** 이다.

---

![](https://velog.velcdn.com/images/hs1430/post/9b57cabc-6519-49f1-b610-b88f9785f7c0/image.png)

![](https://velog.velcdn.com/images/hs1430/post/ecb1b146-a295-4f1c-9003-902e78164972/image.png)


---

## 퀵 정렬 알고리즘

quickSort(A[],p,r)  // A[p ... r]
{

     if(p<r) then {
        q <- partition(A,P,R);         // 분할
        quickSort(A,p,q-1);     // 전반부 정렬
        quickSort(A,q+1,r);   // 후반부 정렬
               
     }


}

partition(A[]p,r)
{

     x <- A[r];
     i <- p-1;
     for j <- p to r-1
          if (A[j]<=x) then A[++i] <-> A[j];
     
     A[i+1] <-> A[r];
     return i+1;



}
 
---

## **파이썬을 이용한 퀵 정렬 구현**

``` 
def quick_sort(arr):
    if len(arr) <= 1:
        return arr
    pivot = arr[len(arr) // 2]
    lesser_arr, equal_arr, greater_arr = [], [], []
    for num in arr:
        if num < pivot:
            lesser_arr.append(num)
        elif num > pivot:
            greater_arr.append(num)
        else:
            equal_arr.append(num)
    return quick_sort(lesser_arr) + equal_arr + quick_sort(greater_arr)
```
---

## **자바를 이용한 퀵 정렬 구현**

```
public class QuickSorter {
    public static List<Integer> quickSort(List<Integer> list) {
        if (list.size() <= 1) return list;
        int pivot = list.get(list.size() / 2);

        List<Integer> lesserArr = new LinkedList<>();
        List<Integer> equalArr = new LinkedList<>();
        List<Integer> greaterArr = new LinkedList<>();

        for (int num : list) {
            if (num < pivot) lesserArr.add(num);
            else if (num > pivot) greaterArr.add(num);
            else equalArr.add(num);
        }

        return Stream.of(quickSort(lesserArr), equalArr, quickSort(greaterArr))
                .flatMap(Collection::stream)
                .collect(Collectors.toList());
    }
}
```
---

## 힙 정렬

힙은 이진 트리로서 맨 아래 층을 제외하고는 완전히 채워져 있고 맨 아래층은 왼쪽부터 꽉 채워져 있다.
힙의 모든 노드는 하나씩의 값을 가지고 있는데 다음 힙성질을 만족한다.

**각 노드의 값은 자기 자식의 값보다 작다(힙에 값이 같은 원소가 두 개 이상 있는 경우에는 "작다" 대신 "작거나 같다")**

힙 정렬의 수행 시간은 항상 **O(nlogn)** 이다.

---

![](https://velog.velcdn.com/images/hs1430/post/8c52fb63-4649-4775-82a9-3b3b4782bd51/image.png)

![](https://velog.velcdn.com/images/hs1430/post/99a37f90-5320-4950-a832-0c054afaf9e4/image.png)

![](https://velog.velcdn.com/images/hs1430/post/6e854097-423b-4223-b619-cc88aa5bae1f/image.png)

위의 자료는 값이 큰순서대로 정렬된 것이고 실제로는 반대로 값이 작은 것이 루트에 와야 한다.


![](https://velog.velcdn.com/images/hs1430/post/446cafd3-9be4-44a3-9691-389f514f0f0e/image.png)

힙을 만들고 루트에 있는 원소를 제거하여 다른곳에 저장하는 동시에 배열의 맨 끝 값을 루트 노드로 옮긴다. 이때 힙의 성질이 깨지게 되는데
깨진 힙을 heapify를 통해 다시 만드는것을 반복하며 정렬을 하는 과정이다.

---

## 힙 정렬 알고리즘

buildHeap(A[],n)
{
  ```
  for i <-⌊(n/2⌋ downto 1
      heapify(A,i,n);
 
 ```
}

heapify(A[],k,n) //힙 성질을 만족하도록 고쳐준다. k = 루트
{

  ```
  left <- 2k; right <- 2k+1;
  
  // 작은 자식 구하기 smaller = 2k와 2k+1 중에 작은 원소
  
  if(right<=n) then {           // k 의 자식이 두개인 경우
    if (A[left] < A[right]) then smaller <- left;  //둘 중 작은애 선택
                            else smaller <- right;
  }
  
  else if (left<=n) then smaller <- left; // k의 왼쪽 자식만 있는 경우
  else return;                            // A[k]의 리프노드 끝남
  
  if (A[smaller] < A[k]) then { //자기 자식중 가장 작은 값이 자신보다 작다면 바꾸고 다시 heap 재귀한다.
      A[k] <-> A[smaller];
      heapify (A,smaller,n);
  }
  ```
}

heapSort(A,n)  
{

     buildHeap(A,n);
     for i <- n downto 2 {
         A[1] <-> A[i];   // 루트의 원소를 배열의 마지막 부분과 교환하여 저장하는 방식으로, 역순으로 저장된다.
         heapify(A,1,i-1);
     }


}
 
---

## **파이썬을 이용한 힙 정렬 구현**

``` 
def heapify(unsorted, index, heap_size):
  largest = index
  left = 2 * index + 1
  right = 2 * index + 2
  
  if left < heap_size and unsorted[right] > unsorted[largest]:
    largest = left
    
  if right < heap_size and unsorted[right] > unsorted[largest]:
    largest = right
    
  if largest != index:
    unsorted[largest], unsorted[index] = unsorted[index], unsorted[largest]
    heapify(unsorted, largest, heap_size)

def heap_sort(unsorted):
  n = len(unsorted)
  
  for i in range(n // 2 - 1, -1, -1):
    heapify(unsorted, i, n)
    
  for i in range(n - 1, 0, -1):
    unsorted[0], unsorted[i] = unsorted[i], unsorted[0]
    heapify(unsorted, 0, i)

  return unsorted
```
---

## **자바를 이용한 힙 정렬 구현**

```
import java.util.*;

class Main {
    public static void main(String[] args) {

        Scanner scanner = new Scanner(System.in);

        System.out.println("배열의 크기를 입력하시오");
        int n = scanner.nextInt()+1;
        int[] arr = new int[n];

        System.out.println("배열에 들어갈 숫자를 입력하시오");
        for(int i = 1; i<n; i++) {
            arr[i] = scanner.nextInt();
        }

        System.out.println("배열에 입력한 숫자");
        System.out.println(Arrays.toString(arr));

        buildHeap(arr); //배열을 힙으로 만드는 메서드

        System.out.println("힙으로 변경한 배열");
        System.out.println(Arrays.toString(arr));

        heapSort(arr); //힙을 이용해서 정렬하는 메서드

        System.out.println("정렬 완료된 배열");
        System.out.println(Arrays.toString(arr));
    }

    static void heapSort(int[] arr) {
        int eNN = arr.length-1;
        while(eNN > 1) {
            swap(arr, 1, eNN);
            eNN--;
            pushDown(arr, 1, eNN);
        }
    }

    //eNN = endNodeNumber
    //tNN = tempNodeNumber
    static void buildHeap(int[] arr) {
        int eNN = arr.length - 1; // 마지막 노드
        int tNN = eNN/2 + 1; //1번째 리프노드 번호

        while(tNN > 1) {
            tNN--; // 자식을 가지고 있는 마지막 노드부터 시작
            pushDown(arr, tNN, eNN);
        }
    }

    static void pushDown(int[] arr, int tNN, int eNN) {
        int y = findLarger(arr, tNN, eNN); 
        //자식 노드중에서 루트 노드보다 더 큰 값을 가지는 노드 번호 얻어냄

        while(arr[tNN] < arr[y]){
            swap(arr, tNN, y);
            tNN = y;
            y = findLarger(arr, tNN, eNN);
            // leaf노드 쪽으로 내려가면서 값의 제자리를 찾아간다.
        }
    }

    static int findLarger(int[] arr, int tNN, int eNN){
        int tmp = tNN*2+1; //오른쪽 자식 노드의 번호
        int y = tNN;

        if(tmp <= eNN){//자식 노드가 두개인 경우
            if(arr[tNN] < arr[tmp]) //오른쪽 자식 노드의 value가 더 크다면
                y = tmp;
            if(arr[y] < arr[tmp-1]) //왼쪽 자식 노드의 value가 더 크다면
                y = tmp-1;
        }
        else if(tmp-1 <= eNN){ //자식 노드가 1개인 경우
            if(arr[tNN] < arr[tmp-1]) // 자식 노드의 value가 더 크다면
                y = tmp-1;
        }
        return y;
    }

    static void swap(int[] arr, int a, int b){
        int tmp = arr[a];
        arr[a] = arr[b];
        arr[b] = tmp;
    }
}
```

---

- 구현되어있는 메서드 사용

```class Main {
    public static void main(String[] args) {
        Queue pq = new PriorityQueue();

        int[] arr = {3,1,5,2,4};
        for(int i=0; i<arr.length; i++)
            pq.offer(arr[i]);

        System.out.println(pq);

        Object obj = null;
        int a = 0;
        while((obj = pq.poll())!=null)
            System.out.printf("%s ",obj);
    }
}
```


---





**참조:**
- 종만북(알고리즘 문제 해결 전략)
- [https://www.101computing.net/big-o-notation/](https://www.101computing.net/big-o-notation/)
- [https://softwareengineering.stackexchange.com/questions/347748/can-sub-linear-still-be-a-straight-line](https://softwareengineering.stackexchange.com/questions/347748/can-sub-linear-still-be-a-straight-line)
- [https://lamfo-unb.github.io/2019/04/21/Sorting-algorithms/](https://lamfo-unb.github.io/2019/04/21/Sorting-algorithms/)
- [https://baactree.tistory.com/26](https://baactree.tistory.com/26)
- [https://blog.encrypted.gg/922](https://blog.encrypted.gg/922)
- 쉽게 배우는 알고리즘
- https://www.daleseo.com/sort-bubble/
- https://www.daleseo.com/sort-insertion/
- https://www.daleseo.com/sort-selection/
- https://www.daleseo.com/sort-merge/
- https://www.daleseo.com/sort-quick/
- https://good-potato.tistory.com/m/50
- https://seanpark11.tistory.com/68