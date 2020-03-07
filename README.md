# aeth
Decentralized Application Platform with proof-of-capacity(POC) consensus.

---

## Start

Wallet Console 

`./aeth-wallet`

Start FullNode P2P Syncing Server

`./aleth --listen 33331 --db-path data`

---

## Default Options

---

Windows Default Blockchain Data Path：

`%APPDATA%\Ethereum`

Linux Default Blockchain Data Path：

`~/.ethereum`

Windows Default Keystore Path：

`%APPDATA%\Roaming\Web3\Keys`

Linux Default Keystore Path：

`~/.web3/Keys`

---

## IPC and HTTP JSON-RPC Server

---

Defualt IPC-RPC Pipe Name:

`Linux ./data/aeth.ipc`
`Win \\.\pipe\data\geth.ipc`

Default HTTP-JSON-RPC Server Port

`33332`