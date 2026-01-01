---
okf_version: "0.2"
---

# Memory Bundle

このディレクトリは、継続テーマの現在状態と重要な状態遷移を保持するOKF Bundleです。

## Structure

- [State Index](state/index.md) - 現在状態の原本
- [Event Index](events/index.md) - 重要な状態変化・節目

## Read Order

継続テーマでは原則として次の順で参照します。

```text
memory/index.md
  ↓
memory/state/index.md
  ↓
関連するMemory State
  ↓
必要な場合のみMemory Event
```
