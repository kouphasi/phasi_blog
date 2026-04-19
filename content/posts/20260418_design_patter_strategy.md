+++
title = "【復習デザインパターン】ストラテジーパターン"
date = 2026-04-18T23:35:00+09:00
draft = false
tags = ["デザインパターン"]
categories = ["blog"]
summary = "デザインパターンを復習 + 言語化してみた→Strategyパターン"
+++

## 前置き

これは個人的なデザインパターンの学習ログです
わかりづらいかもしれませんが、万人が理解できるように書いていないのでご了承ください

## 概要

同じ目的だけどやり方がいくつかある場合に、やり方を切り替えられるようにするパターン

お会計をするときに支払い方法という共通の目的でもやり方はいくつかあるが、それらを切り替えられるようにするイメージ
- 現金
- クレカ
- QRコード



## 例

### Bad
支払い方法に応じて条件分岐しちゃう
```ts
const pay = (way: 'cash' | 'card' | 'qr', price: number) => {
  if (way === 'cash') {
    // 現金で支払う処理
  } else if (way === 'card') {
    // クレカで支払う処理
  } else if (way === 'qr') {
    // QRコードで支払う処理
  }  
}
```

### Good
```ts
type PayStrategy = (price: number) => void

const payByCash: PayStrategy = (price) => {
  // 現金で支払う処理
}

const payByCard: PayStrategy = (price) => {
  // クレカで支払う処理
}

const payByQr: PayStrategy = (price) => {
  // QRコードで支払う処理
}
```

このように方法(Strategy)を分けて定義すると以下のように、支払い方法を切り替えやすくなる

```ts
const pay = (payStrategy: PayStrategy, price: number) => {
  payStrategy(price)  
}
```

また、同じ支払い方法を固定で使い続けようとする場合以下のようにもできる(context)
```ts
const generatePayContext = (payStrategy: PayStrategy) => (price: number) => {
  payStrategy(price)  
}

const payByCashContext = generatePayContext(payByCash)
payByCashContext(1000) // 現金で1000円支払う
```

## Bridgeパターンとの違い
### 目的が違う

Strategyパターン: 同じ目的だけどやり方がいくつかある場合に、やり方を切り替えられるようにするパターン

Bridgeパターン: 変化する可能性がある2つの軸を分離するパターン

(関数型だとより同じに見えやすいが)

### 見分け方

Strategy: 1つアルゴリズムを選ぶだけ

Bridge: 2つの軸を組み合わせる(`n x m`が実現できる)

## 参考
- https://refactoring.guru/ja/design-patterns/strategy
