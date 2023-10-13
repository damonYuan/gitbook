Trading Flow
====

1. RFQ
2. RFT
   1. RFT validation, trading params like underlying index
   2. Idempotence check to check if the trade has been executed
   3. Trade Engine state check to see if the trading engine is up and run
   4. Pre-trade check
      1. quota check
      2. last look
   5. trade execution