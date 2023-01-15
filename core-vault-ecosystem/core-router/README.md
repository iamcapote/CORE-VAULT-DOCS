# CORE Router

<figure><img src="../../.gitbook/assets/image (1) (1).png" alt=""><figcaption></figcaption></figure>

The CORE Router is a set of contracts that allow you to connect your liquidity into the internet of the CORE Ecosystem.

There are four intended functions of the router: **zapper, swapper, wrapper, & flash arbitrage.**

The zapper converts your ETH into LP tokens (which was mainly used for the first version of CORE this also prevented being front run by liquidity bots). This is not the recommended way to obtain LP tokens.&#x20;

The swap system is still in-development and it is intended to be easy swaps between different tokens.

The wrapper allows you to wrap to-and-from ERC-95, so for example wBTC into cBTC and back.

The[ flash arbitrage suite](../../code-overview/flash-arbitrage.md) deserves its own section and is the most complex out of all the router functions.

{% hint style="danger" %}
_Warning! The CORE Router was released for the first version of CORE. Currently it is under active development and it is not recommended to interact with the Router contracts. Stay up to date with dev changes by following the official Twitter & Medium accounts._
{% endhint %}

<figure><img src="../../.gitbook/assets/image (7).png" alt=""><figcaption></figcaption></figure>
