# SunhSOO Code-Review Harness

![flow.png](./assets/flow.png)

Claude Code와 Codex에서 사용할 수 있는 코드리뷰 자동화 파이프라인.

> Git과 함께 사용할 때 가장 효과적입니다.

> 하네스와 파이프라인에 대한 자세한 설명은 [영상](https://www.youtube.com/watch?v=CfLhrS0ww5w)을 참고해주세요.

## Repository Layout

```text
.
├─ .claude-plugin/marketplace.json          # Claude Code marketplace entry
├─ claude/sunhsoo-codereview/               # Claude Code plugin implementation
│  ├─ .claude-plugin/plugin.json
│  ├─ CLAUDE.md
│  ├─ commands/
│  └─ docs/
├─ .agents/plugins/marketplace.json         # Codex marketplace entry
├─ codex/plugins/sunhsoo-codereview/        # Codex plugin implementation
│  ├─ .codex-plugin/plugin.json
│  ├─ skills/
│  ├─ docs/
│  └─ assets/
└─ assets/                                  # Shared README assets
```

Claude Code와 Codex는 같은 파이프라인을 공유하지만, 실제 플러그인 구현 경로는 위처럼 분리되어 있습니다.

## Quick Start

### Claude Code

1. 플러그인을 설치합니다.

   ```sh
   # claude code 실행
   claude

   # 플러그인 설치
   /plugin marketplace add SunhSOO/sunhsoo-codereview-harnes
   /plugin install sunhsoo-codereview
   ```

2. `docs/code-convention.yaml`과 `docs/adr.yaml`를 복사한 뒤, 상황에 맞게 수정합니다.
3. Claude Code에서 `/sunhsoo-codereview:auto`를 실행하세요.

### Codex

이 저장소에는 Claude Code용 플러그인과 별도로 Codex용 로컬 플러그인도 포함되어 있습니다.

```text
.agents/plugins/marketplace.json
codex/plugins/sunhsoo-codereview/
```

Codex에서 repo-local marketplace를 읽도록 설정한 뒤 `sunhsoo-codereview` 플러그인을 설치하면 다음 skill들을 사용할 수 있습니다.

Codex판은 Claude판과 **같은 산출물/같은 리뷰 철학/같은 파이프라인**을 사용하지만, 실행 단위는 slash command가 아니라 Codex skill입니다.

## Claude Code와 Codex 대응 관계

| 목적 | Claude Code | Codex |
| --- | --- | --- |
| 전체 파이프라인 실행 | `/sunhsoo-codereview:auto` | `codereview-auto` |
| 설계의도 작성 | `/sunhsoo-codereview:write-intent` | `write-intent` |
| 평가기준 생성 | `/sunhsoo-codereview:gen-criteria` | `gen-criteria` |
| PR 본문 생성 | `/sunhsoo-codereview:create-pr-body` | `create-pr-body` |
| 코드리뷰 실행 | `/sunhsoo-codereview:review` | `review` |
| 리뷰 반영 + QA | `/sunhsoo-codereview:reflect-review` | `reflect-review` |
| 기준 문서 갱신 | `/sunhsoo-codereview:update-docs` | `update-docs` |

## 동일한 부분

- 파이프라인 순서가 같습니다.
- 산출물 경로가 같습니다: `.review-artifacts/{branch-name}/`
- 산출물 파일명이 같습니다: `design-intent.md`, `code-quality-guide.md`, `pr-body.md`, `review-comments.md`
- `docs/code-convention.yaml`과 `docs/adr.yaml`를 기준 문서로 사용합니다.
- ADR은 전체 복사가 아니라 이번 작업에 관련된 항목만 추출합니다.
- 코드리뷰는 반드시 평가기준에 근거해야 하며, 취향 리뷰를 남기지 않습니다.

## 다른 부분

- Claude Code판은 `claude/sunhsoo-codereview/commands/*.md` 기반의 slash command 플러그인입니다.
- Codex판은 `codex/plugins/sunhsoo-codereview/skills/*/SKILL.md` 기반의 skill 플러그인입니다.
- Claude Code판의 문서에는 `worktree(fork)`와 `sub-agent` 실행이 명시되어 있습니다.
- Codex판은 같은 의도를 유지하되, Codex가 skill 지침을 따라 실행하는 형태입니다.
- 따라서 Codex판은 “동일한 파이프라인을 Codex 규격으로 포팅한 버전”이며, Claude 런타임의 slash command 실행 모델과 완전히 같은 구현은 아닙니다.

## Codex Skill 목록

| Skill | 설명 |
| --- | --- |
| `codereview-auto` | 전체 코드리뷰 파이프라인 오케스트레이션 |
| `write-intent` | 설계의도 문서 작성 |
| `gen-criteria` | 평가기준 생성 |
| `create-pr-body` | PR 본문 생성 |
| `review` | 근거 기반 코드리뷰 실행 |
| `reflect-review` | 리뷰 반영 + QA |
| `update-docs` | code-convention / ADR 항목 관리 |

## 파이프라인 흐름

```
Claude: /sunhsoo-codereview:auto
Codex: codereview-auto

  → [1] 상태 점검 (base branch 확인)
  → [2] 설계의도 작성
  → [3] 평가기준 수립
  → [4] PR 본문 생성
  → [5] 코드리뷰 실행
  → [6] 리뷰 반영 + QA
```

## Claude Code 커맨드 목록

| 커맨드                             | 설명                            |
| ---------------------------------- | ------------------------------- |
| `/sunhsoo-codereview:auto`           | 전체 파이프라인 오케스트레이션  |
| `/sunhsoo-codereview:write-intent`   | 설계의도 문서 작성              |
| `/sunhsoo-codereview:gen-criteria`   | 평가기준 자동 생성              |
| `/sunhsoo-codereview:create-pr-body` | PR 본문 생성                    |
| `/sunhsoo-codereview:review`         | 코드리뷰 실행                   |
| `/sunhsoo-codereview:reflect-review` | 리뷰 반영 + QA                  |
| `/sunhsoo-codereview:update-docs`    | code-convention / ADR 항목 관리 |

## 산출물

작업별 산출물은 `.review-artifacts/{branch-name}/`에 저장됩니다.

## License

MIT
