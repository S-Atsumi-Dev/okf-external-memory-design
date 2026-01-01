# Agent Instructions

このリポジトリは、AI支援作業で継続利用する知識・外部メモリー基盤として使用する。

## Scope

- `memory/` は、利用者・対象環境固有の永続メモリーと継続中の状態を保持する Open Knowledge Format（OKF）v0.2 Bundleとして扱う。
- `knowledge/` は、固有の現在状態から切り離して再利用できる一般化知識を保持する、別の OKF v0.2 Bundleとして扱う。
- この2つのBundle以外のファイルは、リポジトリ全体の運用ドキュメントとして扱う。
- 継続中のテーマを扱う場合、会話履歴から状態を再構築するより、関連する `memory/state/` のConceptを優先する。
- 一般化されたKnowledgeを新規作成・更新する前に `knowledge/index.md` を読み、必要なリンクだけを辿る。

## Information routing

情報を保存しないと判断する前に、次を確認する。

1. この情報は、将来の会話やAgentセッションで、対象テーマを正確に継続するために役立つか。
   - YESなら、`memory/` OKF Bundleへの保存を検討する。

2. この情報は、現在の利用者・対象環境・特定の継続テーマを越えて再利用できるか。
   - YESなら、一般化した部分を `knowledge/` OKF Bundleにも保存することを検討する。

一般的に役立つ情報であることを、Memoryへ保存する必須条件にしない。

## OKF rules shared by both bundles

1. `index.md` と `log.md` は予約ファイルであり、Concept Documentとして扱わない。
2. 各Bundle内の、それ以外のMarkdownファイルには、空ではない `type` を持つYAML Frontmatterを付ける。
3. `type` には、内容が分かる自己説明的な名称を優先する。未知のtypeでもOKF上は有効とする。
4. 編集時は、生成元が独自に追加した未知のFrontmatter fieldを保持する。
5. 実質的な内容変更を行った場合、可能であれば `generated` に変更主体と時刻を記録する。
6. `verified` は実際に検証した場合のみ使用する。生成しただけでは検証済みとしない。
7. ライフサイクルに応じて `status: draft | stable | deprecated` を使用する。
8. 将来の再確認が必要な情報には `stale_after` を使用する。
9. 外部資料に基づくConceptでは、`sources` と安定したsource IDを使用する。
10. 最も近い関連 `index.md` を維持する。Bundle全体に関わる重要な変更は、そのBundleの `log.md` に記録する。

## Memory bundle rules

### Memory State

- `type: Memory State` を使用する。
- 継続中のdomainの現在状態の原本を `memory/state/<domain>.md` に保存する。
- 利用用途に応じた適切な `scope` を使用する。
- 必要な場合、正確な計画、順序、閾値、評価軸、在庫、現在進捗、構成、設定、制約を保持する。
- Stateを短くする目的だけで、重要な詳細を一般化して削除しない。
- 不明な値を推測しない。unknownまたは未解決として扱う。
- 細かな現在値の変更は、別のEventを作成せずStateを直接更新してよい。

### Memory Event

- `type: Memory Event` を使用する。
- 意味のある状態遷移や節目を `memory/events/YYYY/MM/` に保存する。
- Eventは、すべての細かな変更ではなく「どの重要な変化が起きたか」を表す。
- 原則として追記型の履歴として扱う。現在Stateに合わせるために古いEventを書き換えない。
- Event候補には、計画変更、Stage・工程完了、採用・却下された設計、重要な測定・検証、大きな節目、現在Stateがそうなった理由を説明する変化などがある。
- 生の会話全文や、すべての細かな試行をEventへ保存しない。

### Memory consolidation

- 最近のEventだけでなく、最近更新されたState Conceptも確認する。
- 最初にStateを正確な状態へ保つ。
- その後、利用者・対象環境固有の観測から、一般化可能で再利用できる原則が得られたか確認する。
- 得られた場合は、その一般化部分を `knowledge/` に表現する。
- Knowledgeへ一般化したことを理由に、元のStateを削除しない。

## Knowledge bundle rules

1. 重複したConceptを新規作成するより、既存Conceptの更新を優先する。
2. 固有の現在状態ではなく、一般化された再利用可能な知識を保存する。
3. 事実、解釈、仮説、実験結果を区別できる状態に保つ。
4. 内容に応じて、以下のようなConcept Typeを使用する。
   - `Concept`
   - `Tool Guide`
   - `Workflow`
   - `Experiment`
   - `Reference`
   - `Decision`
5. より適切で分かりやすいTypeがある場合、上記のTypeへ無理に当てはめない。

## Startup behavior

既存の継続domainを扱うタスクの場合:

1. `memory/index.md` を読む。
2. `memory/state/index.md` と関連するState Conceptを読む。
3. 履歴や理由が必要な場合のみEventを読む。
4. 一般化されたKnowledgeが必要な場合のみ `knowledge/index.md` を読む。

現在Stateに依存しない一般的な技術・再利用可能な質問では、`knowledge/index.md` から開始する。

どちらのBundleも、デフォルトで全文を読み込まない。

## Safety and privacy

パスワード、APIキー、アクセストークン、秘密鍵、回復コード、Cookie、セッション情報、その他の認証情報や、それらに相当する秘密情報は保存しない。

外部メモリーには、継続性を実質的に改善するために必要な情報だけを保存する。

リポジトリがprivateであっても、不要な機微情報や秘密情報を保存しない。

保存・共有する権限のない第三者または組織の情報は保存しない。

## Change behavior

- Indexやリンクの整合性を保てる範囲で、最小限かつ一貫した変更を行う。
- 矛盾する結論や現在Stateを黙って上書きしない。
- 重要なState変更について、遷移自体を履歴として残す価値がある場合は、新しいEventとして整理する。
- 大量の外部ドキュメントをそのままリポジトリへコピーしない。必要な内容を要約し、可能であれば権威ある参照元へのリンクを保持する。
- 現在の正式な仕様を確認せずに、OKFのversion conventionを変更しない。
- 会話履歴やその他の補助的なMemoryは検索・文脈取得の補助として扱い、完全なSource of Truthであるとはみなさない。
- 実コード、設定、対象システム、公式仕様など、より適切なSource of Truthが存在する場合は、それらとの整合を確認する。

## Read principle

情報量が増えても、リポジトリ全体を無条件にContextへ投入しない。

原則として、

```text
Index
  ↓
関連カテゴリまたはdomain
  ↓
必要なState / Knowledge
  ↓
必要な場合のみEventや追加資料
```

の順で段階的に参照する。

Indexは単なる目次ではなく、必要なContextを選択するための入口として扱う。

## Write principle

既存文書を更新する場合は、現在内容を確認してから変更する。

```text
現在の文書を取得
    ↓
新しい情報と比較
    ↓
矛盾・変更点を確認
    ↓
必要最小限の更新
    ↓
関連indexの整合確認
    ↓
Git commit
```

新しい情報だけを基準に、既存StateやKnowledgeを無条件に上書きしない。
