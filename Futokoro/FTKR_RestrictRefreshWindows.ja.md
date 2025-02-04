[トップページに戻る](README.md)

# [FTKR_RestrictRefreshWindows](FTKR_RestrictRefreshWindows.js) プラグイン

各画面におけるウィンドウのリフレッシュ回数を制限して負荷を抑えます。

ダウンロード: [FTKR_RestrictRefreshWindows.js](https://raw.githubusercontent.com/futokoro/RPGMaker/master/FTKR_RestrictRefreshWindows.js)

## 目次

以下の項目の順でプラグインの使い方を説明します。
1. [概要](#概要)
2. [プラグインの登録](#プラグインの登録)
1. [装備画面の負荷軽量化](#装備画面の負荷軽量化)
* [プラグインの更新履歴](#プラグインの更新履歴)
* [ライセンス](#ライセンス)

## 概要

このプラグインを導入すると、各画面におけるウィンドウのリフレッシュ回数を制限して負荷を抑えます。

対象画面
* 装備画面

[目次に戻る](#目次)

## プラグインの登録

このプラグインはできる限り、プラグイン管理の一番下に登録してください。

[目次に戻る](#目次)

# 装備画面の負荷軽量化

## 描画回数の抑制

装備画面の場合の各操作とその時のウィンドウの再描画回数は、調べたところ以下のようになっている。<br>
⇒矢印以降の数値は、このプラグイン導入による修正後の回数。

| 操作 | status | item | help | slot |
| --- | --- | --- | --- | --- |
| メニュー画面から装備画面に移る | ２回⇒１回 | ２回⇒０回 | ０回 | ２回⇒１回 |
| 装備コマンドを選択 | ０回 | １回 | １回 | ０回 |
| 最強装備コマンドを選択 | １回 | ０回 | ０回 | １回 |
| 全て外すコマンドを選択 | １回 | ０回 | ０回 | １回 |
| コマンドウィンドウでキャンセル | ０回 | ０回 | ０回 | ０回 |
| 装備スロットでカーソルを動かす | ０回 | １回 | １回 | ０回 |
| 装備スロットでスロットを選択 | ２回⇒１回 | ０回 | ２回⇒１回 | ０回 |
| 装備スロットでキャンセル | ０回 | １回 | １回 | ０回 |
| アイテムウィンドウでカーソルを動かす | １回 | ０回 | １回 | ０回 |
| アイテムウィンドウでアイテムを選択 | ２回⇒１回 | １回 | ２回⇒１回 | １回 |
| アイテムウィンドウでキャンセル | １回 | ０回 | ２回⇒１回 | ０回 |

* status ：装備ステータスウィンドウ
* item ：装備アイテムウィンドウ
* help ：ヘルプウィンドウ
* slot ：装備スロットウィンドウ

## その他の負荷抑制

装備画面の負荷を重くしている理由には、ウィンドウの再描画回数だけではなく、以下の処理もある。

* 選択中のアイテム装備後のパラメータを参照するためにダミーのアクターデータが存在しており、２人分のアクターデータを表示している
* 装備アイテムウィンドウでカーソルを動かす度に、このダミーアクターデータを作り直し⇒ウィンドウを再描画という処理を行っている

装備アイテムウィンドウでカーソルを動かす処理が重い理由は、この部分の影響も大きい。

再描画はどうしようもないため、ダミーデータは画面遷移時またはアクター変更時にのみ作成し、装備アイテムウィンドウでカーソルを動かすときには、装備データの入れ替えのみとして負荷を抑えた。

[目次に戻る](#目次)

# プラグインの更新履歴

| バージョン | 公開日 | 更新内容 |
| --- | --- | --- |
| [ver1.0.2](FTKR_RestrictRefreshWindows.js) | 2018/12/16 | 装備アイテムウィンドウの _slotId の更新処理を見直し<br>装備データ比較用のダミーアクターの作成処理に関する軽量化を追加 |
| ver1.0.1 | 2018/12/15 | 装備画面のアイテムウィンドウのリフレッシュの挙動を修正 |
| ver1.0.0 | 2018/12/15 | 新規作成 |

# ライセンス

本プラグインはMITライセンスのもとで公開しています。

[The MIT License (MIT)](https://opensource.org/licenses/mit-license.php)

#
[目次に戻る](#目次)

[トップページに戻る](README.md)