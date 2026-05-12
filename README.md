# git-log
🚀 Git-Log: AI-Powered Tech Retrospective Generator
"기록은 AI에게, 성장은 마스터에게." > GitHub 활동 데이터를 분석하여 기술 블로그 급의 회고록을 자동으로 생성해주는 개발자 브랜딩 어시스턴트입니다.

🛠 Tech Stack
Backend: Java 17, Spring Boot 3.x, Spring Data JPA

Database: MySQL 8.0, Redis (Cache & Rate Limiting)

AI & API: OpenAI/Gemini API (Diff Analysis), GitHub REST/GraphQL API

Infrastructure: Docker, GitHub Actions (CI/CD)

Environment: MacBook Pro M1 16" / IntelliJ IDEA

📌 Core Features & Edge Cases
1. GitHub OAuth & Repository Sync

Feature: GitHub 계정 연동을 통한 레포지토리 목록 및 커밋 이력 조회.

Edge Cases:

Large Repositories: 레포지토리에 수만 개의 커밋이 있는 경우 페이지네이션 처리 및 필요한 범위(최근 1주 등)만 필터링.

Organization Repos: 개인 레포 외에 소속된 조직(Org)의 레포 접근 권한 미승인 시 가이드 제공.

Token Expiration: OAuth 토큰 만료 시 재로그인 유도 및 Refresh Token 처리.

2. Diff 기반 AI 기술 회고 생성 (Core)

Feature: 선택한 커밋들의 Diff를 분석하여 구현 기능, 문제 해결, 개선 사항 도출.

Edge Cases:

Token Limit (Critical): Diff 내용이 너무 길어 LLM의 토큰 제한을 초과할 경우 -> 파일별 중요도 기반 Chunking 또는 핵심 로직 파일(.java, .py 등) 우선순위 분석.

Non-Code Files: package-lock.json, .gitignore, 이미지 파일 등 로직과 무관한 변경 사항 제외 로직.

Empty/Meaningless Commits: "fix", "update" 등 의미 없는 커밋 메시지만 있을 때 코드를 직접 분석하여 맥락 복구.

Conflicting Diffs: 여러 개발자가 동시에 작업한 대규모 Merge 커밋 분석 시 작업 주체 필터링.

3. Markdown 기반 편집 및 내보내기

Feature: AI 생성 회고를 사용자가 수정하고 Markdown 파일로 추출.

Edge Cases:

Auto-Save: 작성 중 브라우저 종료 시 Redis나 LocalStorage를 통한 임시 저장 기능.

Image Handling: 코드 내에 포함된 이미지 링크나 다이어그램(Mermaid 등)의 렌더링 호환성 유지.

🏗 System Architecture (Conceptual)
Client: 웹 인터페이스를 통해 회고 대상 커밋 선택.

API Gateway (Spring Boot): GitHub API를 호출하여 Diff 데이터를 가져오고 전처리(Pre-processing).

Cache (Redis): 동일한 Diff에 대한 중복 분석 요청 방지 및 AI API 호출 비용 절감.

AI Orchestrator: 전처리된 데이터를 프롬프트에 실어 LLM에 전달 및 결과 파싱.

Storage (MySQL): 생성된 회고록 및 유저 메타데이터 저장.

🗄 Database ERD (Draft)
User: ID, GitHub_ID, Email, OAuth_Token

Repository: ID, User_ID, Repo_Name, Full_Path

Retrospective: ID, User_ID, Title, Content(LongText), Created_At, Commit_Range(JSON)

📅 Roadmap
[ ] Phase 1: GitHub OAuth 및 기초 인프라(Spring Boot + MySQL) 구축

[ ] Phase 2: GitHub API 연동 및 Diff 데이터 전처리 로직 개발

[ ] Phase 3: LLM 프롬프트 엔지니어링 및 AI 분석 파이프라인 최적화

[ ] Phase 4: Markdown 에디터 UI 및 내보내기 기능 구현

[ ] Phase 5: Redis 기반 캐싱 및 성능 튜닝 (V1.0 배포)
