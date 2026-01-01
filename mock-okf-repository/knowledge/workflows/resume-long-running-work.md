---
type: Workflow
title: Resume Long-running Work from External State
description: 外部Stateから継続作業を再開するWorkflow。
tags: [workflow, state, context]
status: stable
generated: { by: chatgpt/gpt-5.6-sol, at: 2026-01-01T07:10:00+09:00 }
---

# Goal

長期間継続するテーマを、過去の会話全文に依存せず現在Stateから再開する。

# Flow

```text
Receive task
  ↓
Identify domain
  ↓
Read relevant State
  ↓
Extract current position / constraints / next action
  ↓
Build minimal context
  ↓
Continue task
  ↓
Update State when current truth changes
```

# Principle

会話履歴を唯一のsource of truthにせず、継続に必要な現在状態を外部化する。

# Constraint

Stateが実環境や一次情報と矛盾する場合は、Stateを無条件に優先しない。
