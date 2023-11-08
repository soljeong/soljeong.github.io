
- 단층-다층 신경망
- MLP
- 결정 경계
- 기저 벡터 변환
- 순전파
- 오차 역전파 error backpropagation
- 경사하강법
- 학습률 learning rate
- 보폭 step size
- 입력층에서 출력층까지 일단 순전파가 일어나고, 출력층까지 도달한 다음에 역전파가 일어난다
- **미분의 연쇄 법칙**
- 기울기 소실
	- 순전파에서 시그모이드 함수가 적용됐다면 역전파에서는 시그모이드 함수의 도함수가 적용된다.
	- 시그모이드 함수의 도함수의 최댓값은 0.25 : 여러번 곱할수록 값이 작아진다.
	- 미분해도 값이 줄어들지 않는 활성화 함수가 필요하다? -> ReLU Rectified linear unit
	- ReLU : 0보다 작으면 0, 정보가 없어진다.
- 배치 batch
- 에포크 epoch
- 이터레이션 iteration
	- 1000행의 데이터, 배치 100행이면, 1 에포크를 위해 10 이터레이션이 필요
- 최적화 알고리즘
- 로스를 미분해서 가중치를 업데이트한다
- 디바이스가 다른 두 텐서를 연산할 수 없다
- 합성곱 콘볼루션 convolution
- CNN convolutional neural network
- 필터를 만들어낸다
- 커널 kernel : 셀로판지 역할
- 각각의 색을 추출하는 커널이 모이면 필터
- feature map
- stride
- 여러 층의 커널을 통과한 feature map이 MLP의 입력으로 들어간다
- 커널을 통해 feature를 추출, 그 feature를 가지고 예측
- 평탄화 flatten
- 전이 학습 transfer learning 
- dropout()
![[데이터 증강 data argumentation]]
## RNN
- 최소-최대 정규화
- 초기 은닉 상태
- 모델의 가중치 저장하기 : 할 줄 모름

## 이미지와 텍스트의 차이
```d2
load data -> eda -> dataset class -> dataloader -> model
```
```d2
load data -> eda -> text preprocess -> dataset class -> dataloader -> model

text preprocess {
    cleaning
    tokenize
    encode

    explanation: |md
        텍스트를 의미있는 숫자로 변경한다
  | 


} 
```

