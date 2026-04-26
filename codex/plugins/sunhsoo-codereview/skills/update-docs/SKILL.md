---
name: update-docs
description: Update code-convention.yaml or adr.yaml after analyzing current work, existing rules, conflicts, and cascading documentation changes.
---

# Update Review Docs

`docs/code-convention.yaml` 또는 `docs/adr.yaml`에 항목을 추가, 수정, 삭제한다.

## Rule

문서를 변경하기 전에 반드시 기존 `code-convention.yaml`과 `adr.yaml`을 함께 읽고 충돌 여부를 확인한다.

## Procedure

1. repo-local `docs/code-convention.yaml`, `docs/adr.yaml`을 우선 읽는다.
2. 없으면 plugin bundled `codex/plugins/sunhsoo-codereview/docs/`의 샘플을 참조한다.
3. 현재 작업의 의사결정과 기존 항목을 대조한다.
4. 다음을 분류해 사용자에게 제안한다:
   - MUST: 모순 해결이나 기준 유지를 위해 반드시 필요한 변경
   - RECOMMENDED: 일관성 또는 향후 리뷰 품질을 위해 권장되는 변경
5. 사용자 확인 후 문서를 수정한다.

## code-convention Schema

```yaml
- id: {카테고리}-{번호}
  rule: ...
  stacks: [...]
```

## ADR Schema

```yaml
- id: ADR-{번호}
  title: ...
  status: adopted | deprecated | superseded
  date: YYYY-MM-DD
  stacks: [...]
  context: |
    ...
  decision: ...
  consequence: ...
```

## ADR Context Quality

ADR의 `context`는 왜 결정이 필요했는지 구체적으로 떠올릴 수 있어야 한다. 어떤 문제가 있었는지, 누가 어디서 고통받았는지, 어떤 시도가 실패했는지까지 포함한다.
