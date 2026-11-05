cd "/Users/neromehiro/hiro folder/my_Works/programing/SOME" ; python SOME/infer.py --model ./SOME/experiments/some-v0.0.1/model_ckpt_steps_100000_simplified.ckpt --wav ./SOME/data/さいた.wav


# SOME

**SOME: 歌声向け MIDI 抽出器（Singing-Oriented MIDI Extractor）**

> **警告**
>
> 本プロジェクトは現在ベータ版です。後方互換性は保証されていません。

## 概要

SOME は、歌声を MIDI シーケンスに変換できる MIDI 抽出ツールで、以下の特長があります。

1. **高速**：i5-12400 CPU で実時間の約 9 倍、3080Ti GPU では約 300 倍の速度。
2. **低リソース依存**：カスタムデータセットで学習可能。学習データは 3 時間程度でも良好な結果を得られます。
3. **機能性**：整数に限らない MIDI 値（浮動小数点のピッチ）を出力可能で、DiffSinger のバリアンスラベリングに特に適しています。

## はじめに

> 中文教程 / 中国語チュートリアル：
> [テキスト](https://openvpi-docs.feishu.cn/wiki/RaHSwdMQvisdcKkRFpqclhM7ndc)、[動画](https://www.bilibili.com/video/BV1my4y1N7VR)

### インストール

SOME は **Python 3.8 以上**が必要です。依存関係のインストール前に、Conda や venv で仮想環境を作成することを強く推奨します。

1. OS とハードウェアに応じて、[公式手順](https://pytorch.org/get-started/locally/)に従い **PyTorch 2.1 以上**をインストールします。

2. 以下のコマンドで、その他の依存関係をインストールします。

   ```bash
   pip install -r requirements.txt
   ```

3. （任意）ピッチ抽出精度を向上させるため、[こちら](https://github.com/yxlllc/RMVPE/releases)から RMVPE の事前学習済みモデルをダウンロードし、`pretrained/` ディレクトリに展開してください。

### 事前学習済みモデルによる推論（MIDI ファイル）

SOME の事前学習済みモデルを [releases](https://github.com/openvpi/SOME/releases) からダウンロードし、任意の場所に展開します。

CLI で推論する場合は、次のコマンドを実行します。

```bash
python infer.py --model CKPT_PATH --wav WAV_PATH
```

これにより、`CKPT_PATH` のモデルを読み込み、`WAV_PATH` の音声から MIDI を抽出して MIDI ファイルとして保存します。その他のオプションは以下で確認できます。

```bash
python infer.py --help
```

Web UI で推論する場合は、次のコマンドを実行します。

```bash
python webui.py --work_dir WORK_DIR
```

その後、ブラウザで Gradio インターフェースを開き、Web ページの案内に従って `WORK_DIR` 配下のモデルを使用してください。その他のオプションは以下で確認できます。

```bash
python webui.py --help
```

### 事前学習済みモデルによる推論（DiffSinger データセット）

SOME の事前学習済みモデルを [releases](https://github.com/openvpi/SOME/releases) からダウンロードし、任意の場所に展開します。

既存の DiffSinger データセットで SOME を使用するには、`name`、`ph_seq`、`ph_dur`、`ph_num` を含む `transcriptions.csv` が必要です。次のコマンドを実行してください。

```bash
python batch_infer.py --model CKPT_PATH --dataset RAW_DATA_DIR --overwrite
```

このコマンドは、データセット内の音声から（浮動小数点のピッチ値を含む）すべての MIDI シーケンスを取得し、`transcriptions.csv` を **上書き** して `note_seq` と `note_dur` を追加または置換します。必要に応じて、事前にバックアップを取ってください。

その他のオプションは以下で確認できます。

```bash
python batch_infer.py --help
```

### ゼロからの学習

*学習スクリプトは公開されていますが、まだ十分に整理されていない可能性があります。将来的な安定版リリース後に、自前のモデルを学習することを推奨します。*

## 免責事項

提供者の同意を得ずに入手した録音を学習データとして使用することは、いかなる組織・個人であっても禁止されています。これに違反した場合、著作権法やソフトウェアの EULA に違反する可能性があります。

## ライセンス

SOME は [MIT License](LICENSE) の下で提供されています。