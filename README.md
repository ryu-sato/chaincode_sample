![Build chaincode](https://github.com/ryu-sato/hyperledger_fabric_chaincode_sample/workflows/Build%20chaincode/badge.svg)

ryu-sato/hyperledger_fabric_chaincode_sample は Hyperlegder fabric の chaincode サンプルです。

# Hyperledger fabric を開発モードで実行する

```bash
cd misc
docker-compose up -d
```

# chaincode をインスタンス化する

1. "Hyperledger fabric を開発モードで実行する" を参考にして fabric network を起動する
2. ターミナル 2 つを用意して、下のコマンドを実行する
```bash
# ターミナル1 (c は Docker container のプロンプトを意味する)
## chaincode を変更する度に実行しなおす
$ docker exec -it chaincode bash
(c)$ go build -o hello
(c)$ CORE_PEER_ADDRESS=peer:7052 CORE_CHAINCODE_ID_NAME=hello:0 ./hello

# ターミナル2 (c は Docker container のプロンプトを意味する)
$ docker exec -it cli bash
(c)$ go get  # 依存関係の解決
(c)$ peer chaincode install -p github.com/ryu-sato/hyperledger_fabric_chaincode_sample/ -n hello -v 0
(c)$ peer chaincode instantiate -n hello -v 0 -c '{"Args":[""]}' -C myc
```

# transaction を実行する

1. "chaincode をインスタンス化する" を参考にして、chaincode をインスタンス化する
2. `cli` container から hello chaincode の transaction を実行する
```bash
$ docker exec -it cli bash
(c)$ peer chaincode invoke -n hello -c '{"Args":["helloChaincode"]}' -C myc
```
