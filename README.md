# ai-work-playbook

AIドリブンでチーム開発を行うための、組織共通のテンプレート集。

各メンバーがFDEとしてクライアントワークを進める中で得たドメイン知識や設計判断を、
プロジェクトのGitリポジトリ内に蓄積し、新しいメンバーやエージェントが即座に
キャッチアップできる状態をつくる。エージェントが作業の中で知識を更新し、人間はその差分を
レビューして承認する。

## このリポジトリに入っているもの

```
templates/
├── CLAUDE.md            各プロジェクトのルートに置く正本。AI向けの運用規範
└── docs/
    ├── README.md        AI-Driven Engineering Playbook(導入後はこれを読めば運用できる)
    ├── knowledge.md     案件固有のドメイン知識を蓄積する雛形
    └── adr/
        └── 0000-template.md   設計意思決定記録(ADR)の雛形
```

ここで版管理するのは「各プロジェクトにコピーして使うテンプレート」だけ。運用ルールの詳細は
導入後の [templates/docs/README.md](templates/docs/README.md) にまとまっている。

## 導入手順

まずこのテンプレート集をクローンする(初回のみ)。

```bash
git clone https://github.com/Achroma-inc/ai-work-playbook.git ~/Downloads/ai-work-playbook
```

知識蓄積を始めたいプロジェクトのリポジトリのルートで、以下を実行する。

```bash
PLAYBOOK=~/Downloads/ai-work-playbook

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
