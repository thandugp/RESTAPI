## Cards

## Cards [/cards]
Cards endpoint allows you to add, edit, delete, retrieve cards for business. Responses will be returned in JSON format only.

### Cards
cards for business as a resource represents following information

| Property | Type | As request | As response | Description |
| :-------------------- | :---------- | :-------------------- | :-------------------- | ------------------------------------------------------------ |
| id | String | `Optional` | `Returned always` | It is the save card Identifier for the particular card that is either being added, deleted or edited in SAP. |
| holderName | String | `Manadatory` | `Returned always` | It is the card holder name. |
| cardNo | String |`Optional` | `Returned always` | It represents the card number. |
| cvv | String | `Mandatory` | `Returned always` | It represents the secured card number. |
| cardExpiryMonth | String | `Returned always` | It is the card expiry month. |
| crdExpiryYear | String | `Returned always` | It is the card expiry year. |
| cardType | String | `Returned always` | It represents the card type.|
| priorityCardIndicator | String | `Returned always` | It is a flag of the card that identifies whether a priority card or not for the customer.|

`Note:` This card endpoint should be hit before payment response is being invoked for making a payment.


## Retrieve cards [GET /cards] - Card Details found

### Header
	GET /cards
	Authorization: Bearer {accesstoken}
	cid: 05e230bf-a9cf-4b31-bc54-d3a995f62526
	
### Response 200 (application/json)

```json
	{
		"status": "SUCCESS",
			"data": {
				"cards": [
					{
					"id": "00505683065B1EE78EF34A3F0BD0AF4C",
					"holderName": "test",
					"cardType": "001",
					"cardNumber": "xxxxxxxxxxxx8695",
					"expiryMonth": "09",
					"expiryYear": "18",
					"cvv": "123",
					"priorityCard": "Y"
					}
				]
			},
		"errors": {}
	}
```
## Retrieve cards [GET /cards] - No cards available

### Response 200 (application/json)
```json
	{
		"status": "SUCCESS",
			"data": {
				"cards": []
			},
		"errors": {}
	}
```

## Retrieve specific card detail [GET /cards/{id}]

### Parameters

    id: 00505683065B1EE78EF34A3F0BD0AF4C (string) - SaveCardId

### Response 200 (application/json)
```json
	{
		"status": "SUCCESS",
			"data": {
					"card": {
						"id": "00505683065B1EE78EF34A3F0BD0AF4C",
						"holderName": "test",
						"cardType": "001",
						"cardNumber": "xxxxxxxxxxxx8695",
						"expiryMonth": "09",
						"expiryYear": "17",						
						"cvv": "123",
						"priorityCard": "Y"
					}
			},
		"errors": {}
	}
```			
## Save card Detail [POST /cards] - Card added successfully
### Request

```json
	{
	"card": {
		"holderName": "test",
		"cardType": "001",
		"cardNumber": "xxxxxxxxxxxx8695",
		"expiryMonth": "09",
		"expiryYear": "17",
		"cvv": "123",
		"priorityCard": "Y"
	},
	"meta": {}
	}
```
### Response 200 (application/json)
```json
	{
	"status": "SUCCESS",
		"data": {
			"card": {
				"id": "00505683065B1EE78EF34A3F0BD0AF4C",
				"holderName": "test",
				"cardType": "001",
				"cardNumber": "xxxxxxxxxxxx8695",
				"expiryMonth": "09",
				"expiryYear": "17",
				"cvv": "123",
				"priorityCard": "Y"
				}
		 },
	"errors": {}
	}
```
## Error scenarios
## Case 1: Save card Detail [POST /cards] - Card already exist	
	
### Request	
 ```json          
	{
	"card": {
		"holderName":"test",
		"cardType":"001",
		"cardNumber":"5eeBZ++aHquYp2pmYseO9W3r+1zginDfYs8KsFfHVBk=",
		"expiryMonth":"05",
		"expiryYear":"17",
		"cvv":"123",
		"priorityCard":"Y"               
	},
	}
```	
### Response 400 (application/json)
```json
	{
	"status": "FAIL",
	"data": {},
		"errors": [
			{
			"code": "card",
			"message": "error.card.already.available"
			}
		],
	"meta": {}
	}
```
## Case 2: Save card Detail [POST /cards] - Service unavailable	
	
### Request
```json
	{
	"card": {
		"holderName":"test",
		"cardType":"001",
		"cardNumber":"5eeBZ++aHquYp2pmYseO9W3r+1zginDfYs8KsFfHVBk=",
		"expiryMonth":"05",
		"expiryYear":"20",
		"cvv":"123",
		"priorityCard":"Y"
	},             
	}
```	
### Response 500 (application/json)
```json
	{
	"status": "FAIL",
	"data": {},
		"errors": [
			{
			"code": "Service unavailable",
			"message": "error.sap.pi.operation.failure"
			}
		]
	}
```	
## Case 3: Edit card Detail [PUT /cards] - Card information changed successfully
### Request
      
```json
	{
	"card": {
		"id":"00505683065B1EE78EF34A3F0BD0AF4C";
		"holderName":"test",
		"cardType":"001",
		"cardNumber":"5eeBZ++aHquYp2pmYseO9W3r+1zginDfYs8KsFfHVBk=",
		"expiryMonth":"05",
		"expiryYear":"20",
		"cvv":"123",
		"priorityCard":"Y"			                
	},             
	}
```
### Response 200 (application/json)
```json
	{
    "status": "SUCCESS",
		"data": {
			"card": {
				"id": "00505683065B1EE78EF34A3F0BD0AF4C",
				"holderName": "test",
				"cardType": "001",
				"cardNumber": "5eeBZ++aHquYp2pmYseO9W3r+1zginDfYs8KsFfHVBk=",
				"expiryMonth": "09",
				"expiryYear": "17",
				"cvv": "213",
				"priorityCard": "Y"
			}
		},
		"errors": {}
	}

```	
	
## Case 4: Delete card Detail [DELETE /cards/{id}] - Deleting Card information

### Parameters

    id: 00505683065B1EE78EF34A3F0BD0AF4C (string) - SaveCardId


### Response 200 (application/json)
```json
	{
    "status": "SUCCESS",
		"data": {
			"card": {
				"id": "00505683065B1EE78EF34A3F0BD0AF4C"
			}
		},
    "errors": {}
	}
```	
## Errors from backend
	
## Case 5: Request 'Error from SAP'

### Response 500 (application/json)
```json
        {
            "status": "ERROR",
            "data": {},
            "errors": [
                {
                    "code": "system",
                    "message": "error.system.unexpected.error"
                }
            ],
            "meta": {}
        }
```	
## Case 6: Card details not valid	

### Request 'Card number is blank'

### Response 400 (application/json)
```json
            {
                "status": "FAIL",
                "data": {},
                "errors": [
                    {
                        "code": "cardNumber",
                        "message": "error.client.cardNumber.blank"
                    }
                ],
                "meta": {}
            }
```			
			
## Case 7: Card details not valid	

### Request 'Invalid card number'

### Response 400 (application/json)
```json
            {
                "status": "FAIL",
                "data": {},
                "errors": [
                    {
                        "code": "cardNumber",
                        "message": "error.client.cardNumber.invalid"
                    }
                ],
                "meta": {}
            }
```
