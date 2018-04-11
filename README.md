## Go Ethereum

以太坊协议的官方golang实现。

[![API Reference](
https://camo.githubusercontent.com/915b7be44ada53c290eb157634330494ebe3e30a/68747470733a2f2f676f646f632e6f72672f6769746875622e636f6d2f676f6c616e672f6764646f3f7374617475732e737667
)](https://godoc.org/github.com/ethereum/go-ethereum)
[![Go Report Card](https://goreportcard.com/badge/github.com/ethereum/go-ethereum)](https://goreportcard.com/report/github.com/ethereum/go-ethereum)
[![Travis](https://travis-ci.org/ethereum/go-ethereum.svg?branch=master)](https://travis-ci.org/ethereum/go-ethereum)
[![Gitter](https://badges.gitter.im/Join%20Chat.svg)](https://gitter.im/ethereum/go-ethereum?utm_source=badge&utm_medium=badge&utm_campaign=pr-badge)

自动构建可用于稳定版本和不稳定的主分支。
二进制档案在https://geth.ethereum.org/downloads/上发布。

## 构建源代码

有关先决条件和详细的构建说明，请阅读[安装说明]（https://github.com/ethereum/go-ethereum/wiki/Building-Ethereum）在wiki上。

构建geth需要Go（1.7或更高版本）和C编译器。
你可以使用你喜欢的包管理器来安装它们
一旦所有的依赖关系都安装完毕之后就运行：

    make geth

或者，构建全套实用程序：

    make all

## 可执行文件

go-ethereum项目带有几个在`cmd`目录中找到的包装器/可执行文件。

| Command    | Description |
|:----------:|-------------|
| **`geth`** | 主要以太坊CLI客户端. 它是进入以太坊网络的切入点 (main-, test- or private net), 能够作为完整节点（full node：默认）存档节点（archive node：保留所有历史状态）或轻型节点（light node ：实时检索数据）运行。它可以被其他进程用作通过HTTP，WebSocket和/或IPC传输顶层暴露的JSON RPC端点进入以太坊网络的网关。用于命令行选项的`geth --help`和[CLI Wiki页面]（https://github.com/ethereum/go-ethereum/wiki/Command-Line-Options）。 |
| `abigen` | 源代码生成器将以太坊合约定义转换为易于使用的编译时类型安全的Go软件包。 如果合约字节码也可用它运行在普通的[Ethereum Contract ABI]（https://github.com/ethereum/wiki/wiki/Ethereum-Contract-ABI）上，具有扩展功能。 但它也接受Solidity源文件，使开发更加简化。 详细请看 [Native DApps](https://github.com/ethereum/go-ethereum/wiki/Native-DApps:-Go-bindings-to-Ethereum-contracts) wiki页面. |
| `bootnode` | 剥离了我们以太坊客户端实现的版本，它只参与网络节点发现协议，但不运行任何更高级别的应用协议。它可以用作轻量级引导节点来帮助找到专用网络中的对等设备。 |
| `evm` | EVM（以太坊虚拟机）开发工具版本，能够在可配置环境和执行模式下运行字节码片段。其目的是允许对EVM操作码进行隔离的，细粒度的调试 (e.g. `evm --code 60ff60ff --debug`). |
| `gethrpctest` |开发工具支持我们[ethereum / rpc-test]（https://github.com/ethereum/rpc-tests）测试套件，该套件验证基线符合[Ethereum JSON RPC]（https://github.com/ ethereum / wiki / wiki / JSON-RPC）规格。有关详细信息，请参阅[测试套件的自述文件]（https://github.com/ethereum/rpc-tests/blob/master/README.md）。 |
| `rlpdump` | 用于将二进制RLP（[递归长度前缀]（https://github.com/ethereum/wiki/wiki/RLP））转储（Ethereum协议使用的数据编码以及网络共识）的开发者实用工具给用户更友好的分层表示(e.g. `rlpdump --hex CE0183FFFFFFC4C304050583616263`). |
| `swarm`    | swarm守护进程和工具。这是swarm网络的入口点。 `swarm --help`命令行选项和子命令。有关群集文档，请参阅https://swarm-guide.readthedocs.io。 |
| `puppeth`    |有助于创建新以太坊网络的CLI向导。 |

## 运行 geth

查看所有可能的命令行标志超出了范围（请参考我们的[CLI Wiki page](https://github.com/ethereum/go-ethereum/wiki/Command-Line-Options)），但我们已经列举了几个常用参数组合，以便您快速了解如何运行自己的Geth实例。

### 在以太坊主网络上的完整节点

到目前为止，最常见的情况是人们希望简单地与以太坊网络互动：创建账户;转移资金;部署和与合同交互。对于这种特殊的使用情况，用户不会关心过去几年的历史数据，所以我们可以快速同步到当前的网络状态。要做到这一点：

```
$ geth console
```

This command will:

 * 以快速同步模式启动geth（默认情况下，可以使用--syncmode标志进行更改），导致它下载更多数据以避免处理以太网网络的整个历史记录，这是非常耗费CPU的。
 * 启动Geth的内置交互式[JavaScript console](https://github.com/ethereum/go-ethereum/wiki/JavaScript-Console)（通过尾部控制台子命令），通过它可以调用所有官方[`web3` methods](https://github.com/ethereum/wiki/wiki/JavaScript-API)方法以及Geth自己的[management APIs](https://github.com/ethereum/go-ethereum/wiki/Management-APIs)。这也是可选的，如果你把它放在外面，你可以使用geth attach附加到已经运行的Geth实例。 

### 以太坊测试网络上的完整节点

向开发人员过渡时，如果您想要创建以太坊合约，几乎可以肯定的是，除非您掌握整个系统，否则不需要真正的资金。换句话说，您不需要连接到主网络，而是希望加入与您的节点相同的测试网络，该节点完全等同于主网络，但只能使用Play-Ether。

```
$ geth --testnet console
```

The `console` subcommand have the exact same meaning as above and they are equally useful on the
testnet too. Please see above for their explanations if you've skipped to here.

Specifying the `--testnet` flag however will reconfigure your Geth instance a bit:

 * Instead of using the default data directory (`~/.ethereum` on Linux for example), Geth will nest
   itself one level deeper into a `testnet` subfolder (`~/.ethereum/testnet` on Linux). Note, on OSX
   and Linux this also means that attaching to a running testnet node requires the use of a custom
   endpoint since `geth attach` will try to attach to a production node endpoint by default. E.g.
   `geth attach <datadir>/testnet/geth.ipc`. Windows users are not affected by this.
 * Instead of connecting the main Ethereum network, the client will connect to the test network,
   which uses different P2P bootnodes, different network IDs and genesis states.
   
*Note: Although there are some internal protective measures to prevent transactions from crossing
over between the main network and test network, you should make sure to always use separate accounts
for play-money and real-money. Unless you manually move accounts, Geth will by default correctly
separate the two networks and will not make any accounts available between them.*

### Rinkeby测试网络上的完整节点

上述测试网络是基于ethash工作证明共识算法的跨客户端网络。因此，由于网络的低难度/安全性，它有一定的额外开销，并且更容易受到重组攻击。 Go Ethereum还支持连接到称为[*Rinkeby*](https://www.rinkeby.io)的权威证明测试网络（由社区成员运营）。这个网络更轻，更安全，但只受到以太坊的支持。

```
$ geth --rinkeby console
```

### Configuration

作为将众多标志传递给geth二进制文件的替代方法，您还可以通过以下方式传递配置文件：

```
$ geth --config /path/to/your_config.toml
```

要想知道文件应该如何看起来像你可以使用dumpconfig子命令来导出你现有的配置：

```
$ geth --your-favourite-flags dumpconfig
```

*注意：这只适用于geth v1.6.0及以上版本。*

#### Docker quick start

在您的机器上启动并运行以太坊的最快方法之一是使用Docker：

```
docker run -d --name ethereum-node -v /Users/alice/ethereum:/root \
           -p 8545:8545 -p 30303:30303 \
           ethereum/client-go
```

这将在快速同步模式下启动，具有1GB的DB内存容量，就像上述命令一样。它还会在您的主目录中创建一个永久卷，以保存您的区块链以及映射默认端口。还有一个阿尔卑斯标签可用于图像的纤细版本。

如果您想从其他容器和/或主机访问RPC，请不要忘记--rpcaddr 0.0.0.0。默认情况下，geth绑定到本地接口，RPC端点无法从外部访问。

### 以编程方式连接Geth节点

作为一名开发人员，不久后你会想通过自己的程序开始与Geth和Ethereum网络进行交互，而不是通过控制台手动进行交互。为此，Geth内置了对基于JSON-RPC的API（([standard APIs](https://github.com/ethereum/wiki/wiki/JSON-RPC)和[Geth specific APIs](https://github.com/ethereum/go-ethereum/wiki/Management-APIs)）的支持。这些可以通过HTTP，WebSockets和IPC（基于unix的平台上的unix套接字，以及Windows上的命名管道）公开。

IPC接口默认启用并公开Geth支持的所有API，而HTTP和WS接口需要手动启用，并且由于安全原因只公开API的子集。这些可以打开/关闭，并按照您的预期进行配置。

基于HTTP的JSON-RPC API选项：

  * `--rpc` 启用HTTP-RPC服务器
  * `--rpcaddr` HTTP-RPC服务器侦听接口 (default: "localhost")
  * `--rpcport` HTTP-RPC服务器侦听端口 (default: 8545)
  * `--rpcapi` API通过HTTP-RPC接口提供 (default: "eth,net,web3")
  * `--rpccorsdomain` 逗号分隔的域名列表，用于接受跨源请求 (浏览器强制执行)
  * `--ws` 启用WS-RPC服务器
  * `--wsaddr` WS-RPC服务器侦听接口 (default: "localhost")
  * `--wsport` WS-RPC服务器侦听端口 (default: 8546)
  * `--wsapi` API通过WS-RPC接口提供 (default: "eth,net,web3")
  * `--wsorigins` 接受websockets请求的起源
  * `--ipcdisable` 禁用IPC-RPC服务器
  * `--ipcapi` API通过IPC-RPC接口提供 (default: "admin,debug,eth,miner,net,personal,shh,txpool,web3")
  * `--ipcpath` 数据区中IPC套接字/管道的文件名（显式路径将其转义）

您需要使用您自己的编程环境的功能（库，工具等）通过HTTP，WS或IPC连接到配置有上述标志的Geth节点，并且您需要在所有传输中说出[JSON-RPC](http://www.jsonrpc.org/specification)。您可以对多个请求重复使用相同的连接！

**注意：请理解在此之前打开基于HTTP / WS的传输所带来的安全隐患！互联网上的黑客正在积极尝试用暴露的API来颠覆以太节点！此外，所有浏览器选项卡都可以访问本地运行的Web服务器，因此恶意网页可能会尝试颠覆本地可用的API！**

### 操作专用网络

维护您自己的专用网络更为重要，因为官方网络中许多理所当然的配置需要手动设置。

#### 定义私人起源状态

首先，您需要创建您的网络的起源状态，所有节点都需要了解并达成一致。这包含一个小的JSON文件（例如，将其称为genesis.json）：

```json
{
  "config": {
        "chainId": 0,
        "homesteadBlock": 0,
        "eip155Block": 0,
        "eip158Block": 0
    },
  "alloc"      : {},
  "coinbase"   : "0x0000000000000000000000000000000000000000",
  "difficulty" : "0x20000",
  "extraData"  : "",
  "gasLimit"   : "0x2fefd8",
  "nonce"      : "0x0000000000000042",
  "mixhash"    : "0x0000000000000000000000000000000000000000000000000000000000000000",
  "parentHash" : "0x0000000000000000000000000000000000000000000000000000000000000000",
  "timestamp"  : "0x00"
}
```

上述字段对于大多数目的应该没问题，但我们建议将随机数更改为某个随机值，以防止未知的远程节点能够连接到您。如果您想预先为某些帐户提供资金以便于测试，则可以使用帐户配置填充alloc字段：

```json
"alloc": {
  "0x0000000000000000000000000000000000000001": {"balance": "111111111"},
  "0x0000000000000000000000000000000000000002": {"balance": "222222222"}
}
```

在上面的JSON文件中定义了Geneis状态之后，您需要在启动每个Geth节点之前初始化它，以确保正确设置所有区块链参数：

```
$ geth init path/to/genesis.json
```

#### 创建会合点

如果所有要运行的节点都初始化为所需的生成状态，则需要启动引导程序节点，其他人可以使用它来在网络中和/或通过互联网找到彼此。干净的方法是配置和运行专用的引导节点：

```
$ bootnode --genkey=boot.key
$ bootnode --nodekey=boot.key
```

在bootnode联机的情况下，它会显示一个[`enode` URL](https://github.com/ethereum/wiki/wiki/enode-url-format)，其他节点可以使用它来连接它并交换对等信息。确保使用外部可访问IP替换显示的IP地址信息（最可能是[::]）以获取实际的`enode` URL。

*注意：您也可以使用完全成熟的Geth节点作为引导节点，但这是不太推荐的方式。*

#### 启动您的成员节点

在bootnode运行并可从外部访问的情况下（您可以尝试telnet <ip> <port>以确保它确实可以访问），请通过--bootnodes标志启动每个后续Geth节点，指向用于对等发现的bootnode。将私有网络的数据目录分开可能也是可取的，所以也要指定一个自定义--datadir 标志。

```
$ geth --datadir=path/to/custom/data/folder --bootnodes=<bootnode-enode-url-from-above>
```

*注意：由于您的网络将与主网络和测试网络完全隔离，因此您还需要配置矿工来处理事务并为您创建新块。*

#### 经营私人矿工

在公共以太坊网络上进行挖掘是一项复杂的任务，因为它只能使用GPU，需要OpenCL或CUDA启用的`ethminer`实例。有关这种设置的信息，请咨询[EtherMining subreddit](https://www.reddit.com/r/EtherMining/)和[Genoil miner](https://github.com/Genoil/cpp-ethereum)资源库。

但是，在专用网络设置中，单个CPU采集器实例对于实际应用来说已经足够了，因为它可以在不需要大量资源的情况下以正确的时间间隔产生稳定的数据块流（考虑在单个线程上运行，不需要多个）。要为挖掘启动Geth实例，请使用所有通常的标志运行它，并通过扩展：

```
$ geth <usual-flags> --mine --minerthreads=1 --etherbase=0x0000000000000000000000000000000000000000
```

这将在单个CPU线程上开始挖掘块和事务，并将所有过程记录到由--etherbase 指定的帐户。您可以通过将默认气体限制块更改为（--targetgaslimit）并在（--gasprice）接受价格交易来进一步调整采矿。

## Contribution

Thank you for considering to help out with the source code! We welcome contributions from
anyone on the internet, and are grateful for even the smallest of fixes!

If you'd like to contribute to go-ethereum, please fork, fix, commit and send a pull request
for the maintainers to review and merge into the main code base. If you wish to submit more
complex changes though, please check up with the core devs first on [our gitter channel](https://gitter.im/ethereum/go-ethereum)
to ensure those changes are in line with the general philosophy of the project and/or get some
early feedback which can make both your efforts much lighter as well as our review and merge
procedures quick and simple.

Please make sure your contributions adhere to our coding guidelines:

 * Code must adhere to the official Go [formatting](https://golang.org/doc/effective_go.html#formatting) guidelines (i.e. uses [gofmt](https://golang.org/cmd/gofmt/)).
 * Code must be documented adhering to the official Go [commentary](https://golang.org/doc/effective_go.html#commentary) guidelines.
 * Pull requests need to be based on and opened against the `master` branch.
 * Commit messages should be prefixed with the package(s) they modify.
   * E.g. "eth, rpc: make trace configs optional"

Please see the [Developers' Guide](https://github.com/ethereum/go-ethereum/wiki/Developers'-Guide)
for more details on configuring your environment, managing project dependencies and testing procedures.

## License

The go-ethereum library (i.e. all code outside of the `cmd` directory) is licensed under the
[GNU Lesser General Public License v3.0](https://www.gnu.org/licenses/lgpl-3.0.en.html), also
included in our repository in the `COPYING.LESSER` file.

The go-ethereum binaries (i.e. all code inside of the `cmd` directory) is licensed under the
[GNU General Public License v3.0](https://www.gnu.org/licenses/gpl-3.0.en.html), also included
in our repository in the `COPYING` file.
