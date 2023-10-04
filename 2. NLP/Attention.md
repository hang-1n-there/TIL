: 출력 단어를 예측하는 매 time step마다 인코더의 전체 입력 문장을 참고한다. 이 때, 동일한 비율로 참고하는 것이 아니라 해당 시점에서 연관 있는 부분을 집중해서 본다.

![[Pasted image 20231004081630.png]]

```
1. 어텐션 스코어를 구하기 위해 디코더의 hidden state를 전치하고, 인코더의 hidden state와
내적한다. 
2. 어텐션 스코어의 집합을 소프트맥스를 통해 어텐션 분포(가중치)를 구할 수 있다.
3. 인코더의 hidden state와 각 인코더의 어텐션 가중치를 가중합하여 어텐션 값을 구한다.
4. t시점의 어텐션 값과 디코더의 hidden state를 concat하여 y_hat의 입력으로 사용된다.
5. 출력층으로 보내기 전에 가중치와 곱하고 편향을 더해 tanh 함수를 지납니다.
6. 따라서 t시점의 y_hat은 softmax(Wy+s_hat_t+b_y)입니다.
```
---
![[Pasted image 20231004081155.png]]

- Query : Decoder의 hidden state
- Key : Query와의 연관성을 체크해야 하는 Enconder LSTM 셀의 hidden vector들
- Value : key에서 빼온 hidden vector들
> 유튜브에 무언가를 검색한다면, 검색 창의 텍스트는 *Query* 이고, 비디오의 제목은 *Key*, 그 안의 내용을 