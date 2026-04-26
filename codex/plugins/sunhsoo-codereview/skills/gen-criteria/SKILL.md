---
name: gen-criteria
description: Generate a code-quality guide for the current branch by filtering code conventions and ADRs to the relevant stack and design intent.
---

# Review Criteria

작업 설계를 분석하고, code convention과 ADR에서 이번 변경에 적용되는 항목만 추출해 평가기준 문서를 생성한다.

## Inputs

- `.review-artifacts/{branch-name}/design-intent.md`
- repo-local `docs/code-convention.yaml`, `docs/adr.yaml` if present
- fallback plugin docs under `codex/plugins/sunhsoo-codereview/docs/`
- `git diff {base}...HEAD`

## Output

`.review-artifacts/{branch-name}/code-quality-guide.md`

## Procedure

1. 설계의도와 diff를 읽어 작업 범위와 stack을 추정한다.
2. `code-convention.yaml`에서 `stacks: [all]` 및 관련 stack 항목만 필터링한다.
3. `adr.yaml`은 `stacks`, `status`, `context`, `decision`을 기준으로 이번 작업과 관련된 항목만 고른다.
4. 관련 없는 ADR은 제외한다.
5. 각 기준이 이번 작업에 어떻게 적용되는지 구체적으로 작성한다.
6. 기준 적용 범위가 모호하면 사용자에게 확인한다.

## Document Template

```markdown
# 평가기준

## 공통 기준 (Code Convention)
- {ID}: {rule}
  - 적용 방법: ...

## 이 작업에 적용되는 ADR

### ADR-XXX: ...
- 결정: ...
- 적용 방법: ...
```
