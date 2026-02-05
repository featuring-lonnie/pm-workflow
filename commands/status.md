---
description: PM 상태 확인. 담당 프로젝트 현황 및 진행 중 작업 조회.
---

# /pm-workflow:status - 상태 확인

## 출력 형식

```
[PM] 작업 상태

============================================
내 담당 티켓 요약
============================================

| 상태 | 개수 |
|------|------|
| To Do | 5 |
| In Progress | 3 |
| In Review | 2 |
| Done (이번 주) | 8 |

============================================
진행 중 작업
============================================

1. FC-123 - 회원가입 리뉴얼 PRD
   상태: In Progress
   시작: 2026-02-03

2. BE-456 - 결제 시스템 요구사항
   상태: In Progress
   시작: 2026-02-04

============================================
리뷰 대기
============================================

1. FC-120 - 알림 시스템 PRD
   리뷰어: @stakeholder
   대기 기간: 2일

============================================
주요 일정
============================================

02/06 - FC v2.3.0 릴리즈
02/08 - BE 코드 프리즈
02/10 - QA 시작
```

---

## 실행 단계

### 1. 내 담당 티켓 조회

```
Tool: mcp__atlassian__searchJiraIssuesUsingJql
Parameters:
  - jql: "assignee = currentUser() AND status != Done ORDER BY updated DESC"
```

### 2. 상태별 집계

```
# In Progress
jql: "assignee = currentUser() AND status = 'In Progress'"

# In Review
jql: "assignee = currentUser() AND status = 'In Review'"

# 이번 주 완료
jql: "assignee = currentUser() AND status = Done AND resolved >= startOfWeek()"
```

### 3. 통합 상태 출력

---

## 사용 예시

```
/pm-workflow:status
# 전체 상태 확인

/pm-workflow:status --project FC
# FC 프로젝트만

/pm-workflow:status --all
# 모든 담당 프로젝트 상세
```

---

## 상태별 아이콘

| 상태 | 표시 |
|------|------|
| To Do | [TODO] |
| In Progress | [WIP] |
| In Review | [REVIEW] |
| Done | [DONE] |
| Blocked | [BLOCKED] |

---

## --all 옵션

모든 담당 프로젝트의 상세 현황:

```
Tool: mcp__atlassian__searchJiraIssuesUsingJql
Parameters:
  - jql: "project in ({pm_projects}) ORDER BY project, status"
```

출력:
```
[PM] 전체 프로젝트 현황

============================================
FC - Featuring
============================================
To Do: 5
In Progress: 3
Done: 12

============================================
BE - Backend
============================================
To Do: 8
In Progress: 4
Done: 25
```

---

## 빠른 명령어 안내

```
빠른 명령어:
   /pm-workflow:start {ticket}  - 작업 시작
   /pm-workflow:track           - 대시보드
   /pm-workflow:done {ticket}   - 작업 완료
```
