+++
title = "Agent SkillsをいろんなAIで使えるようにするための壁"
date = 2026-03-13T00:15:00+09:00
draft = false
tags = ["AI", "ポエム"]
categories = ["blog"]
summary = "20260313時点ではCodexとClaudeの両方でskillsを使えるように共通化するのは難しい"
+++

## はじめに

ClaudeとCodexの両刀使いとなったわたくしは、AI関連のファイルをできるだけいろんなAIから参照しやすいように共通化する方法を考えていました。

`CLAUDE.md`と`AGENTS.md`もその一つで共通のファイルを作ってシンボリックリンク経由で参照するという方法を行った。

しかしskillsに関してはその方法が通じない。その苦悩を記す。

## ディレクトリのシンボリックリンクは辿れない

`CLAUDE.md`などの場合はシンボリックリンクは正常に動作しますが、skillsに関してはディレクトリのため、うまく認識できませんでした。

この現象はClaudeでもCodexでも同じ。

## Claude CodeとCodexのSkillsの置き場所が違う

- **claude code**: `./.claude/skills/`
- **codex**: `./codex/skills/` or `./.agent/skills/`

同じ中身をコピーしないとうまく実現できないです。
結構不便。


## 思うこと

SKILLSの規格は揃えてるんだったら、その置き場所も含めて揃えてほしい。

あと一歩惜しいから不便なんだよな。


## 参考
- https://code.claude.com/docs/ja/overview
- https://developers.openai.com/codex/skills/
