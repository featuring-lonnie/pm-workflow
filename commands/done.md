---
description: PM 작업 완료. Jira 티켓을 Done으로 변경 (PR 머지 없음).
---

# /pm-workflow:done - 작업 완료

## 개요

Jira 티켓을 Done으로 변경합니다. PM은 PR 머지 과정이 없습니다.

---

## 실행 단계

### 1. Jira 티켓 확인

```
Tool: mcp__atlassian__getJiraIssue
Parameters:
  - issueIdOrKey: "{ticket_id}"
```

**확인 항목:**
- 현재 상태
- 관련 문서 (PRD, 릴리즈 노트 등)

### 2. 완료 체크리스트 확인

**AskUserQuestion:**
```
완료 전 체크리스트:

- [ ] PRD 또는 관련 문서 작성 완료
- [ ] 스테이크홀더 리뷰 완료
- [ ] 관련 서브태스크 모두 완료

모두 완료되었습니까?
```

### 3. Jira 상태 변경 → Done

```
Tool: mcp__atlassian__getTransitionsForJiraIssue
→ "Done" transition ID 찾기

Tool: mcp__atlassian__transitionJiraIssue
Parameters:
  - issueIdOrKey: "{ticket_id}"
  - transitionId: "{done_id}"
```

### 4. 완료 요약

```
작업 완료

Jira: {ticket_id}
   제목: {summary}
   상태: Done

관련 문서:
   - PRD: {prd_link}
   - 릴리즈 노트: {release_notes_link}

완료 일시: 2026-02-05 15:30
```

---

## 사용 예시

```
/pm-workflow:done FC-123
# FC-123 티켓 완료 처리

/pm-workflow:done
# 현재 작업 중인 티켓 완료 (최근 In Progress 티켓)
```

---

## 에러 처리

| 상황 | 대응 |
|------|------|
| 서브태스크 미완료 | "서브태스크를 먼저 완료해주세요" |
| Done transition 없음 | "현재 상태에서 Done으로 변경할 수 없습니다" |

---

## 완료 후 제안

```
다음 작업 제안:

1. /pm-workflow:track - 다른 진행 중 작업 확인
2. /pm-workflow:update - 스테이크홀더에게 완료 알림
3. /pm-workflow:ticket - 새 티켓 생성
```
