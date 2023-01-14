# ERC-95

<figure><img src="../../.gitbook/assets/image (10).png" alt=""><figcaption></figcaption></figure>

The ERC-20 token standard is considered to be a static token that relies on external price oracles that can be sources of malicious attacks and exploits. That is why CORE ERC-95 instead is a smarter token standard that relies on complex networking effects to secure its ecosystem and continuously increase the security within the system.

The CORE Ecosystem does not use off-chain price oracles but instead relies on its own ability to collect volume information via the Fee of Transfer. The FoT is much more than a redistribution of funds: it is volume precisely recorded in every transaction (on-chain). This in effect makes faking this information incredibly expensive and CORE achieves this level of security without the use or need of any external oracle source.

Another mechanic securing the entire system is the ever-rising price floor which is created due to CORE fundamentals such as permanently locked liquidity, capped supply, and FoT. If the locked liquidity inside the ecosystem increases so does the floor price which also increases the difficulty and price to exploit the system. Every transaction in CORE incurs a fee which slowly and gradually raises the amount of locked liquidity inside its whole ecosystem.

While the CORE token has a limited cap supply of 10k, it has the possibility to mint new kinds of tokens inside the ecosystem which are CORE Liquidity Tokens (LP Tokens). These LP tokens contain the synthetic asset equivalent which converts assets into wrapped CORE assets, for example, wBTC into coreBTC. These wrapped assets are then deposited into liquidity pools along with core-wrapped CORE (cCORE). Synthetic/wrapped assets in CORE can also be wrapped into percentages (25%ETH / 50%BTC/25%DAI for example) which can further increase arbitrage possibilities.

Trading synthetic assets in the CORE ecosystem forces arbitrage from the LP-Token pairs going to the CORE token. This makes CORE act as a price oracle by reporting and keeping track of volume statistics directly on-chain without the use of a price oracle. This information can be used to assess risk between pairs which in turn influences vault yield strategies’ performance and accuracy.

Every time an asset is wrapped with ERC95 a Prime is created which can track volume and collect rich data on price movements. This can be used to create “greeks” for setting up automated options markets that are too expensive to manipulate. The ERC95 standard takes advantage of the movement of other coins by mirroring their price movements and allowing the ecosystem to benefit from the volatility and volume created by the networking effects.



References:

ERC95: A New Standard: [ERC95: A New Standard. Growing CORE’s Ecosystem by Innovation | by 0xdec4f | CORE Vault | Medium](https://medium.com/core-vault/erc95-a-new-standard-e59e806b7d82)

CORE Floor Price and Lending: [CORE: Floor Price and Lending. With coreDEX: Lending releasing later… | by 0xdec4f | CORE Vault | Medium](https://medium.com/core-vault/core-floor-price-and-lending-32c0e1a223c1)

CORE: The Gateway to Limitless Growth: [CORE: The Gateway to Limitless Growth | by 0xdec4f | CORE Vault | Medium](https://medium.com/core-vault/core-the-gateway-to-limitless-growth-1a04112d9892)
