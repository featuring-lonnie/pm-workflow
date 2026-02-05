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
- 우선순위 (Priority)
- 담당자 (Assignee) - 선택

### 4. 티켓 생성

```
Tool: mcp__atlassian__createJiraIssue
Parameters:
  - projectKey: "{project_key}"
  - summary: "{title}"
  - description: "{description}"
  - issueType: "{issue_type}"
  - priority: "{priority}"
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
티켓 생성 완료

Jira: {ticket_id}
   제목: {summary}
   타입: {issue_type}
   우선순위: {priority}
   URL: https://featuring-corp.atlassian.net/browse/{ticket_id}

다음 단계:
   /pm-workflow:start {ticket_id}  - 작업 시작
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
