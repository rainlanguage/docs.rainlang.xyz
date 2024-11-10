# The Order Screen including adding and removing orders
By clicking on an order hash throughout Raindex, you will be taken to the order detail screen.

## Top Section - Order Details

### Browsing Order Details
<img src="/img/raindex/raindex_order_screen_intro.png" />  

* The top left section shows the order hash of the order along with its current status `Active` or `Inactive` 
* Then the orderbook under which the order was deployed, order owner (the wallet that deployed the order), and the timestamp when the order was added
* We can also see the input and output vaults associated with the order; in this particular case, these are `WETH` and `USDC` vaults
* Input vaults hold tokens that are being sold in trading strategies, e.g., sell `USDC` for `WETH`
* Output vaults hold tokens that are being bought in trading strategies, e.g., buy `WETH` with `USDC`
* An order can have multiple input and output vaults associated with it
* In the top right, we can see a trading graph which gives a visual depiction of price once the order starts trading. For example, the following is the vault screen of an actively trading order:

<img src="/img/raindex/raindex_order_top.png" />

## Middle Section - Order Quotes
* Within `Order Quotes`, we can see the order pair and the maximum output amount offered by the order along with its price
* The maximum input amount is the input token amount received for offering the output tokens
* If the order adjusts its price by pinging an on-chain oracle, the quote prices may change from block to block. To view what price is offered at a particular block, we can use the debugger tool by clicking on the debug icon, which will show the elements currently present on the stack for that specific block
* The stack elements may change from block to block, and the quotes are refreshed as new blocks are mined. By clicking on the refresh and pause icon at the top right of the quotes section, we can fetch new blocks or pause at the current one and run the debugger
<img src="/img/raindex/raindex_order_debug_quote.png" /> 
- If we recall from our example of reading the stack elements, the debugger shows the `calculate-io` stack indexed `0` elements which are visible here
   - E.g., `0.3` is the `final-ratio` and `0.2` is the `final-amount` from our Rainlang source

## Bottom Section - Source, Trades, and Volume
* In the bottom section of the order screen, there are tabs for `Rainlang` source, `Trades`, and `Volume` for the order
* The `Rainlang` tab shows the fully composed Rainlang source associated with the order. This is the same as the resultant composition of the production scenario mentioned in the deployment section
* If the Rainlang source isn't what we expected or if there is an incorrect binding associated with the source, we can view it in this tab before adding funds, or use it to troubleshoot if the strategy doesn't perform as expected
<img src="/img/raindex/raindex_order_screen_rainlang.png" /> 
* The `Trades` tab gives an overview of the order trades with details like trade timestamp, trade counterparty, transaction hash, amount of input tokens received, amount of output tokens offered, and the i/o ratio
* We can view historical trades broken into complete history and last 48 and 24 hours by applying the trade filter

<img src="/img/raindex/raindex_order_trades_tab.png" /> 

* The Volumes tab shows the token trade volumes for the vaults associated with the order
* Metrics like net volume and total volume are visible for the same 24-48 hours/all time period
* We can view historical volumes broken into complete history and last 48 and 24 hours by applying the volume filter
* `In Volume` is the total amount of input tokens received by aggregating all the trades under this order
* `Out Volume` is the total amount of output tokens offered for all the trades under the order
* `Net Volume` is the difference between in and out volume for the token
* `Total Volume` is the sum of in and out volume for the token
<img src="/img/raindex/raindex_order_vol_tab.png" /> 

## Removing an Order
* Order owners, and ***`only order owners`*** can remove any active order by simply clicking the `Remove` button on the top right of the top section and confirming the transaction

<img src="/img/raindex/raindex_remove_order_button.png" /> 

Once the order is removed:
* The order detail screen, order lists, vault lists, and vault detail screens will show `Inactive` instead of `Active` 
* The remove button will no longer be visible
* The order will stop executing

<img src="/img/raindex/raindex_order_inactive.png" />
