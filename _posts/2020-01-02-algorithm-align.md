---
layout: post
title: 정렬 알고리즘
summary: 정렬 알고리즘 정리
featured-img: algorithm
---

# 시작하며

PS 공부를 하며 알고리즘 정렬에 대해 간략히 정리해봤습니다.  
예제 코드는 파이썬으로 작성되었습니다.

[선택 정렬](#select_align)  
[삽입 정렬](#insert_align)  
[퀵 정렬](#quick_align)

---

<a name="select_align"></a>

## 선택 정렬

데이터가 무작위로 여러 개 있을 때, 이 중 가장 작은 데이터를 선택해 맨 앞에 있는 데이터와 바꾸고, 그 다음 작은 데이터를 선택해 앞에서 두 번째 데이터와 바꾸는 과정을 반복하면 모든 데이터가 정렬될 것이다.

가장 원시적인 방법으로 매번 가장 작은 것을 '선택' 한다는 의미에서 선택 정렬 알고리즘이라고 한다.

```python
array = [7, 5, 9, 0, 3, 1, 6, 2, 4, 8]

for i in range(len(array)):
    min = i
    for j in range(i+1, len(array)):
        if array[min] > array[j]:
            min = j
    array[i], array[min] = array[min], array[i] #swap

print(array)
```

### 시간 복잡도

연산 횟수는 N + (N - 1) + (N - 2) + ... + 2로 볼 수 있다. 따라서 시간 복잡도는 빅오 표기법으로 **_O(N^2)_** 으로 표기할 수 있다.

---

<a name="insert_align"></a>

## 삽입 정렬

삽입 정렬은 특정한 데이터를 적절한 위치에 '삽입' 한다는 의미에서 삽입 정렬이라고 부른다.  
삽입 정렬은 첫 번째 데이터는 정렬되어 있다고 보고 두 번째 데이터에서 부터 시작한다.  
그 이전의 정렬된 데이터들과 비교하여 삽입할 위치를 정한다.

삽입 정렬은 정렬이 이루어진 원소는 항상 정렬을 유지하고 있다는 특징이 있다.  
이러한 특징 때문에 삽입될 데이터보다 작은 데이터를 만나면 그 위치에서 멈추면 된다.

```python
array = [7, 5, 9, 0, 3, 1, 6, 2, 4, 8]

for i in range(1, len(array)):
    for j in range(i, 0, -1):
        if array[j] < array[j-1]:
            array[j-1], array[j] = array[j], array[j-1]
        else:
            break

print(array)
```

### 시간 복잡도

삽입 정렬 또한 **_O(N^2)_** 이다. 그러나 리스트의 데이터가 거의 정렬되어 있는 상태라면 매우 빠르게 동작한다. 최선의 경우 **_O(N)_**의 시간 복잡도를 가진다.

---

<a name="quick_align"></a>

## 퀵 정렬

퀵 정렬의 identity는 다음과 같다.

> _기준 데이터를 설정하고 그 기준보다 큰 데이터와 작은 데이터의 위치를 바꾸자._

퀵 정렬에는 pivot이 사용된다.

특정한 리스트에서 피벗을 설정하여 정렬을 수행한 후에, 피벗을 기준으로 왼쪽, 오른쪽 리스트에서 각각 다시 정렬을 수행한다.

퀵 정렬은 **재귀 함수** 형태로 작성하였을 때 구현이 매우 간단해진다.
재귀 함수와 동작 원리가 같다면, 종료 조건도 있어야 할 것이다.

퀵 정렬의 종료 조건은 리스트의 데이터 개수가 1개인 경우이다. 리스트의 원소가 1개라면 분할이 불가능하다.

### 전통적 형태의 퀵 정렬 소스코드

```python
array = [7, 5, 9, 0, 3, 1, 6, 2, 4, 8]

def quick_sort(array, start, end):
    if start >= end: #원소가 1개인 경우 종료
        return
    pivot = start
    left = start + 1
    right = end
    while left <= right:
        while left <= end and array[left] <= array[pivot]:
            left += 1   # 피벗보다 큰 데이터 탐색
        while right > start and array[right] >= array[pivot]:
            right -= 1  # 피벗보다 작은 데이터 탐색
        if left > right:   # 교차되는 경우 작은 데이터와 피벗 교체
            array[right], array[pivot] = array[pivot], array[right]
        else:   # 엇갈리지 않을 경우 작은 데이터와 큰 데이터 교체
            array[left], array[right] = array[right], array[left]
    quick_sort(array, start, right-1)
    quick_sort(array, right+1, end)

quick_sort(array, 0, len(array)-1)

print(array)
```
