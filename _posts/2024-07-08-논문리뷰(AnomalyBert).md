---
layout: single
title:  "논문리뷰"
---

# ANOMALYBERT (ICLR 23)

## Motivation

- **현실적인 문제 :** 제조 및 다양한 상황 속에서 많은 시계열 데이터들이 관측된다. 하지만 모든 데이터에 대해 이상치 여부를 라벨링하기 어렵다.
- 사전지식 필요 : 시계열 데이터에서 나타나는 이상치의 종류는 point-global, point-contextual, pattern-shapelet, pattern-seasonal, pattern-trend 5가지가 있다. 하지만 5가지 종류의 이상치를 생성하기 위해서는 해당 시계열 데이터에 대한 탐색이 필요하다. pattern-shapelet의 경우, 기존과 다른 pattern을 생성해야 하기에 기존 pattern이 무엇인지 알아야 한다.
    
    
    
    **고려대학교 산업경영공학부 DSBA 연구실-유튜브에서 발취**
    
- 그렇기에 저자는 간단한 이상치 생성 방법을 통해 학습하는  Self-Supervised Method를 제시한다.

---

## Model Architecture



### 1. 데이터 : Data Degradation(이상치 생성) →  시계열 데이터 Patching

- Data Degradation
    
    
    
    - A weighted sequence with the outside of the window (Soft replacement) : 다른 window의 값과의 랜덤 교체
    - A constant sequence (Uniform replacement)
    - A lengthened or shortened sequence (Length adjustment)
    - A single peak value (Peak noise)
    
    → 해당 Data Degradation을 통해 사전지식 필요 없이 시계열의 5가지 이상치 종류를 다 커버할 수 있다.  저자는 실험을 통해 이를 증명했고, 아래의 테이블은 Data Degradation을 통해 학습한 모델이 5가지 종류 이상치 분류 성능을 나타낸 것이다.
    
    

### 2. 모델 : Linear Embedding Layer for Patches → Transformer(1D relative position bias) → Prediction Block for Anomaly score($a_t$)

- 1D relative position bias
    
    : Pair 간 상대적인 순서나 거리가 중요한 경우 사용 → Patch 간의 상대적인 거리 반영
    
    $$
    ⁍
    $$
    
    
    **Rethinking and Improving Relative Position Encoding for Vision Transformer (CVPR 21)**
    

### 3. Loss Function : binary cross entropy

- 전체 데이터 포인트를 정상/비정상 포인트로 한 번에 분류 ($a_t$ : Anomaly score)

$$
L = -\frac{1}{N} \sum^{t_1}_{t=t_0} I_{[t^*_0, t^*_1]}(t)*loga_t + (1-I_{[t^*_0, t^*_1]}(t))*log(1-a_t)
$$

---

## Result

- 모든 데이터셋에서 가장 높은 $F1$ 및 $F1_{PA}$에서도 준수한 성능



- Soft replacement 기법이 가장 효과적 / Length adjustment는 데이터에 따라 유용할 수도, 안 좋을 수도..



---

## 나의 생각

- 합리적인 이상치 생성 방법
- 1D relative position 기법은 특히 시계열에서 유용할 듯
- PITS와 같은 Patch-Independent 모델 시도도 좋을 듯
- TS2VEC과 같은 대조학습 기법도 적용해볼만한 여지가 있는 듯
