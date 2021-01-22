---
layout: post
title: 최단 경로 알고리즘
summary: 최단 경로 알고리즘에 대해 알아봅니다.
featured-img: algorithm
---

## 다익스트라 최단 경로 알고리즘

다익스트라 알고리즘은 그래프에서 여러 개의 노드가 있을 때, 특정한 노드에서 출발하여 다른 노드로 가는 각각의 최단 경로를 구해주는 알고리즘이다. '음의 간선'이 없을 때 정상적으로 동작하며, 현실 세계의 간선은 음의 간선으로 표현되지 않으므로 실제 GPS 소프트웨어의 기본 알고리즘으로 채택되곤 한다.

다익스트라 최단 경로 알고리즘은 그리디 알고리즘으로 분류된다. 매번 '가장 비용이 적은 노드'를 선택해서 임의의 과정을 반복하기 때문이다. 원리는 다음과 같다.

1. 출발 노드를 설정한다.
2. 최단 거리 테이블을 초기화한다.
3. 방문하지 않은 노드 중에서 최단 거리가 가장 짧은 노드를 선택한다.
4. 해당 노드를 거쳐 다른 노드로 가는 비용을 계싼하여 최단 거리 테이블을 갱신한다.
5. 위 과정을 거쳐 3과 4번을 반복한다.