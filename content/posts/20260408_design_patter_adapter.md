+++
title = "【復習デザインパターン】アダプタパターン"
date = 2026-04-09T23:35:00+09:00
draft = false
tags = ["デザインパターン"]
categories = ["blog"]
summary = "デザインパターンを復習 + 言語化してみた→Adapterパターン"
+++

## 概要

すでに出来上がってしまったいじれないクラスと実装したいインターフェースがあって、両者をつなげるためのパターン

## 例

`VanillaShake`(変更できないクラス)
```dart
class VanillaShake {
  void eat() {
    print("eating");
  }
}
```

`Drink`(望むインターフェース)
```dart
abstract class Drink {
  void drink();
}
```

`VanillaShakeAdapter`(望むインターフェースを提供する`VanillaShake`)
(継承を利用する方法)
```dart
class VanillaShakeAdapter extends VanillaShake implements Drink {
  @override
  void drink() {
    super.eat();
  }
}
```

(継承を使わない方法)
```dart
class VanillaShakeAdapter implements Drink {
  final VanillaShake vanillaShake;
  
  VanillaShakeAdapter(this.vanillaShake);
  
  @override
  void drink() {
    vanillaShake.eat();
  }
}
```


## 使い所

- すでに出来上がっていて変更コストが高いメソッドやクラスを柔軟に使いたい時
- 外部ライブラリなどで本体のコードを触りづらい時


## 参考
- https://www.techscore.com/tech/DesignPattern/Adapter/
