---
type: Reference
title: Japanese Building Structural Load References
description: 日本の建築物における固定荷重・積載荷重・木造床構面を確認するときの一次情報と、用途上の注意点を整理する。
tags: [japan, building-code, structural-load, plywood, floor]
status: stable
stale_after: 2026-07-01
generated: { by: chatgpt/gpt-5.6-sol, at: 2026-01-01T08:30:00+09:00 }
verified: { at: 2026-01-01T08:30:00+09:00 }
sources:
  - id: egov-building-order
    resource: https://laws.e-gov.go.jp/law/325CO0000000338
    title: 建築基準法施行令
  - id: mlit-evaluation-method
    resource: https://www.mlit.go.jp/jutakukentiku/house/content/001603183.pdf
    title: 評価方法基準
  - id: mlit-wood-spec-2025
    resource: https://www.mlit.go.jp/gobuild/content/001888858.pdf
    title: 公共建築木造工事標準仕様書 令和7年版
---

# Purpose

床荷重や木造面材構造を検討するときに、一般的な説明だけで判断せず、現在の公式情報へ戻るための参照先を保持する。

この文書は法令適合判定そのものではなく、確認入口として使用する。

# Building Standards Act Enforcement Order

建築基準法施行令では、構造計算に用いる荷重について主に次を確認する。

- 第84条: 固定荷重
- 第85条: 積載荷重
- 第86条: 積雪荷重

現行条文:

https://laws.e-gov.go.jp/law/325CO0000000338

# Example: Live Load for Residential Rooms

第85条の積載荷重は、用途だけでなく計算対象によって値が異なる。

住宅の居室について確認できる代表値:

| Calculation use | Live load |
|---|---:|
| 床の構造計算 | 1,800 N/m² |
| 大ばり・柱・基礎の構造計算 | 1,300 N/m² |
| 地震力計算 | 600 N/m² |

重要:

これらは構造計算に用いる設計用積載荷重であり、

```text
1,800 N/m²
=
その床に常に1,800 N/m²まで物を置いてよい
```

という意味ではない。

実際の設置可否には、床構造、集中荷重、部材スパン、支持条件、既存建物の状態などが関係する。

# Evaluation Method Standard: Floor Diaphragm

国土交通省の評価方法基準では、木造床組の床倍率について、構造用合板の厚さ、根太間隔、釘種類、釘間隔などを組み合わせた仕様が示されている。

例として、評価方法基準には次のような構成がある。

- 厚さ12 mm以上の構造用合板
- N50釘
- 150 mm以下の釘間隔
- 根太間隔に応じて異なる床倍率

また、厚さ24 mm以上の構造用合板を使用する仕様では、N75釘や四周支持など、別の施工条件が規定されている。

公式資料:

https://www.mlit.go.jp/jutakukentiku/house/content/001603183.pdf

# What Floor Multiplier Represents

床倍率は床構面の水平力に対する性能を扱う指標であり、鉛直方向の積載荷重許容値ではない。

```text
床倍率
→ 水平方向の構面性能

積載荷重
→ 鉛直方向の構造計算条件
```

両者を同じ「床の強さ」として扱わない。

# Public Building Timber Specification

公共建築木造工事標準仕様書では、構造用面材の支持条件や留付けについて確認できる。

令和7年版では、根太を設けない床組で構造用面材を床梁や受材へ留める場合などについて、面材端部の支持や留付け条件が記載されている。

公式資料:

https://www.mlit.go.jp/gobuild/content/001888858.pdf

# Reading Rule

法令や仕様の数値をKnowledgeへ転記するときは、次を一緒に保持する。

- 根拠となる条文・資料
- 対象用途
- 何の計算に使う値か
- 単位
- 確認日
- 適用条件

数値だけを切り出して「床の許容荷重」として再利用しない。

# Related Knowledge

- [Floor Load and Load Path](../concepts/floor-load-and-load-path.md)
- [Sheathed Frame Stiffness](../concepts/sheathed-frame-stiffness.md)
- [DIY Structural Load Check](../workflows/diy-structural-load-check.md)
