# Browsing, searching and filtering orders

## Browsing orders
Now that the order is added to the orderbook, the transaction is confirmed on chain and indexed by the indexer. We can browse all the orders by navigating to the `Orders` tab on the sidebar where we can see all the added orders added to the orderbook.
<img src="/img/raindex/raindex_all_orders.png" />  
Sometimes it may happen that the orderbook has many orders added under the orderbook and it becomes diffcult to find our order, but we can search for our order by using filters.

## Searching orders
* Once we navigate to the `Orders` tab we can see a list of filter available at the top of the screen which we can use to filter out the order that we want to see. 
* You can search for existing orders by entering the order-hash in the search box, an order-hash is a unique id generated for the order at the time of deployment. Order hash for the order can be know by quering the indexer associated with the orderbook.

## Filtering orders
* You can filter by order status, account (owner list), network and order book filter present at the top of the screen.
* Filter by active or inactive orders using the status drop down
<img src="/img/raindex/raindex_order_status_filter.png" />  

* You can also use the accounts filter to filter out orders added by that particular account
<img src="/img/raindex/raindex_account_filter.png" />  

* The `Orderbook` and `network` filters filter out the orders added under the paritcular OrderBook contract for that specific network.
<img src="/img/raindex/raindex_network_ob_filter.png" />  
