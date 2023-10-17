[[Attention]] 

### 전이 학습 (Transer Learning)
- 특정 태스크를 학습한 모델을 다른 태스크 수행에 재사용하는 기법
- 전이 학습 방법은 **업스트림 태스크**, **다운스트림 태스크** 두 가지로 나뉜다.
### 업스트림 태스크 (Upstream task)
- 넓게 배우는 것. 넓게 배우기 위해 corpus를 학습한다. 대표적인 방법으로는 크게 2가지가 있다.
	1.  단어(문장) 맞히기 (Next Sentence Prediction, **NSP**)
		- 문장의 연관성을 학습하여 다음 단어가 무엇이지 맞히는 것
		
	1.  빈칸 맞히기 (Masked Language Model, **MLM**)
		- 단어에 빈칸을 둔 뒤 이를 맞히는 것
> Pre-training : 이러한 방법을 통해 업스트림 태스크를 수행하는 것





