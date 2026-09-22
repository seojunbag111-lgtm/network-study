# 04-dns-resolution-check

## 1. 目的 (Objective)

この実習では、DNSによる名前解決を行い、ドメイン名google.comからIPアドレスを取得する過程を実際に確認する。

## 2. 環境 (Environment)

- OS : Linux (Ubuntu)
- Environment : WSL2
- Tools / Commands : `dig`、`nslookup`

## 3. 実習 (Practice)

### Step 1. `dig`によるDNS情報の確認

```bash
dig google.com
```

```text
; <<>> DiG 9.20.24-1ubuntu0.3-Ubuntu <<>> google.com
;; global options: +cmd
;; Got answer:
;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 8667
;; flags: qr rd ra; QUERY: 1, ANSWER: 6, AUTHORITY: 0, ADDITIONAL: 1

;; OPT PSEUDOSECTION:
; EDNS: version: 0, flags:; udp: 1232
;; QUESTION SECTION:
;google.com.                    IN      A

;; ANSWER SECTION:
google.com.             85      IN      A       172.217.213.113
google.com.             85      IN      A       172.217.213.102
google.com.             85      IN      A       172.217.213.100
google.com.             85      IN      A       172.217.213.101
google.com.             85      IN      A       172.217.213.139
google.com.             85      IN      A       172.217.213.138

;; Query time: 27 msec
;; SERVER: 10.255.255.254#53(10.255.255.254) (UDP)
;; WHEN: Tue Sep 22 19:04:47 KST 2026
;; MSG SIZE  rcvd: 135
```

### Step 2. `nslookup`によるDNS情報の確認

```bash
nslookup google.com
```

```text
Server:         10.255.255.254
Address:        10.255.255.254#53

Non-authoritative answer:
Name:   google.com
Address: 172.217.209.138
Name:   google.com
Address: 172.217.209.101
Name:   google.com
Address: 172.217.209.100
Name:   google.com
Address: 172.217.209.102
Name:   google.com
Address: 172.217.209.113
Name:   google.com
Address: 172.217.209.139
Name:   google.com
Address: 2404:6800:400a:1009::66
Name:   google.com
Address: 2404:6800:400a:1009::8b
Name:   google.com
Address: 2404:6800:400a:1009::65
Name:   google.com
Address: 2404:6800:400a:1009::71
```

## 4. 結果 (Result)

- `dig google.com`では、6つのIPv4アドレスが表示された。
- `nslookup google.com`では、`dig google.com`と同じDNSサーバーを使用していることを確認した。
- `nslookup google.com`では、IPv4アドレスだけではなく、IPv6アドレスも表示された。
- `nslookup google.com`では、応答したDNSサーバーがgoogle.comの権限DNSサーバーではないことを表す`Non-authoritative answer`が表示された。

## 5. 結果の分析 (Analysis)

- `dig google.com`の`google.com.  85  IN  A  172.217.213.113`から、TTLが85秒であり、Aレコードであることを確認できた。
- `dig google.com`の`;; Query time: 27 msec`と`;; SERVER: 10.255.255.254#53(10.255.255.254) (UDP)`から、使用したDNSサーバー(10.255.255.254)、ポート番号(53)、トランスポートプロトコル(UDP)、Query time(27ms)を確認できた。

## 6. 学びと考察 (Learning & Insights)

- `dig`と`nslookup`を使用することで、ドメイン名からIPアドレスを取得するDNSの名前解決を実際に確認することができた。
- DNSの名前解決結果にはIPアドレスだけではなく、DNSサーバーやレコードの種類など、さまざまな情報が含まれていることを理解した。