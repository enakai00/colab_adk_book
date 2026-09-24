# 実践！Google Cloud と ADK による AI エージェント開発入門：サンプルコード集

**[注意]**

- 大規模言語モデルの出力は確率的に変化するため、ノートブックに記載のコードの実行結果（AI エージェントの応答メッセージ）は実行ごとに変化します。
- GitHub のページでノートブックファイルを開くと、コードの内容が崩れて表示されることがあります。コードをコピペする際は、ダウンロードしたノートブックファイルを Colaboratory で開いて行うようにしてください。

## 事前準備で Cloud Shell から実行するコマンド

### API の有効化
```
gcloud services enable \
    aiplatform.googleapis.com \
    mapstools.googleapis.com \
    cloudkms.googleapis.com  \
    iamcredentials.googleapis.com \
    cloudresourcemanager.googleapis.com \
    apphub.googleapis.com
```

### サービスエージェントの作成
```
PROJECT_ID=$(gcloud config list --format 'value(core.project)' 2>/dev/null)
PROJECT_NUMBER=$(gcloud projects describe $PROJECT_ID --format='value(projectNumber)' 2>/dev/null)
gcloud beta services identity create \
    --service=aiplatform.googleapis.com --project=$PROJECT_ID
```

### サービスエージェントへの IAM ロールの付与
```
gcloud projects add-iam-policy-binding $PROJECT_ID \
    --member="serviceAccount:service-${PROJECT_NUMBER}@gcp-sa-aiplatform.iam.gserviceaccount.com" \
    --role='roles/storage.objectUser'
gcloud projects add-iam-policy-binding $PROJECT_ID \
    --member="serviceAccount:service-${PROJECT_NUMBER}@gcp-sa-aiplatform-re.iam.gserviceaccount.com" \
    --role='roles/storage.objectUser'
```

## サンプルコードをダウンロードする際にノートブック上で実行するコマンド

### Google Drive のマウント
```
from google.colab import drive
drive.mount('/content/gdrive')
```

### GitHub リポジトリのクローン
```
%%bash
cd '/content/gdrive/My Drive/Colab Notebooks'
git clone https://github.com/enakai00/colab_adk_book.git
```
