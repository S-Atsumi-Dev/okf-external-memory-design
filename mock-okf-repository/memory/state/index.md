# Memory State Index

Memory Stateは「現在どうなっているか」の原本です。

## State Documents

- [Soundproof Booth Project Plan](soundproof-booth-plan.md) - 防音ブース施工の設計条件、工程、完了条件を保持する計画State。
- [Soundproof Booth Structural Design](soundproof-booth-structural-design.md) - 床荷重・荷重経路・フレーム構面の設計根拠と採用判断を保持するState。
- [Soundproof Booth Construction Control](soundproof-booth-construction.md) - 防音ブース施工の工程進捗、実測、課題、依存関係、次作業を保持する施工管理State。
- [Soundproof Booth Material Inventory](soundproof-booth-material-inventory.md) - 必要数、購入数、使用数、残数、工程割当、再発注状態を保持する材料在庫State。
- [Learning Topic A](learning-topic-a.md) - 継続学習テーマの現在状態。

## Typical Contents

- Current Position
- Current Plan
- Configuration
- Constraints
- Evaluation Criteria
- Next Action
- Open Questions

頻繁に変化する現在値は、Eventを増やさずStateを直接更新できます。
