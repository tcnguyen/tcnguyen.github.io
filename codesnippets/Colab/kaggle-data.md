---
layout: page
title: Colab - Download Kaggle Data
---
## Setup
Note that you may have many notebooks that run on a same server so you may need to do this once.

### Install Kaggle CLI
Install Kaggle CLI on the virtual server where the the notebook is running. 

```bash
!pip install kaggle
```

### Download Kaggle Authentication Key

Go to your Kaggle account and create a new Authentication Key. You will get a `kaggle.json` file. Upload it to your Google Drive

### Upload the key to the colab virtual server

Run this in the notebook to authenticate to Google Drive and select the Kaggle key. It will be put to `/content/.kaggle/` folder.
``` python
from googleapiclient.discovery import build
import io, os
from googleapiclient.http import MediaIoBaseDownload
from google.colab import auth
auth.authenticate_user()
drive_service = build('drive', 'v3')
results = drive_service.files().list(
        q="name = 'kaggle.json'", fields="files(id)").execute()
kaggle_api_key = results.get('files', [])
filename = "/content/.kaggle/kaggle.json"
os.makedirs(os.path.dirname(filename), exist_ok=True)
request = drive_service.files().get_media(fileId=kaggle_api_key[0]['id'])
fh = io.FileIO(filename, 'wb')
downloader = MediaIoBaseDownload(fh, request)
done = False
while done is False:
    status, done = downloader.next_chunk()
    print("Download %d%%." % int(status.progress() * 100))
os.chmod(filename, 600)
```

Run this to check that the Kaggle key is uploaded succesfully.

```
%ls /content/.kaggle/
```

## Kaggle commands

### Get Kaggle competition list

```
!kaggle competitions list
```

### Get Kaggle competition files list

```
!kaggle competitions files -c dogs-vs-cats
```

### Download Kaggle competition files

```
!kaggle competitions download  -c dogs-vs-cats -p /content/kaggle/dogscats
```
