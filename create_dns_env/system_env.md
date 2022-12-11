# DNS環境構築

実際に運用されている環境を参考にしながら構築しました。  

## 1. 環境

- macOS Monterey12.1
- Vagrant 2.3.1

### ①イメージ

### ②ネットワーク図

## 2. ソフトウェア条件

### 選定条件

権威DNSサーバー

- DNSレスポンスが早い
- セキュリティ対策ができる
- 運用が簡単にできる

キャッシュDNSサーバー

- 速度が早い
- セキュリティ対策が可能

### 前提条件

権威DNSサーバー

- 権威DNSサーバーはプライマリーサーバー、セカンダリサーバーのつに分ける(マスター/スレーブ構成)
  - 権威DNSサーバーの冗長化
- BINDは使用しない
  - セキュリティアップデートのコストが大きく、運用が大変になるため
  - デファクトスダンダードではあるが、現段階からDNSサーバーを構築する場合、BINDにする意味があまりない。
- OSSをインストールするLinuxディストリビューションの決まりは特になし。

キャッシュDNSサーバー

- BINDは使用しない
  - セキュリティアップデートのコストが大きく、運用が大変になるため
- OSSをインストールするLinuxディストリビューションの決まりは特になし。

## 3. ソフトウェア選定

### 権威DNSサーバー(プライマリサーバー)

PowerDNS社が開発したPowerDNS Authoritative Nameserverを採用。  
<https://doc.powerdns.com/authoritative/#powerdns-authoritative-nameserver>

- MySQL、PostgreSQLなどのミドルウェア連携をすることができる
  - PowerAdminというGUIを使用して簡単にゾーンファイルの修正などを行えるので、運用しやすい。
  - PowerAdminのレコードはAPIから更新可能
- BINDよりもレスポンスが早い
- 脆弱性が少ない
- DNSSECに対応

### 権威DNSサーバー(セカンダリサーバー)

NLnet LabsとPIPE NCCが共同で開発した、NSDを採用。  
<https://www.nlnetlabs.nl/projects/nsd/about/>

- 権威DNSを提供しているサービスの中では、レスポンス速度がかなり早い
  - PowerDNSやKnotDNSより早い
  - セカンダリサーバーをフロントに立たせるため、レスポンス速度は重視しました
- 設定自体は簡単に行うことができる
- BINDよりも脆弱性が少ない

### キャッシュDNSサーバー(フルリゾルバ)

NLnet Labsが開発し公開しているunboundを採用。  
<https://www.nlnetlabs.nl/projects/unbound/about/>  

- キャッシュサーバーとしての用途に特化している
- 多くのLinuxディストリビューションに対応している
- 設定自体が簡単に行うことができる
- BINDに比べてかなり高速に動作する
- キャッシュ攻撃を検知したり、セキュリティも高い。

### Webサーバー

今回はnginxを採用。  
理由としては特にありませんが、前回Apacheを使用してサーバーを構築したのでなんとなく触っておこうと思いまして笑
<https://nginx.org/en/>
