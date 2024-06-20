## Token Rules

1. Transaction must have version set to 800 (Uganda's ISO code)
2. First push of each output should sum to a value exactly equal to the sum of the first push to the locking scripts of each input.
3. If there are more inputs than outputs, a change output of BSV should be added paying the delta number of satoshis back to the service operator.
4. All signatures ought to be sighash all anyonecanpay to allow fees to be covered by the operator.

The operator will add a BSV satoshi input to pay BSV fees (1 satoshi), only after checking that the transaction structure is otherwise as required. It will sign sighash ALL.

### Example Transaction 1
|inputs | outputs |
|---|---|
| 200 UGX 1 sat | 10 UGX 1 sat |
| 400 UGX 1 sat | 590 UGX 1 sat |
| BSV fees 1 sat |  |

### Example Transaction 2
|inputs | outputs |
|---|---|
| 1 UGX 1 sat | 3 UGX 1 sat |
| 2 UGX 1 sat |  |

### Example Transaction 3
|inputs | outputs |
|---|---|
| 500 UGX 1 sat | 700 UGX 1 sat |
| 200 UGX 1 sat | 800 UGX 1 sat |
| 100 UGX 1 sat | BSV change 3 sats |
| 500 UGX 1 sat |  |
| 100 UGX 1 sat |  |
| 100 UGX 1 sat |  |