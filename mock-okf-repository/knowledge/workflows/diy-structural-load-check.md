---
type: Workflow
title: DIY Structural Load Check
description: 重量のあるDIY設備を既存床へ設置する前に、重量、接地条件、床構成、法令・設計条件を順番に確認するための手順。
tags: [diy, structure, floor-load, workflow]
status: stable
generated: { by: chatgpt/gpt-5.6-sol, at: 2026-01-01T08:20:00+09:00 }
---

# Goal

重量のあるDIY設備や造作物について、「平均kg/m²だけ計算して終わる」ことを避け、確認すべき情報を順番に整理する。

# Step 1: Define the Object

記録する。

- 外形寸法
- 材料
- 各材料の数量
- 設備本体重量
- 使用中に追加される重量
- 人が内部へ入る場合の想定重量
- 重心位置
- 接地方法

# Step 2: Estimate Total Mass

各部材について:

```text
mass = volume × density
```

板材の場合:

```text
volume = width × height × thickness
```

全部材を合計する。

```text
M_total =
  frame
+ panels
+ insulation
+ door
+ equipment
+ occupants
+ other components
```

材料密度に幅がある場合は、根拠のある範囲または保守側の値を使用する。

# Step 3: Identify Contact Conditions

次を確認する。

- 底面全面で接するか
- 木枠だけで接するか
- 脚で支持するか
- キャスターか
- 防振材を介するか
- 荷重分散板を使うか

平均面荷重:

```text
q_avg = M_total / A_contact
```

脚支持の場合は、各支持点の反力も別に確認する。

# Step 4: Identify the Existing Floor

可能な範囲で確認する。

- 木造 / RC / 鉄骨などの構造形式
- 床仕上げ
- 下地材
- 根太の方向
- 根太間隔
- 根太スパン
- 梁位置
- 壁・柱位置
- 設計図書の有無
- 劣化・たわみ・損傷の有無

構成が不明な場合は「不明」を明示し、推測だけで安全判定しない。

# Step 5: Trace the Load Path

```text
DIY設備
 ↓
接地部
 ↓
床下地
 ↓
根太 / スラブ
 ↓
梁
 ↓
柱・壁
 ↓
基礎
```

特に集中荷重がどの根太・梁へ入るかを確認する。

# Step 6: Check Regulatory and Design References

確認対象の例:

- 建築基準法施行令の固定荷重・積載荷重
- 建物用途
- 既存設計図書
- 構造計算書
- 採用されている構造仕様
- 面材・接合部の仕様

法令の積載荷重値を、そのまま「載せてよい最大重量」として扱わない。

Reference:

- [Japanese Building Structural Load References](../references/japan-building-structural-loads.md)

# Step 7: Separate Vertical and Horizontal Checks

重量物設置:

- 鉛直荷重
- 根太・梁の曲げ
- たわみ
- 局部荷重

箱形フレームや壁面材:

- 水平変形
- 面材せん断
- 接合部
- 床・壁構面

は別の確認項目として扱う。

# Step 8: Record the Result

プロジェクト固有の結果はKnowledgeではなくMemory Stateへ保存する。

例:

```text
Knowledge:
  荷重計算方法
  確認手順
  法令Reference

Memory State:
  実際の寸法
  実際の重量
  設置位置
  根太方向
  採用した対策
  現在の判定
```

# Stop Conditions

次のような場合は、簡易計算だけで施工可否を決めない。

- 既存床構造が確認できない
- 大きな集中荷重になる
- 明らかな床のたわみ・劣化がある
- 梁・根太など構造部材を加工する
- 法令適合や構造安全性の証明が必要
- 設計時の用途と異なる重量物を恒久設置する

この場合は図面・構造計算書の確認や、建築士・構造設計者等への確認を行う。
