---
name: write-intent
description: Create a design-intent document for the current branch from conversation context, requirements, implementation choices, and git diff. Use before generating review criteria or PR body.
---

# Design Intent

현재 대화에서 논의된 요구사항, 설계 결정, 트레이드오프, 구현 diff를 기반으로 설계의도 문서를 작성한다.

## Output

`.review-artifacts/{branch-name}/design-intent.md`

## Procedure

1. `git branch --show-current`로 branch명을 확인한다.
2. 사용자가 base branch를 지정하지 않았으면 `main`을 사용한다.
3. `git diff {base}...HEAD --stat`과 필요한 범위의 diff를 읽는다.
4. 작업 개요, 핵심 설계 결정, 의도적으로 제외한 것, 주의사항을 정리한다.
5. 모호하거나 구체화가 필요한 논의점이 있으면 사용자에게 먼저 제시한다.
6. 확정된 내용을 산출물 경로에 저장한다.

## Document Template

```markdown
# 설계의도

## 작업 개요
(이 작업이 무엇을 해결하는가)

## 핵심 설계 결정
(어떤 선택을 했고, 왜 그 선택이 적절한가)

### 결정 1: ...
- 선택지: A vs B
- 선택: A
- 이유: ...

## 의도적으로 제외한 것
(이번 작업 범위에서 빠진 것과 그 이유)

## 주의사항
(구현/리뷰 시 특히 신경 써야 할 점)
```

