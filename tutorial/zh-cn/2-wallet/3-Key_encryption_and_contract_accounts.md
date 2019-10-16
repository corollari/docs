# 加密私钥与合约账户

## 私钥加密（NEP-6文件）

将原始的私钥存储在磁盘上会有安全隐患。任何可以访问原始私钥的人都可以使用这些资金。对这些密钥进行加密会是一种更为安全的做法。出于这个原因，我们提供了NEP-2标准格式（https://github.com/neo-project/proposals/blob/master/nep-2.mediawiki）

该加密密钥为原始私钥提供了一层额外的安全保障，攻击者要想获得这笔资金就需要同时获取加密密钥以及密码。这个做法是不错，但往往用户会需要多个钱包，这意味着他们会有多个密钥。对每个NEP-2加密密钥都加以存储是非常麻烦的，因此我们可以创建一种文件结构来一并存储这些加密的密钥。

该标准提供了一种将钱包导入各种区块链客户端的标准化方法，并提供了对NEP-2格式额外的安全性保证。

可以在此处查看有关文件格式的完整规范说明（https://github.com/neo-project/proposals/blob/master/nep-6.mediawiki） 。 文件遵循JSON结构，其中包含有关私钥/公钥对的信息以及每个账户的元数据信息。元数据信息包含：应将哪个钱包设置为默认钱包、加密参数以及其他相关的元数据。

NEP-6文件还支持watch-only模式的地址。Watch-only地址不包含与私钥相关的任何信息，如果账户是单独存储在一个更为安全的位置的话，这个功能可能会很有用。

## 合约账户
NEO还支持更加复杂的账户类型。 在这些类型中，资金不与单个用户相关联，而是存储在智能合约中。 合约会定义一些特殊规则，用于说明从账户中提取资金所需的条件。

这种账户类型最常见的是多方签名账户。 多方签名账户要求X个人中有N个人为交易提供签名从而才能执行转账操作。 例如，3个账户所有者中必须要有2个对交易进行签名从而才能提取资金。

我们可以使用NEO的操作码为账户生成一个简单的合约。假设我们想为3个人（公钥）创建一个多方签名合约账户：

**需要注意的是，我们需要在操作之前按ECPoint（X，Y）对公钥进行升序排序，否则我们将得到一个不同的脚本哈希，从而导致生成不同的NEO地址。**

```
//公钥1
036245f426b4522e8a2901be6ccc1f71e37dc376726cc6665d80c5997e240568fb

//公钥2
0303897394935bb5418b1c1c4cf35513e276c6bd313ddd1330f113ec3dc34fbd0d

//公钥3
02e2baf21e36df2007189d05b9e682f4192a101dcdf07eed7d6313625a930874b4
```

我们希望至少有2个人对交易进行签名。 因此，我们必须创建一个自定义脚本。 脚本内容如下：

```
// 最少签名数(2)
PUSH OPCODE 52

// 附上所有的公钥
PUSH PUBKEY 1
PUSH PUBKEY 2
PUSH PUBKEY 3

//公钥总数 (3)
PUSH OPCODE 53

//检验多签
PUSH OPCODE AE
```

从而得到了如下的脚本

```
5221036245f426b4522e8a2901be6ccc1f71e37dc376726cc6665d80c5997
e240568fb210303897394935bb5418b1c1c4cf35513e276c6bd313ddd1330
f113ec3dc34fbd0d2102e2baf21e36df2007189d05b9e682f4192a101dcdf
07eed7d6313625a930874b453ae
```

然后，我们使用前面说过的方法计算该账户的脚本哈希和地址。

计算脚本哈希（和地址）：

4d0c0932fa032debdceaaf5cd8086cf3f882961f / AJetuB7TxUkSmRNjot1G7FL5dDpNHE6QLZ

*多方签名示例由NEOResearch提供*

这个合约信息也可以存储在NEP-6文件中，从而允许用户跟踪那些不一定与单个私钥相关联的账户。 可以使用NEO的脚本功能来创建更加复杂的账户类型。

NEO-GUI钱包目前支持多方签名。

### NEO DB3
在引入NEP-6文件格式之前，NEO db3是NEO GUI所支持的一种遗留的文件格式。强烈建议升级到NEP-6文件格式，这可以在 [NEO-GUI](../../../docs/zh-cn/node/gui/wallet.md) 中完成。

## 阅读下节

[UTXO 与账户模型](4-UTXO_and_account_models.md)
