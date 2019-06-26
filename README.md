# Web AR Viewer
ブラウザで3DモデルをAR表示する機能を検証  
動作環境は限られるが設定が簡単

検証は以下2つ
- [ARCore: Scene Viewer](https://developers.google.com/ar/develop/java/scene-viewer)
- [ARKit: AR Quick Look](https://developer.apple.com/jp/arkit/gallery/)

[デモ](https://ohagip.github.io/research-web-ar-viewer/)  
上から順に、Scene Viewer（iOS非対応）、Scene Viewer（iOS対応）、AR Quick Look


## ARCore Scene Viewer
Androidで利用可能なビューア

Android以外は、ARは利用できないがモデルのビューアは利用できる  
iOSの場合はAR Quick Lookを利用することができる

### 動作環境
- Android 7.0以降（端末によっては8.0以降）
- ARCore 1.9以降（Playストアでアップデート）
など詳細は[こちら](https://developers.google.com/ar/discover/supported-devices)  

そん他のブラウザ  
Chrome, Firefox, Safari, Edgeの最新から2つ前のバージョンまで  
IE11も可能（ただしポリフィルの設定が必要）  
[Browser Support](https://github.com/GoogleWebComponents/model-viewer#user-content-browser-support)

### 対応モデル
対応フォーマットは`glTF 2.0/glb`

推奨モデルサイズ
- 頂点数: 30,000 
- マテリアル: 10
- テクスチャ: 2048x2048

など詳細は[こちら](https://developers.google.com/ar/develop/java/scene-viewer#file_requirements_for_models)を参照

### 実装方法
scriptを読み込み、Web Componentsにモデルのリンクを設定
```html
<body>
  <model-viewer ar src="model.gltf"></model-viewer>
  <script type="module" src="https://unpkg.com/@google/model-viewer@v0.5.0/dist/model-viewer.js"></script>
</body>
```
[設定可能な項目](https://googlewebcomponents.github.io/model-viewer/index.html#section-attributes)


## AR Quick Look
iOSで利用可能なビューア

iOS以外のブラウザでは動作せず、モデルのファイルがダウンロードされる  
そのため不要な場合、ブラウザ判定で非表示にするなどの対応が必要  
サーバで3Dモデルファイルの`MIME type` を設定する必要があるため（`.htaccess`など）、設定できない場合は利用できないかもしれない（未検証）

リファレンスは[こちら](https://developer.apple.com/videos/play/wwdc2018/603/)の動画、もしくはページ内のPresentation Slides (PDF)を参照

### 動作環境
iOS 12以降

### 対応モデル
対応フォーマットは`usdz`のみ

推奨モデルサイズ
- ポリゴン: 100,000
- テクスチャ: 2048x2048PBR
- アニメーション: 10秒
- マテリアルとテクスチャは1セットがいい

など詳細はリファレンスのPDF参照

`usdz`はXcode10に組み込まれているコマンドラインツールで変換  
変換可能なフォーマットは`OBJ` `Alembic` `USD`

### 実装方法
モデルのリンクを設定
```html
<a rel="ar" href="model.usdz">
  <img src="model-preview.jpg">
</a>
```

サーバで設定する`MIME type`
```
AddType model/vnd.pixar.usd .usdz
AddType model/usd usdz
```