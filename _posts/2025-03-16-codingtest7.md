---
layout: single
title: "파이썬 - 시간 여행자의 최단 여정"
categories: coding
tag: [알고리즘, 코딩테스트, 누적 합, Brute Force, Python]
toc: true
author_profile: false
sidebar:
  nav: "docs"
---

## 문제
당신은 시간 여행자가 되어 `N`개의 시간 좌표를 순서대로 방문해야 한다. <br>
각 시간 좌표는 **2차원 공간** 상의 한 점으로 주어지며, 당신은 첫 번째 좌표에서 시작하여 마지막 좌표까지 순서대로 이동해야 한다.

하지만 **시간 왜곡 장치**를 한 번 사용할 수 있다. 이 장치는 특정 시간 좌표 하나를 **건너뛸** 수 있도록 해주며, 더 빠르게 목적지에 도착할 수 있다. <br>
다만, 시간 왜곡 장치를 사용할 때 특정 시간은 시작점과 도착점은 제외한다.

당신의 목표는 한 개의 시간 좌표를 건너뛸 때, 이동 거리의 최솟값을 구하는 것이다.

이동 거리는 맨해튼 거리(Manhattan Distance)로 계산된다. 두 점 `(x1, y1)`과 `(x2, y2)` 사이의 맨해튼 거리는 다음과 같다.

<div class="notice--info">
|x1 - x2| + |y1 - y2|
</div>

### 입력 형식
첫 번째 줄에 시간 좌표의 개수 N이 주어진다. <br>
이후 N개의 줄에 각 시간 좌표 `x y`가 주어진다. <br>
시간 좌표는 입력 순서대로 방문한다.

### 출력 형식
한 개의 시간 좌표를 건너뛰었을 때, 이동 거리의 최솟값을 출력한다.

### 입출력 예제
입력
```
5
9 -9
-9 -4
1 -8
2 9
1 1
```
출력
```
36
```

### 예제 설명
예제에서 원래 경로의 총 이동 거리는 `(|-9 - 9|+|-4 - (-9)|) + (|1 - (-9)|+|-8 - (-4)|) + (|2 - 1|+|9 - (-8)|) + (|1 - 2|+|1 - 9|) = 23 + 14 + 18 + 9 = 64`이다.

지점을 건너뛰면 다음과 같다.
1. `(-9, 4)`를 건너뛴다면
  - `(|1 - 9|+|-8 - (-9)|) + (|2 - 1|+|9 - (-8)|) + (|1 - 2|+|1 - 9|) = 9 + 18 + 9 = 36`
2. `(1, -8)`를 건너뛴다면
  - `(|-9 - 9|+|-4 - (-9)|) + (|2 - (-9)|+|9 - (-4)|)  + (|1 - 2|+|1 - 9|) = 23 + 24 + 9 = 56`
3. `(2, 9)`를 건너뛴다면
  - `(|-9 - 9|+|-4 - (-9)|) + (|1 - (-9)|+|-8 - (-4)|) + (|1 - 1|+|1 - (-8)|)  = 23 + 14  + 9 = 46`

따라서 `(-9, 4)`를 건너뛰었을 때 36이 최소 이동 거리임을 알 수 있다.

## 풀이법 1
```python
import sys
n = int(input())
points = [list(map(int, input().split())) for _ in range(n)]

ans = sys.maxsize
for i in range(1, n-1):
    skip_points = points[:i] + points[i+1:]
    
    dist = 0
    for j in range(1, n-1):
        dist += abs(skip_points[j][0] - skip_points[j-1][0]) + abs(skip_points[j][1] - skip_points[j-1][1])
    ans = min(dist, ans)

print(ans)
```
1. 최소 거리 계산을 위해 초기값을 매우 큰 값으로 설정한다.
2. 첫 번째 반복문의 `i`는 건너뛸 점의 인덱스이다. 시작점 (0번)와 마지막점 (N - 1번)은 건너뛸 수 없어 1번부터 N-2까지만 순회한다.
3. `i`번째 점을 제외한 새로운 리스트 `skip_points`를 만든다.
4. 두 번째 반복문을 통해 맨해튼 거리를 계산하여 `i`번째 점을 제외한 이동 거리 `dist`를 구한다.
5. 최소 거리를 업데이트하여 이동 거리의 최솟값을 구한다.

## 풀이법 2
```python
import sys

n = int(input())
points = [list(map(int, input().split())) for _ in range(n)]

ans = sys.maxsize
dist = []
total_dist = 0

for i in range(1, n):
  dist.append(abs(points[i][0] - points[i-1][0]) + abs(points[i][1] - points[i-1][1]))
  total_dist += abs(points[i][0] - points[i-1][0]) + abs(points[i][1] - points[i-1][1])

for i in range(1, n-1):
  ans = min(ans, total_dist - (dist[i-1] + dist[i]) + (abs(points[i+1][0] - points[i-1][0]) + abs(points[i+1][1] - points[i-1][1])))

print(ans)
```
### 아이디어
예를 들어, 4개의 좌표가 있다고 가정하자.

<div class="notice--info">
<ol>
  <li>(x1, y1)</li>
  <li>(x2, y2)</li>
  <li>(x3, y3)</li>
  <li>(x4, y4)</li>
</ol>
</div>

4개의 지점을 이동하는 거리는 `(|x2 - x1| + |y2 - y1|) + (|x3 - x2| + |y3 - y2|) + (|x4 - x3| + |y4 - y3|)` 이다. 이를 `total_dist`라고 하자.

만약 건너뛸 지점을 <code>(x<sub>i</sub>, y<sub>i</sub>)</code>라고 하자. 이 지점을 건너뛰어 이동한 거리는 다음과 같다.
<div class="notice--info">
total_dist - (|x<sub>i</sub> - x<sub>i-1</sub>| + |y<sub>i</sub> - y<sub>i-1</sub>| + |x<sub>i+1</sub> - x<sub>i</sub>| + |y<sub>i+1</sub> - y<sub>i</sub>|) + (|x<sub>i+1</sub> - x<sub>i-1</sub>| + |y<sub>i+1</sub> - y<sub>i-1</sub>|)
</div>

### 풀이법 2 코드 설명
1. `dist` : 연속된 두 점 사이의 맨해튼 거리를 저장할 리스트
2. `total_list` : 모든 점을 방문할 때 총 이동 거리
3. 반복문을 순회하면서 (`1`부터 `n-1` 인덱스까지) `dist`에 거리를 저장한다. 이 때 `dist[i-1]`은 <code>(x<sub>i-1</sub>, y<sub>i-1</sub>)</code>에서 <code>(x<sub>i</sub>, y<sub>i</sub>)</code> 간의 이동 거리이다.
4. 동시에 모든 점에 대한 총 거리를 누적하여 `total_list`에 저장한다.
5. 반복문을 순회하면서 (`1`부터 `n-1` 인덱스까지) `i`번째 점을 제거할 경우의 이동 거리를 계산한다. 제거하는 경우는 다음과 같다. <br><br>
  5-1. 기존의 `dist[i-1]` (이전 점과 현재 점 사이 거리) + `dist[i]` (현재 점과 다음 점 사이 거리)를 제거 <br>
  5-2. <code>(x<sub>i-1</sub>, y<sub>i-1</sub>)</code>에서 <code>(x<sub>i+1</sub>, y<sub>i+1</sub>)</code> 로 이동하는 거리를 추가 <br>
6. 새로운 총 거리에 대한 최솟값을 구한다.

## 풀이법 비교
1. 풀이법 1
- 중간 점을 제거한 후 리스트를 새로 만들어 거리를 계산한다. (Brute Force)
- 매 반복마다 리스트를 생성하고 내부 반복문에서 거리 계산을 다시 하므로 <code>O(n<sup>2</sup>)</code>이 걸린다.
2. 풀이법 2
- 전체 거리를 계산 후, 점을 제거할 때만 조정하며 거리를 계산한다. (누적 합 활용)
- 한 번의 반복문으로 전체 이동 거리 및 각 구간의 거리를 미리 계산한다. (<code>O(n)</code>)
- 점을 제거할 때 기존 거리 `dist[i-1] + dist[i]`를 빼고 `points[i-1] → points[i+1]` 거리만 추가한다.
- 따라서 <code>O(1)</code> 연산으로 거리를 수정하고 한 번의 반복문으로 제거할 점을 탐색하므로 <code>O(n)</code>이 걸린다.
- 총 시간 복잡도는 <code>O(n)</code> + <code>O(n)</code> = <code>O(n)</code> 이 된다.
