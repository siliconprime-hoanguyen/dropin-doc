#Dropin API

NewestProduction-API: https://api.dropininc.com (02-12-2016)
NewestProduction-Web: https://web.dropininc.com (02-12-2016)

Dev-API: https://apidev21.dropininc.com
Dev-Web: https://appdev.dropininc.com

QA-API: https://apiqa21.dropininc.com
QA-Web: https://appqa.dropininc.com

Staging-API: https://apistaging21.dropininc.com
Staging-Web: https://appstaging.dropininc.com

Production-API: https://apiprod21.dropininc.com
Production-Web: https://app.dropininc.com

NewProduction-API: https://apiprod22.dropininc.c
NewProduction-Web: https://app2.dropininc.com


Demo-Staging-API: https://apidemo.dropininc.com
Demo-Staging-Web: https://demo.dropininc.com

Research-QA-API: https://apiqa22.dropininc.com
Research-QA-Web: https://appqa22.dropininc.com



## Config

### Get app price config

```javascript
GET /apppriceconfig
```

### Response 
```javascript
{
  "initialFee": 100,
  "streamingFee": 2,
  "travelFee": 2
}
```


## Device

### Add device

```javascript
POST /devices
```

### Body
```javascript
{
  deviceAddress:'xxxxxx', //required
  deviceType: 'ios', //or 'android', required
  bundleId: 'com.dropininc.com', //if not provided, then default bundle id configed is server will be used.
  type: 'push' //or 'voip', default is push
}
```


## Upload


### Update status after uploading

```javascript
PATCH /storage/:id
```

### Body
```javascript
{
  status:'uploaded', //or 'failed' if fail

}
```


### Get signed URL for multiple assets

```javascript
POST /storagebatchlinks
```
### Body
```javascript
{
 
  assetIds: ['fdsgdgfgdf','fdsfdsfds'] //array of assetIds
}
```



### Get signed URL for upload with login

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

### Get signed URL for uploading org logo

```javascript
POST /storagelogo
```

## Get signed URL for uploading org logo without text

```javascript
POST /storagelogowithouttext
```

### Body
```javascript
{

  organization: 'organizationId'  organizationId
}
```


### removal of ALL org logos

```javascript
DELETE /organizations/:organizationId/logos/all
```

### removal of ALL org logos (without text)

```javascript
DELETE /organizations/:organizationId/logoswithouttext/all
```

## Upload

### Get signed URL for upload without login

```javascript
POST /storagenologin
```

### Body
```javascript
{
  type:'gig', //required
  gig: 'fdsgdgfgdf' gigId
}
```

After that, please call PUT to the url returned to upload to S3.



## Admin

### Getting gigs and counting gigs. 

```javascript
GET /admin/gigs/count?limit=100&skip=0&term=cod
```

### Query
```javascript
{
  term:'abc', //email, phone
  skip: 0, //start from record 0
  limit: 100, //get 100 records.
  identity: ['phone', 'email'], // one of the 2 value, do not provide to ignore.
  inOrg: [true, false], // one of true or false, do not provide to ignore
  role :['viewer', 'operator'], //viewer or operator, do not provide to inore
  status: ['pendingOperator', 'operatorFound', 'allOperatorsRejected', 'operatorEnroute', 'handshaking', 'streaming', 'purchased', 'voted', 'invoiceGenerated', 'invoiceSent', 'compensated', 'expired', 'failed', 'cancelled', 'customerRejected', 'terminated'] //one or some of them, separated by comma, do not provide to get all
}
```



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

### Logging pusher message to server

```javascript
POST /notification/logs
```

#### Body
```javascript
{
   "code":3,
   "messageId": "32432432",
   "gig": "fdsfdsfds",
   "metaData": {
      "fdsfs":"Fdsfds"
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

### Pinging

This API is to ping 

- For viewer please call this ONLY when streaming
- For droperat please call this when enrouting and streaming

```javascript
POST /ping
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


### Request operator to capture photo

This API is to send capture screen request to droperator, given customer fulfilled all requirements.

```javascript
POST /gigs/capture/:gigId
```

#### Notification sent to operator

```javascript
{
  "code": 21,
  "title": "Capture photo",
  "message": "Capture photo",
  "messageId": "6d800621-8fbc-4a3f-b91b-83172c4a8b01",
  "isUserNotification": false,
  "gigId": "58183312831dc27b37676fca",
  "resource": {
    "location": "https://dropin-dev-public-photo-v2.s3-us-west-1.amazonaws.com/b8f20d10-9ffa-11e6-a29c-97857e854058?AWSAccessKeyId=AKIAIGJ3SBRYGVQBRNBQ&Content-Type=binary%2Foctet-stream&Expires=1477981887&Signature=kwSJWD5AbMsV1z28a%2F1cK9B9Yv8%3D&x-amz-acl=public-read-write"
  }
}
```

### Inform customer that photo is uploaded

This API is to notify customer that the photo is uploaded

```javascript
POST /gigs/capturedone/:gigId
```
### Change video provider (web)

This API is to switch video provider while streaming

```javascript
POST /gigs/switchVideoProvider/:gigId
```

#### Body

```javascript
{
 provider: 'twilio' //or 'opentok'
}
``

#### Notification sent to customer
```javascript
{
  "code": 23,
  "title": "Finished capturing photo",
  "message": "Finished capturing photo",
  "messageId": "ecffec9c-717d-49fc-9ad2-57b57ef59bc5",
  "isUserNotification": false,
  "gigId": "58183312831dc27b37676fca",
  "location": "https://dropin-dev-public-photo-v2.s3-us-west-1.amazonaws.com/b8f20d10-9ffa-11e6-a29c-97857e854058"
}
```

### Customer requests drone streaming 

This API is to send stream request to available droperators, given customer fulfilled all requirements.

```javascript
POST /gigs/drone/request
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


### Check bandwidth


This API is to download a file to test bandwidth

```javascript
GET /testfile/12312
```


### Log network for gig 
streaming

This API is to log status for gig while streaming

```javascript
POST /gigs/logStatusWhileStreaming/:gigId
```

#### Body
```javascript
{
  bandwidth: 123
  //Whatever to put here
}
```

### Check network for gig before streaming

This API is to log status for gig before streaming

```javascript
POST /gigs/checkStatusBeforeStreaming/:gigId
```

#### Body
```javascript
{
  bandwidth: 123
  //Whatever to put here
}
```

### Log network

This API is to log network status

```javascript
POST /gigs/logNetwork
```

#### Body
```javascript
{
  bandwidth: 123
  //Whatever to put here
}
```


### Customer requests drone direct streaming 

This API is to send stream request directly to drone.

```javascript
POST /gigs/drone/directrequest
```

#### Body
```javascript
{
  email: 'abc@def.com',
  phone: 'xxxxxx',
  claimId: "fdsfdsfd" //optional
  metaData: { //optional, json

    instruction: "sdfdsfsd"
    //put whatever here
  }
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
  claimId: "fdsfdsfd" //optiona
  metaData: { //optional, json
    city:"Saigon"
    //put whatever here
  }
}
```

OR 


#### Body
```javascript
{
  email: 'abc@def.com',
  phone: 'xxxxxx',
  uploadFullSizePhoto: true, //or false, whether to upload full size photo.
  claimId: "fdsfdsfd" //optional
  metaData: { //optional, json

    instruction: "sdfdsfsd"
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
  response: ['accept', 'reject'], //string, required, must be one of the 2 values
  travelTime: 333,  //estimate travel time measured in seconds
  travelDistance: 33.3 //estimate travel distance measured in miles.
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

### Count the gig for customer role of users

```javascript
GET admin/gigs/count?start=xxx&limit=xxx&from=xxx&to=xxx&status=xxxx
```

### Get users

```javascript
GET accounts/search/all
```
#### Query

```javascript
{
    q: 'sdfsdfs' //search term
    from: '2016-10-9' //search from
    to: '2016-10-9' // search to
    backgroundStatus: ['incompletedForm', 'completedForm', 'passed', 'failed'],
    preferred: [true,false]
    droneAccepted: [true,false]
    accountType: ['organization','mobile'] //ignore for all
}
```


#### Params

```javascript
{
  skip: 0,
  limit: 20 //default 99999
  from: '2015-02-20', //gig from?
  to: '2015-02-30' //gig to?,
  status: 'voted,purchased' //good shit comes here, separated by comma, omit this param to get all.
}
```

#### Result sample
```javascript
{
    "total": 2, //how many records in total?
    "gigs": [
        {
            "_id": "57359b5efd50c1af4e0c642f", //customer id
            "count": 1
        },
        {
            "_id": "572d71dde971734904c2c254", //customer id
            "count": 1
        }
    ]
}
```

### Notes: who fails rating will be locked down from further gig's related ations.


## Streaming History

### Get all payout records

```javascript
'get /payouts'
```

**params**

|  key  |  type  | optional | default values |      description       |
|-------|--------|----------|----------------|------------------------|
| start | int    | true     | 0              | start from             |
| limit | int    | true     | 0              | limit number of record |
| sort  | string | true     | id desc        | sort by field/order    |
| from  | date   | true     | NA             | from date              |
| to    | date   | true     | AN             | to date                |


**output**

```javascript
[{
    "gig": {
        "id": "5720955de2db76686c1f7c4d",
        "createdAt": "2016-04-27T10:33:01.579Z",
        "duration": 107,
        "status": "voted",
        "chatChannel": "chat_5720955de2db76686c1f7c4d",
        "latitude": 10.784079034302,
        "longitude": 106.704355850816,
        "metaData" : {
	        "location" : "123 Lê Quý Đôn, phường 7, Quận 3, Hồ Chí Minh, Vietnam", 
	        "city" : "Ho Chi Minh City", 
	        "state" : "CA", 
	        "zipCode" : "70000", 
	        "phoneNumber" : "84975787776", 
	        "instruction" : "Show me!"
	    }, 
    },
    "payment": {
        "id": "572095c9e2db76686c1f7c58",
        "amount": "5.89",
        "createdAt": "2016-04-27T10:34:49.299Z"
    },
    "rating": {
        "value": 1
    },
    "account": {
        "firstName": "0",
        "lastName": "1",
        "profileImage": ""
    }
}]
```

**note**

*metaData could be null

### Get all purchases records

```javascript
'get /purchases'
```

**params**

|  key  |  type  | optional | default values |      description       |
|-------|--------|----------|----------------|------------------------|
| start | int    | true     | 0              | start from             |
| limit | int    | true     | 0              | limit number of record |
| sort  | string | true     | id desc        | sort by field/order    |
| from  | date   | true     | NA             | from date              |
| to    | date   | true     | AN             | to date                |


**output**

```javascript
[{
    "gig": {
        "id": "5720955de2db76686c1f7c4d",
        "createdAt": "2016-04-27T10:33:01.579Z",
        "duration": 107,
        "status": "voted",
        "chatChannel": "chat_5720955de2db76686c1f7c4d",
        "latitude": 10.784079034302,
        "longitude": 106.704355850816,
        "metaData" : {
	        "location" : "123 Lê Quý Đôn, phường 7, Quận 3, Hồ Chí Minh, Vietnam", 
	        "city" : "Ho Chi Minh City", 
	        "state" : "CA", 
	        "zipCode" : "70000", 
	        "phoneNumber" : "84975787776", 
	        "instruction" : "Show me!"
	    }, 
    },
    "payment": {
        "id": "572095c9e2db76686c1f7c58",
        "amount": "5.89",
        "createdAt": "2016-04-27T10:34:49.299Z"
    },
    "rating": {
        "value": 1
    },
    "account": {
        "firstName": "0",
        "lastName": "1",
        "profileImage": ""
    }
}]
```

**note**

*metaData could be null


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

**note**


## Chat History

### Get All Chat record of a gig

```javascript
'get /chat/:gigId'
```

**params**

|  key  |   type   | optional | default values |         description          |
|-------|----------|----------|----------------|------------------------------|
| gigId | objectId | false    | NA             | ID of gig want to get detail |
| skip  | int      | true     | 0              | number of record to skip     |
| limit | int      | true     | 10             | number of messsage to fetch  |

**output**

```js
[
    {
        "gig": "57077c882fe344bb6def7f58",
        "account": "57077c912fe344bb6def7f62",
        "message": "third mess ",
        "createdAt": "2016-04-27T20:35:28.102Z",
        "updatedAt": "2016-04-27T20:35:28.102Z",
        "id": "572122906a6efa361698ae6c"
    },
    {
        "gig": "57077c882fe344bb6def7f58",
        "account": "57077c912fe344bb6def7f62",
        "message": "fourth mess ",
        "createdAt": "2016-04-27T20:35:32.210Z",
        "updatedAt": "2016-04-27T20:35:32.210Z",
        "id": "572122946a6efa361698ae6d"
    },
    {
        "gig": "57077c882fe344bb6def7f58",
        "account": "57076063674ea5da598a6a19",
        "message": "fifth mess from the other",
        "createdAt": "2016-04-27T20:35:46.042Z",
        "updatedAt": "2016-04-27T20:35:46.042Z",
        "id": "572122a26a6efa361698ae6e"
    }
]
```

**note**

* Messages are ordered in time
* Use start/limit for paging


 ### Save chat message

```javascript
'post /chat/save'
```

**params**

|   key   |   type   | optional | default values |         description          |
|---------|----------|----------|----------------|------------------------------|
| gig     | objectId | false    | NA             | ID of gig want to get detail |
| from    | objectId | false    | NA             | Sender of message            |
| message | String   | false    | NA             | message content              |


**output**

```js
{
    "account": "57076063674ea5da598a6a19",
    "message": "fifth mess from the other",
    "gig": "57077c882fe344bb6def7f58",
    "createdAt": "2016-04-27T20:45:20.565Z",
    "updatedAt": "2016-04-27T20:45:20.565Z",
    "id": "572124e0a528836216cfdb59"
}
```

**note**

* Clients save message after receiving it
* Clients **do not** save message it send


 
## Zendesk

### Get all articles

```javascript
GET /zendesk/articles
```

**params**

**output**

[Sample output](./zendeskArticles.json)



### Get all articles

```javascript
GET /zendesk/articles
```

**params**

**output**

[Sample output](./zendeskArticles.json)


### Submit new ticket

```javascript
POST /zendesk/tickets
```

**params**

|   key      |   type   | optional | default values |         description          |
|------------|----------|----------|----------------|------------------------------|
| subject    | String   | false    | NA             | subject of ticket            |
| body       | String   | false    | NA             | body of ticket               |
| email      | String   | false    | NA             | email of user                |
| attachment | String   | true     | NA             | attachment to send           |

**output**

[Sample output](./zenDeskTicketResponse.json)

**notes**
- add multiple attachment fields for multiple file upload



## MixPanel

### Traking event

```javascript
post /mixpanel/track
```


**body**
```javascript
{
  "udid": "56fd6925058babeb4e977fbf",
  "event": "testing-event2",
  "properties":{
    "field3": "value 3",
    "field4": "value 4"
  }
}
```


|   key      |   type   | optional | default values |         description          |
|------------|----------|----------|----------------|------------------------------|
| udid       | objectId | false    | NA             | udid of device               |
| event      | String   | false    | NA             | event to track               |
| properties | Object   | true     | NA             | property to store            |

**output**

```js
{
    code:200
}
```
**note**

* Mobile can use any unique id associate with phone for udid field



### upload to smart3d

```javascript
POST /storage/:assetId/3dsmartpicture
```

### share link

```javascript
POST storage/3dsmart/share
```

```javascript
{
	email: 'avsd@fdsfd.com',
	message: 'test message',
	url: 'http://localhost'
}
```
