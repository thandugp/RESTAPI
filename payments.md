## payment

## payment [/payment]
payment endpoint allows you to make a Payment anonymously or by logging in for Britishgas business customers. Responses will be returned in JSON format only.

### payment
payment as a resource represents following information

| Property | Type | As request | As response | Description |
| :-------------------- | :---------- | :-------------------- | :-------------------- | ------------------------------------------------------------ |
| id | String | `Not Required` | `Returned always` | It is the payment document number considered as identifier for payment transaction.|
| card-details | String | `Manadatory` | `Returned always` | It is the card token which has encrypted saved card id in case of save card payment or encrypted card details in case of new card payment. |
| amount | int | `Mandatory` | `Returned always` | It is the amount the customer wish to pay. |
| contract-account | String | `Mandatory` | `Returned always` | It is the account for which the customer is making a payment. |
| email-address | `Optional` | `Returned always` | It is the email address to which email will be sent for payment success or failure for anoymous payment. |
| save-card-indicator | `Optional` | `Returned always` | It is an identifier that as value 'X' in case card details have to be stored. |


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

