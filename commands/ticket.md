---
description: Jira 티켓 생성 및 관리. 티켓 생성, 수정, 서브태스크 추가.
---

# /pm-workflow:ticket - 티켓 관리

## 개요

Jira 티켓을 생성하고 관리합니다.

---

## 티켓 생성

### 1. 프로젝트 선택

```
Tool: mcp__atlassian__getVisibleJiraProjects
```

**AskUserQuestion:**
- "어떤 프로젝트에 티켓을 생성하시겠습니까?"
- 옵션: config의 `roles.pm.jiraProjects` 목록

### 2. 이슈 타입 선택

```
Tool: mcp__atlassian__getJiraProjectIssueTypesMetadata
Parameters:
  - projectKey: "{project_key}"
```

**AskUserQuestion:**
- "이슈 타입을 선택하세요"
- 옵션: Story, Task, Bug, Epic 등

### 3. 티켓 정보 입력

**AskUserQuestion:**
- 제목 (Summary)
- 설명 (Description)

### 3-1. 우선순위 선택 (필수)

**AskUserQuestion:**
- "우선순위를 선택하세요"
- 옵션:
  - `(P1) Highest` - 긴급, 즉시 처리 필요
  - `(P2) High` - 중요, 빠른 처리 필요
  - `(P3) Medium` - 일반적인 우선순위
  - `(P4) Low` - 낮은 우선순위
  - `(P5) Lowest` - 가장 낮은 우선순위

### 3-2. 기한 설정 (필수)

**AskUserQuestion:**
- "기한(Due Date)을 설정하세요"
- 입력 형식: `YYYY-MM-DD`
- 예시: `2026-02-14`

> ⚠️ **기한은 필수입니다.** 기한 없이 티켓을 생성할 수 없습니다.

### 3-3. 바로 시작 vs 백로그

**AskUserQuestion:**
- "티켓을 바로 시작하시겠습니까?"
- 옵션:
  - `바로 시작` - 상태를 In Progress로 변경하고 시작일을 오늘로 설정
  - `백로그` - To Do 상태로 유지

### 3-4. 담당자 (선택)

**AskUserQuestion:**
- "담당자를 지정하시겠습니까?"
- 옵션: config의 `users` 목록 또는 "지정 안함"

### 4. 티켓 생성

```
Tool: mcp__atlassian__createJiraIssue
Parameters:
  - cloudId: "{config.integrations.jira.cloudId}"
  - projectKey: "{project_key}"
  - summary: "{title}"
  - description: "{description}"
  - issueTypeName: "{issue_type}"
  - additional_fields:
      priority: { "name": "{priority}" }
      duedate: "{due_date_YYYY-MM-DD}"
```

### 4-1. 바로 시작 선택 시 추가 처리

"바로 시작"을 선택한 경우:

```
# 상태를 In Progress로 변경
Tool: mcp__atlassian__getTransitionsForJiraIssue
→ "In Progress" transition ID 찾기

Tool: mcp__atlassian__transitionJiraIssue
Parameters:
  - cloudId: "{config.integrations.jira.cloudId}"
  - issueIdOrKey: "{new_ticket_id}"
  - transition: { "id": "{in_progress_id}" }

# 시작일을 오늘로 설정
Tool: mcp__atlassian__editJiraIssue
Parameters:
  - cloudId: "{config.integrations.jira.cloudId}"
  - issueIdOrKey: "{new_ticket_id}"
  - fields: { "customfield_10015": "{today_date_YYYY-MM-DD}" }
```

---

## 티켓 수정

### 기존 티켓 정보 조회

```
Tool: mcp__atlassian__getJiraIssue
Parameters:
  - issueIdOrKey: "{ticket_id}"
```

### 티켓 수정

```
Tool: mcp__atlassian__editJiraIssue
Parameters:
  - issueIdOrKey: "{ticket_id}"
  - summary: "{new_summary}"
  - description: "{new_description}"
```

---

## 서브태스크 추가

```
Tool: mcp__atlassian__createJiraIssue
Parameters:
  - projectKey: "{project_key}"
  - summary: "{subtask_title}"
  - issueType: "Sub-task"
  - parent: "{parent_ticket_id}"
```

---

## 출력 형식

### 티켓 생성 완료

```
✅ 티켓 생성 완료

📋 Jira: {ticket_id}
   제목: {summary}
   타입: {issue_type}
   우선순위: {priority}
   기한: {due_date}
   상태: {status}  ← (바로 시작 시 "진행 중", 백로그 시 "해야 할 일")
   URL: https://featuring-corp.atlassian.net/browse/{ticket_id}

⚠️ 보고자(Reporter) 설정 안내:
   현재 보고자는 티켓 생성자로 자동 설정됩니다.
   다른 사람을 보고자로 지정하려면 Jira에서 직접 수정하세요:
   1. 위 URL로 이동
   2. 상세 정보 > 보고자 클릭
   3. 보고자 변경

📝 다음 단계:
   /pm-workflow:start {ticket_id}  - 작업 시작 (백로그인 경우)
   /pm-workflow:prd {ticket_id}    - PRD 작성
```

---

## 사용 예시

```
/pm-workflow:ticket
# 대화형 티켓 생성

/pm-workflow:ticket create --project FC --type Story
# 옵션 지정하여 생성

/pm-workflow:ticket edit FC-123
# 티켓 수정

/pm-workflow:ticket subtask FC-123 "API 설계 검토"
# 서브태스크 추가
```

---

## 티켓 템플릿

### Story 템플릿

```markdown
## 사용자 스토리
As a {user_type}, I want to {action} so that {benefit}.

## 인수 조건
- [ ] 조건 1
- [ ] 조건 2

## 기술 요구사항
- 요구사항 1
- 요구사항 2

## 관련 문서
- [PRD](confluence_link)
```

### Bug 템플릿

```markdown
## 버그 설명
{description}

## 재현 단계
1. Step 1
2. Step 2
3. Step 3

## 예상 결과
{expected}

## 실제 결과
{actual}

## 환경
- 브라우저:
- OS:
```
