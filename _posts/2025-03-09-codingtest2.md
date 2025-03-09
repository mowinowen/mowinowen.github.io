---
layout: single
title: "파이썬 - 괄호 쌍 세기"
categories: coding
tag: [알고리즘, 코딩테스트, 괄호, Python]
toc: true
author_profile: false
sidebar:
  nav: "docs"
---

## 문제
문자열이 주어졌을 때, 문자열 내에서 올바른 괄호 쌍 `()`의 개수를 세는 프로그램을 작성하라. <br>
여기서 올바른 괄호 쌍이란, 왼쪽 괄호 `(`가 먼저 나오고, 그 이후에 오른쪽 괄호 `)`가 나오는 경우를 의미한한다.

### 입력 형식
첫째 줄에 문자열이 주어진다. <br>
문자열은 `(` 또는 `)`로만 이루어져 있다.

### 출력 형식
올바른 괄호 쌍의 개수를 출력한다.

### 입출력 예제
입력
```
(()())
```
출력
```
8
```

### 예제 설명
<ul>
  <li><strong>(</strong>(<strong>)</strong>())</li>
  <li><strong>(</strong>()(<strong>)</strong>)</li>
  <li><strong>(</strong>()()<strong>)</strong></li>
  <li>(<strong>(</strong><strong>)</strong>())</li>
  <li>(<strong>(</strong>)(<strong>)</strong>)</li>
  <li>(<strong>(</strong>)()<strong>)</strong></li>
  <li>(()<strong>(</strong><strong>)</strong>)</li>
  <li>(()<strong>(</strong>)<strong>)</strong></li>
</ul>
따라서 올바른 괄호 쌍은 8개이다.

## 풀이법 1
```python
arr = list(input())

cnt = 0
for i in range(len(arr)):
    for j in range(i+1, len(arr)):
        if arr[i] == '(' and arr[j] == ')':
            cnt += 1

print(cnt)
```
1. 문자열을 입력받아 리스트로 변환한다.
2. 이중 반복문을 사용하여 가능한 `(i, j)`를 확인한다.
3. j는 i 이후의 위치이다.
4. 이 때, `i`번째 위치가 `(`이고 `j`번째 위치가 `)`이면 개수를 1 증가한다.

## 풀이법 2
```python
string = input()

ans, open_pair = 0, 0
for i in string:
    if i == '(':
        open_pair += 1
    else:
        ans += open_pair
print (ans)
```

1. `open_pair`는 현재까지 나온 `(`의 개수이다.
2. 괄호 문자열을 순회하면서 `(`가 나오면 `open_pair`을 1 증가한다.
3. 이를 통해 여는 괄호 개수를 누적한다.
4. `)`가 나오면 `ans`에 현재까지의 `open_pair`을 더한다.
5. 이를 통해 현재까지 나온 모든 `(`과 짝을 이룰 수 있는 `)`의 개수를 추가할 수 있다.

## 풀이법 비교
**풀이법 2**에서는 `)`가 나올 때마다 이전에 등장한 `(`의 개수만큼 괄호 쌍을 구할 수 있다.

**풀이법 1**은 이중 반복문을 사용하여 O(n<sup>2</sup>)이 걸리지만, **풀이법 2**는 한 번의 반복문을 사용하여 O(n)이 걸려 더 효과적이다.