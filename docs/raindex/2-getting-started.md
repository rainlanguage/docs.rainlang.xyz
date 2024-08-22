---
sidebar_position: 2
title: Getting started
description: A guide to getting started with Raindex.
---

<div style={{ position: 'relative', paddingBottom: '64.63%', height: 0 }}>
    <iframe
      src="https://www.loom.com/embed/fca750f31f0a43258891cea0ddacb588?sid=60d203be-a4a0-4597-ab18-5ab43fc10516"
      frameborder="0"
      allowFullScreen
      style={{ position: 'absolute', top: 0, left: 0, width: '100%', height: '100%' }}
    ></iframe>
  </div>

### Download the app

First, head to the [download page](./1-download.md) and get a copy of the app.

### Settings

When you open the app for the first time you won't see any orders or vaults. This is because there can be many Raindex orderbook contracts across many chains, so you'll need to set them up.

Head to the settings page (link in the sidebar of the app), paste in the settings below and click 'Apply settings'.

These are example settings for Raindex contracts currently deployed, but new versions are being deployed often so head over to the [Rainlang Telegram](https://t.me/+w4mJbCT6IfI2YTU0) to keep up with updates.

```
networks:
  sepolia:
    rpc: https://rpc.ankr.com/eth_sepolia
    chain-id: 11155111
    currency: ETH
  base:
    rpc: https://base.llamarpc.com
    chain-id: 8453
    currency: ETH
  flare:
    rpc: https://rpc.ankr.com/flare	
    chain-id: 14
    currency: FLR

subgraphs:
  sepolia: https://api.goldsky.com/api/public/project_clv14x04y9kzi01saerx7bxpg/subgraphs/ob4-sepolia/1.2/gn

metaboards:
  sepolia: https://api.goldsky.com/api/public/project_clv14x04y9kzi01saerx7bxpg/subgraphs/mb-sepolia-0x77991674/0.1/gn
  
orderbooks:
  sepolia:
    address: 0x2b44Ead335F6fe5ADCb5B8660C6785B680fd81a9
    network: sepolia
    subgraph: sepolia
  base:
    address: 0x80DE00e3cA96AE0569426A1bb1Ae22CD4181dE6F
    network: base
    subgraph: base
  flare:
    address: 0xaa3b14Af0e29E3854E4148f43321C4410db002bC
    network: flare
    subgraph: flare
  

deployers:
  sepolia:
    address: 0xfD36cc9152595bA216A5E08D088112eaE935FDED
    network: sepolia
  base:
    address: 0x56394785a22b3BE25470a0e03eD9E0a939C47b9b
    network: base
  flare:
    address: 0xEBe394cff4980992B826Ec70ef0a9ec8b5D4C640
    network: flare
```

### Deploying your first order

You should now see orders and vaults across multiple networks on the Orders and Vaults pages.

Try deploying your first order by following one of [the examples](./example-strats/1-examples.md).
