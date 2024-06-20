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

## Transport

Two options for delivery of the payment.

### Online
You can send the transaction to the receiving party wallet server, where it will be validated and broadcast to the network.

The merchant device would recieve a notification from their wallet server which would then reflect that the payment had been recieved successfully.

### Offline
This may be the preference whether or not the user has an internet connection as there will have lower latency.

Using NFC we transmit the payment data directly to the recipient phone. It then validates the transaction before delivering it to its own wallet server, where it will be validated again before broadcast to the network.

If the merchant doesn't have an internet connection, they can still recieve transactions, validating them using SPV, simply broadcast everything when they get back online.

## Storage
The offline state of transactions and their outputs needs to be available to the device and synced when it has an internet connection.

Block headers must also be kept to allow local validation. These can be synced whenever the device has an internet connection.

Received transcations are stored. Merkle paths are attached when recieved. Outputs are stored if ours, deleted on spend. When last output spend is confirmed in a block, the transaction is deleted from local, but kept for records in wallet server.

## Merkle Paths
When callbacks hit from ARC they'll probably come in a set because of how BlockTx works. There's no need to poll. It should be sufficient to request new merklepaths from the wallet server whenever the app is opened on device, and whenever a new payment is being considered prior to the construction of a transaction, to minimize BEEF size.