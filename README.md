# Image Inpainting Web App

背景パターンを保持しながらオブジェクトを消去する Image Inpainting Webアプリケーション。

## 機能

複数の Inpainting 手法をサポート:

| 手法 | 説明 | 必要な環境 |
|------|------|-----------|
| OpenCV NS | Navier-Stokes法 | 標準 |
| OpenCV Telea | Fast Marching法 | 標準 |
| Texture Aware | テクスチャ考慮型（独自実装） | 標準 |
| LaMa | AI (Large Mask Inpainting) | `simple-lama-inpainting` |
| Stable Diffusion | AI (Diffusion Model) | `diffusers` + GPU |
| ClipDrop | Cloud API | APIキー |
| Replicate | Cloud API | APIキー |

## セットアップ

### 基本インストール

```bash
# Python 3.12 推奨
python -m venv .venv
.venv\Scripts\activate  # Windows
# source .venv/bin/activate  # Linux/Mac

pip install -r requirements.txt
```

### LaMa (ローカルAI) を使用する場合

**重要:** Anaconda/Miniconda ではなく、[Python公式版](https://www.python.org/downloads/) の使用を推奨します。

```bash
# 1. pip を最新化
pip install --upgrade pip

# 2. PyTorch のインストール（CPU版）
pip install torch torchvision --index-url https://download.pytorch.org/whl/cpu

# 3. LaMa のインストール
pip install simple-lama-inpainting
```

初回実行時にモデル（約200MB）が自動ダウンロードされます。

#### よくあるトラブル

| 症状 | 原因 | 解決策 |
|------|------|--------|
| `No module named 'torch'` | PyTorch未インストール | 上記の手順2を実行 |
| インストールが途中で止まる | ネットワーク問題 | 再試行、またはVPN経由で試す |
| `ModuleNotFoundError` | Anaconda環境の問題 | Python公式版で仮想環境を作り直す |

> **詳細なトラブルシューティングは [LAMA_INSTALL_GUIDE.md](./LAMA_INSTALL_GUIDE.md) を参照してください。**

## 起動

```bash
# Windows
start.bat

# または直接
python app.py
```

ブラウザで http://localhost:5001 を開いてください。

## 使い方

1. 画像をアップロード（またはデモ画像を使用）
2. 消したい部分を赤色でマスク
3. Inpainting 手法を選択
4. 「実行」ボタンをクリック

## スクリーンショット

（準備中）

## ライセンス

MIT
