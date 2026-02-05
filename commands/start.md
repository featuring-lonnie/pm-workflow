---
description: PM 작업 시작. Jira 티켓을 In Progress로 변경 (Git 작업 없음).
---

# /pm-workflow:start - 작업 시작

## 개요

Jira 티켓을 In Progress로 변경합니다. PM은 Git 브랜치를 생성하지 않습니다.

---

## 실행 단계

### 1. Jira 티켓 확인

```
Tool: mcp__atlassian__getJiraIssue
Parameters:
  - issueIdOrKey: "{ticket_id}"
```

**확인 항목:**
- 티켓 존재 여부
- 현재 상태
- 담당자

### 2. Jira 상태 변경 → In Progress

```
Tool: mcp__atlassian__getTransitionsForJiraIssue
→ "In Progress" transition ID 찾기

Tool: mcp__atlassian__transitionJiraIssue
Parameters:
  - issueIdOrKey: "{ticket_id}"
  - transitionId: "{in_progress_id}"
```

### 3. 작업 요약 출력

```
작업 시작

Jira: {ticket_id}
   제목: {summary}
   상태: In Progress
   담당자: {assignee}

다음 단계:
   /pm-workflow:prd     - PRD 문서 작성
   /pm-workflow:track   - 프로젝트 현황 확인
   /pm-workflow:update  - 스테이크홀더 업데이트
```

---

## 사용 예시

```
/pm-workflow:start FC-123
# FC-123 티켓 작업 시작

/pm-workflow:start https://featuring-corp.atlassian.net/browse/FC-123
# URL로 티켓 지정
```

---

## 에러 처리

| 상황 | 대응 |
|------|------|
| 티켓 없음 | "티켓을 찾을 수 없습니다" |
| 이미 In Progress | "이미 진행 중인 티켓입니다" |
| 권한 없음 | "티켓 상태 변경 권한이 없습니다" |
