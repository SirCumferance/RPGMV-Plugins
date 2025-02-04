[トップページに戻る](README.md)

# [FTKR_ItemSelfVariables](FTKR_ItemSelfVariables.js) プラグイン

アイテムやスキルなどにセルフ変数を実装するプラグインです。

ダウンロード: [FTKR_ItemSelfVariables.js](https://raw.githubusercontent.com/futokoro/RPGMaker/master/FTKR_ItemSelfVariables.js)

## 目次

以下の項目の順でプラグインの使い方を説明します。
1. [概要](#概要)
2. [セルフ変数の設定](#セルフ変数の設定)
1. [制御文字](#制御文字)
3. [スクリプトコマンド](#スクリプトコマンド)
4. [プラグインコマンド](#プラグインコマンド)
* [プラグインの更新履歴](#プラグインの更新履歴)
* [ライセンス](#ライセンス)

## 概要

本プラグインを実装することで、以下の対象にセーブ時に記録し値の操作が
可能な、セルフ変数を実装することができます。

1. アイテム
2. 武器
3. 防具
4. スキル
5. アクター
6. エネミー

[目次に戻る](#目次)

## セルフ変数の設定

以下の、プラグインパラメータで設定を変更します。

### プラグインパラメータの設定

```
<*** Number>
```
使用するセルフ変数の数を設定します。
0 の場合は、セルフ変数を無効にします。

```
<Enabled Save>
```
セーブ時にセルフ変数を保存するか指定します。
保存しない場合、ゲーム起動毎にリセットします。
この設定は、アイテム、武器、防具、スキル共通です。

### セルフ変数の初期値

以下の、ノートタグをアイテム(武器・防具含む)やスキルのメモ欄に追記すると初期値を設定できます。

```
<ISV IV[x]: y>
<ISV セルフ変数[x]: y>
```
セルフ変数ID x の初期値を y に設定します。

### セルフ変数のダメージ計算式への適用

セルフ変数は、アイテムおよびスキルのダメージ計算式に使用できます。
以下の短縮コードでセルフ変数を参照します。
* `iv[x]` - 使用したアイテムまたはスキルのセルフ変数ID x の値を参照します。
* `av[x]` - 使用者のセルフ変数ID x の値を参照します。
* `bv[x]` - 対象者のセルフ変数ID x の値を参照します。


入力例）
```
a.atk(4 + iv[1]) - b.def2
```

#### `iv[x]`と入力すると、必ずダメージを与えられなくなる場合

他のプラグインと競合しており、短縮コードが使用できません。
この場合は、短縮コードの替わりに、以下の表記に変えてください。
* `iv[x]` ⇒ `item._seflVariables._data[x]`
* `av[x]` ⇒ `a.actor()._seflVariables._data[x]`
* `bv[x]` ⇒ `b.actor()._seflVariables._data[x]`

なお、使用者 や 対象者が エネミーの場合は、actor() を enemy() に変えてください。

[目次に戻る](#目次)

## 制御文字

以下の制御文字で、各セルフ変数の値を表示できます。

```
\ITV[アイテムID,セルフ変数ID]
\WEV[武器ID,セルフ変数ID]
\ARV[防具ID,セルフ変数ID]
\SKV[スキルID,セルフ変数ID]
\ACV[アクターID,セルフ変数ID]
\ENV[エネミーID,セルフ変数ID]
```

説明文やプロフィールで、上記制御文字を使う場合に[]内の "x,"部分を省略することが可能です。<br>
省略した場合は、例えばアイテムならそのアイテムのセルフ変数を参照します。

[目次に戻る](#目次)

## スクリプトコマンド

セルフ変数に対して、以下のスクリプトコマンドを使用できます。
なお、`item(x)`の部分は、武器は`weapon(x)`、防具は`armor(x)`、スキルは`skill(x)`、アクターは`actor(x)`、エネミーは`enemy(x)`に変えてください。

### セルフ変数の値の取得
```
$gameSelfVariables.item(x).value(y)
```
アイテムID x のセルフ変数 y の値を取得します。

### セルフ変数の値の変更
```
$gameSelfVariables.item(x).setValue(y, value (, code) )
```
アイテムID x のセルフ変数 y に value を代入します。
code部に以下の文字を入力することで、代入以外の計算が可能です。
* 加算(+) - セルフ変数 y の値に value を加算します。
* 減算(-) - セルフ変数 y の値から value を減算します。
* 積算(*) - セルフ変数 y の値に value を積算します。
* 除算(/) - セルフ変数 y の値から value を除算します。
* 剰余(%) - セルフ変数 y の値から value を除算した余りを代入します。

#### 入力例
```
$gameSelfVariables.item(10).setValue(1, 10)
```
アイテムID 10 のセルフ変数 1 に 10 を代入する

```
$gameSelfVariables.weapon(12).setValue(2, 6, '加算')
$gameSelfVariables.weapon(12).setValue(2, 6, '+')
```
武器ID 12 のセルフ変数 2 に 6 を加算する

### セルフ変数の削除
```
$gameSelfVariables.item(x).clear()
```
アイテムID x のセルフ変数をすべて削除します。
0 が代入されるわけではありません。

### セルフ変数の初期化
```
$gameSelfVariables.item(x).allReset(value)
```
アイテムID x のすべてのセルフ変数に value を代入します。
このときの'すべて'とは、プラグインパラメータ`<Item Number>`で設定した数を指します。

[目次に戻る](#目次)

## プラグインコマンド

本プラグインでは、以下のコマンドを使用できます。
なお、大文字小文字は区別しないため、どちらを使用しても構いません。

プラグインコマンドの引数として指定する「タイプ」には、以下の文字列のいずれかを指定してください。(日本語または英字のいずれか)
* アイテム(item)
* 武器(weapon)
* 防具(armor)
* スキル(skill)
* アクター(actor)
* エネミー(enemy)

### セルフ変数の変更
指定したアイテムIDのセルフ変数を変更します。

```
ISV_セルフ変数変更 タイプ アイテムID セルフ変数ID 演算方法 値
ISV_SET_SELF_VARIABLES TYPE itemId selfVariableId CALCTYPE value
```
* 「演算方法」には、代入(=)、加算(+)、減算(-)、積算(*)、除算(/)、剰余(%)のいずれかの文字または記号を指定してください。
* 「アイテムID」、「セルフ変数ID」、「値」には、ゲーム内変数を指定できます。
* ゲーム内変数を使用する場合は、数値の変わりに v[n] を入力してください。n はゲーム内変数ID。

#### 入力例
```
ISV_セルフ変数変更 アイテム 10 1 代入 v[5]
ISV_SET_SELF_VARIABLES item 10 1 = v[5]
```
アイテムID10 のセルフ変数ID1 に ゲーム内変数ID5 の値を代入する

### セルフ変数の取得
指定したアイテムIDのセルフ変数を、ゲーム内変数に代入します。

```
ISV_セルフ変数取得 ゲーム内変数ID アイテムタイプ アイテムID セルフ変数ID
ISV_GET_SELF_VARIABLES variableId ITEMTYPE itemId selfVariableId
```

* 「ゲーム内変数ID」、「アイテムID」、「セルフ変数ID」には、ゲーム内変数を指定できます。
* ゲーム内変数を使用する場合は、数値の変わりに v[n] を入力してください。n はゲーム内変数ID。

#### 入力例
```
ISV_セルフ変数取得 5 武器 10 1
ISV_GET_SELF_VARIABLES 5 weapon 10 1
```
武器ID10 のセルフ変数ID1 の値を ゲーム内変数ID5 に代入する

[目次に戻る](#目次)

## プラグインの更新履歴

| バージョン | 公開日 | 更新内容 |
| --- | --- | --- |
| [ver1.2.2](FTKR_ItemSelfVariables.js) | 2018/01/08 | ヘルプ追記 |
| ver1.2.1 | 2017/10/20 | アイテムの説明欄に制御文字を使用する場合に、アイテムIDを省略できる機能を追加 |
| ver1.2.0 | 2017/10/20 | 制御文字に対応 |
| ver1.1.3 | 2017/05/03 | 計算式の処理見直し |
| ver1.1.1 | 2017/04/26 | ダメージ計算式に例外処理を追加 |
| ver1.1.0 | 2017/04/18 | 個別にセルフ変数の数を設定できるように変更 <br> アクターとエネミーにセルフ変数を追加 |
| [ver1.0.1](/archive/FTKR_ItemSelfVariables_1.0.1.js) | 2017/04/14 | セルフ変数に初期値を設定する機能を追加 |
| ver1.0.0 | 2017/03/26 | 初版公開 |

## ライセンス

本プラグインはMITライセンスのもとで公開しています。

[The MIT License (MIT)](https://opensource.org/licenses/mit-license.php)

#
[目次に戻る](#目次)

[トップページに戻る](README.md)