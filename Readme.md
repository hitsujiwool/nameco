
# nameco

## API Draft
    
### 構造定義用のAPI    
    
    var manager = nameco(),
        foo = manager.router('foo'),
        bar = manager.router('bar'),
        baz = manager.router('baz'),
        qux = manager.router('qux');
    
    foo
      .connect(bar)
      .through(function(next) { 
        // fooからbarへ遷移するときのコールバック
        next();
      })
      .through(function(next) {
        // fooからbarへ遷移するときのコールバック（2つめ）
        next();          
      })
      .connect(baz) // fooとbazを連結するが、後ろにthrough()がないので何もしない
      .then()
        // then()を使うと最後に連結したノード、つまりbazに進む
        .connect(qux)
        .through(function(next) {
          // bazからquxへ遷移するときのコールバック
          next();
        })
      .end()
      // end()を使うとthen()から離脱する、つまりfooに戻る    
      ......;
  
### 移動用のAPI

    nameco.currentRouter.move().via('foo').to('bar'); // fooを経由してbarへ遷移する
    nameco.currentRouter.skip().to('bar'); // コールバックをスキップしてbarへ遷移する
    

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
