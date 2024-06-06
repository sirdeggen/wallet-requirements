## [ DEPRECATED ] QR Code Example (team had a better idea, will update)

The data merchants encode for recieving payments would be:
```json
{ 
    "id": "E28udaM", 
    "to": "03f6c83eba40d9383205a12c75d9c1c87a931e93d2593264bcb2f7680eb32a1fbd", 
    "amount": 658841 
}
```

![qr](./payment_qr_code.png)

## Output Calculation

The wallet would need to calculate keys based on this QR and their local masterKey:

```javascript
import { PrivateKey, PublicKey, Transaction } from '@bsv/sdk'
import { Token } from '@mystuff/custom'

const qr = { 
    "id": "E28udaM", 
    "to": "03f6c83eba40d9383205a12c75d9c1c87a931e93d2593264bcb2f7680eb32a1fbd", 
    "amount": 658841 
}

const myKey = PrivateKey.fromRandom()
const lockingPKH = PublicKey.fromString(qr.to).deriveChild(myKey, qr.id).toHash()

const lockingScript = new Token().lock(qr.amount, lockingPKH)

const tx = new Transaction(800)

tx.addOutput({
    satoshis: 1,
    lockingScript
})
```
