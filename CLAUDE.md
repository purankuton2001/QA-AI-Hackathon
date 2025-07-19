# Agent Communication System

## プロジェクトタイプ
使用する指示書は実行するプロジェクトによって異なります：

### 1. Hello World プロジェクト（デフォルト）
- **PRESIDENT**: @instructions/president.md
- **boss1**: @instructions/boss.md
- **worker1,2,3**: @instructions/worker.md

### 2. eコマースセキュリティレポートプロジェクト
- **PRESIDENT**: @instructions/president_ecommerce.md
- **boss1**: @instructions/boss_ecommerce.md
- **worker1**: @instructions/worker1_ecommerce.md (ISTQB準拠・法規制コンプライアンス)
- **worker2**: @instructions/worker2_ecommerce.md (マネジメント・顧客要件)
- **worker3**: @instructions/worker3_ecommerce.md (テストアナリスト・技術評価)

## エージェント構成
- **PRESIDENT** (別セッション): プロジェクト統括責任者
- **boss1** (multiagent:agents): 評価チーム管理者
- **worker1,2,3** (multiagent:agents): 各専門分野の評価担当

## メッセージ送信
```bash
./agent-send.sh [相手] "[メッセージ]"
```

## 基本フロー
PRESIDENT → boss1 → workers → boss1 → PRESIDENT

## eコマースセキュリティレポートの実行
PRESIDENTに以下のメッセージを送信してプロジェクトを開始：
```
あなたはpresidentです。eコマーステスト評価を開始してください
```

## 期待される成果物
- 各workerによる専門評価レポート
- 統合評価レポート（重要課題とリリース判定を含む）
- レポート保存場所: `./tmp/ecommerce_test_results/`