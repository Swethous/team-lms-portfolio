# Team-LMS Portfolio

> (공공 SI) **관리자/학생/교수 LMS 플랫폼** 팀 프로젝트 포트폴리오입니다.  
> 원본 코드는 **팀 Org 레포**에 있으며, 이 레포는 **데모/문서/제 기여도**를 중심으로 정리한 허브입니다.

## Service (Live)
- https://teamlms.duckdns.org
- 👉 [Demo Accounts](#demo-accounts)

---

## Links
- Org Repository (Source): https://github.com/team-lms-2026-1/LMS-project
- Portfolio Repo (this): https://github.com/Swethous/team-lms-portfolio
- Figma (prototype, view-only): https://www.figma.com/proto/RxRjps2RteNVlH6J0vTxx9/Team-LMS-FIGMA-?node-id=17-20&t=CECAPJa6TQnkEFcM-1
- Use Case Diagram (draw.io): https://drive.google.com/file/d/1vGpn3qZlbVyoDKWs38B_-1B4RwdgZc89/view?usp=sharing

---

## Demo Video
[![Demo Video](assets/demo-thumbnail.png)](https://youtu.be/QJM7d27KuBM)

---

## Screenshots
| Auditing Logs (Index) | Auditing Logs (Detail) | MBTI AI Advisor (Result) |
|---|---|---|
| ![Logs](assets/Log-1.png) | ![Logs](assets/Log-2.png) | ![AI](assets/AiAdvisor-1.png) |

| Extra-curricular (Offering) | Extra-curricular (Session) | Extra-curricular (Grade) |
|---|---|---|
| ![Extra1](assets/ExtraCurricular-1.png) | ![Extra2](assets/ExtraCurricular-2.png) | ![Extra3](assets/ExtraCurricular-3.png) |

---

## My Role / Contribution
### PM / Project Operations
- 일정 관리 및 GitHub 기반 협업(Issues/PR) 운영 리딩
- CI/CD 파이프라인 구축 및 운영(빌드/테스트/배포 자동화)
- 브랜치 전략/PR 템플릿/리뷰 기준 수립으로 협업 프로세스 표준화
- API/설계 문서 체계화 (`docs/api` 구조 정리 및 도메인별 스펙 관리)

### Architecture / Data Design
- ERD 설계 주도 및 도메인 경계/관계 정의
- 공공 SI 관점의 상태값/삭제정책/감사·로그(Audit) 기준 수립

### Platform Standards (Backend / Frontend)
- 백엔드 공통 기반: 인증/로그인, RBAC, BaseEntity(감사 컬럼), audit 로깅
- 프론트 공통 기반: Next.js 구조 표준화, BFF/API 레이어, 공용 컴포넌트/클라이언트 세팅
- 다국어(i18n) 표준: UI + API(`Accept-Language`) + DB i18n 모델 + 에러 메시지 정책
- 공통 API 규격: 응답 포맷(`data/meta`), 에러 코드, 페이징 규칙 정의

### Owned Domains (Design & Implementation)
- 권한관리 / 시스템관리 / 교과관리 / 비교과관리 / MBTI AI Advisor 도메인 설계 및 구현

---

## Team & Collaboration
- 팀 규모: 5명 (역할: PM / Backend / Frontend)
- 협업 방식: GitHub Issues + PR 리뷰, feature 브랜치 기반 개발, 리뷰 체크리스트 운영
- 제 책임 범위: PM + 플랫폼 표준화 + 주요 도메인 오너십
- 문서화: `docs/api`에 **도메인별 API 스펙 템플릿/작성 규칙을 정리하고 업데이트를 리딩**

---

## Background (Public-SI context)
- 목표: 역할 기반 접근(ADMIN/PROFESSOR/STUDENT)과 감사(Audit) 중심 운영이 가능한 LMS 구축
- 강조점: 상태값/로그/다국어/API 규격 등 **예측 가능한 표준화**로 유지보수성과 확장성 확보
- 데모 범위: Security+i18n, Auditing Logs, 비교과 Workflow, MBTI AI Advisor

---

## Architecture
![Runtime Architecture](assets/architecture/runtime.png)

런타임 흐름: **Nginx → Next.js(BFF) → Spring Boot → RDS/S3/OpenAI**
- Nginx에서 SSL 종료(443) 후 내부 서비스로 reverse proxy
- Next.js는 UI + BFF(Route Handlers)로 서버 간 통신을 단순화
- Spring Boot는 `@PreAuthorize` 기반 권한 검증 및 audit/logging 수행
- RDS는 운영 DB, S3는 파일 저장, OpenAI는 AI Advisor에 사용

---

## Tech Stack

### Backend
| 기술 | 버전 | 채용 이유 |
|---|---:|---|
| Java | 17 | LTS 기반 안정적인 서버 런타임 |
| Spring Boot | 3.5.9 | 표준 백엔드 프레임워크 |
| Spring Data JPA | - | JpaRepository + 파생 메서드 + `@Query` 중심 조회/CRUD |
| Querydsl | - | 복잡한 검색/동적 조건 조회에 선택 적용 |
| Spring Security + JWT | - | Stateless 인증 구성 |
| RBAC (2계층 권한) | - | URL Role + Method Authority 이중 접근 제어 |
| PostgreSQL | - | 운영 안정성 높은 RDB |
| Flyway + ddl validate | - | 마이그레이션/스키마 검증 기반 배포 안정성 |
| AWS S3 (Presigned URL) | - | 파일 업로드/다운로드 처리 |
| Spring AI + OpenAI | gpt-4o-mini | MBTI 추천 AI (schema 제약 + fallback 적용) |
| OpenAPI (Swagger UI) | - | API 문서화/테스트, 협업 커뮤니케이션 향상

### Frontend / BFF
| 기술 | 버전 | 채용 이유 |
|---|---:|---|
| Next.js (BFF) | 14.2.5 | BFF 계층으로 API 집계/인증 처리 |
| HttpOnly Cookie Auth | - | 토큰 노출 최소화(클라이언트 저장소 회피) |
| next-intl | - | 다국어 지원(ko/en/ja) |
| Chart.js / Recharts | - | 대시보드/통계 시각화 |

### Infrastructure / DevOps
| 기술 | 버전 | 채용 이유 |
|---|---:|---|
| AWS EC2 | - | 서비스 호스팅(단일 서버) |
| Nginx + Let's Encrypt | - | HTTPS(SSL 종료) + Reverse Proxy |
| AWS RDS (PostgreSQL) | - | 운영 DB |
| AWS S3 | - | 파일 저장(Presigned URL) |
| Docker | - | 컨테이너 기반 실행/배포 |
| Docker Compose | - | 로컬 개발환경 재현 |
| GitHub Actions | - | 배포 자동화(Backend 빌드 중심, 테스트 스킵/프론트 CI 제한) |

---

## Project Scope (Team)
- Auth & RBAC
- Admin: Account / Department / Semester administration
- Curricular
- Extra-curricular
- Mentoring
- Community: Notice / Resources / FAQ / QnA
- Surveys
- Competency diagnosis & dashboard
- MBTI & AI Advisor
- Study space rental
- Notifications (Alarm)
- MyPage
- System status & log export

> Demo video focus: Security+i18n → Auditing Logs → Extra-curricular Workflow → MBTI AI Advisor

---

## Problem Solving / Technical Highlights (My Contribution)

- **Presigned URL로 비교과 동영상 업로드 병목 해결**
  - 서버 중계 업로드 대신 S3 Presigned URL을 발급해 브라우저가 직접 업로드하도록 전환했습니다.
  - `IN_PROGRESS` 상태에서만 업로드를 허용하고, `video/mp4` allowlist + 만료시간(5분) 정책을 적용해 보안/운영 안정성을 확보했습니다.

- **DB 레벨 i18n 설계로 MBTI 품질 개선**
  - 단순 번역 파일이 아니라 MBTI 핵심 도메인을 i18n 테이블로 분리했습니다.
  - `mbti_question_i18n`, `mbti_choice_i18n`, `interest_keyword_master_i18n`, `job_catalog_i18n` 구조에 `(원본ID, locale)` 유니크 제약/인덱스를 적용해 조회 일관성과 확장성을 확보했습니다.

- **MBTI 전 구간 다국어 일관성 확보 (UI → BFF → Backend)**
  - 프론트 locale(ko/en/ja)을 BFF와 백엔드로 전달하고, 질문/선택지/키워드/직업명/추천 사유까지 동일 언어로 응답하도록 통합했습니다.
  - 데이터 누락 시 기본 언어 fallback을 적용해 응답 공백을 방지했습니다.

- **AI 추천 안정성 강화: Structured Output + Retry + Fallback**
  - LLM 결과를 JSON 스키마(정확히 5개 추천, 중복 금지, 후보군 외 코드 금지)로 강제하고 파싱/검증을 수행했습니다.
  - 실패 시 재시도 후에도 불가하면 템플릿 기반 fallback 추천으로 전환해 장애 상황에서도 기능이 끊기지 않게 설계했습니다.

- **비교과 상태 전이 규칙화로 운영 오류 방지**
  - `DRAFT → OPEN → ENROLLMENT_CLOSED → IN_PROGRESS → COMPLETED` 단방향 전이만 허용하고, 잘못된 전이는 도메인 예외로 차단했습니다.
  - 완료 상태 잠금/세션 상태 제약까지 적용해 관리자 실수를 시스템적으로 방어했습니다.

- **비교과 완료 시점 정합성 자동 검증/확정**
  - `COMPLETED` 전환 시 회차 포인트/인정시간 합계가 운영 상한과 일치하는지 검증했습니다.
  - 역량 매핑 완성 조건 확인 후 수강생 `PASSED/FAILED`를 자동 확정하도록 구현했습니다.

- **인증/권한 복잡도 축소: BFF + 2계층 RBAC**
  - Next.js BFF로 API 호출을 집약하고 HttpOnly Cookie 기반 인증으로 토큰 노출을 줄였습니다.
  - 백엔드에서는 URL Role + Method Permission(`hasAuthority`) 2계층 권한 제어로 보안성과 운영 유연성을 확보했습니다.

---

## ERD
- 상세 ERD 문서: [ERD 상세 보기](docs/ERD.kr.md)

---

## Demo Accounts
> 데모용 계정이며 비밀번호는 운영상 변경될 수 있습니다.

- **Admin**: `a20001122` / `Admin!2345`
- **Professor**: `p20260001` / `Professor!2345`
- **Student**: `s20260001` / `Student!2345`
