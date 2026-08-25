# AI-Health-Bio-Data-Team-4-Professor-yoon-
[README.md](https://github.com/user-attachments/files/31420795/README.md)
# ADNI 기반 뇌 연령 측정 (Brain Age Gap) 및 알츠하이머 취약성 분석

이 프로젝트는 ADNI의 구조적 T1 MRI FreeSurfer ROI를 이용해 정상 인지군(NCI)을 기준으로 뇌 연령을 예측하고, 실제 연령과의 차이인 Brain Age Gap(BAG)을 계산하여 MCI와 알츠하이머병(AD)에서의 뇌 노화/취약성을 탐색하는 분석 노트북입니다.

본 노트북은 다음을 수행합니다.

- ADNI 데이터셋에서 T1 MRI 기반 FreeSurfer 요약 변수를 로드
- 정상 인지군을 기준으로 뇌 연령 예측 모델 학습
- 예측 연령의 편향 보정 및 BAG 계산
- 진단군별 BAG 차이 비교
- 인지 점수 및 임상 변수와의 연관성 탐색
- 해석 가능한 주요 ROI(뇌 영역) 확인
- 필요한 경우 결과를 CSV 또는 시각화 형태로 저장

> 본 결과는 연구용 분석 결과이며, 개인 진단, 치료 결정, 임상적 위험 예측의 근거로 사용해서는 안 됩니다.

---

## 1. 프로젝트 목적

핵심 원칙은 질병으로 인한 뇌 위축을 정상 노화로 학습하지 않도록, 나이 예측 모델을 정상 인지군(NCI)만으로 학습하는 것입니다.

- 모델 학습: NCI만 사용
- BAG 계산: 실제 연령과 예측 뇌 연령의 차이
- 해석: BAG가 양수이면 뇌가 실제 연령보다 더 늙어 보이는 상태
- 비교 대상: MCI, AD, NCI 간 BAG 차이 확인

이 방식은 뇌의 기저 정상 노화를 기준선으로 삼아, 질병군의 추가적 뇌 노화/취약성 정도를 상대적으로 평가하는 데 초점을 둡니다.

---

## 2. 사용 데이터

본 노트북은 ADNI의 구조적 뇌 영상 데이터와 임상 메타데이터를 사용합니다.

주요 입력 파일(예시):

- ADSP_PHC/ADSP_PHC_T1_FS_22Jan2026.csv
- Assessments/DXSUM_22Jan2026.csv
- 기타 ADNI 제공 PHC, 미세 구조/인지 관련 파일

데이터는 일반적으로 로컬 경로 또는 Google Drive/Colab 환경에서 압축 해제 후 불러옵니다.

> 데이터셋은 ADNI 배포 정책에 따라 사용 범위와 재배포 제한이 있으므로, 개인 식별 정보가 포함된 자료는 외부 공유를 삼가야 합니다.

---

## 3. 노트북 구조

이 워크스페이스에는 다음과 같은 파일이 포함되어 있습니다.

```text
.
├─ brain_age_gap_alzheimer_vulnerability_colab.ipynb
├─ brain_age_gap_alzheimer_vulnerability_colab_ipynb의_사본의_사본.ipynb
├─ README.md
├─ ADNI 기반 뇌 연령 측정 (BAG 예측) 후 취약 부위 시각.txt
├─ 인영/
│  └─ ADNI.V3/
│     └─ ADNI.V3/
│        ├─ ADNI_Brain_Age_Gap_V3_Integrated.ipynb
│        ├─ README_KO.md
│        ├─ requirements.txt
│        ├─ modeling_data/
│        └─ outputs/
├─ 프로젝트용 자료zip/
└─ ...
```

주요 노트북은 아래 두 개입니다.

- `brain_age_gap_alzheimer_vulnerability_colab.ipynb`
- `brain_age_gap_alzheimer_vulnerability_colab_ipynb의_사본의_사본.ipynb`

이 README는 현재 작업 중인 사본 노트북을 기준으로 프로젝트 전체 흐름을 설명합니다.

---

## 4. 분석 흐름

노트북은 보통 아래 순서로 진행됩니다.

1. 데이터 경로 탐색 및 압축 파일 확인
2. ADNI MRI/임상 데이터 병합
3. 기저시점 기준 선택
4. 정상 인지군(NCI) 기반 예측 모델 학습
5. 보정 전/후 BAG 산출
6. 진단군 비교 및 통계 검정
7. 인지 점수와의 연관성 검사
8. 해석 가능한 ROI 중요도 확인
9. 결과 시각화 및 저장

---

## 5. 핵심 개념

### BAG
Brain Age Gap는 다음과 같이 계산됩니다.

- BAG = 예측된 뇌 나이 - 실제 나이

양수이면 뇌가 실제 나이보다 더 늙어 보이는 상태로 해석될 수 있습니다.

### NCI 기준 모델
질병군을 학습에 넣지 않고 정상 인지군만 학습하므로, 뇌 노화의 정상 기준선을 확보하는 데 목적이 있습니다.

### FreeSurfer ROI
MRI에서 계산된 피질 두께, 부피, 표면 영역 등 구조적 지표를 이용합니다. 본 분석에서는 이러한 ROI들이 뇌 연령 예측의 입력으로 사용됩니다.

---

## 6. 실행 환경

권장 환경:

- Python 3.10 이상
- Jupyter Notebook / Google Colab
- pandas, numpy, scikit-learn, matplotlib, seaborn, statsmodels 등

필요한 패키지는 프로젝트 내 `인영/ADNI.V3/ADNI.V3/requirements.txt`를 참고할 수 있습니다.

로컬 환경에서 실행할 경우 아래처럼 준비합니다.

```bash
pip install -r 인영/ADNI.V3/ADNI.V3/requirements.txt
```

Colab 환경에서는 데이터 경로와 압축 해제 경로를 본인 환경에 맞게 수정한 뒤 실행하면 됩니다.

---

## 7. 실행 방법

1. 이 워크스페이스에 데이터 압축 파일이 준비되어 있는지 확인
2. 노트북 파일을 엽니다.
3. 데이터 경로 셀에서 `ZIP_PATH`, `DATA_ROOT`, `extract_root` 등을 본인 환경에 맞게 수정
4. 노트북을 순서대로 실행
5. 결과가 생성되면 그래프와 요약 테이블을 확인
6. 필요 시 CSV 결과를 저장해 보고서 작성에 사용

예시 경로 구조는 아래와 같습니다.

```python
ZIP_PATH = '/content/drive/MyDrive/.../ADNI_data_Do_NOT_redistribute (1).zip'
extract_root = Path('/content/adni_project_data')
```

설정값은 실제 데이터 위치에 맞춰 변경해야 합니다.

---

## 8. 결과물

실행 후 생성되는 결과물은 보통 다음과 같습니다.

- 환자/대상별 BAG 결과
- 연령 예측 그래프
- 진단군별 BAG 박스플롯
- 회귀/통계 결과 표
- 주요 ROI 중요도/영향도 시각화
- CSV 결과 파일

실험 결과를 보고서나 발표 자료로 사용하는 경우, 반드시 데이터 경로와 분석 조건을 함께 명시해야 합니다.

---

## 9. 주의사항

- 본 분석은 관찰연구 기반이며 인과관계를 입증하지 않습니다.
- BAG 값은 개인 진단 도구로 사용될 수 없습니다.
- ADNI 원본 데이터는 재배포가 금지되며, 개인 식별 정보가 포함된 파일은 외부에 공유하지 않아야 합니다.
- 모델은 정상 인지군을 기준으로 학습되므로, 질병군의 예측을 절대적인 임상 판단 기준으로 보아서는 안 됩니다.

---

## 10. 참고 문서

관련 상세 문서는 아래 폴더를 참고할 수 있습니다.

- `인영/ADNI.V3/ADNI.V3/README_KO.md`
- `인영/ADNI.V3/ADNI.V3/README.md`
- `인영/ADNI.V3/ADNI.V3/ANALYSIS_SUMMARY_KO.md`
- `인영/ADNI.V3/ADNI.V3/MODEL_CARD.md`

---

## 11. 요약

이 프로젝트는 ADNI MRI 데이터로부터 정상 인지군 기반 뇌 연령 예측 모델을 만들고, 그 차이를 통해 알츠하이머 관련 취약성의 경향을 정량적으로 탐색하는 연구용 노트북입니다. BAG를 이용한 집단 수준 비교와 뇌 영역 수준 해석은 질병 관련 뇌 노화 패턴을 이해하는 데 유용하지만, 임상적 의사결정에는 별도의 검증이 필요합니다.

필요한 경우 이 README를 프로젝트 발표용 문서 형태로 확장하거나, 노트북 실행 전 준비 단계와 데이터 경로 수정을 더 자세히 보강할 수 있습니다.
