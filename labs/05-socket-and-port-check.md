# 05-socket-and-port-check

## 1. 目的 (Objective)

この実習では、サーバーを起動・停止する際のリスニングポートの変化を実際に確認する。

## 2. 環境 (Environment)

- OS : Linux (Ubuntu)
- Environment : WSL2
- Tools / Commands : `ss`、`python3 -m http.server 8000`

## 3. 実習 (Practice)

### Step 1. ソケットの情報を確認

```bash
ss -tuln
```

```text
Netid        State         Recv-Q        Send-Q                Local Address:Port               Peer Address:Port
udp          UNCONN        0             0                        127.0.0.54:53                      0.0.0.0:*
udp          UNCONN        0             0                     127.0.0.53%lo:53                      0.0.0.0:*
udp          UNCONN        0             0                    10.255.255.254:53                      0.0.0.0:*
udp          UNCONN        0             0                         127.0.0.1:323                     0.0.0.0:*
udp          UNCONN        0             0                         127.0.0.1:323                     0.0.0.0:*
udp          UNCONN        0             0                             [::1]:323                        [::]:*
udp          UNCONN        0             0                             [::1]:323                        [::]:*
tcp          LISTEN        0             4096                     127.0.0.54:53                      0.0.0.0:*
tcp          LISTEN        0             1000                 10.255.255.254:53                      0.0.0.0:*
tcp          LISTEN        0             4096                  127.0.0.53%lo:53                      0.0.0.0:*
```

### Step 2. サーバーを起動し、ソケットの情報を確認

```bash
python3 -m http.server 8000
```

```text
Serving HTTP on 0.0.0.0 port 8000 (http://0.0.0.0:8000/) ...
```

```bash
ss -tuln
```

```text
Netid     State      Recv-Q     Send-Q          Local Address:Port         Peer Address:Port
udp       UNCONN     0          0                  127.0.0.54:53                0.0.0.0:*
udp       UNCONN     0          0               127.0.0.53%lo:53                0.0.0.0:*
udp       UNCONN     0          0              10.255.255.254:53                0.0.0.0:*
udp       UNCONN     0          0                   127.0.0.1:323               0.0.0.0:*
udp       UNCONN     0          0                   127.0.0.1:323               0.0.0.0:*
udp       UNCONN     0          0                       [::1]:323                  [::]:*
udp       UNCONN     0          0                       [::1]:323                  [::]:*
tcp       LISTEN     0          4096               127.0.0.54:53                0.0.0.0:*
tcp       LISTEN     0          1000           10.255.255.254:53                0.0.0.0:*
tcp       LISTEN     0          4096            127.0.0.53%lo:53                0.0.0.0:*
tcp       LISTEN     0          5                     0.0.0.0:8000              0.0.0.0:*
```

### Step 3. サーバーを停止し、ソケットの情報を確認

```bash
^C
```

```text
Keyboard interrupt received, exiting.
```

```bash
ss -tuln
```

```text
Netid        State         Recv-Q        Send-Q                Local Address:Port               Peer Address:Port
udp          UNCONN        0             0                        127.0.0.54:53                      0.0.0.0:*
udp          UNCONN        0             0                     127.0.0.53%lo:53                      0.0.0.0:*
udp          UNCONN        0             0                    10.255.255.254:53                      0.0.0.0:*
udp          UNCONN        0             0                         127.0.0.1:323                     0.0.0.0:*
udp          UNCONN        0             0                         127.0.0.1:323                     0.0.0.0:*
udp          UNCONN        0             0                             [::1]:323                        [::]:*
udp          UNCONN        0             0                             [::1]:323                        [::]:*
tcp          LISTEN        0             4096                     127.0.0.54:53                      0.0.0.0:*
tcp          LISTEN        0             1000                 10.255.255.254:53                      0.0.0.0:*
tcp          LISTEN        0             4096                  127.0.0.53%lo:53                      0.0.0.0:*
```

## 4. 結果 (Result)

- サーバーを起動し、ソケットの情報を確認すると、`tcp  LISTEN  0   5   0.0.0.0:8000    0.0.0.0:*`が追加された。
- サーバーを停止して再びソケットの情報を確認すると、`tcp  LISTEN  0   5   0.0.0.0:8000    0.0.0.0:*`が消えた。

## 5. 結果の分析 (Analysis)

- tcpとLISTENから、TCPソケットが接続を待ち受けている状態であることを確認できた。
- 0.0.0.0:8000から、HTTPサーバーが8000番ポートで接続を待ち受けていることを確認できた。
- サーバーを起動した際に8000番ポートのリスニングソケットが追加され、サーバーの停止後に表示されなくなったことから、サーバーの起動・停止とソケットの状態が関連していることを確認できた。

## 6. 学びと考察 (Learning & Insights)

- `ss`を使用することで、サーバーが実際にどのポートで接続を待ち受けているか確認できることを理解した。
- サーバーの起動・停止によるソケットの変化を比較することで、サーバーとソケット、ポートの関係を理解することができた。