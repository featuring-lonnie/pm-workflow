---
description: PM 워크플로우 초기 설정. 외부 연동 확인 후 프로젝트 설정.
---

# /pm-workflow:init - PM 워크플로우 초기 설정

## 개요

PM 워크플로우 사용을 위한 초기 설정을 진행합니다.

---

## Phase 1: 외부 연동 확인

### 1.1 Slack MCP 확인

```
Tool: mcp__slack__channels_list
Parameters:
  - limit: 1
```

### 1.2 Atlassian MCP 확인 (Jira)

```
Tool: mcp__atlassian__atlassianUserInfo
```

### 1.3 Atlassian MCP 확인 (Confluence)

```
Tool: mcp__atlassian__getConfluenceSpaces
Parameters:
  - limit: 1
```

### Phase 1 결과 출력

```
PM 워크플로우 연동 상태

| 서비스          | 상태   | 비고                |
|-----------------|--------|---------------------|
| Slack MCP       | OK/FAIL| 연결됨 / 설정필요   |
| Jira MCP        | OK/FAIL| 연결됨 / 설정필요   |
| Confluence MCP  | OK/FAIL| 연결됨 / 설정필요   |
```

---

## Phase 2: PM 설정

### 2.1 전문분야 선택

```
PM 전문분야를 선택하세요:

1. Product - PRD, 기능 기획, 사용자 리서치
2. Project - 일정 관리, 리소스 조율
3. Technical - 기술 PM, 아키텍처 검토
```

### 2.2 담당 프로젝트 설정

```
Tool: mcp__atlassian__getVisibleJiraProjects
```

**AskUserQuestion:**
- "담당하는 Jira 프로젝트를 선택하세요 (복수 선택 가능)"

### 2.3 Slack 채널 설정

**AskUserQuestion:**
- "스테이크홀더 업데이트에 사용할 채널을 선택하세요"

### 2.4 Confluence 스페이스 설정

**AskUserQuestion:**
- "PRD 문서를 저장할 스페이스를 선택하세요"
- "릴리즈 노트를 저장할 스페이스를 선택하세요"

---

## 설정 파일

`~/.claude/workflow/config.json` 의 `roles.pm` 섹션:

```json
{
  "roles": {
    "pm": {
      "enabled": true,
      "specialization": "product",
      "commands": ["start", "ticket", "track", "prd", "release", "update", "done", "status"],
      "slack": {
        "channelId": "...",
        "channelName": "...",
        "mentionGroup": "@PM"
      },
      "jiraProjects": ["FC", "FS", "BE"],
      "confluence": {
        "prdSpaceKey": "...",
        "releaseNotesSpaceKey": "...",
        "meetingNotesSpaceKey": "..."
      }
    }
  }
}
```

---

## 사용 예시

```
/pm-workflow:init
# 전체 초기 설정

/pm-workflow:init --check
# 연동 상태만 확인

/pm-workflow:init --reconfigure
# 재설정
```

---

## 설정 완료 후 안내

```
PM 워크플로우 설정 완료!

사용자: lonnie
역할: pm
전문분야: product

사용 가능 명령어:
   /pm-workflow:start    - 티켓 작업 시작
   /pm-workflow:ticket   - 티켓 생성/관리
   /pm-workflow:track    - 프로젝트 대시보드
   /pm-workflow:prd      - PRD 문서 작성
   /pm-workflow:release  - 릴리즈 노트 작성
   /pm-workflow:update   - 스테이크홀더 업데이트
   /pm-workflow:done     - 작업 완료
   /pm-workflow:status   - 상태 확인
```
