# LaMa インストールガイド

LaMa (Large Mask Inpainting) をローカル環境で動かすための詳細な手順です。

## 動作環境

| 項目 | 要件 |
|------|------|
| OS | Windows 10/11, macOS, Linux |
| Python | 3.8 〜 3.12 |
| メモリ | 8GB以上推奨 |
| ディスク | 500MB以上の空き容量（モデルファイル用） |
| GPU | 不要（CPUで動作可能、GPUあれば高速化） |

---

## インストール手順

### Step 1: Python のバージョン確認

```bash
python --version
```

**3.8〜3.12** であることを確認してください。

> もし古いバージョンの場合は、[Python公式サイト](https://www.python.org/downloads/)から3.12をインストールしてください。

---

### Step 2: 仮想環境の作成

プロジェクトフォルダに移動して仮想環境を作成します。

**Windows:**
```powershell
cd C:\path\to\inpaint_demo
python -m venv .venv
.venv\Scripts\activate
```

**macOS / Linux:**
```bash
cd /path/to/inpaint_demo
python3 -m venv .venv
source .venv/bin/activate
```

> コマンドプロンプトの先頭に `(.venv)` が表示されれば成功です。

---

### Step 3: 基本パッケージのインストール

```bash
pip install --upgrade pip
pip install -r requirements.txt
```

---

### Step 4: PyTorch のインストール

LaMaを動かすには **PyTorch** が必要です。

#### CPU版（GPUなしの場合）

```bash
pip install torch torchvision --index-url https://download.pytorch.org/whl/cpu
```

#### GPU版（NVIDIA GPU + CUDA がある場合）

CUDA 11.8 の場合:
```bash
pip install torch torchvision --index-url https://download.pytorch.org/whl/cu118
```

CUDA 12.1 の場合:
```bash
pip install torch torchvision --index-url https://download.pytorch.org/whl/cu121
```

> お使いのCUDAバージョンは `nvcc --version` で確認できます。

#### PyTorch インストール確認

```bash
python -c "import torch; print(f'PyTorch: {torch.__version__}')"
```

バージョン番号が表示されればOKです。

---

### Step 5: simple-lama-inpainting のインストール

```bash
pip install simple-lama-inpainting
```

---

### Step 6: モデルのダウンロード確認

初回実行時に自動でモデル（約200MB）がダウンロードされます。

手動で確認する場合:

```bash
python -c "from simple_lama_inpainting import SimpleLama; SimpleLama()"
```

モデルの保存場所:
- **Windows:** `C:\Users\<ユーザー名>\.cache\torch\hub\checkpoints\big-lama.pt`
- **macOS/Linux:** `~/.cache/torch/hub/checkpoints/big-lama.pt`

---

## 動作確認

### 方法1: Webアプリで確認

```bash
python app.py
```

ブラウザで http://localhost:5001 を開き、Inpainting手法で「LaMa」を選択して実行。

### 方法2: コマンドラインで確認

```bash
python ai_inpaint.py
```

以下のように表示されれば成功:
```
AIモデル利用可能状況:
----------------------------------------
  ✅ lama
  ❌ sd
  ❌ iopaint
----------------------------------------
```

---

## よくあるエラーと解決方法

### エラー1: `ModuleNotFoundError: No module named 'torch'`

**原因:** PyTorchがインストールされていない

**解決:**
```bash
pip install torch torchvision --index-url https://download.pytorch.org/whl/cpu
```

---

### エラー2: `ModuleNotFoundError: No module named 'simple_lama_inpainting'`

**原因:** simple-lama-inpaintingがインストールされていない

**解決:**
```bash
pip install simple-lama-inpainting
```

---

### エラー3: `pip install` で SSL エラーが出る

**原因:** 企業プロキシやセキュリティソフトの影響

**解決:**
```bash
pip install --trusted-host pypi.org --trusted-host files.pythonhosted.org simple-lama-inpainting
```

---

### エラー4: `error: Microsoft Visual C++ 14.0 or greater is required`

> **注意:** このエラーは稀です。通常は事前ビルド済みパッケージが使われるため発生しません。

**原因:** 特殊な環境でソースからのビルドが必要になった場合

**解決:**
1. [Visual Studio Build Tools](https://visualstudio.microsoft.com/visual-cpp-build-tools/) をダウンロード
2. 「C++ Build Tools」をチェックしてインストール
3. PCを再起動
4. 再度 `pip install simple-lama-inpainting`

**代替案:** Python 3.12 の公式版を使うと、このエラーは発生しにくいです。

---

### エラー5: モデルのダウンロードが途中で止まる

**原因:** ネットワーク不安定

**解決:**
1. 手動でモデルをダウンロード:
   - URL: https://github.com/enesmsahin/simple-lama-inpainting/releases/download/v0.1.0/big-lama.pt
2. ダウンロードしたファイルを以下に配置:
   - Windows: `C:\Users\<ユーザー名>\.cache\torch\hub\checkpoints\big-lama.pt`
   - macOS/Linux: `~/.cache/torch/hub/checkpoints/big-lama.pt`

---

### エラー6: `RuntimeError: CUDA out of memory`

**原因:** GPU メモリ不足

**解決:** CPU版を使う
```bash
pip uninstall torch torchvision
pip install torch torchvision --index-url https://download.pytorch.org/whl/cpu
```

---

### エラー7: `AttributeError` や謎のエラー

**原因:** パッケージのバージョン不整合

**解決:** クリーンインストール
```bash
pip uninstall torch torchvision simple-lama-inpainting -y
pip cache purge
pip install torch torchvision --index-url https://download.pytorch.org/whl/cpu
pip install simple-lama-inpainting
```

---

## 環境情報の確認コマンド

問題が解決しない場合、以下の情報を共有してください:

```bash
python --version
pip --version
pip list | grep -E "torch|lama|opencv"
python -c "import torch; print(torch.__version__)"
```

Windows の場合:
```powershell
python --version
pip --version
pip list | findstr "torch lama opencv"
python -c "import torch; print(torch.__version__)"
```

---

## クイックスタート（コピペ用）

### Windows（GPU なし）

```powershell
cd C:\path\to\inpaint_demo
python -m venv .venv
.venv\Scripts\activate
pip install --upgrade pip
pip install -r requirements.txt
pip install torch torchvision --index-url https://download.pytorch.org/whl/cpu
pip install simple-lama-inpainting
python app.py
```

### macOS / Linux（GPU なし）

```bash
cd /path/to/inpaint_demo
python3 -m venv .venv
source .venv/bin/activate
pip install --upgrade pip
pip install -r requirements.txt
pip install torch torchvision --index-url https://download.pytorch.org/whl/cpu
pip install simple-lama-inpainting
python app.py
```

---

## 補足: LaMa なしでも使えます

LaMa がどうしてもインストールできない場合でも、以下の手法は使えます:

| 手法 | 特徴 |
|------|------|
| OpenCV NS | 軽量、標準インストールで動作 |
| OpenCV Telea | 軽量、標準インストールで動作 |
| Texture Aware | 独自実装、テクスチャを考慮 |

これらは `requirements.txt` のインストールだけで動作します。

---

## 質問・問題報告

GitHub Issues: https://github.com/yozu/inpaint-demo/issues
