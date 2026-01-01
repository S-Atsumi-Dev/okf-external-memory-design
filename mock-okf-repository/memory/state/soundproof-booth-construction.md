---
type: Memory State
title: Soundproof Booth Construction Control
description: 防音ブース施工の工程進捗、実測値、課題、依存関係、次作業を保持する施工管理State。
tags: [soundproof-booth, construction, progress-control]
status: stable
scope: project
domain: soundproof-booth-project
updated_at: 2026-01-01T10:00:00+09:00
generated: { by: codex, at: 2026-01-01T10:00:00+09:00 }
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


# Overall Status

| Phase | Status | Progress | Blocker |
|---|---|---:|---|
| Planning | completed | 100% | none |
| Frame | in progress | 45% | none |
| Panels and Insulation | not started | 0% | frame incomplete |
| Door and Sealing | not started | 0% | wall opening incomplete |
| Ventilation | not started | 0% | route position unresolved |
| Verification | not started | 0% | construction incomplete |

# Current Work

現在作業中:

- Phase 2: Frame
- 床フレーム: completed
- 左右壁フレーム: in progress
- 前後壁フレーム: not started
- 天井フレーム: not started
- ドア開口補強: not started

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
| 左壁フレーム組立 | in progress | 縦材固定中 |
| 右壁フレーム組立 | not started | 左壁完了後 |
| 前壁フレーム組立 | not started | ドア開口を含む |
| 後壁フレーム組立 | not started | - |
| 天井フレーム組立 | not started | 壁4面固定後 |
| ドア開口補強 | not started | 前壁施工時 |

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
| 吸気位置決定 | pending | 配置確認 |
| 排気位置決定 | pending | 配置確認 |
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
| Left wall height | 2100 mm | not measured | pending |
| Door opening width | 650 mm | not measured | pending |
| Door opening height | 1750 mm | not measured | pending |

# Tolerance Rules

- フレーム外寸: 計画値 ±5 mm以内を目安とする
- 対角差: 5 mm以内を目安とする
- ドア開口: ドア製作前に現物実測値を優先する
- 許容外の差が出た場合は、後工程へ進む前に原因と対応を記録する

# Issues

## ISSUE-01: Ventilation route

Status: open

吸排気口の最終位置が未決定。

影響:

- Phase 5を開始できない
- 面材施工前に開口位置を確定する必要がある

Required action:

壁フレーム完成後、室内側・室外側の干渉を確認して位置を決定する。

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

# Next Actions

優先順:

1. 左壁フレームの縦材を固定する
2. 左壁フレーム外寸を実測する
3. 右壁フレームを組み立てる
4. 前壁フレームのドア開口位置を確認する
5. 換気経路候補を確認する

# Exit Criteria for Current Phase

Phase 2: Frameを完了扱いにする条件:

- 床フレーム完成
- 壁4面のフレーム完成
- ドア開口補強完成
- 天井フレーム完成
- 各主要寸法の実測完了
- フレーム固定上の未解決Issueがない
