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


**참조:**
- 종만북(알고리즘 문제 해결 전략)
- [https://www.101computing.net/big-o-notation/](https://www.101computing.net/big-o-notation/)
- [https://softwareengineering.stackexchange.com/questions/347748/can-sub-linear-still-be-a-straight-line](https://softwareengineering.stackexchange.com/questions/347748/can-sub-linear-still-be-a-straight-line)
- [https://lamfo-unb.github.io/2019/04/21/Sorting-algorithms/](https://lamfo-unb.github.io/2019/04/21/Sorting-algorithms/)
- [https://baactree.tistory.com/26](https://baactree.tistory.com/26)
- [https://blog.encrypted.gg/922](https://blog.encrypted.gg/922)
