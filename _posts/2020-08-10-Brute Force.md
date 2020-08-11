---
title: Brute Force
author: Chris
layout: post
---
## 크게 만들기(No. 2812)
### 문제
N자리 숫자가 주어졌을 때, 여기서 숫자 K개를 지워서 얻을 수 있는 가장 큰 수를 구하는 프로그램을 작성하시오.

### 입력
1 <= K < N <= 500000

### 코드
#### 첫 코드
```
""" Input """
N,K = map(int,input().split())
number = list(map(int,input()))
check = sorted(number)[:K]

""" Solve """
for i in number:
    if i not in check:
        print(i,end='')
```
시간초과 발생

```
import sys
input = sys.stdin.readline
n, k = map(int, input().split())
s = list(input().strip())
t, tk = [], k
for i in range(n):
    while tk > 0 and t and t[-1] < s[i]:
        del t[-1]
        tk -= 1
    t.append(s[i])
print(''.join(t[:n - k]))
```
**그런데 이해 못함;;;**
http://blog.naver.com/occidere/220809919539 설명

## 택배(NO. 8980)
### 문제
트럭이 마을을 순서대로 돌기 시작한다. (마을1 -> 마을 4)
이 트럭은 택배를 보내는 마을과 받는 마을 그리고 크기가 일정한 박스를 몇개 보내는지 알고있다.
또한 트럭에 실을 수 있는 최대 박스 또한 정해져 있다.

- 조건 1 : 박스를 트럭에 실으면, 이 박스는 받는 마을에서만 내린다.
- 조건 2 : 트럭은 지나온 마을로 되돌아가지 않는다.
- 조건 3 : 박스들 중 일부만 배송할 수도 있다.

목적 : 위의 조건을 모두 만족하면서 최대한 많은 박스들을 각 마을에 배송하라

### 생각
문제에서 처럼 마을 1에서 부터 실어 나르는 것을 구현한다.

각 마을마다 얼마나 박스를 내려놔야 하는지 따로 기록

먼저 내려주고 이후에 다시 실는 코드를 작성

> 힌트에서는 도착하는 마을을 기준으로 정렬하라 함 

### 코드
#### 첫 코드
```
import sys
input = sys.stdin.readline


""" Input & Set """
villages, limits = map(int,input().split())
num = int(input())
info = []
for _ in range(num):
    info.append(list(map(int,input().strip().split())))
info.sort(key = lambda x:(x[0],x[1]))
record = [0]*(villages+1)
res = 0

""" Solve """
for i in range(villages):
    # 내려주는 코드
    if record[i+1]:
        limits += record[i+1]
        res += record[i+1]
        record[i+1] = 0

    # 실어 나르는 코드
    for start,end,boxes in info:
        if start == i+1:
            # limits를 넘어가지 않는 경우
            if boxes <= limits :                         # 실어도 남는 것이 있는 경우
                record[end] += boxes
                limits -= boxes
            # limits를 넘어가는 경우
            else:
                record[end] += limits
                limits = 0
                break

print(res)
```
틀림
https://www.acmicpc.net/board/view/6327 설명 참조

## 센서(No. 2212)
### 문제
고속도로 위에 N개의 센서를 설치하였다. 센서들의 정보를 모으기 위해서 몇개의 집중국을 고속도로위에 K개를 설치하려 한다.
N개의 센서가 적어도 하나의 집중국과는 통신이 가능해야 하며, 집중국의 유지비 문제로 인해 각 집중국의 수신 기능 영역의 길이의 합을 최소화해야 한다.
각 집중국의 수신 가능영역의 거리의 합의 최솟값을 구하는 프로그램을 작성하세요.

### 입력
- N : 센서의 개수 (1<= N <= 10000)
- K : 집중국의 개수(1<= K <= 1000)
- N개의 센서의 좌표가 리스트로 입력된다.

### 문제 이해 못함;;;

## 배(No. 1092)
### 문제
모든 화물은 박스 안에 넣어져 있다. 항구에는 크레인이 N대 있고, 1분에 박스를 하나씩 배에 실을 수 있다.
각 크레인에는 무게 제한이 있다. 이 무게 제한보다 무거운 박스는 크레인으로 움직일 수 없다. 모든 박스를 배로 옮기는데 드는 시간의 최솟값을 구하는 프로그램을 작성하시오.

### 입력
- N : 크레인의 개수(0 < N <= 50)
- 각 크레인의 무게 제한(<= 1000000)
- M : 박스의 수(0 < M <= 10000)
- 각 박스의 무게(<= 1000000)

### 생각
- 박스를 배로 옮길 수 없는 경우는 박스의 최고 무게가 크레인의 최대 하중량보다 더 큰 경우이다.
- 크레인이 동시에 움직이므로 최대한 효율적으로 옮긴다.
- 크레인간의 범위 내에 존재하는 박스를 배정한다.
- 큰 크레인은 자신 이하의 박스를 옮길 수 있으니 배정된 박스를 다 옮기고 그 밑의 박스를 계속 옮겨준다.
 