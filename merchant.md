# Merchant UX

## Smartphones

### Scenario 1: Both merchant and customer have NFC phones
```mermaid
sequenceDiagram
    participant Merchant
    participant Customer

    note over Merchant: Tap [Receive]
    note over Merchant: Enter Amount
    note over Merchant: Tap [Confirm]
    note over Merchant: Display QR and NFC prompt
    note over Customer: Tap [Pay]
    note over Customer,Merchant: Bring Phones together
    Merchant->>Customer: NFC: { paymentId, amount, merchantPubkey, merchantName }
    alt Amount is over threshold
        note over Merchant: Display Awaiting Payment
        note over Customer: Display Merchant & Amount
        note over Customer: Tap [Approve] or [Reject]
    end
    note over Customer,Merchant: Bring Phones together
    Customer->>Merchant: NFC: { paymentId, beef, customerPubkey, customerName, beef }
    alt Success
        Merchant->>Customer: NFC: { paymentId, txid }
        note over Merchant: Display Accepted
        note over Customer: Display Success
    else Failure
        note over Merchant: Display Rejected
        note over Customer: Display Failure
    end
```
<!-- 
### Scenario 2: Neither have NFC
```mermaid
sequenceDiagram
    participant Merchant
    participant Customer
    participant WalletServer

    note over Merchant: Press "Receive" button
    note over Merchant: Enter total amount
    note over Merchant: Press "Confirm"
    note over Merchant: Display QR code prompt "SCAN"
    note over Customer: Press "Pay" button
    note over Customer: Open QR code scanner
    note over Customer: Scan QR code
    note over Customer: Display amount to be paid
    note over Customer: Search for public key in previous contacts
    alt Public key not found
        Customer->>WalletServer: Request data for public key
        WalletServer->>Customer: Send data for public key
    end
    note over Customer: Display recipient data
    alt Amount is low enough
        note over Customer: Single tap to confirm
    else
        note over Customer: Display confirmation prompt
        note over Customer: Approve or reject transfer
    end
    alt Successful payment
        note over Customer: Display acknowledgement until dismissed
    else
        note over Customer: Try Online Payment
    end
```

### Scenario 3: Customer doesn't have a camera on their phone
```mermaid
sequenceDiagram
    participant Merchant
    participant Customer
    participant WalletServer

    note over Merchant: Press "Receive" button
    note over Merchant: Enter total amount
    note over Merchant: Press "Confirm"
    note over Merchant: Display QR code and NFC prompt "SCAN OR TAP"
    note over Customer: Press "Pay" button
    alt NFC available
        note over Customer: Initiate NFC scan mode
        note over Customer: Scan NFC
        note over Customer: Display amount to be paid
        note over Customer: Search for public key in previous contacts
        alt Public key not found
            Customer->>WalletServer: Request data for public key
            WalletServer->>Customer: Send data for public key
        end
        note over Customer: Display recipient data
        alt Amount is low enough
            note over Customer: Single tap to confirm
        else
            note over Customer: Display confirmation prompt
            note over Customer: Approve or reject transfer
            alt Approved
                Customer->>Merchant: Wave phone near merchant's to complete payment
            end
        end
        alt Successful payment
            note over Customer: Display acknowledgement until dismissed
        else
            note over Customer: Attempt "Dumbphones" payment flow
        end
    else
        note over Customer: Attempt "Dumbphones" payment flow
    end
```


1. The merchant presses the "Receive" button, and is prompted to enter a total amount.
2. They press confirm whereby a QR code is displayed encoding their public key, a payment id, and an amount, and this information is also available through NFC, with an on screen prompt to allow a customer to bring their paying phone close. "SCAN OR TAP".
3. The customer hits a "pay" button which initiates NFC scan mode if available if not then opens QR code scanner.
4. Either scan mode populates their screen with information about the amount they owe. Their wallet searches their previous contacts for the public key, if not found a request is made to the wallet server to retrieve the data associated with the public key. That data is then displayed on screen indicating the repient of the funds.
5. If the amount to transfer is low enough, a single tap is all that is required. If the amount is over that threshold then a confirmation is displayed.
6. `OPTIONAL` The customer either rejects or approves the transfer, having checked the amount on screen. If approved is instructed to wave their phone near the merchant's a second time to complete the payment.
7. If successful an acknowledgement is displayed until dismissed.
8. If it fails they should attempt to use the "Dumbphones" payment flow instead.

## Dumbphones

1. We need service discovery, so merchant should give them a merchant id which is equivalent to a paymail.
2. Customer types that in, and sets the amount to whatever the merchant is expecting then merchant will see payment arrive in their phone assuming they're connected to the internet. -->
