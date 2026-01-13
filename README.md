# poi-sim

POI（場所カテゴリ）とメッシュ（3次メッシュ）を状態として、人流・滞在分布を前処理し、
調布市周辺の合成人口データと遷移確率を用いたシミュレーションを検討するための作業用リポジトリです。
主な作業は Jupyter Notebook で行います。

## 目的

- 位置情報トレースから POI カテゴリを整理し、滞在・移動の状態系列を構築する
- 夜間滞在から推定居住地（home）を付与する
- メッシュ × POI の遷移/滞在分布を作成し、シミュレーションに利用する

## ディレクトリ構成

- `src/notebook-eda/`：EDA・可視化・集計ノートブック
- `src/notebook-dev/`：シミュレーション実行ノートブック
- `src/preprocessed.ipynb`：前処理の中心ノートブック
- `data/raw/`：生データ（配置のみ想定、`.gitkeep`）
- `data/processed/`：加工済みデータ（配置のみ想定、`.gitkeep`）

## セットアップ

Python 3.13 以上を想定しています。依存関係は `pyproject.toml` を参照してください。

例（uv を使う場合）:

```bash
uv sync
```

例（venv の場合）:

```bash
python -m venv .venv
. .venv/bin/activate
pip install -r <(python - <<'PY'
import tomllib
with open('pyproject.toml','rb') as f:
    data = tomllib.load(f)
print('\n'.join(data['project']['dependencies']))
PY
)
```

## 使い方（ノートブック）

- 前処理: `src/preprocessed.ipynb`
  - POI の正規化
  - 15分スロットへの補完
  - 夜間滞在から home 推定
  - メッシュ × POI の状態系列作成
- シミュレーション: `src/notebook-dev/run_sim.ipynb`
  - 合成人口データ読み込み
  - 遷移確率行列のロード
  - シミュレーション実行

## データについて

`data/` 以下の実データはこのリポジトリに含めていません。
ノートブック内のパスはローカル環境を前提としているため、必要に応じてパスを書き換えてください。

## 補足

- 依存ライブラリは `pyproject.toml` を参照してください
- Notebook の実行順序は `src/preprocessed.ipynb` → `src/notebook-eda/` → `src/notebook-dev/` を想定しています
