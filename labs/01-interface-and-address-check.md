# 01-interface-and-address-check

## 1. 目的 (Objective)

この実習では、Linux環境のネットワークインターフェースと、それぞれに割り当てられたアドレスを実際に確認する。

## 2. 環境 (Environment)

- OS : Linux (Ubuntu)
- Environment : WSL2
- Tools / Commands : `ip link`、`ip addr`

## 3. 実習 (Practice)

### Step 1. ネットワークインターフェースの確認

``` bash
ip link
```

```text
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN mode DEFAULT group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
2: eth0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc mq state UP mode DEFAULT group default qlen 1000
    link/ether 00:15:5d:28:1b:b8 brd ff:ff:ff:ff:ff:ff
```

### Step 2. IPアドレスの確認

```bash
ip addr
```

``` text
[1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
    inet6 ::1/128 scope host
       valid_lft forever preferred_lft forever
2: eth0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc mq state UP group default qlen 1000
    link/ether 00:15:5d:28:1b:b8 brd ff:ff:ff:ff:ff:ff
    inet 172.26.172.182/20 brd 172.26.175.255 scope global eth0
       valid_lft forever preferred_lft forever
    inet6 fe80::215:5dff:fe28:1bb8/64 scope link
       valid_lft forever preferred_lft forever]
```

## 4. 結果 (Result)

`ip link`と`ip addr`の実行結果から、以下の情報を確認した。

- loというループバックインターフェースが存在し、127.0.0.1/8が割り当てられている。
- eth0というネットワークインターフェースが存在し、MACアドレスとして00:15:5d:28:1b:b8が割り当てられている。
- eth0には、IPv4アドレスとして172.26.172.182/20が割り当てられている。
- loとeth0 では、それぞれ異なるMTUが設定されている。

## 5. 結果の分析 (Analysis)

- loは、ホストが自分自身と通信するために使用するループバックインターフェースである。
- eth0は、ネットワーク通信に使用されるネットワークインターフェースである。
- `ip link`では、ネットワークインターフェースの情報を確認でき、`ip addr`では、それに加えてIPアドレスも確認できる。

## 6. 学びと考察 (Learning & Insights)

- loやeth0のようなネットワークインターフェースを実際に確認することで、インターフェースごとに異なる役割や設定があることを理解した。
- `ip link`と`ip addr`の出力を比較することで、ネットワークインターフェースの情報と、それに割り当てられたIPアドレスの情報を確認できることを理解した。