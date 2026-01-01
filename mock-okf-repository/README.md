# OKF Repository Example

LLM / Agentとの継続作業で利用する、**固有の永続メモリー**と**一般化された再利用知識**を、Open Knowledge Format（OKF）v0.2で管理する構成例です。

## 目的

- 過去の会話をすべて再走査しなくても、現在状態から作業を再開できるようにする
- 利用者・対象環境固有の進捗、構成、判断、次回開始点を永続化する
- 一般化できる知見は別のKnowledge Bundleへ整理し、別の会話や作業でも再利用する
- Git履歴で状態や知識の変化を追跡する
- 生の会話履歴ではなく、AIが再利用しやすい構造化された情報を維持する
- Git履歴へ委ねられる古い状態や重複情報は統合し、リポジトリの肥大化を抑える

## 構成

```text
.
├── README.md
├── AGENTS.md
├── memory/                 # OKF v0.2: 固有の永続メモリー
│   ├── index.md
│   ├── log.md
│   ├── state/
│   │   ├── index.md
│   │   └── learning-topic-a.md
│   └── events/
│       ├── index.md
│       └── YYYY/MM/*.md
└── knowledge/              # OKF v0.2: 一般化された再利用知識
    ├── index.md
    ├── log.md
    ├── concepts/
    ├── tools/
    ├── workflows/
    ├── references/
    ├── experiments/
    └── decisions/
```

このリポジトリには2つのOKF Bundleがあります。

## 日時について

この構成内の日時は実際の作成・作業日時ではなく、`2026-01-01` を起点にした説明用の時系列です。工程の進行を示す場合は、前の作業からおおむね1時間ずつ進むように設定しています。

## memory/

「将来の会話や作業で知っている方が、対象テーマを正確に継続できる情報」を保存します。

### Memory State

現在状態のSource of Truthです。

例:

- 現在の計画
- 作業・学習の現在位置
- 数値・在庫
- 構成・設定
- 採用中の設計
- 判断基準・制約
- 次回開始点

頻繁に変わる現在値や細かな進捗はStateを直接更新します。

### Memory Event

独立した履歴として残す価値が高い重要な状態遷移を保存します。

例:

- 計画変更
- Stage・工程完了
- 採用案の確定・却下
- 重要な測定・検証
- 大きな進捗
- 設計・運用方針変更

すべての変更をEvent化する必要はありません。細かな変更履歴や過去のStateはGit履歴から確認できます。

## knowledge/

利用者・対象環境・現在状態から切り離しても再利用できる一般化知識を保存します。

例:

- 技術Concept
- Tool Guide
- 再利用可能なWorkflow
- Experiment
- Reference
- Decision

新しいKnowledgeを追加する前に、既存Conceptへ統合できないか確認します。

重複・旧版を無制限に残さず、現在の再利用価値がなくGit履歴から追跡できるものは統合・整理対象とします。

## 保存判断

情報が得られた場合は、次の順で判断します。

1. **将来の会話・作業で知っている方が正確に継続できるか？**
   - YES → `memory/` への保存候補
2. **利用者・対象環境・現在状態を除いても再利用できるか？**
   - YES → 一般化した部分を `knowledge/` へ反映

同じ情報からMemoryとKnowledgeの両方が生まれても構いませんが、同じ内容を単純複製せず役割を分けます。

## Read

継続テーマでは、原則として次の順で必要な情報だけを参照します。

```text
memory/index.md
    ↓
memory/state/index.md
    ↓
関連するMemory State
    ↓
必要な場合のみEvent / Knowledge
```

一般化された技術知識が必要な場合は、`knowledge/index.md` から関連Conceptだけを辿ります。

リポジトリ全体を毎回Contextへ投入することは想定していません。

## Write

既存文書を更新する場合は、現在内容を確認してから変更します。

```text
現在の文書を取得
    ↓
新しい情報と比較
    ↓
矛盾・変更点を確認
    ↓
State / Event / Knowledgeへ反映
    ↓
必要なindexを更新
    ↓
Git commit
```

## 整理・統合

定期整理では、追加だけでなく統合・削除も検討します。

- Stateが現在の正確な原本か確認
- 重複Stateを統合
- 独立価値の低いEventを整理
- 重複Knowledgeを既存Conceptへ統合
- 旧版・supersededなKnowledgeを整理
- indexを現在構成へ合わせる
- 細かな変更履歴はGitへ委ねる

現在の継続・判断・再利用に必要な情報まで削除しないことを前提とします。

## 主なファイル

- [Agent Instructions](AGENTS.md)
- [Memory Bundle](memory/index.md)
- [Memory State](memory/state/learning-topic-a.md)
- [Memory Event](memory/events/2026/01/2026-01-01-learning-topic-a-stage1-completed.md)
- [Knowledge Bundle](knowledge/index.md)
- [Knowledge Capture Workflow](knowledge/workflows/knowledge-capture.md)
- [Long-running Work Workflow](knowledge/workflows/resume-long-running-work.md)

## 保存しないもの

- パスワード
- APIキー
- アクセストークン
- 秘密鍵
- 回復コード
- Cookie・セッション情報
- その他の認証情報
- 生の会話全文の無差別コピー
- 次回以降に影響しない雑談や細部
- 根拠のない推測を確定事項として固定したもの
- 継続や判断に不要な機微情報
- 保存・共有する権限のない第三者・組織の情報
