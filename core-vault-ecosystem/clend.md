---
description: cLend is a lending module that adds lending against coreDAO and CORE tokens.
---

# cLEND

<figure><img src="../.gitbook/assets/2 (2).png" alt=""><figcaption></figcaption></figure>

Both tokens can be used as collateral in cLend with a value of 1 DAI per coreDAO and 5,500 DAI for CORE.

Contrary to other products there is **no liquidation risk from price movement of the underlying collateral**. In other words, price fluctuations in the tokens will not cause liquidations. The liquidations are caused by interest. Currently there is a **20% yearly interest rate** and a **liquidation threshold of 110%** on your loans. This means that in order to avoid liquidating your loan position, you need to repay the interest before the loan accrues the amount in the liquidation threshold.

Loans are possible thanks to locked liquidity and the price floor that it creates for the tokens. Transferring the liquidity from the three initial LP pools into coreDAO caused a lot of CORE to be put out of circulation and brought the **total circulating supply closer to 1602 tokens**. This in turn caused around 50,000,000 DAI to be left in the pool, which formed our Total Value Permanently Locked (TVPL). This means that coreDAO is backed by 4 DAI in the cLend pool of which only 1 DAI is available for loans. These values in DAI do not represent the market value of each asset but tell us what value contributes to the floor price of CORE and the amount available to be lent out.



You can find the website for cLend over at beta.corefinance.eth



**References**

* [cLend Launch 4](https://medium.com/core-vault/coredao-and-clend-launch-c7da261ec25)
* [CORE: Floor Price and Lending 2](https://medium.com/core-vault/core-floor-price-and-lending-32c0e1a223c1)
