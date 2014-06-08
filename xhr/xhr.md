# XMLHttpRequestとは
ブラウザ上でサーバーとHTTP通信を行うためのAPIのこと。

## XMLHttpRequestの機能と特徴
* JSONPと機能的に大きな違いはない。
* スキーム、ドメイン、ポート（まとめてオリジン）すべてが一致するURLとしか通信できない点がJSONPとの決定的な違い

## XMLHttpRequestの使い方
```
<button id="xhr-btn1" value="click">button!!!</button>

<textarea id="xhr-result1"></textarea>

<script>
var btn1 = document.getElementById('xhr-btn1');
btn1.onclick = function(){

  var xhr = new XMLHttpRequest();
  xhr.open('GET', location.href, true);
  xhr.onreadystatechange = function(){
    // 本番用
    if (xhr.readyState === 4 && xhr.status === 200){
      var result1 = document.getElementById('xhr-result1');
      result1.value = xhr.responseText;
    }
    // ローカルファイル用
    if (xhr.readyState === 4 && xhr.status === 0){
      var result1 = document.getElementById('xhr-result1');
      result1.value = xhr.responseText;
    }
  };
  xhr.send(null);
};
</script>
```
* new XMLHttpRequestでオブジェクトを作る。
* openメソッドで送信時のメソッド、送信先URL、非同期通信か同期通信か真偽値で設定
第三引数で同時通信にすることも可能。省略時は非同期通信になる。
* onreadstatechangeで読み込み時の動作を指定
* sendメソッドでリクエストを送信、引数はGETの場合nullを、POSTの場合はデータを&で繋げた文字列にして渡す。

### レスポンスの処理
* onreadstatechange
  読み込み途中、読み込み完了、失敗時の処理を行う。
  * readyState
読み込みの状態を管理
readyStateが4のときに、読み込み完了

  * statusプロパティ
読み込みが成功したのか、失敗したのかを示す。
成功時のステータスコードは、200。
成功時、responseTextからレスポンスの内容を読み取る
responseXML
XMLを受け取ったとときに読み取る。

### JSONを受け取った処理
XMLHttpRequestのレスポンスとしてJSONを受け取った場合、
responseTextのデータとして読み取る。
JSONをパースしてオブジェクトに変換する。
そのため、json2.jsを用いる。（読み込むだけ）

```
var data = JSON.parse(xhr.responseText);
```
