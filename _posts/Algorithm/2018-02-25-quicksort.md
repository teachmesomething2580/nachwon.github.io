---
layout: post
title: 'Quick sort 알고리즘'
excerpt: 퀵 정렬 알고리즘에 대해 알아보고 파이썬으로 구현해보자.
category: Algorithm
tags:
  - Algorithm
  - divide-and-conquer
  - Quick sort
  - Sort algorithm
---

> ## Quick Sort
>  
> 퀵 정렬(Quicksort)은 찰스 앤터니 리처드 호어가 개발한 정렬 알고리즘이다. 다른 원소와의 비교만으로 정렬을 수행하는 비교 정렬에 속한다.  
> 퀵 정렬은 n개의 데이터를 정렬할 때, 최악의 경우에는 O(n2)번의 비교를 수행하고, 평균적으로 O(n log n)번의 비교를 수행한다.  
> 퀵 정렬의 내부 루프는 대부분의 컴퓨터 아키텍처에서 효율적으로 작동하도록 설계되어 있고(그 이유는 메모리 참조가 지역화되어 있기 때문에 CPU 캐시의 히트율이 높아지기 때문이다.), 대부분의 실질적인 데이터를 정렬할 때 제곱 시간이 걸릴 확률이 거의 없도록 알고리즘을 설계하는 것이 가능하다. 때문에 일반적인 경우 퀵 정렬은 다른 O(n log n) 알고리즘에 비해 훨씬 빠르게 동작한다. 그리고 퀵 정렬은 정렬을 위해 O(log n)만큼의 memory를 필요로한다. 또한 퀵 정렬은 불안정 정렬에 속한다.  
> 퀵 정렬은 배열 내의 요소들을 순서대로 재배치하는 정렬 알고리즘 중 하나이다.  
> 출처: [위키피디아](https://ko.wikipedia.org/wiki/%ED%80%B5_%EC%A0%95%EB%A0%AC)

- - -

## 실행 순서

퀵 정렬 알고리즘은 [분할 정복](/div-and-conquer) 방식을 사용하여 배열을 정렬한다.  

1. 배열에서 임의로 하나의 원소를 선택한다. 이를 `피벗(pivot)` 이라고 한다. 
2. 피벗을 기준으로 피벗의 왼쪽에는 피벗보다 작은 값들을 배치하고 피벗의 오른쪽에는 피벗보다 큰 값들을 배치한다. 결과적으로 피벗을 기준으로 두 개의 배열로 분할된다.  
3. 분할된 리스트들에 대해서 재귀적으로 1 ~ 2 를 다시 수행한다. 분할된 배열의 크기가 0 또는 1이 될 때까지 반복한다.

- - -

## Python 으로 구현하기

- - -

#### 기본 단계 정의

먼저 가장 기본적인 단계를 정의한다. 배열에서 가장 기본단계는 정렬이 필요없는 배열을 받았을 때 일 것이다.  
그러므로 배열의 길이가 2 미만일 때는 그냥 배열을 그대로 리턴하도록 한다.

```py
def quicksort(arr):
    if len(arr) < 2:
        return arr
```

- - -

#### 배열의 길이가 2 이상일 때

배열의 길이가 2 이상이라면 피벗을 선택하고 피벗보다 작은 값들의 배열을 피벗의 왼쪽에, 피벗보다 큰 값들의 배열을 피벗의 오른쪽에 위치시킨다.  
`[4, 2, 6]` 이라는 배열이 주어졌을 경우를 생각해보자. 만약 `4` 를 피벗으로 정했다면 아래와 같이 배열을 세 부분으로 분할할 수 있을 것이다.  


```py
[2] {4} [6]
```

즉, 피벗 보다 작은 원소들의 배열, 피벗, 피벗 보다 큰 원소들의 배열로 나누어지게 된다.  

만약 `2` 를 피벗으로 정했다면 아래와 같이 분할된다.  

```py
[] {2} [4, 6]
```

길이가 0인 배열과 길이가 2인 배열로 분할되었다.  

길이가 2인 배열의 경우에도 마찬가지로 분할해줄 수 있다.  

배열 `[4, 6]` 에서 4 를 피벗으로 선택하는 경우 아래와 같이 분할된다.  

```
[] {4} [6]
```

6 을 피벗으로 선택하는 경우도 마찬가지이다.

```
[4] {6} []
```

길이가 0인 배열의 처리 방법에 대해서는 이미 알기 때문에 나머지 분할된 배열들을 바로 처리해줄 수 있다.  

배열의 길이가 4인 경우를 한 번 살펴보자.  

배열 `[33, 14, 9, 5]` 이 주어졌을 경우, 만약 피벗이 `33` 이라면 다음과 같이 분할 할 수 있다.  

```py
[] {33} [14, 9, 5]
```

길이가 0 인 배열과 길이가 3인 배열로 분할되었다. 이는 다시 아래와 같이 분할할 수 있다.  

```py
[] {33} ([5] {9} [14])
```

괄호안은 길이가 3인 배열을 피봇으로 9를 선택하여 분할한 결과인 것이다.  

이런 방식으로 배열의 길이가 몇개인지 상관없이, 또, 어떤 원소를 피봇으로 선택하는지에 상관없이 배열을 재귀적으로 분할할 수 있다.  

- - -

`quicksort` 함수의 나머지부분을 채워보면 아래와 같다.  

```py
def quicksort(arr):
    if len(arr) < 2:
        return arr
    else:
        pivot = array[0]  # 항상 첫 번째 원소를 피벗으로 선택
        less = [i for i in arr[1:] if i <= pivot]  # 피벗보다 작거나 같은 원소들의 배열
        greater = [i for i in arr[1:] if i > pivot]  # 피벗보다 큰 원소들의 배열

        # less 와 greater 배열에 대해서 다시 quicksort 함수를 실행해준다.
        # 각각의 결과를 피벗의 양 쪽에 이어붙여주면 최종적으로 정렬된 배열이 리턴된다!
        return quicksort(less) + [pivot] + quicksort(greater)  
```

- - - 

#### Reference

- Hello Coding 그림으로 개념을 이해하는 알고리즘, *한빛미디어*