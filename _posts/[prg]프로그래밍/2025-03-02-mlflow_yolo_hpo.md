---
title: "[MLOps][MLflow] MLflow를 활용한 YOLOv11-nano 학습 과정 추적"
excerpt: "간단한 MLflow 추적 실습"

categories:
  - prg-mlops
tags:
  - mlops
  - mlflow

date: 2025-03-02
last_modified_at: 2025-03-02
---

[MLflow 소개 글](https://stevenhskim.github.io/prg-mlflow/mlflow_intro/)에 이어서 MLflow를 활용한 간단한 실습에 대한 글이다. 해당 글에 소개된 실험에 대한 소스코드는 [여기](https://github.com/StevenHSKim/MLFlow-study)에서 확인할 수 있다.


## 1. 실험 설정
해당 실습에 대한 실험 세팅은 아래와 같다.

**실험 목표**: MLflow를 이용하여 모델의 전체 라이프사이클을 분석한다.

**실험 세부 과정**:
  1. COCO 데이터셋으로 사전학습된 YOLO v11 nano 모델을 Pascal VOC 데이터셋으로 파인튜닝한다.
  2. 파인튜닝 과정에서, Optuna의 세가지 탐색 방법론을 이용하여 하이퍼파라미터 최적화(HPO)를 진행한다.
  3. HPO의 최적 결과 모델을 추후 배포 가능한 형태로 저장한다.
  4. 각 실험 내용 및 결과를 MLflow를 통해 비교 분석한다.

**실험 세팅 정리**
<p align="center"><img width="400" alt="Image" src="https://github.com/user-attachments/assets/5cae0d6b-a9af-4805-aa8e-1eebd83def9b" /></p>



## 2. 실험 내용

### 모델 로드 및 HPO 관련 코드

<p align="center"><img width="600" alt="Image" src="https://github.com/user-attachments/assets/92456c0a-ddcb-461f-876a-c21efd920e19" /></p>

- COCO 데이터셋으로 사전학습된 YOLO v11 nano를 로드한다.


<p align="center"><img width="600" alt="Image" src="https://github.com/user-attachments/assets/2d06565a-db66-48d9-a78b-c582528242a1" /></p>

- Optuna에서 세 가지 탐색 방법(TPE, Random, Grid)을 설정한다.


### 학습 및 검증 시 MLflow 모델 서빙 관련 코드

<p align="center"><img width="600" alt="Image" src="https://github.com/user-attachments/assets/b8461eea-76e2-441d-bc3e-786322006bb8" /></p>

- MLflow가 실험 결과(런, 파라미터, 메트릭, 아티팩트 등)를 저장할 위치를 지정한다.
- 현재 실행될 실험(Experiment)의 이름을 지정한다.

<p align="center"><img width="600" alt="Image" src="https://github.com/user-attachments/assets/ac8209f2-6d5b-4deb-91f4-4e41b559f977" /></p>

박스 순서대로
- MLflow 런(run)을 시작하는 컨텍스트 매니저이다.
- 실행 중인 런에 태그를 설정한다.
- 학습에 사용된 하이퍼파라미터와 설정값들을 MLflow에 기록한다.
- 검증 후에 산출된 성능 지표를 기록한다.


### 학습 및 검증 후 MLflow 모델 저장 관련 코드

<p align="center"><img width="600" alt="Image" src="https://github.com/user-attachments/assets/6c1921f9-d6a8-462c-8187-92bd065d8d05" /></p>

박스 순서대로
- log_model을 통해 pytorch 모델을 저장한다.
- ONNX와 TorchScript 포맷으로 모델을 export하여 각각의 파일을 MLflow에 아티팩트로 등록한다.


## 3. 실험 결과
실험은 Optuna의 세 가지 탐색 방법론에 대해서, 각각 5번의 최적화 시도를 가졌다. 따라서 총 15번의 실험이 진행되었다.

<p align="center"><img width="600" alt="Image" src="https://github.com/user-attachments/assets/96098f05-f905-4843-a40e-3590b963afb6" /></p>

아래 그림은 전체 15번의 실험의 Tag 그룹 별 메트릭 값 시각화이다.

<p align="center"><img width="600" alt="Image" src="https://github.com/user-attachments/assets/a408e165-e79c-4467-bc92-670c4a37cc8a" /></p>


<br>

Parallel Coordinates Plot을 통해 임의의 메트릭-하이퍼파라미터 조합이 어떻게 영향을 미치는지 시각적으로 분석할 수 있다.

<p align="center"><img width="600" alt="Image" src="https://github.com/user-attachments/assets/b0f1d68f-9fc9-4f4e-8b4f-7d7846626e58" /></p>

- 하이퍼파라미터 조합(Batch size, Initial learning rate, momentum)에 따른 메트릭(mAP50-95) 값을 나타낸다.
- Parallel Coordinates Plot 이외에도, Scatter Plot, Box Plot, Contour Plot 등으로 시각화 가능하다.


Scatter Plot으로는 실험별 특정 하이퍼파라미터와 특정 메트릭 간의 상관관계를 파악할 수 있다.

<p align="center"><img width="800" alt="Image" src="https://github.com/user-attachments/assets/f4f0d663-3dc7-4c79-bcb1-4d49e9d61579" /></p>

- 위 Scatter Plot 예시는 Random 탐색과 Grid 탐색의 각 다섯 번의 실험에서 Batch size와 mAP50-95 간의 상관관계를 보여준다.
- 나아가, Box Plot으로는 하이퍼파라미터별 성능의 중앙값 및 이상치를 확인할 수 있으며, Contour Plot으로는 다차원 공간에서 성능이 최적화되는 영역을 탐색할 수 있다.


Visualization 뿐만 아니라, Run details, Parameters, Metric, Tags 등의 실험 별 값을 표 형식으로 볼 수도 있다.

<p align="center"><img width="600" alt="Image" src="https://github.com/user-attachments/assets/a4197827-96c4-4202-8d2b-6e3b7851bc2b" /></p>

- 노란색 하이라이트는 실험이 진행됨에 따라 변동이 된 항목을 나타낸다.


이러한 다양한 도구를 통해 최적의 하이퍼파라미터 조합을 분석하고 모델 개선 방향을 효율적으로 도출할 수 있다.

---

모델을 저장했다면 아래의 예시처럼 모델이 저장됨을 알 수 있다.

<p align="center"><img width="800" alt="Image" src="https://github.com/user-attachments/assets/9a783f22-3819-467b-a7a2-90dd911fb276" /></p>

- MLflow를 통해 모델을 저장하면 해당 모델이 패키징되어 의존성과 함께 관리되므로, 일관된 환경에서 불러와 활용할 수 있다.
- 또한 저장된 위치를 기반으로 불러오는 예시 코드를 함께 제공하여 쉬운 배포와 재현이 가능하다.

---

추가로, MLflow를 통해 모델을 저장하였다면 이를 도커화 시켜서 추론을 진행할 수 있다.

<p align="center"><img width="600" alt="Image" src="https://github.com/user-attachments/assets/62d5a661-b5bd-4a31-8728-a64e1a9e64ea" /></p>

- 만약 AWS S3 클라우드 스토리지 등을 연동하여 모델을 저장했다면, 위 코드처럼 해당 스토리지에서 저장된 모델을 불러와 도커 이미지를 빌드할 수 있다.

<p align="center"><img width="600" alt="Image" src="https://github.com/user-attachments/assets/b237b4f3-9dea-4543-bc5a-b6e094c32d05" /></p>

- 빌드된 Docker 파일 내에, 위 코드처럼 Miniconda를 통힌 환경 활성화 및 Gunicorn 통한 서버 실행 코드를 추가하는 커스터마이즈를한 뒤 도커 파일을 실행한다면, 쉽게 모델을 서버에 배포할 수 있다.