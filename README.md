# Software Design Docs

`software-design-docs` は、機能アイデア、会話、既存コード、既存ドキュメント、要件メモから、実装に進めるためのソフトウェア設計ドキュメントを作成する Claude Code / Codex 互換の Skill です。

これは汎用的な PRD 作成 Skill ではありません。プロダクト意図を、要件定義、基本設計、詳細設計、DB/API/バッチ/画面設計、試験観点、影響範囲、注意事項、未決事項といったエンジニアリング向けドキュメントへ落とし込むときに使います。

## 使う場面

この Skill は、次のような場面で使います。

- 一貫したソフトウェア設計ドキュメント一式を作成・改訂したい。
- アイデアや議論を実装計画に使える資料へ変換したい。
- 要件、システム設計、詳細な振る舞い、データ契約、画面、ジョブ、API、試験、運用上の注意点を揃えたい。
- ドキュメントファーストのプロジェクトで、実装前に設計資料を準備したい。
- 下流の設計書が、上流の要件やアーキテクチャ決定と矛盾していないか確認したい。

## 使い方

まず [`SKILL.md`](SKILL.md) を読んでください。ワークフロー、出力ルール、参照ファイルの読み込み方針が定義されています。

標準的な作成順は次のとおりです。

1. 要件定義
2. 基本設計
3. 詳細設計
4. DB/API/バッチ/画面設計
5. 試験観点
6. 影響範囲
7. 注意事項・未決事項

既存プロジェクトでは、まずプロジェクトルールを読みます。特に `AGENTS.md` や、`Documents/INDEX.md` / `docs/INDEX.md` のようなドキュメント索引を優先してください。既存の正式ドキュメントを黙って上書きしてはいけません。

## 参照ファイル

依頼された成果物に必要なファイルだけを読み込みます。

| 用途 | 参照ファイル |
|---|---|
| 要件定義 | [`references/requirements.md`](references/requirements.md) |
| ドキュメント配置、索引、アーカイブ、メタデータ規則 | [`references/document_indexing.md`](references/document_indexing.md) |
| 基本設計 | [`references/basic-design.md`](references/basic-design.md) |
| 詳細設計 | [`references/detailed-design.md`](references/detailed-design.md) |
| DB設計 | [`references/db-design.md`](references/db-design.md) |
| API設計 | [`references/api-design.md`](references/api-design.md) |
| バッチ設計 | [`references/batch-design.md`](references/batch-design.md) |
| 画面設計 | [`references/screen-design.md`](references/screen-design.md) |
| 分割設計ドキュメントの配置・命名・索引ルール | [`references/split-design-docs.md`](references/split-design-docs.md) |
| 試験観点 | [`references/test-viewpoints.md`](references/test-viewpoints.md) |
| 可観測性（ログ、メトリクス、トレース、アラート、SLO） | [`references/observability.md`](references/observability.md) |
| 影響範囲・注意事項 | [`references/impact-and-cautions.md`](references/impact-and-cautions.md) |
| ドキュメントパターン例 | [`references/kabureka-patterns.md`](references/kabureka-patterns.md) |

`references/kabureka-patterns.md` はパターン例であり、すべてのプロジェクトに適用する汎用ルールではありません。

## 品質ルール

- コード、スキーマ、ルート、ジョブ、テスト、既存ドキュメント、プロジェクトルールなど、根拠となる情報に基づいて書く。
- 仮定、見積もり、未解決事項を明示する。
- 決定事項と未決事項を分ける。
- 同じ事実を複数ドキュメントに重複記載しない。
- 実装済みの振る舞いはコードを根拠にする。ただし設計意図との不一致は、黙って書き換えず差分として明示する。
- ドキュメントを追加・移動するときは、索引を作成または更新して導線を維持する。

## ディレクトリ構成

```text
.
|-- SKILL.md
|-- README.md
|-- agents/
|   `-- openai.yaml
`-- references/
    |-- api-design.md
    |-- basic-design.md
    |-- batch-design.md
    |-- db-design.md
    |-- detailed-design.md
    |-- document_indexing.md
    |-- impact-and-cautions.md
    |-- kabureka-patterns.md
    |-- observability.md
    |-- requirements.md
    |-- screen-design.md
    |-- split-design-docs.md
    `-- test-viewpoints.md
```
