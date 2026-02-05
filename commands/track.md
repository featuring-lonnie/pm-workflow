---
description: 프로젝트 대시보드. 담당 프로젝트의 티켓 현황 및 진행 상태 조회.
---

# /pm-workflow:track - 프로젝트 대시보드

## 개요

담당 프로젝트의 전체 현황을 대시보드 형태로 조회합니다.

---

## 실행 단계

### 1. 담당 프로젝트 티켓 조회

```
Tool: mcp__atlassian__searchJiraIssuesUsingJql
Parameters:
  - jql: "project in ({pm_projects}) AND assignee = currentUser() ORDER BY updated DESC"
```

### 2. 상태별 집계

```
Tool: mcp__atlassian__searchJiraIssuesUsingJql
Parameters:
  - jql: "project in ({pm_projects}) AND status = 'In Progress'"
```

### 3. 스프린트 현황 (있는 경우)

```
Tool: mcp__atlassian__searchJiraIssuesUsingJql
Parameters:
  - jql: "project = {project} AND sprint in openSprints()"
```

---

## 출력 형식

```
프로젝트 대시보드

============================================
전체 현황
============================================

| 프로젝트 | To Do | Progress | Review | Done |
|----------|-------|----------|--------|------|
| FC       | 5     | 3        | 2      | 12   |
| BE       | 8     | 4        | 1      | 25   |
| FS       | 3     | 2        | 0      | 8    |

============================================
내 담당 티켓 (10건)
============================================

In Progress (3)
   - FC-123: 회원가입 리뉴얼 PRD
   - BE-456: 결제 시스템 요구사항
   - FS-789: 대시보드 기획

In Review (2)
   - FC-120: 알림 시스템 PRD (리뷰 대기)
   - BE-450: API 스펙 검토

Blocked (1)
   - FC-115: 외부 연동 - 담당자 확인 필요

============================================
이번 주 완료 (5건)
============================================

   - FC-110: 로그인 개선
   - FC-112: 프로필 수정
   ...

============================================
주요 일정
============================================

   02/06 - FC 릴리즈 v2.3.0
   02/08 - BE 코드 프리즈
   02/10 - QA 시작
```

---

## 필터 옵션

### --project

특정 프로젝트만 조회:

```
/pm-workflow:track --project FC
```

### --sprint

현재 스프린트 현황:

```
/pm-workflow:track --sprint
```

### --blocked

블로커 티켓만 조회:

```
/pm-workflow:track --blocked
```

---

## 사용 예시

```
/pm-workflow:track
# 전체 대시보드

/pm-workflow:track --project FC
# FC 프로젝트만

/pm-workflow:track --summary
# 요약만 표시
```

---

## JQL 템플릿

### 내 담당 티켓

```
assignee = currentUser() AND status != Done ORDER BY priority DESC
```

### 이번 주 완료

```
assignee = currentUser() AND status = Done AND resolved >= startOfWeek()
```

### 블로커 티켓

```
project in ({projects}) AND (status = Blocked OR labels = blocked)
```

### 스프린트 번다운

```
project = {project} AND sprint in openSprints() ORDER BY rank
```
