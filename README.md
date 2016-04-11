#Dropin API

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

This API is to send stream request to available droperators, given customer fulfilled all requiremennts.

```javascript
POST /gigs
```

#### Body
```javascript
{
  longitude: 10.232132132 //float number, required
  latitude: 10.2323232 //float number, required
  deviceId: '12321432432'//string, required for mobile user, optional for web user
}
```

### Operator responses streaming

```javascript
post /gigs/claim/:gigId
```

#### Params
{
  gigId: 'fdfdsfdsfds' //string, required, the id of gig to be claimed
}

#### Body

{
  response: ['accept', 'reject'] //string, required, must be one of the 2 values
}


### Customer confirms streaming

```javascript
post /gigs/confirm/:gigId
```

#### Params
{
  gigId: 'fdfdsfdsfds' //string, required, the id of gig to be confirmed
}


#### Body
```javascript
null
```
### OR customer cancels streaming

```javascript
post /gigs/reject/:gigId
```

#### Params
{
  gigId: 'fdfdsfdsfds' //string, required, the id of claimed gig to be rejected
}


#### Body
```javascript
null
```


### Customer OR operator cancels a confirmed gig

```javascript
post /gigs/cancel/:gigId
```

#### Params
{
  gigId: 'fdfdsfdsfds' //string, required, the id of confirmed gig to be cancelled
}


#### Body
```javascript
null
```
### Customer AND operator handshake to get opentok keys

```javascript
post /gigs/handshake/:gigId
```

#### Params
{
  gigId: 'fdfdsfdsfds' //string, required, the id of confirmed gig to be handshaked
}

#### Body
```javascript
null
```

### Customer OR operator starts the stream

```javascript
post /gigs/start/:gigId
```

#### Params
{
  gigId: 'fdfdsfdsfds' //string, required, the id of handshaked gig to be started
}

#### Body
```javascript
null
```

### Customer OR operator stops the stream

```javascript
post /gigs/stop/:gigId
```

#### Params
{
  gigId: 'fdfdsfdsfds' //string, required, the id of started gig to be stopped
}

#### Body
```javascript
null
```


### Customer AND operator rate the stream

```javascript
post /gigs/rate/:gigId
```

#### Params
{
  gigId: 'fdfdsfdsfds' //string, required, the id of stopped gig to be rate
}

#### Body
```javascript
{
  rating: [1, 2, 3, 4, 5] //int, required.
}
```

Notes: who fails rating will be locked down from further gig's related ations.





