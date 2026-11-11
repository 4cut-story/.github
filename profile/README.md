<div align="center">

# 4cut story

**대화를 녹음하면 AI가 4컷 만화로 그려 주는 서비스**

녹음 대신 뉴스 기사 주소를 넣거나 RSS 피드를 구독해 두어도 4컷이 만들어져요.

[![Demo](https://img.shields.io/badge/demo-4cut.java21.net-ff6b6b?style=flat-square)](https://4cut.java21.net)
[![About](https://img.shields.io/badge/about-소개-4c6ef5?style=flat-square)](https://4cut.java21.net/about)
![Next.js](https://img.shields.io/badge/Next.js-16-000000?style=flat-square&logo=nextdotjs&logoColor=white)
![Spring Boot](https://img.shields.io/badge/Spring_Boot-4-6DB33F?style=flat-square&logo=springboot&logoColor=white)
![Java](https://img.shields.io/badge/Java-21-ED8B00?style=flat-square&logo=openjdk&logoColor=white)
![PyTorch](https://img.shields.io/badge/PyTorch-CUDA_12.8-EE4C2C?style=flat-square&logo=pytorch&logoColor=white)
![Kubernetes](https://img.shields.io/badge/Kubernetes-Argo_CD-326CE5?style=flat-square&logo=kubernetes&logoColor=white)

[데모](https://4cut.java21.net) · [소개](https://4cut.java21.net/about) · [아키텍처](https://github.com/4cut-story/.github/blob/main/docs/architecture.md) · [AI 모델](https://github.com/4cut-story/.github/blob/main/docs/models.md) · [개선 기록](https://github.com/4cut-story/.github/blob/main/docs/improvements.md)

</div>

---

## ✨ 주요 기능

- 🎙️ **녹음 한 번으로 만화** 휴대폰 브라우저로 녹음하면 받아 적고, 웃음·반전·진심이 담긴 순간을 골라 4~10컷으로 그려요
- 📰 **기사·RSS로 만화** 기사 속 사실만 가져와 새 대화로 각색하고, 구독한 피드는 새 기사가 올라올 때마다 저절로 만들어요
- 🧑‍🎨 **같은 인물, 같은 얼굴** 캐릭터 1,013명(참조 그림 42,546장) 중 어울리는 인물을 골라 모든 컷에서 같은 모습으로 그려요
- 🎨 **테마와 사투리** 그림 테마 7가지, 사투리에 맞춘 대사, 말풍선까지 넣은 한 장으로 완성해요
- 🔒 **개인정보 보호** 실명은 가명으로 바꾸고, 원본 녹음은 받아 적자마자 지워요

## 📊 한눈에 보기

| 서비스 + GPU 워커 | 단계별 AI 모델 | 컷 한 장 그리기 | 그림 품질 점수 |
| :---: | :---: | :---: | :---: |
| **5 + 1** | **11개** | **약 5.4초** | **81.4 → 97.5점** |

- 🖥️ **GPU 한 장으로** 모델 세 묶음을 RTX 5060 Ti 16GB 한 장에 나눠 올리고, 모델 전환은 1.2~1.4초
- 🔌 **포트 없는 GPU 연결** GPU PC 워커가 HTTPS로 일을 가져가는(pull) 구조라 PC가 꺼져 있어도 일은 대기열에 남아요
- 🛟 **멈추지 않는 LLM** Claude → Gemini → 로컬 Qwen3 8B 순서로 대체해요

## 🏗️ 아키텍처

<p align="center"><img src="images/architecture.png" alt="전체 아키텍처" width="100%"></p>

밖에 열린 것은 **web 하나**이고, 그 뒤에서 서비스 다섯 개가 역할을 나눠 맡아요. GPU가 필요한 일은 PostgreSQL 대기열(`SKIP LOCKED`)에 쌓이고 GPU PC 워커가 가져가 처리해요. 배포는 GitHub Actions → GHCR → Argo CD로 자동으로 이어져요.

👉 서비스별 역할, 통신 방식, 로그인·배포 흐름은 [아키텍처 문서](https://github.com/4cut-story/.github/blob/main/docs/architecture.md)에 있어요.

## 🧰 기술 스택

| 분야 | 사용한 것 |
| --- | --- |
| **웹** | Next.js 16 · React 19 · TypeScript · Tailwind CSS v4 |
| **백엔드** | Java 21 · Spring Boot 4 · Spring Cloud Gateway · Spring Modulith · Spring AI |
| **AI 실행** | Python · PyTorch · diffusers · WhisperX · Ollama |
| **AI 모델** | WhisperX · Claude · Gemini · Qwen3 · BGE-M3 · FLUX.2 klein |
| **데이터** | PostgreSQL · pgvector · Flyway · MinIO |
| **운영** | Kubernetes · Argo CD · GitHub Actions · GHCR |

## 📂 저장소

| 저장소 | 설명 |
| --- | --- |
| [**web**](https://github.com/4cut-story/web) | 화면과 BFF. 외부에 열린 유일한 서비스 |
| [**api-gateway**](https://github.com/4cut-story/api-gateway) | 백엔드 입구, JWT 확인, 워커 전용 경로 |
| [**auth-service**](https://github.com/4cut-story/auth-service) | GitHub·Google 로그인, 토큰, 개인 액세스 토큰 |
| [**conversation-service**](https://github.com/4cut-story/conversation-service) | 녹음부터 4컷 완성까지 지휘, GPU 작업 대기열, LLM 대체 |
| [**content-service**](https://github.com/4cut-story/content-service) | 내 스토리, 게시, 피드, 좋아요, 댓글, 팔로우 |
| [**model-runtime**](https://github.com/4cut-story/model-runtime) | GPU PC 워커, 그림 모델 비교 기록 |
| [**common**](https://github.com/4cut-story/common) | OpenAPI 계약과 공통 코드 |
| [**deploy**](https://github.com/4cut-story/deploy) | 쿠버네티스 배포 설정 (Argo CD) |
| [**docs**](https://github.com/4cut-story/docs) | 설계 문서와 개선 기록 |

## 📚 자세한 문서

| 문서 | 내용 |
| --- | --- |
| [아키텍처](https://github.com/4cut-story/.github/blob/main/docs/architecture.md) | 서비스별 역할, 통신 방식, 저장소, GPU 작업·로그인·배포 흐름 |
| [생성 흐름](https://github.com/4cut-story/.github/blob/main/docs/pipeline.md) | 녹음·기사에서 4컷까지 단계별 규칙, 캐릭터 찾기, 배경음, 가상 인물 목소리 |
| [AI 모델](https://github.com/4cut-story/.github/blob/main/docs/models.md) | 단계별 모델과 설정값, 그림 모델 비교, 콘티 틀과 2단 그리기 |
| [하드웨어](https://github.com/4cut-story/.github/blob/main/docs/hardware.md) | GPU PC와 쿠버네티스 클러스터 구성 |
| [설계 결정](https://github.com/4cut-story/.github/blob/main/docs/design-decisions.md) | 고른 것과 버린 대안, 그 이유 |
| [개선 기록](https://github.com/4cut-story/.github/blob/main/docs/improvements.md) | 성능 개선과 문제 해결 기록 |
| [직접 만든 도구](https://github.com/4cut-story/.github/blob/main/docs/tools.md) | Crowfoot(ERD), nginx-manager |
| [용어 풀이](https://github.com/4cut-story/.github/blob/main/docs/glossary.md) | 문서에 나오는 용어 설명 |

---

<div align="center">

**녹음 한 번이 4컷 만화가 되는 서비스** · [4cut.java21.net](https://4cut.java21.net)

</div>
