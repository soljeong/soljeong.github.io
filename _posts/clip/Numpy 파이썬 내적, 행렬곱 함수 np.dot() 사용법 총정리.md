---
source: https://jimmy-ai.tistory.com/75
---


> ## Excerpt
> 파이썬 넘파이 내적 함수 : np.dot() 안녕하세요. 이번 시간에는 파이썬 넘파이 라이브러리에서 제공하는 벡터 내적, 행렬곱 함수인 np.dot 함수의 사용법을 array의 차원에 따라서 총정리해보는 시간을 가져보겠습니다. 설명은 공식 document 글을 기반으로 하여 작성하였습니다. 기본적으로 np.dot 함수는 2개의 input 만을 받습니다. 3개 이상의 array에 대한 곱은 np.dot 함수 1회로는 수행할 수 없고, 여러번 함수를 겹쳐서 실행해야만 합니다. 따라서, 여기서는 2개의 input array의 차원에 따라 연산 수행이 어떤 패턴으로 달라지는지를 위주로 글을 작성해보겠습니다. 벡터 내적 : 1차원 x 1차원 가장 기본적인 경우로, 두 개의 input array가 모두 1차원 벡터..

---
안녕하세요. 이번 시간에는 파이썬 넘파이 라이브러리에서 제공하는

벡터 내적, 행렬곱 함수인 np.dot 함수의 사용법을 array의 차원에 따라서

총정리해보는 시간을 가져보겠습니다.

설명은 공식 document 글을 기반으로 하여 작성하였습니다.

기본적으로 **np.dot 함수는 2개의 input** 만을 받습니다.

3개 이상의 array에 대한 곱은 np.dot 함수 1회로는 수행할 수 없고,

여러번 함수를 겹쳐서 실행해야만 합니다.

따라서, 여기서는 2개의 input array의 차원에 따라 연산 수행이 어떤 패턴으로

달라지는지를 위주로 글을 작성해보겠습니다.

### **벡터 내적 : 1차원 x 1차원**

가장 기본적인 경우로, 두 개의 input array가 모두 1차원 벡터인 경우입니다.

이 경우, **element-wise 방식으로 각 원소를 곱한 값들을 더한 내적 연산**을 수행합니다.

```
import numpy as np

# 1차원 x 1차원
a = np.array([1, 3, 5])
b = np.array([4, 2, 1])
np.dot(a, b)

# 결과 : 15
```

![](https://blog.kakaocdn.net/dn/bvKYkt/btrpdoWQ5wE/PHMm3Efs12bxPVWsr9Pcx0/img.jpg)

참고로, 아래처럼 **양쪽 array의 원소 개수가 불일치한다면, ValueError가 발생**합니다.

고차원의 dot 연산에서도 곱할 element의 원소 개수가 일치하는지 꼭 체크해주세요.

```
# 양쪽 array의 원소 개수 불일치
a = np.array([1, 3, 5])
b = np.array([4, 2, 1, 4])
np.dot(a, b) # ValueError: shapes (3,) and (4,) not aligned: 3 (dim 0) != 4 (dim 0)
```

### **행렬곱 : 2차원 x 2차원**

양쪽 input array가 모두 2차원인 경우, 행렬곱이 수행됩니다.

이 경우, @를 이용한 연산 혹은 np.matmul 함수와 사용법이 동일합니다.

numpy 공식 문서에서는 **np.matmul 혹은 @을 사용하는 것을 권장**하고 있습니다.

```
# 2차원 x 2차원
a = np.array([[1, 3], [2, 4]])
b = np.array([[1, 6], [3, 0]])
np.dot(a, b)

# a @ b, np.matmul(a, b)와 결과 동일, dot 미사용 권장

# 결과
array([[10,  6],
       [14, 12]])
```

![](https://blog.kakaocdn.net/dn/qAe84/btrpcNJaEVK/qutTzD4z7hLCYpM0YuCpMK/img.jpg)

### **스칼라배 : n차원 x 스칼라**

한쪽이라도 스칼라 element(0차원)이 들어오면,

단순 스칼라배 연산이 수행됩니다.

이 경우도 마찬가지로, \*를 이용한 연산 혹은 np.multiply 함수를 이용한 결과와

동일하며, **np.dot 함수를 사용하지 않는 것을 권장**하고 있습니다.

```
# n차원 x 스칼라
a = np.array([[1, 3], [2, 4]])
b = 2
np.dot(a, b)

# a * b, np.multiply(a, b)와 결과 동일, dot 미사용 권장

# 결과
array([[2, 6],
       [4, 8]])
```

![](https://blog.kakaocdn.net/dn/kUw5s/btro38nk2km/YKVFUXqAV0NvNxUdNGMjQK/img.jpg)

### **다차원 내적 : n차원 x 1차원**

다소 복잡한 경우로 볼 수 있는데,

한 쪽의 element가 n차원, 반대 쪽의 element가 1차원인 경우입니다.

이 경우, **왼쪽이 n차원인 경우 각 행마다 오른쪽의 벡터와 내적**을 수행합니다.

만일 **오른쪽이 n차원이면 각 열마다 왼쪽의 벡터와 내적**을 수행할 것입니다.

```
# n차원 x 1차원
a = np.array([[1, 3], [2, 4]])
b = np.array([2, 5])
np.dot(a, b)

# 결과
array([17, 24])
```

![](https://blog.kakaocdn.net/dn/0XMFd/btro12OODyU/gMgZ8fuAzLPCIVhVenax50/img.jpg)

1차원 x n차원인 경우는 **a 성분의 각 열을 가지고 b 벡터와 내적을 수행**한

결과도 비교해보시면 좋을 듯 합니다.

```
# 1차원 x n차원
a = np.array([[1, 3], [2, 4]])
b = np.array([2, 5])
np.dot(b, a)

# 결과
array([12, 26])
```

1 \* 2 + 2 \* 5 = 12,

3 \* 2 + 4 \* 5 = 26이 등장한 결과를 봐두시면 이해가 쉬울 것으로 생각됩니다.

### **다차원 행렬곱 : n차원 x m차원**

가장 복잡한 경우의 dot 함수 사용 예시입니다.

왼쪽 array의 last axis, 오른쪽 array의 second last axis 끼리 곱한다고 되어있지만,

쉽게 설명하면 **왼쪽 array의 각 행,** **오른쪽 array의 각 열**끼리

**순서대로 내적을 수행한 결과**를 반환하는 의미로 생각해두시면 좋습니다.

```
# n차원 x m차원(m >= 2)
a = np.array([[1, 3], [2, 4]])
b = np.array([[[1, 1], [0, 1]], [[0, 0], [0, 0]]])
np.dot(a, b)

# 결과
array([[[1, 4],
        [0, 0]],

       [[2, 6],
        [0, 0]]])
```

![](https://blog.kakaocdn.net/dn/BNuDq/btro6bR1mNk/0VtM220jWapuVHwS4cNDf0/img.jpg)

위의 예시를 살펴보면, **a의 첫 번째 행에 b의 각 열을 순서대로 내적시킨 결과를**

**먼저 첫 번째 행렬**에 담고, 이어서 **a의 두 번째 행에 b의 각 열을 마찬가지로**

**차례대로 내적한 결과를 두 번째 행렬**에 담은 것을 확인할 수 있습니다.
