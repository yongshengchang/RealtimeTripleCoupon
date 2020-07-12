# RealtimeTripleCoupon

全國郵局即時振興三倍券販賣存量
----------------------------

存取、解析  ODS 格式的檔案並製作成WebAPI

資料來源: 

* [全國郵局即時振興三倍券販賣存量](https://quality.data.gov.tw/dq_download_ods.php?nid=127751&md5_url=ff02dedeecfbc115c19dd0dd37db17f4)

## Prpcess

1. 自動去官網下載[全國郵局即時振興三倍券販賣存量]檔案。
2. 解析[ 全國郵局即時振興三倍券販賣存量.ods ] 的檔案。
3. 設定所需欄位並回傳JSON格式的資料。

## Tool

* PuCharm

## Install

By PyPi

```
$ pip install Flask
$ pip inStall pyexcel_ods
```

解析 ODS 格式的檔案並回傳JSON格式資料
----------------------------

```python
from pyexcel_ods import get_data
from flask import jsonify
from urllib.request import urlretrieve
import pyexcel_ods
import json
import os

#新增所需陣列
results = []

urlretrieve ("https://quality.data.gov.tw/dq_download_ods.php?nid=127751&md5_url=ff02dedeecfbc115c19dd0dd37db17f4","./全國郵局即時振興三倍券販賣存量資訊.ods")

data = get_data("全國郵局即時振興三倍券販賣存量資訊.ods", start_row=1, row_limit=1300)

for TABLE in data["Sheet1"]:
      results.append({  
        'hsnCd': TABLE[0],
        'hsnNm': TABLE[1],
        'townNm': TABLE[3],
        'addr': TABLE[6],
        'tel': TABLE[8],
        'busiTime': TABLE[9],
        'busiMemo': TABLE[10],
        'longitude': TABLE[11],
        'latitude': TABLE[12],
        'total': TABLE[13],
        'updateTime': TABLE[14],
      })

@app.route('/')
def start():
    return jsonify(results)

```