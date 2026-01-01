---
type: Memory State
title: Soundproof Booth Structural Design
description: 防音ブースの床荷重・荷重経路・フレーム構面について、Knowledgeを適用して確定した設計根拠を保持するState。
tags: [soundproof-booth, structural-design, floor-load, sheathing]
status: stable
scope: project
domain: soundproof-booth-project
updated_at: 2026-01-01T09:05:00+09:00
generated: { by: chatgpt/gpt-5.6-sol, at: 2026-01-01T09:05:00+09:00 }
---

# Purpose

防音ブースの構造設計について、単に採用仕様だけを残すのではなく、

- どのKnowledgeを参照したか
- どの入力条件を使ったか
- 何を確認したか
- どの判断を採用したか
- 何が未確認か

を再確認できる状態で保持する。

施工工程そのものは [Soundproof Booth Construction Control](soundproof-booth-construction.md) を参照する。

# Applied Knowledge

この設計では以下を参照した。

- [Floor Load and Load Path](../../knowledge/concepts/floor-load-and-load-path.md)
- [Sheathed Frame Stiffness](../../knowledge/concepts/sheathed-frame-stiffness.md)
- [DIY Structural Load Check](../../knowledge/workflows/diy-structural-load-check.md)
- [Japanese Building Structural Load References](../../knowledge/references/japan-building-structural-loads.md)

# Design Inputs

## Geometry

| Item | Value |
|---|---:|
| External width | 1760 mm |
| External depth | 1280 mm |
| External height | 2100 mm |
| Target internal width | 1620 mm |
| Target internal depth | 1140 mm |
| Target internal height | 1960 mm |
| Door opening | 650 × 1750 mm |

## Main Materials

設計時に想定する主材料:

- 木製フレーム
- 構造用合板
- 石膏ボード
- 吸音材
- 内装仕上げ材
- ドア
- 換気ダクト・ファン

# Weight Estimation

設計段階では、材料ごとに次の式で質量を概算する。

```text
mass = volume × density
```

板材:

```text
volume = width × height × thickness
```

設計用の概算値:

| Component | Estimated mass |
|---|---:|
| Timber frame | 82 kg |
| Plywood | 96 kg |
| Gypsum board | 142 kg |
| Absorber / insulation | 24 kg |
| Door assembly | 38 kg |
| Ventilation components | 8 kg |
| Other hardware / finish | 20 kg |
| Booth subtotal | 410 kg |
| Occupant allowance | 90 kg |
| Design total | 500 kg |

これらは施工計画用の概算値であり、製品仕様・実測重量が得られた場合は更新する。

# Floor Load Check

## Average Load

外形投影面積:

```text
A = 1.760 × 1.280
  = 2.2528 m²
```

設計総質量500 kgを単純平均した場合:

```text
q_mass = 500 / 2.2528
       ≈ 222 kg/m²
```

力として概算すると:

```text
W = 500 × 9.8
  ≈ 4900 N

q = 4900 / 2.2528
  ≈ 2175 N/m²
```

# Interpretation

この平均値だけでは設置可否を判断しない。

理由:

- 実際の支持は外周フレームへ偏る
- 局所的な集中荷重が生じる可能性がある
- 既存床の根太・梁方向が影響する
- 法令上の積載荷重は単純な許容重量ではない
- 人や機材の位置により反力分布が変わる

したがって、床荷重確認では平均面荷重に加えて荷重経路を確認する。

# Load Path Assumption

想定する荷重経路:

```text
ブース本体・利用者
        ↓
床フレーム
        ↓
既存床仕上げ / 下地
        ↓
根太または床構造
        ↓
梁・壁・柱
        ↓
基礎
```

設計時点では既存床内部の詳細構成は未確認。

# Regulatory Reference

住宅居室等に関する積載荷重は、建築基準法施行令第85条で計算対象ごとに異なる値が定められている。

この設計では、その値を「載せてよい最大重量」として直接使用しない。

法令・公式資料の確認先:

- [Japanese Building Structural Load References](../../knowledge/references/japan-building-structural-loads.md)

# Frame Behavior

## Bare Frame

木枠だけでは、水平力に対して接合部が回転・滑りし、矩形が平行四辺形状へ変形しやすい。

## Sheathed Frame

合板をフレームへ多数の留め具で固定すると、

```text
水平力
  ↓
合板の面内せん断
  ↓
釘・ビス
  ↓
枠材
  ↓
接合部
```

という経路で力を伝えられる。

このため、完成状態では木枠単体ではなく、面材を含む構面として変形を抑える設計とする。

# Simplified Stiffness Model

同じ水平力 `F` に対する変位を比較する場合:

```text
K = F / δ
```

```text
K_bare     = F / δ_bare
K_sheathed = F / δ_sheathed
```

この比較は施工前後の変形傾向を見るために使用できる。

ただし、実際の剛性は次の合計として考える。

```text
δ_total
  ≈ δ_frame
  + δ_panel
  + δ_fastener
  + δ_connection
  + δ_anchor
```

# Adopted Structural Decisions

## DEC-01: Sheathing is part of the structural system

木枠だけを完成構造として扱わない。

合板を面材として固定し、水平変形を抑える構成を採用する。

## DEC-02: Door opening requires reinforcement

ドア開口部は面材が連続しないため、前壁フレーム側で開口補強を行う。

## DEC-03: Average floor load is not sufficient

床への影響は平均kg/m²だけでは判断しない。

接地部、集中荷重、根太・梁方向を確認対象に含める。

## DEC-04: Regulatory values are references, not direct capacity limits

建築基準法上の積載荷重値を、そのまま対象床の許容積載重量として扱わない。

## DEC-05: Actual opening dimensions override nominal door dimensions

ドア製作時は計画寸法より施工後の実測開口寸法を優先する。

# Design Constraints

- 合板はフレームへ連続的に固定する
- 面材継ぎ目は可能な範囲で枠材上に配置する
- ドア開口周辺ではフレーム補強を行う
- 換気開口は構面を過度に分断しない位置とする
- 既存床構造が不明な状態では、平均荷重だけを根拠に安全判定しない

# Verification During Construction

施工時に確認する項目:

- フレーム外寸
- フレーム対角差
- ドア開口実測
- 面材固定状態
- 留め具間隔
- 面材端部の支持
- 開口周辺の補強
- 完成後の変形・ガタつき

# Open Questions

- 既存床の根太方向
- 根太・梁の寸法とスパン
- 合板の最終厚さ
- 留め具の最終種類と間隔
- 実材料から算出した最終重量
- 完成後に簡易水平変形比較を行うか

# Current Design Status

Status: adopted

床荷重は平均値だけで判断せず荷重経路まで確認し、フレームは面材を含む構面として設計する方針を採用済み。

未確認項目は施工・現場確認に合わせて更新する。
