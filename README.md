# 🚀 Git-Log

> “기록은 AI에게, 성장은 개발자에게.”
>
> GitHub 활동 데이터를 기반으로  
> AI가 기술 회고와 개발 기록을 자동 생성하는  
> 개발자 브랜딩 SaaS 플랫폼입니다.

---

# ✨ Overview

Git-Log는 GitHub의 Commit, Pull Request, Diff 데이터를 분석하여  
개발자의 기술 회고를 자동 생성해주는 AI 기반 서비스입니다.

단순 커밋 로그 요약이 아닌 실제 코드 변경점(Diff)을 기반으로:

- 구현 기능
- 문제 해결 과정
- 기술적 의사결정
- 개선 포인트

등을 분석하여 기술 블로그 수준의 회고 초안을 생성합니다.

---

# 🛠 Tech Stack

## Backend
- Java 17
- Spring Boot 3.x
- Spring Data JPA

## Database
- MySQL 8.0
- Redis (Caching / Rate Limiting)

## AI & External API
- OpenAI API / Gemini API
- GitHub REST API
- GitHub GraphQL API

## Infrastructure
- Docker
- GitHub Actions (CI/CD)

## Environment
- MacBook Pro M1 16"
- IntelliJ IDEA

---

# 📌 Core Features

## 1. GitHub OAuth & Repository Sync

GitHub 계정을 연동하여 사용자의 Repository 및 Commit 데이터를 조회합니다.

### Features
- GitHub OAuth 로그인
- Repository 목록 조회
- Commit 및 Diff 데이터 수집

### Edge Cases
- 대용량 Repository Pagination 처리
- Organization Repository 권한 예외 처리
- OAuth Token 만료 및 재인증 처리

---

## 2. AI 기반 기술 회고 생성 (Core)

선택한 Commit들의 Diff를 분석하여 기술 회고를 자동 생성합니다.

### AI Analysis
- 구현 기능 요약
- 문제 해결 과정 분석
- 기술적 의사결정 추론
- 개선 및 리팩토링 포인트 도출

### Edge Cases

#### Token Limit Handling
LLM 토큰 제한 초과 시:
- 파일 단위 Chunking
- 핵심 로직 파일 우선 분석
- 중요도 기반 Diff 압축 처리

#### Non-Code File Filtering
다음 파일들은 자동 제외:
- `package-lock.json`
- `.gitignore`
- 이미지 및 바이너리 파일

#### Meaningless Commit Recovery
`fix`, `update` 등 의미 없는 커밋 메시지는  
실제 Diff 기반으로 맥락을 재구성합니다.

#### Merge Conflict Handling
대규모 Merge Commit 분석 시:
- 작업 사용자 필터링
- 변경 주체 기반 분석 수행

---

## 3. Markdown Editor & Export

생성된 회고를 수정하고 Markdown 형식으로 내보낼 수 있습니다.

### Features
- Markdown 기반 편집
- `.md` 파일 Export
- 기술 블로그 초안 활용

### Edge Cases
- LocalStorage 기반 Auto Save
- Mermaid / 이미지 링크 호환성 유지

---

# 🏗 System Architecture

```text
Client
   ↓
Spring Boot API Server
   ↓
GitHub API Integration
   ↓
Diff Pre-processing
   ↓
Redis Cache Layer
   ↓
AI Orchestrator
   ↓
LLM Analysis
   ↓
MySQL Storage
```

---

# 🗄 Database ERD (Draft)

## User
| Column | Type |
|---|---|
| id | BIGINT |
| github_id | VARCHAR |
| email | VARCHAR |
| oauth_token | TEXT |

---

## Repository
| Column | Type |
|---|---|
| id | BIGINT |
| user_id | BIGINT |
| repo_name | VARCHAR |
| full_path | VARCHAR |

---

## Retrospective
| Column | Type |
|---|---|
| id | BIGINT |
| user_id | BIGINT |
| title | VARCHAR |
| content | LONGTEXT |
| created_at | DATETIME |
| commit_range | JSON |

---

# 🎯 Key Value

Git-Log는 단순한 GitHub 분석 도구가 아닙니다.

개발자의 GitHub 활동을:
- 기술 회고
- 포트폴리오
- 블로그 초안
- 성장 기록

으로 자동 변환하여  
개발자 브랜딩과 기록 자동화를 지원합니다.

---

# 📅 Roadmap

## Phase 1
- [ ] GitHub OAuth 구축
- [ ] Spring Boot + MySQL 인프라 구성

## Phase 2
- [ ] GitHub API 연동
- [ ] Diff 전처리 로직 개발

## Phase 3
- [ ] LLM Prompt Engineering
- [ ] AI 분석 파이프라인 최적화

## Phase 4
- [ ] Markdown Editor UI 개발
- [ ] Export 기능 구현

## Phase 5
- [ ] Redis 기반 캐싱
- [ ] 성능 최적화
- [ ] V1 배포

---

# 📈 Future Plans

- Velog / Notion 연동
- GitHub Webhook 실시간 분석
- AI 기반 면접 질문 생성
- 기술 성장 리포트 시각화
- Vector DB 기반 유사 문제 추천

---

# 🧠 Philosophy

> “GitHub에는 코드가 남고,
> Git-Log에는 성장 과정이 남습니다.”
