# 1. **Abstract**

- 이 논문에서는 단일 깊은 신경망(single deep neural network)을 사용한 객체 탐지 기법, SSD를 제안합니다.
- SSD는 다양한 크기의 객체를 탐지하도록 서로 다른 해상도를 갖는 여러 feature map의 예측 결과를 활용합니다.
- Region proposal 단계와 반복적인 feature sampling을 제거하여 모든 연산을 단일 네트워크로 수행하기 때문에 훈련하기 수월하고, 다른 탐지 기법에 대해 간단합니다.

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/aca464a4-ec74-492b-bd16-5ad50bf9cffc/Untitled.png)

- SSD는 (300 x 300) 크기의 입력 이미지를 활용할 때 mAP 74.3%를 달성했으며, 59 FPS의 속도를 보였습니다.
    
- Faster R-CNN보다 우수한 성능입니다. 다른 1-stage 모델과 비교해도 SSD는 더 작은 이미지 크기로도 높은 정확도를 보입니다.
    
    ---
    

# 2**. Introduction**

- 최근 객체 탐지 모델은 다음과 같은 접근법을 활용해왔습니다. 1) 경계 박스 추정, 2) 각 경계 박스마다 피처 재추출(resample), 3) 성능 좋은 분류기(feature extractor) 적용. 이 방법은 정확성이 높지만, fps가 낮다는 단점이 있습니다.
- 이 논문에서는 '경계 박스 추정을 위한 피처 재추출'을 하지 않는 딥러닝 기반 객체 탐지 모델을 제안합니다. 정확도도 높은 모델입니다. VOC 2007 테스트 데이터셋에서 59 FPS의 속도로 mAP 74.3%를 달성했죠. Faster R-CNN은 7 FPS 속도로 mAP 73.2%를 기록하고, YOLO는 45 FPS 속도로 mAP 63.4%를 기록한 것과 비교하면 속도와 정확도 측면에서 모두 앞서는 모습입니다. 속도가 빨라진 중요한 이유는 경계 박스 추정과 연이은 피처 재추출(resampling) 단계를 없앴기 때문이고, 정확도가 높은 이유는 multi scale feature map을 각 scale마다 detection을 진행해 주었기 때문입니다.

---

# 3. SSD 모델

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/5b42a8fb-115f-4991-99f3-9435c9119f03/Untitled.png)

- SSD는 합성곱 신경망을 통해 '고정된 크기의 경계 박스 모음'을 예측하고, '각 경계 박스 내 객체 클래스의 존재 여부'를 점수화합니다. 이어서 비최댓값 억제(Non-Maximum Suppression, NMS)를 적용해 최종적으로 object detection 결과를 도출합니다.

**Multi-scale feature maps for detection**

> 기본 네트워크 다음에 여러 convolution feature layer을 덧붙였습니다. 다양한 scale의 object들을 detection 하기 위해 layer의 크기는 점점 작아집니다. 각 feature layer마다 객체 탐지를 수행하고, 그 결과를 한 곳으로 추출하게 됩니다.

**Convolutional predictors for detection**

> 추가된 각 feature layer는 convolution filter를 통해 object detection를 수행합니다. 이때 detection된 결과의 크기는 일정합니다. 다음 그림의 위쪽은 SSD 모델 구조를 나타냅니다.

- SSD 모델은 기본 네트워크 끝에 여러 feature layer을 덧붙였습니다. 각 feature layer마다 '다양한 스케일과 가로세로 비율을 갖는 디폴트 박스(Anchor box)의 좌표값'과 '해당 디폴트 박스가 나타내는 객체의 존재 여부 신뢰도'를 예측합니다.

**Default boxes and aspect ratios**

- SSD 네트워크에서는 다양한 피처 맵을 구한다고 했습니다. 다양한 피처 맵마다 디폴트 박스와 피처 맵의 각 grid와 연관시킵니다. 각 grid마다 Anchor box의 좌표와 각 객체들의 **sofmax값**(bounding box안에 어떠한 객체가 있을지에 대한 확률값)을 가집니다.
- 객체 클래스가 c개 있다고 하면, 각 디폴트 박스마다 예측하는 값은 (c + 4)개입니다(경계 박스 좌표는 4개이기 때문에). 한 픽셀마다 갖는 anchor box가 k개라 하면, 한 grid마다 예측하는 값은 (c + 4) * k개입니다. 만약 feature map의 크기가 m x n이라면 feature map당 예측하는 값은 (c + 4)k* m* n개입니다. 만약 3*3의 conv이 있다면

$$ Conv = 3_3_(k*(classfier+4) $$

이렇게 정리해볼 수 있습니다.

## 3-2. Training

**Matching strategy**

훈련하는 동안 어떤 anchor box가 ground truth box(, GT Box)와 매칭되는지 확인해야 합니다. GT Box에 매칭되는 anchor를 찾는 방식으로 훈련이 진행됩니다. 매칭되는 anchor box는 크기도, 위치도, 가로세로 비율도 다양합니다. GT Box와 IoU가 0.5보다 큰 anchor box를 찾습니다. IoU가 가장 큰 anchor box 하나만 찾는 게 아니라, 우선은 0.5보다 큰 모든 anchor box를 찾습니다.

**Training objective**

SSD 훈련 손실 함수는 MultiBox 훈련 손실 함수에서 가져왔습니다. 다만, 여러 객체 클래스를 다루도록 조금 변형했습니다. 각 객체 클래스마다 anchor box와 GT Box가 매칭되는지 여부를 다음 변수로 설정합니다.

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/4ab61479-811d-4659-93ba-aa7801431716/Untitled.png)

손실 함수는 아래와 같습니다.

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/3d330827-40ec-47d6-994b-5e9280c29c16/Untitled.png)

localization 손실은 예측한 anchor box(l)와 GT Box(g) 사이의 거리를 Smooth L1 손실로 구한 값입니다.

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/69d717f3-e8af-438d-8e54-2dcc85743b2a/Untitled.png)

confidence loss는 각 객체 클래스마다 sofmax로 구합니다.

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/92a631a4-bc1f-454a-ae43-1c0553c7d4e8/Untitled.png)

---

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/17135085-3a1f-4f6d-933c-3b4b94bbdd58/Untitled.png)

- 다양한 객체 크기를 다루기 위해 Overfeat이나 SPP-net 모델에서는 이미지를 다양한 크기로 처리합니다. 하지만, SSD에서는 conv layer의 feature map을 여러 scale로 처리합니다. 이렇게 하면 모든 객체 scale끼리 파라미터도 공유할 수 있습니다. 또한, 각 conv layer의 feature map을 활용하기 때문에 객체의 전반적인 특성도 파악할 수 있고, 세세한 특성도 파악할 수 있습니다.

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/e77dd663-c69a-4ecd-9d6c-87726f11cb27/Untitled.png)

- 위 그림과 같은 방식으로, 여러 feature map을 활용해 object detection을 수행합니다. 이때 사용하는 k개의 anchor box의 크기는 feature map마다 일정합니다. 그러면 큰 feature map에서는 anchor box가 작은 객체를 탐지할 테고, 작은 feature map에서는 큰 객체를 탐지할 겁니다. 따라서 feature map이 점점 작아지면서 상대적으로 큰 object들을 detection 수행해나갑니다.

---

# 4. S**ummary**

1. SSD에서는 feature **re-extraction**을 하지 않아 faster-rcnn보다 속도가 향상된다.
2. 다양한 scale의 object들을 detection 하기 위해 layer의 크기는 점점 작아진다.
3. 각 conv layer마다 object detection을 수행한 결과를 마지막 layer에 결과를 반영하고, NMS를 수행하여 GT Box와 가장 비슷한 값만 예측한다.