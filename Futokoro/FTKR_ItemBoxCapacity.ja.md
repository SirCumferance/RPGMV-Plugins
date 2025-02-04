[トップページに戻る](README.md)

# [FTKR_ItemBoxCapacity](FTKR_ItemBoxCapacity.js) プラグイン

アイテムボックスに所持容量を追加するプラグインプラグインです。

ダウンロード: [FTKR_ItemBoxCapacity.js](https://raw.githubusercontent.com/futokoro/RPGMaker/master/FTKR_ItemBoxCapacity.js)

# 目次

以下の項目の順でプラグインの使い方を説明します。
1. [概要](#概要)
1. [アイテムボックスの所持容量の設定](#アイテムボックスの所持容量の設定)
1. [アイテムのスタックの設定](#アイテムのスタックの設定)
* [プラグインの更新履歴](#プラグインの更新履歴)
* [ライセンス](#ライセンス)

# 概要

本プラグインを実装することで、アイテムボックスに所持容量を追加します。

また、アイテムの最大スタック数を超えた場合、別にスタックすることができます。

[目次に戻る](#目次)

# アイテムボックスの所持容量の設定

アイテムボックスに所持容量を設定することができます。
この機能を使うためには、プラグインパラメータ`<Enable Capacity>`を有効にしてください。

所持容量を設定すると、そのカテゴリーは設定した数の種類までしか所持することができなくなります。
この設定数以上のアイテムは所持することができず、入手しても自動的に破棄します。

アイテムの所持容量を 10 に設定した場合、アイテムは 10種類までしか所持できません。
この時、各アイテムが何個持っているかは関係ありません。

設定できるカテゴリーは「アイテム」「武器」「防具」の３つです。
「大事なもの」は、アイテムに含まれます。

アイテムボックスに空きがあるかどうかは、以下のスクリプトで判定できます。
```
$gameParty.isItemsCapacityOk()   - アイテムの所持容量
$gameParty.isWeaponsCapacityOk() - 武器の所持容量
$gameParty.isArmorsCapacityOk()  - 防具の所持容量
```

## プラグインパラメータで[初期値]を設定
以下のプラグインパラメータで、初期値を設定することができます。
0 を設定した場合は、容量が無制限になります。

### [アイテム]の所持容量
`<Item Capacity>`

### [武器]の所持容量
`<Weapon Capacity>`

### [防具]の所持容量
`<Armor Capacity>`

## プラグインコマンドで[追加分]を設定
プラグインコマンドでも所持容量を設定できます。
ただし、この追加分はプラグインパラメータの値とは別に計算します。
```
EIB_所持容量設定 [カテゴリー] [数値] [計算方法]
EIB_SET_CAPACITY [category] [value] [calc_method]
```
[カテゴリー]の入力内容で、どのカテゴリーの容量を変えるか指定します。
* アイテム or ITEM
* 武器 or WEAPON
* 防具 or ARMOR

[数値]の入力内容
* \V[x] でゲーム内変数ID x の値を参照できます。

[計算方法]の入力内容で、[数値]をどのように計算するか指定します。
計算方法を指定しない場合は、代入を適用します。
* 加算 or ADD or +
* 減算 or SUBTRACT or -
* 乗算 or MULTIPLY or *
* 除算 or DIVIDE or /
* 剰余 or MOD or %
* 代入 or SUBSTITUTE or =

例)
```   
EIB_所持容量設定 アイテム 10 加算
EIB_所持容量設定 防具 \V[1] 代入
EIB_SET_CAPACITY WEAPON 5 SUBTRACT
```

上記の設定によって所持容量は以下の結果になります。
```
所持容量　＝　初期値　＋　追加分
```
[目次に戻る](#目次)

# アイテムのスタックの設定

アイテムボックスの中で、１つのアイテムは１行にまとめて表示します。
そして、所持数を表示することでそのアイテムを何個所持しているか分かります。
これがアイテムのスタックです。

当プラグインでは、このスタック機能を変更することができます。
この機能を使うためには、プラグインパラメータ`<Enable Capacity>`を有効にしてください。

## アイテムのスタック数の設定
アイテムをスタックできる数は以下の方法で設定できます。

### プラグインパラメータで設定
`<Max Stack Number>`

### アイテムのメモ欄に以下のタグを記入
```
<EIB_スタック: x>
<EIB_STACK: x>
```
* x - 最大スタック数

設定が被った場合、メモ欄の設定を優先します。
この設定数以上のアイテムは所持することができず、入手しても自動的に破棄します。

## スタックの重複

プラグインパラメータ`<Enable Duplicate Stack>`を許可設定するとスタック数以上にアイテムを所持することができます。

スタック数を超えた分は、別の行に表示します。
なお、この別の行に表示したアイテムは、所持容量上別のアイテムとして計算します。

例えば「ポーション」を20個所持していて、スタック数の最大が10の場合
「ポーション」の行が 2つできます。
この時、アイテムを２種類持っていると数えます。

[目次に戻る](#目次)

# プラグインの更新履歴

| バージョン | 公開日 | 更新内容 |
| --- | --- | --- |
| [ver1.0.1](FTKR_ItemBoxCapacity.js) | 2018/09/22 | スクリプトが正常に動作しない不具合を修正 |
| ver1.0.0 | 2017/06/09 | 初版公開 |

# ライセンス

本プラグインはMITライセンスのもとで公開しています。

[The MIT License (MIT)](https://opensource.org/licenses/mit-license.php)

#
[目次に戻る](#目次)

[トップページに戻る](README.md)