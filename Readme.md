
# nameco

「なめこ」は「状態遷移」をコントロールするJavaScriptのライブラリです。現在はアイデアのみの段階です。

## API Draft

「なめこ」が提供するAPIは、(1) 構造定義のAPI、(2) 状態遷移のAPI、の2つに大別されます。ユーザはまず前者を用いて、自分が扱う世界をグラフ構造として、すなわちノードと、ノード間を結ぶエッジの集合としてモデル化します。続いて後者のAPIによって、特定のイベント（たとえば「次ヘ」ボタンがクリックされた、など）に対応する状態間の遷移をトリガします。状態遷移が始まると、目的地のノードまでの経路を構成するエッジに登録されたコールバックが順々に実行されます。コールバックには`move(data)`で指定した`data`が渡されるので、目的地に到達するまでに入力を加工することができます。これらの実装の一部には、より一般的な用途を想定したグラフ生成用のライブラリ（現在制作中）を使う予定です。


### 構造定義

    require('nameco');
    
    var manager = nameco(),
        foo = manager.node('foo'),
        bar = manager.node('bar'),
        baz = manager.node('baz'),
        qux = manager.node('qux');
    
    foo
      .connect(bar) // fooとbarを連結する
      .through(function(err, data, next) { 
        // fooからbarへ遷移するときのコールバック
        next(data + 1); // データを加工して次のコールバックに渡す
      })
      .through(function(err, data, next) {
        // fooからbarへ遷移するときのコールバック（2つめ）
        next(new Error('error')); // エラーオブジェクトの渡せる
      })
      .connect(baz) // fooとbazを連結するが、後ろにthrough()がないので何もしない
      .then()
        // then()を使うと最後に連結したノード、つまりbazに進む
        .connect(qux)
        .through(function(err, data, next) {
          // bazからquxへ遷移するときのコールバック
          next();
        })
      .end()
      // end()を使うとthen()から離脱する、つまりfooに戻る
      // この後もいろいろ書ける
      
      
    /*
     *  以上の操作でこんな感じのグラフができる
     *
     *  foo --- bar
     *   |
     *  baz --- qux
     */
  
### 状態遷移

    nameco.currentNode.move(data).via('foo').to('bar'); // fooを経由してbarへ遷移する
    nameco.currentNode.skip(data).to('bar'); // コールバックをスキップしてbarへ遷移する
    
## 「なめたけ」との関わり

「なめこ」は将来的には[「なめたけ」](http://github.com/hitsujiwool/jquery-nametake)のコアモジュールとして機能する予定です。つまり「なめこ」に対して、(1) グラフの構造をツリーに限定し、(2) 遷移管理のインターフェースをウェブの画面遷移に特化させ、(3) 「なめこ」が関与しないDOMレベルでのAPIを追加したもの、が「なめたけ」という位置付けになります。ちなみに「なめこ」というライブラリの名前も"NAMEtake COre"に由来します。

「なめたけ」の基本的なコンセプトは、「ウェブにおける画面遷移の演出は、ノード間を移動する際に生じる副作用である」という考え方です。「なめたけ」においては、パスを流れるデータをどう加工して、次のノードに受け渡すかはあまり重要ではありません。むしろ`through()`に渡したコールバック処理に記述した内容が副次的な効果として画面遷移の演出を行う、というイメージです。

## 懸案事項

* 現状の構造定義用APIでは孤立ノードを作成できないが、それで良いのか？
* 「ノードはルーティング、エッジが実際の処理」というモデルをやめて、「何らかの処理を行うノード同士をpipeする」という発想にした方が良いかもしれない。直感的には現在のノードをエッジに、エッジをノードに反転させるイメージ。こちらの方が「状態」をダイナミックにとらえることができそう。ただ、ウェブの静的な画面遷移という応用にうまく結びつかないのが難点。
  



## License 

(The MIT License)

Copyright (c) 2012 hitsujiwool &lt;utatanenohibi@gmail.com&gt;

Permission is hereby granted, free of charge, to any person obtaining
a copy of this software and associated documentation files (the
'Software'), to deal in the Software without restriction, including
without limitation the rights to use, copy, modify, merge, publish,
distribute, sublicense, and/or sell copies of the Software, and to
permit persons to whom the Software is furnished to do so, subject to
the following conditions:

The above copyright notice and this permission notice shall be
included in all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED 'AS IS', WITHOUT WARRANTY OF ANY KIND,
EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF
MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT.
IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY
CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT,
TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE
SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.
