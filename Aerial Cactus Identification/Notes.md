### 딥러닝 모델링 절차 by 파이토치

1. 시드값 고정 및 GPU 장비 설정
   - 시드값 고정 : 결과 재현을 위한 작업
   - GPU 장비 설정 : 훈련 속도를 높이기 위해 데이터를 GPU가 처리하도록 변경

2. 데이터 준비
   - 훈련/검증 데이터 분리
   - 데이터셋 클래스 정의 : 이미지 데이터를 모델링에 적합한 형태로 불러오도록 해줌
   - 데이터셋 생성
   - 데이터 로더 (데이터셋으로부터 데이터를 배치 단위로 불러와주는 객체) 생성

3. 모델 생성 : 신경망 모델 클래스를 직접 설계한 후 인스턴스 생성

4. 모델 훈련
   - 손실 함수와 옵티마이저 설정 : 훈련에 앞서 손실 함수(예측값과 실젯값의 차이를 구하는 함수)와 옵티마이저(최적 가중치를 찾아주는 알고리즘) 설정
   - 모델 훈련 : 신경망의 가중치(파라미터)를 갱신하며 모델 훈련

5. 성능 검증 : 검증 데이터로 모델 성능 검증

6. 예측 및 제출 : 테스트 데이터로 예측 후 결과 제출

### 이미지 데이터 압축 푸는 방법
```python
from zipfile import ZipFile

# 훈련 이미지 데이터 압축 풀기
with ZipFile(data_path + 'train.zip') as zipper:
    zipper.extractall()
    
# 테스트 이미지 데이터 압축 풀기
with ZipFile(data_path + 'test.zip') as zipper:
    zipper.extractall()
```

### 이미지 출력하는 방법
```python
import matplotlib.gridspec as gridspec
import cv2 # OpenCV 라이브러리 임포트

mpl.rc('font', size = 7)
plt.figure(figsize = (15, 6))  # 전체 Figure 크기 설정
grid = gridspec.GridSpec(2, 6) # 서브플롯 배치 (2행 6열로 출력)

# 1. 선인장을 포함하는 이미지 파일명 (마지막 12개)
last_has_cactus_img_name = labels[labels['has_cactus'] == 1]['id'][-12:]

# 2. 이미지 출력
for idx, img_name in enumerate(last_has_cactus_img_name):
    img_path = 'train/' + img_name                 # 3. 이미지 파일 경로
    image = cv2.imread(img_path)                   # 4. 이미지 파일 읽기
    image = cv2.cvtColor(image, cv2.COLOR_BGR2RGB) # 5. 이미지 색상 보정
    ax = plt.subplot(grid[idx])
    ax.imshow(image)                               # 6. 이미지 출력
```
5번 : cv2.imread()로 이미지를 읽으면 색상 채널을 BGR(파랑, 초록, 빨강) 순서로 불러옵니다. 하지만 RGB 순서이어야 실제 색상과 맞으므로 채널 순서를 바꿔줘야 합니다.

### Tips
- 파이토치로 신경망 모델을 구축하려면 데이터셋도 일정한 형식에 맞게 정의해줘야 합니다.
- 파이토치로 이미지를 처리할 때는 형상이 (채널 수, 가로 픽셀 수, 세로 픽셀 수) 순서여야 합니다.
- 배치 크기는 2의 제곱수로 설정하는 게 효율적입니다.
  - 배치 크기가 작으면 규제 효과가 있어 일반화 성능이 좋아집니다.
  - 단, 한 번에 불러오는 데이터가 적어 훈련 이터레이션이 많아지고 훈련 시간도 길어집니다.
  - 게다가 배치 크기가 작을수록 학습률도 작게 설정해야 하는데, 훈련 시간을 지연시키는 또 다른 원인입니다.
