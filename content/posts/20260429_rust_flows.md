+++
title = "【Rust】制御フローの書き方"
date = 2026-04-29T16:02:00+09:00
draft = false
tags = ["Rust"]
categories = ["blog"]
summary = "Rustの学習まとめ"
+++

## 条件分岐

### if式
ifは**式**である

ifの結果を変数に代入できる

ifの後は`()`で条件式を括る必要がない

それ以外は普通のif文と同じ

```rs
let age = 24;

let age_group = if age < 20 {
  "未成年"
} else if age < 65 {
  "成人"
} else {
  "高齢者"
};
```

## 繰り返し

### loop式
**loop**を使用すると無限ループになる

loopももちろん**式**である

```rs
let mut count = 0;

let final_count = loop {
  count += 1;

  if count == 5 {
    break count * 2; // breakの後に値を指定することで、loop式全体の値を指定できる
  }
};
```

`break`は一番内側のループを抜ける(外側には影響しないので安心)

この特性はloop以外のループ構文でも同様

### while
loopの条件付きバージョン(もちろん**式**)

```rs
let mut count = 0;

let final_count = while count < 5 {
  count += 1;
};
```


### for式

いわゆる**for**で**式**である

書き方は

```rs
for value in iterable {
  // 繰り返し処理
}
```

```rs
let numbers = [1, 2, 3, 4, 5];

for n in numbers {
  println!("{}", n);
}
```

## 参考
- https://doc.rust-jp.rs/book-ja/ch03-05-control-flow.html
