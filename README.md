# 🤖 QA AI Hackathon - Multi-Agent Communication System

Claude Code を使った分散型AI品質評価システムのデモンストレーション

**📖 Read this in other languages:** [English](README-en.md)

## 🎯 プロジェクト概要

このプロジェクトは、複数のClaude Codeエージェントが連携してeコマースアプリケーションの品質評価を行う分散システムです。PRESIDENT → BOSS → Workersの階層型指示システムにより、体系的な品質評価プロセスを実現します。

### 🏢 システムアーキテクチャ

```
📊 PRESIDENT セッション (プロジェクト統括)
└── PRESIDENT: 全体方針決定・最終判断

📊 multiagent セッション (評価チーム)
├── boss1: 評価チーム管理者
├── worker1: ISTQB準拠・法規制コンプライアンス評価
├── worker2: マネジメント・顧客要件評価
└── worker3: テストアナリスト・技術品質評価
```

### 🛒 評価対象アプリケーション

- **Live Demo**: https://ecommerce-with-stripe-six.vercel.app/
- **Repository**: https://github.com/kychan23/ecommerce-with-stripe
- **Technology Stack**: Next.js + Stripe決済統合
- **Market**: 日本市場向けeコマースサイト（JPY通貨対応）

## 🚀 クイックスタート

### 1. 環境構築

```bash
# リポジトリクローン
git clone <this-repository>
cd qa-ai-hackathon

# tmux環境自動構築
./setup.sh
```

⚠️ **注意**: 既存の `multiagent` と `president` tmuxセッションは自動削除されます

### 2. Claude Code起動

```bash
# 1. PRESIDENT認証（最初に実行）
tmux send-keys -t president 'claude' C-m

# 2. 認証完了後、全エージェント一括起動
tmux list-panes -t multiagent:agents -F '#{pane_id}' | while read pane; do
    tmux send-keys -t "$pane" 'claude' C-m
done
```

### 3. システム動作確認

```bash
# セッション状態確認
tmux list-sessions
tmux attach-session -t multiagent  # エージェント確認
tmux attach-session -t president   # 統括確認（別ターミナル）
```

## 📋 実行パターン

### Pattern 1: Hello World デモ（基本動作確認）

PRESIDENTセッションで実行：
```
あなたはpresidentです。指示書に従って
```

**期待動作フロー:**
1. PRESIDENT → boss1: プロジェクト開始指示
2. boss1 → workers: Hello World作業指示
3. workers: 作業実行 + 完了ファイル作成
4. 最後のworker → boss1: 全員完了報告
5. boss1 → PRESIDENT: 完了通知

### Pattern 2: eコマース品質評価（本格運用）

PRESIDENTセッションで実行：
```
あなたはpresidentです。eコマーステスト評価を開始してください
```

**評価プロセス:**
1. **法規制コンプライアンス評価** (worker1)
   - 特定商取引法準拠確認
   - PCI DSS・個人情報保護法適合性
   - ISTQB原則に基づくテスト観点

2. **ビジネス要件評価** (worker2)
   - 顧客要件との整合性確認
   - ROI観点での機能妥当性評価
   - ユーザビリティ・運用効率性検証

3. **技術品質評価** (worker3)
   - パフォーマンステスト実行
   - セキュリティ脆弱性検査
   - コード品質・保守性評価

4. **統合レポート作成**
   - 重要課題の優先順位付け
   - リリース判定（Go/条件付きGo/No-Go）
   - 対応期間見積もり

## 🛠️ 運用ツール

### agent-send.sh - メッセージ送信

```bash
# 基本使用法
./agent-send.sh [エージェント名] "[メッセージ]"

# 実用例
./agent-send.sh president "緊急: セキュリティ問題を発見"
./agent-send.sh boss1 "追加評価項目の検討要請"
./agent-send.sh worker1 "法的要件の再確認"

# エージェント状態確認
./agent-send.sh --list
```

### ログ・デバッグ機能

```bash
# 送信履歴確認
cat logs/send_log.txt
grep "worker1" logs/send_log.txt

# 評価結果確認
ls -la ./tmp/ecommerce_test_results/
cat ./tmp/ecommerce_test_results/integrated_report.md

# 作業状況確認
ls -la ./tmp/worker*_done.txt
```

## 📁 プロジェクト構造

```
qa-ai-hackathon/
├── README.md                    # このファイル
├── CLAUDE.md                    # Claude Code設定
├── setup.sh                     # 環境構築スクリプト
├── agent-send.sh                # メッセージ送信ツール
├── instructions/                # エージェント指示書
│   ├── president.md             # 統括責任者用（Hello World）
│   ├── boss.md                  # チーム管理者用（Hello World）
│   ├── worker.md                # 作業者用（Hello World）
│   ├── president_ecommerce.md   # 統括責任者用（eコマース評価）
│   ├── boss_ecommerce.md        # チーム管理者用（eコマース評価）
│   ├── worker1_ecommerce.md     # ISTQB・法規制評価者用
│   ├── worker2_ecommerce.md     # ビジネス要件評価者用
│   └── worker3_ecommerce.md     # 技術品質評価者用
├── ecommerce-with-stripe/       # 評価対象アプリケーション
├── tmp/                         # 作業ファイル・評価結果
└── logs/                        # システムログ
```

## 🔧 トラブルシューティング

### 環境リセット

```bash
# 完全リセット（セッション削除 + ファイルクリア）
tmux kill-session -t multiagent 2>/dev/null
tmux kill-session -t president 2>/dev/null
rm -rf ./tmp/*
./setup.sh
```

### よくある問題

1. **tmuxセッションが起動しない**
   ```bash
   # tmux状態確認
   tmux list-sessions
   
   # 強制リセット
   ./setup.sh
   ```

2. **Claude Codeが応答しない**
   ```bash
   # セッション再アタッチ
   tmux attach-session -t multiagent
   # Ctrl+C でリセット後、再実行
   ```

3. **評価結果が生成されない**
   ```bash
   # 作業ディレクトリ確認
   ls -la ./tmp/
   
   # ログ確認
   cat logs/send_log.txt
   ```

## 🎯 成果物

### Hello Worldデモ
- `./tmp/worker*_done.txt`: 各ワーカーの完了マーカー

### eコマース品質評価
- `./tmp/ecommerce_test_results/worker1_compliance_report.md`: 法規制コンプライアンス評価
- `./tmp/ecommerce_test_results/worker2_business_report.md`: ビジネス要件評価  
- `./tmp/ecommerce_test_results/worker3_technical_report.md`: 技術品質評価
- `./tmp/ecommerce_test_results/integrated_report.md`: 統合評価レポート

## 🔒 セキュリティ注意事項

このシステムは**防御的セキュリティ目的**でのみ使用してください：
- ✅ セキュリティ分析・脆弱性検出
- ✅ 品質評価・コンプライアンス確認
- ✅ 防御ツール・検出ルール開発
- ❌ 悪意のあるコード作成・攻撃手法開発

---

## 📄 ライセンス

このプロジェクトは[MIT License](LICENSE)の下で公開されています。

## 🤝 コントリビューション

プルリクエストやIssueでのコントリビューションを歓迎いたします！

---

🚀 **Agent Communication を体感してください！** 🤖✨ 