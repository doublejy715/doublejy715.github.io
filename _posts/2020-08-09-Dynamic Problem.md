---
title: Dynamic Problem
author: Chris
layout: post
---
## 파일 합치기
### 조건
1. 여러 장으로 이루어진 파일들을 병합하고자 한다.
2. 병합 시 최소의 비용을 들여서 모든 장을 합하라
3. 병합 시 항상 붙어있는 장만 가능하다.


### 생각
1. 최소 장끼리 더해서  반복적으로 합쳐나간다. (재귀함수 이용) --> 이러면 안됨
2. 길이가 1이 될 때까지 반복한다.


### 코드
#### 첫 코드

## LCS1
### 조건

두 문자열 사이의 최대 부분 수열을 찾는 것이다.

### 생각

한 글자를 기준으로 삼아서 다른 문자열을 비교한다.

반복작업 : 한 글자를 잡고 다른 문자열과 비교한다.(재귀) --> 공부해 보니 부분 중복 문제 존재로 인하여 DP로 푸는것이 더 효율적

마치는 조건 : 기준 문자열의 마지막 까지 진행

### 코드
#### 첫 코드
```

""" Function """
def LCS1(str1,str2):
    len1, len2 = len(str1),len(str2)
    DP = [[None]*(len2+1) for _ in range(len1+1)]

    for i in range(len1+1):
        for j in range(len2+1):
            if i == 0 or j == 0:
                DP[i][j] = 0
            elif str1[i-1] == str2[j-1]:
                DP[i][j] = DP[i-1][j-1] + 1
            else:
                DP[i][j] = max(DP[i-1][j],DP[i][j-1])
    return DP[-1][-1]

""" Input """
str1 = input()
str2 = input()

""" Solve """
res = LCS1(str1,str2)
print(res)
```


## LCS2
### 조건

두 문자열 사이의 최대 부분 수열을 찾는 것이다. 또한 최대 부분 수열까지 출력한다.

### 생각

한 글자를 기준으로 삼아서 다른 문자열을 비교한다.

반복작업 : 한 글자를 잡고 다른 문자열과 비교한다.(재귀)

마치는 조건 : 기준 문자열의 마지막 까지 진행

### 코드
#### 첫 코드
```

""" Function """
def LCS2(str1,str2):
    len1, len2 = len(str1),len(str2)
    DP = [[None]*(len2+1) for _ in range(len1+1)]

    for i in range(len1+1):
        for j in range(len2+1):
            if i == 0 or j == 0:
                DP[i][j] = [0,'']
            elif str1[i-1] == str2[j-1]:
                DP[i][j] = [DP[i-1][j-1][0] + 1,DP[i-1][j-1][1]+str1[i-1]]
            else:
                DP[i][j] = max(DP[i-1][j],DP[i][j-1])
    return DP[-1][-1]

""" Input """
str1 = input()
str2 = input()

""" Solve """
res_N,res = LCS2(str1,str2)
print(res_N)
print(res)
```


## 1학년
### 조건
1. 숫자 사이에 + / - 를 넣어준다.
2. 결과 값은 음수가 되면 안되며 20을 넘어선 안된다.

### 생각
1. DP의 입력으로 수식을 넣어준다.
2. 그런데 재귀함수가 더 맞지않나?

### 코드
#### 내 코드
```
""" Input """
N = int(input())
numbers = list(map(int,input().split()))
maps = [[0]*21 for _ in range(N-1)]

""" Solve """
maps[0][numbers[0]] = 1
for i in range(N-2):
    for j in range(21):
        if maps[i][j]:
            if 0 <= j - numbers[i+1] <= 20:
                maps[i+1][j-numbers[i+1]] += maps[i][j]
            if 0 <= j + numbers[i+1] <= 20:
                maps[i + 1][j + numbers[i + 1]] += maps[i][j]
                
print(maps[-1][numbers[-1]] % (2**63-1))
```


## 줄세우기(No. 2631)
### 조건
어질러져 있는 번호들을 다시 번호 순서대로 줄을 세우려 한다.
아이들의 옮기는 수를 최소화 한다.
아이들의 위치를 서로 바꾸는 것이 아니라 한명씩 옮겨준다.

### 생각
무엇을 DP로 이용해야 하는가? -> 아이들의 위치가 바뀔때?

## 감소하는 수(No. 1038)
### 문제
음이 아닌 정수 X의 자릿수가 가장 큰 자릿수부터 작은 자릿수까지 감소한다면, 그 수를 감소하는 수라고 한다.
N번째 감소하는 수를 출력하는 프로그램을 작성하시오
> 감소하는 수 : 321, 950
> 
> 감소하는 수가 아닌 경우 : 322, 958

### 생각
DP의 요소를 찾아야 한다.

### 코드
#### 첫 코드
오직 BF로만 해결

```

""" Function """
def Check():
    global count,number

    lens = str(number)
    flag = True

    for i in range(len(lens)-1):
        if not int(lens[i]) > int(lens[i+1]):
            flag = False
    if flag:
        count += 1
    number += 1

    return count
    
""" Input """
N = int(input())

""" Solve """
count = 10
number = 10
while True:
    # 기저조건
    if 0 <= N < 10:
        print(N)
        break
    elif Check() == N:
        print(number)
        break
```
> 시간 계산을 할 경우?

#### 두번째 코드
DP를 통해서 풀어야 시간초과를 해결할 수 있을듯