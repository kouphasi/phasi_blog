+++
title = "【復習デザインパターン】シングルトンパターン"
date = 2026-04-24T00:00:00+09:00
draft = false
tags = ["デザインパターン"]
categories = ["blog"]
summary = "デザインパターンを復習 + 言語化してみた→Singletonパターン"
+++

## Singletonパターン
### 概要

そのクラスのインスタンスが1つしか存在しないことを保証するパターン

### 例

クラスでの例
```ts
class DBConnection {
  private static instance: DBConnection | null = null;

  private constructor() {
    // プライベートコンストラクタ
  }

  public static getInstance(): DBConnection {
    if (!DBConnection.instance) {
      DBConnection.instance = new DBConnection();
    }
    return DBConnection.instance;
  }
}
```

関数型での例
```ts
const getDBConnection = (() => {
  let connection: DBConnection | null = null;

  return () => {
    if (!connection) {
      connection = new DBConnection();
    }
    return connection;
  };
})();
```

### Good

- インスタンスが1つしか存在しないことを保証できる
- リソースの節約ができる
- グローバルにアクセスが可能

### More

- グローバルで状態を持つため、テストがしづらい
- 依存関係が隠れやすい
- マルチスレッドの時に、複数のインスタンスが生成される可能性がある

### 使い所

- フロントエンドでグローバルな状態を定義するとき
- エッジ環境(独立した環境)で、グローバルな状態を定義するとき(DB接続など)
- Logger, 環境変数取得, DB接続など

### 注意

デメリットも多いため、使う時は本当に必要かを考える必要がある
`DI`や`Context`などの他のパターンで代替できないかを考えると良いかも


## 参考
- https://refactoring.guru/ja/design-patterns/singleton
