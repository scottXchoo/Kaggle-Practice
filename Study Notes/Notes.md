> 일단 라이브러리 관련된 정보들 다 적고 나중에 한꺼번에 정리하기
# Library
## pandas
- 판다스에서 object 타입은 문자열 타입이다.
- apply() 함수
  - DataFrame의 데이터를 일괄 가공해준다.
  - 종종 람다(lambda) 함수와 함께 사용된다.

## matplotlib
- 파이썬으로 데이터를 시각화할 때 표준처럼 사용되는 라이브러리
- `%matplotlib inline`을 추가하면, matplotlib이 그린 그래프를 주피터 노트북에서 바로 출력해준다.

## seaborn
- matplotlib에 고수준 인터페이스를 덧씌운 라이브러리

## numpy
- np.nan_to_num() 함수는 결측값을 모두 0으로 바꾸는 기능을 합니다.
- np.log(y + 1)은 간단히 np.log1p(y)로 표현하기도 합니다.

# Data Visualization
## 분포도
- 회귀 모델이 좋은 성능을 내려면 데이터가 정규분포를 따라야 한다. 편향된 분포는 모델링하면 좋은 성능을 기대하기 어렵다.
  - 로그변환 : 데이터 분포를 정규분포에 가깝게 만들기 위해 가장 많이 사용하는 방법
    ```python
    sns.displot(np.log(train['count']))
    ```
    - 피처를 count가 아닌, log(count)로 변환해 사용 but 마지막에 실제 타깃값인 count로 복원하기 위해 지수변환을 해줘야 함
   
## 포인트플롯
- 막대 그래프와 동일한 정보를 제공하지만, 한 화면에 여러 그래프를 그려 서로 비교해보기에 더 적합하다.

## 회귀선을 포함한 산점도 그래프
- 수치형 데이터 간 상관관계를 파악하는 데 사용합니다.
- seaborn의 regplot() 함수로 그릴 수 있습니다.

## 히트맵
- seaborn()의 heatmap() 함수로 그릴 수 있습니다.
- 상관관계가 낮은 피처는 제거하면 좋다.

### Baseline Model Process
### 1. 데이터 불러오기 (Load the data)
### 2. 기본적인 피처 엔지니어링 (Basic Feature Engineering)
- 이상치 제거
- 데이터 합치기

### 3. 평가지표 계산 함수 작성 (Create an evaluation index calculation function)
### 4. 모델 훈련 (Model training)
### 5. 성능 검증 (Performance verification)
### 6. 제출 (Submission)

## Baseline Model Process
### 1. Load the data (데이터 불러오기)
### 2. Basic Feature Engineering (기본적인 피처 엔지니어링)
- 2-1. Remove Outlier (이상치 제거)
- 2-2. Combine data (데이터 합치기)
  - 판다수의 concat() 함수를 이용하면 축에 따라 DataFrame을 이어붙일 수 있다.
    - `ignore_index = True` : 원래 데이터의 인덱스를 무시하고 이어붙이기
- 2-3. Add derived features (variables) (파생 피처(변수) 추가)
- 2-4. Remove unneeded features (필요 없는 피처 제거)
- 2-5. Divide data (데이터 나누기)
### 3. Create an evaluation index calculation function (평가지표 계산 함수 작성)
### 4. Train Model (모델 훈련)
### 5. Validate model performance (모델 성능 검증)
### 6. Submission (제출)

## Improve Model Performance Process
### 1. Load the data (데이터 불러오기)
### 2. Feature Engineering (피처 엔지니어링)
### 3. Create an evaluation index calculation function (평가지표 계산 함수 작성)
### 4. Optimize hyperparameters (하이퍼 파라미터 최적화)
### 5. Validate model performance (모델 성능 검증)
### 6. Submission (제출)

# Tip 💡
## 피처 엔지니어링
- `month` 피처는 `season` 피처의 세부 분류로 볼 수 있습니다. 데이터가 지나치게 세분화되어 있으면 분류별 데이터 수가 적어서 오히려 학습에 방해가 됩니다.
  - 지나치게 세분화된 피처를 더 큰 분류로 묶으면 성능이 좋아지는 경우가 있다.
- 훈련 데이터의 피처로 사용하려면 훈련 데이터와 테스트 데이터의 공통된 값이 있어야 된다. 없으면, 훈련 데이터에서 그 피처 삭제.
- 머신러닝 모델은 숫자만 인식하기 때문에 훈련할 때는 피처 값을 문자로 바꾸면 안 된다.
- 피처 엔지니어링 하기 전에 이상치를 제거하고 훈련 & 테스트 데이터를 합쳐준다.


- 폭우, 폭설 내릴 때, 자전거 대여하는 것 같은 "이상치"는 제거하면 모델 성능이 더 좋아진다.
- '0'인 데이터가 많다는 것은 결측값이 많다는 얘기이기도 할 수 있으니 해당 피처를 제거할 고려를 하자.
- 다른 참가자들이 베이스라인 모델을 공유하는데, 이를 사용해도 좋고 직접 자신만의 모델을 만들어도 된다. (아니면, 내가 빠르게 베이스라인 모델을 만들어서 공유하는 것도 방법일 듯?)
- 훈련 데이터와 테스트 데이터의 분포가 비슷하면 과대적합 문제가 상대적으로 적기 때문에 훈련 데이터에서 성능이 좋다면 테스트 데이터에서도 좋은 가능성이 큽니다.
