## payment

## payment [/payment]
payments endpoint allows you to make a Payment anonymously or by logging in for Britishgas business customers. Responses will be returned in JSON format only.

### payment
payment as a resource represents following information

| Property | Type | As request | As response | Description |
| :-------------------- | :---------- | :-------------------- | :-------------------- | ------------------------------------------------------------ |
| id | String | `Mandatory` | `Returned always` | It is the payment document number considered as identifier for payment transaction.|
| card | String | `Manadatory` | `Returned always` | It is the card identifier used for making payment. |
| postcode | String | 'Optional' | 'Returned always' | It is the post code which is required for making anonymous payment. |
| amount | int | `Mandatory` | `Returned always` | It is the amount the customer wish to pay. |
| contractAccount | String | `Returned always` | It is the account for which the customer is making a payment. |

`Note`: cards endpoint for business should be called to save the card or to store the card in memcache before payments resource is being invoked. This would return card(Id) for further payment trasaction reference which is required for payment resource.

`Note`: When the error code returned from the SAP for payment failure status is either of DCCARD_1 ... DCCARD_16 values error code 500 will be returned from the endpoint.

## POST /payment - logged in payment
	post operation on /payments is used to make a payment for logged in customer.
		
### Headers
	POST /payment
	Authorization: Bearer {accesstoken}
	cid: 05e230bf-a9cf-4b31-bc54-d3a995f62526
			
### Request (application/json)
  ```json
  {
        "payment": {
            "card": "42563574674",                
            "amount": "58.00",
            "contractAccount": "600134652"
          }
      }
```
### Response 200 (application/json) - successful payment

```json
    {
        "status": "SUCCESS",
            "data": {
                "payment": {
			"id": "003400026738",
			"card": "42563574674",
			"amount": "58.00",
			"contractAccount": "600134652"                
                }
            },
        "errors": [],
        "meta": {}
    }
```

## POST /payment - anonymous payment
	post operation on /payments is used to make a payment for anonymous payment.
### Headers
	POST /payment
	cid: 05e230bf-a9cf-4b31-bc54-d3a995f62526
			
### Request (application/json)
  ```json
  {
        "payment": {
		"contractAccount": "600134652",
		"postcode": "LE4 5EX",
		"card": "42563574674",    
		"amount": "58.00"     
          }
      }
```
### Response 200 (application/json) - successful payment

```json
    {
        "status": "SUCCESS",
            "data": {
                "payment": {
		    "id": "003400026738",
                    "contractAccount": "600134652",
		    "postcode": "LE4 5EX",
		    "card": "42563574674",
		    "amount": "58.00"
                }
            },
        "errors": [],
        "meta": {}
    }
```
## POST /payment - SAP payment failure scenarios

## Case 1: SAP error for fraud indicated customer.

### Headers
	POST /payment
	Authorization: Bearer {accesstoken}
	cid: 05e230bf-a9cf-4b31-bc54-d3a995f62526
	    
### Request (application/json)

  ```json
  {
        "payment": {          
		"card": "1434552445",                
		"amount": "58.00",
		"accountNumber": "600134652"
          }
      }
```

### Response 500 (application/json)

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
## Case 2: SAP error for payment difference given without items to be cleared.

### Headers
	POST /payment
	Authorization: Bearer {accesstoken}
	cid: 05e230bf-a9cf-4b31-bc54-d3a995f62526
	    
### Request (application/json)

  ```json
  {
        "payment": {          
		"card": "1434552445",                
		"amount": "58.00",
		"accountNumber": "600134652"
          }
      }
```

### Response 500 (application/json)

```json
    {
        "status": "ERROR",
        "data": {},
            "errors": [
                {
                    "code": "service",
                    "message": "Payment difference given without items to be cleared"
                }
            ],
        "meta": {}
    }
```
