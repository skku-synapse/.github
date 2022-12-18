2022 산학협력 프로젝트 시냅스 이미징
----------------------------
One-Class Classification for Anomaly Detection By CS-Flow
=========================================================
<center><img src="profile/simulator.png"></center><br>

> **시냅스이미징**사와 함께 진행한 프로젝트입니다. <br>첨단 전자제품 제조 현장에서 수 백만 장의 정상 샘플이 획득되는 동안, **소수의 결함 샘플**이 취득되는 상황이 대부분입니다. 
<br> 이 경우 결함 데이터셋을 확보하는데 **시간과 비용**이 많이 들어갑니다.
<br> 따라서 저희는 결함 이미지 없이, **정상 이미지로만 학습**하여 양불을 구분하는 **One-class Classification Anomaly Detection** 모델을 개발하였습니다.

<br>

# 개발 과정
## 4 ~ 5월: 선행 학습
- 머신 러닝 기반 이상치 모델 실습
    - 구현 모델: One-class SVM, Isolation Forest
    - 데이터셋: 산업용 공개 데이터 MVTec AD
- 딥러닝 기반 이상치 모델 실습
    - 구현 모델: CS-Flow, CFlow-AD, OCR-GAN, FastFlow, PaDiM
    - 데이터셋: MVTec-AD
## 6 ~ 8월: 하계 집중학습
- 제조공정 이미지 데이터셋 분석
    - 데이터셋: 기업에서 제공한 카메라 렌즈, Flex, SMT(카메라 모듈) 데이터에 대해 분석.
    - Anomaly Detection 모델 개발
            - SOTA 모델 중 CS-Flow, CFlow-AD, FastFlow, PatchCore, CFA를 선정하여 집중 개발 및 최적화.
    - 성능평가 지표 정리 및 목표 설정
            - 성능평가 지표: 미검율, 과검율, 모델 사이즈, 분류 속도
            - 목표: 미검율 0.5% 이내에서 과검율 30% 이내 <br> 
                모델 사이즈 300MB ~ 400MB 이내
    - Anomaly Detection 시뮬레이션 SW 제작
            - 모델의 Test 결과를 실시간으로 시각화해서 보여줌.
## 9월 ~ 12월: 모델 최적화
- 모델 선정
    - 모든 성능평가 지표를 종합해봤을 때 **CS-Flow**가 선정됨.
- CS-Flow 성능 개선
    - 하계 집중학습 결과 모델 사이즈 1GB 정도로 크고, Flex 데이터와 SMT 데이터의 과검율이 목표를 만족하지 못했음.
    - 성능 개선 방법
            - **모델 사이즈 축소**: 모델의 파라미터를 조정하여 사이즈를 300MB대로 축소.
            - **데이터셋 재분류**: 보다 정확한 기준으로 모호한 데이터들을 재분류함.
            - **Score 산출 방식 변경**: 픽셀 별 score에서 최종 score를 산출하는 방식을 평균에서 표준편차 계산으로 바꿈.
            - **Data Agumentation 실험**: Random Crop, Regular Crop 등으로 데이터를 증강함.

<br>

# 성능 평가

<br><img src=".github/profile//lens.png" width="300px"><br>

### • 카메라 렌즈 데이터
- 특징: 이상치 데이터의 불량 크기가 큼.

|모델|미검율|과검율|모델 사이즈|분류 속도|
|:---:|:---:|:---:|:---:|:---:|
|**CS-Flow**|**0.0%**|**15.2%**|371MB|23ms|
|PatchCore|0.0%|29.5%|64MB|81ms|
|CFA|0.0%|54.3%|184MB|65ms|

<br><img src=".github/profile//flex.png" width="300px"><br>

### • Flex 데이터
- 특징: 이상치 데이터의 불량이 흐리고 양/불 구분이 애매함.

|모델|미검율|과검율|모델 사이즈|분류 속도|
|:---:|:---:|:---:|:---:|:---:|
|CS-Flow|0.13%|35.8%|371MB|23ms|
|**CS-Flow**|**0.50%**|**25.4%**|371MB|23ms|
|PatchCore|0.18%|68.1%|301MB|81ms|
|CFA|0.55%|55.0%|184MB|65ms|

<br><img src=".github/profile//smt.png" width="300px"><br>

### • SMT 데이터 (카메라 모듈)
- 특징: 이상치 데이터의 불량 크기가 상대적으로 작음.

|모델|미검율|과검율|모델 사이즈|분류 속도|
|:---:|:---:|:---:|:---:|:---:|
|CS-Flow|0.16%|24.8%|371MB|23ms|
|**CS-Flow**|**0.49%**|**15.6%**|371MB|23ms|
|PatchCore|0.0%|27.0%|482MB|81ms|
|CFA|0.0%|91.4%|184MB|65ms|

<br>

# Anomaly Detection Simulator
## 기능
1. Test 데이터셋의 Prediction 결과 시각화.
2. Analysis를 통해 과검율, 미검율, 정확도를 확인할 수 있음.
3. Score Histogram을 통해 양/불 이미지의 score를 확인하고 threshold를 조정할 할 수 있음.
