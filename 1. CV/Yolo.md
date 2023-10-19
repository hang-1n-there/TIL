- 1 stage detector. localization , classification 을 동시에 실행한다.
---
### Yolo의 동작과정
- 
	1.  먼저 사진이 입력되면 가로 세로를 동일한 그리드 영역으로 나눈다.
	2. 각 그리드 영역에 대해 객체가 존재하는지에 대한 bbox와 Confidence score를 예측합니다.
	3. core가 높을수록 bbox의 테두리가 굵어진다.
	4. 이와 동시에 각 grid 셀에 대한 classification이 수행된다.
-