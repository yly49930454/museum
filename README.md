# 博物馆动静皆宜
## 加值宣言
身障者一般不方便上前观看解说词，本可以拍摄展品识别后显示解说词，若用户需要继续观察展品，则可以选择语音播报听取解说词
## 核心价值
让身障者可以和普通人一样观看展品以及观看解说词

## 用户痛点
* 身障者不方便上前观看展品解说词
* 用户想边看展品边听解说词

## 用户需求列表与API

| 用户需求|API|重要程度 |
| ---|—--|--- |
| 用户无需上前即可获取解说词|百度图像识别API|重要 |
| 用户可以边观看展品边听取解说词|百度语音合成API|重要 |


## 原型
* 使用手机相机对展品进行拍摄并通过百度图像识别API对资料库里的内容进行对比，并显示展品介绍
* 点击小喇叭按钮就可以使用百度语音合成API进行语音播报
![产品原型](https://github.com/yly49930454/museum/blob/master/media/产品原型.png)
## 人工智能概率

用户若是识别错误可以重新拍照或上传，并无重大影响

## API的运用及其价值宣言
1. 百度相似图像搜索API
* HTTP 方法：POST
* 请求URL：https://aip.baidubce.com/rest/2.0/image-classify/v1/realtime_search/similar/search
* API价值主张：相似图片搜索，可以建立一个本地的图像库，当用户上传时可以跟库里的图片进行比对，相似度最高的返回其的展品介绍
* API使用实例
``` python
import base64
f = open(‘th (1).png’, ‘rb’)
img = base64.b64encode(f.read())

import requests
host = ‘https://aip.baidubce.com/rest/2.0/image-classify/v1/realtime_search/similar/search’
headers={
   ‘Content-Type’:’application/x-www-form-urlencoded’
}
access_token= ’24.c95f780f275c859fa0463438cdc9ebda.2592000.1578373144.282335-17966623’
host=host+’?access_token=‘+access_token

data={}
data[‘access_token’]=access_token
data[‘image’] =img
res = requests.post(url=host,headers=headers,data=data)
req=res.json()
print(req[‘result’])
```
* API返回实例
``` 
{
    “result_num”: 1,
    “result”: [
        {
            “score”: 0.97976700290421,
            “brief”: “./data/jay1.jpg”,
            “cont_sign”: “475124309,1080176642”
        }
    ],
	“has_more”: “false”,
    “log_id”: 1968648150
}
```

2. 百度语音合成API
* HTTP 方法：POST
* 请求URL：http://tsn.baidu.com/text2audio 
* API价值主张： 将提供高度拟人、流畅自然的语音合成服务，让您的应用、设备开口说话，更具个性

* API使用实例
``` python
  from aip import AipSpeech

  “”” 你的 APPID AK SK “””
  APP_ID = ‘你的 App ID’
  API_KEY = ‘你的 Api Key’
  SECRET_KEY = ‘你的 Secret Key’

  client = AipSpeech(APP_ID, API_KEY, SECRET_KEY)
  

  result  = client.synthesis(‘你好百度’, ‘zh’, 1, {
    ‘vol’: 5,
})

# 识别正确返回语音二进制 错误则返回dict 参照下面错误码
if not isinstance(result, dict):
    with open(‘auido.mp3’, ‘wb’) as f:
        f.write(result)  

```









