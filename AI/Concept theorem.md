# Overfitting
    Training error가 generalization error(학습되지 않은 일반적인 데이터)에 비해 현저히 낮아지는 현상

## Underfitting
    모델의 capacity가 부족하여 Training error가 충분히 낮지 않은 현상
---
## __분류(Classification)와 회귀(Regression)__
1. 이진 분류 문제(Binary Classification)
    - 이진 분류는 주어진 입력에 대해서 둘 중 하나의 답을 정하는 문제이다.
    - input이 기존의 Linear Layer를 통과하고,output이 sigmoid 함수를 통과하여 y_hat을 만든다.
2. 다중 클래스 분류(Multi-class Classification)
    - 다중 클래스 분류는 주어진 입력에 대해서 세 개 이상의 정해진 선택지 중에서 답을 정하는 문제이다.
    - 이진 분류에서 sigmoid 함수가 아닌 softmax 함수를 사용하여 y_hat을 만든다.
    - 예를 들어 개와 고양이 참새를 분류할 때, 실제값이 '개'라면 y는 (1,0,0)인 3차원 원핫 백터가 된다.
 이처럼 원핫 백터는 무작위성과 균등함을 가지고 있고, 선택지의 수에 따라 백터의 차원이 달라진다.