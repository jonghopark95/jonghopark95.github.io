---
layout: post
title: Dynamic Programming
summary: PS에서 자주 사용되는 개념인 Dynamic Programming에 대해 알아봅니다.
featured-img: algorithm
---

## 다이나믹 프로그래밍이란?

다이나믹 프로그래밍은 다음 조건을 만족할 경우 사용할 수 있다.

1. 큰 문제를 작은 문제로 나눌 수 있다.
2. 작은 문제에서 구한 정답은 그것을 포함하는 큰 문제에도 동일하다.

피보나치 수열은 이러한 조건을 만족하는 대표 문제이다. 이 문제를 메모이제이션 기법을 사용해서 해결해보자. 문제를 풀기 전, Memoization 기법은 Dynamic Programming을 구현하는 방법 중 한 종류로, 한 번 구한 결과를 메모리 공간에 메모해두고 같은 식을 다시 호출하면 메모한 결과를 그대로 가져오는 기법이다.

메모이제이션은 리스트에 저장하는 방식으로 구현할 수 있다. 한 번 구한 정보를 리스트에 저장하고 다이나믹 프로그래밍을 재귀적으로 수행하다가 같은 정보가 필요할 경우 구한 정답을 리스트에서 가져오는 것이다.

```python
d = [0] * 100

def fibo(x):
  if x==1 or x==2:
    return 1
  if d[x] != 0:
    return d[x]
  d[x] = fibo(x - 1) + fibo(x - 2)
  return d[x]

print(fibo(99))
```

## 탑다운? 바텀업?

재귀 함수를 이용하여 dynamic programming 코드를 작성하는 방법을 큰 문제를 해결하기 위해 작은 문제를 호출한다고 하여 Top-Down 방식이라고 한다. 반면, 단순히 반복문을 이용하여 소스코드를 작성하는 경우 작은 문제로부터 차근차근 답을 도출한다고 하여 Bottom-Up 방식이라고 한다. 피보나치 수열 문제를 Bottom-Up 방식으로 풀면 다음과 같다.

```python
d = [0] * 100

d[1] = 1
d[2] = 1
n = 99

for i in range(3, n + 1):
  d[i] = d[i - 1] + d[i - 2]

print(d[n])
```

탑다운 방식은 "하향식" 방식이라고도 하며 바텀업 방식은 "상향식" 이라고도 한다. Dynamic-Programming의 전형적인 형태는 상향식 방식이다. Bottom-Up 방식에서 사용되는 결과 저장용 리스트는 "DP 테이블" 이라고 부르며 메모이제이션은 Top-Down 방식에 국한되어 사용되는 표현이다.
