# 직접 만들어 쓰는 도구

[← 4cut story 소개로 돌아가기](https://github.com/4cut-story)

## Crowfoot

무료 온라인 ERD 툴: 설계부터 DB 생성까지. 설치 없이 브라우저에서 표(테이블)와 관계를 그리는 ERD 도구예요. 그림만 그리고 끝나지 않고, 그린 설계를 실제 데이터베이스까지 만들어 줘요.

- 여러 사람이 같은 ERD를 실시간으로 함께 고쳐요
- 이미 있는 DB를 연결하면 ERD를 자동으로 그려 줘요
- 설계에서 DDL과 마이그레이션 SQL을 만들고 DB에 바로 반영해요
- 무료 MySQL·PostgreSQL 데이터베이스를 바로 발급해요
- 요구사항을 등록하고 표마다 근거가 된 요구사항을 이어 둬요
- pgvector 벡터 컬럼과 HNSW 색인까지 설계할 수 있어요
- MCP 서버가 있어 AI 에이전트가 요구사항과 ERD를 읽고 고칠 수 있어요

- **기술** React · React Flow · Spring Cloud Gateway · Spring Boot 마이크로서비스 · WebSocket(STOMP) 협업 · Spring AI MCP · Apache-2.0
- **이 프로젝트에서**
  - 이 서비스의 PostgreSQL 설계를 Crowfoot 문서 하나로 했어요: 표 14개, 요구사항 16개
  - AI 코딩 에이전트가 Crowfoot MCP로 요구사항을 등록하고 ERD를 고쳤어요
  - 완성한 설계를 DDL로 뽑아 서비스마다 Flyway 첫 마이그레이션(V1)으로 나눴어요
  - 개발용 DB는 Crowfoot이 발급한 PostgreSQL을 썼어요
- [crowfoot.java21.net](https://crowfoot.java21.net) · [GitHub](https://github.com/crowfoot-erd)

## nginx-manager

nginx와 관리 화면을 컨테이너 두 개로 띄우는 웹 nginx 관리 도구. nginx 설정 파일을 손으로 고치지 않고 화면의 폼으로 사이트를 만들어요. 도메인을 적고 HTTPS를 고르면 인증서 발급부터 갱신까지 알아서 해요.

- Let's Encrypt 인증서를 자동으로 받고 만료 전에 갱신해요. 실패하면 메일·웹훅으로 알려요
- 부하 분산, 웹소켓, 타임아웃, 리디렉션, HTTP/3를 폼에서 정하고 만들어질 설정을 옆에서 바로 봐요
- 설정 검사·다시 읽기·헬스 체크 중 하나라도 실패하면 직전 설정으로 자동 롤백해요
- 사이트별 접속·오류 로그, 점검 모드, 2단계 인증, 권한과 감사 기록
- 명령 한 줄로 설치하고 4개 언어와 다크 모드를 지원해요

- **기술** Spring Boot 관리 화면 · Go 에이전트 · nginx 1.31 · Docker 컨테이너 2개
- **이 프로젝트에서**
  - 이 서비스의 도메인 연결을 nginx-manager로 했어요
  - HTTPS 인증서 발급과 갱신을 nginx-manager가 맡아요
- [GitHub](https://github.com/nginx-manager)
