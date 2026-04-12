+++
title = "【復習デザインパターン】アダプタパターン"
date = 2026-04-09T23:35:00+09:00
draft = false
tags = ["デザインパターン"]
categories = ["blog"]
summary = "デザインパターンを復習 + 言語化してみた→Adapterパターン"
+++

## 概要

インターフェースの違うオブジェクトを繋ぎ合わせるパターン

## 例

`VanillaShake`
```typescript
interface VanillaShake {
  eat(): void;
}
```

`Drink`
```typescript
interface Drink {
  drink(): void;
}
```

`haveMeals`
```typescript
const haveMeals = (food: Food, drink: Drink) => {
  food.eat()
  drink.drink()
}
```
この引数に直接`VanillaShake`を渡すことはできない

`vanillaShakeAdapter`
```typescript
const vanillaShakeAdapter = (vanillaShake: VanillaShake) => ({
  drink: () => vanillaShake.eat()  
})
```

このメソッドにより、インターフェースの違う`VanillaShake`を`Drink`として扱えるようになる
```typescript
haveMeals(food, vanillaShakeAdapter(vanillaShake))
```

## 使い所

- すでに出来上がっていて変更コストが高いメソッドやクラスを別のインターフェースに当て嵌めたい時
- 外部ライブラリなどで本体のコードを触りづらい時

## Bridgeパターンとの違い

Adapterは2つの異なるinterfaceを繋げるパターン。

しかし、Bridgeパターンは処理の中身を外から注入するパターン

Bridgeを使わないと以下のようになってしまう
```typescript
// リモコン
const tvController = {
  sendIr: () => {
    console.log("IR signal sent") // 赤外線を送る処理
  },
  turnOn: () => {
    sendIr()
    console.log("turn on tv")
  }
}

// 物理スイッチ
const tvSwitch = {
  turnOn: () => {
    console.log("turn on tv")
  }
}

const radioController = {
  sendIr: () => {
    console.log("IR signal sent") // 赤外線を送る処理
  },
  turnOn: () => {
    sendIr()
    console.log("turn on radio")
  }
}

const radioSwitch = {
  turnOn: () => {
    console.log("turn on radio")
  }
}
```
実行命令を送る方法(リモコンや物理スイッチ)が増えるとデバイス分の命令方法の定義が必要になる
デバイスが増えると、同じような命令方法でも再度定義が必要になる


デバイスの動作(機能)と命令方法(実装)を分離して、命令にを外から注入するようにすれば、デバイス x 命令の定義が必要なくなる
```typescript
interface Device = {
  turnOn: () => void;
}

const tv: Device = {
  turnOn: () => {
    console.log("turn on tv")
  } 
}

const radio: Device = {
  turnOn: () => {
    console.log("turn on radio")
  }
}

const createController = (device: Device) => ({
  turnOn: () => {
    console.log("IR signal sent") // 赤外線を送る処理
    device.turnOn()
  }
})

const createSwitch = (device: Device) => ({
  turnOn: () => {
    device.turnOn()
  }
})

const tvController = createController(tv)
const tvSwitch = createSwitch(tv)
const radioController = createController(radio)
const radioSwitch = createSwitch(radio)
```


## 参考
- https://www.techscore.com/tech/DesignPattern/Adapter/
- https://refactoring.guru/ja/design-patterns/adapter
