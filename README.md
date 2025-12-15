# Household Power Consumption Forecasting (LSTM)

본 프로젝트는 **가정 전력 사용 시계열 데이터**를 기반으로 LSTM(Long Short-Term Memory) 모델을 학습하여 **미래 전력 소비량을 예측**하고, 특히 피크 시간대(peak time)에서의 예측 성능과 오차 특성을 분석하는 것을 목표로 한다.

---

## 1. Project Overview

* **과목**: Data Mining (서울과학기술대학교 컴퓨터공학과)
* **주제**: LSTM 기반 가정 전력 소비 시계열 예측
* **핵심 목표**
  * 과거 전력 사용 패턴 학습
  * 단기 미래 전력 소비량 예측
  * 전체 구간 vs 피크 타임 구간 예측 성능 비교

---

## 2. Dataset

* **Dataset**: UCI Electric Power Consumption Dataset (Kaggle)
* **기간**: 2006-12 ~ 2010-11
* **해상도**: 1분 단위 시계열 데이터
* **주요 변수**
  * Global_active_power
  * Global_reactive_power
  * Voltage
  * Global_intensity
  * Sub_metering_1, 2, 3

> 데이터는 KaggleHub를 사용하여 자동 다운로드하며, GitHub에는 업로드하지 않는다.
---

## 3. Project Structure

```text
household-power-lstm-forecasting/
│
├── data/
│   ├── raw/                # 원본 데이터 (.txt)
│   └── processed/          # 전처리 완료 CSV
│       └── power_1min.csv
│
├── notebooks/
│   ├── eda.ipynb           # 탐색적 데이터 분석
│   ├── preprocess.ipynb    # 시계열 전처리
│   └── train.ipynb         # LSTM 학습 및 평가
│
├── src/
│   ├── download_data.py    # Kaggle 데이터 다운로드
│   ├── load_and_clean.py   # 데이터 로드 및 정제
│   └── model_lstm.py       # LSTM 모델 정의
│
├── runs/
│   └── figures/            # 결과 시각화 이미지
│
└── README.md
```

---

## 4. Data Processing Pipeline

### (1) Data Download

```bash
python src/download_data.py
```

* KaggleHub를 통해 데이터 자동 다운로드
* `data/raw/` 디렉토리에 원본 파일 저장

### (2) Data Cleaning & Preprocessing

```bash
python src/load_and_clean.py
```

* Date + Time → Datetime 인덱스 생성
* 결측치 처리: forward fill (ffill)
* 모든 변수 float 타입 변환
* 결과 저장: `data/processed/power_1min.csv`

---

## 5. Exploratory Data Analysis (EDA)

* 일/주 단위 전력 사용 패턴 분석
* 시간대별 평균 전력 소비 확인
* 피크 시간대(peak time) 정의 및 분포 확인

EDA는 `notebooks/eda.ipynb`에서 수행한다.

---

## 6. Model: LSTM

### Model Architecture

```text
Input (time window × features)
 → LSTM (units=64)
 → Dropout (0.2)
 → Dense (1)
```

* **Loss**: Mean Squared Error (MSE)
* **Metric**: MAE
* **Optimizer**: Adam

모델 정의는 `src/model_lstm.py`에 구현되어 있다.

---

## 7. Training & Evaluation

* 학습 및 평가는 `notebooks/train.ipynb`에서 수행
* 전체 테스트 구간 성능 평가
* **Peak time 샘플만 분리하여** 실제값 vs 예측값 비교

### Evaluation Focus

* 전체 구간 RMSE / MAE
* 피크 시간대 예측 오차 증가 여부
* 피크 구간에서의 LSTM 한계 분석

---

## 8. Results

* 일반 시간대에서는 비교적 안정적인 예측 성능 확인
* 피크 시간대에서는 급격한 변동으로 인해 예측 오차 증가
* 피크 구간은 불규칙성 증가 → 단순 LSTM의 한계 확인

---

## 9. Future Work

* 다변량 입력(feature engineering 강화)
* Attention 기반 모델 또는 Transformer 적용
* 피크 타임 전용 모델 분리 학습
* 외생 변수(날씨, 요일, 공휴일) 추가

---

## 10. References

* UCI Machine Learning Repository: Electric Power Consumption Dataset
* Kaggle: [https://www.kaggle.com/datasets/uciml/electric-power-consumption-data-set](https://www.kaggle.com/datasets/uciml/electric-power-consumption-data-set)
* Hochreiter, S., & Schmidhuber, J. (1997). *Long Short-Term Memory*

---

> 본 프로젝트는 **학습용 / 과제 제출용**으로 작성되었으며, 재현 가능성과 코드 구조화를 중점으로 한다.
