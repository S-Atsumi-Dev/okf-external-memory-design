---
type: Concept
title: Sheathed Frame Stiffness
description: 木製フレーム単体と構造用面材を留め付けた後で水平変形特性が変わる理由と、簡易的な比較方法を整理する。
tags: [structure, sheathing, plywood, stiffness, diy]
status: stable
stale_after: 2026-07-01
generated: { by: chatgpt/gpt-5.6-sol, at: 2026-01-01T08:10:00+09:00 }
sources:
  - id: mlit-evaluation-method
    resource: https://www.mlit.go.jp/jutakukentiku/house/content/001603183.pdf
    title: 評価方法基準
---

# Purpose

木枠へ合板などの面材を取り付けると、フレーム単体より水平変形しにくくなる。

この変化を「面材そのものが硬いから」とだけ説明せず、面材・留め具・枠材・支持条件を含むシステムとして理解する。

# Bare Frame

矩形の木製フレームへ水平力を加えると、接合部の回転や滑りによって平行四辺形状へ変形しやすい。

```text
┌──────┐       ／─────／
│      │  →   ／     ／
│      │     ／     ／
└──────┘    ／─────／
```

フレーム単体の水平剛性は、木材そのものの曲げ剛性だけでなく、接合部の挙動に強く影響される。

# After Sheathing

合板などの面材をフレームへ多数の釘・ビスで留め付けると、面材が面内せん断を負担し、その力を留め具を通して枠材へ伝える。

```text
水平力
  ↓
面材の面内せん断
  ↓
釘・ビスのせん断 / 滑り
  ↓
枠材
  ↓
接合部・支持部
```

そのため、全体の変形量は次の要素の組合せで決まる。

- 枠材の変形
- 面材のせん断変形
- 釘・ビスの滑り
- 枠材と面材の端部条件
- アンカーや接合部の変形
- 開口や継ぎ目

# Simplified Stiffness Comparison

同じ大きさの水平力 `F` を与え、水平変位 `δ` を測定できる場合、見かけの割線剛性は:

```text
K = F / δ
```

比較例:

```text
K_bare     = F / δ_bare
K_sheathed = F / δ_sheathed
```

`K_sheathed > K_bare` であれば、面材施工後に同じ荷重に対する変形量が小さくなったことを示せる。

これは比較用の指標であり、法令上の耐力を直接表す値ではない。

# Idealized Panel Shear

矩形パネルだけを理想化して、幅 `b`、厚さ `t`、高さ `h`、せん断弾性係数 `G`、水平せん断力 `V` とした場合、面材部分のせん断変形は概念的に:

```text
δ_panel ≈ V × h / (G × b × t)
```

と表せる。

ただし実際の木造面材フレームでは、留め具の滑りや接合部変形が無視できないため、この式だけから全体剛性を決めることはできない。

# Deformation Components

簡易的な考え方:

```text
δ_total
  ≈ δ_frame
  + δ_panel
  + δ_fastener
  + δ_connection
  + δ_anchor
```

したがって、

> 合板のヤング率やせん断弾性係数だけから壁・床全体の剛性を決めない

ことが重要になる。

# Fastener Pattern Matters

構造用面材の性能は、次のような施工条件でも変わる。

- 面材厚さ
- 面材種類
- 釘・ビスの種類
- 留付け間隔
- 面材端部の支持
- 根太・梁・柱の間隔
- 面材継ぎ目の位置
- 開口の有無

国土交通省の住宅性能表示制度における床倍率の評価方法でも、構造用合板の厚さ、根太間隔、釘種類、釘間隔などによって倍率が異なる。

これは「面材だけ」ではなく、面材と留付け方法を含む構成全体が性能を決める例になる。

# Floor Multiplier Is Not Vertical Capacity

住宅性能表示等で使われる床倍率は、主に水平力に対する床構面の性能を扱う指標であり、床へ重量物を載せられるかという鉛直荷重の許容量とは別の概念。

次を混同しない。

```text
鉛直方向:
固定荷重 / 積載荷重 / 梁・根太の曲げ・たわみ

水平方向:
床構面 / 壁構面 / 面材せん断 / 接合部
```

# Related Reference

- [Japanese Building Structural Load References](../references/japan-building-structural-loads.md)

# Limitation

簡易式や比較試験は、構造安全性の証明や法令適合判定の代わりにはならない。

実際の耐力評価では、適用する構造方式に対応した基準、認定仕様、試験値、設計方法を使用する。
