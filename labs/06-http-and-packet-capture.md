# 06-http-and-packet-capture

## 1. 目的 (Objective)

この実習では、HTTP通信を行い、TCP接続からHTTPリクエスト・レスポンス、TCP接続の終了までの通信をtcpdumpで実際に確認する。

## 2. 環境 (Environment)

- OS : Linux (Ubuntu)
- Environment : WSL2
- Tools / Commands : `tcpdump`、`curl`、`python3 -m http.server 8000`

## 3. 実習 (Practice)

### Step 1. サーバーを起動し、tcpdumpを実行

```bash
python3 -m http.server 8000
```

```text
Serving HTTP on 0.0.0.0 port 8000 (http://0.0.0.0:8000/) ...
```

```bash
sudo tcpdump -i lo -nn port 8000
```

```text
tcpdump: verbose output suppressed, use -v[v]... for full protocol decode
listening on lo, link-type EN10MB (Ethernet), snapshot length 262144 bytes
```

### Step 2. curlでHTTPリクエストを送信

```bash
curl http://127.0.0.1:8000
```

```text
Hello Network
```

### Step 3. tcpdumpのキャプチャ内容を確認

```text
tcpdump: verbose output suppressed, use -v[v]... for full protocol decode
listening on lo, link-type EN10MB (Ethernet), snapshot length 262144 bytes
22:19:20.306333 IP 127.0.0.1.40648 > 127.0.0.1.8000: Flags [S], seq 1530755386, win 65495, options [mss 65495,sackOK,TS val 1188704625 ecr 0,nop,wscale 10], length 0
22:19:20.306358 IP 127.0.0.1.8000 > 127.0.0.1.40648: Flags [S.], seq 325041635, ack 1530755387, win 65483, options [mss 65495,sackOK,TS val 190218233 ecr 1188704625,nop,wscale 10], length 0
22:19:20.306371 IP 127.0.0.1.40648 > 127.0.0.1.8000: Flags [.], ack 1, win 64, options [nop,nop,TS val 1188704625 ecr 190218233], length 0
22:19:20.306445 IP 127.0.0.1.40648 > 127.0.0.1.8000: Flags [P.], seq 1:79, ack 1, win 64, options [nop,nop,TS val 1188704625 ecr 190218233], length 78
22:19:20.306451 IP 127.0.0.1.8000 > 127.0.0.1.40648: Flags [.], ack 79, win 64, options [nop,nop,TS val 190218233 ecr 1188704625], length 0
22:19:20.313589 IP 127.0.0.1.8000 > 127.0.0.1.40648: Flags [P.], seq 1:186, ack 79, win 64, options [nop,nop,TS val 190218240 ecr 1188704625], length 185
22:19:20.313633 IP 127.0.0.1.40648 > 127.0.0.1.8000: Flags [.], ack 186, win 64, options [nop,nop,TS val 1188704632 ecr 190218240], length 0
22:19:20.313737 IP 127.0.0.1.8000 > 127.0.0.1.40648: Flags [P.], seq 186:200, ack 79, win 64, options [nop,nop,TS val 190218240 ecr 1188704632], length 14
22:19:20.313742 IP 127.0.0.1.40648 > 127.0.0.1.8000: Flags [.], ack 200, win 64, options [nop,nop,TS val 1188704632 ecr 190218240], length 0
22:19:20.313858 IP 127.0.0.1.8000 > 127.0.0.1.40648: Flags [F.], seq 200, ack 79, win 64, options [nop,nop,TS val 190218240 ecr 1188704632], length 0
22:19:20.313962 IP 127.0.0.1.40648 > 127.0.0.1.8000: Flags [F.], seq 79, ack 201, win 64, options [nop,nop,TS val 1188704632 ecr 190218240], length 0
22:19:20.313997 IP 127.0.0.1.8000 > 127.0.0.1.40648: Flags [.], ack 80, win 64, options [nop,nop,TS val 190218240 ecr 1188704632], length 0
```

## 4. 結果 (Result)

- `curl`でHTTPサーバーにリクエストを送信すると、サーバーからHello Networkというレスポンスが返された。
- `tcpdump`では、127.0.0.1の40648番ポートと8000番ポートの間で複数のTCPパケットが送受信されていることを確認した。
- 通信の開始時には[S]、[S.]、[.]、データの送受信時には[P.]、通信の終了時には[F.]が表示された。

## 5. 結果の分析 (Analysis)

- `curl`は40648番ポートを使用し、HTTPサーバーは8000番ポートを使用して通信していることを確認できた。
- [S]（SYN）→ [S.]（SYN + ACK）→ [.]（ACK）から、TCPの3-way handshakeを実際に確認できた。
- TCP接続後、[P.]が表示されたパケットでデータが送受信され、サーバーログとcurlの結果からGET/に対して200のレスポンスが返されたことを確認できた。
- 最後に[F.]が表示されたことから、TCP接続を終了する過程も確認できた。

## 6. 学びと考察 (Learning & Insights)

- `tcpdump`を使用することで、TCP接続の開始、データの送受信、接続の終了までの流れを実際のパケットで確認することができた。
- これまで学んだTCPの3-way handshakeやHTTPリクエスト・レスポンスの仕組みを実際の通信を確認することで理解することができた。