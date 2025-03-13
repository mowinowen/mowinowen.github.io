---
layout: single
title: "파이썬 - 마법의 성 탈출하기"
categories: coding
tag: [알고리즘, 코딩테스트, Brute Force, 경로 탐색, Python]
toc: true
author_profile: false
sidebar:
  nav: "docs"
---

## 문제
당신은 `R x C` 크기의 마법의 성에 갇혀 있다. <br> 
성을 탈출하기 위해서는 포탈을 타고 이동해야 한다. <br>
포탈은 2가지 유형이 있고 `A`와 `Z`가 있다. <br>
당신은 특정한 점프 규칙을 따르며 포탈을 통해 이동할 수 있다.

**점프 규칙**
1. 점프는 항상 오른쪽 아래 대각선 방향으로만 움직일 수 있다. (최소 오른쪽 한 칸 이상, 아래쪽 한 칸 이상 이동)
2. 포탈을 이동할 때에는 다른 유형의 포탈로 이동할 수 있다. (현재 `A` 포탈이면 `Z` 포탈로 이동해야 함.)
3. 시작 지점에서 무조건 2번의 포탈을 이동 후 끝점을 이동해야 탈출할 수 있다.

시작 지점은 항상 성의 좌측 상단이고, 도착 지점은 성의 우측 하단이다. <br>
위 조건을 만족할 수 있는 이동 경로의 경우의 수를 구하시오.

### 입력 형식
첫 번째 줄에 `R, C`가 공백으로 구분되어 주어진다. <br>
두 번째 줄부터 `R x C` 크기의 직사각형이 `A`, `Z`로 주어진다.

### 출력 형식
가능한 이동 경로의 수를 구하라.

### 입출력 예제
입력
```
7 5
A Z A A Z
Z A Z A A
A Z A Z A
A A Z A Z
Z A A Z A
A Z A A Z
Z A Z A Z
```
출력
```
7
```

### 예제 설명
<ul>
  <li>시작 지점과 도착 지점은 각각 <code>A</code>, <code>Z</code> 이다.</li>
  <li>따라서 이동 경로는 <code>A</code> -> <code>Z</code> -> <code>A</code> -> <code>Z</code> 이다.</li>
  <li>이 때, 2번째 지점인 <code>Z</code>와 3번째 지점인 <code>A</code>을 만족하는 (행, 열)은 다음과 같다.</li>
  <ol>
    <li>(1, 2) -> (3, 3)</li>
    <li>(1, 2) -> (5, 3)</li>
    <li>(2, 1) -> (3, 3)</li>
    <li>(2, 1) -> (4, 2)</li>
    <li>(2, 1) -> (5, 2)</li>
    <li>(2, 1) -> (5, 3)</li>
    <li>(3, 2) -> (5, 3)</li>
  </ol>
  <li>총 7가지가 있다.</li>
</ul>

## 풀이법 1
```python
R, C = map(int, input().split())
grid = [input().split() for _ in range(R)]

ans = 0
for i in range(1, R-2):
    for j in range(1, C-2):
        for k in range(i+1, R-1):
            for l in range(j+1, C-1):
                if grid[0][0] != grid[i][j] and grid[i][j] != grid[k][l] and grid[k][l] != grid[R-1][C-1]:
                    ans += 1
print(ans)  
```
1. 2번째 지점을 `(1, 1) ~ (R-3, C-3)`에서 선택. <br> 이유는 시작 지점이 `(0, 0)`이므로 최소 이동할 수 있는 지점은 `(1, 1)` 이다. <br> 또한 3번째 지점은 도착 지점인 `(R-1, C-1)`에서 최소 직전 지점인 `(R-2, C-2)` 이므로 2번째 지점은 최대 `(R-3, C-3)`까지 범위를 설정할 수 있다.
2. 3번째 지점을 `(i+1, j+1) ~ (R-2, C-2)`에서 선택 <br> 이유는 2번째 지점이 `(i, j)`라면 최소 이동할 수 있는 지점은 `(i+1, j+1)`이다.
3. 3가지 조건. 즉, 시작 지점과 2번째 지점, 2번째 지점과 3번째 지점, 3번째 지점과 도착 지점이 모두 다를 경우 경로 개수 `ans` 값이 증가한다.

## 풀이법 2
```python
R, C = map(int, input().split())
grid = [input().split() for _ in range(R)]

def counting_route(portal_list1, portal_list2):
    ans = 0
    for x1, y1 in portal_list1:
        for x2, y2 in portal_list2:
            if x1 < x2 and y1 < y2:
                ans += 1
    return ans

if grid[0][0] == grid[R-1][C-1]:
    ans = 0
else:
    A_list, Z_list = [], []

    for i in range(1, R-1):
        for j in range(1, C-1):
            A_list.append((i, j)) if grid[i][j] == 'A' else Z_list.append((i, j))

    if grid[0][0] == 'A' and grid[R-1][C-1] == 'Z':
        ans = counting_route(Z_list, A_list)
    else:
        ans = counting_route(A_list, Z_list)

print(ans)
```
- 2번째, 3번째 지점은 가장자리 (0번째, `R-1`번째 행과 0번째, `C-1`번째 열)를 제외한 나머지 부분에서 선택된다.
- 이중 반복문을 통해 범위 내부를 순회하면서 `A`는 `A_list`, `Z`는 `Z_list`에 좌표를 추가한다.
- 만약 시작점과 끝점이 같으면 0을 반환하는 예외 처리를 한다. <br> 이유는 어떤 경로를 선택하든 중간 과정에서 같은 포털을 지나가게 되기 때문이다.
- 만약 시작점과 끝점이 다르면 `counting_route`를 적용하여 가능한 경로 개수를 계산한다. <br> `counting_route` 함수는 다음과 같다. <br> 2개의 포탈 좌표 리스트를 입력 받고 좌표 쌍을 비교하며 `(x1, y1)` -> `(x2, y2)` 경로가 유효한지 확인한다. <br> 여기서 유효한 경로란 `x1 < x2` (아래로 이동), `y1 < y2` (오른쪽으로 이동) 2가지가 성립되어야 한다. 
- 시작점이 `A`이고 도착점이 `Z`인 경우 `Z` -> `A` 순서로 경로를 만들어야 한다.
- 반대인 경우 `A` -> `Z` 순서로 경로를 만들어야 한다.
- 유요한 경로의 개수만큼 `ans`에 할당한다.

## 풀이법 비교
- 풀이법1은 완전 탐색 (Brute Force)을 사용하여 모든 가능한 경로를 탐색했다.
- 풀이법2는 특정 지점을 리스트에 저장 후 가능한 경로를 계산했다.
- 풀이법1은 4개의 루프를 사용해서 <code>O(R<sup>2</sup> x C<sup>2</sup>)</code> 시간이 걸린다.
- 풀이법2는 두 리스트를 비교하는 것이므로 <code>O(n<sup>2</sup>)</code> 시간이 걸린다.
- 풀이법2는 모든 가능한 조합을 비교하는 풀이법1과는 다르게 필요한 조합만 선택해서 탐색한다.
