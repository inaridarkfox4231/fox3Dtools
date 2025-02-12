# fox3Dtools
3D描画用ツール  
必要最低限の内容です。  
こちらをコピペしてOpenProcessingのあそこに貼り付けるとよいかと思います。  
```
https://inaridarkfox4231.github.io/fox3Dtools/fox3Dtools.js
```
CDNバージョンはこちら：
```html
<script src="https://inaridarkfox4231.github.io/fox3Dtools/fox3Dtools.js"></script>
```
## Vecta
　3次元ベクトルのクラスです。p5で出来そうなことは一通り揃えました。軸周りの回転やちょっと変わったランダム抽出の関数を持っています。
## Quarternion
　いわゆるクォータニオン、四元数のクラスです。正規直交基底を4つの数で表せるだけでなく、それらの軸を自由自在に回転させるのに適した枠組みを提供します。
## MT4
　4次正方行列のクラスです。webGLに必要なTRSモデル変換、及び射影行列の生成に必要な関数を揃えました。射影行列は今のところ投射投影のみ用意してあります。
## QCamera
　ビュー行列を扱うための枠組み、すなわちカメラです。射影の情報は入っていません。あくまでビューのみの取り扱いに徹しています。ステートの管理、及びクォータニオンを使った補間機能を有しています。

　コーディングお絵描きのツールを提供する為というよりは、もうそういうことにはあまり興味が無いので、自分用のまとめのためにやっています。要するに3Dの学習用です。QiitaでwebGLのまとめ記事を自分用に書いているのですが、3Dは数学の道具立てが多くて説明が難しいので、ここにすべてまとめることにしました。  

　読めばわかりますがp5.jsには依存していません。行列など、あちらと異なる流儀を採用しているので、整合性はあまりないと思います。
