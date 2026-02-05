---
description: PRD 문서 작성. Confluence에 PRD 생성 후 Jira 티켓에 연동.
---

# /pm-workflow:prd - PRD 작성

## PRD 템플릿

```markdown
# [FC-123] {기능명} PRD

## 문서 정보
| 항목 | 내용 |
|------|------|
| Jira | [FC-123](https://featuring-corp.atlassian.net/browse/FC-123) |
| 작성자 | @lonnie |
| 작성일 | 2026-02-05 |
| 상태 | Draft / In Review / Approved |
| 버전 | 1.0 |

---

## 1. 개요

### 1.1 배경
- 왜 이 기능이 필요한가?
- 현재 문제점은 무엇인가?

### 1.2 목표
- 이 기능으로 달성하고자 하는 목표

### 1.3 성공 지표 (KPI)
| 지표 | 현재 | 목표 |
|------|------|------|
| 전환율 | 10% | 15% |
| DAU | 1000 | 1500 |

---

## 2. 사용자 스토리

### 2.1 대상 사용자
- Primary: {주요 사용자}
- Secondary: {부가 사용자}

### 2.2 사용자 여정
1. 사용자가 {action}
2. 시스템이 {response}
3. 사용자가 {result}

### 2.3 사용자 스토리
- As a {user}, I want to {action} so that {benefit}

---

## 3. 기능 요구사항

### 3.1 필수 기능 (Must Have)
- [ ] 기능 1
- [ ] 기능 2

### 3.2 권장 기능 (Should Have)
- [ ] 기능 3

### 3.3 선택 기능 (Could Have)
- [ ] 기능 4

### 3.4 제외 범위 (Won't Have)
- 기능 5 (사유: ...)

---

## 4. 비기능 요구사항

### 4.1 성능
- 응답 시간: < 200ms
- 동시 접속자: 1000명

### 4.2 보안
- 인증 방식
- 권한 관리

### 4.3 확장성
- 향후 확장 계획

---

## 5. 기술 제약사항

- 제약 1
- 제약 2

---

## 6. 와이어프레임 / 목업

[Figma 링크]

---

## 7. 일정

| 마일스톤 | 일정 | 담당 |
|----------|------|------|
| 기획 완료 | 02/05 | PM |
| 디자인 완료 | 02/10 | Designer |
| 개발 완료 | 02/20 | Developer |
| QA 완료 | 02/25 | QA |
| 릴리즈 | 02/28 | All |

---

## 8. 리스크 및 의존성

### 8.1 리스크
| 리스크 | 영향 | 대응 |
|--------|------|------|
| 외부 API 지연 | High | 대안 API 준비 |

### 8.2 의존성
- 디자인 시안 완료 필요
- 백엔드 API 준비 필요

---

## 9. 변경 이력

| 버전 | 일자 | 작성자 | 변경 내용 |
|------|------|--------|----------|
| 1.0 | 02/05 | lonnie | 초안 작성 |
```

---

## 실행 단계

### 1. Jira 티켓 정보 조회

```
Tool: mcp__atlassian__getJiraIssue
Parameters:
  - issueIdOrKey: "{ticket_id}"
```

### 2. Confluence 페이지 생성

```
Tool: mcp__atlassian__createConfluencePage
Parameters:
  - spaceKey: {roles.pm.confluence.prdSpaceKey}
  - title: "[{ticket_id}] {기능명} PRD"
  - body: "{rendered_template}"
```

### 3. Jira 티켓에 문서 링크 추가

```
Tool: mcp__atlassian__editJiraIssue
Parameters:
  - issueIdOrKey: "{ticket_id}"
  - description: "{기존 description}\n\n## 관련 문서\n- [PRD]({confluence_url})"
```

---

## 출력 형식

```
PRD 생성 완료

문서: [FC-123] 회원가입 리뉴얼 PRD
   URL: https://featuring-corp.atlassian.net/wiki/...
   스페이스: Product

Jira 연동:
   - FC-123 티켓에 PRD 링크 추가됨

다음 단계:
   1. PRD 내용 작성
   2. 스테이크홀더 리뷰
   3. 상태를 "Approved"로 변경
```

---

## 사용 예시

```
/pm-workflow:prd FC-123
# FC-123 티켓용 PRD 생성

/pm-workflow:prd FC-123 --template lite
# 간소화 템플릿으로 생성
```

---

## Attribution

```
Written with Claude Code
```
