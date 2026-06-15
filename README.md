# AI-Driven Engineering Playbook

AIエージェントを主体としたチーム開発のためのテンプレート集。どの企業の開発チームでも導入できる。

プロジェクトを進める中で得たドメイン知識や設計判断を、そのGitリポジトリ内に蓄積し、
新しいメンバーやエージェントが即座にキャッチアップできる状態をつくる。
エージェントが作業の中で知識を更新し、人間はその差分をレビューして承認する。

## このリポジトリに入っているもの

```
templates/
├── CLAUDE.md            各プロジェクトのルートに置く正本。AI向けの運用規範
└── docs/
    ├── README.md        AI-Driven Engineering Playbook(導入後はこれを読めば運用できる)
    ├── knowledge.md     プロジェクト固有のドメイン知識を蓄積する雛形
    └── adr/
        └── 0000-template.md   設計意思決定記録(ADR)の雛形
```

ここで版管理するのは「各プロジェクトにコピーして使うテンプレート」だけ。運用ルールの詳細は
導入後の [templates/docs/README.md](templates/docs/README.md) にまとまっている。

## 導入手順

まずこのテンプレート集をクローンする(初回のみ)。

```bash
git clone https://github.com/Achroma-inc/ai-driven-engineering-playbook.git ~/Downloads/ai-driven-engineering-playbook
```

知識蓄積を始めたいプロジェクトのリポジトリのルートで、以下を実行する。

```bash
PLAYBOOK=~/Downloads/ai-driven-engineering-playbook

# 知識ファイル(knowledge.md と ADR)と運用ガイドを docs/ ごと配置する
cp -R "$PLAYBOOK/templates/docs" ./docs
```

`CLAUDE.md`(正本)はルート直下に置く。**既存の `CLAUDE.md` があるかどうかで手順が変わる。**

- **無い場合** — そのままコピーする。

  ```bash
  cp "$PLAYBOOK/templates/CLAUDE.md" ./CLAUDE.md
  ```

- **既存の `CLAUDE.md` がある場合** — 上書きせず、既存の内容と整合を取った上で **マージする**。
  エージェントに「雛形の内容を既存 `CLAUDE.md` にマージして」と依頼すると差分を提案させられる。

`docs/` を上記と違う場所に配置した場合は、`CLAUDE.md` 内の「知識ファイルの場所」に書かれた
パスを実際の配置に合わせて書き換える(これでエージェントが正しく辿れる)。

導入後の運用は [templates/docs/README.md](templates/docs/README.md)(導入先では `docs/README.md`)を参照。

## グローバル CLAUDE.md(全てのプロジェクトで共通の設定・任意)

このリポジトリが扱うのは **プロジェクトの** `CLAUDE.md` だが、その手前に **グローバルな**
`CLAUDE.md` を置ける。各メンバーが自分のマシンに一度だけ設定する、全プロジェクト・全エージェント
共通の作業規範。コミュニケーション・コードスタイル・ワークフローの基本方針をチーム内で揃えられる。

- **グローバル**(`~/.claude/CLAUDE.md`、Codex は `~/.codex/AGENTS.md`) — 個人のマシン全体に効く。
  どのプロジェクトでも共通の振る舞い。リポジトリには含めない、各自の環境設定。
- **プロジェクト**(`<repo>/CLAUDE.md`) — このプレイブックがコピーする、リポジトリ固有の運用規範。
  知識ファイルの場所や knowledge.md / ADR の扱いなど、そのプロジェクト限定の指示。

エージェントは両方を読む(グローバルが土台、プロジェクトが上書き・追加)。設定後は新規セッションから
反映される(既存セッションは `/compact` で読み直す)。チームで内容を共有しておくと、誰が書いても
出力の方向性が揃い、このプレイブックの効果も上がる。

配置場所や読み込みの仕様は各エージェントの公式ドキュメントを参照。

- [Claude があなたのプロジェクトを記憶する方法 — Claude Code Docs](https://code.claude.com/docs/ja/memory)
- [Custom instructions with AGENTS.md — Codex / OpenAI Developers](https://developers.openai.com/codex/guides/agents-md)

### コードスタイルの参考

グローバル `CLAUDE.md` の一例。これをベースに各チーム・各自で調整するとよい。

```markdown
# コミュニケーション
- 否定的な意見も含めて率直に回答する
- emojiは使わない
- 自明な説明を省き、冗長な助詞・語尾・修飾を使わず簡潔な常体で密度を最大化する
- 日本語で会話をする
- 質問・調査タスクは推測で答えず、コード・ドキュメント・一次情報で事実確認して回答する

# コードスタイル
- 関数型アプローチを優先し、副作用を最小化する
- 厳密な型付け（anyではなくunknown）
- エラーは握りつぶさず、意味のあるメッセージを付けて処理する
- KISS: 最も単純な実装を選び、過度な抽象化を避ける
- YAGNI: 要求されていない機能・抽象化・ファイルを作らない
- DRY: ロジックの重複は共通化する。ただしKISS/YAGNIを優先し、早すぎる抽象化はしない

# ワークフロー
- 着手前に関連コードを読み、ゴール・プラン・判断項目で承認を得る
- 承認後はゴール達成まで自走する
- テストが書ける変更はテストを先に書き、失敗を確認してから実装する
- テストの削除・コメントアウト、動作するコードのリファクタは事前確認する
- 完了報告前にテスト・リント・型チェックを実行し、結果を含める
- コミット前にdiffを確認。Conventional Commits形式、本文は日本語
- 成果物はレビューを依頼し、git管理プロジェクトは<repo>/CLAUDE.md末尾のSTATEを更新する
- 同じ誤りが繰り返されたらCLAUDE.mdの更新を提案する
```
