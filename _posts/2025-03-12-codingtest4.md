---
layout: single
title: "파이썬 - 초고속 레이싱 트랙"
categories: coding
tag: [알고리즘, 코딩테스트, 슬라이딩 윈도우, Python]
toc: true
author_profile: false
sidebar:
  nav: "docs"
---

## 문제
당신은 레이싱 팀의 감독으로, 세계 최고의 레이싱 트랙을 분석하는 임무를 맡았다. <br>
`N x N` 크기의 레이싱 트랙 데이터가 주어지며, 각 칸에는 해당 구간의 최대 속도 제한이 적혀 있다. <br>
당신의 목표는 **한 개의 직선 구간**에서 연속한 `K`개의 트랙을 선택했을 때, **최고 속도**를 낼 수 있는 구간을 찾아라. <br>

### 입력 형식
첫 번째 줄에 두 정수 `N`, `K`가 주어진다. <br>
이후 `N`개의 줄에 걸쳐 각 줄마다 `N`개의 정수가 주어진다. <br>
각 정수는 해당 구간의 속도 제한을 나타낸다.

### 출력 형식
한 직선 구간에서 연속한 `K`개의 트랙을 선택했을 때, 가능한 최고 속도 제한의 합 중 최댓값을 출력한다.

### 입출력 예제
입력
```
4 3
1 3 1 5
2 2 4 1
6 1 2 3
7 4 1 8
```
출력
```
13
```

### 예제 설명
각 행에서 연속한 `K=3`개의 구간을 선택했을 때
<ul>
  <li>첫 번째 행의 최댓값 : 3 + 1 + 5 = 9 </li>
  <li>두 번째 행의 최댓값 : 2 + 2 + 4 = 8 </li>
  <li>세 번째 행의 최댓값 : 6 + 1 + 2 = 9 </li>
  <li>네 번째 행의 최댓값 : 4 + 1 + 8 = 13 </li>
</ul>
따라서 최대 속도의 합은 네 번째 행의 13 입니다.

## 풀이법 1
```python
n, k = map(int, input().split())
grid = [list(map(int, input().split())) for _ in range(n)]

ans = 0
for i in range(n):
  for j in range(n-k+1):
    ans = max(sum(grid[i][j:j+k]), ans)

print(ans)
```
1. 첫 번째 반복문 : 각 행을 순회
2. 두 번째 반복문 : 해당 행에서 연속한 `k`개의 숫자를 선택할 수 있는 모든 구간 확인
3. 행 `i`에서 열 `j`부터 `k`개의 숫자를 선택 후 `k`개의 숫자 합을 구함
4. 현재까지의 최댓값과 비교하여 최댓값을 갱신한다.

위 코드를 간단하게 하면 다음과 같다.
```python
n, k = map(int, input().split())
grid = [list(map(int, input().split())) for _ in range(n)]

ans = max(sum(grid[i][j:j+k]) for j in range(n-k+1) for i in range(n))
print(ans)
```

## 풀이법 2
```python
n, k = map(int, input().split())
grid = [list(map(int, input().split())) for _ in range(n)]
ans = 0

for row in grid:
    curr_sum = sum(row[:k])
    ans = max(ans, curr_sum)

    for j in range(1, n-k+1):
        curr_sum -= row[j] - row[j+k-1]
        ans = max(ans, curr_sum)

print(ans)
```
**슬라이딩 윈도우 기법**을 사용하여 부분합을 구한다.

**슬라이딩 윈도우**는 처음 계산한 합을 기준으로, 윈도우를 한 칸씩 오른쪽으로 이동시키면서 이전 값을 빼고 새로운 값을 더하여 계산한다.

<ol>
  <li>첫 번째 반복문 : 각 행을 순회</li>
  <ul>
    <li><code>curr_sum</code> -> 행에서 처음 `k`개의 숫자 합을 구한다.</li>
    <li>구간 합이 <code>ans</code>보다 크면 <code>ans</code>를 갱신한다.</li>
  </ul>
  <li>두 번째 반복문 : <code>row</code>의 나머지 부분 탐색</li>
  <ul>
    <li>이전 구간의 맨 왼쪽 값 <code>row[j-1]</code>을 뺀다.</li>
    <li>새로 추가된 구간의 맨 오른쪽 값 <code>row[j+k-1]</code>을 더한다.</li>
    <li><code>curr_sum</code>과 <code>ans</code>를 비교하여 큰 값을 <code>ans</code>에 저장한다.</li>
  </ul>
</ol>

## 풀이법 비교
**풀이법 1**은 **매 구간**마다 sum()을 호출하여 **부분합**을 계산한다. <br>
**풀이법 2**은 **구간 합**을 갱신하여 계산한다. <br>
풀이법 1은 <code>O(n<sup>2</sup> * k)</code> 시간이 걸리고, 풀이법 2는 <code>O(n<sup>2</sup>)</code> 시간이 걸린다.

따라서 매번 구간 합을 새로 계산하는 대신, **슬라이딩 윈도우 기법**이 이전 값에서 변경된 부분만 갱신하기 때문에 계산 시간이 단축되므로 더 효율적이다.
