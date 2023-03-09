# Overfitting
    Training error가 generalization error(학습되지 않은 일반적인 데이터)에 비해 현저히 낮아지는 현상

## Underfitting
    모델의 capacity가 부족하여 Training error가 충분히 낮지 않은 현상
---
## __분류(Classification)와 회귀(Regression)__
1. 이진 분류 문제(Binary Classification)
    - 이진 분류는 주어진 입력에 대해서 둘 중 하나의 답을 정하는 문제이다. 
    - input이 기존의 Linear Layer를 통과하고,output이 sigmoid 함수를 통과하여  y_hat을 만든다.