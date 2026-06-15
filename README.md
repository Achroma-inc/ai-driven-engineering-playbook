# ai-work-playbook

AIドリブンでチーム開発を行うための、組織共通のテンプレート集。

各メンバーがFDEとしてクライアントワークを進める中で得たドメイン知識や設計判断を、
プロジェクトのGitリポジトリ内に自動で蓄積し、新しいメンバーやエージェントが即座に
キャッチアップできる状態をつくることを目的とする。

## 思想

- **知識はコードと同じ場所(Git)に置く** — プロジェクトのリポジトリ内に知識を残すことで、
  コードを読むエージェントが自然に知識へ辿り着ける。中央集約はしない。
- **人が一から書かない** — 知識ファイルはAIエージェントが作業の中で更新する。
  人はその差分をレビューして承認するだけ。書く負担がないので腐らない。
- **ADRは人が指示して残す** — knowledge.md と違い、ADR は設計などが固まったタイミングで
  人が明示的に指示して更新する。エージェントが自走で書き散らさず、意思決定が定まった節目だけ記録する。
- **エージェント中立** — 正本は `CLAUDE.md` だが、`AGENTS.md` のsymlinkを張ることで
  Codexなど他のコーディングエージェントからも同じ知識を参照できる。

## このリポジトリに入っているもの

```
templates/
├── README.md        各案件でテンプレートをどう使い、どう運用するか(人間向け)
├── CLAUDE.md         各プロジェクトに置く正本。AI向けの運用ルールを全て含む
├── knowledge.md      案件固有のドメイン知識を蓄積する雛形
└── adr/
    └── 0000-template.md   設計意思決定記録(ADR)の雛形
```

横断的なノウハウ(FDEの標準フロー、クライアント対応の知恵など、特定案件に依存しない知識)は
このリポジトリではなく **Notion の FDE Playbook** に置く。このリポジトリで版管理するのは
「各プロジェクトにコピーして使うテンプレート」だけに絞る。

## 使い方

まずこのテンプレート集をクローンする(初回のみ)。

```bash
git clone https://github.com/Achroma-inc/ai-work-playbook.git ~/開発/ai-work-playbook
```

新しいプロジェクトで知識蓄積を始めるとき、そのプロジェクトのリポジトリのルートで以下を実行する。

```bash
PLAYBOOK=~/開発/ai-work-playbook

# 正本をコピー(Claude Code が読む)
cp "$PLAYBOOK/templates/CLAUDE.md" ./CLAUDE.md

# 案件固有知識の雛形
cp "$PLAYBOOK/templates/knowledge.md" ./knowledge.md

# ADR雛形
mkdir -p docs/adr
cp "$PLAYBOOK/templates/adr/0000-template.md" docs/adr/
```

導入後の運用ルール(knowledge.md と ADR の使い分け・更新方法)は
[templates/README.md](templates/README.md) を参照。

## 対応エージェント

- **Claude Code(デスクトップ)** — 前提とする環境。`CLAUDE.md` を自動で読み込む。
- **Claude Code CLI** — 同じく `CLAUDE.md` を読む。追加設定は不要。
- **Codex / その他** — `AGENTS.md` を読むエージェントは、symlinkを張れば同じ内容を参照できる(オプション。下記参照)。

## オプション

### Codex など他エージェントからも参照する

`AGENTS.md` を読むエージェント(Codex 等)も併用したい場合は、正本の `CLAUDE.md` に
symlinkを張って同一実体にしておくと、同じ知識を参照できる。基本フローには含めないので、
必要なプロジェクトでだけ実行する。

```bash
# CLAUDE.md を正本として AGENTS.md をsymlinkで同一実体にする
ln -s CLAUDE.md AGENTS.md
```
