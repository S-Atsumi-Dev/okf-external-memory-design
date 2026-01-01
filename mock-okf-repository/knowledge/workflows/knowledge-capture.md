---
type: Workflow
title: Knowledge Capture Workflow
description: 会話・調査・検証結果を、固有のOKF Memoryと一般化されたOKF Knowledgeへ適切に振り分け、重複を抑えて維持する標準手順。
tags: [knowledge-capture, memory, okf, workflow]
status: stable
generated: { by: chatgpt/gpt-5.6-sol, at: 2026-01-01T07:00:00+09:00 }
---

# Goal

会話・作業・調査・検証で得た情報を、「一般知識ではないから保存しない」と判断するのではなく、用途に応じて2つのOKF Bundleへ振り分ける。

- 利用者・対象環境・継続テーマ固有の情報 -> `memory/`
- 固有の現在状態を越えて再利用できる一般化知識 -> `knowledge/`

生の会話ログや作業履歴そのものを蓄積することは目的としない。

Gitのcommit履歴を過去状態の追跡手段として利用し、現在のリポジトリには今後の継続・判断・再利用に必要な情報を優先して残す。

# First Routing Decision

情報が得られた場合、まず次を判断する。

1. **将来の会話や作業で、この情報を知っている方が正確・効率的に継続できるか？**
   - YES -> Memory保存候補。

2. **さらに、利用者・対象環境・現在状態を越えて別の作業でも再利用できるか？**
   - YES -> Knowledge保存候補。

同じ情報からMemoryとKnowledgeの両方が生まれてよい。

ただし、その場合も同じ文章を単純複製するのではなく、

```text
固有の現在状態
    ↓
Memory

そこから得られた一般原則・手順・知見
    ↓
Knowledge
```

のように役割を分ける。

詳細な整理・統合については `external-memory-consolidation.md` を参照する。

# Memory Capture Procedure

## State

現在状態を保持する必要がある場合は、既存の `Memory State` を更新する。

適切なStateが存在しない場合のみ、新規作成を検討する。

Memory Stateでは原則として以下を使用する。

- `type: Memory State`
- 用途に応じた適切な `scope`
- `domain`
- `title`
- `description`
- `tags`
- `status`
- 必要に応じて `updated_at`
- 必要に応じて `generated`

以下のような情報を、将来正確に再利用できる粒度で保持する。

- 現在の計画
- 現在位置・進捗
- 数値
- 順序
- 評価軸
- 構成・設定
- 所有状況
- 制約
- 採用中の設計
- 次回開始点
- 未完了事項

短くすることを優先して、重要な情報を一般化・省略しない。

不明な情報を推測で補完しない。

同じdomainの現在状態を複数Stateへ重複して保持しない。

## Event

重要な遷移そのものを独立して残す価値がある場合だけ `Memory Event` を追加する。

Memory Eventでは原則として以下を使用する。

- `type: Memory Event`
- 用途に応じた適切な `scope`
- `domain`
- `occurred_at`
- `captured_at`
- `title`
- `description`
- `tags`
- `status`
- 必要に応じて `generated`

Event候補:

- 計画・カリキュラム変更
- Stage・工程完了
- 採用案・却下案の確定
- 重要な測定・検証
- 大きな進捗
- 設計・運用方針変更
- 現在Stateがそうなった理由を後から理解するために重要な変化

細かなState変更ごとにEventを作らない。

変更履歴そのものはGitでも追跡できるため、単独で参照する価値が低いEventを増やし続けない。

既存Eventの内容が現在StateやKnowledgeへ十分統合され、独立して保持する価値がなくなった場合は、定期整理時に統合・削除を検討できる。

# Knowledge Capture Procedure

一般化された長期知識へ反映する場合は、次の順で処理する。

1. `knowledge/index.md` を確認する。
2. 該当カテゴリの `index.md` を確認する。
3. 同じテーマまたは近いテーマの既存Conceptが存在しないか確認する。
4. 新規作成より既存Conceptへの統合を優先する。
5. 事実、解釈、仮説、実験観測を区別する。
6. 内容に適した `type` を選択する。
7. OKF Frontmatterを付与する。
8. 外部依存情報には必要に応じて `sources` を記録する。
9. 前提・制約・適用条件を本文へ含める。
10. 関連する `index.md` を更新する。
11. Bundle全体に意味のある変更の場合のみ `knowledge/log.md` を更新する。

## Knowledge Type

代表的なType:

- `Concept`
- `Workflow`
- `Tool Guide`
- `Experiment`
- `Reference`
- `Decision`

内容により、より適切な自己説明的Typeがある場合は、それを使用してよい。

## Facts and Uncertainty

Knowledge内では、少なくとも以下を混同しない。

```text
確認済みの事実
解釈
仮説
実験で観測した結果
未確認事項
```

未確定の知識を確定事項として固定しない。

必要に応じて、

- `type: Experiment`
- `status: draft`
- `stale_after`

などを使用する。

# Prefer Integration Over Creation

Knowledgeの追加では、ファイル数を増やすこと自体を成果としない。

同じテーマに関するKnowledgeが既に存在する場合は、原則として既存Conceptへ統合する。

特に以下の場合は統合を優先する。

- 内容の大部分が既存Conceptと重複する
- 既存Conceptの新しい事例・補足として表現できる
- 同じ原則を別ファイルで言い換えているだけ
- 既存Workflowの一工程として整理できる
- 新しい情報によって既存Knowledgeを更新できる

新規Conceptを作成するのは、既存Conceptへ統合すると責務や意味が不明確になる場合に限定する。

# Consolidation and Repository Size

Git履歴から過去の内容を確認できるため、Knowledgeファイルへ過去のすべての版や重複した説明を保持し続ける必要はない。

Knowledge Capture時にも、追加だけでなく既存情報の整理を検討する。

確認項目:

- 同じテーマのConceptが複数存在していないか
- 同じ説明が複数ファイルへコピーされていないか
- 新しい知識によって完全に置き換えられた旧Knowledgeが残っていないか
- `deprecated` 文書を現在も保持する必要があるか
- 過去経緯だけを残すために本文が肥大化していないか
- indexに重複・旧リンクが残っていないか
- `knowledge/log.md` がGit履歴の複製になっていないか

古いKnowledgeが新しいConceptへ完全に統合され、現在の参照価値がなく、必要ならGit履歴から復元できる場合は、旧文書の削除を検討する。

ただし、以下は無理に統合しない。

- 責務が異なるConcept
- 独立して再利用する価値があるWorkflow
- 再現条件を保持する必要があるExperiment
- 意思決定理由自体に価値があるDecision
- 外部仕様の独立したReference

判断基準は、

「この文書を独立して残すことで、将来の検索・判断・再利用が明確に改善するか」

とする。

NOなら既存Conceptへの統合を優先する。

# Capture Timing and Context

会話履歴や過去の作業履歴は、関連する文脈を見つけるための補助情報として使用する。

完全な履歴を常に取得できることを前提にしない。

重要な現在状態は、明確になった時点でMemoryへ外部化してよい。

一般化知識についても、十分に確定し再利用価値が明確になった時点でKnowledgeへ保存してよい。

役割は次のように分ける。

```text
Conversation / Work History
    = 文脈探索の補助

Current Interaction
    = 状態・知識が発生する場所

Memory State
    = 固有の現在状態

Memory Event
    = 独立して残す価値がある重要な遷移

Knowledge
    = 一般化された再利用知識

Git History
    = ファイル変更・過去状態の履歴
```

# Update Instead of Duplicate

- Memory StateはdomainごとのCurrent Source of Truthを優先する。
- 同じ現在状態を複数Stateへ重複させない。
- 細かなState変更をEventへ複製しない。
- Knowledgeは既存Conceptの更新・統合を優先する。
- Git履歴で追跡可能な旧版を、単に履歴保存のためだけに現行ファイルへ残さない。
- 未確定な一般知識は `Experiment` や `status: draft` を利用する。

# Index and Log

Conceptを追加・統合・削除・移動した場合は、関連する `index.md` を現在の構成へ合わせる。

Indexは、現在利用可能なConceptへ到達するための入口として維持する。

`log.md` はGit commit履歴の複製として使用しない。

以下のようなBundle全体に意味のある変更のみ記録を検討する。

- 大きな構造変更
- カテゴリ再編
- 複数Conceptの統合
- 運用ルール変更

細かなConcept更新はGit履歴へ委ねてよい。

# What Not to Capture

以下は保存しない。

- パスワード
- APIキー
- アクセストークン
- 秘密鍵
- 回復コード
- Cookie・セッション情報
- その他の認証情報
- 継続に不要な生の会話全文
- 次回以降に影響しない雑談
- 根拠のない推測を確定事項として固定したもの
- 外部文書の巨大な全文コピー
- 継続や判断に不要な機微情報
- 保存・共有する権限のない第三者または組織の情報

# Completion Check

## Memory

- 非予約Markdownに `type` がある
- State / Eventの役割が明確
- 適切な `scope` / `domain` がある
- 数値・順序・評価軸等を勝手に一般化していない
- 不明情報を推測していない
- 同一domainのStateを不要に重複させていない
- 必要なindexから到達できる
- 不要なEventを増やしていない

## Knowledge

- 非予約Markdownに `type` がある
- 既存Conceptへの統合を先に検討した
- 重複Conceptを不要に作成していない
- 事実・解釈・仮説・実験結果が区別されている
- 必要な外部依存情報に出典がある
- 前提・制約・適用条件が明確
- 関連indexが現在の構成と一致している
- 不要になった旧Knowledgeを保持し続けていない
- `knowledge/log.md` に細かなGit履歴を重複させていない
