---
description: cLend is a lending module that adds lending against coreDAO and CORE tokens.
---

# cLEND

<figure><img src="../.gitbook/assets/image (11).png" alt=""><figcaption></figcaption></figure>

[CORE](core/) and [coreDAO](coredao.md) can be used as collateral in cLend with a value of 1 DAI per coreDAO and 5,500 DAI for CORE.

Contrary to other products there is **no liquidation risk from price movement of the underlying collateral**. In other words, price fluctuations in the tokens will not cause liquidations. The liquidations are simply caused by interest. Currently there is a **20% yearly interest rate** and a **liquidation threshold value of 110%** on your loans. This means that in order to avoid liquidating your loan position, you need to repay the interest before the loan accrues the amount in the liquidation threshold value.

Loans are possible thanks to locked liquidity, the low circulating supply of CORE and the price floor that it creates for the tokens.

You can find the website for cLend over at [beta.corefinance.eth](https://beta.corefinance.eth/)

<figure><img src="../.gitbook/assets/2 (2).png" alt=""><figcaption></figcaption></figure>



**References**

* [cLend Launch](https://medium.com/core-vault/coredao-and-clend-launch-c7da261ec25)
* [CORE: Floor Price and Lending](https://medium.com/core-vault/core-floor-price-and-lending-32c0e1a223c1)
