# harness-kit

個人開発の複数プロジェクトで使い回す、Claude Code の共通ハーネスです。共通の指示・フック・スキル・コマンドを `harness` プラグイン 1 本にまとめています。

## 導入（利用するプロジェクト側）

プロジェクトの `.claude/settings.json` に次を追加してコミットするだけです。

```json
{
  "extraKnownMarketplaces": {
    "harness-kit": {
      "source": { "source": "github", "repo": "rintarop/harness-kit" },
      "autoUpdate": true
    }
  },
  "enabledPlugins": {
    "harness@harness-kit": true
  }
}
```

そのプロジェクトで Claude Code を起動し、フォルダを信頼すると、プラグインが自動でインストールされます。プロジェクト固有の内容は、これまでどおり各リポジトリの `CLAUDE.md` に書いてください（共通指示よりプロジェクトの `CLAUDE.md` が優先されます）。

## 更新

1. このリポジトリで `plugins/harness/` を編集し、`main` に push します。
2. 利用プロジェクトでは `autoUpdate` が有効なので、Claude Code の起動時に最新版が取り込まれます。すぐに反映したいときは次を実行し、新しいセッションを開始（または `/reload-plugins`）します。

```sh
claude plugin marketplace update harness-kit
claude plugin update harness@harness-kit
```

`plugin.json` に `version` を書いていないため、コミットごとに新しい版として扱われます。

## 含まれるもの

| 種類 | 名前 | 内容 |
| --- | --- | --- |
| 共通指示 | `instructions.md` | 日本語でのやりとり、変更を小さく保つ、終了前にテストを実行する、秘密情報を書かないなど。SessionStart フックで毎セッション読み込まれる |
| スキル | `commit-message` | 変更内容から日本語のコミットメッセージを作る |
| コマンド | `/harness:handoff` | 作業の現状を引き継ぎメモにまとめる |

## ハーネス自体の開発

このリポジトリを Claude Code で開くと、開発用の `CLAUDE.md` と `.claude/` が読み込まれます。構成・書き方・確認方法はそちらを参照してください。
