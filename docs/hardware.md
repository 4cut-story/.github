# 하드웨어 환경

[← 4cut story 소개로 돌아가기](https://github.com/4cut-story)

유료 GPU 서버 없이 **소비자용 GPU 한 장**으로 돌려요. 세 모델 묶음을 한꺼번에 올리면 16GB를 넘기 때문에, 단계마다 한 묶음만 올리고 내려요.

**GPU PC**

- NVIDIA RTX 5060 Ti 16GB (Blackwell 세대, CUDA 12.8용 PyTorch)
- PC 메모리 32GB, Windows
- Python 작업 프로그램(워커) + Ollama(로컬 LLM 실행기)
- 밖에서 들어오는 연결은 막고, 워커가 HTTPS로 일을 가져와요

**쿠버네티스 클러스터**

- 서버 쿠버네티스 클러스터 (Rancher)
- Argo CD가 배포 레포를 보고 자동으로 맞춰요
- 서비스끼리 정해진 방향으로만 통신 (NetworkPolicy), 외부 공개는 web 하나
- PostgreSQL + pgvector(좌표 검색), MinIO(파일 저장소)

VRAM 16GB 배분

| 모델 묶음 | VRAM | 내용 |
| --- | :---: | --- |
| 받아 적기 묶음 | 약 7GB | WhisperX large-v3 + 시각 정렬 모델 + pyannote |
| LLM 묶음 | 약 7GB | Qwen3 8B 4비트 (대체 경로일 때만) |
| 그림 묶음 | 약 7GB | FLUX.2 klein 4B 4비트 + 캐릭터 참조 그림, 최대 약 3.7GB (실패하면 DreamShaper XL Lightning + IP-Adapter, 약 11GB) |

16GB 한 장으로 운영하며 내린 판단

| | |
| --- | --- |
| **GPU에 남긴 것** | 받아 적기(WhisperX + pyannote)와 그림(FLUX.2 klein 4B, 실패하면 SDXL + IP-Adapter): 녹음과 그림을 밖으로 보내지 않고 직접 돌려요 |
| **밖으로 뺀 것** | 콘티 LLM은 클라우드(Claude, Gemini)를 먼저 써서 GPU를 비워 둬요. Qwen3는 대체가 필요할 때만 올려요<br>임베딩(BGE-M3)은 별도 임베딩 서버에서 돌려요<br>내린 모델은 PC 메모리(32GB)에 보관해 GPU 메모리를 늘린 것처럼 써요 |
| **내려놓은 것** | 1024px 대신 768px, 30단계 대신 4단계<br>LLM은 14B 대신 8B<br>여러 작업 동시 처리 대신 한 번에 한 작업 |
| **얻은 것** | 캐릭터를 참조한 컷 한 장 약 5.4초(GPU 약 3.7GB), SDXL 모델 전환 1.2~1.4초<br>GPU 한 장으로 받아 적기부터 그림까지 전 과정 운영 |
