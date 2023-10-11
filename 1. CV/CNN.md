## 1. 배경

- 하나의 이미지를 fc layer로 분류한다고 가정했을 때, 이미지의 픽셀을 일렬로 나열해서 벡터로 만든 뒤 연산을 수행하게 된다. 이렇게 되면 어떤 이미지였는지 알아보기 힘들고 공간적인 정보가 유실된 상태가 된다. 결국 이미지의 공간적인 구조 정보를 보존하면서 학습할 수 있는 방법이 필요해졌고, 이를 위해 합성곱 신경망을 사용한다.

---

## 2. 채널

- 이미지의 크기는 **(Channel * Height * Width)**로 이루어져 있는데, 여기서 **Channel은** 이미지의 색 정보를 의미한다.
- Gray Scale의 이미지는 채널 수가 1이고, 컬러 이미지는 **R G B**를 포함하여 채널 수가 3개가 된다.

---

## 3. 합성곱 연산

- 합성곱 층(Convolution Layer)에서 합성곱 연산(Convolution Operate)은 이미지의 특징을 추출하는 연산이다.
- 한 이미지에 대하여 Filter , kernel라는 n*m 행렬을 곱하여 행렬연산을 수행한 뒤 더해준다. 이 때 이미지와 filter의 계산 순서는 왼쪽 맨 위에서 오른쪽 맨 아래로, 순차적으로 연산을 수행하게 된다. 이 연산의 결과를 **Feature Map** 이라고 한다.

---

## 4. 패딩

- padding은 테두리 바깥쪽의 빈 공간을 뜻한다.
- feature map이 작아지는 문제를 해결하기 위해 padding 값을 설정하여 차원을 조절할 수 있다.

---

## 5. Stride

- 합성곱 연산을 수행할 때, 순차적으로 이동하는 필터의 이동 간격을 의미한다.
- Stride의 크기가 증가할수록 feature map의 크기도 작아지고, 모델의 계산 복잡도도 줄어든다.

---

## 6. Pooling

- Convolution Operate를 수행할 때 이미지를 feature map으로 나눠서 값을 추출하여 feature map을 만들게 된다.
- max pooling은 최댓값을, average pooling은 평균값을 feature map에 추출한다.

---

## 7. Feature map의 parameter 수

- Input height : H
- Input width : W
- Filter height : FH
- Filter width : FW
- Stride : S
- Padding : P

$$ Output height = {(H + 2P) - FH} /S + 1 $$

$$ OutputWidth = {(H + 2P) - FH} /S + 1 $$

---
## 8. Pooling Layer 계산
$$ OutputRowSize = InputRowSize / PoolingSize $$
$$ OutputColumnSize = InputColumnSize / PoolingSize $$
---
