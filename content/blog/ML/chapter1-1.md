---
title: '[파이썬 머신러닝 완벽 가이드] 1.파이썬 기반의 머신 러닝과 생태계 이해(1)'
date: 2022-01-24 01:55
category: 'ML'
draft: false
---

## 1. 머신러닝의 개념

`머신러닝 (Machine Learning)`이란 애플리케이션을 수정하지 않고도 데이터를 기반으로 패턴을 학습하고 결과를 예측하는 알고리즘 기법을 통칭합니다.
머신러닝 알고리즘은 자연어 처리, 데이터 마이닝, 영상 및 음성 인식 등의 다양한 분야에서 데이터를 기반으로 숨겨진 패턴을 인지해 주어진 문제를 해결합니다.

머신러닝은 크게 지도학습, 비지도학습, 강화학습으로 구분합니다.

`지도 학습(Supervised Learning)`이란 정답 라벨이 있는 데이터를 학습시키는 것으로, 입력 값(X) 와 그 입력 값에 대응하는 정답 (Y) 를 주어 학습시켜 새로운 입력 값에 대응하는 정답을 찾는 것을 목표로 합니다. 대표적인 예로 분류, 회귀, 추천 시스템, NLP 등이 있습니다.

`비지도 학습(Un-supervised Learning)`이란 정답 라벨이 없는 데이터를 학습시키는 것으로, 비슷한 특징끼리 군집화하여 새로운 데이터에 대한 결과를 예측하는 것을 목표로 합니다. 대표적인 예로 클러스터링, 차원 축소 등이 있습니다.

`강화 학습(Reinforcement Learning)`이란 앞선 두 학습 방법과 달리, 에이전트가 행동을 취하고 그 행동에 대한 보상을 받으며 최대한 큰 보상을 받기 위한 행동을 스스로 학습하는 방식을 말합니다.

머신러닝에서 알고리즘만큼이나 중요한 요소가 `데이터` 입니다. 머신러닝의 가장 큰 단점은 데이터에 매우 의존적이라는 것이기 때문에, 올바른 학습 결과를 얻기 위해서는 최적의 머신러닝 알고리즘과 모델 파라미터를 구축하는 능력만큼 데이터를 올바르고 효율적으로 가공, 처리, 추출하는 능력 역시 매우 중요합니다.

## 2. 파이썬 머신러닝 생태계를 구성하는 주요 패키지

머신러닝 프로그램을 작성하는데 가장 많이 사용되는 프로그램 언어는 파이썬과 R입니다. 이 책은 그중 파이썬을 이용한 머신러닝을 다루고 있으며, 저 또한 파이썬이 더 익숙하기 때문에 이 책을 골랐습니다. 이 책에서는 `scikit-learn` (머신러닝), `numpy` & `scipy` (선형대수/통계), `pandas` (데이터 핸들링), `matplotlib` & `seaborn` (시각화) 등을 사용하여 머신 러닝 프로그램을 작성하는 법을 익힙니다.

저는 `Windows 10` 에서 `Anaconda` 를 통해 실습 환경을 구성하여 `python 3.8.12` 버전에서 `jupyter notebook`을 이용해 실습을 진행하였습니다.

## 3. 넘파이(Numpy)

머신러닝의 주요 알고리즘은 선형대수와 통계에 기반합니다. `Numpy` 패키지를 통해 파이썬에서 선형대수 연산을 쉽게 수행할 수 있기 때문에 대부분의 머신러닝 알고리즘이 넘파이 기반으로 작성되어 있고 입력과 출력 데이터를 넘파이 배열 타입으로 사용합니다. 넘파이를 이해하는 것은 파이썬 기반의 머신러닝에서 매우 중요합니다.

### 3.1 넘파이 ndarray 개요

Numpy 패키지를 사용하기 위해서 다음과 같이 Numpy를 임포트합니다.

```python
import numpy as np
```

Numpy 의 `array()` 함수를 통해 파이썬의 리스트로 인자를 입력을 받아 `ndarray` 형태의 변환할 수 있습니다. `ndarray` 배열의 `shape` 변수를 이용해 행과 열의 수를 튜플 형태로 받고 배열의 차원을 알 수 있습니다. 열과 행에 대한 정보 없이 차원만 알고 싶을 때에는 `ndim` 변수를 사용하면 됩니다.

```python
array1=np.array([1,2,3])
print('array1 type:', type(array1))
print('array1 array 형태:', array1.shape)

array2=np.array([[1,2,3],[4,5,6]])
print('array2 type:', type(array2))
print('array2 array 형태:', array2.shape)

array3=np.array([[1,2,3]])
print('array3 type:', type(array3))
print('array3 array 형태:', array3.shape)
```

[output]

```
array1 type: <class 'numpy.ndarray'>
array1 array 형태: (3,)
array2 type: <class 'numpy.ndarray'>
array2 array 형태: (2, 3)
array3 type: <class 'numpy.ndarray'>
array3 array 형태: (1, 3)
array1: 1차원, array2: 2차원, array3:  2차원
```

간단한 예제입니다. 주의하여야 할 점은 array1과 array3은 서로 다르다는 것입니다. array1은 3개의 원소로 이루어진 1차원의 array이며, array3은 1개의 행과 3개의 열로 이루어진 2차원 array입니다.

## 3.2 ndarray의 데이터 타입

서로 다른 데이터 타입을 저장할 수 있는 `list`와는 달리, `ndarray`에서는 모든 원소가 같은 자료형을 가져야 합니다. 따라서 여러 자료형이 섞여있는 리스트를 `ndarray`로 변환할 경우 더 큰 자료형으로 변환됩니다. 따로 변환을 원하는 자료형이 있을 경우에는 `astype()` 매서드를 이용하여 변경이 가능합니다.
`ndarray` 내 원소들의 데이터 타입은 `dtype`을 사용하여 확인할 수 있습니다.

```python
list1=[1,2,3]
array1 = np.array(list1)
print(array1, array1.dtype)

list2=[1,2,'test']
array2 = np.array(list2)
print(array2, array2.dtype)

list3=[1.1, 3.5, 4.7]
array3 = np.array(list3).astype('int32')
print(array3, array3.dtype)
```

[output]

```
[1 2 3] int32
['1' '2' 'test'] <U11
[1 3 4] int32
```

첫 번째 리스트의 경우 모든 원소가 `int32` 자료형이기 때문에 `ndarray`형으로 변환시켰을 때 `int32` 자료형 원소들을 가진 행렬이 자동으로 생성됩니다. 한편 두 번째 리스트의 경우 원소에 정수와 문자열이 섞여 있기 때문에 `<U11` 형의 문자열로 모든 원소가 변환되어 저장이 됩니다. 세 번째 리스트의 경우 모든 원소가 실수형이지만, `astype(int32)` 매서드를 사용하였기 때문에 모든 원소가 `int32` 형으로 소숫점 아래를 버린 `ndarray` 가 생성됩니다.

### 3.3 ndarray를 편리하게 생성하기 - arange, zeros, ones

`arange()`, `zeros()` , `ones()` 매서드를 사용하면 쉽게 ndarray를 생성하고 초기화할 수 있습니다.

`arange()` 매서드는 파이썬의 `range()` 와 유사하게, 특정한 범위에서 순차적으로 ndarray의 데이터 값으로 변환해 줍니다. `zeros()` 매서드와 `ones()` 매서드는 튜플 형태의 shape 값을 입력하면 모든 값을 각각 0과 1로 채운 해당 shape를 가진 ndarray를 반환합니다. 함수 인자로 자료형을 설정해 줄 수 있으며, 함수 인자를 정해주지 않으면 기본값으로 `float64` 를 사용합니다.

```python
array = np. arange(1, 15, 2)
print(array)
print(array.dtype, array.shape)


zero_array = np.zeros((3,2))
print(zero_array)
print(zero_array.dtype, zero_array.shape)

one_array = np.ones((2,3), dtype='int32')
print(one_array)
print(one_array.dtype, one_array.shape)
```

[output]

```
[ 1  3  5  7  9 11 13]
int32 (7,)
[[0. 0.]
 [0. 0.]
 [0. 0.]]
float64 (3, 2)
[[1 1 1]
 [1 1 1]]
int32 (2, 3)
```

`array`에는 1에서 시작하여 2 씩 증가하며, 15 미만의 값까지를 저장한 `ndarray`가 저장됩니다.`zero_array` 와 `one_array`에는 지정한 크기와 자료형에 맞게 각각 0과 1로 채워진 `ndarray`가 저장됩니다.

### 3.4 ndarray의 차원과 크기를 변경하는 reshape()

`reshape()` 매서드는 `ndarray`를 원하는 차원과 크기로 변환할 수 있습니다. 이때 바꾸고자 하는 사이즈로 변경이 불가능할 경우 (처음 `ndarray`과 나중 `ndarray`의 원소 수가 다른 경우)에는 에러가 발생합니다. 함수 인자로 -1 을 사용하는 경우에는 원래 `ndarray`와 호환되는 새로운 차원과 크기로 자동으로 변환시켜 줍니다.

```python
array1=np.arange(8)
print('array1:\n', array1)

array2=array1.reshape(2,2,2)
print('array2:\n', array2)

array3=array1.reshape(4,-1)
print('array3:\n', array3)
```

[output]

```
array1:
 [0 1 2 3 4 5 6 7]
array2:
 [[[0 1]
  [2 3]]

 [[4 5]
  [6 7]]]
array3:
 [[0 1]
 [2 3]
 [4 5]
 [6 7]]
```

`array2` 는 원소가 8개인 `array1` 을 2x2x2의 형태로 변환하여 저장합니다. `array3`은 `array1`을 4개의 행을 갖도록, 4x2의 형태로 자동으로 변환하여 저장합니다.

### 3.5 넘파이의 ndarray의 데이터 세트 선택하기- 인덱싱(Indexing)

#### 3.5.1 단일 값 추출

일반적인 파이썬에서의 `list`와 같은 방법으로, `ndarray`에서 구하고자하는 위치의 인덱스 값을 [] 안에 넣어 단일 값을 찾거나, 수정할 수 있습니다.

```python
array= np.arange(1, 10).reshape(3,3)

print('row=1, col=2 가 가리키는 값:',  array[1][2])
array[1][2] = 5;
print('row=1, col=2 가 가리키는 값:',  array[1][2])
```

[output]

```
row=1, col=2 가 가리키는 값: 6
row=1, col=2 가 가리키는 값: 5
```

#### 3.5.2 슬라이싱

`:` 기호를 이용해 연속한 데이터를 슬라이싱하여 추출할 수 있습니다. `:` 기호를 사이에 두고 시작 인덱스와 종료 인덱스를 표시하면 시작 ~ 종료-1 인덱스의 위치에 있는 데이터의 `ndarray`를 반환합니다. 시작/끝 인자가 생략되면 자동으로 맨 처음/맨 끝 인덱스로 간주됩니다.

```python
array= np.arange(1, 10)
print('array[3:7]\n',  array[3:7])

array2= np.arange(1, 10).reshape(3,3)
print('array2[0:2,0:2]\n',  array2[0:2,0:2])
print('array2[1:,1:2]\n',  array2[1:,1:2])
print('array2[:2,0]\n',  array2[:2,0])
```

[output]

```
array[3:7]
 [4 5 6 7]
array2[0:2,0:2]
 [[1 2]
 [4 5]]
array2[1:,1:2]
 [[5]
 [8]]
array2[:2,0]
 [1 4]
```

#### 3.5.3 팬시 인덱싱

팬시 인덱싱 (Fancy Indexing) 은 `list` 나 `ndarray` 로 인덱스 집합을 지정하면 해당 위치의 인덱스에 해당하는 `ndarray`를 반환하는 인덱싱 방식입니다.

```python
array= np.arange(1, 10).reshape(3,3)

print('array[[0,2],2]\n', array[[0,2],2])
```

[output]

```
array[[0,2],2]
 [3 9]
```

`array[[0,2],2]` 는 `array`의 0행과 2행의 2번쨰 열만 인덱싱하여 새로운 `ndarray` [3 9] 를 리턴합니다.

#### 3.5.4 불린 인덱싱

불린 인덱싱 (Boolean Indexing) 은 조건 필터링과 검색을 동시에 할 수 있기 때문에 매우 자주 사용되는 인덱싱 방식입니다.

```python
array= np.arange(1, 10).reshape(3,3)

print('array[array>5]\n', array[array>5])
```

[output]

```
array[array>5]
 [6 7 8 9]
```

`ndarray` 의 인덱스로 조건을 넣어주면 그 조건을 만족하는 값들만 필터링하요 새로운 `ndarray` 를 리턴합니다.

### 3.6 행렬의 정렬 - sort()와 argsort()

#### 3.6.1 행렬 정렬

행렬을 정렬하기 위해서는 `sort()` 매서드를 사용하면 됩니다. 기본적으로 오름차순 정렬을 수행하며, `sort()[::-1]` 을 이용하면 내림차순 정렬을 수행합니다.

`sort()`를 호출하는 방법에는 `np.sort()` 와 `ndarray.sort()`가 있습니다. `np.sort()`는 원 행렬을 유지한 채 정렬된 행렬을 새롭게 생성하여 리턴하는 반면, `ndarray.sort()` 는 원 행렬 자체를 정렬된 상태로 변환합니다.

```python
org_array = np.array([3, 1, 9, 5])
print('원본 행렬:', org_array)

print('np.sort 호출 후 반환된 정렬 행렬:', np.sort(org_array)[::-1])
print('np.sort 호출 후 원본 행렬:', org_array)

org_array.sort()
print('ndarray.sort 호출 후 원본 행렬:', org_array)
```

[output]

```
원본 행렬: [3 1 9 5]
np.sort 호출 후 반환된 정렬 행렬: [9 5 3 1]
np.sort 호출 후 원본 행렬: [3 1 9 5]
ndarray.sort 호출 후 원본 행렬: [1 3 5 9
```

행렬이 2차원 이상인 경우 `axis` 값을 함수인자로 제공하여 축 방향의 정렬을 수행할 수 있습니다.

```python
org_array = np.arange(9,0,-1).reshape(3,3)
print('원본 행렬:\n', org_array)

print('열 방향 정렬:\n', np.sort(org_array, axis=0))
print('행 방향 정렬:\n', np.sort(org_array, axis=1))
```

[output]

```
원본 행렬:
 [[9 8 7]
 [6 5 4]
 [3 2 1]]
열 방향 정렬:
 [[3 2 1]
 [6 5 4]
 [9 8 7]]
행 방향 정렬:
 [[7 8 9]
 [4 5 6]
 [1 2 3]]
```

#### 3.6.2 정렬된 행렬의 인덱스 반환하기

원본 행렬이 정렬되었을 때 기존 원본 행렬의 원본에 대한 인덱스를 필요로 할 때 np.argsort()를 이용합니다.

```python
name_array = np.array(['John', 'Mike', 'Sarah', 'Kate', 'Samuel'])
score_array= np.array([78, 95, 84, 98, 88])
print('성적 오름차순 정렬:',np.sort(score_array))

sort_indices_asc = np.argsort(score_array)
print('성적 오름차순 정렬 시 score_array의 인덱스:', sort_indices_asc)
print('성적 오름차순으로 name_array의 이름 출력:', name_array[sort_indices_asc])
```

[output]

```
성적 오름차순 정렬: [78 84 88 95 98]
성적 오름차순 정렬 시 score_array의 인덱스: [0 2 4 1 3]
성적 오름차순으로 name_array의 이름 출력: ['John' 'Sarah' 'Samuel' 'Mike' 'Kate']
```

다음과 같은 예시에서, 학생들의 점수를 오름차순으로 정렬하고 각 점수에 대응되는 학생을 찾기 위해 점수에 대한 원본 행렬에서의 인덱스가 필요하므로 `argsort()`를 이용하여 해당 과정을 수행해 줄 수 있습니다.

### 3.7 선형대수 연산 - 행렬 내적과 전치 행렬 구하기

#### 3.7.1 행렬 내적 (행렬 곱)

행렬의 내적은 np.dot()을 이용하여 수행합니다.

```python
A = np.array([[1,2,3],[4,5,6]])
B = np.array([[7,8],[9,10],[11,12]])

dot_product = np.dot(A,B)
print('행렬 내적 결과 \n', dot_product)
```

[output]

```
행렬 내적 결과
 [[ 58  64]
 [139 154]]
```

#### 3.7.2 전치 행렬

전치 행렬은 np.transpose()를 이용하여 구할 수 있습니다.

```python
A=np.array([[1,2],[3,4]])
transpose_mat= np.transpose(A)
print('전치 행렬 \n',transpose_mat)
```

[output]

```
전치 행렬
 [[1 3]
 [2 4]]
```

Chapter 1 의 나머지 내용은 [다음 글](https://kunheekimkr.github.io/ML/chapter1-2/) 에서 이어서 설명하겠습니다.
