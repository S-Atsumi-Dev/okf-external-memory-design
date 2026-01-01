# 質問ごとの外部メモリー参照と応答例

この文書では、`mock-okf-repository/` の防音ブースStateを現在状態として、ユーザーから異なる種類の質問・報告が来たときに、どの外部メモリーを参照し、どのような回答や更新につなげるかを示します。

ここで示すのは内部推論の記録ではなく、**参照対象・利用情報・更新先の設計例**です。

回答文そのものを固定することが目的ではありません。同じStateから、質問に必要な情報だけを選んで回答することを重視します。

## 現在状態

例で使用する現在状態は次の通りです。

```text
Project
  Planning                  completed
  Frame                     completed
  Panels and Insulation     not started
  Door and Sealing          not started
  Ventilation               not started
  Verification              not started

Construction
  Phase 2 Frame             completed
  Phase 3                   ready to start

Materials
  Phase 3開始分             secured
  Project全体               一部不足あり
```

主な参照先:

- [Project Plan](../mock-okf-repository/memory/state/soundproof-booth-plan.md)
- [Construction Control](../mock-okf-repository/memory/state/soundproof-booth-construction.md)
- [Material Inventory](../mock-okf-repository/memory/state/soundproof-booth-material-inventory.md)
- [Structural Design](../mock-okf-repository/memory/state/soundproof-booth-structural-design.md)
- [Frame Phase Completed Event](../mock-okf-repository/memory/events/2026/01/2026-01-01-soundproof-booth-frame-phase-completed.md)
- [Floor Load and Load Path](../mock-okf-repository/knowledge/concepts/floor-load-and-load-path.md)
- [Sheathed Frame Stiffness](../mock-okf-repository/knowledge/concepts/sheathed-frame-stiffness.md)

## 基本方針

質問ごとに必要な範囲だけを読みます。

```text
User message
    ↓
対象domainと質問の種類を特定
    ↓
Index
    ↓
必要なState
    ↓
必要な場合だけ Event / Knowledge / Reference
    ↓
回答
    ↓
新しい事実が確定した場合だけ書き戻し
```

たとえば「合板は足りる？」という質問に、過去Eventや床荷重Knowledgeまで読む必要はありません。

一方、「なぜ合板を使う設計なのか？」なら、現在の設計Stateだけでなく、その判断に使ったKnowledgeまで辿ります。

---

## 例1: 「今どうなっていますか？」

### User

```text
現在の状況を教えてください
```

### Read

```text
memory/index.md
  ↓
memory/state/index.md
  ↓
soundproof-booth-plan.md
soundproof-booth-construction.md
soundproof-booth-material-inventory.md
```

EventやKnowledgeは通常不要です。

### 利用する情報

- Phase 2 Frameは完了
- Phase 3 Panels and Insulationは未着手
- Phase 3開始条件は満たしている
- 材料不足による開始Blockerはない
- Project完了までには追加購入が必要

### 想定回答

> 現在はPhase 2のFrame工程まで完了しています。次はPhase 3のPanels and Insulationです。
>
> Phase 3開始に必要な合板8枚、吸音材8pack、石膏ボード6枚、Panel screw 2箱、Sealant 2本は確保済みなので、現時点で開始Blockerはありません。
>
> Project全体では合板4枚、石膏ボード6枚、Panel screw 1箱などが不足しています。次は換気開口位置を面材へマーキングし、外側合板の施工順を確認してPhase 3へ進めます。

### Write

なし。

質問された時点では新しい事実が発生していないため、Memoryは変更しません。

---

## 例2: 「作業を進めます」

### User

```text
作業を進めます
```

### Read

```text
Project Plan
  ↓
Construction Control
  ↓
Material Inventory
```

### 利用する情報

Construction StateのNext Actions:

1. 材料在庫確認
2. 換気開口位置のマーキング
3. 外側合板の施工順確認
4. Phase 3開始

InventoryではPhase 3開始分が確保済み。

### 想定回答

> Phase 3へ進められます。
>
> 最初に前壁下部の吸気・後壁上部の排気位置を面材へマーキングし、その後、外側合板の施工順を確認して面材施工へ入る流れです。
>
> Phase 3開始分の材料は確保済みなので、材料待ちで止める必要はありません。施工後に使用した合板・吸音材・固定具の数量と、完了した面を教えてもらえればConstruction StateとInventory Stateへ反映できます。

### Write

この発話だけでは「作業を開始する意思」が示されただけで、実際の施工結果はまだ確定していません。

原則としてStateはまだ変更しません。

---

## 例3: 「合板は足りますか？」

### User

```text
合板は足りますか？
```

### Read

```text
memory/state/index.md
  ↓
soundproof-booth-material-inventory.md
```

必要なら現在Phase確認のためConstruction Stateを追加で読む。

### 利用する情報

Structural plywood 12 mm:

```text
Required              12 sheets
Purchased              8 sheets
Consumed               0 sheets
Available              8 sheets
Next Phase Allocation  8 sheets
Shortage               4 sheets
```

### 想定回答

> Phase 3を開始する分には足ります。現在8枚あり、Phase 3開始時の割当も8枚です。
>
> ただしProject全体では12枚必要なので、完了までに4枚不足します。つまり「今すぐ施工開始は可能だが、後で4枚追加購入が必要」という状態です。

### Write

なし。

---

## 例4: 「次に何を買えばいいですか？」

### User

```text
次に買う材料を教えてください
```

### Read

```text
Material Inventory
  ↓
Procurement Queue
```

### 想定回答

> 優先度が高いのは、構造用合板12 mmを4枚、石膏ボード12.5 mmを6枚です。どちらもPhase 3完了前に追加が必要です。
>
> その次がPanel screw 32 mmを1箱、Sealantを4本、内装仕上げパネルを4枚です。Door seal 8 mはPhase 4までに用意すればよいので優先度は低めです。
>
> 現在の在庫だけでPhase 3の開始自体は可能です。

### Write

なし。

---

## 例5: 「なぜ合板を使う設計なのですか？」

### User

```text
なぜフレームに合板を張る設計なのですか？
```

### Read

```text
Structural Design State
  ↓
Applied Knowledge
  ↓
Sheathed Frame Stiffness
  ↓
必要ならReference
```

### 利用する情報

Structural Design:

- 木枠単体を完成構造として扱わない
- 合板を面材として固定する
- 水平変形を抑える構面として利用する

Knowledge:

- 面材が面内せん断を負担する
- 留め具を通じて枠材へ力を伝える
- 性能は面材だけでなく留め具・枠材・接合部にも依存する

### 想定回答

> 木枠だけでは接合部の回転や滑りによって矩形が変形しやすいためです。
>
> この設計では合板を単なる仕上げ材ではなく、面内せん断を負担する面材として扱っています。合板を枠へ連続的に固定することで、合板 → 留め具 → 枠材へ水平力を伝え、フレーム全体の変形を抑える考え方です。
>
> そのため性能は合板の厚さだけではなく、留め具の種類・間隔、面材端部の支持、開口位置などにも影響されます。

### Write

なし。

現在採用済みの設計理由を説明しただけなので、Stateは変化しません。

---

## 例6: 「Frame工程はどう終わりましたか？」

### User

```text
Frame工程が終わった時の状態を教えてください
```

### Read

現在状態だけではなく履歴上の節目を尋ねているためEventを読む。

```text
memory/events/index.md
  ↓
soundproof-booth-frame-phase-completed.md
  ↓
必要ならConstruction State
```

### 想定回答

> Phase 2 Frame完了時点では、床・壁4面・ドア開口補強・天井フレーム・主要寸法の実測・接合部確認まで完了しています。
>
> 最終実測は幅1759 mm、奥行1279 mm、高さ2099 mm、ドア開口648 × 1748 mmで、設定した許容範囲内でした。
>
> この完了を節目としてMemory Eventへ残し、その後の現在StateはPhase 3開始可能へ更新されています。

### Write

なし。

---

## 例7: 「合板を4枚買いました」

### User

```text
構造用合板を4枚追加で買いました
```

### Read

```text
Material Inventory
```

既存値を取得してから差分を適用する。

### Before

```text
Purchased  8
Consumed   0
Available  8
Shortage   4
```

### Write

Material Inventoryを更新する。

```text
Purchased  12
Consumed    0
Available  12
Shortage    0
Status      sufficient
```

Procurement Queueから合板のreorderも解消する。

### Event

作らない。

通常の在庫数量更新であり、独立した節目として残す価値は低いため。

### 想定回答

> 構造用合板4枚の追加購入を在庫へ反映します。
>
> これで購入済み12枚、使用済み0枚、利用可能12枚となり、Project全体の合板不足は0枚になります。合板の再発注項目も解消できます。

実際に永続先への書き込みが成功した場合だけ、更新済みとして扱います。

---

## 例8: 「外側合板を4枚張りました」

### User

```text
外側の合板を4枚張りました
```

### Read

```text
Construction Control
Material Inventory
```

### Write

2つのStateへ役割を分けて反映する。

Construction State:

```text
外側合板施工:
  not started
    ↓
  in progress

施工済み:
  4 sheets
```

Material Inventory:

```text
Consumed +4
Available -4
```

### Event

原則作らない。

「合板4枚施工」は工程途中の通常進捗であり、現在値はState、細かな履歴はGitで追跡できるため。

### 想定回答

> 外側合板4枚の施工を進捗へ反映します。
>
> Construction Stateでは外側合板施工を進行中として扱い、Inventory Stateでは構造用合板の使用数を4枚増やします。
>
> Phase 3全体の完了条件にはまだ達していないため、この時点ではMemory Eventは追加しません。

---

## 例9: 「Phase 3が終わりました」

### User

```text
吸音材と面材施工が全部終わってPhase 3完了です
```

### Read

```text
Project Plan
Construction Control
Material Inventory
```

必要な完了条件と現在状態を確認する。

### Write

```text
Construction State
  Phase 3 -> completed

Project Plan
  Panels and Insulation -> completed
  Current Position -> Phase 4

Material Inventory
  実際の使用数量を反映

Memory Event
  Phase 3 completed
```

### Event

追加する。

Phase完了は、次回開始条件がPhase 4へ切り替わる重要な状態遷移だからです。

### 想定回答

> Phase 3完了として整理します。
>
> Project PlanとConstruction StateをPhase 3 completedへ更新し、次の開始位置をPhase 4 Door and Sealingへ変更します。使用した材料数量もInventoryへ反映します。
>
> 工程単位の完了なので、Phase 3完了はMemory Eventとしても残す対象です。

実際の使用数量が発話だけでは分からない場合、数量は推測せず未確定のまま扱います。

---

## 例10: 「合板をOSBへ変えたい」

### User

```text
合板ではなくOSBに変更したいです
```

### Read

これは単なる施工進捗ではなく設計変更候補。

```text
Structural Design State
  ↓
Project Plan
  ↓
Material Inventory
  ↓
Sheathed Frame Stiffness
  ↓
必要な外部仕様・Reference
```

### 処理

「変更したい」は採用確定とは限らないため、この時点では既存設計を即座に書き換えない。

まず、

- 現在の合板を採用した理由
- 必要な面材性能
- 留め具条件
- 重量差
- 在庫への影響
- 法令・仕様上の扱い

を確認する。

### 想定回答

> 現在の設計では12 mm構造用合板を、フレームの水平変形を抑える構面の一部として採用しています。
>
> OSBへ変更する場合は、単に同じ厚さへ置き換えるのではなく、面材仕様、留め具、間隔、端部支持、重量への影響を再確認する必要があります。
>
> 変更案を採用する場合はStructural Design、Project Plan、Material Inventoryを更新し、設計方針変更としてEventを残す対象になります。

### Write

提案段階では原則なし。

採用が確定した時点で書き戻す。

---

## 例11: 「床に500 kg載せても大丈夫ですか？」

### User

```text
この設計なら500 kgでも床は大丈夫ですか？
```

### Read

```text
Structural Design State
  ↓
Floor Load and Load Path
  ↓
Japanese Building Structural Load References
```

### 利用する情報

現在の設計概算:

```text
Design total ≈ 500 kg
Projected area = 2.2528 m²
Average ≈ 222 kg/m²
```

ただしStructural Designには、

- 既存床内部構成は未確認
- 根太方向は未確認
- 根太・梁寸法とスパンは未確認
- 平均面荷重だけで安全判定しない

と記録されている。

### stale_after

外部仕様を使うReferenceが `stale_after` を過ぎている場合、現在の法令・仕様を再確認してから利用する。

### 想定回答

> 現在の外部メモリーだけでは「500 kgでも安全」とは判定できません。
>
> 設計上は総質量500 kgを想定し、単純平均では約222 kg/m²ですが、この値だけでは床の安全性を判断しない方針です。現在は根太方向、根太・梁の寸法とスパン、既存床構成が未確認です。
>
> したがって、床構造と荷重経路を確認してから判断する必要があります。法令値についても「載せてよい最大重量」として直接使うことはできません。

### Write

追加調査で床構成が確認できた場合、その対象固有結果をStructural Design Stateへ反映する。

一般化可能な知見が新たに得られた場合だけKnowledgeも更新する。

---

## 例12: 「前回から何が変わりましたか？」

### User

```text
前回から何が変わりましたか？
```

### Read

「現在状態」ではなく「差分」を求めているため、Stateだけでは不十分。

```text
Current State
  +
Recent Memory Events
  +
必要な範囲のGit History
```

### 想定回答

> 前回の主要な節目はPhase 2 Frame完了です。
>
> 壁4面、ドア開口補強、天井フレーム、主要寸法確認まで完了し、現在位置がPhase 3開始可能へ変わっています。
>
> 細かな途中更新まで必要であれば、Git履歴から「左右壁完了 → 壁4面完了 → 天井完了 → Frame完了」の順に差分を追えます。

### Write

なし。

---

# 質問の種類と参照先

| User intent | 最初に読むもの | 必要に応じて追加で読むもの | 主なWrite先 |
|---|---|---|---|
| 現在の状況 | Project / Construction State | Inventory | なし |
| 次に何をするか | Construction State | Plan / Inventory | なし |
| 材料が足りるか | Inventory State | Construction State | なし |
| 材料購入報告 | Inventory State | - | Inventory State |
| 施工進捗報告 | Construction State | Inventory | Construction + Inventory |
| Phase完了報告 | Construction + Plan | Inventory | State + Event |
| 設計理由 | Structural Design | Knowledge / Reference | なし |
| 設計変更相談 | Structural Design | Knowledge / Reference / Inventory | 採用後に複数State + Event |
| 過去の節目 | Event Index | Current State | なし |
| 前回との差分 | Current State | Event / Git History | なし |
| 外部仕様依存の質問 | Knowledge / Reference | 最新一次情報 | 必要ならKnowledge更新 |

# Read範囲を広げる条件

最初からすべての外部メモリーを読むのではなく、質問に応じて段階的に範囲を広げます。

```text
Current stateだけで答えられる
  -> Stateで停止

なぜその状態なのか必要
  -> Eventを追加

設計理由・一般原則が必要
  -> Knowledgeを追加

現在の外部仕様が結論に影響
  -> Referenceのfreshness確認
  -> 必要なら一次情報を再確認
```

# Write判断

ユーザーの発話が質問だけなら、通常はWriteしません。

```text
質問
  -> Read
  -> Answer
  -> No Write
```

新しい事実が確定した場合だけWriteを検討します。

```text
新しい事実
  ↓
次回も必要?
  ├─ No  -> 保存しない
  └─ Yes -> State更新
              ↓
       重要な状態遷移?
          ├─ Yes -> Event
          └─ No  -> Stateのみ
              ↓
       一般化して再利用可能?
          └─ Yes -> Knowledgeへ統合
```

この分離により、単なる質問でMemoryを不必要に変更せず、進捗・在庫・設計変更など、次回へ影響する情報だけを永続化できます。
