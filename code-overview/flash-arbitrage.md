---
description: >-
  The CORE Flash Arbitrage suite is composed of two important principles:
  arbitrage & flash swaps.
---

# Flash Arbitrage

## Flash Arbitrage

CORE’s high level arbitrage suite consists of three contracts:

* **Arbitrage Executor**

The Arbitrage Executor is the brain of our Flash Arbitrage, it calculates scenarios and executes the trades. This contract is closed source.

* **Arbitrage Controller**

The Arbitrage Controller is responsible for adding and storing all strategies. It calls the Arbitrage Executor and then splits the profits between the Caller and the CORE Buyer Contract. It vastly simplifies the interface to the Executor.

* **Core Buyer Contract**

This contract turns the arbitrage profits into CORE and transfers them directly to farmers.

> No input is required to run these contracts, the loan is created by using Uniswaps Flash Swaps. This means only your gas cost is at stake. While the contract isn’t guaranteeing gas returns, you can always query the amount returned.

### **Overview of the Arbitrage Controller functions:** <a href="#overview-of-the-arbitrage-controller-functions" id="overview-of-the-arbitrage-controller-functions"></a>

The creation of a new strategy emits an event which can be listened to and queried by

```
//     event StrategyAdded(string indexed name, uint256 indexed id, address[] pairs, bool feeOff, address indexed originator);

    struct Strategy {
        string strategyName;
        bool[] token0Out; // An array saying if token 0 should be out in this step
        address[] pairs; // Array of pair addresses
        uint256[] feeOnTransfers; //Array of fee on transfers 1% = 10
        bool cBTCSupport; // Should the algorithm check for cBTC and wrap/unwrap it
                          // Note not checking saves gas
        bool feeOff; // Allows for adding CORE strategies - where there is no fee on the executor
    }
```

The Arbitrage Controller is storing each strategy in a public array inside the smart contract. Each strategy has a name and an array of _token0out — ****_ this is to support Uniswap so we know which way to conduct the trade in the Executor

```
address[] pairs; // Array of pair addresses
```

This shows the array of pairs that the trade will be in

```
unit256[] feeOnTransfers; //Array of fee on transfers 1% = 10

```

Strategies support **Fee on Transfer tokens** with fee on transfer array here. It allows people to input that this strategy has a fee on transfer, which signals the contract to calculate and execute the strategy correctly.

```
bool cBTCSupport;  // Should the algorithm check for cBTC and 
                   wrap/unwrap it                                
                   // Note not checking saves gas
```

cBTC support is determined by itself. You can not input it.

```
bool feeOff; // Allows for adding CORE strategies - where there is 
                no fee on the executor
```

This allows for a FoT Off strategy. Adding strategies involving CORE and feeOff is not permitted by anyone. To avoid atypical scenarios with feeOff.

**Revenue split:** The revenue split between the Caller and CORE Buyer Contract is dependant on the strategy utilizing feeOff or feeOn.

<figure><img src="https://cdn-images-1.medium.com/max/800/1*-cJl6Unf19dsWxbgcgejLQ.png" alt=""><figcaption></figcaption></figure>

Strategies that are outside of CORE reward the Caller with the majority of the profits. The ease of use and profit split encourages usage of the service. It is easier to add strategies to this contract and share the profits than having to create your own complex suite of contracts. All strategies, including new creations, are open source and can be used by everyone.

### **Adding New Strategies** <a href="#adding-new-strategies" id="adding-new-strategies"></a>

CORE pairs can not be added by the community. Only by CORE devs. Users can add new strategies as long as they are valid. To create a valid strategy one must input an array of pairs (comma separated list) which has a logically flow from each other.&#x20;

<figure><img src="https://cdn-images-1.medium.com/max/800/1*V-fLGPlrWuLOy5ccUyNXlg.png" alt=""><figcaption></figcaption></figure>

This closes the loop by itself, no need to input CORE/wETH again since the last pair will be treated as a return pair. The controller detects if cBTC is used and sets the support automatically. It makes sure the Executor picks the correct branch to execute and save gas in case support for cBTC is not on.

```
function addNewStrategy(bool borrowToken0, address[] memory pairs) public returns (uint256 strategyID) {

        uint256[] memory feeOnTransfers = new uint256[](pairs.length);
        strategyID = addNewStrategyWithFeeOnTransferTokens(borrowToken0, pairs, feeOnTransfers);

}
```

**addNewStrategy** function is used for adding new strategies with no fee on transfers anywhere.

```
/function addNewStrategyWithFeeOnTransferTokens(bool borrowToken0, address[] memory pairs, uint256[] memory feeOnTransfers) public returns (uint256 strategyID)
```

**addNewStrategyWithFeeOnTransferTokens** supports fee on transfer input, to support fee on transfer tokens all over.

```
function numberOfStrategies() public view returns (uint256) {
        return strategies.length;
```

**numberOfStrategies** function returns the number of strategies (routes of arbitrage) inside the contract which everyone can easily scan in a loop.

```
function strategyProfitInReturnToken(uint256 strategyID) public view returns (uint256 profit) {
        Strategy memory currentStrategy = strategies[strategyID];
        return executor.getStrategyProfitInReturnToken(currentStrategy.pairs, currentStrategy.feeOnTransfers, currentStrategy.token0Out);
    }
```

**Querying Profits from Strategies**. The executor has a view function which returns the queried profit if the strategy was executed at that exact moment. This function should be used to scan all outstanding strategies from the previous step and calling executions immediately.

### **Strategy Execution** <a href="#strategy-execution" id="strategy-execution"></a>

There are two paths that strategy execution can take:

1. Skip calculating Optimal Input
2. Calculate Optimal Input at the time of execution

Number 1 is meant for miners, which know the optimal input without calculating it on chain.

```
function executeStrategy(uint256 strategyPID) public {
        require(!depreciated, "This Contract is depreciated");
        Strategy memory currentStrategy = strategies[strategyPID];

        executor.executeStrategy(currentStrategy.pairs, currentStrategy.feeOnTransfers, currentStrategy.token0Out, currentStrategy.cBTCSupport);


        // Eg. Token 0 was out so profit token is token 1
        address profitToken = currentStrategy.token0Out[0] ? 
            IUniswapV2Pair(currentStrategy.pairs[0]).token1() 
                : 
            IUniswapV2Pair(currentStrategy.pairs[0]).token0();


        uint256 profit = IERC20(profitToken).balanceOf(address(this));

        // We split the profit based on the strategy
        if(currentStrategy.feeOff) {
            safeTransfer(profitToken, msg.sender, profit.mul(revenueSplitFeeOffStrategy).div(1000));
        }
        else {
            safeTransfer(profitToken, msg.sender, profit.mul(revenueSplitFeeOnStrategy).div(1000));
        }

        safeTransfer(profitToken, distributor, IERC20(profitToken).balanceOf(address(this)));

    }
```

Self calculating execution just requires strategy PID. Strategies that do not automatically calculate amount input, meant for miners.

#### **Adding Optimal Input:** <a href="#adding-optimal-input" id="adding-optimal-input"></a>

```
function getOptimalInput(uint256 strategyPID) public view returns (uint256) {
        Strategy memory currentStrategy = strategies[strategyPID];
        return executor.getOptimalInput(currentStrategy.pairs, currentStrategy.feeOnTransfers, currentStrategy.token0Out);
    }
```

A view that returns the optimal input amount, which should be inputted in the execution function below:

```
function executeStrategy(uint256 inputAmount, uint256 strategyPID) public {
```

Both execution strategy functions ensure you get promised profit and non-opaque split numbers.

The Flash Arbitrage executor does not hold any tokens, it only swaps them, picking the best path that will lower the gas used as much as possible as well as not writing any balances in its memory to save additional gas costs.

#### **CORE Buyer Contract** <a href="#core-buyer-contract" id="core-buyer-contract"></a>

This contract receives tokens, either from the **Arbitrage Controller,** or any other source in general and sells them for CORE.If a CORE pair exists for the received token, it will sell it directly to CORE. In case of not having a token pair with CORE, the contract will sell the token for Ethereum and then buy CORE in the CORE/WETH pool immediately. All CORE will be distributed to farmers.

```

function buyAndGiveOutCOREForToken(address _token) public {
        
        // We check if this token is CORE token if it is we just send it out
        if(_token == address(CORE)) {
            // return breaks out no need to else..else..
            return sendCOREToVault();
        }
        uint256 balInputToken =  IERC20(_token).balanceOf(address(this));

        address pairWithCORE = uniswapFactory.getPair(_token, address(CORE));
        // We check if there is a pair for CORE token with that token
        if(pairWithCORE != address(0) && supportedPair[pairWithCORE]){ // we check supported pair so people don't make 1 liquidity pair iwth CORE
            // It mens we have a pair with CORE
            // So we should just swap with it
            //Suport FoT tokens

            uint256 amountOut = swapSupportingFeeOnTransfertokens(_token, pairWithCORE, balInputToken);
            emit COREBought(amountOut);
            return sendCOREToVault();
        }
        
        // This is the case we are not finding a pair with CORE so we try to find one with wETH
        address pairWithWETH = uniswapFactory.getPair(_token, wETH);
        if(pairWithWETH != address(0)) {
            uint256 amountOut = swapSupportingFeeOnTransfertokens(_token, pairWithWETH, balInputToken);
            
            amountOut = swapSupportingFeeOnTransfertokens(wETH, CORExWETHPair, amountOut);
            emit COREBought(amountOut);
            sendCOREToVault();
        }
        else {
            revert("FA COREBuyer : Unsupported token");
        }




    }
```

Anyone can call this function by inserting an address for the token. This will automatically conduct a swap and distribute the token to LP stakers.
