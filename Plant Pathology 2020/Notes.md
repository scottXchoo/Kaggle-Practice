
### 검증 및 테스트 데이터용 변환기
```python
transform_test = A.Compose([
    A.Resize(450, 650),
    A.Normalize(),
    ToTensorV2()
])
```
- 크기는 당연히 훈련 데이터와 똑같에 맞추는 게 좋고
- 픽셀 값 범위도 비슷해야(정규화해야) 서로 비교하기 쉽습니다.
- 파이토치는 텐서 객체만 취급하기 때문에 ToTensorV2() 변환기도 꼭 필요합니다.

### 멀티프로세싱
- 모델 훈련 시간을 단축시켜줄 수 있다.
- 멀티프로세싱을 사용하려면 다음과 같이 데이터 로더의 시드값을 고정해야 합니다. 먼저 seed_worker()를 정의하고 제너레이터를 생성합니다.
```python
def seed_worker(worker_id):
    worker_seed = torch.initial_seed() # 2**32
    np.random.seed(worker_seed)
    random.seed(worker_seed)
    
g = torch.Generator()
g.manual_seed(0)
```

### 파이토치로 사전 훈련 모델을 이용하는 방법
1. torchvision.models 모듈 이용
2. pretrainedmodels 모듈 이용
3. 직접 구현한 모듈 이용

### Tip
- 학습률은 0.00006으로 설정했습니다. 여러 학습률을 적용해 실험해보시기를 권장합니다.
- '매 에폭마다 검증'하는 방식으로 훈련을 진행하겠습니다. 더 오래 걸리지만, 과대적합 없이 훈련이 잘 되고 있는지 확인할 수 있다는 장점이 있습니다.
```python
from tqdm.notebook import tqdm # 진행률 표시 막대
```


### 'best single model' : 적합한 모델 찾는 토론 활용
