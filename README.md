マインクラフトでゲームを作ろうとしたときに**ああ、マインクラフトってクソなんだな。** <br>
と思ったことについて書いていきます。<br>

## 衝撃吸収を貫通するダメージについて

体力と衝撃吸収を持っているプレイヤーがいるするじゃないですか。<br>
通常のゲームプレイにはないけど、衝撃吸収を貫通してダメージを与えたい！<br>
そうして実装した後、気づくでしょう。<br>

> サーバー側の体力はあってるのに、クライアントの表示だと体力は減らずに衝撃吸収が減っているぞ？<br>

ﾏｼﾞでｶｽ。<br>

なぜかクライアント側で勝手にダメージ計算されて衝撃吸収に肩代わりされます。<br>

普通に体力を減らしてクライアントに送信するだけだとうまくいきません。<br>
なのでダメージ処理、つまり体力の増減処理をする前にクライアントの**衝撃吸収を0にした後**、処理を実行します<br>
こうすれば勝手に肩代わりされずに済みます<br>

```
(Health = 20, Absorption = 10)

体力増減を検知 (5 の衝撃吸収貫通ダメージ)
↓
UpdateAttributesPacket (Health = 20, Absorption = 0)
↓
体力増減処理 (ダメージ処理)
↓
UpdateAttributesPacket (Health = 15, Absorption = 10)
↓
TADA!!
```
