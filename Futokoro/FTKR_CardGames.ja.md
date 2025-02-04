[トップページに戻る](README.md)

# [FTKR_CardGames](FTKR_CardGames.js) プラグイン

ゲーム内でトランプゲームで遊べるプラグインです。

ダウンロード: [FTKR_CardGames.js](https://raw.githubusercontent.com/futokoro/RPGMaker/master/FTKR_CardGames.js)

サンプルプロジェクト: [アツマール公開版+](sampleProject/CardGameSample.zip)
※(MVコアスクリプト v1.6.1対応)

## 目次

以下の項目の順でプラグインの使い方を説明します。
1. [概要](#概要)
1. [初期設定](#初期設定)
    1. [カード画像の準備](#カード画像の準備)
    1. [カード画像の登録](#カード画像の登録)
    1. [変数およびスイッチの設定](#変数およびスイッチの設定)
    1. [ゲームポイントの設定](#ゲームポイントの設定)
    1. [ゲーム内での設定](#ゲーム内での設定)
1. [カードゲームの遊び方](#カードゲームの遊び方)
    1. [カードゲーム画面を表示する](#カードゲーム画面を表示する)
    1. [プレイヤーの操作](#プレイヤーの操作)
    1. [並び替えコマンドについて](#並び替えコマンドについて)
1. [NPCの設定](#npcの設定)
    1. [特徴に関する内容](#特徴に関する内容)
    1. [表情に関する内容](#表情に関する内容)
    1. [台詞に関する内容](#台詞に関する内容)
    1. [設定例](#設定例)
1. [コモンイベント](#コモンイベント)
    1. [カードゲーム中のコモンイベント](#カードゲーム中のコモンイベント)
    1. [カードゲーム中のコモンイベントの実行条件の設定](#カードゲーム中のコモンイベントの実行条件の設定)
    1. [コモンイベントで使用できるスクリプト](#コモンイベントで使用できるスクリプト)
1. [プラグインの詳細設定](#プラグインの詳細設定)
* [プラグインの更新履歴](#プラグインの更新履歴)
* [ライセンス](#ライセンス)

# 概要

ゲーム中に、以下のトランプゲームで遊べます。

 1. ババ抜き(2～4人まで)

[目次に戻る](#目次)

# 初期設定
カードゲームで遊ぶために、まず以下の設定が必要になります。

## カード画像の準備
カード画像は、以下の仕様のものを各自準備してください。
カード画像は、img/picturesフォルダに保存してください。

### 絵札および数札
***01.png のように、１枚毎に個別に画像を用意します。<br>
*** はカードのスート(マーク)ごとに同じ文字列にしてください。<br>
01 の数字部は、カードのランク(数字)を二桁で記載してください。<br>

### ジョーカーおよび裏面
名前は自由に設定してください。

### ディプロイメント対応について
ディプロイメントで、「未使用ファイルを含まない」を有効にする場合は、絵札および数札のファイル名を、以下にしてください。

* スペード : s01.png ~ s13.png
* クラブ　 : c01.png ~ c13.png
* ハート　 : h01.png ~ h13.png
* ダイヤ　 : d01.png ~ d13.png

[目次に戻る](#目次)

## カード画像の登録
プラグインパラメータに、準備したカード画像を設定します。

### 絵札および数札
各スート毎に、用意した画像のファイル名を以下の形式で設定します。
```
***%1
```
***は画像ファイルの文字列部分
   
### ジョーカーおよび裏面
img/picturesフォルダから選択できます。

[目次に戻る](#目次)

## 変数およびスイッチの設定
当プラグインのゲーム内の設定のいくつかは、ゲーム内変数やスイッチの値を参照するようになっています。

以下のプラグインパラメータに、使用する変数IDやスイッチIDを設定してください。

### 変数の設定
※印がついているパラメータは、必ず設定してください。
```
Game Count ID　(※)
Draw Speed ID
Player 1 ID　(※)
Player 1 Result
Player 2 ID　(※)
Player 2 Result
Player 3 ID　(※)
Player 3 Result
Player 4 ID　(※)
Player 4 Result
```
各パラメータの意味は以下の通りです。

#### Game Count ID
1セット内で行うゲーム数を格納するゲーム変数IDです。
必ず設定してください。

#### Draw Speed ID
手札を引く早さを格納するゲーム変数IDです。
プラグインパラメータ`Draw Speed`で設定した値が初期値として、ここで設定したゲーム変数に格納されます。
手札を引く早さを固定する場合は、設定不要です。

#### Player * ID
各プレイヤーのアクターIDを格納するゲーム変数IDです。
必ず設定してください。

#### Player * Result
各プレイヤーのゲームポイントを格納するゲーム変数IDです。
ゲームポイントを使用しない場合は、設定不要です。

### スイッチの設定
```
Random Start ID
NPC Dialogues ID
NPC Facial Expressions ID
NPC Characteristics ID
```
各パラメータの意味は以下の通りです。

#### Random Start ID
ゲーム開始時の最初のプレイヤーをランダムで選ぶか設定するためのスイッチIDです。
ここで設定したスイッチがONの時にランダムで開始、OFFの時はプレイヤー１から開始になります。

このスイッチIDを設定しない場合は、プレイヤー１から開始で固定です。

#### NPC Dialogues ID
NPCの台詞機能の有効無効を設定するスイッチのIDです。
このスイッチIDを設定しない場合は、NPCの台詞機能は無効になります。

#### NPC Facial Expressions ID
NPCの表情変更機能の有効無効を設定するスイッチのIDです。
このスイッチIDを設定しない場合は、NPCの表情変更機能は無効になります。

#### NPC Characteristics ID
NPCの特徴機能の有効無効を設定するスイッチのIDです。
このスイッチIDを設定しない場合は、NPCの特徴機能は無効になります。

[目次に戻る](#目次)

## ゲームポイントの設定
以下のプラグインパラメータで設定します。
```
Ranking Points
```
1ゲームで、上がり順に取得するポイントを設定します。<br>
各プレイヤーが取得したポイントは、プラグインパラメータで設定したゲーム内変数に保存します。<br>
複数回のゲームを行うと、ポイントは加算していきます。

[目次に戻る](#目次)

## ゲーム内での設定
この項目で設定するパラメータは、ゲーム内のイベントにて設定してください。

### プレイヤーの設定

以下の、プラグインコマンドでゲームに参加するキャラを設定します。

```
CRD_プレイヤー設定 [プレイヤー1のアクターID] [プレイヤー2のアクターID] ...
CRD_SETTING_PLAYER [player1 actorId] [player2 actorId] ...
```

ゲームに参加するプレイヤーを設定します。<br>
アクターIDを設定してください。(最大4人まで)<br>
-1 を設定すると、パーティーのリーダーキャラになります。<br>
プレイヤー1が操作キャラになります。<br>

入力例
```
CRD_プレイヤー設定 1 2 3 4
CRD_SETTING_PLAYER 1 2 3 4
```
この設定で、プレイヤー１にアクターID1のキャラを、プレイヤー２にアクターID2のキャラ、といったように設定されます。<br>
このコマンドを実行すると、プラグインパラメータ`Player * ID`で設定したゲーム変数に、指定したアクターIDの値が代入されます。

なお、上記プラグインコマンドを使用せずに、直接プラグインパラメータ`Player * ID`で設定したゲーム変数の値を変更しても、プレイヤーの設定を行うことができます。

### ゲーム数の設定

1セット内で行うゲーム数を設定します。
プラグインパラメータ`Game Count ID`で指定したゲーム変数の値を変更してください。

[目次に戻る](#目次)

# カードゲームの遊び方

## カードゲーム画面を表示する
以下のプラグインコマンドを実行すると、ゲーム画面を表示します。

```
CRD_カードゲーム表示
CRD_OPEN_CARDGAME
```
ゲーム画面を表示して、ゲームを開始します。
設定したゲームの回数の分、プレイすると元の画面に戻ります。

[目次に戻る](#目次)

## プレイヤーの操作

プレイヤー1は操作キャラとして、ゲームに参加します。

相手からカードを引くときは、任意の場所にカーソルを合わせることで好きな位置のカードを引くことが出来ます。

また、引いたカードは手札の任意の場所にセットすることができます。

後述のNPCの特徴により、手札の中の位置が重要になるため、配置する場所を考えながらゲームを進めましょう。

[目次に戻る](#目次)

## 並び替えコマンドについて
引いたカードの配置を決めると、ターンを終了するか、並び替えをするか選択できます。

並び替えコマンドを選択すると、再度手札にカーソルが移り手札の配置を変更することができます。

手順は、位置を変えたいカードにカーソルを合わせて決定ボタンを押し、その後移したい場所にカーソルを合わせて、決定ボタンを押します。

この時、同じ場所を選択すると、カードを突き出した状態にすることができます。

[目次に戻る](#目次)

# NPCの設定

プレイヤー1以外に設定されたアクターは、NPCとしてゲームに参加します。

この時、それぞれのアクターのメモ欄に以下のタグを記載することでNPC時に特徴を設定することができます。

### 特徴および表情の場合
```
<CRD_特徴>
内容: 確率
</CRD_特徴>
```

### 台詞の場合
```
<CRD_台詞>
内容: 確率, 台詞
</CRD_台詞>
```

確率は、1～10の値で、10％刻みで内容を実行する確率を設定できます。
台詞は、表示させる文字列を記入してください。
内容には以下の項目があります。

[目次に戻る](#目次)

## 特徴に関する内容
### カードの取り方に関する内容

1. 端を取る　　　　　 - 相手の手札の両端のカードから選びます
2. 端を取らない　　　 - 相手の手札の両端のカードを選びません

3. 中心を取る　　　　 - 相手の手札の中心のカードを選びます
                       (手札が偶数の場合は中心2枚から選ぶ)
4. 中心を取らない　　 - 相手の手札の中心のカードを選びません
                       (手札が偶数の場合は中心2枚を選ばない)

5. 右から取る　　　　 - 相手の手札の中心よりも右側のカードから選びます
6. 左から取る　　　　 - 相手の手札の中心よりも左側のカードから選びます

7. 突き出しを取る　　 - 相手の手札で突き出しているカードを選びます
8. 突き出しを取らない - 相手の手札で突き出しているカードを選びません

### 取ったカードの並べ方に関する内容

1. 端に入れる　　　　 - 自分の手札の両端のどちらかに入れます
2. 端に入れない　　　 - 自分の手札の両端には入れません

3. 中心に入れる　　　 - 自分の手札の中心に入れます
4. 中心に入れない　　 - 自分の手札の中心には入れません

5. 右に入れる　　　　 - 自分の手札の中心よりも右側に入れます
6. 左に入れる　　　　 - 自分の手札の中心よりも左側に入れます

### 手札カードの突き出しに関する内容

1. ジョーカーを突き出す　　 - 手札にジョーカーが有る場合、突き出します
2. ジョーカー以外を突き出す - 手札のジョーカー以外のカードを、突き出します

[目次に戻る](#目次)

## 表情に関する内容

### 一時的な表情の変化に関する内容
この変化は自分のターンが終わると解除されます

1. ジョーカーを引く(x)　　 - ジョーカーを引いた時に顔画像をx番に変えます
2. ジョーカー以外を引く(x) - ジョーカー以外を引いた時に顔画像をx番に変えます
3. ペアができた(x)　　　　 - ペアができた時に顔画像をx番に変えます
4. ペアができない(x)　　　 - ペアができなかった時に顔画像をx番に変えます

### 常時変化する表情に関する内容

1. ジョーカーを持っている(x)　 - ジョーカーを持っている間、顔画像をx番に変えます
2. ジョーカーを持っていない(x) - ジョーカーを持っていない間、顔画像をx番に変えます

[目次に戻る](#目次)

## 台詞に関する内容
この台詞は自分のターンが終わると消去されます

1. ジョーカーを引く　　 - ジョーカーを引いた時に指定の台詞を表示します
2. ジョーカー以外を引く - ジョーカー以外を引いた時に指定の台詞を表示します
3. ペアができた　　　　 - ペアができた時に指定の台詞を表示します
4. ペアができない　　　 - ペアができなかった時に指定の台詞を表示します

[目次に戻る](#目次)

## 設定例
特徴は複数設定することができます。
複数の特徴がある場合は、上から順番に内容を処理します。

例)
```
<CRD_特徴>
端から取る: 7
右から取る: 10
</CRD_特徴>
<CRD_台詞>
ジョーカーを引く: 7,ジョーカーだ！
</CRD_台詞>
```

この場合、NPCのカードの取り方は以下の様になります。
まず、相手の端のカードを取るかどうか判定します。
判定に成功すると、カードを取る対象が２枚に絞られますが、失敗すると、対象はすべてのカードになります。

その後、対象の中の右側に絞ります。(２番目の内容を処理)
「端から取る」に成功している場合、対象は２枚しかないため、その右側、つまり相手の手札の右端のカードが最終的に選ばれます。
失敗している場合は、相手の手札の右側の中からランダムで選ばれます。

また、ジョーカーを引いた時に、7割の確率で台詞を話すようになります。

[目次に戻る](#目次)

# コモンイベント

## カードゲーム中のコモンイベント

プラグインパラメータ`Card Game Event`にコモンイベントIDを設定するとカードゲーム中にコモンイベントを実行できます。

プラグインパラメータ`Card Game Event`には、カンマ(,)とハイフン(-)を使うことで複数のコモンイベントIDを設定できます。
ハイフン(-)は、繋げた前後のIDの間のすべてのIDを登録します。

入力例)
```
1, 4, 5, 10-15
```
複数のイベントIDを入力した場合、入力した順番(左から)に実行条件を判定して、条件を満たしていればそのイベントを実行します。

[目次に戻る](#目次)

## カードゲーム中のコモンイベントの実行条件の設定

コモンイベントに以下の注釈を入力することで、実行条件を設定できます。

```
<スパン: [タイミング]>
<SPAN: [TIMING]>
```
実行回数に関する条件を設定します。
スパンを指定しない場合は、この設定を適用します。
[タイミング] には以下を入力してください。
* ゲーム or GAME　　   - ゲーム中に１回だけ実行します。
* ターン or TURN　　   - ターン中に１回だけ実行します。
* モーメント or MOMENT - 条件を満たす度に実行します。

```
<ターン終了>
<TURNEND>
```
ターン終了時に実行します。

```
<カード引く>
<DRAWCARD>
```
カードを引いた時に実行します。

```
<アクター:a>
<ACTOR:a>
```
アクターID a のターンに実行します。

```
<ターン:a + b *X>
<TURN:a + b *X>
```
指定したターンに実行します。
a と b に数値を入力してください。(* と X は半角, XはそのままXと入力)
* a - 最初に実行するターン数
* b - 次に何ターン後に実行するか(以降この値のターンが経過する毎に実行)

例)
```
<ターン:1 + 2 *X>
<TURN:2 + 4 *X>
```

```
<カード枚数: #a b 枚以下>
<CARD_NUMBERS: #a LESS THAN b>
```
指定したプレイヤーのカード枚数によって実行します。
a と b に数値を入力してください。
a と b の間には必ず半角スペースを入れてください。
* a - プレイヤーNo(1~4)
* b - 残りカード枚数

例)
```
<カード枚数: #1 5 枚以下>
<CARD_NUMBERS: #2 LESS THAN 7>
```

```
<スイッチ: a>
<SWITCH: a>
```
指定したスイッチがONの時に実行します。
a に数値を入力してください。
* a - スイッチID

[目次に戻る](#目次)

## コモンイベントで使用できるスクリプト

コモンイベントには以下のスクリプトが使用できます。

`CardGameManager._turnCount`

ターン数

`CardGameManager.drawCard()`

引いたカードの情報
* CardGameManager.drawCard().suit - カードのスート(マーク)
* CardGameManager.drawCard().rank - カードのランク

`CardGameManager.isDrawJoker()`

ジョーカーを引いたかどうか判定する

[目次に戻る](#目次)

# プラグインの詳細設定
プラグインパラメータで設定可能な、詳細設定の内容を説明します。

* [カードの設定](#カードの設定)
* [ゲームの設定](#ゲームの設定)
* [サウンドの設定](#サウンドの設定)
* [勝敗結果の取得](#勝敗結果の取得)
* [画面レイアウト](#画面レイアウト)
* [コマンドの設定](#コマンドの設定)

## カードの設定
### Suit Type
使用するスート(マーク)を選択します。
spade/club/heart/diamondの文字をカンマ(,)で区切って入力してください。
なお、ババ抜きを行うためには、かならず２の倍数の数のスートを設定してください。

デフォルトでは４種類のスートを使用する設定になっていますが、以下のような設定にすることも可能です。

設定例１)
```
spade,heart
```
この場合、スペードとハートの２種類のみ使用します。
カード枚数はスペード１３枚、ハート１３枚、＋ジョーカーになります。
この設定でも、ババ抜きは問題なく実行可能です。

設定例２)
```
spade,spade
```
この場合、スペードの１種類のみ使用します。
ただし、カード枚数はスペード１３枚×２、＋ジョーカーになります。
この設定でも、ババ抜きは問題なく実行可能です。

### Max Rank
使用するカードの最大ランク(数字)を選択します。
デフォルトでは１３です。（全てのランクを使用）

この値を変更すると、カード枚数が変更されます。

### Number Of Jokers
使用するジョーカーの数を選択します。
ただし、ババ抜きを行うためには必ず１に設定してください。

[上に戻る](#プラグインの詳細設定)

## ゲームの設定
### Subject Player Color
現在ターンのプレイヤーのカラーを設定します。
ゲーム画面のプレイヤーウィンドウに表示されているプレイヤー名が、ここで設定した文字色になります。

カラーは０～３２の値で設定します。
各番号の色は、`img/systemsWindow.png`のカラーパレットに従います。

### Subject Window Tone
現在ターンのプレイヤーのウィンドウカラーを設定します。
R,G,B  とカンマ(,)で区切って０～２５５の範囲の数値を入力してください。
R が赤、G が緑、B が青を意味します。

### Target Player Color
現在ターンのプレイヤーが手札を引く相手のカラーを設定します。
ゲーム画面のプレイヤーウィンドウに表示されているプレイヤー名が、ここで設定した文字色になります。

カラーは０～３２の値で設定します。
各番号の色は、`img/systemsWindow.png`のカラーパレットに従います。

### Ranking Points
ゲームの勝敗の結果で取得するポイントを設定します。
詳しくは[ゲームポイントの設定](#ゲームポイントの設定)を参照してください。

### Draw Speed
手札を引く速さを設定します。
手札を引く高さになるまでの時間が変動します。

なお、この値はプラグインパラメータ`Draw Speed ID`を設定することで、ゲーム中に変更することができます。

### Draw Height
手札を引く高さを設定します。
相手プレイヤーの手札から選んだカードを引く時に、ここで設定した高さの分だけ、カードが動きます。

### Deal Speed
ゲーム開始時に山札を配る時の速さを設定します。

### First Discard Speed
初期の手札からカードを捨てる時の速さを設定します。

[上に戻る](#プラグインの詳細設定)

## サウンドの設定
### Game BGM
カードゲーム中のBGMを設定します。
Name、Pitch、Volumeをそれぞれ設定してください。

なお、カードゲーム実行前に再生していたBGMは、自動的に保存し、カードゲーム終了後に再生します。

### Draw SE
相手プレイヤーから手札を引く時のSEを設定します。
Name、Pitch、Volumeをそれぞれ設定してください。

### Deal SE
カードを配る時のSEを設定します。
Name、Pitch、Volumeをそれぞれ設定してください。

[上に戻る](#プラグインの詳細設定)

## 勝敗結果の取得
### Reset Variables
ゲーム画面表示時に、プラグインパラメータ`Player * Result`で指定した変数に 0 を代入するか設定します。

この設定を有効にすると、自動的にゲーム開始時に各プレイヤーの獲得ポイントが初期化されます。

[上に戻る](#プラグインの詳細設定)

## 画面レイアウト
### Hand ～
各プレイヤーの手札の表示エリアを設定します。
Widthで幅、Heightで高さを設定します。

カードの画像サイズよりも値が小さい場合、カードサイズを縮小して表示します。

### Dialogue ～
台詞ウィンドウの表示エリアを設定します。

Widthで幅、Heightで高さを設定します。

Offset Yで台詞ウィンドウとアクターウィンドウのY座標の差を設定します。

Skinで台詞ウィンドウに個別のウィンドウスキンを設定できます。
設定しない場合は、デフォルトスキンを使用します。

[目次に戻る](#目次)

## コマンドの設定
### Turn End Command
ターンを終了するコマンド名を設定します。

### Sort Command
手札を並び替えるコマンド名を設定します。

[目次に戻る](#目次)

## プラグインの更新履歴

| バージョン | 公開日 | 更新内容 |
| --- | --- | --- |
| [ver1.2.4](FTKR_CardGames.js) | 2018/08/13 | 二人プレイでゲーム終了時に、点数獲得の前にゲーム終了しまう不具合を修正 |
| ver1.2.3 | 2018/08/12 | プレイヤーの最後の台詞が、次のゲームまで残ってしまう不具合を修正<br>手札ウィンドウの背景枠の表示有無の設定機能を追加 |
| ver1.2.2 | 2018/08/12 | プレイヤーが最後まで残った場合に、他に残っているNPCがいないのにカードが引けてしまいエラーになる不具合を修正<br>２ゲーム目以降で、カードを配っている間に決定ボタンを押すと、カードをすべて配っていないにもかかわらず、その時点でカード配りを終了してゲームが始まってしまう不具合を修正 |
| ver1.2.1 | 2017/10/02 | NPCの台詞の設定に関するヘルプの誤記修正 |
| ver1.2.0 | 2017/07/29 | カードゲーム中にコモンイベントを実行する機能を追加 |
| ver1.1.1 | 2017/07/22 | カードを配る時とペアを捨てる時にカードに動きをつける機能を追加<br>カードを配る時のSEを設定する機能を追加 |
| ver1.1.0 | 2017/07/19 | カードを引くときと手札に入れる時にカードに動きをつける機能を追加<br>NPCのターンを自動で進めるように変更<br>ターン中のキャラのステータスウィンドウの色を変更する機能を追加<br>２人プレイと、３人プレイの時の配置を見直し<br>ゲーム中のBGMとカードを引くときのSEを設定する機能を追加 |
| ver1.0.2 | 2017/07/11 | ゲームが進行不能になる不具合修正 |
| ver1.0.1 | 2017/07/09 | プラグインコマンドでエラーになる不具合修正 |
| ver1.0.0 | 2017/07/09 | 正式版公開 |

## ライセンス

本プラグインはMITライセンスのもとで公開しています。

[The MIT License (MIT)](https://opensource.org/licenses/mit-license.php)

#
[目次に戻る](#目次)

[トップページに戻る](README.md)