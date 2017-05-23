## payment

## payment [/payment]
payment endpoint allows you to make a Payment anonymously or by logging in for Britishgas business customers. Responses will be returned in JSON format only.

### payment
payment as a resource represents following information

| Property | Type | As request | As response | Description |
| :-------------------- | :---------- | :-------------------- | :-------------------- | ------------------------------------------------------------ |
| id | String | `Not Required` | `Returned always` | It is the payment document number considered as identifier for payment transaction.|
| card | String | `Manadatory` | `Returned always` | It is the card identifier which is either saved in SAP or saved in memcache in case of payment using saved card or payment using new card details respectively. |
| amount | int | `Mandatory` | `Returned always` | It is the amount the customer wish to pay. |
| contract-account | String | `Returned always` | It is the account for which the customer is making a payment. |

`Note`: cards endpoint[/cards] for business should be called to save the card or to store the card in memcache before payments resource is being invoked. This would return card(Id) for further payment transaction reference which is required for payment resource.

`Note`: When the error code returned from the SAP for payment failure status is either of DCCARD_1 ... DCCARD_16 values error code 500 will be returned from the endpoint.

## POST /payment - logged in payment
	post operation on /payments is used to make a payment for logged in customer.
		
### Headers
	POST /payment
	Authorization: Bearer {accesstoken}
	cid: 05e230bf-a9cf-4b31-bc54-d3a995f62526
	Content-Type: application/vnd.api+json
	Accept: application/vnd.api+json
			
### Request 

  ```json
  {
	"jsonapi": {
	"version": "1.0"
	},
	"payment": {
		"card": "42563574674",                
		"amount": "58.00",
		"contractAccount": "600134652"
	}
      }
```
### Response 200 (application/vnd.api+json) - successful payment

```json
    {
        "jsonapi": {
            "version": "1.0"
        },
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
	Content-Type: application/vnd.api+json
	Accept: application/vnd.api+json
			
### Request
	
  ```json
  {
        "jsonapi": {
            "version": "1.0"
        },
        "payment": {
		"contractAccount": "600134652",
		"card": "42563574674",    
		"amount": "58.00"     
          }
      }
```
### Response 200 (application/vnd.api+json) - successful payment

```json
    {
        "jsonapi": {
            "version": "1.0"
        },
        "status": "SUCCESS",
            "data": {
                "payment": {
		    "id": "003400026738",
                    "contractAccount": "600134652",
		    "card": "42563574674",
		    "amount": "58.00"
                }
            },
        "errors": [],
        "meta": {}
    }
```
## POST /payment - payment failure scenarios

## Case 1: SAP error for fraud indicated customer.

### Headers
	POST /payment
	Authorization: Bearer {accesstoken}
	cid: 05e230bf-a9cf-4b31-bc54-d3a995f62526
	Content-Type: application/vnd.api+json
	Accept: application/vnd.api+json
	    
### Request 

  ```json
  {
        "jsonapi": {
            "version": "1.0"
        },
        "payment": {          
		"card": "1434552445",                
		"amount": "58.00",
		"contractAccount": "600134652"		
          }
      }
```

### Response 500 (application/vnd.api+json)

```json
    {
        "jsonapi": {
            "version": "1.0"
        },
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
	Content-Type: application/vnd.api+json
	Accept: application/vnd.api+json
	    
### Request 

  ```json
  {
        "jsonapi": {
            "version": "1.0"
        },
        "payment": {          
		"card": "1434552445",                
		"amount": "58.00",
		"contractAccount": "600134652"
          }
      }
```

### Response 500 (application/vnd.api+json)

```json
    {
        "jsonapi": {
            "version": "1.0"
        },
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

