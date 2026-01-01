---
type: Memory State
title: Soundproof Booth Construction Control
description: 防音ブース施工の工程進捗、実測値、課題、依存関係、次作業を保持する施工管理State。
tags: [soundproof-booth, construction, progress-control]
status: stable
scope: project
domain: soundproof-booth-project
updated_at: 2026-01-01T14:05:00+09:00
generated: { by: chatgpt/gpt-5.6-sol, at: 2026-01-01T14:05:00+09:00 }
---

# Purpose

防音ブース施工の現在位置を管理し、次の作業開始時に以下を即座に確認できるようにする。

- 完了した工程
- 作業中の工程
- 未着手工程
- 実測済み寸法
- 発生中の課題
- 次工程へ進むための条件
- 次に行う作業

設計条件そのものは [Soundproof Booth Project Plan](soundproof-booth-plan.md) を参照する。

材料の現在在庫と再発注状態は [Soundproof Booth Material Inventory](soundproof-booth-material-inventory.md) を参照する。

# Overall Status

| Phase | Status | Progress | Blocker |
|---|---|---:|---|
| Planning | completed | 100% | none |
| Frame | completed | 100% | none |
| Panels and Insulation | not started | 0% | none |
| Door and Sealing | not started | 0% | none |
| Ventilation | not started | 0% | none |
| Verification | not started | 0% | construction incomplete |

# Current Work

現在状態:

- Phase 2: Frame completed
- 床フレーム: completed
- 左右壁フレーム: completed
- 前後壁フレーム: completed
- 天井フレーム: completed
- ドア開口補強: completed
- 全接合部の固定確認: completed
- Phase 3: ready to start

# Task Board

## Phase 1: Planning

| Task | Status | Result |
|---|---|---|
| 設置スペース確認 | completed | 設置可能 |
| 外形寸法決定 | completed | 1760 × 1280 × 2100 mm |
| 基本構造決定 | completed | 木製フレーム + 吸音層 + 二重面材 |
| 工程分割 | completed | 6 Phase |
| 部材洗い出し | completed | 在庫Stateで管理予定 |

## Phase 2: Frame

| Task | Status | Result / Note |
|---|---|---|
| 床材切断 | completed | 寸法確認済み |
| 床フレーム組立 | completed | 対角差 3 mm |
| 左壁フレーム組立 | completed | 高さ実測済み |
| 右壁フレーム組立 | completed | 高さ実測済み |
| 前壁フレーム組立 | completed | ドア開口を含めて固定済み |
| 後壁フレーム組立 | completed | 垂直確認済み |
| 壁4面本固定 | completed | 床フレームへ固定済み |
| 天井フレーム組立 | completed | 外寸実測済み |
| ドア開口補強 | completed | 開口実測済み |

## Phase 3: Panels and Insulation

| Task | Status | Dependency |
|---|---|---|
| 吸音材充填 | not started | Frame completed |
| 外側合板施工 | not started | Frame completed |
| 石膏ボード施工 | not started | 外側面材固定 |
| 内装面材施工 | not started | 吸音材施工 |
| 継ぎ目処理 | not started | 面材施工 |

## Phase 4: Door and Sealing

| Task | Status | Dependency |
|---|---|---|
| ドア本体製作 | not started | 開口実測 |
| ヒンジ取付 | not started | ドア完成 |
| 戸当たり調整 | not started | ヒンジ取付 |
| 気密材施工 | not started | 戸当たり調整 |

## Phase 5: Ventilation

| Task | Status | Dependency |
|---|---|---|
| 吸気位置決定 | completed | 前壁下部 |
| 排気位置決定 | completed | 後壁上部 |
| ダクト開口施工 | not started | 位置決定 |
| ファン取付 | not started | ダクト施工 |
| 動作確認 | not started | ファン取付 |

## Phase 6: Verification

| Task | Status |
|---|---|
| 外寸実測 | not started |
| 内寸実測 | not started |
| ドア開閉確認 | not started |
| 換気確認 | not started |
| 遮音確認 | not started |
| 残作業確認 | not started |

# Measurements

施工中に確定した実測値を保持する。

| Measurement | Planned | Actual | Status |
|---|---:|---:|---|
| Floor frame width | 1760 mm | 1758 mm | accepted |
| Floor frame depth | 1280 mm | 1279 mm | accepted |
| Floor diagonal A | - | 2174 mm | measured |
| Floor diagonal B | - | 2177 mm | measured |
| Left wall height | 2100 mm | 2098 mm | accepted |
| Right wall height | 2100 mm | 2101 mm | accepted |
| Front wall width | 1760 mm | 1759 mm | accepted |
| Rear wall width | 1760 mm | 1761 mm | accepted |
| Door opening width | 650 mm | 648 mm | accepted |
| Door opening height | 1750 mm | 1748 mm | accepted |
| Ceiling frame width | 1760 mm | 1760 mm | accepted |
| Ceiling frame depth | 1280 mm | 1278 mm | accepted |
| Final frame width | 1760 mm | 1759 mm | accepted |
| Final frame depth | 1280 mm | 1279 mm | accepted |
| Final frame height | 2100 mm | 2099 mm | accepted |

# Tolerance Rules

- フレーム外寸: 計画値 ±5 mm以内を目安とする
- 対角差: 5 mm以内を目安とする
- ドア開口: ドア製作前に現物実測値を優先する
- 許容外の差が出た場合は、後工程へ進む前に原因と対応を記録する

# Issues

## ISSUE-01: Ventilation route

Status: resolved

吸排気口の最終位置を決定した。

影響:

- Phase 5開始時の開口位置が確定
- 面材施工前に開口マーキングを行う

Required action:

後壁上部を排気、前壁下部を吸気とする。面材施工前に開口中心位置をマーキングする。

## ISSUE-02: Floor frame diagonal difference

Status: accepted

床フレームの対角差は3 mm。

許容範囲内のため修正せず施工継続。

# Dependencies

- 壁面材施工は、各壁フレームの固定完了後に開始する
- ドア製作寸法は、前壁フレーム完成後の実測値から決定する
- 換気開口位置は、面材を閉じる前に確定する
- 天井フレームは4面の壁フレーム固定後に施工する
- Verificationは全施工Phase完了後に開始する

# Decisions During Construction

- 床フレームの3 mm対角差は許容する
- ドア寸法は計画値ではなく施工後の開口実測を優先する
- 換気開口は位置確定まで加工しない
- 換気経路は後壁上部を排気、前壁下部を吸気とする

# Next Actions

Phase 2は完了。

次回はPhase 3開始前に次を行う。

1. [部材在庫State](soundproof-booth-material-inventory.md)で吸音材・合板・石膏ボードの残数を確認する
2. 換気開口位置を面材へマーキングする
3. 外側合板の施工順を確認する
4. Phase 3: Panels and Insulationを開始する

# Exit Criteria for Current Phase

Phase 2: Frame 完了確認:

- [x] 床フレーム完成
- [x] 壁4面のフレーム完成
- [x] ドア開口補強完成
- [x] 天井フレーム完成
- [x] 各主要寸法の実測完了
- [x] 全接合部の固定確認完了
- [x] フレーム固定上の未解決Issueなし

Result: completed
