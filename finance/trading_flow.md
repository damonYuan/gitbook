Trading Flow
====

1. RFQ
2. RFT
   1. Trade request validation, eg., underlying index, option type and channel name
   2. Idempotence check
   3. Engine state check
   4. Pre-trade check, e.g., indicative product check, prepare economics, quota check, sales config check
   5. Quote
   6. Last look, e.g., quote result threshold check
   7. Trade execution
   