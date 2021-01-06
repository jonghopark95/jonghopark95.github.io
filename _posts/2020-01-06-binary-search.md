---
layout: post
title: 이진 탐색 알고리즘
summary: 이진 탐색 알고리즘 정리
featured-img: algorithm
---

# 시작하며

PS 공부를 하며 이진 탐색 알고리즘에 대해 간략히 정리해봤습니다.  
예제 코드는 파이썬으로 작성되었습니다.

---

## 순차 탐색

순차 탐색이란 리스트 안에 있는 특정 데이터를 찾기 위해 앞에서부터 데이터를 하나씩 차례대로 확인하는 방법이다.

```python
def sequential_search(n, target, arr):
    for i in range(n):
        if array[i] == target:
            return i + 1
```

## 이진 탐색

이진 탐색이랑 배열 내부의 데이터가 정렬되어 있어야만 사용할 수 있는 알고리즘이다.
이진 탐색은 시작점, 끝점, 중간점을 활용한다.
찾으려는 데이터와 중간점에 있는 데이터를 비교하여 원하는 데이터를 찾는다.

```python
# 재귀 함수 이용
def binary_search(array, target, start, end):
    if start > end:
        return None
    mid = (start + end) // 2
    if array[mid] == target:
        return mid
    elif array[mid] > target:
        return binary_search(arr, target, start, mid-1)
    else:
        return binary_search(arr, target, mid+1, end)

# 반복문 이용
def binary_search(array, target, start, end):
    while start <= end:
        mid = (start + end) // 2
        if array[mid] == target:
            return mid
        elif arry[mid] > target:
            end = mid - 1
        else:
            start = mid + 1
    return None
```

## 트리 자료구조

그래프 자료구조의 일종으로 데이터베이스 시스템이나 파일 시스템 같은 곳에서 많은 양의 데이터를 관리하기 위한 목적으로 사용한다.

- 트리는 부모, 자식 노드의 관계로 표현된다.
- 최상단 노드를 루트 노드라도 한다.
- 최하단 노드를 단말 노드라고 한다.
- 일부를 떼어내도 트리 구조이며 서브 트리라 한다.
- 파일 시스템과 같이 계층적이고 정렬된 데이터를 다루기에 적합하다.

## 이진 탐색 트리

이진 탐색 트리는 다음과 같은 특징을 가진다.

- 부모 노드보다 왼쪽 자식 노드가 작다.
- 부모 노드보다 오른족 자식 노드가 크다.

이진 탐색 문제는 입력 데이터가 많거나, 탐색 범위가 매우 넓다.
따라서 데이터의 개수가 1000만 개를 넘어가거나 탐색 범위의 크기가 1000억 이상이라면 이진 탐색 트리를 고려하는 편이 좋다.

이렇게 입력 데이터의 개수가 많은 문제에 input()을 사용하면 시간 초과가 될 수 있으므로 sys 라이브러리의 readline()함수를 사용하자.

```python
import sys

data = sys.stdin.readline().rstrip()
```

sys 라이브러리를 사용할 경우 한 줄 입력 후 rstrip()을 호출해야 한다. 호출하지 않을 경우 enter가 입력 값으로 들어간다.
