---
title: "[MLflow] MLflow의 개념"
excerpt: ""

categories:
  - prg-mlflow
tags:
  - mlops
  - mlflow

date: 2025-03-02
last_modified_at: 2025-03-02
---

## 1. MLflow 소개
MLFlow는 오픈소스 MLOps 플랫폼으로, ML 모델 개발, 실험 추적, 배포 및 관리를 지원한다. 즉, 모델 라이프사이클 전반을 다루는 도구이다.

<img width="500" alt="Image" src="https://github.com/user-attachments/assets/9bef355a-ae9e-40d9-947b-5caaf2e1249d" />


MLFlow는 크게 아래의 세가지 기능을 지원한다.

- **Tracking**: 실험의 하이퍼파라미터, 메트릭, 결과 등을 체계적으로 기록하고 관리하여 재현성을 높임
- **Project**: 코드, 환경 설정, 종속성을 표준화된 방식으로 패키징하여 재현 가능한 실행 환경을 제공함
- **Model**: 다양한 형식의 모델을 저장하고, 로드 및 배포할 수 있도록 지원하여 효율적인 모델 관리가 가능하게 함

## 2. MLflow의 주요 요소

한 가지씩 자세히 살펴보면 아래와 같다.

### a. Tracking

MLflow의 Tracking은 실험의 하이퍼파라미터, 메트릭, 결과 등을 체계적으로 기록하고 관리하여 재현성을 높이며, 아래의 구성 요소를 가진다.

- **Parameters**: 모델 학습에 사용된 하이퍼파라미터 값을 기록하여 실험을 비교하고 재현성을 확보함
- **Metrics**: 정확도, 손실 함수 값 등 모델 성능을 평가하는 지표를 저장하여 실험 결과를 분석할 수 있도록 함
- **Artifacts**: 모델 파일, 데이터셋, 시각화 결과 등 실험과 관련된 파일을 보존하여 관리할 수 있도록 함
- **Tags and Notes**: 실험에 대한 추가 정보를 태그와 메모로 기록하여 검색과 관리가 용이하도록 도움

MLflow를 Run하면 아래와 같이 실험의 고유한 Run ID가 생성된다.

<img width="400" alt="Image" src="https://github.com/user-attachments/assets/d1a33c0c-976a-4da0-87c9-4b5f0c4b8c6e" />

- **Parameters**: 각 실험에서 모델 학습에 사용된 하이퍼파라미터 값을 기록하여 실험을 비교하고 재현성을 확보한다.
<img width="400" alt="Image" src="https://github.com/user-attachments/assets/a1fb27e2-86d7-49ac-8848-0da342d7995f" />


- **Metrics**: 정확도, 손실 함수 값 등 모델 성능을 평가하는 지표를 저장하여 실험 결과를 분석할 수 있도록 한다.
<img width="400" alt="Image" src="https://github.com/user-attachments/assets/33272627-9f64-4dc0-a025-529878e05b1c" />

    아래와 같이 Metric을 Step 별로 추적할 수도 있다.

    <img width="400" alt="Image" src="https://github.com/user-attachments/assets/38886335-426e-40aa-bfce-6acc67f91291" />


- **Artifacts**: 실험에서 사용 및 출력된 임의의 파일을 업로드하여 관리할 수 있으며, 이를 통해 실험 간 특징 비교 분석이 가능하다.

    아래는 각 실험별 Artifacts에 저장된 PR Curve를 비교하는 이미지이다. 이미지 뿐만 아니라, 모델 가중치 저장 파일 등 실험과 관련된 다양한 파일을 저장할 수 있다.
    
    <img width="400" alt="Image" src="https://github.com/user-attachments/assets/bd1fa145-2469-491b-bbe7-8751b4c303e6" />


- **Tags and Notes**: 실험에 임의의 Tag를 달아두고 추후 원하는 실험을 필터링하여 찾아볼 수 있어, 실험 결과 해석에 유용하게 사용 가능하다. 

    아래 예시는 탐색 방법 별 Tag로 실험을 분류한 것이다.

    <img width="400" alt="Image" src="https://github.com/user-attachments/assets/4d359197-d750-4c04-8587-e229a860775f" />


Tag를 통해 실험을 필터링 하여, 파라미터와 메트릭 간의 상관관계를 실험 별로 분석할 수도 있다.

이전 예시에서 Random 탐색 방법과, Grid 탐색 방법으로 실험을 구분하였다. 아래 예시는 해당 탐색방법 별 초기 학습률(lr0)과 메트릭(mAP50-95) 간의 상관관계를 보여준다.

<img width="500" alt="Image" src="https://github.com/user-attachments/assets/531b5a98-d846-4f36-9b4b-7ffabc1a252e" />



### b. Projects

MLflow의 Projects는 코드, 환경 설정, 종속성을 표준화된 방식으로 패키징하여 재현 가능한 실행 환경을 제공한다.

<img width="600" alt="Image" src="https://github.com/user-attachments/assets/a968c483-d4e7-4496-8541-55af73f2c5bb" />

- mlflow.projects를 이용하면 다양한 환경(로컬, 클러스터, 클라우드 등)에서 손쉽게 실행이 가능하다.
- MLProject 파일이 존재하면, 예를 들어 이를 Github환경 또는 Docker 환경 등 다양한 환경에서 직접 실행 가능하다.


### c. Models

MLflow의 Models는 다양한 형식의 모델을 저장하고, 로드 및 배포할 수 있도록 지원하여 효율적인 모델 관리가 가능하게 한다.

<img width="500" alt="Image" src="https://github.com/user-attachments/assets/0f27013f-6672-4b0d-8f8d-d0a93a85753f" />

- 모델을 저장하면, 각 모델의 Unique ID와 함께 AWS S3, GCS 등 클라우드 스토리지에 저장할 수 있으며 추후 다시 로드하여 사용할 수 있음

모델을 불러올 때, PyFunc를 함께 사용할 수 있다. PyFunc은 다양한 프레임워크 모델을 하나의 공통 인터페이스로 제공하는 모듈이다.

<img width="500" alt="Image" src="https://github.com/user-attachments/assets/14244832-424f-4729-ab98-afd04154a020" />

- mlflow.pyfunc.load_model()을 사용하면 Scikit-learn, PyTorch, TensorFlow 등 다양한 모델을 동일한 방식으로 호출이 가능하다.



MLflow로 저장된 모델은 모델 스키마와 함께 저장되며, 모델을 로드할 수 있는 가이드 코드가 함께 보여진다.

<img width="600" alt="Image" src="https://github.com/user-attachments/assets/22972914-272d-4959-9a27-eab46b599f12" />

- 앞서 소개한 것처럼, 외부 저장소에 저장된 모델을 pyfunc.load_model()을 이용하여 불러옴으로써 프레임워크에 상관 없이 동일한 인터페이스를 통해 예측을 수행할 수 있도록 패키징됨


## 3. MLflow 추가 기능
MLflow를 통해 모델을 저장하였다면, 이를 도커화 시켜서 추론을 진행할 수 있다.

<img width="600" alt="Image" src="https://github.com/user-attachments/assets/62d5a661-b5bd-4a31-8728-a64e1a9e64ea" />

- 만약 AWS S3 클라우드 스토리지 등을 연동하여 모델을 저장했다면, 위 코드처럼 해당 스토리지에서 저장된 모델을 불러와 도커 이미지를 빌드할 수 있다.

<img width="600" alt="Image" src="https://github.com/user-attachments/assets/b237b4f3-9dea-4543-bc5a-b6e094c32d05" />

- 빌드된 Docker 파일 내에, 위 코드처럼 Miniconda를 통힌 환경 활성화 및 Gunicorn 통한 서버 실행 코드를 추가하는 커스터마이즈를한 뒤 도커 파일을 실행한다면, 쉽게 모델을 서버에 배포할 수 있다.



아래는 ’콴다(QANDA)’ 서비스를 제공하는 ‘매스프레소(MATHPRESSO)’사의 MLflow 활용 배포 파이프라인 예시이다. 기존에 Elastic Beanstalk 기반 배포 파이프라인을 가지고 있었기 때문에, 이에 통합하는 방식을 취하였다고 한다.

<img width="500" alt="Image" src="https://github.com/user-attachments/assets/ad094cba-7e2e-40f6-943f-949686e5f10b" />

**배포 세부 프로세스**:

1. MLflow와 Github를 연동하여, 연구자들이 본인의 실험 내용 및 결과를 Push하면 이를 패키징하여 Tracking 함
2. Github에서 Run ID, 모델 저장 경로 등을 담은 특정 Tag를 Jenkins에 반환함
3. Jenkins가 Tag를 기반으로 저장된 모델을 당겨옴
4. 당겨온 모델을 기반으로 ECR에서 도커 컨테이너를 빌드함
5. 빌드한 도커를 로드 밸런싱 또는 버전 관리 등의 기능을 제공하는 Elastic Beanstalk을 통해 배포함
6. 배포가 끝나면, Jenkins가 배포 기록들을 MLflow에 기록함
    - 배포된 버전을 기록
    - 각 실험마다 빌드된 ECR 이미지를 Tag로 기록 등



---

[다음 글]()에서는 MLflow를 이용하여 모델의 전체 라이프사이클을 분석하는 간단한 실습을 확인할 수 있다.