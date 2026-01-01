---
type: Memory State
title: Soundproof Booth Project Plan
description: 防音ブース施工プロジェクトの目標、設計条件、工程、完了条件を保持する現在状態。
tags: [soundproof-booth, construction, project-plan]
status: stable
scope: project
domain: soundproof-booth-project
updated_at: 2026-01-01T10:00:00+09:00
generated: { by: codex, at: 2026-01-01T10:00:00+09:00 }
---

# Goal

室内で楽器練習に使用する1人用防音ブースを製作する。

主な目的:

- 室内の演奏音を周囲へ伝わりにくくする
- 1回30分程度の練習を想定する
- 分解・補修しやすい構造にする
- 施工途中でも寸法・材料・進捗を追跡できる状態にする

# Design Conditions

構造設計の根拠は [Soundproof Booth Structural Design](soundproof-booth-structural-design.md) を参照する。


## Dimensions

計画寸法:

| Item | Value |
|---|---:|
| External width | 1760 mm |
| External depth | 1280 mm |
| External height | 2100 mm |
| Target internal width | 1620 mm |
| Target internal depth | 1140 mm |
| Target internal height | 1960 mm |
| Door opening width | 650 mm |
| Door opening height | 1750 mm |

寸法は施工前の計画値とし、実測値が確定した場合はこのStateを更新する。

## Wall / Floor / Ceiling

基本構成:

- 木製フレーム
- 空気層
- 吸音材
- 合板
- 石膏ボード
- 内装仕上げ材

施工性を優先し、壁・天井・床はそれぞれ独立して補修可能な構成とする。

# Project Scope

含むもの:

- 木製フレーム製作
- 壁・床・天井の面材施工
- 吸音材施工
- ドア製作・取付
- 気密処理
- 換気経路の取付
- 完成後の寸法確認
- 完成後の遮音確認

含まないもの:

- 建物本体への恒久的な構造変更
- 電気設備の新設工事
- 空調設備の増設

# Work Phases

## Phase 1: Planning

Status: completed

- 設置スペース確認
- 外形寸法決定
- 基本構造決定
- 工程分割
- 必要部材の洗い出し

## Phase 2: Frame

Status: in progress

- 床フレーム
- 壁フレーム
- 天井フレーム
- 開口部補強

## Phase 3: Panels and Insulation

Status: not started

- 吸音材
- 外側面材
- 内側面材
- 継ぎ目処理

## Phase 4: Door and Sealing

Status: not started

- ドア製作
- ヒンジ取付
- 戸当たり調整
- 気密材施工

## Phase 5: Ventilation

Status: not started

- 吸気経路
- 排気経路
- ファン取付
- 騒音確認

## Phase 6: Verification

Status: not started

- 外形寸法確認
- 内寸確認
- ドア動作確認
- 換気確認
- 遮音測定

# Constraints

- 設置場所から搬出できる分割構造にする
- 一人で交換できない大型部材を増やしすぎない
- メンテナンス時に壁全体を解体しなくてよい構造を優先する
- 現場実測で計画寸法と差が出た場合は、施工を進める前にStateへ反映する
- 部材変更時は在庫Stateと整合させる

# Completion Criteria

完成扱いにする条件:

- フレーム、壁、床、天井、ドアが固定済み
- 主要な隙間の処理が完了
- 換気経路が使用可能
- 内外寸法を実測済み
- ドアが正常に開閉可能
- 遮音確認を1回以上実施
- 未施工箇所が施工管理Stateに残っていない

# Current Position

- Planning: completed
- Frame: in progress
- Panels and Insulation: not started
- Door and Sealing: not started
- Ventilation: not started
- Verification: not started

# Next

床フレームの寸法確認後、壁フレームの組立へ進む。

# Open Questions

- 換気経路の最終位置
- ドア下端の気密方式
- 完成後の遮音確認条件
