## BusinessPayment

## Payment collection [/BusinessPayments]
Payments endpoint allows you to make a Payment anonymously or by logging in. Responses will be returned in JSON format only.

### BusinessPayment
Payment as a resource represents following information

| Property | Type | As request | As response | Description |
| :-------------------- | :---------- | :-------------------- | :-------------------- | ------------------------------------------------------------ |
| id | String | `Mandatory` | `Returned always` | It is the Identifier which is required to track the payment for that particular account.|
| cardId | String | `Manadatory` | `Returned always` | It is the Identifier for the card that is saved in memcache. |
| cards | BusinessCards |`Optional` | `Returned always` | It is the BusinessCards endpoint which is cinvoked before payment resource to save the save or store in memcache to make the payment. [BusinessPayments/BusninessCards]. |
| amount | int | `Mandatory` | `Returned always` | It is the amount the customer wish to pay |
| accountNumber | String | `Returned always` | It is the account for which the customer is making a payment. |
| paymentDocumentNumber | String | `Returned always` | It is the transaction reference number returned from SAP for the payment made. |

'Note': BusinessCards resource endpoint should be called to save the card or to store the card in memcahe. This would return cardId for payment trasaction reference.

`Note`: When the error code returned from the SAP for payment failure status is either of DCCARD_1 ... DCCARD_16 values error code 500 will be returned from the endpoint.

## Case 1: Logged In successful payment

### Parameters
	id
		
### Headers
	POST/payments/{id}
	Authorization: Bearer {accesstoken}
	cid: 05e230bf-a9cf-4b31-bc54-d3a995f62526
			
### Request (application/json)
  ```json
  {
        "Payment": {
            "id": "43566453",
            "cardId": "42563574674",
                "cards": {
                    "id": "7647885",
                    "holderName": "test",
                    "cardNo": "xxxxxxxxxxx0084",
                    "cvv": "123",
                    "cardType": "0001",
                    "cardExpiryMonth": "08",
                    "cardExpiryYear": "17",
                    "saveCardId": "42563574674",
                    "priorityCardIndicator": "X"
                },
            "amount": "58.00",
            "accountNumber": "600134652",
            "paymentDocumentNumber": "47875899"
          }
      }
```
### Response 200 (application/json) - successful payment

```json
    {
        "status": "SUCCESS",
            "data": {
                "Payment": {
                    "accountNumber": "600134652",
                    "paymentDocumentNumber": "003400026738"
                }
            },
        "errors": [],
        "meta": {}
    }
```

## Case 2: Logged In payment - Error from SAP for payment failure scenario

### Headers
	POST/payments/{id}
	Authorization: Bearer {accesstoken}
	cid: 05e230bf-a9cf-4b31-bc54-d3a995f62526
	    
### Request (application/json)

  ```json
  {
        "Payment": {
            "id": "43566453",
            "cardId": "42563574674",
                "cards": {
                    "id": "7647885",
                    "holderName": "test",
                    "cardNo": "xxxxxxxxxxx0084",
                    "cvv": "123",
                    "cardType": "0001",
                    "cardExpiryMonth": "08",
                    "cardExpiryYear": "17",
                    "saveCardId": "42563574674",
                    "priorityCardIndicator": "X"
                },
            "amount": "58.00",
            "accountNumber": "600134652",
            "paymentDocumentNumber": "47875899"
          }
      }
```

### Response 500 (application/json) - payment failure in SAP

```json
    {
        "status": "ERROR",
        "data": {},
            "errors": [
                {
                    "code": "service",
                    "message": "error.fraud.indicator.marked.for.this.account"
                }
            ],
        "meta": {}
    }
```
