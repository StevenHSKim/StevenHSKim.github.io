---
title: "[아나콘다] 가상환경 복제 및 이동"
excerpt: ""

categories:
  - prg-anaconda
tags:
  - programming
  - anaconda

date: 2024-08-17
last_modified_at: 2024-08-17
---

\* 서버 A → 서버 B 의 상황이라고 가정하겠다.

## 1. [서버 A] 옮기고자 하는 가상 환경 실행
```bash
conda activate [가상환경 이름]
```


## 2-(1). [서버 A] 가상 환경 추출
- environment.yml 이라는 파일이 동일 폴더에 생성된다. 하지만 지저분하다.
```bash
conda env export > environment.yml
```

## 2-(2). [서버 A] 가상 환경 추출 (더 정돈된 ver.)
- 조금 더 정돈된 environment.yml 파일이 동일 폴더에 생성된다.
```bash
conda env export --no-builds | grep -v “prefix” > environment.yml
```
<p align="center"><img src="https://github.com/user-attachments/assets/32dfb265-75fb-4b4f-bdc1-9419c276bd62" width="400"></p>

## 3. environment.yaml 전송
- 환경을 옮기고 싶은 서버(서버B)로 environment.yml를 전송한다.
```bash
scp -P [포트 번호] [서버A username]@[서버A IP 주소]:[서버A에서 보낼 파일 경로] [서버B username]@[서버B IP 주소]:[서버B에서 받을 폴더 경로]
```
\* 자세한 파일/폴더 전송 명령어 정보는 [여기](https://stevenhskim.github.io/prg-linux/linux_scp/) 참고


## 4. [서버 B] environment.yml의 name과 prefix 수정
- 변경하고 싶은 가상환경의 이름(name)과 가상환경의 경로(prefix)로  environment.yml 파일을 수정한다.

<p align="center"><img src="https://github.com/user-attachments/assets/4543bedd-ff1e-4647-bd28-8f37e85f0a26" width="600"></p>

<p align="center"><img src="https://github.com/user-attachments/assets/d1e0cfae-0a0d-423a-9f23-c2aae6cda436" width="600"></p>


## 5. [서버 B] 환경 생성
- 최종적으로 아래의 명령어로 서버 B에 환경을 생성한다.
```bash
conda env create --file environment.yaml
```