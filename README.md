AI-Health-Bio-Data-Team-4-Professor-yoon-
ADNI Brain Age Gap: 편향 보정 Ridge 파이프라인 + 집단별 3D 시각화
`ADNI_Brain_Age_Gap_Ridge_BiasCorrected_3D_Colab.ipynb`는 ADNI ComBat FreeSurfer ROI 데이터로 뇌 연령(brain age) 을 예측하고, 실제 연령과의 차이인 BAG(Brain Age Gap) 를 계산한 뒤, 정상(CN)·경도인지장애(MCI)·알츠하이머병(AD) 집단 간 차이를 3D로 시각화하는 Google Colab 노트북입니다.
이 노트북이 구현하는 것
연령 편향 보정(bias correction) — brain-age 모델은 구조적으로 젊은 사람은 실제보다 늙게, 고령자는 실제보다 젊게 예측하는 경향(regression-to-the-mean)이 있습니다. CN 검증 집합에서 추정한 역선형 보정식 `predicted_age = a + b * chronological_age`으로 이를 교정합니다.
Index MRI만 사용 — RID(참가자)별 가장 이른 유효 T1 MRI 1건만 사용해, 동일 참가자의 반복 촬영이 훈련/검증/테스트 분할에 섞여 생기는 정보 누수를 제거합니다.
특징(feature) 구성 — 해마, 피질 두께/용적(Desikan-Killiany 68개 parcel), 뇌간, eTIV, 뇌실(가쪽·3·4뇌실), 기타 피질하 용적(편도체·시상·미상핵·조가비핵·창백핵 등) + 성별을 입력으로 사용합니다. (교육이수는 현재 소스 파일(FreeSurfer 구조 ROI)에 없어 자동 감지 코드만 포함하고 한계로 명시했습니다.)
모델 = Ridge 회귀(고정 선택) — ROI 간 상관관계가 높은 상황에서 안정적으로 계수를 추정하고, 해석·재현이 쉬운 Ridge를 사전에 정한 고정 모델로 사용합니다. ElasticNet은 성능 비교용으로만 함께 학습합니다.
BAG 계산 — `BAG = 보정된 예측 뇌연령 − 실제 연령`.
과적합 평가 — Train(in-sample) / 5-fold CV / Validation / 독립 Test MAE·R2 비교, learning curve, 정규화 강도(alpha) 곡선, 500회 순열검정(permutation test)으로 과적합 여부와 통계적 유의성을 점검합니다.
집단별(CN → MCI → AD) 3D 시각화 — 각 ROI를 CN 훈련 집합 기준 규준 모델(`ROI ~ age + sex + ICV`)로 정규화한 뒤 집단별 평균 노화 부담(Z-score)을 계산하고, 이를 두 가지 방식으로 3D 렌더링합니다.
Plotly(Mesh3d) — 좌우 피질 표면 + 해마·편도체·뇌실 등 피질하 구조를 회전 가능한 HTML로 렌더링.
PyVista/VTK(9-1절, 고급) — smooth shading, sulcal depth 음영, PBR 재질의 피질하 구슬(glyph)을 적용해 훨씬 입체적으로 보이는 CN/MCI/AD 3분할 비교 렌더링. 정지 이미지(PNG)·360도 회전 동영상(MP4) 저장 코드도 포함.
주요 결과 (노트북 내 재현됨)
독립 CN 테스트 대비 보정 BAG는 CN < MCI < AD 순서로 유의하게 증가합니다.
집단	보정 BAG 평균(년)
CN (테스트)	약 −1.4
MCI	약 +4.8
AD	약 +13.4
가장 부담이 큰 부위는 해마(Hippocampus) 용적 감소, 내후각피질(Entorhinal cortex)·측두엽(Temporal lobe) 피질 두께 감소, 가쪽뇌실(Lateral ventricle) 확장입니다.
실행 방법 (Google Colab)
`ADNI_Brain_Age_Gap_Ridge_BiasCorrected_3D_Colab.ipynb`를 Colab에서 엽니다.
`런타임 > 모두 실행`을 누릅니다.
실행 중 파일 업로드 창이 뜨면 `adni_personal_modeling_cohort.csv`, `left.aparc.annot`, `right.aparc.annot` 3개 파일을 함께 업로드합니다. (RID별 index MRI 1건만 남긴 ComBat ROI 데이터이며, 원본 식별정보는 포함하지 않습니다.)
필요한 패키지(`nilearn`, `nibabel`, `plotly`, `statsmodels`, `pyvista`, `trame` 등)는 노트북이 자동으로 설치합니다.
한계 및 주의
교육이수(education) 변수는 현재 입력 파일에 없어 이번 실행에는 포함되지 않았습니다. ADNIMERGE 인구통계 자료를 RID·방문일 기준으로 병합하면 코드 수정 없이 자동 반영됩니다.
피질하 구조 3D 글리프(구슬)는 실제 세그멘테이션 형상이 아닌, 부담 크기(Z-score)를 비교하기 위한 근사 위치 표식입니다.
본 파이프라인은 연구용 집단 수준 분석이며, 개별 환자의 확정적 임상 진단이나 치료 결정 도구로 사용해서는 안 됩니다.
