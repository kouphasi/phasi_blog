+++
title = "【復習デザインパターン】イテレータパターン"
date = 2026-03-31T23:35:00+09:00
draft = false
tags = ["デザインパターン"]
categories = ["blog"]
summary = "デザインパターンを復習 + 言語化してみた→イテレータパターン"
+++

## 登場人物(?)

### Iterator

要素を順番にアクセスするためのオブジェクト

最低限以下の要素が必要
1. *次の要素が存在するか*
2. *今の要素が何か*

### Aggregate(Iterable)

Aggregate内の要素を順番にアクセスするためのIteratorを生成するためのオブジェクト

## 概要(ざっくりイメージ)

Aggregator内の要素を順番に取り出すためのメソッドがIterator

## Iterator

Iteratorには、以下の二つを持つオブジェクトである必要がある
1. *次の要素が存在するか*
2. *今の要素が何か*

dartでの例
```dart
class Iterator<T> {
  bool hasNext() {
    // 次の要素が存在するか
  }

  <T> next() {
    // 今の要素が何か
  }
}
```

Iteratorはこれだけを提供する

`hasNext`や`next`のロジックはなんでもいい

dartのclassで実装しているが、
jsのオブジェクトでもいい。

とにかくこの2種類のメソッドを提供するオブジェクトであれば、Iteratorと呼べる


## Aggregator

Aggregator自身が持ってる要素に順番にアクセスするためのIteratorを生成するためのオブジェクト

```dart
class Aggregator<T> {
  List<T> items;
  Iterator<T> createIterator() {
    // Iteratorを生成して返す
    return Iterator<T>(items);
  }
}
```

## 具体例

宝くじ(`Lottery`)で例える

```dart
class Lottery {}
```

Iteratorは購入者に対して宝くじをランダムで返すためのメソッド(定員さん)
```dart
class LotteryStaff {
  List<Lottery> lotteries;
  LotteryStaff(this.lotteries);

  Lottery hasNext() {
    // 次の宝くじが存在するか
  }
  Lottery next() {
    // 渡す宝くじがどのくじか
  } 
}
````

Aggregateは宝くじ売り場
- 宝くじがたくさん用意されている
- 定員さんがいる(Iterator)
```dart
class LotteryShop {
  List<Lottery> lotteries;
  LotteryStaff staff;
  LotteryShop(this.lotteries) {
    staff = LotteryStaff(lotteries);
  }

  LotteryStaff createIterator() {
    // 定員さんを生成して返す
    return staff;
}
```

## 使い所(まとめ)

基本的には配列などのコレクションで十分だが、

`取り出し方を工夫したい時` や `それを共通化したい時`

には役に立ってくれるだろう

## 参考
- https://www.techscore.com/tech/DesignPattern/Iterator/Iterator1
