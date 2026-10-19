# Alchemy Protocol

[FRICK-ELDY](https://github.com/FRICK-ELDY) 管理の **ワイヤ契約**（主に **Protobuf `.proto`**）の単一ソースです。ゲームルールや公式状態のドメイン SSoT は [alchemy-engine](https://github.com/FRICK-ELDY/alchemy-engine) 側の Elixir にあります（二層の整理）。

## 含まれるもの

- `proto/` — Zenoh 等で共有するメッセージ定義（`alchemy.render` / `alchemy.input` 等）
- `docs/wire-overview.md` — 経路・ペイロードの要約とエンジン側詳細ドキュメントへのリンク

## バージョン方針

- 本リポジトリのみ **`vMAJOR.MINOR.PATCH`** 形式の Git タグでバージョンを打ちます（alchemy-engine のリリース番号とは独立）。
- **破壊的変更**（フィールド削除・型変更・番号の再利用など）は `MAJOR` を上げ、`CHANGELOG.md` に必ず記載してください。
- **同一 MAJOR 内**の既定の後方互換変更は、proto3 における **フィールド追加のみ** とします。`reserved` の運用は `CONTRIBUTING.md` を参照してください。

## 消費者側の取り込み（推奨）

実行リポジトリ（[alchemy-world-server](https://github.com/FRICK-ELDY/alchemy-world-server) 等）は **生成物（Elixir `*.pb.ex` / Rust の生成コード）をコミット**し、日常ビルドでは本リポのツリーを必須にしません。

追従するときは本リポの **タグを固定**し、`PROTO_ROOT` に本リポの `proto/` を明示して再生成し、**Elixir と Rust の生成物を同一 PR**で更新します。

### スーパープロジェクト（alchemy-engine）での配置

[alchemy-engine](https://github.com/FRICK-ELDY/alchemy-engine) では閲覧・再生成用に submodule パス **`protocol/`** でピンします（ビルド契約ではない）。推奨 SHA はエンジン側 `.workspace/0_docs/protocol-lock.md` を参照。

```bash
# 例: 親一式を clone している場合の再生成
export PROTO_ROOT="$PWD/protocol/proto"   # Windows は set PROTO_ROOT=...
cd world-server
mix alchemy.gen.proto
```

コードに `../protocol` を焼き込まないこと。必ず **環境変数等で明示**する。

### 旧方式（廃止予定）

以前のモノレポ／world-server 内 **`3rdparty/alchemy-protocol`** は過渡的なコピーです。`proto/` 内容は本リポのタグ（例: `v0.1.2`）と揃えています。段階的に削除し、上記の生成物コミット運用へ移行します。

### Mix git 依存 + sparse（任意）

```elixir
{:alchemy_protocol_files,
  git: "https://github.com/FRICK-ELDY/alchemy-protocol.git",
  sparse: "proto",
  ref: "v0.1.2",
  compile: false,
  app: false}
```

`sparse: "proto"` のとき、展開先のルートが `.proto` の親になります。`PROTO_ROOT` にそのパスをそのまま渡し、末尾に `/proto` を二重に付けないでください。

## ローカル検証

```bash
protoc -I proto --include_imports --descriptor_set_out=/tmp/alchemy-descriptor.pb \
  proto/render_frame.proto proto/input_events.proto proto/frame_injection.proto proto/client_info.proto
```

Windows の場合は `--descriptor_set_out=%TEMP%\alchemy-descriptor.pb` などに置き換えてください。

## ライセンス

[Eclipse Public License 2.0](LICENSE)（alchemy-engine と同一）。
