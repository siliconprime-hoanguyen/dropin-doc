#Dropin API

## Upload

### Get signed URL for upload

```javascript
POST /storage
```

### Body
```javascript
{
  type:['profileImage','chat','gig'], //required
  gig: 'fdsgdgfgdf' //optional, gigId
}
```

After that, please call PUT to the url returned to upload to S3.



## Push notification

### Send push notification to account

```javascript
POST /notifications/push
```

### Body
```javascript
{
  "recipientId":"571071114372f31b0edae280",
  "data":{
    "message":"sfdsf" // data is decided by user
  }	
}
```



## Logs

### Logging to server

```javascript
POST /logs
```

#### Body
```javascript
{
  "level":"debug",
  "message":"testing message",
  "data":{
    "test":"test"
  }
}
```



## Account APIs

### Register a new account

```javascript
POST /accounts
```
#### Body

```javascript
{

    "firstName" : "Nguyen",
    "lastName" : "Hoa",

    "archive" : false,
    "status" : "active",
    "type" : "user",
    "operator" : null,
    "organization": "organization",
    //app user, this field is NULL, 
    //web user, this field must have a value map to organziation collection
"identities":[{	
		"type":"email",
		"value":"codehubio+29@gmail.com"
	},
	{	
		"type":"phone",
		"value":"841662203830"
	}]
}

```

### Get profile

This API is to get user profile

```javascript
GET accounts/:accountId
```

#### Params

```javascript
{
  accountId: '12312312' //required, id of account to get profile
}
```

#### Result (example)
```javascript
{
    "identities": [
        {
            "type": "email",
            "value": "xxxx@gmail.com",
            "status": "unverified",
            "createdAt": "2016-04-12T03:10:12.542Z",
            "updatedAt": "2016-04-12T03:10:12.542Z",
            "id": "570c6714c0a4eea545970638"
        },
        {
            "type": "phone",
            "value": "xxxxxx",
            "status": "unverified",
            "createdAt": "2016-04-12T03:10:12.544Z",
            "updatedAt": "2016-04-12T03:10:12.544Z",
            "id": "570c6714c0a4eea545970639"
        }
    ],
    "firstName": "Nguyen",
    "lastName": "Hoa",
    "archive": false,
    "status": "active",
    "type": "user",
    "referralRate": 5,
    "createdAt": "2016-04-12T03:10:12.477Z",
    "updatedAt": "2016-04-12T03:10:12.583Z",
    "referralCode": "HyTzwy9",
    "id": "570c6714c0a4eea545970637",
    "operatorRating": 5,
    "customerRating": 5
}
```
## Gig APIs

### Customer searches for operators

This API to get the list of available droperators around a touple of (lat, long)

```javascript
GET map/:lat/:long
```

#### Params
```javascript
{
  lat: 10.12323232 //float number, required
  long: 108.2323232 //float numer, required
}
```


### Customer requests streaming 

This API is to send stream request to available droperators, given customer fulfilled all requirements.

```javascript
POST /gigs
```

#### Body
```javascript
{
  longitude: "10.232132132", //float number, required
  latitude: "10.2323232", //float number, required
  deviceId: "12321432432", //string, required for mobile user, optional for web user
  claimId: "fdsfdsfd" //optional, for web user
  metaData: { //optional, json
    city:"Saigon"
    //put whatever here
  }
}
```

### Operator responses streaming

```javascript
post /gigs/claim/:gigId
```

#### Params
```javascript
{
  gigId: 'fdfdsfdsfds' //string, required, the id of gig to be claimed
}
```
#### Body
```javascript
{
  response: ['accept', 'reject'] //string, required, must be one of the 2 values
}
```

### Customer confirms streaming

```javascript
post /gigs/confirm/:gigId
```

#### Params
```javascript
{
  gigId: 'fdfdsfdsfds' //string, required, the id of gig to be confirmed
}
```

#### Body
```javascript
null
```
### OR customer cancels streaming

```javascript
post /gigs/reject/:gigId
```

#### Params
```javascript
{
  gigId: 'fdfdsfdsfds' //string, required, the id of claimed gig to be rejected
}
```


#### Body
```javascript
null
```


### Customer OR operator cancels a confirmed gig

```javascript
post /gigs/cancel/:gigId
```

#### Params

```javascript
{
  gigId: 'fdfdsfdsfds' //string, required, the id of confirmed gig to be cancelled
}
```


#### Body
```javascript
null
```
### Customer AND operator handshake to get opentok keys

```javascript
post /gigs/handshake/:gigId
```

#### Params

```javascript
{
  gigId: 'fdfdsfdsfds' //string, required, the id of confirmed gig to be handshaked
}
```

#### Body
```javascript
null
```

### Customer OR operator starts the stream

```javascript
post /gigs/start/:gigId
```

#### Params

```javascript
{
  gigId: 'fdfdsfdsfds' //string, required, the id of handshaked gig to be started
}
```

#### Body
```javascript
null
```

### Customer OR operator stops the stream

```javascript
post /gigs/stop/:gigId
```

#### Params

```javascript
{
  gigId: 'fdfdsfdsfds' //string, required, the id of started gig to be stopped
}
````

#### Body
```javascript
null
```


### Customer AND operator rate the stream

```javascript
post /gigs/rate/:gigId
```

#### Params

```javascript
{
  gigId: 'fdfdsfdsfds' //string, required, the id of stopped gig to be rate
}
```

#### Body
```javascript
{
  rating: [1, 2, 3, 4, 5] //int, required.
}
```

### Notes: who fails rating will be locked down from further gig's related ations.


## Streaming History

### Get All Stream Record

```javascript
'get /gigs/:accountId/getall'
```

**params**

|    key    |   type   | optional | default values |                                                                                                                              description                                                                                                                               |
|-----------|----------|----------|----------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| status    | string   | true     | NA             | status of gig ['pendingOperator', 'operatorFound', 'allOperatorsRejected', 'operatorEnroute', 'handshaking', 'streaming', 'purchased', 'voted', 'invoiceGenerated', 'invoiceSent', 'compensated', 'expired', 'failed', 'cancelled', 'customerRejected', 'terminated'], |
| accountId | objectId | false    | NA             | ID of current user                                                                                                                                                                                                                                                                       |
**output**

```js
[
    {
        "customer": "570732086a5e9bd853b4f00b",
        "operator": null,
        "customerRating": null,
        "operatorRating": null,
        "longitude": 106.70653,
        "latitude": 10.787859,
        "duration": 36,
        "status": "expired",
        "id": "57076378674ea5da598a6a67"
    },
    {
        "customer": "570732086a5e9bd853b4f00b",
        "operator": null,
        "customerRating": null,
        "operatorRating": null,
        "longitude": 106.69966366142,
        "latitude": 10.7900511675788,
        "duration": 42,
        "status": "expired",
        "id": "57076453674ea5da598a6a8f"
    }
]
```

**note**

* To get all success stream, use status=voted
* User only get the involed gigs
 


### Get Detail Stream Record

```javascript
'get /gigs/:accountId/:gigId'
```

**params**

|    key    |   type   | optional | default values |         description          |
|-----------|----------|----------|----------------|------------------------------|
| gigId     | objectId | false    | NA             | ID of gig want to get detail |
| accountId | objectId | false    | NA             | ID of current user           |


**output**

```js
{
    "gig": {
        "duration": 69,
        "status": "voted",
        "customer": {
            "firstName": "Ninh",
            "lastName": "Vo"
        },
        "operator": {
            "firstName": "Ninh Vo",
            "lastName": "Thanh"
        },
        "customerRating": {
            "value": 1
        },
        "operatorRating": {
            "value": 1
        },
        "latitude": 10.81507,
        "longitude": 106.673498
    },
    "purchase": {
        "status": "confirmed",
        "amount": 11.15
    },
    "payment": {
        "status": "need_invoice",
        "amount": 5.58
    }
}
```

** note **
