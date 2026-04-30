+++
title = "【復習デザインパターン】Flyweightパターン"
date = 2026-04-29T15:45:00+09:00
draft = false
tags = ["デザインパターン"]
categories = ["blog"]
summary = "デザインパターンを復習 + 言語化してみた→Flyweightパターン"
+++

## Flyweightパターン

### 概要

共通部分を切り出して、同じものを使い回すことで、メモリ使用量を減らすパターン(正規化と似ている)

### 例

**bad**

```ts
type Tree = {
  poaition: { x: number; y: number };
  color: string;
  length: number;
}

// 大量の木を生成
const tree1 = {
  poaition: { x: 1, y: 2 },
  color: 'green',
  length: 10,
};

const tree2 = {
  poaition: { x: 1, y: 2 },
  color: 'green',
  length: 10,
};

//...
```

`color`や`length`が同じ値を持っているが、それぞれの木で同じ値を持っているため、メモリの無駄遣いになっている

**good**

```ts
type TreeType = {
  color: string;
  length: number;
}

type Tree = {
  poaition: { x: number; y: number };
  type: TreeType;
}

const oakType = {
  color: 'green',
  length: 10,
}

const tree1 = {
  poaition: { x: 1, y: 2 },
  type: oakType,
};

const tree2 = {
  poaition: { x: 1, y: 2 },
  type: oakType,
};

//...
```

### 使い所

- メモリ使用量を減らしたいとき
  - 繰り返しの多いデータが大きいほど効果が大きい(当たり前)

### 注意点

- 共有部分を切り出すと、コードが複雑になる可能性がある
  - データが追いづらくなったり、変更が難しくなったりする可能性がある

## 参考
- https://refactoring.guru/ja/design-patterns/flyweight
