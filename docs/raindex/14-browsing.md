# Browsing, Searching, and Filtering Orders
## Browsing Orders
Now that the order is added to the orderbook, the transaction is confirmed on-chain and indexed by the indexer. We can browse all the orders by navigating to the `Orders` tab on the sidebar where we can see all the orders added to the orderbook.
<img src="/img/raindex/raindex_all_orders.png" />  

Sometimes it may happen that the orderbook has many orders added under it, and it becomes difficult to find our order, but we can search for our order by using filters.

## Searching Orders
* Once we navigate to the `Orders` tab, we can see a list of filters available at the top of the screen which we can use to filter out the orders that we want to see. 
* You can search for existing orders by entering the order hash in the search box. An order hash is a unique ID generated for the order at the time of deployment. The order hash for the order can be known by querying the indexer associated with the orderbook.

## Filtering Orders
* You can filter by order status, account (owner list), network, and orderbook using filters present at the top of the screen.
* Filter by active or inactive orders using the status dropdown
<img src="/img/raindex/raindex_order_status_filter.png" />  
* You can also use the accounts filter to filter out orders added by a particular account
<img src="/img/raindex/raindex_account_filter.png" />  
* The `Orderbook` and `Network` filters filter out the orders added under the particular OrderBook contract for that specific network.
<img src="/img/raindex/raindex_network_ob_filter.png" />
