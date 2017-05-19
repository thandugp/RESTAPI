## BusinessCards

## Business cards [/BusinessCards]
BusinessCards endpoint allows you to add, edit, delete, retrieve cards. Responses will be returned in JSON format only.

### BusinessCards
Payment as a resource represents following information

| Property | Type | As request | As response | Description |
| :-------------------- | :---------- | :-------------------- | :-------------------- | ------------------------------------------------------------ |
| id | String | `Mandatory` | `Returned always` | It is the Identifier which is required to track the payment which is being made for the particular card. |
| holderName | String | `Manadatory` | `Returned always` | It is the card holder name. |
| cardNo | String |`Optional` | `Returned always` | It represents the card number. |
| cvv | String | `Mandatory` | `Returned always` | It represents the secured card number. |
| cardExpiryMonth | String | `Returned always` | It is the card expiry month. |
| crdExpiryYear | String | `Returned always` | It is the card expiry year. |
| saveCardId | String | `Returned always` | It is the identifier for the card that has been saved in SAP. |
| cardType | String | `Returned always` | It represents the card type.|
| priorityCardIndicator | String | `Returned always` | It is a flag of the card that identifies whether a priority card or not for the customer.|




### Use cases

##Retrieve cards [GET/cards] - Card Details found

	#Response 200 (application/json)
		{
			"status": "SUCCESS",
				"data": {
					"cards": [
						{
							"id": "987654321",
							"holderName": "test",
							"cardType": "001",
							"cardNumber": "00505683065B1EE78EF34A3F0BD0AF4C",
							"expiryMonth": "09",
							"expiryYear": "18",
							"saveCardID": "00505683065B1EE78EF34A3F0BD0AF4C",
							"cvv": "123",
							"priorityCard": "Y"
						}
					]
				},
			"errors": {}
		}
	
##Retrieve cards [GET/cards] - No cards available

	#Response 200 (application/json)
			{
				"status": "SUCCESS",
					"data": {
						"cards": []
							},
				"errors": {}
			}

### Retrieve specific card details [GET /cards/{id}]

+ Parameters

    + id: 00505683065B1EE78EF34A3F0BD0AF4C (string) - SaveCardId

+ Response 200 (application/json)

	{
		"status": "SUCCESS",
			"data": {
					"card": {
						"id": "987654321",
						"holderName": "test",
						"cardType": "001",
						"cardNumber": "xxxxxxxxxxxx8695",
						"expiryMonth": "09",
						"expiryYear": "17",
						"saveCardID": "00505683065B1EE78EF34A3F0BD0AF4C",
						"cvv": "123",
						"priorityCard": "Y"
					}
			},
		"errors": {}
	}
			
### Save card Detail [POST /cards] - Card added successfully
+ Request

	 + Body

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

+ Response 200 (application/json)

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
### Save card Detail [POST /cards] - Card already exist	
	
	+ Request	
		+ Headers

           
		+ Body

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
	
	+ Response 400 (application/json)

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
### Save card Detail [POST /cards] - Service unavailable	
	
	+ Request	
		+ Headers

           
		+ Body

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
	
	+ Response 500 (application/json)

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
	
### Edit card Detail [PUT/cards] - Card information changed successfully
+ Request
      
	 + Body

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

+ Response 200 (application/json)

	{
    "status": "SUCCESS",
		"data": {
			"card": {
				"id": "24564758",
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

	
	
### Delete card Detail [DELETE /cards/{id}] - Deleting Card information

+ Parameters

    + id: 00505683065B1EE78EF34A3F0BD0AF4C (string) - SaveCardId


+ Response 200 (application/json)

	{
    "status": "SUCCESS",
		"data": {
			"card": {
				"id": "00505683065B1EE78EF34A3F0BD0AF4C"
			}
		},
    "errors": {}
	}
	
### Error from backend
	
+ Request 'Error from SAP'

+ Response 500 (application/json)

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
	
### Card details not valid	

+ Request 'Card number is blank'

+ Response 400 (application/json)

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
			
			
### Card details not valid	

+ Request 'Invalid card number'

+ Response 400 (application/json)

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
