---
layout: rsk
title: Conversion with node and console
tags: rsk, rbtc, conversion, peg, 2-way, peg-in, peg-out, federation, node, cli
description: "How to perform the 2-way peg mechanism using node and console"
collection_order: 3140
---

- [General requirements](#general-requirements)
- [Lock: From BTC to R-BTC](#from-btc-to-r-btc)
- [Release: From R-BTC to BTC](#from-r-btc-to-btc)

## General Requirements

* Clone [this project](https://github.com/rsksmart/utilities/tree/master/peg/hw/ledger).
* You need a [Ledger](https://www.ledger.com/) with Bitcoin and RSK Apps installed. We recommend you to have [Ledger Live](https://www.ledger.com/pages/ledger-live) and review this tutorial [Use the Manager](https://support.ledgerwallet.com/hc/en-us/articles/360006523674-Use-the-Manager).
* You need to have [Electrum](https://electrum.org/). Install it and [configure it to be used with Ledger](https://support.ledgerwallet.com/hc/en-us/articles/115005161925-Set-up-and-use-Electrum).
* Set the correct [RSK public node](/rsk/public-nodes) in the `config.json` file depending on the network you are going to use.
* Use Ledger firmware less than `1.5.5`

## From BTC to R-BTC

How to perform a peg-out.

:exclamation: **Before start:** read [lock requirements](/rsk/rbtc/conversion/#1-btc-to-r-btc-conversion)
1. Set a derivation path in the key `derivationPath` in `config.json` file. You should use BTC mainnet/testnet derivation path `44'/0'/0'/0/0`.
2. Unlock Ledger and open the **Bitcoin App**.

3. In your terminal exec ```node ./utils.js```. After that, you'll get the *BTC address derived* from the derivation path that you've specified, and also the *RSK Federation Address*. Here is a response example:
```
Using web3 https://public-node.testnet.rsk.co
Federation Address: 2M...4B
BTC Address
Derivation Path: 44'/1'/0'/0/0
{
  publicKey: '0404e3afdccf84c4b80e3eaab5664fc9814e283f9a16da32fca99d099df4857bc4baad8a78bf5aa60d14e5f6ad8650bede1c2347aceb4a2efe6afb461047f2bfb0',
  bitcoinAddress: 'mow8mDqiUCf17DxE9uV97SvDvzfYjrBzEG',
  chainCode: '491bf74110805f8f118f92d66abc92cbee5ce57bd24ab426bf00dd6b913f5348'
}
```
4. Send some funds to the address `bitcoinAddress` obtained.

5. You need to whitelist the `bitcoinAddress` in the RSK network [(what's to be whitelisted?)](/rsk/rbtc/conversion/whitelist)
6. Use Electrum to send some BTC to the *RSK Federation Address* (you got it in step 2) from `bitcoinAddress` obtained. To do that:
    - Open Electrum
    - Go to Addresses Tab
    - Find the whitelisted address (your `bitcoinAddress`)
    - Right click over it
    - Select the option "Spend From":
![Spend from](/assets/img/rsk/peg-ledger/electrumSpendFromOption.png)
    - Finally make a payment to the RSK Federation Address
*The minimum amount to send is 0.01 BTC*
![Sending Payment](/assets/img/rsk/peg-ledger/electrumSpendFrom.png)

7. Wait the stipulated time.

8. Then use our [Testnet Explorer](https://explorer.testnet.rsk.co) or [Mainnet Explorer](https://explorer.rsk.co) to see your R-BTC balance.

## From R-BTC to BTC

How to perform a peg-in.

:exclamation: **Before start:** read [release requirements](/rsk/rbtc/conversion/#2-r-btc-to-btc-conversion).
1. Set a derivation path in the key `derivationPath` in `./config.json` file. You should use BTC mainnet/testnet derivation path `44'/0'/0'/0/0`.

2. Set the correct `chainId` in the `config.json` file. It depends on the network you are going to use (See [EIP-155](https://github.com/ethereum/EIPs/blob/master/EIPS/eip-155.md#list-of-chain-ids)).

3. Choose the value to send and set it in hexa in `valueToSend` in `config.json` file.
*The minimum amount to send is 0.005 R-BTC for Testnet and 0.008 R-BTC for Mainnet*

4. Unlock Ledger and open **RSK App**.

5. Exec `node ./to-btc.js`. Here is an output example:
    ```
    Using web3 https://public-node.testnet.rsk.co
    Sender address:  0x4820a5D22c3Ba3DA1606756DC85b14cdEcf098d0
    Account 0x0000000000000000000000000000000001000006
    Gas 0xABE0
    Gasprice 0x1
    Nonce 0x1
    { nonce: '0x1',
      gasPrice: '0x1',
      gas: '0xABE0',
      value: '0x16345785D8A0000',
      to: '0x0000000000000000000000000000000001000006',
      v: 31,
      r: 0,
      s: 0,
      data: '' }
    Raw tx:  f867010182abe094000000000000000000000000000000000100000688016345785d8a00008061a056c169b8a889e4b1352b89808d1315e7bb23b1dbec81299d076b4a6879bd0b45a005a9979c7684e49c9d6b0fe5f40289910606b4cee09a3431ed85ce77fb223fd1
    0x570f61b0f9e8a3d8f044b059e9c0386778df2ed1a74f18cea46d2d02dcb3b77a
    ```

6. Wait the stipulated time for your funds to arrive to your BTC address. This address is the same that you generated in the previous section.
