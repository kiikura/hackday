# hackday

## Rasp pi で SoundCloud Streaming

鳴らすだけならおもったより簡単にできた。

まずGstreamerとsoundcloudのpython sdkをいれる（後者はなくてもいいかしら。。。）
'''
sudo apt-get install gstreamer1.0
pip install soundcloud
pip install distribute --upgrade
sudo pip install soundcloud
'''

アプリ登録をする（client_idとclient_secretを取得するため）

access_tokenを取得する（oauth2なしで取るタイプ）

```
t=requests.post(api_url,data={"client_id":"d30b38b7c3cd11b951448eea57165b2b",
     "client_secret":'1d8ba78b0500b9360e012f88e888bfff',
     'username':"ken.iikura.xe@hitachi-systems.com",
     "password":"kankan2403",
     "grant_type":"password"})
t.text
```
t.textがjsonなんでそんなかに入ってる

あとはstream_urlの後ろにaccess_tokenとclient_idをくっつけてcurlよべばとれる。
それをgstreamerの引数にすればこんどは鳴る。

```
sudo gst-launch-1.0 -v playbin uri= "https://api.soundcloud.com/tracks/213656775/stream?access_token=1-136912-161792722-3395d26512b3f&client_id=d30b38b7c3cd11b951448eea57165b2b"
```

paybin2がよかったんだけど1.0だとない感じ？
音質わるいけど我が家のrasp piだと30分で鳴らせました。

あとはpythonのラッパーから制御すれば細かく制御できそう。鳴らすだけならコマンドでもいいんじゃない？
