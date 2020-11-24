# 概要

自宅等のプライベートネットワークでホスト名叩いてWebアプリをSP端末テストする用のローカルDNSを立てる

- 開発PCからローカルで名前解決したい場合はhostsファイルいじればOK。この方法は不要。
- アプリ毎にポート変えて立ち上げしてる場合もこの方法は不要。
- 複数アプリをビルドして1つのWebサーバでバーチャルホストしてる場合はこの方法が有効。

## 手順（最低限）

1. `hosts-dnsmasq` に名前解決するホスト名列挙
2. dnsmasq立ち上げ

## 手順（解説付き）

開発PCとSP端末を自宅wifi等で同一ネットワークに置く。

```
docker-dns
 - dnsmasq.conf
 - honts-dnsmasq
 - docker-compose.yml
```

ここをカレントディレクトリに作業する。

`hosts-dnsmasq` にIP群を記載。

- 開発PCで直に立ち上げている場合は開発PCのプライベートIPアドレス
- vagrantや仮想化でネットワーク構築している場合は、そのアプリが動いている環境のIPを適宜確認。

例

```
192.168.3.3 hogehoge.com
192.168.3.3 hugahuga.com
```

`docker-compose up -d` でDNSサーバを立ち上げる。

`nslookup` で正しくDNSサーバが立ち上がってるか確認。

例

```
nslookup hogehoge.com 192.168.3.3
Server:		192.168.3.3
Address:	192.168.3.3#53

Name:	hogehoge.com    //ここ2行があればOK
Address: 192.168.3.3    //ここ2行があればOK
```

確認したら、SP端末にDNSを設定してホスト名アクセスしてみる。
SP端末のキャッシュ削除は再起動する。
