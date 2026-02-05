---
description: 릴리즈 노트 작성. Confluence에 릴리즈 노트 생성.
---

# /pm-workflow:release - 릴리즈 노트 작성

## 릴리즈 노트 템플릿

```markdown
# Release Notes v{version}

**릴리즈 일자**: 2026-02-05
**프로젝트**: {project_name}

---

## 주요 변경사항

### New Features
- **[FC-123] 회원가입 리뉴얼**: 소셜 로그인 추가
- **[FC-124] 대시보드 개선**: 실시간 알림 기능

### Improvements
- **[FC-125]**: 페이지 로딩 속도 30% 개선
- **[FC-126]**: 모바일 UI 최적화

### Bug Fixes
- **[FC-127]**: 결제 시 간헐적 오류 수정
- **[FC-128]**: 프로필 이미지 업로드 실패 수정

### Breaking Changes
- API v1 deprecation (v2로 마이그레이션 필요)

---

## 포함된 티켓

| 티켓 | 타입 | 제목 | 담당자 |
|------|------|------|--------|
| FC-123 | Feature | 회원가입 리뉴얼 | @dev1 |
| FC-124 | Feature | 대시보드 개선 | @dev2 |
| FC-127 | Bug | 결제 오류 수정 | @dev1 |

---

## 배포 정보

- **환경**: Production
- **배포 시간**: 2026-02-05 14:00 KST
- **롤백 계획**: v2.2.0으로 롤백 가능

---

## 알려진 이슈

- 일부 구형 브라우저에서 UI 깨짐 (Chrome 90 이상 권장)

---

## 다음 릴리즈 예정

- v2.4.0 (2026-02-20 예정)
  - 결제 시스템 개선
  - 알림 고도화
```

---

## 실행 단계

### 1. 릴리즈 대상 티켓 수집

```
Tool: mcp__atlassian__searchJiraIssuesUsingJql
Parameters:
  - jql: "project = {project} AND fixVersion = '{version}' ORDER BY issuetype"
```

또는 날짜 범위로:

```
jql: "project = {project} AND status = Done AND resolved >= '{start_date}' ORDER BY issuetype"
```

### 2. 티켓 분류

- Feature: issuetype = "Story" OR issuetype = "Feature"
- Improvement: issuetype = "Improvement"
- Bug Fix: issuetype = "Bug"

### 3. Confluence 페이지 생성

```
Tool: mcp__atlassian__createConfluencePage
Parameters:
  - spaceKey: {roles.pm.confluence.releaseNotesSpaceKey}
  - title: "Release Notes v{version}"
  - body: "{rendered_template}"
```

---

## 출력 형식

```
릴리즈 노트 생성 완료

버전: v2.3.0
문서: https://featuring-corp.atlassian.net/wiki/...

포함된 변경사항:
   - Features: 5
   - Improvements: 3
   - Bug Fixes: 8

다음 단계:
   1. 릴리즈 노트 검토
   2. 스테이크홀더 공유
   3. /pm-workflow:update로 Slack 알림
```

---

## 사용 예시

```
/pm-workflow:release v2.3.0
# v2.3.0 릴리즈 노트 생성

/pm-workflow:release v2.3.0 --project FC
# FC 프로젝트만

/pm-workflow:release --from 2026-02-01 --to 2026-02-05
# 날짜 범위로 수집
```

---

## Slack 알림

릴리즈 노트 작성 후 `/pm-workflow:update` 와 연동:

```
Release v2.3.0 배포 안내

주요 변경사항:
- 회원가입 리뉴얼 (소셜 로그인)
- 대시보드 실시간 알림

릴리즈 노트: {confluence_url}
```
