---
layout: single
title: "파이썬 - 증가하는 세 숫자의 개수"
categories: coding
tag: [알고리즘, 코딩테스트, Brute Force, Python, Dynamic Counting]
toc: true
author_profile: false
sidebar:
  nav: "docs"
---

## 문제
정수 `n`개로 이루어진 배열 `arr`이 주어진다. <br>
`i < j < k`를 만족하는 모든 조합 `(i, j, k)` 중에서 다음 조건을 만족하는 경우의 수를 구하시오.
<ul>
  <li><code>arr[i] <= arr[j] <= arr[k]</code></li>
</ul>

### 입력 형식
첫째 줄에 정수 `n`이 주어진다. <br>
둘째 줄에 `n`개의 정수가 공백을 기준으로 주어진다.

### 출력 형식
조건을 만족하는 `(i, j, k)`의 개수를 출력하시오.

### 입출력 예제
입력
```
5
1 2 3 4 5
```
출력
```
10
```

### 예제 설명
가능한 경우의 수는 다음과 같다.
<ol>
  <li>(1, 2, 3)</li>
  <li>(1, 2, 4)</li>
  <li>(1, 2, 5)</li>
  <li>(1, 3, 4)</li>
  <li>(1, 3, 5)</li>
  <li>(1, 4, 5)</li>
  <li>(2, 3, 4)</li>
  <li>(2, 3, 5)</li>
  <li>(2, 4, 5)</li>
  <li>(3, 4, 5)</li>
</ol>
따라서 10가지의 경우의 수가 있다.

## 풀이법 1
```python
n = int(input())
arr = list(map(int, input().split()))
ans = 0

for i in range(n):
  for j in range(i+1, n):
    for k in range(j+1, n):
      if arr[i] <= arr[j] <= arr[k]:
        ans += 1
```
1. `i, j, k`의 모든 가능한 조합을 탐색
2. `i < j < k`와 `arr[i] <= arr[j] <= arr[k]` 2가지 조건을 만족
3. 조건을 만족하면 1 증가

풀이법을 간소화하면 다음과 같다.
```python
n = int(input())
arr = list(map(int, input().split()))
cnt = sum(arr[i] <= arr[j] <= arr[k] for i in range(n) for j in range(i+1, n) for k in range(j+1, n))
print(cnt)
```

## 풀이법 2
```python
n = int(input())
arr = list(map(int, input().split()))

left_count = [0] * n  # left_count[j] : j 이전의 arr[i] <= arr[j]를 만족하는 개수
for j in range(1, n):
  for i in range(j):
    if arr[i] <= arr[j]:
      left_count[j] += 1

right_count = [0] * n  # right_count[j] : j 이후의 arr[j] <= arr[k]를 만족하는 개수
for j in range(n-1):
  for k in range(j+1, n):
     if arr[j] <= arr[k]:
       right_count[j] += 1

ans = sum(left_count[i] * right_count[i] for i in range(n))
print(ans)
```

1. `0 <= i < j < k <= n-1`를 만족해야 한다.
2. j를 기준으로 위 조건을 2가지로 나누면 `0 <= i < j`과 `j < k <= n-1`이다.
3. `0 <= i < j`일 때 `arr[i] <= arr[j]` 만족하고 `j < k <= n-1`일 때 `arr[j] <= arr[k]`을 만족해야 한다.
4. 두 가지 조건으로 인한 두 개의 보조 배열 (`left_count`, `right_count`)로 문제를 해결한다.
5. `left_count[j]` : `j`번째 수 **이전에** 숫자들 중 `arr[i] <= arr[j]` 를 만족하는 개수
6. `right_count[j]` : `j`번째 수 **이후에** 숫자들 중 `arr[j] <= arr[k]` 를 만족하는 개수
7. `arr[i] <= arr[j]`와 `arr[j] <= arr[k]`을 **동시에** 만족해야 하므로 `left_count[j]`와 `right_count[j]`을 곱한다.

## 풀이법 비교 
**풀이법 1**는 3중 반복문을 사용하여 <code>O(n<sup>3</sup>)</code>의 시간이 걸린다.

**풀이법 2**는 `left_count[j]`, `right_count[j]` 두 개의 보조 배열을 미리 계산하고 2중 반복문을 사용하여 <code>O(n<sup>2</sup>)</code>의 시간이 걸려 더 효율적이다.
