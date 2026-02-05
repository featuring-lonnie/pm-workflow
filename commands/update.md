---
description: 스테이크홀더 업데이트. Slack 채널에 프로젝트 현황 공유.
---

# /pm-workflow:update - 스테이크홀더 업데이트

## 개요

Slack 채널에 프로젝트 현황을 공유합니다.

---

## 메시지 템플릿

### 주간 업데이트

```
{mentionGroup} 주간 업데이트

[{project}] {date_range}

이번 주 완료
- FC-123: 회원가입 리뉴얼
- FC-124: 대시보드 개선

진행 중
- FC-125: 결제 시스템 개선 (70%)
- FC-126: 알림 고도화 (50%)

다음 주 예정
- FC-127: 검색 기능 개선
- FC-128: 성능 최적화

블로커/이슈
- 외부 API 응답 지연 - 대응 중

Written with Claude Code
```

### 릴리즈 알림

```
Release v2.3.0 배포 안내

배포 일시: 2026-02-05 14:00

주요 변경사항:
- 회원가입 리뉴얼 (소셜 로그인)
- 대시보드 실시간 알림
- 결제 오류 수정

릴리즈 노트: {confluence_url}

문의: @pm-team

Written with Claude Code
```

### 긴급 공지

```
[긴급] 서비스 점검 안내

일시: 2026-02-05 02:00 ~ 04:00 KST
영향: 전체 서비스 접속 불가

사유: 데이터베이스 마이그레이션

문의: @pm-team
```

---

## 실행 단계

### 1. 업데이트 유형 선택

**AskUserQuestion:**
```
업데이트 유형을 선택하세요:

1. 주간 업데이트 - 정기 현황 공유
2. 릴리즈 알림 - 배포 안내
3. 긴급 공지 - 장애/점검 알림
4. 커스텀 - 직접 작성
```

### 2. 데이터 수집 (주간 업데이트)

```
Tool: mcp__atlassian__searchJiraIssuesUsingJql
Parameters:
  - jql: "project = {project} AND status changed to Done DURING (startOfWeek(), now())"
```

### 3. Slack 메시지 전송

```
Tool: mcp__slack__conversations_add_message
Parameters:
  - channelId: {roles.pm.slack.channelId}
  - text: "{message}"
```

---

## 출력 형식

```
업데이트 전송 완료

채널: #general
메시지 타입: 주간 업데이트

전송 내용:
---
{message_preview}
---

메시지 링크: {slack_message_url}
```

---

## 사용 예시

```
/pm-workflow:update
# 대화형으로 업데이트 작성

/pm-workflow:update --weekly
# 주간 업데이트 자동 생성

/pm-workflow:update --release v2.3.0
# 릴리즈 알림

/pm-workflow:update --urgent "서비스 점검 안내"
# 긴급 공지
```

---

## 채널 설정

config.json에서 채널 설정:

```json
{
  "roles": {
    "pm": {
      "slack": {
        "channelId": "...",
        "mentionGroup": "@channel"
      }
    }
  }
}
```

---

## 스케줄 알림 (수동)

```
정기 업데이트 알림:
- 매주 금요일 오후 5시 주간 업데이트
- 릴리즈 1일 전 배포 예고
- 릴리즈 직후 배포 완료 알림
```
