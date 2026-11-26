# harness-kit 開発ガイド

このリポジトリは、個人開発の複数プロジェクトで共通に使う Claude Code ハーネスを、プラグイン 1 本として配布するためのものです。ここにある CLAUDE.md と `.claude/` はこのリポジトリ自体を開発するためのもので、配布物には含まれません。

## 構成

- `.claude-plugin/marketplace.json`: 自分用マーケットプレイスの定義。`plugins/harness` を `harness` として公開する。
- `plugins/harness/`: 配布するプラグイン本体。
  - `.claude-plugin/plugin.json`: プラグインのマニフェスト。`version` はあえて書かない（コミットごとに更新が届くようにするため）。
  - `instructions.md`: 全プロジェクト共通の指示。SessionStart フックでセッション開始時に読み込まれる。
  - `hooks/hooks.json`: フック定義。
  - `skills/<名前>/SKILL.md`: スキル。新しい機能は基本的にスキルとして足す。
  - `commands/<名前>.md`: スラッシュコマンド（`/harness:<名前>` で呼ぶ）。
- `README.md`: 利用プロジェクトでの導入手順と更新方法。

## 書き方

- 日本語で、短く具体的に書く。凝った仕組み（自動生成、外部依存、複雑なスクリプト）は入れない。
- `instructions.md` はどのプロジェクトにも当てはまる内容だけにする。特定のプロジェクトの内容は、そのリポジトリの CLAUDE.md に書く。
- `instructions.md` は毎セッション読み込まれるので、短く保つ（目安 100 行以内、上限 10,000 文字）。
- スキルやコマンドを追加・削除したら、README の「含まれるもの」も更新する。

## 公開リポジトリとしてのルール

このリポジトリは公開されています。次のものは絶対に書かないでください。

- 秘密情報（API キー、トークン、パスワード、認証情報、`.env` の中身）
- 個人の環境に依存するパス（`/Users/<名前>/...`、`C:\Users\...` など）
- 非公開プロジェクトの名前・内容・URL

## 確認方法

変更したら次を実行して、エラーがないことを確かめる。

```sh
claude plugin validate .
claude plugin validate plugins/harness
```

実際の動作を試すときは、インストールせずにローカルのプラグインを読み込める。

```sh
claude --plugin-dir ./plugins/harness
```

起動後、共通指示が読み込まれているか、`/harness:handoff` などが使えるかを確認する。
