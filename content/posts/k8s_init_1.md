+++
title = "k8sざっくり入門"
date = 2026-03-03T22:45:00+09:00
draft = false
tags = ["k8s", "インフラ"]
categories = ["blog"]
summary = "k8sのそもそもの概念を自分なりにまとめてみた"
+++

## はじめに

そもそもk8sは概念が難しい
勉強会で複数回説明を聞いてやっと腑に落ちたので、脳内の整理のために自分なりにまとめてみる

## k8sの概念

### 種類
1. Cluster
2. Master Node
3. Node
4. Pod

### 1. Cluster
typoに注意(`clustor`ではなく`cluster`)

k8sの一連の塊を指す
k8sの中で一番大きな概念

### 2. Master Node
k8sのNodeやPodの管理を司るNode
PodやNodeの数の増減やそれ以外の包括的な管理を行うNode
AWSのEKSでは基本的にはデフォルトで用意されているため、特に設定は不要

### 3. Node
Podの実行を行うマシンのことを指す

AWSのEKSではEC2インスタンスのことを指す

### 4. Pod
コンテナの集合体のことを指す
複数のコンテナのまとまりを指す(docker-composeにイメージが近い)

起動も停止も一心同体にしたいものをPodにまとめると良い

そうでないケースは1つのPodに1つのコンテナを配置する方がいい


## マニフェスト

### マニフェストファイルとは
宣言的にNodeの状態やPodの状態を定義できるファイル
設定内容のリソース(kind)を指定して、細かく設定を記述していく

### 代表的なリソース)
#### 1. Deployment
どこNodeでもいいからPodを起動してくれるリソース
逆にどのNodeでPodが起動するかわからない
(Node内に複数のPodが立ち上がるケースがある)

#### 2. DemonSet
1つのNodeに必ず1つのPodを起動してくれるリソース
Nodeが立ち上がったら、まずこのリソースのPodが優先的に立ち上がるようになってる

#### 3. StatefulSet
StatefulなPodを立ち上げる
そのPodの中のVolumeはPodが削除されても残る(再度同じPodを立ち上げるときに同じVolumeをマウントすることができる)

#### 4. Service
Podの通信を管理するリソース
いくつか種類がある
- ClusterIP: クラスター内で通信のために公開する
- NodePort: クラスター外から通信のために公開する
- LoadBalancer: クラスター外から通信のために公開する(AWSのALBなどの外部のロードバランサーと連携して公開する)
- ExternalName: ?

#### 5. HorizontalPodAutoscaler
Podの数を自動で増減させるリソース
Podのコントロールをするリソースなのではなく、DeploymentなどのPodの数の設定値を自動で増減させるリソース
(Podのコントロールをするわけでないので注意)

#### 6. ConfigMap
Podの中の設定ファイルを管理するリソース

#### 7. Secret
Podの中の非公開の設定ファイルを管理するリソース

#### 8. Ingress
Podへの外部からのアクセスを管理するリソース
外部とServiceをつなぐ役割を持つリソース

#### 9. Pod Disruption Budget
Deploymentで定義されたPodの立ち上がる最低の数(これ以上は少なくなれない数)を定義するリソース
Podの数がこの数と一致すると、Podの削除などができなくなる


## 出典
- https://qiita.com/tadashiro_ninomiya/items/6e6fea807b2a16732b5b
- https://zenn.dev/sasakiki/articles/a71d9158020266
