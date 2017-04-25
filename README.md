jquery-pwd-measure
================

[![Travis](https://img.shields.io/travis/tsuyoshiwada/jquery-pwd-measure.svg?branch=master&style=flat-square)](https://travis-ci.org/tsuyoshiwada/jquery-pwd-measure)
[![npm](https://img.shields.io/npm/v/jquery-pwd-measure.svg?style=flat-square)](https://www.npmjs.com/package/jquery-pwd-measure)
[![Bower](https://img.shields.io/bower/v/jquery-pwd-measure.svg?style=flat-square)](http://bower.io/search/?q=jquery-pwd-measure)


パスワードの強度を測定して表示するためのjQueryプラグインです。


## Demo
[http://tsuyoshiwada.github.io/jquery-pwd-measure/](http://tsuyoshiwada.github.io/jquery-pwd-measure/)


## Features
`PwdMeasure`の特徴は下記のとおりです。

* パスワードの強度をパーセント表示可能
* 単一の入力フィールド、確認フィールドへの対応
* 強度を示すラベルを詳細に設定可能
* 必要最低限のパスワード強度をパーセント指定可能
* 判定をクリアした時、値が変わった時などのコールバックをサポート
* スタイルシートで自由にデザインを変更可能



## How To Use

### npm

```
$ npm install jquery-pwd-measure --save
```

### Bower (非推奨:更新予定無)

```
$ bower install jquery-pwd-measure --save
```

### Basic

```html
<script type="text/javascript" src="//code.jquery.com/jquery-1.11.2.min.js"></script>
<script type="text/javascript" src="js/jquery.pwdMeasure.min.js"></script>
```

### HTML

```html
<input type="password" id="pwd" name="pwd" value="" placeholder="Your Password">
<p id="pm-indicator"></p>
```

### JavaScript

```javascript
$(document).ready(function(){
	$("#pwd").pwdMeasure();
});
```


## Options

### Defaults

```javascript
// Default Options
{
  minScore: 50,
  minLength: 6,
  events: "keyup change",
  labels: [
    {score:10,         label:"とても弱い", className:"very-weak"},   //0~10%
    {score:30,         label:"弱い",       className:"weak"},        //11~30%
    {score:50,         label:"平均",       className:"average"},     //31~50%
    {score:70,         label:"強い",       className:"strong"},      //51~70%
    {score:100,        label:"とても強い", className:"very-strong"}, //71~100%
    {score:"notMatch", label:"不一致",     className:"not-match"},   //not match
    {score:"empty",    label:"未入力",     className:"empty"}        //empty
  ], 
  indicator: "#pm-indicator",
  indicatorTemplate: "パスワード強度: <%= label %> (<%= percentage %>%)",
  confirm: false,

  // Callbacks
  onValid: false,
  onInvalid: false,
  onNotMatch: false,
  onEmpty: false,
  onChangeState: false,
  onChangeValue: false
}
```


### minScore
**Default: `50`**  
**Type: `integer`**

最低限必要な強度を0~100のパーセントで指定します。  


### minLength
**Default: `6`**  
**Type: `integer`**

最低限必要な文字数を指定します。  
ここで指定した文字数と`minScore`の値から入力されたパスワードの強度を求めます。  


### events
**Default: `keyup change`**  
**Type: `string`**  

指定したイベントで強度判定を行います。  


### labels
**Type: `array`**  
**Default:**

強度を示すラベルを配列で指定します。  

```javascript
[
  {score:10,         label:"とても弱い", className:"very-weak"},   //0~10%
  {score:30,         label:"弱い",       className:"weak"},        //11~30%
  {score:50,         label:"平均",       className:"average"},     //31~50%
  {score:70,         label:"強い",       className:"strong"},      //51~70%
  {score:100,        label:"とても強い", className:"very-strong"}, //71~100%
  {score:"notMatch", label:"不一致",     className:"not-match"},   //not match
  {score:"empty",    label:"未入力",     className:"empty"}        //empty
]
```

キーに対応する役割は下記のとおりです。
`indicator`オプションを指定した場合、自動的にラベル、クラスが適用されます。  
また、コールバック内で取得出来ます。

* `score`: 該当する範囲をパーセント(整数)指定 `0~100` or `"notMatch"`
* `label`: 対応するラベル `string`
* `className`: 対応するクラス名 `string`

`score`には例外が存在します。それぞれのルールは下記のとおりです。

| key           | rule                                                      |
| :------------ | :-------------------------------------------------------- |
| `"notMatch"`  | メイン、確認用フィールドの値が違う場合                    |
| `"empty"`     | メイン、確認用フィールドのどちらも空の場合                |



### indicator
**Type: `string | jQueryObj | DOM Elements`**  
**Default: `#pm-indicator`**

強度を示すラベルを表示する要素を指定します。  


### indicatorTemplate
**Type: `string | jQueryObj | DOM Elements`**  
**Default: `パスワード強度: <%= label %> (<%= percentage %>%)`**

強度を示すラベルを表示する`<%= キー %>`形式で指定します。  
使用できるキーは下記です。

* `label`
* `className`
* `percentage`

### confirm
**Type: `string | jQueryObj | DOM Elements`**  
**Default: `false`**

確認用のフィールドを指定します。確認用フィールドを指定することで、
パスワードが一致しているかどうかの判定も行ないます。  



## Callbacks

### onValid
**Default: `false`**  
**Type: `function`**

強度が`minScore`の値を上回ったらコールされます。  


### onInvalid
**Default: `false`**  
**Type: `function`**

強度が`minScore`の値を上回った後、再び`minScore`よりも低い値になった際にコールされます。  


### onNotMatch
**Default: `false`**  
**Type: `function`**

メインの入力フィールドと、確認用フィールドの値が異なる際にコールされます。  
毎回のイベント時に呼ばれるのでは無く、内部的なステータス値が変更されたタイミングを採用します。  


### onEmpty
**Default: `false`**  
**Type: `function`**

メインの入力フィールドと、確認用フィールドのどちらも値が空の時コールされます。  


### onChangeState
**Default: `false`**  
**Type: `function`**

`onValid`, `onInvalid`, `onNotMatch`のタイミングでコールされます。  


### onChangeValue
**Default: `false`**  
**Type: `function`**

パスワードの値が変更される度にコールされます。  
※実際の値の変化では無く、`events`オプションで指定したイベント実行の度にコールされます。  


## CSS Example
`jQuery.pwdMeasure.js`では、スタイルを柔軟に設定することが出来ます。
このページで強度表示に使用しているスタイルは次のとおりです。是非参考にしてみてください。

### CSS

```css
.pm-indicator {
  margin:10px 0;
  padding:1.5em 1em;
  color:#2c3e50;
  font-size:12px;
  text-align:center;
  border:1px solid #ccc;
  border-radius:2px;
  background:#e4e4e4;
  text-shadow:1px 1px 0 rgba(255,255,255,.8);
  -webkit-transition:all .2s ease-in-out;
          transition:all .2s ease-in-out;
}

.pm-indicator.very-weak,
.pm-indicator.not-match {
  border-color:#be1d30;
  background-color:#ffc3cf;
}

.pm-indicator.weak {
  border-color:#ff787d;
  background-color:#ffe6e5;
}

.pm-indicator.strong {
  border-color:#78bc42;
  background-color:#bceea6;
}

.pm-indicator.very-strong {
  border-color:#4f85a7;
  background-color:#68c6d7;
}
```

### Scss

```scss
.pm-indicator {
  margin:10px 0;
  padding:1.5em 1em;
  background:#e4e4e4;
  border:1px solid #ccc;
  border-radius:2px;
  color:#2c3e50;
  font-size:12px;
  text-shadow:1px 1px 0 rgba(255, 255, 255, .8);
  text-align:center;
  transition:all .2s ease-in-out;
  &.very-weak,
  &.not-match {
    background-color:#ffc3cf;
    border-color:#be1d30;
  }
  &.weak {
    background-color:#ffe6e5;
    border-color:#ff787d;
  }
  &.average {
  }
  &.strong {
    background-color:#bceea6;
    border-color:#78bc42;
  }
  &.very-strong {
    background-color:#68c6d7;
    border-color:#4f85a7;
  }
}
```

## Dependencies
jQuery 1.7.2 +


## TODO
以下、優先順位順。

1. <del>テストを書く</del>
2. <del>デモページの移行 (Github上に作成)</del>
3. コールバックとは別にイベントを提供する
4. 文字列から、強度を測定するグローバルAPIを提供
5. CSSのテーマを作成 (オプションで選択できるようにする予定)


## Licence
Released under the [MIT Licence](https://raw.githubusercontent.com/tsuyoshiwada/jquery-pwd-measure/master/LICENSE)


## Author
[tsuyoshi wada](https://github.com/tsuyoshiwada/)
