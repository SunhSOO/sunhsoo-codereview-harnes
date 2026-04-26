---
name: reflect-review
description: Decide whether to accept or reject review comments, update review-comments.md, apply accepted changes after user confirmation, and run QA.
---

# Reflect Review

코드리뷰 코멘트를 검토하고, 수용 여부를 판단한 뒤 사용자 확인을 받아 코드를 수정한다.

## Input

`.review-artifacts/{branch-name}/review-comments.md`

## Procedure

1. 리뷰 코멘트를 읽는다.
2. 각 코멘트에 대해 ACCEPT/REJECT 판단과 근거를 작성한다.
3. 사용자에게 판단 결과를 제시한다.
4. 사용자 확인 후 ACCEPT 항목만 코드에 반영한다.
5. 빌드와 테스트를 실행한다.
6. 실패가 있으면 수정 루프를 반복한다.
7. `review-comments.md`에 판정을 추가한다.

## Decision Rules

- `[p1]`은 기본적으로 ACCEPT한다. REJECT하려면 명확한 근거가 필요하다.
- `[p2]`는 영향과 트레이드오프를 설명하고 사용자 확인을 받는다.
- `[p3]`는 판단을 제시하되 사용자 재량을 둔다.
- `[p4]`는 일괄 수용 또는 무시할 수 있다.

## Decision Format

```markdown
### [p1] src/payment/service.ts:42
- 근거: ...
- 내용: ...
- 제안: ...
- **판정: ACCEPT** — 수정 완료. `extractPaymentData()` 함수로 분리.
```

