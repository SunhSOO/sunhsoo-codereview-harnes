---
name: create-pr-body
description: Create a PR body from the current branch diff, design intent, and review criteria.
---

# PR Body

구현 완료된 코드의 git diff와 설계 문서를 기반으로 PR 본문을 생성한다.

## Inputs

- `.review-artifacts/{branch-name}/design-intent.md`
- `.review-artifacts/{branch-name}/code-quality-guide.md`
- `git diff {base}...HEAD`

## Output

`.review-artifacts/{branch-name}/pr-body.md`

## Procedure

1. base branch가 지정되지 않았으면 `main`을 사용한다.
2. `git diff {base}...HEAD --stat`과 필요한 diff를 읽는다.
3. 변경 파일/모듈 단위로 변경사항을 정리한다.
4. breaking change, migration, 관련 이슈가 불명확하면 사용자에게 확인한다.
5. 확정된 PR 본문을 산출물 경로에 저장한다.

## Template

```markdown
# PR: {제목}

## Summary
(이 PR이 해결하는 문제와 접근 방식 요약)

## Changes

### {모듈/영역}
- ...

## Breaking Changes
없음

## Test Plan
- [ ] ...

## Related
- ...
```

