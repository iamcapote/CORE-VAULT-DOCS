# CORE Flash Arbitrage

<figure><img src="../.gitbook/assets/image (11).png" alt=""><figcaption></figcaption></figure>

Arbitrage refers to the low risk opportunity to profit from price discrepancies of assets in different markets. If one asset is traded in two or more markets anyone can profit if assets are traded at different prices. Furthermore, this process can be automated with trading systems that rely on algortihms that can spot these price discrepancy before the market can react.

Flash swaps allows users to take out a loan from any liquidity pool without any cost with the condition that it is returned at the end of the transaction. If you are unable to meet these conditions the flash swap transaction will be canceled.

The CORE Flash Arbitrage suite combines flash swaps with the low risk opportunity of arbitrage to increase its own liquidity and APY.

Specifically the Flash Arbitrage Suite consists of 3 contracts: Arbitrage Executor, Arbitrage Controller, and the Core Buyer Contract.

* The Arbitrage Executor (arb.exe for short) is a closed source contract that calculates scenarios and executes trades.
* The Arbitrage Controller (arb.ctrl for short) interacts with the arb.exe by calling it, adding strategies and split profits between the Caller and the CORE Buyer Contract.
* The Core Buyer Contract (buyer.ctrl) turns the arbitrage profits into CORE and transfers them directly to farmers.

The arb.exe does not hold any tokens but only swaps them in the most optimal gas efficient way.

The arb.ctrl can bypass the Fee of Transfer of CORE in case of the unlikely scenario that the arbitrage strategy requires it and has two different fee distribution depending on whether or not the FoT is active or not.

Strategies that are outside of the CORE Ecosystem can be used and this in fact these strategies have a higher reward for callers. These contracts are open and available for any to add complex strategies using CORE liquidity to arbitrage opportunities.

The contracts are made to keep specific CORE pools at a designated APY or to add liquidity to CORE and burn the corresponding LP tokens. Due to CORE’s unique ecosystem the Flash Arbitrage Suit directly contribute to the Total Value Permanently Locked (TVPL).



**References:**

Introducing CORE Router v1.0: [Introducing CORE Router v1.0. simplified user experience | by 0xdec4f | Medium](https://0xdec4f.medium.com/introducing-core-router-v1-0-ef4b47c2add6)

Spotlight: Flash Arbitrage: [Spotlight: Flash Arbitrage. Flash Arbitrage is build to bring a… | by 0xdec4f | CORE Vault | Medium](https://medium.com/core-vault/spotlight-flash-arbitrage-d8c1a38a809e)
