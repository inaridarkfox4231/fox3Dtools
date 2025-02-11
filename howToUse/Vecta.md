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
　対象はベクトル限定とします。自身とそのベクトルの間のなす角を、0～PIの範囲内で絶対値として返します。いわゆるゼロの判定はMath.atan2()に従うものとします。つまり0になるわけですが、ここはこだわっても仕方のないところかと思うのでナイーブに済ませてあります。
```js
  const v0 = new Vecta(1,0,0);
  const v1 = new Vecta(0,1,0);
  const a01 = v0.angleBetween(v1); // PI/2
  const a10 = v1.angleBetween(v0); // PI/2
  const v2 = new Vecta();
  const a22 = v2.angleBetween(v2); // 0
```
