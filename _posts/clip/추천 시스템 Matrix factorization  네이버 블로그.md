---
source: https://blog.naver.com/PostView.naver?blogId=shino1025&logNo=222394488801
---


> ## Excerpt
> 이번 포스팅에서는 추천 알고리즘 중에서도 Collaborative Filtering 방법론 중 하나인 Matrix Factori...

---
\[추천 시스템\] Matrix factorization

 [![프로파일](https://blogpfthumb-phinf.pstatic.net/MjAxODAzMTlfMjMg/MDAxNTIxNDAzMjY4NTgx.LjkDxXNWu2Af9I42GSl6H5wE5ZSLMOZbGhBsswrSdCAg.A5MY___jgn0AJ1cK2d7K9S9eS2pTJKSQWHUpHaA_S8og.PNG.shino1025/imiml.png?type=s1)](https://blog.naver.com/shino1025) [IML](https://blog.naver.com/shino1025) _・_ 2021\. 6. 11. 21:42

이번 포스팅에서는 추천 알고리즘 중에서도 Collaborative Filtering 방법론 중 하나인 **Matrix Factorization**에 대해 알아보고, 간단한 실습도 진행해보자.



**예제 코드 배포**

![](https://user-images.githubusercontent.com/29897277/121314162-0db62f00-c942-11eb-81eb-bd41780028e8.png)

Matrix Factorization(MF)는 **User와 Item 간의 평가 정보를 나타내는 Rating Matrix를 User Latent Matrix와 Item Latent Matrix로 분해**하는 기법을 말한다.



Rating Matrix는 (User의 수) \* (Item의 수)로 구성된 행렬인데, 이때 각 칸에는 각 유저가 기록한 해당 아이템에 대한 평가가 수치로써 기록된다. 물론 비어있는 부분이 보통은 더 많다. 따라서 Rating Matrix는 왠만하면 Sparse Matrix가 되기 마련인데, MF는 이러한 행렬 분해 과정에서 이러한 빈칸을 채울만한 평점을 예측하는 과정이라고 볼 수 있다.



이는 Explicit Feedback(명시적 지표)으로 평점이나 별점이 되기도 하지만, 사실상 근본적인 평가 그 자체라면 Implicit Feedback(암시적 지표)라도 충분히 사용해볼 수 있다.



Rating Matrix는 MF를 통해 2개의 행렬로 나누어지는데, 각 행렬의 형태는 다음과 같다.

**\- User Latent Matrix(U) = (User의수) \* K**

**\- Item Latent Matrix(I) = (Item의 수) \* K**

**\- Rating Matrix(R) = (User \* K) X (K \* Item) => User \* Item**



이때 둘 중 하나의 행렬을 Transpose(전치)하여 그대로 matrix multiplication(행렬곱)을 해주면 기존의 Rating matrix의 형태로 돌아오게 되는 셈이다.



Latent Matrix은 각각, 각 유저와 아이템에 대하여 row를 가지게 되는데, K개만큼의 공간을 할당받는다.

해당 row는 유저 혹은 아이템의 잠재적 정보를 저장하고 있는 벡터로, User Matrix의 유저(u) row와 Item Matrix의 아이템(i) row에 대하여 행렬 곱을 수행하면 **"유저u의 아이템i에 대한 평가"**가 나오는 셈이다.

$Rating\_{ui}^=I\_i^⊺\\times U\_u$Ratingui\=I⊺i×Uu

이때, K는 Latent Factor의 크기로 사용자에 의해 Hyperparamter로 입력받는다.

K는 유저 및 아이템에 대한 정보를 저장하는 공간이다. **K를 크게 잡으면 기존의 Rating Matrix로부터 다양한 정보를 가져갈 수 있지만, K를 작게 잡아야만 핵심적인 정보외의 노이즈를 제거**할 수 있다.



**얼마만큼의 Latent Factor 크기를 할당하냐에 따라, 설명력이 낮은 정보를 삭제하고 설명이 높은 정보를 남긴다는 방향성으로 Matrix가 형성**되게 된다.



MF는 행렬을 분해시킨다는 Key point 아래에서 굉장히 다양한 알고리즘이 존재하는데, 필자는 이 중에서 2가지를 예로 들어서 실습까지 해보도록 하겠다.



**SVD - Singular Value Decomposition**

SVD(특이값 분해) 알고리즘은 특정 M \* N의 행렬 A에 대하여 아래와 같은 3개의 행렬의 곱으로 분해나는 과정을 말한다. Matrix Factorization을 실습하기 전에 간단하게 개념을 이해하기 위해 살펴보자.

![](https://user-images.githubusercontent.com/29897277/121314621-7dc4b500-c942-11eb-88f7-95cc7e42983f.png)



SVD 알고리즘은 본래 딱히 **"추천 시스템"**을 위해서만 존재하는 것은 아니기 때문에 어느 정도의 사전 지식이 필요하다. 아래의 링크에서 SVD에 대해 굉장히 잘 설명을 하고 있기 때문에 참고하도록 하자.



**A 행렬을 Rating Matrix라고 했을 때**, **U는 User Matrix**, **V^t는 Item Matrix**라고 볼 수 있다. 중간에 있는 **S 행렬은 본래의 SVD에서는 행렬의 A 특이값**이라고 불리는 대각 행렬(벡터로 봐도 무방)인데, **2개의 행렬로 분해할 필요가 있기 때문에 어느 한쪽 (U or V^t)에 행렬곱을 하여 합쳐주면 된다**. 물론 이 또한 추천 성능에 어떻게 영향을 끼칠지는 해봐야 알기 때문에 충분히 고려해볼 대상이다.



**TSVD - Truncated SVD**

**절단된 SVD**라고 불리는 이 방법은 기존의 full SVD 행렬에서 일부 벡터를 삭제시키는 것이다.

![](https://wikidocs.net/images/page/24949/svd%EC%99%80truncatedsvd.PNG)

U, S, V 행렬에 대해서 t 만큼의 크기만 남기고 나머지를 삭제하더라도, 행렬곱을 할 때 크기가 맞춰지기 때문에, 원상태의 행렬 A의 크기를 유지할 수 있다.

이때, 행렬 A는 일부 벡터가 삭제된 상태로 복구가 이루어지게 되는데, 이를 데이터의 차원을 줄인다고 정의한다.

데이터의 차원이 줄게 되더라도, 행렬 A\`은 최대한 **기존 행렬 A에 대한 잠재적 의미를 유지하려고 하기 때문에 좁은 공간안에서 선택적으로 중요하지 않는 정보의 삭제**가 일어난다.



이로 인해, 기존 행렬 A에서는 드러나지 않았던 심층적인 의미가 겉으로 드러나게 된다.



**SVD 실습하기**

해당 코드는 **matrix-factorization/model/svd.py**에서 확인할 수 있다.

svd model은 다음과 같은 순서대로 작동한다.



**1\. Rating Matrix를 입력받은 후, 전처리 (\_\_init\_\_, init\_sparse\_matrix)**

**2\. SVD 알고리즘으로 행렬 분해, 절단한 후, 다시 재병합 (train, get\_svd)**

**3\. 새롭게 생성된 행렬 pred\_matrix와 기존 Rating Matrix와의 Error 계산 (evaluate)**



SVD 알고리즘을 적용시에, 평가가 기록되지 않는 Rating Matrix의 Nan 값을 채워줄 필요가 있다.

그냥 0으로 채워버려도 작동은 잘되지만, 평가라는 관점에서 굉장히 부정적인 피드백으로 작용할 수 있다고 생각해서, 필자의 경우 아래와 같이 해당 각 Item에 대한 평균 평점으로 채워주었다.

def init\_sparse\_matrix(self): self.train\_matrix \= self.sparse\_matrix.apply(lambda x: x.fillna(x.mean()), axis\=1) self.sparse\_matrix \= self.sparse\_matrix.fillna(0)

사실상, 행렬을 나눈 과정이 끝이기 때문에 학습이라고 부르기는 애매하지만 필자는 일단 일관성을 위해 train 함수라고 정의를 해주었다. 전처리가 끝났다면 바로 행렬 분해 작업을 수행해준다.

def train(self): item\_factors, user\_factors \= self.get\_svd(self.train\_matrix, self.K) result\_df \= pd.DataFrame( np.matmul(item\_factors, user\_factors), columns\=self.sparse\_matrix.columns.values, index\=self.sparse\_matrix.index.values ) self.item\_factors \= item\_factors self.user\_factors \= user\_factors self.pred\_matrix \= result\_df

가장 핵심 부분인 SVD 행렬 분해 과정 함수는 아래와 같다.

@staticmethod def get\_svd(sparse\_matrix, K): U, s, VT \= np.linalg.svd(sparse\_matrix.transpose()) # U \= (user\_n, user\_n), s \= (user\_n,), VT \= (item\_n, item\_n) U \= U\[:, :K\] # (user\_n, K) s \= s\[:K\] \* np.identity(K, np.float) # (K, K) VT \= VT\[:K, :\] # (K, item\_n) item\_factors \= np.transpose(np.matmul(s, VT)) # (item\_n, K) user\_factors \= np.transpose(U) # (K, user\_n) return item\_factors, user\_factors

예측 행렬이 완성되았다면, 얼마나 잘 맞추었는가 확인을 해보자. 평가를 위한 Error Function은 **RMSE**이며, 원본 행렬과 예측 행렬의 각 자리가 얼마나 차이나는지를 RMSE 함수로 측정하는 모습을 확인할 수 있다.

def evaluate(self): idx, jdx \= self.sparse\_matrix.to\_numpy().nonzero() ys, preds \= \[\], \[\] for i, j in zip(idx, jdx): ys.append(self.sparse\_matrix.iloc\[i, j\]) preds.append(self.pred\_matrix.iloc\[i, j\]) error \= mean\_squared\_error(ys, preds) return np.sqrt(error)

해당 코드를 실행해보려면 쉘에서 아래와 같이 커맨드를 입력하면 된다.

$ python train.py \--svd \--k 300

SVD에서는 행렬을 절단시키는 형태로 K를 사용하기 때문에 반드시 User 혹은 Item의 총 수보다 Latent Factor K가 작아야 한다.



**GD - Gradient Descent**

GD는 본래 머신러닝에서 굉장히 많이 사용되는 개념인데, 해당 내용은 포스팅 하나를 따로 파야할 정도이기 때문에, GD의 기초 개념 자체는 넘어가도록 하겠다.



혹시나 GD가 뭔지 모르겠다면, 아래의 링크를 참고하도록 하자.

사실상 순한맛의 MF가 SVD였다면 제대로 된 MF 기법이 바로 GD이다.

![](https://media.vlpt.us/images/vvakki_/post/efb8e47e-25c8-48bb-909b-a579f6b67b02/image.png)

맨처음 User Latent Matrix와 Item Latent Matrix를 위에서 정의한 크기로 만든 후, 임의의 값을 채워놓는다.

마음대로 채워진 두 행렬을 합쳐봤자 당연히 기존의 Rating Matrix와 유사할리가 없기 때문에,

**\- User/Item Matrix의 weight 값을 변경하고,**

**\- 두 행렬을 합쳐서 기존의 Rating Matrix와의 유사한지 비교**



**위 두 과정을 계속 반복한다.** 이 때, 두 Latent Matrix의 행렬곱과 Rating Matrix와의 Error를 최소화시키기 위해 GD를 사용하는 것이다. 본래 MF 관련 논문에서 사용된 Error Function 식은 다음과 같다.

![](https://postfiles.pstatic.net/MjAyMTA2MTBfMjYx/MDAxNjIzMzI5ODAzODk4.QouH97T2zmEho1DcYsD7cGAii4FicdGDgyCWP3pT5ocg.VW3XbYaCMmZZMfNisLtrQJ8T3mchV9Fo9TYht0nNPkEg.PNG.shino1025/image.png?type=w80_blur)

r은 Rating matrix, q, p는 각각 Item와 User Matrix를 뜻한다. 뒤에 붙은 λ은 Overfitting을 피하기 위해 넣은 Regularization이며, 본래 GD의 핵심적인 로직과는 크게 상관이 없다.



필자의 코드는 공부 삼아서 본래의 논문에서 기재된 Regularization을 아래와 같이 적용해보았다.

![](https://postfiles.pstatic.net/MjAyMTA2MTBfMTMx/MDAxNjIzMzMwMDQ4MzQ2.djZ-d0Mj4c8Ipxi06MyO1QMgbYeSaIDfKXOT54o0XmYg.DmaNvA1JyHvVjJhyB4_e5Snp93L0tuJpX-suT01x-YYg.PNG.shino1025/image.png?type=w80_blur)

**GD 실습하기**

해당 코드는 **matrix-factorization/model/sgd.py**에서 확인할 수 있다.

svd model은 다음과 같은 순서대로 작동한다.



**1\. Rating Matrix(R)를 입력받은 후, 전처리 (\_\_init\_\_)**

**2\. 임의의 U, I 행렬을 선언후, R 행렬에 대하여 GD 수행. (train, predict)**

**3\. 새롭게 생성된 행렬 pred\_matrix와 기존 Rating Matrix와의 Error 계산 (evaluate)**



이전 SVD에서는 행렬 분해 과정에서 Nan 값을 0으로 채우는 것이 좋지 않게 작용할 수 있기 때문에 필자가 임의의 값을 넣어주었는데, GD의 경우 애초에 평가를 하지 않은 항목은 학습에 반영되지 않기 때문에 그냥 0으로 초기화를 해주었다.



또한 SVD와 달리, 학습을 위해 필요한 하이퍼파라미터인 learning late, beta, epochs num 정도가 추가적으로 필요하다.

def \_\_init\_\_(self, sparse\_matrix, K, lr, beta, n\_epochs): """ Arguments \- sparse\_matrix : user\-item rating matrix \- K (int) : number of latent dimensions \- lr (float) : learning rate \- beta (float) : regularization parameter \- n\_epochs (int) : Num of Iteration """ # convert ndArray self.sparse\_matrix \= sparse\_matrix.fillna(0).to\_numpy() self.item\_n, self.user\_n \= sparse\_matrix.shape self.K \= K self.lr \= lr self.beta \= beta self.n\_epochs \= n\_epochs

다음은 실제로 학습을 수행하는 코드를 살펴보자.

def train(self): self.I \= np.random.normal(scale\=1./self.K, size\=(self.item\_n, self.K)) self.U \= np.random.normal(scale\=1./self.K, size\=(self.user\_n, self.K)) self.item\_bias \= np.zeros(self.item\_n) self.user\_bias \= np.zeros(self.user\_n) self.total\_mean \= np.mean(self.sparse\_matrix\[np.where(self.sparse\_matrix != 0)\]) idx, jdx \= self.sparse\_matrix.nonzero() samples \= list(zip(idx, jdx)) training\_log \= \[\] progress \= trange(self.n\_epochs, desc\="train-rmse: nan") for idx in progress: np.random.shuffle(samples) for i, u in samples: # get error y \= self.sparse\_matrix\[i, u\] pred \= self.predict(i, u) error \= y \- pred # update bias self.item\_bias\[i\] += self.lr \* (error \- self.beta \* self.item\_bias\[i\]) self.user\_bias\[u\] += self.lr \* (error \- self.beta \* self.user\_bias\[u\]) # update latent factors I\_i \= self.I\[i,:\]\[:\] self.I\[i, :\] += self.lr \* (error \* self.U\[u,:\] \- self.beta \* self.I\[i,:\]) self.U\[u, :\] += self.lr \* (error \* I\_i \- self.beta \* self.U\[u,:\]) rmse \= self.evaluate() progress.set\_description("train-rmse: %0.6f" % rmse) progress.refresh() training\_log.append((idx, rmse)) self.pred\_matrix \= self.get\_pred\_matrix()

train() 함수는 다음과 같은 순서로 시작된다.



**1\. Latent Matrix 및 bias 초기화**

위 2개의 지표는 학습을 거칠때 마다, GD를 통해 loss를 줄이는 방향으로 최적화가 이루어지게 된다. 맨 처음에는 그냥 임의의 값으로 아무거나 넣어주었다.

**2\. Sample 선정**

Rating Matrix는 Sparse하기 때문에, 이 중에서 실제로 평가를 한 정보만 학습에 활용되어야 한다. 따라서 평점이 0점인 경우, 평가를 하지 않았다고 의미하므로 Sample에서 제외해주었다.

**3\. Y(실제 평점)과 Pred(예측 평점)과의 Error을 통해 GD 수행**

예측 평점의 정답과의 차이로 Error(loss)값을 구한다. 이때 사용된 공식이 바로 위에서 사용된 공식인데, predict() 함수의 반환값을 Y에서 빼주는 것으로 Error를 측정한다.

def predict(self, i, u): """ :param i: item index :param u: user index :return: predicted rating """ return ( self.total\_mean + self.item\_bias\[i\] + self.user\_bias\[u\] + self.U\[u,:\].dot(self.I\[i,:\].T) )

**4\. 산출된 Error로 Latent Matrix 및 bias 최적화**

GD를 통해 해당 Error에서 Learnig rate 만큼의 step size만큼 loss를 minimize하는 방향으로 움직여주었다.



evalute 방식은 위의 SVD와 완전히 똑같기 때문에 넘어가도록 하겠다.

해당 코드를 실행하려면 아래의 커맨드를 실행하면 된다.

$ python train.py \--sgd \--k 300 \--lr 0.01 \--beta 0.01 \--n\_epochs 100



**Inferences**

위에서 수행한 Evaluate는 Train loss이기 때문에 실제 모델의 성능을 평가하기에는 무리가 있다. 따라서 필자는 일부 데이터(약 20%)를 테스트 셋으로 활용하여 평가를 수행해봤더니, 아래와 같은 결과를 확인할 수 있었다.

<table><tbody><tr><td colspan="1" rowspan="1"><p id="SE-cc59e2bc-4ca7-455b-b44a-d7f0827f8e45"><span id="SE-2509b1e6-f8da-40a0-a752-e1c47de37406"><b>parameters</b></span></p></td><td colspan="1" rowspan="1"><p id="SE-adcfebc3-f89b-47eb-879a-cef24aadbf99"><span id="SE-8937cc1f-9c98-4a2d-b2e9-dce34b4175e8"><b>SVD</b></span></p></td><td colspan="1" rowspan="1"><p id="SE-7d75b3ad-46d3-4a02-9c7d-e18d39386615"><span id="SE-f5020592-99ba-4c4f-8f82-76b0a0d3246e"><b>GD</b></span></p></td></tr><tr><td colspan="1" rowspan="1"><p id="SE-85101293-7ad7-442d-b9b4-b7cab59720f5"><span id="SE-57dce112-6767-4ec1-909d-50a2126b6bb4">K</span></p></td><td colspan="1" rowspan="1"><p id="SE-a9261089-82b5-44b8-8c0e-05f45bfbc3cb"><span id="SE-0c7b455a-bfa7-4890-909e-42168a2cc984">300</span></p></td><td colspan="1" rowspan="1"><p id="SE-04bf517c-4cfe-43db-ab96-49baf4945c12"><span id="SE-14d8db9c-ea0f-47b1-b3a7-ef2d4086c29b">300</span></p></td></tr><tr><td colspan="1" rowspan="1"><p id="SE-51f51681-3f2c-4c42-9005-da21b4339315"><span id="SE-0fc1d6b1-1ad6-4e89-b694-4ee58cd8e2d2">learning rate</span></p></td><td colspan="1" rowspan="1"><p id="SE-09380162-95a6-41b9-a015-7ede52ec6fe6"><span id="SE-fc4dda09-bfd9-4433-9c2d-2ea6549a4f30">-</span></p></td><td colspan="1" rowspan="1"><p id="SE-0e903af1-4181-4270-9a8c-ac0ed96ec8c2"><span id="SE-13eff593-9731-4963-907d-cb2ed2e5fb81">0.01</span></p></td></tr><tr><td colspan="1" rowspan="1"><p id="SE-ef121ecd-1ec9-4bf5-be5c-7338658e20cc"><span id="SE-83a94738-29a8-49c9-abba-574e56b0ebd2">beta</span></p></td><td colspan="1" rowspan="1"><p id="SE-9ad53666-90ee-4379-aff7-d10b3a4c600d"><span id="SE-b1b6ab97-a30f-4c42-b816-2017e746084a">-</span></p></td><td colspan="1" rowspan="1"><p id="SE-29026963-0eab-4519-83a6-df709abef8a0"><span id="SE-f1996b69-3b3b-4968-b62c-cd31dead8150">0.001</span></p></td></tr><tr><td colspan="1" rowspan="1"><p id="SE-59392f17-67c3-44b1-8cea-61fe3fa280b1"><span id="SE-71ec20cd-663a-4855-999c-64067a752bb2">n_epochs</span></p></td><td colspan="1" rowspan="1"><p id="SE-355d676b-dc50-43e5-b458-3c57d1480a74"><span id="SE-88b5bda4-0ae8-4956-9442-d85e6649cfdd">-</span></p></td><td colspan="1" rowspan="1"><p id="SE-7d02baaf-7dcf-4263-a715-01244c41ad0c"><span id="SE-d67c3d5a-60cd-4450-86ec-8b5b1c5eaeaf">300</span></p></td></tr><tr><td colspan="1" rowspan="1"><p id="SE-952f3f32-64b3-44ad-88f9-6d5b85179699"><span id="SE-ed649f0d-6437-4392-bacb-55aab3c70c59"><b>Train RMSE</b></span></p></td><td colspan="1" rowspan="1"><p id="SE-a60bc231-a6e0-460c-9203-e1c0c7f85bf9"><span id="SE-04a55a15-bdf6-49a0-a511-976f391c737b"><b>0.1796847</b></span></p></td><td colspan="1" rowspan="1"><p id="SE-c60c2958-866d-4199-b562-910025ac125f"><span id="SE-33a23314-8f8b-4768-a8cd-05cc4cb7524c"><b>0.060330</b></span></p></td></tr><tr><td colspan="1" rowspan="1"><p id="SE-54e33dbc-d31c-4184-b981-87234bf0d2eb"><span id="SE-a8b74707-e383-4a8b-bded-a16546cbc800"><b>Test RMSE</b></span></p></td><td colspan="1" rowspan="1"><p id="SE-c84a1634-2cc1-4392-b506-4768210c1567"><span id="SE-180c4412-da83-45e5-812f-222beab389c6"><b>3.0905050</b></span></p></td><td colspan="1" rowspan="1"><p id="SE-36feaf77-715d-42f4-bc75-9ace913256ed"><span id="SE-a6a2b358-ba14-4450-91f8-802db41aec7e"><b>0.759681</b></span></p></td></tr></tbody></table>

Train loss 뿐만 아니라 Test loss도 GD 방식이 압도적으로 좋은 걸 확인할 수 있다. RMSE는 해당 loss 값이 실제 차이 그자체이기 때문에 GD 모델에 대한 평가를 한 문장으로 요약하자면

**"평점을 한번 예측할 때, 평균적으로 0.75점 이내의 오차가 발생한다"**라고 볼 수 있을 것이다.

그에 반에 SVD는 Train loss는 그럴듯 해보이지만, 실사용에는 치명적일 것 같다.



이처럼 MF는 굉장히 간단한 코드로도 Sparse Matrix에 대해 높은 성능을 보여주는 것이 특징이지만, 콜드스타트에서 치명적인 단점은 존재한다.



쉽계 예로 들자면,

**\- 새로운 User, Item에 대한 평점 예측은 불가능하거나 예측을 굉장히 못할 것이다.**

**\- User, Item이 있을지라도 평가 정보가 적다면 좋은 성능을 내지 못한다(=데이터가 굉장히 중요하다)**


