---
type: Concept
title: Floor Load and Load Path
description: DIY設備や重量物を床へ設置するときに、重量・面荷重・集中荷重・荷重経路を分けて考えるための基本概念。
tags: [structure, floor-load, diy, load-path]
status: stable
stale_after: 2026-07-01
generated: { by: chatgpt/gpt-5.6-sol, at: 2026-01-01T08:00:00+09:00 }
sources:
  - id: egov-building-order
    resource: https://laws.e-gov.go.jp/law/325CO0000000338
    title: 建築基準法施行令
---

# Purpose

重量のある設備や工作物を既存床へ設置するとき、「総重量」だけで判断せず、荷重がどのように床へ伝わるかを整理する。

# Basic Quantities

## Mass and Weight

質量 `m` [kg] から重量 `W` [N] を求める場合:

```text
W = m × g
```

通常は `g ≈ 9.8 m/s²` を用いる。

概算段階ではkgを使って比較することもできるが、構造計算では力の単位を区別する。

## Average Area Load

設備全体の質量を `m` [kg]、接地面積を `A` [m²] とすると、平均的な面荷重の概算は:

```text
q_mass = m / A
```

力として扱う場合:

```text
q = W / A
```

ただし、この値だけで床の安全性を判定することはできない。

# Distributed Load vs Concentrated Load

同じ総重量でも、床へ伝わる方法によって影響が変わる。

## Distributed Load

底面全体で荷重を受ける場合。

例:

- 面で接する架台
- 床全面に接する箱形設備

## Concentrated Load

脚やキャスターなど、狭い範囲へ荷重が集中する場合。

4本脚で均等に支持される単純モデルでは:

```text
R_i ≈ W / 4
```

実際には重心位置、床の不陸、架台剛性などにより各支持点の反力は均等にならない。

# Load Path

床上の重量は、直接「建物全体」へ均等に伝わるわけではない。

代表的な荷重経路:

```text
設置物
  ↓
接地面 / 脚
  ↓
床仕上げ
  ↓
下地材・構造用面材
  ↓
根太 / 床梁
  ↓
梁・壁・柱
  ↓
基礎
  ↓
地盤
```

どの部材がどの範囲の荷重を受け持つかを確認することが重要になる。

# Why Average Load Is Not Enough

平均面荷重が小さくても、次の条件で局所的な負担が大きくなる。

- 脚が細い
- 根太と根太の中間へ集中荷重が載る
- 根太や梁のスパンが長い
- 設備の重心が偏っている
- 下地材が薄い
- 既存床に劣化や損傷がある
- 重量物が壁際・開口部付近など特定位置へ集中する

# Regulatory Load vs Actual Capacity

法令で定められる積載荷重は、構造計算で用いる設計上の荷重条件であり、「この床にはその数値まで載せてよい」という単純な許容重量ではない。

例えば住宅の居室について、建築基準法施行令第85条では計算対象に応じて異なる積載荷重が設定されている。

法令値を利用する場合は、必ず現行条文と対象用途を確認する。

関連Reference:

- [Japanese Building Structural Load References](../references/japan-building-structural-loads.md)

# Practical Use

DIYで重量物を設置するときは、最低限次を分けて確認する。

1. 設置物の総重量
2. 接地面積
3. 脚や支持点ごとの集中荷重
4. 床下地の構成
5. 根太・梁の方向とスパン
6. 荷重が最終的にどこへ伝わるか
7. 法令・設計図書・既存建物の条件

# Limitation

ここで扱う式は荷重を整理するための基本モデルであり、既存建築物の構造安全性を証明する計算ではない。

構造形式、部材寸法、接合、劣化状態などが不明な場合や、重量設備を設置する場合は、図面確認や専門家による検討が必要になる。
