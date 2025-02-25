以下は、元の英語ドキュメントの日本語訳です。

---

English | [简体中文](README.md)

# 回転物体検出

## 目次
- [はじめに](#はじめに)
- [モデルゾーン](#モデルゾーン)
- [データ準備](#データ準備)
- [インストール](#インストール)

## はじめに
回転物体検出は、角度情報を含む長方形のバウンディングボックスを検出する技術です。つまり、長方形のバウンディングボックスの長辺と短辺が画像の座標軸と平行でなくなります。通常、回転バウンディングボックスは、水平なバウンディングボックスに比べて背景情報が少なくなります。回転物体検出は、リモートセンシング（衛星画像解析）などのシナリオでよく使用されます。

## モデルゾーン
| モデル | mAP | Lrスケジューラ | 角度 | Aug | GPU数 | images/GPU | ダウンロード | 設定ファイル |
|:---:|:----:|:---------:|:-----:|:--------:|:-----:|:------------:|:-------:|:------:|
| [S2ANet](./s2anet/README_en.md) | 73.84 | 2x | le135 | - | 4 | 2 | [モデル](https://paddledet.bj.bcebos.com/models/s2anet_alignconv_2x_dota.pdparams) | [設定](https://github.com/PaddlePaddle/PaddleDetection/tree/develop/configs/rotate/s2anet/s2anet_alignconv_2x_dota.yml) |
| [FCOSR](./fcosr/README_en.md) | 76.62 | 3x | oc | RR | 4 | 4 | [モデル](https://paddledet.bj.bcebos.com/models/fcosr_x50_3x_dota.pdparams) | [設定](https://github.com/PaddlePaddle/PaddleDetection/tree/develop/configs/rotate/fcosr/fcosr_x50_3x_dota.yml) |
| [PP-YOLOE-R-s](./ppyoloe_r/README_en.md) | 73.82 | 3x | oc | RR | 4 | 2 | [モデル](https://paddledet.bj.bcebos.com/models/ppyoloe_r_crn_s_3x_dota.pdparams) | [設定](https://github.com/PaddlePaddle/PaddleDetection/tree/develop/configs/rotate/ppyoloe_r/ppyoloe_r_crn_s_3x_dota.yml) |
| [PP-YOLOE-R-s](./ppyoloe_r/README_en.md) | 79.42 | 3x | oc | MS+RR | 4 | 2 | [モデル](https://paddledet.bj.bcebos.com/models/ppyoloe_r_crn_s_3x_dota_ms.pdparams) | [設定](https://github.com/PaddlePaddle/PaddleDetection/tree/develop/configs/rotate/ppyoloe_r/ppyoloe_r_crn_s_3x_dota_ms.yml) |
| [PP-YOLOE-R-m](./ppyoloe_r/README_en.md) | 77.64 | 3x | oc | RR | 4 | 2 | [モデル](https://paddledet.bj.bcebos.com/models/ppyoloe_r_crn_m_3x_dota.pdparams) | [設定](https://github.com/PaddlePaddle/PaddleDetection/tree/develop/configs/rotate/ppyoloe_r/ppyoloe_r_crn_m_3x_dota.yml) |
| [PP-YOLOE-R-m](./ppyoloe_r/README_en.md) | 79.71 | 3x | oc | MS+RR | 4 | 2 | [モデル](https://paddledet.bj.bcebos.com/models/ppyoloe_r_crn_m_3x_dota_ms.pdparams) | [設定](https://github.com/PaddlePaddle/PaddleDetection/tree/develop/configs/rotate/ppyoloe_r/ppyoloe_r_crn_m_3x_dota_ms.yml) |
| [PP-YOLOE-R-l](./ppyoloe_r/README_en.md) | 78.14 | 3x | oc | RR | 4 | 2 | [モデル](https://paddledet.bj.bcebos.com/models/ppyoloe_r_crn_l_3x_dota.pdparams) | [設定](https://github.com/PaddlePaddle/PaddleDetection/tree/develop/configs/rotate/ppyoloe_r/ppyoloe_r_crn_l_3x_dota.yml) |
| [PP-YOLOE-R-l](./ppyoloe_r/README_en.md) | 80.02 | 3x | oc | MS+RR | 4 | 2 | [モデル](https://paddledet.bj.bcebos.com/models/ppyoloe_r_crn_l_3x_dota_ms.pdparams) | [設定](https://github.com/PaddlePaddle/PaddleDetection/tree/develop/configs/rotate/ppyoloe_r/ppyoloe_r_crn_l_3x_dota_ms.yml) |
| [PP-YOLOE-R-x](./ppyoloe_r/README_en.md) | 78.28 | 3x | oc | RR | 4 | 2 | [モデル](https://paddledet.bj.bcebos.com/models/ppyoloe_r_crn_x_3x_dota.pdparams) | [設定](https://github.com/PaddlePaddle/PaddleDetection/tree/develop/configs/rotate/ppyoloe_r/ppyoloe_r_crn_x_3x_dota.yml) |
| [PP-YOLOE-R-x](./ppyoloe_r/README_en.md) | 80.73 | 3x | oc | MS+RR | 4 | 2 | [モデル](https://paddledet.bj.bcebos.com/models/ppyoloe_r_crn_x_3x_dota_ms.pdparams) | [設定](https://github.com/PaddlePaddle/PaddleDetection/tree/develop/configs/rotate/ppyoloe_r/ppyoloe_r_crn_x_3x_dota_ms.yml) |

**注意事項:**

- **GPU数**や**ミニバッチサイズ**が変更された場合は、**学習率**も以下の式に基づいて調整する必要があります。  
  **lr<sub>new</sub> = lr<sub>default</sub> * (batch_size<sub>new</sub> * GPU_number<sub>new</sub>) / (batch_size<sub>default</sub> * GPU_number<sub>default</sub>)**
- モデルゾーンに掲載されているモデルは、デフォルトで単一スケールで学習・テストされています。データ拡張の列に `MS` と記載がある場合は、マルチスケール学習およびマルチスケールテストが使用されていることを意味します。`RR` と記載がある場合は、トレーニング時にRandomRotateデータ拡張が使用されていることを示します。

## データ準備
### DOTAデータセットの準備
DOTAデータセットは、回転および水平バウンディングボックスの注釈を含む大規模なリモートセンシング画像データセットです。データセットは[Official Website of DOTA Dataset](https://captain-whu.github.io/DOTA/)からダウンロード可能です。データセットを解凍すると、ディレクトリ構造は以下のようになります。

```
${DOTA_ROOT}
├── test
│   └── images
├── train
│   ├── images
│   └── labelTxt
└── val
    ├── images
    └── labelTxt
```

ラベル付きデータでは、各画像に対して同じ名前のtxtファイルがあり、txtファイル内の各行が回転バウンディングボックスを表します。フォーマットは以下の通りです:

```
x1 y1 x2 y2 x3 y3 x4 y4 class_name difficult
```

#### 単一スケールでの画像スライス
DOTAデータセットの画像解像度は比較的高いため、トレーニングおよびテスト前に画像をスライスすることが一般的です。単一スケールで画像をスライスするには、以下のコマンドを使用します。

``` bash
# ラベル付きデータのスライス
python configs/rotate/tools/prepare_data.py \
    --input_dirs ${DOTA_ROOT}/train/ ${DOTA_ROOT}/val/ \
    --output_dir ${OUTPUT_DIR}/trainval1024/ \
    --coco_json_file DOTA_trainval1024.json \
    --subsize 1024 \
    --gap 200 \
    --rates 1.0
# --image_only オプションを指定して、ラベルなしデータもスライス
python configs/rotate/tools/prepare_data.py \
    --input_dirs ${DOTA_ROOT}/test/ \
    --output_dir ${OUTPUT_DIR}/test1024/ \
    --coco_json_file DOTA_test1024.json \
    --subsize 1024 \
    --gap 200 \
    --rates 1.0 \
    --image_only
```

#### マルチスケールでの画像スライス
複数のスケールで画像をスライスする場合は、以下のコマンドを使用します。

``` bash
# ラベル付きデータのスライス
python configs/rotate/tools/prepare_data.py \
    --input_dirs ${DOTA_ROOT}/train/ ${DOTA_ROOT}/val/ \
    --output_dir ${OUTPUT_DIR}/trainval/ \
    --coco_json_file DOTA_trainval1024.json \
    --subsize 1024 \
    --gap 500 \
    --rates 0.5 1.0 1.5
# --image_only オプションを指定して、ラベルなしデータもスライス
python configs/rotate/tools/prepare_data.py \
    --input_dirs ${DOTA_ROOT}/test/ \
    --output_dir ${OUTPUT_DIR}/test1024/ \
    --coco_json_file DOTA_test1024.json \
    --subsize 1024 \
    --gap 500 \
    --rates 0.5 1.0 1.5 \
    --image_only
```

### カスタムデータセット
回転物体検出では標準のCOCOデータ形式が使用されており、独自のデータセットをCOCO形式に変換してモデルの学習に利用できます。標準のCOCO形式の注釈は、以下の情報を含みます:

``` python
'annotations': [
    {
        'id': 2083, 'category_id': 9, 'image_id': 9008,
        'bbox': [x, y, w, h], # 水平バウンディングボックス
        'segmentation': [[x1, y1, x2, y2, x3, y3, x4, y4]], # 回転バウンディングボックス
        ...
    }
    ...
]
```

**注意:**  
`bbox` は水平バウンディングボックスを表し、`segmentation` は回転バウンディングボックスの4点（時計回りまたは反時計回り）を表します。トレーニング時には `bbox` が空の場合もあり、`segmentation` に基づいて `bbox` を生成することが推奨されます。  
PaddleDetection 2.4以前のバージョンでは、`bbox` が回転バウンディングボックスの [x, y, w, h, angle] を表し、`segmentation` は空でしたが、**PaddleDetection 2.5以降はこのフォーマットはサポートされなくなりました。** 最新のデータセットをダウンロードするか、標準のCOCO形式に変換してください。

## インストール
回転物体検出のモデルは、トレーニングや評価などのために外部オペレーターに依存しています。Linux環境では、以下のコマンドを実行してコンパイルおよびインストールが可能です。

```
cd ppdet/ext_op
python setup.py install
```

Windows環境の場合は、以下の手順を実行してください。

1. Visual Studio（Visual Studio 2015 Update3以降が必要）
2. 「スタート」メニューから「Visual Studio 2017」→「X64 Native Tools コマンドプロンプト for VS 2017」を起動
3. 環境変数を設定：`set DISTUTILS_USE_SDK=1`
4. `ppdet/ext_op`ディレクトリに移動し、`python setup.py install`を実行

インストール後、`ppdet/ext_op/unittest`のユニットテストを実行して、外部オペレーターが正しくインストールされたかを確認できます。

---