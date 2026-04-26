---
name: review
description: Perform a criteria-based code review using design intent, PR body, generated quality guide, and git diff.
---

# Criteria-Based Review

평가기준, 설계의도, PR 본문, git diff를 입력받아 근거 기반 코드리뷰를 수행한다.

## Inputs

- `.review-artifacts/{branch-name}/code-quality-guide.md`
- `.review-artifacts/{branch-name}/design-intent.md`
- `.review-artifacts/{branch-name}/pr-body.md`
- `git diff {base}...HEAD`

## Output

`.review-artifacts/{branch-name}/review-comments.md`

## Review Rules

- 모든 코멘트는 `code-quality-guide.md`의 기준에 근거해야 한다.
- 설계의도 문서에 명시된 의도적 결정을 존중한다.
- 의도와 실제 구현이 불일치하면 지적한다.
- 변경 제안의 side effect를 함께 설명한다.
- 파일 경로와 라인 번호를 명시한다.

## Priority

- `[p1]`: 반드시 수정. 버그, 기준 위반, 의도-구현 불일치
- `[p2]`: 강력 권장. 유지보수성, 가독성에 유의미한 영향
- `[p3]`: 권장. 개선하면 좋지만 현재 동작에는 문제 없음
- `[p4]`: 사소한 개선. 네이밍, 포맷팅 등

## Template

```markdown
# Code Review

## Summary
(전체 코드 품질에 대한 간략한 총평)

## Comments

### [p1] {파일경로}:{라인}
- 근거: {code-quality-guide.md의 기준}
- 내용: ...
- 제안: ...
- side effect: ...
- 제안 이유: ...
```

