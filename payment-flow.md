# Payment Flow

To request a specific amount the information a merchant shares need only be: their publickey, an reference id, and an amount of tokens.

Where amount is any number from 0 to 2^53 non-inclusive.

```json
{ "id": "<reference_id>", "to": "<recipient_public_key>", "amount": 14389742 }
```

The subsequent payment should be a [BEEF](https://brc.dev/62) transaction, paying to outputs in a specified token format:

```
{amount} drop dup hash160 a7e6725b34e4271c15062d40438675cff6c73915 equalverify checksig
```

```json
{
    "id": "<reference_id>",
    "beef": "<beef_transaction_hex>",
    "from": "<sender_public_key>",
}
```

The number of outputs should be random, between 2 minimum and 18 maximum, to improve privacy.


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

## Transport

Two options for delivery of the payment.

### Online
You can send the transaction to the receiving party wallet server, where it will be validated and broadcast to the network.

### Offline
This may be the preference whether or not the user has an internet connection as there will be less latency.

Using NFC we transmit the payment data directly to the recipient phone. It then validates the transaction before delivering it to its own wallet server, where it will be validated again before broadcast to the network.

## On Device Storage
The offline state of utxos available to the device needs to be tracked such that 