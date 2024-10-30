---
slug: jade-city-techncal-overview
title: "Jade City Platform & Raindex Technical Overview"
authors: [dcatki]
tags: [jade, platform, builds, strategy, raindex]
---

# Jade City Platform & Raindex Technical Overview

## Revolutionizing the $50B Jade Market with Blockchain Technology

Jade City is leveraging blockchain to democratize access to the $50+ billion global jade market. By integrating real-world assets (RWAs) into a secure, transparent, and decentralized system, Jade City creates a global Jade market.

The MVP is running on Base.

<img width="1076" alt="Screenshot 2024-10-24 at 13 45 54" src="https://github.com/user-attachments/assets/dec59e36-e511-4bd9-acae-cc0478159fda">


### Key Platform Features

1. **Bond Issuance & Claim System**
   * We issue jade-backed bonds that allow investors to pre-order jade. Investors deposit stablecoins like USDC and receive collateral tokens in exchange at predetermined rates. 
   * They can then claim jade tokens as rewards at maturity determined by the bond amount. 
   * Once the bond matures, the investors can claim their stablecoins back in exchange for collateral tokens at the same rate along with the rewards received in form of the jade tokens.
   
2. **Asset-backed Jade Collateral tokens**
   * Real world jade assets are tokenized as semi-fungible tokens(SFTs) which are comptaible with ERC20 standards, wherein each unit of jade collateral is represented on-chain with certified metadata, ensuring transparency and trust.
   * Anybody can simply read and verify the authenticity of the jade tokens on the blockchain by reviewing the on-chain data, ensuring intergirty between the digital and physical assets and make an informed decision to invest.
   * SFT tokens have permissioned roles for operations, oversight and auditing which can be delegated to investors(for minting, depositing and withdrawing), auditors(for producing certifications),retail traders(for trading) etc.
   * The mine operator can deposit jade, mint tokens, and withdraw by burning corresponding receipts, and external auditors can audit the collateral on-chain to ensure transparency and produce their own certifications.
   * Retail traders can simply look at this on-chain data to verify the integrity of the system(the on-chain tokens and real world assets), and trade the tokens on any decentralized exchange providing liquidity for them.

<img width="1076" alt="Screenshot 2024-10-24 at 13 46 12" src="https://github.com/user-attachments/assets/a9423064-ca1b-4305-a487-c50fae494b0c">

3. **Jade-backed Jade Tokens**
   * We offer a jade-backed semi-fungible token(SFT) that is also compatible with ERC20 standards. Each unit of deposited jade is tied to certified metadata, certifying the weight, origin, and certification of the jade boulder by an external auditor, and can be traded in the digital market.
   * Anybody can simply read and verify the authenticity of the jade tokens by reviewing the on-chain data.
   * By ensuring the integrity between the digital and physical assets, we can offer digital assets backed by the real-world value of jade, which is now available to trade on-chain and can be seamlessly integrated into the DeFi ecosystem.

<img width="1076" alt="Screenshot 2024-10-24 at 13 46 21" src="https://github.com/user-attachments/assets/f4fce482-e0bb-4d50-85db-dccdde4d58b2">

### Rainlang and Raindex integration

The Jade platform is built using Rainlang. This allows Jade to automate complex financial agreements, including bond issuance and claim processes, while maintaining high flexibility, efficiency and scalability.

#### Highlights:
* **Rainlang**: The platform utilizes Rainlang, a stack-based, interpreted smart contract language for all on-chain interactions. The bond function is written as a rainlang expression which dictates the on-chain behaviour of the bond issuance and claim process ensuring precision, flexibility and immutability for the bonds in managing jade-backed assets.
* **Raindex integration**: Raindex is a decentralized exchange that allows anyone to write, deploy and manage perpetual token trading strategies, written in rainlang, on any EVM network. The Jade-backed assets are traded on Raindex, ensuring liquidity and transparency in the digital market.
* **Rainlang strategies**: The bond issuance and claim processes are implemented as perpetual token trading strategies, written in rainlang and executed on Raindex. From user buying bonds, to interest calculations, to claim executions based on the interest received by the user at particular tiers and user claim requests, all are written in rainlang which compiles to immutable on-chain bytecode and executed on Raindex.
* **Raindex Vaults**: Raindex vaults are used to store the Jade collateral and the Jade tokens. Raindex vaults are unique to each user, per token, per vault-id providing ease of use and security to the users in managing their assets, ensuring only vault owners can deposit and withdraw.

### Platform Demo

You can explore the platform's functionalities, including bond issuance, asset-backed tokens, and jade-backed stablecoins, at [Jade Frontend](https://jade-frontend.pages.dev/).
