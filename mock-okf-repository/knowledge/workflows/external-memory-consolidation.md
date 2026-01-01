---
type: Workflow
title: External Memory Capture and Consolidation
description: 継続作業に必要な固有Stateと重要EventをMemoryへ保持し、再利用可能な部分だけをKnowledgeへ統合する外部メモリー運用。
tags: [memory, knowledge-management, consolidation, okf, workflow]
status: stable
generated: { by: chatgpt/gpt-5.6-sol, at: 2026-01-01T07:20:00+09:00 }
---

# Goal

継続する会話・作業・プロジェクトで必要な現在状態をMemoryへ保持し、そこから得られた再利用可能な知識をKnowledgeへ整理する。

このrepositoryでは2つのOKF Bundleを役割で分離する。

```text
Conversation / Work
  -> memory/state  : current target-specific truth
  -> memory/events : important transitions
  -> knowledge/    : generalized reusable knowledge
```

# Bundle Responsibilities

## memory/state

`Memory State` は各domainの現在状態のSource of Truth。

保持する例:

- 現在の計画・工程
- 実測値・在庫
- 構成・設定
- 採用中の設計
- 制約・未解決事項
- 次回開始点

細かな変化はStateを直接更新し、過去状態はGit履歴から追跡する。

## memory/events

`Memory Event` は現在Stateへ至った重要な節目を保持する。

例:

- 設計方針の採用
- Stage / Phase完了
- 採用案・却下案の確定
- 重要な測定
- 大きな構成変更

Eventは全変更ログではない。独立して参照する価値がある変化だけを残す。

## knowledge

特定の現在状態から切り離しても再利用できる知識を保持する。

例:

- Concept
- Workflow
- Tool Guide
- Reference
- Experiment
- Decision

Memoryから一般化できる知見が得られた場合、一般化した部分だけをKnowledgeへ反映する。

# Capture Decision

情報が得られたら2段階で判断する。

```text
Q1. 次回以降、この対象を正確に継続するために必要か？
    YES -> Memory候補

Q2. 対象固有の条件を除いても別の場面で再利用できるか？
    YES -> Knowledge候補
```

同じ出来事からMemoryとKnowledgeの両方が生まれてよいが、同じ文章を複製しない。

例:

```text
今回のブースの外寸・重量・施工状況
  -> Memory State

重量物を床へ置くときの荷重経路
  -> Knowledge Concept
```

# Real-Time Capture

定期整理だけに依存しない。

現在状態が変わり、次回以降へ影響する場合はその時点でStateを更新する。

Stateだけ更新する例:

- Progress更新
- 実測値追加
- 在庫数変更
- 次作業変更

EventとStateを両方更新する例:

- 設計方針確定
- Phase完了
- 重要な検証結果確定
- 大きな構成変更

# Consolidation

定期整理ではEventだけでなく、最近更新されたStateも確認する。

1. `memory/index.md` と関連indexを確認する。
2. 最近のEventを確認する。
3. 最近更新されたStateを確認する。
4. Stateが現在の正確な原本か確認する。
5. 古い値、重複、矛盾を整理する。
6. 数値・順序・評価軸・制約を不用意に一般化しない。
7. 不明事項を推測で補完しない。
8. 独立して残す価値がある節目だけEvent化する。
9. 再利用可能な原則・手順・検証知識が得られていないか確認する。
10. 明確な場合だけKnowledgeへ統合する。
11. 必要なindexを更新する。
12. Bundle全体に意味のある変更だけlogへ記録する。

新しい情報がなく、StateもKnowledgeも正しければ変更しない。

# Exactness of State

Memory Stateでは短さより継続精度を優先する。

寸法、閾値、順序、評価基準、在庫、構成、Progress、制約など、次回再開に必要な具体値を失わない。

```text
Exact State:
External width = 1760 mm

Too generalized:
Small booth
```

# Generalizing into Knowledge

MemoryからKnowledgeへ反映するときは、固有値を削るだけではなく、再利用可能な原理・判断方法へ変換する。

```text
Memory:
面材施工後にフレームの変形が小さくなった

Knowledge:
面材フレームは、面材・留め具・枠材・接合部を
含む構面として変形を評価する
```

次のような場合にKnowledge化を検討する。

- 別の対象でも使える原則
- 再利用可能な確認手順
- 繰り返し使える計算方法
- 外部仕様を確認するReference
- 再現可能なExperiment
- 将来の設計判断に使えるDecision

一般化する価値がない情報を無理にKnowledge化しない。

# Update Instead of Duplicate

Memoryでは、同じdomainの現在状態を既存Stateへ統合する。

Knowledgeでは、同じ概念が存在する場合は既存Conceptへの統合を優先する。

新規Conceptは、既存Conceptへ入れると責務が曖昧になる場合に作成する。

# Git as History

```text
Current State = 今どうなっているか
Memory Event  = 重要な節目
Git History   = 細かな変更と過去状態
```

Git履歴を利用できる場合、State本文へ過去の全状態を残し続ける必要はない。

# Conflict Resolution

- 現在の対象固有状態 -> `memory/state/`
- 重要な状態遷移 -> `memory/events/`
- 一般化された知識 -> `knowledge/`
- 実コード -> 対象repository
- 実設定 -> 対象system
- 外部仕様・法令 -> 公式一次情報

新しく確認された現在値とStateが矛盾する場合はStateを更新する。遷移自体に価値がある場合はEventも追加する。

# Retention

保存しないもの:

- パスワード、APIキー、アクセストークン
- 秘密鍵、回復コード
- Cookie・セッション情報
- その他の認証情報
- 継続に不要な生の会話全文
- 次回以降に影響しない細部
- 根拠のない推測を確定事項として扱ったもの
- 保存・共有する権限のない情報

現在の継続・判断・再利用に不要で、Git履歴から追跡可能な古い重複情報は統合・削除を検討する。

# Completion Check

## Memory

- Stateは現在の正確な原本か
- 重要な具体値を失っていないか
- 不明事項を推測していないか
- 同じ現在状態を重複保存していないか
- Eventを細かな変更ログとして増やしていないか
- 次回開始点が分かるか

## Knowledge

- 対象固有情報をそのままコピーしていないか
- 再利用価値が明確か
- 既存Conceptへ統合できないか
- 事実・解釈・仮説・Experimentを区別しているか
- 外部依存情報に必要なsourcesがあるか
- 時間依存性がある場合はstale_afterを検討したか

## Repository

- 関連indexから到達できるか
- 壊れたリンクがないか
- logがGit履歴の複製になっていないか
- 現在不要な重複文書が残っていないか

# Related

- [Knowledge Capture Workflow](knowledge-capture.md)
- [Resume Long-running Work from External State](resume-long-running-work.md)
