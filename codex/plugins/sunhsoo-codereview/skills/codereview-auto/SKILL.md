---
name: codereview-auto
description: Run the full SunhSOO code-review pipeline in Codex: inspect branch state, create design intent, generate criteria, draft PR body, review code, and reflect accepted comments. Use when the user asks to run the whole code-review automation flow.
---

# Codex Code-Review Pipeline

전체 코드리뷰 파이프라인을 오케스트레이션한다.

이 skill은 Claude Code의 `/sunhsoo-codereview:auto`와 같은 목적을 가진 Codex용 포팅이다. 산출물, 평가 원칙, 파이프라인 순서는 동일하게 유지하되, 실행 단위는 Claude slash command가 아니라 Codex skill이다.

## Preconditions

- 현재 작업 디렉터리는 git 저장소여야 한다.
- 구현이 완료된 feature branch에서 실행한다.
- 팀 상황에 맞는 기준 문서가 있어야 한다. 우선순위는 다음과 같다:
  1. repo-local `docs/code-convention.yaml`, `docs/adr.yaml`
  2. plugin bundled `codex/plugins/sunhsoo-codereview/docs/code-convention.yaml`, `codex/plugins/sunhsoo-codereview/docs/adr.yaml`

## Artifact Path

모든 산출물은 `.review-artifacts/{branch-name}/` 하위에 저장한다.
`{branch-name}`은 `git branch --show-current`로 확인한다.

산출물:

- `design-intent.md`
- `code-quality-guide.md`
- `pr-body.md`
- `review-comments.md`

## Pipeline

1. 상태 점검
   - `git branch --show-current`로 현재 branch를 확인한다.
   - base branch는 사용자가 명시하지 않으면 `main`으로 둔다.
   - `git diff {base}...HEAD --stat`으로 변경 파일 목록을 확인한다.
   - 산출물 디렉터리를 만든다.

2. 설계의도 작성
   - `write-intent` skill의 지침을 따른다.
   - 현재 대화, 변경 파일, diff를 바탕으로 `design-intent.md`를 작성한다.
   - 모호한 논의점이 있으면 사용자에게 확인한다.

3. 평가기준 수립
   - `gen-criteria` skill의 지침을 따른다.
   - code convention과 ADR 중 이번 작업에 적용되는 항목만 추출한다.
   - `code-quality-guide.md`를 작성한다.

4. PR 본문 생성
   - `create-pr-body` skill의 지침을 따른다.
   - diff 기반으로 `pr-body.md`를 작성한다.

5. 코드리뷰 실행
   - `review` skill의 지침을 따른다.
   - 모든 코멘트는 `code-quality-guide.md`의 기준에 근거해야 한다.
   - `review-comments.md`를 작성한다.

6. 리뷰 반영
   - `reflect-review` skill의 지침을 따른다.
   - 코멘트별 ACCEPT/REJECT 판단을 제시한다.
   - 사용자 확인 후 코드를 수정하고 빌드/테스트를 수행한다.

## Claude Compatibility

| Claude Code command | Codex skill |
| --- | --- |
| `/sunhsoo-codereview:auto` | `codereview-auto` |
| `/sunhsoo-codereview:write-intent` | `write-intent` |
| `/sunhsoo-codereview:gen-criteria` | `gen-criteria` |
| `/sunhsoo-codereview:create-pr-body` | `create-pr-body` |
| `/sunhsoo-codereview:review` | `review` |
| `/sunhsoo-codereview:reflect-review` | `reflect-review` |
| `/sunhsoo-codereview:update-docs` | `update-docs` |

## Rules

- 산출물 작성 단계에서는 소스코드를 수정하지 않는다. 소스 수정은 리뷰 반영 단계에서만 한다.
- ADR 전체를 무비판적으로 복사하지 않는다. 이번 작업과 관련된 항목만 포함한다.
- 근거 없는 취향 리뷰를 남기지 않는다.
- 사용자 확인이 필요한 모호함은 구체적인 선택지와 영향으로 정리한다.
