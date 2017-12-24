### 什么是Cardano SL？
Cardano SL是由IOHK与爱丁堡大学、雅典大学以及康涅狄格大学协作设计开发的加密货币。Cardano SL是基于Haskell实现的，可以看看由Aggelos Kiayias, Alexander Russell, Bernardo David和Roman Oliynykov所写的白皮书 [Ouroboros协议](https://iohk.io/research/papers/#9BKRHCSI)。你可以将Cardano SL想象成比特币重新构想的一种可以自由修复比特币设计缺陷的升级版比特币。请阅读[Cardano SL的与众不同](https://cardanodocs.com/introduction/#what-makes-cardano-sl-special)来获取关于Cardano SL与比特币相似点与不同处的更多信息。

### Cardano 的分层结构
Cardano的设计是分层的，Cardano SL是Cardano平台的第一个组成部分， 也就是第一层。最终，它会拓展到"控制层"，作为可信任的计算框架来评估特殊的证明，保证一定量的计算被正确的执行。在游戏和赌博中，这种系统用来验证随机数产生和游戏结果的真实性。加上侧链，可以完成像游戏中可证明的公平分配奖金的任务。不过控制层的应用远远超出了游戏和赌博。身份管理、信用卡系统等等都会成为Cardano平台的一部分。我们还致力于将Cardano SL的钱包应用Daedalus发展成一个通用的加密货币钱包，具有自动化的加密货币之间的交易以及加密货币与法币之间的交易。

### 如何在nix 模式编译Cardano SL？

* 1.安装 Nix (full instructions at https://nixos.org/nix/download.html)

    curl https://nixos.org/nix/install | sh

* 2.在/etc/nix/nix.conf 下增加索引
```bash
        $ sudo mkdir -p /etc/nix
        $ sudo vi /etc/nix/nix.conf       # ..or any other editor, if you prefer
```
    ..如下两行添加到/etc/nix/nix.conf
```
        binary-caches             = https://cache.nixos.org https://hydra.iohk.io
        binary-caches-public-keys = hydra.iohk.io:f/Ea+s+dFdN+3Y/G+FDgSq+a5NEWhJGzdjvKNGv0/EQ=
```
* 3.进入nix-shell模式
```bash
        $ nix-shell
```
* 4.编译
```
        $ ./scripts/build/cardano-sl.sh
```        
编译好后， 所有编译好的组件会在cardano-sl/.stack-work/install/x86_64-linux-nix（体系架构不同，该目录不同）/lts-9.1/8.0.2/bin/ 目录下

### 如何运行Cardano SL 链接主网？
* 1. 拷贝 可执行文件**cardano-node** 到cardano-sl 目录下
* 2. 生成topology-mainnet文件，配置如下：
```
  wallet:
    relays: [[{ host: relays.cardano-mainnet.iohk.io }]]
    valency: 1
    fallbacks: 7
```
 
 * 3. 运行如下启动命令：
 ``` bash
 ./cardano-node --web --no-ntp --configuration-key mainnet_full --tlscert ./scripts/tls-files/server.crt --tlskey ./scripts/tls-files/server.key --tlsca ./scripts/tls-files/ca.crt --log-config ./scripts/log-templates/log-config-qa.yaml --topology "topology-mainnet" --logs-prefix "state-wallet-mainnet/logs" --db-path "state-wallet-mainnet/db" --wallet-db-path 'state-wallet-mainnet/wallet-db'
 ```
 数据最后都存储在cardano-sl/state-wallet-mainnet
