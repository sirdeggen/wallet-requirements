## NFC Data Example
```json
{ 
    "id": "E28udaM", 
    "merchantPubkey": "03f6c83eba40d9383205a12c75d9c1c87a931e93d2593264bcb2f7680eb32a1fbd", 
    "amount": 658841 
}
```

## QR Data Example
```json
{
    "id": "E28udaM"
}
```
![QR code](./qr_code.png)

## Output Calculation

The wallet would need to calculate keys based on this data and their local masterKey:

```javascript
import { PrivateKey, PublicKey, Transaction } from '@bsv/sdk'
import { Token } from '@mystuff/custom'

// imagine we got this from NFC or QR and a web request
const nfcData = { 
    "id": "E28udaM", 
    "to": "03f6c83eba40d9383205a12c75d9c1c87a931e93d2593264bcb2f7680eb32a1fbd", 
    "amount": 658841
}

const UGANDA_ISO_CODE = 800

const myKey = PrivateKey.fromRandom()
const lockingPKH = PublicKey.fromString(nfcData.merchantPubkey).deriveChild(myKey, nfcData.id).toHash()

const lockingScript = new Token().lock(nfcData.amount, lockingPKH)

const tx = new Transaction(UGANDA_ISO_CODE)

tx.addOutput({
    satoshis: 1,
    lockingScript
})
```
