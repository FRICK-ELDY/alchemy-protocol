# Changelog

本ファイルに、スキーマの**破壊的変更**と**リリース単位のまとめ**を記載します。

## [0.1.1] - 2026-04-22

### Changed

- `README.md`: Git submodule の配置例を **`vendor/` から `3rdparty/`** に更新（alchemy-engine 側の既定レイアウトと一致）。

## [0.1.0] - 2026-04-22

### Added

- 初回: `proto/` ツリー全体を [alchemy-engine](https://github.com/FRICK-ELDY/alchemy-engine) のワーキングツリーからインポート（移設元 git 短縮ハッシュ: **`a8464e7`**）。
- `README.md` / `CONTRIBUTING.md` / `docs/wire-overview.md` / CI（`protoc` によるコンパイル検証）。
