---
type: Memory State
title: Soundproof Booth Material Inventory
description: 防音ブース施工で使用する主要部材の必要数、購入数、使用数、残数、工程割当、再発注状態を保持する在庫State。
tags: [soundproof-booth, construction, materials, inventory]
status: stable
scope: project
domain: soundproof-booth-project
updated_at: 2026-01-01T14:10:00+09:00
generated: { by: chatgpt/gpt-5.6-sol, at: 2026-01-01T14:10:00+09:00 }
---

# Purpose

施工に必要な主要部材について、現在の在庫と不足を一か所で確認する。

このStateでは、次を区別する。

- Project全体で必要な数量
- 購入済み数量
- 使用済み数量
- 現在利用可能な数量
- 次工程へ割り当てる数量
- 不足数量
- 再発注状態

施工工程そのものは [Soundproof Booth Construction Control](soundproof-booth-construction.md) を参照する。

# Inventory Summary

| Material | Unit | Required | Purchased | Consumed | Available | Next Phase Allocation | Shortage | Status |
|---|---|---:|---:|---:|---:|---:|---:|---|
| SPF lumber 38 × 89 × 2400 mm | piece | 30 | 32 | 24 | 8 | 0 | 0 | sufficient |
| Structural plywood 12 mm | sheet | 12 | 8 | 0 | 8 | 8 | 4 | reorder |
| Gypsum board 12.5 mm | sheet | 12 | 6 | 0 | 6 | 6 | 6 | reorder |
| Acoustic insulation | pack | 10 | 10 | 0 | 10 | 8 | 0 | sufficient |
| Interior finish panel | sheet | 8 | 4 | 0 | 4 | 0 | 4 | planned purchase |
| Wood screw 65 mm | box | 3 | 3 | 2 | 1 | 0 | 0 | sufficient |
| Panel screw 32 mm | box | 3 | 2 | 0 | 2 | 2 | 1 | reorder |
| Sealant | cartridge | 8 | 4 | 0 | 4 | 2 | 4 | planned purchase |
| Door hinge | piece | 3 | 3 | 0 | 3 | 0 | 0 | reserved |
| Door seal | m | 8 | 0 | 0 | 0 | 0 | 8 | not purchased |
| Ventilation duct | m | 6 | 6 | 0 | 6 | 0 | 0 | reserved |
| Ventilation fan | unit | 1 | 1 | 0 | 1 | 0 | 0 | reserved |

# Quantity Rules

基本計算:

```text
Available = Purchased - Consumed
Shortage  = max(Required - Purchased, 0)
```

次工程開始可否は、Project全体の不足ではなく、その工程へ必要な数量を基準に確認する。

```text
Ready for next phase
  if Available >= Next Phase Allocation
```

# Phase Allocation

## Phase 2: Frame

Status: completed

使用済み:

- SPF lumber: 24 pieces
- Wood screw 65 mm: 2 boxes

残材は補修・追加補強用として保持する。

## Phase 3: Panels and Insulation

Status: ready with procurement actions

開始時に確保する数量:

| Material | Required for start | Available | Result |
|---|---:|---:|---|
| Structural plywood 12 mm | 8 sheets | 8 sheets | ready |
| Acoustic insulation | 8 packs | 10 packs | ready |
| Gypsum board 12.5 mm | 6 sheets | 6 sheets | ready |
| Panel screw 32 mm | 2 boxes | 2 boxes | ready |
| Sealant | 2 cartridges | 4 cartridges | ready |

Phase 3開始分は確保済み。

ただしProject全体では合板、石膏ボード、Panel screw、Sealantに不足があるため、後工程へ進む前に追加購入する。

## Phase 4: Door and Sealing

予約済み:

- Door hinge: 3 pieces

不足:

- Door seal: 8 m
- Sealant: Project全体必要数に対して4 cartridges不足

## Phase 5: Ventilation

予約済み:

- Ventilation duct: 6 m
- Ventilation fan: 1 unit

# Procurement Queue

| Priority | Material | Quantity | Needed Before | State |
|---|---|---:|---|---|
| high | Structural plywood 12 mm | 4 sheets | Phase 3 completion | reorder |
| high | Gypsum board 12.5 mm | 6 sheets | Phase 3 completion | reorder |
| medium | Panel screw 32 mm | 1 box | Phase 3 completion | reorder |
| medium | Sealant | 4 cartridges | Phase 4 | planned |
| medium | Interior finish panel | 4 sheets | Phase 3 interior finish | planned |
| low | Door seal | 8 m | Phase 4 | not purchased |

# Reservations

購入済みでも、別工程で使用予定の部材は自由在庫として扱わない。

現在の予約:

- Door hinge -> Phase 4
- Ventilation duct -> Phase 5
- Ventilation fan -> Phase 5

# Inventory Update Rules

材料を購入・使用・破損・返品した場合、このStateを直接更新する。

通常の数量変化だけではMemory Eventを作成しない。

Eventを検討する例:

- 主要材料を別仕様へ変更した
- 材料不足により工程計画を変更した
- 部材不良により設計変更が必要になった

# Reconciliation Check

更新時は次を確認する。

```text
Purchased >= Consumed

Available = Purchased - Consumed

Reserved <= Available

Project shortage
  = max(Required - Purchased, 0)
```

数量が一致しない場合は推測で補正せず、差異をIssueとして残す。

# Current Status

- Phase 2で使用した木材・固定具は反映済み
- Phase 3開始に必要な最低数量は確保済み
- Project完了までには追加購入が必要
- 現在、材料不足によるPhase 3開始Blockerはない

# Next

1. Phase 3開始時に実際の使用数量を記録する
2. 合板4枚と石膏ボード6枚を追加購入する
3. Panel screw 1箱を追加購入する
4. Phase 4開始前にDoor sealとSealantを確保する
