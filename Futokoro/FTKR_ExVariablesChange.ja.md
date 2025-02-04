[トップページに戻る](README.md)

# [FTKR_ExVariablesChange](FTKR_ExVariablesChange.js) プラグイン

変数の操作を拡張するプラグインです。

ダウンロード: [FTKR_ExVariablesChange.js](https://raw.githubusercontent.com/futokoro/RPGMaker/master/FTKR_ExVariablesChange.js)

# 目次

以下の項目の順でプラグインの使い方を説明します。
1. [概要](#概要)
1. [プラグインの登録](#プラグインの登録)
2. [アイテム_武器_防具の増減時の変更](#アイテム_武器_防具の増減時の変更)
2. [アイテム_スキルの使用時の変更](#アイテム_スキルの使用時の変更)
2. [戦闘終了時の変更](#戦闘終了時の変更)
2. [プラグインパラメータの汎用処理について](#プラグインパラメータの汎用処理について)
2. [タグに記述する計算式について](#タグに記述する計算式について)
* [プラグインの更新履歴](#プラグインの更新履歴)
* [ライセンス](#ライセンス)

# 概要

本プラグインを実装することで、以下の状況において変数やスイッチを操作することができます。

1. アイテム、武器、防具の購入（増加）・売却（減少）時
2. アイテム、スキルの使用時(成功時、失敗時)
3. 戦闘終了時(勝利、敗北、逃走)

[FTKR_ItemSelfVariables.js](FTKR_ItemSelfVariables.ja.md)によって作成したセルフ変数も操作できます。

# プラグインの登録

[FTKR_ExBattleEvent](FTKR_ExBattleEvent.ja.md)プラグインと組み合わせて使用する場合は、当プラグインを下にしてください。
```
FTKR_ExBattleEvent.js
FTKR_ExVariablesChange.js
```
順番が逆(当プラグインが上)の場合、FTKR_ExBattleEventで戦闘勝利イベントや敗北イベントを差し替えていると、戦闘勝利時および戦闘敗北時の変数操作が実行できません。
また、戦闘終了時の処理は、戦闘勝利イベントや敗北イベントの後に実行します。

# アイテム_武器_防具の増減時の変更
対象のメモ欄に以降のタグを追記すると、次の状況において、変数・スイッチの変更ができます。

対象：アイテム、武器、防具

## アイテム・武器・防具の購入時
```
<EVC 購入時>
計算式
</EVC 購入時>
```

## アイテム・武器・防具の売却時
```
<EVC 売却時>
計算式
</EVC 売却時>
```

## アイテム・武器・防具の増加時
```
<EVC 増加時>
計算式
</EVC 増加時>
```
購入、イベントで入手、敵から入手などで実行

## アイテム・武器・防具の減少時
```
<EVC 減少時>
計算式
</EVC 減少時>
```
売却、イベントなどで減少すると実行

## 購入時と売却時の処理の注意点
この処理は、購入毎、売却毎に実行します。
つまり、1度のショップ画面の中で、ポーションを5個買う場合と、ポーション1個を5回買う場合では、前者は1回実行することに対して、後者は5回実行されます。

[目次に戻る](#目次)

# アイテム_スキルの使用時の変更
対象のメモ欄に以降のタグを追記すると、次の状況において、変数・スイッチの変更ができます。

対象：アイテム、スキル、アクター、エネミー、クラス、装備、ステート

なお、タグで設定した変数の計算が行われる順番は以下の通りです。
```
アイテム/スキル ⇒ アクター/エネミー ⇒ クラス ⇒ 装備 ⇒ ステート
```

## アイテム・スキルを使用した場合
タグそれぞれの実行タイミングは以下の順番です。
```
与ダメージ時 ⇒ 被ダメージ時 ⇒ 使用時 ⇒ 成功時 ⇒ 討伐時
回避時 ⇒ 使用時 ⇒ 失敗時
```

### アイテム・スキルの使用時
```
<EVC 使用時>
計算式
</EVC 使用時>
```
成功失敗問わずに、常に実行する

### アイテム・スキルの成功時
```
<EVC 使用成功時>
計算式
</EVC 使用成功時>
```
アイテム・スキルの使用に成功すると実行する。

### アイテム・スキルの失敗時
```
<EVC 使用失敗時>
計算式
</EVC 使用失敗時>
```
アイテム・スキルの使用に失敗すると実行する。

### ダメージを与えた時
```
<EVC 与ダメージ時>
計算式
</EVC 与ダメージ時>
```
アイテム・スキルを使用してダメージを与えると実行する。

### ダメージを与えて敵を倒した時
```
<EVC 討伐時>
計算式
</EVC 討伐時>
```
アイテム・スキルを使用してダメージを与えてエネミーを倒すと実行する。

## アイテム・スキルを使用された場合

### ダメージを受けた時
```
<EVC 被ダメージ時>
計算式
</EVC 被ダメージ時>
```
アイテム・スキルを使用されてダメージを受けると実行する。

### 回避した時
```
<EVC 回避時>
計算式
</EVC 回避時>
```
アイテム・スキルを使用されて回避すると実行する。

[目次に戻る](#目次)

# 戦闘終了時の変更
対象のメモ欄に以降のタグを追記すると、次の状況において、変数・スイッチの変更ができます。

対象：アクター、エネミー、クラス、装備、ステート

なお、タグで設定した変数の計算が行われる順番は以下の通りです。
```
アクター/エネミー ⇒ クラス ⇒ 装備 ⇒ ステート
アクター ⇒ エネミー
```
この処理は、戦闘に参加したアクターに対して実行します。
戦闘不能状態でも実行します。
なお、戦闘終了時の汎用処理には、セルフ変数は使用できません。

## 戦闘終了時
```
<EVC 戦闘終了時>
計算式
</EVC 戦闘終了時>
```
戦闘の結果を問わずに、常に実行する

## 戦闘勝利時
```
<EVC 勝利時>
計算式
</EVC 勝利時>
```
戦闘で勝利すると実行する。

## 戦闘敗北時
```
<EVC 敗北時>
計算式
</EVC 敗北時>
```
戦闘に敗北すると実行する。

## 戦闘逃走時
```
<EVC 逃走時>
計算式
</EVC 逃走時>
```
逃走で戦闘が終了すると実行する。

[目次に戻る](#目次)

# プラグインパラメータの汎用処理について
各状況において、アイテムやアクター等のタグで設定しなくても毎回実行する処理を規定できます。

この汎用処理は、アイテムやアクター等のタグによる処理の後に実行します。

複数の計算を実行する場合は、以下のようにセミコロン(;)で計算式を区切ってください。

例)
```
v[1] += 1; v[2] += 1
```

[目次に戻る](#目次)

# タグに記述する計算式について

計算式(eval)は、ダメージ計算式のように、計算式を入力することで、固定値以外の値を使用することができます。以下のコードを使用できます。
* s[x]    - スイッチID x の値を意味します。
* v[x]    - 変数ID x の値を意味します。
* a       - 使用者のゲームデータを参照します。(使用者がいる場合のみ)
* b       - 対象者のゲームデータを参照します。(対象者がいる場合のみ)
* item    - 使用したアイテムのデータを参照します。(*2)
* av[x]   - 使用者のセルフ変数ID x の値を意味します。(*1)
* bv[x]   - 対象者のセルフ変数ID x の値を意味します。(*1)
* iv[x]   - アイテムのセルフ変数ID x の値を意味します。(*1)(*2)
* number  - 購入・売却・増減時のアイテム数を意味します。
* result  - スキル・アイテムを使用した結果を参照します。(*3)

(*1) セルフ変数を使用する場合は、[FTKR_ItemSelfVariables.js](FTKR_ItemSelfVariables.js)が必要です。<br>
(*2) アイテムとは、使用したスキルまたはアイテム、購入・売却したアイテムのことです。<br>
(*3) result で取得できる値は以下の通り。<br>
* result.hpDamage - HPダメージ
* result.mpDamage - MPダメージ
* result.critical - クリティカルなら true

その他のパラメータ例(戦闘中)
* $gameTroop._turnCount - ターン数

### 設定例
購入時に変数を操作するためアイテムに以下のノートタグを設定する。

```
<EVC 購入時>
s[1] = true
v[10] = 10
iv[1] += v[5]
iv[2] += number
</EVC 購入時>
```
スイッチID1 を ON にします。<br>
変数ID10 に 10 を代入します。<br>
購入したアイテムのセルフ変数ID1 に 変数ID5 の値を加算します。<br>
購入したアイテムのセルフ変数ID2 に 購入数を加算します。

[目次に戻る](#目次)

## プラグインの更新履歴

| バージョン | 公開日 | 更新内容 |
| --- | --- | --- |
| [ver1.2.4](FTKR_ExVariablesChange.js) | 2018/07/01 | 他プラグインとの競合回避のためダメージ時のデータ取得タイミングを変更 |
| ver1.2.3 | 2017/08/24 | アクターの職業IDを正しく取得できない不具合を修正 |
| ver1.2.2 | 2017/06/08 | 被ダメージ時の処理タイミングを修正。(v1.2.1の修正による不具合) |
| ver1.2.1 | 2017/06/07 | 被ダメージ時に使用者・対象者・使用スキル(アイテム)のゲームデータを参照できない不具合を修正<br>戦闘終了時のタグをエネミーに適用<br>与ダメージ時の処理タイミングを変更 |
| ver1.2.0 | 2017/06/05 | 回避時の実行処理を追加 |
| ver1.1.1 | 2017/05/19 | ヘルプ修正 |
| ver1.1.0 | 2017/05/11 | エネミー討伐時や戦闘終了時を追加<br>プラグインパラメータで設定できる汎用計算処理を追加 |
| ver1.0.6 | 2017/05/02 | 計算式および非ダメージ時の処理見直し |
| ver1.0.5 | 2017/05/02 | アイテム増減時の処理の順番を見直し |
| ver1.0.4 | 2017/04/28 | アイテム増減時に例外処理を追加 |
| ver1.0.3 | 2017/04/26 | ダメージ時の変数操作機能を追加<br>アイテム・スキル使用時のタグ適用先を拡張。 |
| ver1.0.2 | 2017/04/25 | 対象者のセルフ変数が正しく反映されない不具合を修正 |
| ver1.0.1 | 2017/04/19 | 不具合修正 |
| ver1.0.0 | 2017/04/18 | 初版公開 |

## ライセンス

本プラグインはMITライセンスのもとで公開しています。

[The MIT License (MIT)](https://opensource.org/licenses/mit-license.php)

#
[目次に戻る](#目次)

[トップページに戻る](README.md)