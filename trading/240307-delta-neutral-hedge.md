Delta Neutral Hedge
====

1. calculate the total delta
2. check if it needs to hedge
   1. hedge to 0
   2. hedge to threshold
3. cancel all the existing orders which have opposite delta to the total delta to hedge
4. calculate the target price
5. cancel orders of which the price is too aggressive/passive compared with the target price
6. calculate the quantity of the contracts used for hedging
   1. check the lots size limit from the exchange
   2. check the minimum trade size limit from the exchange
   3. check the minimum notional value limit from the exchange
   4. check the order quantity limit from the desk
   
   note that the requirements differ among different exchanges.
7. place the limit order with the target price and target quantity
