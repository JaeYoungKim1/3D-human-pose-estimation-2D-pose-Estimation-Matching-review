2D pose x 는 joint수 * 2 차원의 실수텐서에 속해있고, 3D pose X는 joint수 * 3 차원의 실수텐서에 속해있다.
결합확률의 일반적인 표현은 p(X,x,I) = p(X|x,I) * p(x|I) * p(I)로 나타낸다.

조건부 독립) 3D pose X가 2D pose x로 주어진 이미지 I와 조건부로 독립적이라고 가정한다. 2D 스켈레톤이 주어지고 그에맞는 3D 스켈레톤의 예측을 할땐 2D 이미지 측정에 영향을 안받는다고 가정하는 것
실제로는 아니지만, 1차 근사치로 보일 수 있다. 이 인수분해는 여전히 Image -> 2D pose인 p(x|I)를 임의로 복잡해지도록 하는데, occlusion동안 2D 프로젝션과 이미지 특징 간의 복잡한 상호작용을 모델링하는데 필요하다.
 p(X,x,I) = p(X|x)*p(x|I)*p(I) 에서, P(X|x)는 P(X|x,I)에서 3D 포즈인 X와 이미지I가 독립적이라서 제외한 간단한 식이고 Nearest Neighbor 방법으로 2D pose를 3D pose로 바꿀 수 있다. 
 p(x|I)는 RGB이미지에서 2D pose로 바꾸는 것으로, 히트맵을 예측하도록 CNN을 만들어 해결한다.
RGB Image -> 2D pose estimation

3.1 Image-based 2D pose Estimation

독립적인 추정이 주어져서, 이미지 측정으로 2D pose를 예측할 수 있다. P(x|I) = CNN(I)로 이미지가 주어졌을때 2D 포즈의 조건을 모델링 할 수 있다. 여기서 CNN은 N개의 2D 히트맵을 반환하는 Non-linear function이라고 가정한다.
개별 body joint에 대해 N개의 히트맵을 반환하는 convolution pose machine(CPMs)를 사용할 것이다. 히트맵을 정규화하여 히트맵을 각 joint의 주변분포(marginal distribution)으로 해석할 수 있다.

CPM은 가장 최신의 포즈 추정 시스템이다.(MPII 데이터셋에서 88.5% PCKh라는 것이 나온다고 하는데, 가장 최신 값인 90.9%에 매우 근접하다고 한다.) CPM은 MPII데이터셋으로 학습되었으며, 이 모델을 휴먼3.6M 학습셋으로 파인튜닝하였다.
<b>ㅁㄴㅇㄹ</b>
