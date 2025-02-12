# Vecta
 3次元のベクトルのクラスです。必要最低限の内容です。
## constructor
　引数を列挙して使います。無い場合は大きい方から順に0が格納されます。
```js
  const v0 = new Vecta(); // 0,0,0
  const v1 = new Vecta(2,3); // 2,3,0
  const v2 = new Vecta(4,5,6); // 4,5,6
```
## set
　対象となるベクトルや配列の内容をコピーします。単数の場合はすべて一緒になります。
```js
  const v = new Vecta();
  v.set([2,3,5]); // 2,3,5
  v.set(new Vecta(4,7,9)); // 4,7,9
  v.set(1); // 1,1,1
  v.set(2,3,-1); // 2,3,-1
```
## copy
　自分のコピーを生成します。これは別のオブジェクトとなります。
```js
  const v = new Vecta(1,3,9);
  const w = v.copy(); // 1,3,9
```
## show
　ベクトルの成分を表示します。戻り値は内容を示す文字列ですが、引数にtrueを指定することで、実行すると同時にその内容をコンソールに出力します。デフォルトはfalseなので、文字列が返されるのみです。
```js
  const v = new Vecta(1,2,3);
  const strings = v.show(); // 1, 2, 3（コンソール出力無し）
  v.show(true); // コンソール出力「1, 2, 3」
```
## array
　ベクトルの成分からなる長さ3の配列を返します。
```js
  const v = new Vecta(3,2,1);
  const vArray = v.array(); // [1, 2, 3]
```
## add
　対象となるベクトルを加えます。引数には配列、ベクトル、列挙が使えます。数が一つの場合、全部同じとみなします。また、最後に真偽値を加えると、デフォルトはfalseですが、trueの場合は別の新しいオブジェクトが返されます。
```js
  const v = new Vecta(1,2,3);
  v.add(4,2,9); // 5,4,12
  v.add([1,2,4]); // 6,6,16
  v.add(new Vecta(3,1,10)); // 9,7,26
  v.add(5); // 14,12,31
  const w = v.add(1,2,7,true); // 15,14,38
  v.show(true); // 14, 12, 31
```
## sub
　引き算です。内容的にはaddで足すところを引くだけなので割愛します。
## mult
　掛け算です。内容的にはaddで足すところを掛けるだけなので割愛します。
## div
　割り算です。内容的には（略）割る（略）。ただ、0の場合はNumber.EPSILONで置き換える処理にしています。つまりエラー処理をしていません。なので運用には注意してください。たとえば0割る0は0になるということです。
## cross
　いわゆる外積です。これもimmutableについてはaddやsubと同じ流儀に従っているので、真偽値をtrueにしないと違うベクトルを返すことにはなりません。crossはそういう使い方の方が多いのは承知していますが、整合性の観点から揃えることにしました。
```js
  const v0 = new Vecta(1,2,3);
  const v1 = new Vecta(3,2,4);
  const v2 = v0.cross(v1, true); // 2,5,-4
  v0.show(true); // 1,2,3
  v0.cross(v1); // 2,5,-4
```
## angleBetween
　第一引数（唯一の引数）はベクトル限定とします。自身とそのベクトルの間のなす角を、0～PIの範囲内で絶対値として返します。いわゆるゼロの判定はMath.atan2()に従うものとします。つまり0になるわけですが、ここはこだわっても仕方のないところかと思うのでナイーブに済ませてあります。
```js
  const v0 = new Vecta(1,0,0);
  const v1 = new Vecta(0,1,0);
  const a01 = v0.angleBetween(v1); // PI/2
  const a10 = v1.angleBetween(v0); // PI/2
  const v2 = new Vecta();
  const a22 = v2.angleBetween(v2); // 0
```
## angleTo
　第一引数はベクトルですが、第二引数として軸ベクトルを取ることができます。指定がない場合、ベクトル(0,0,1)が採用されます。内容としては、軸ベクトルの方向から見た場合の、自身をそのベクトルに回す際の符号付きの角度を返します。すなわちp5のangleBetweenと同じ挙動ですが、あちらと異なり軸は自由に取ることができます。軸ベクトルの方に限り、列挙、配列、ベクトルによる指定が可能です。先ほども述べたように未指定の場合は(0,0,1)となるので、p5のangleBetweenと同じ挙動となります。
```js
  const v0 = new Vecta(1,0,0);
  const v1 = new Vecta(0,1,0);
  const a01 = v0.angleTo(v1); // PI/2
  const a10 = v1.angleTo(v0); // -PI/2
  const v2 = new Vecta(0,0,1);
  const a = v1.angleTo(v2, v0); // PI/2
  const a = v2.angleTo(v1, v0); // -PI/2
```
## rotate
　軸ベクトルと回転角度を指定することで、軸ベクトルの周りに角度の分だけ対象を回転させます。3次元の回転です。軸の指定方法はベクトル、配列、成分列挙の三種類です。成分単独は認められていません。回転量はスカラー限定です。なおこれも最後に真偽値を指定することで、もしtrueであれば異なるベクトルを返すようになっています。デフォルトはfalseなので、対象を回転させます。
```js
  const v0 = new Vecta(1,0,0);
  v0.rotate(0,0,1,PI/4).show(true); // 1/sqrt(2), 1/sqrt(2), 0
  v0.rotate([0,0,1],PI/4).show(true); // ほぼ0,1,0
  v0.rotate(new Vecta(0,0,1),PI/2).show(true); // ほぼ-1,0,0
 
  const v1 = new Vecta(1,0,0);
  v1.rotate(0,0,1,PI/4,true).show(true); // 1/sqrt(2), 1/sqrt(2), 0
  v1.rotate([0,0,1],PI/4,true).show(true); // 1/sqrt(2), 1/sqrt(2), 0
  v1.rotate(new Vecta(0,0,1),PI/2,true).show(true); // ほぼ0,1,0
  v1.show(true); // 1,0,0
```
## addScalar
　引数はベクトルとスカラーです。ベクトルのスカラー倍を加えるものです。真偽値をtrueにすると新しいベクトルを返します。指定方法はrotateと同じで3種類です。
```js
  const v0 = new Vecta(1,1,1);
  v0.addScalar(1,2,3,4).show(true); // 5,9,13
  v0.addScalar([1,2,3],4).show(true); // 9,17,25
  v0.addScalar(new Vecta(1,2,3),4).show(true); // 13,25,37
  v0.show(true); // 13,25,37
 
  const v1 = new Vecta(1,2,3);
  v1.addScalar(3,2,1,4,true).show(true); // 13,10,7
  v1.addScalar([3,2,1],4,true).show(true); // 13,10,7
  v1.addScalar(new Vecta(3,2,1),4,true).show(true); // 13,10,7
  v1.show(true); // 1,2,3
```
## lerp
　線形補間ですね。引数はやはりベクトルとスカラーです。つまり同じ指定方法です。trueだと新しいベクトルを返します。
```js
  const v0 = new Vecta(4,2,1);
  v0.lerp(8,6,5,0.5).show(true); // 6, 4, 3
  v0.lerp([8,6,5], 0.25).show(true); // 6.5, 4.5, 3.5
  v0.lerp(new Vecta(10,10,10),0.5).show(true); // 8.25, 7.25, 6.75
 
  const v1 = new Vecta(10,20,40);
  v1.lerp(4,3,2, 0.5, true).show(true); // 7,11.5,21
  v1.lerp([4,3,2],0.5,true).show(true); // 同上
  v1.lerp(new Vecta(4,3,2),0.5,true).show(true); // 同上
  v1.show(true); // 10,20,40
```
## dot
　いわゆる内積ですね。addやsubと同じ指定方法を許しています。ただ多くの場合は単独のベクトルの引数を指定することでしょう。
```js
  const v0 = new Vecta(1,3,5);
  const v1 = new Vecta(4,3,2);
  const ip = v0.dot(v1); // 23
```
## dist
　2つのベクトルの差のノルムを返します。要するに距離です。ほとんどの場合は単独のベクトルを指定することと思います。
```js
  const v0 = new Vecta(1,2,3);
  const v1 = new Vecta(2,4,5);
  const d01 = v0.dist(v1); // 3
```
## mag
　ベクトルの大きさを返します。いわゆるノルムです。
```js
  const v = new Vecta(1,2,2);
  const n = v.mag(); // 3;
```
## magSq
　ベクトルの成分の2乗の和を返します。magは実はこれの平方根で定義しています。
```js
  const v = new Vecta(1,2,2);
  const n = v.magSq(); // 9;
```
## normalize
　正規化する関数です。これに関してはimmutableを許していません。基本的に対象のベクトルを正規化する形でしか使わないでしょうから。なおゼロベクトルの場合はゼロとします。
```js
  const v = new Vecta(1,2,2);
  v.normalize(); // 1/3, 2/3, 2/3
```
## static: create
　コンストラクタとは違って、配列やベクトルを引数に取れる生成用の関数です。配列からベクトルを作るのに使う可能性があるかもしれません。
```js
  const v = Vecta.create([1,2,4]); // 1,2,4
```
## static: validate, validateForAngleTo, validateForScalar
　一応供用されてはいますが、これらはバリデーション用の関数なので、わざわざ使う機会はないと思います。
## static: getOrtho
　与えられたベクトルに直交する単位ベクトルを、特定の規則に従ってひとつだけ提供します。ゼロベクトルの場合は(0,0,1)を返します。地味に重宝する関数です。
## static: random2D
　単位円周上のベクトルをランダムで一つだけ返します。要するにz成分は0で、x,y成分は三角関数で与えられます。
## static: random3D
　単位球面上のベクトル、すなわちノルムが1のベクトルをランダムで一つだけ返します。
## static: random3Dinside
　単位球面上のベクトルを返すんですが、第一引数に軸ベクトル、第二引数に角度を指定します。軸ベクトルは正規化されます。軸ベクトル方向の、角度より小さい範囲からランダムに球面上のベクトルを抽出します。たとえばPI/2を指定すれば軸ベクトル方向の半球面から選出されます。
　実行した結果がこちらです。傘のような領域にちりばめています。

![wref34f44](https://github.com/user-attachments/assets/7aab11a1-2ea4-402a-8d97-350296770c3b)

## static: random3Doutside
　insideに似ていますが、こちらは軸ベクトルとは反対側の球面上から選ばれます。たとえば角度をPI/6に指定すればPI/6～PIの範囲、つまり軸ベクトル方向に穴が開いたような領域からの選出となります。逆ベクトルで似たようなことをしているわけです。  
　実行した結果がこちらです。上方向の軸でやっています。3Dでないと可視化できないので、効果を実感するのが難しいのが難点ですね。線とかで実験してみるといいと思います。

![edgergeg4h](https://github.com/user-attachments/assets/e1a2c771-e57b-4d13-8bdc-71cc810883be)

　以上です。p5非依存ではありますが、p5で便利な関数もいくつか用意されています。
