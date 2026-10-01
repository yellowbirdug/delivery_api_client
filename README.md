# YellowBIRD Delivery API Client

Before usage, [sign up](https://logistic.groupngs.com) today to get an API Key (Private Key) which will be used for Authorization.

## Usage

The following examples show how to consume different api actions

## Delivery Request

### 1. Parameters

| Parameter            | Type   | Status   | Description                                                                                                                                                                                                                                                                                                                                                   |
| :------------------- | :----- | :------- | :------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| action               | string | REQUIRED | directRequestDelivery  | directRequestDeliveryRange                                                                                                                                                                                                                                                                                                                                       |
| privateKey           | string | REQUIRED | Basic authentication Key obtained from the dashboard credentials inserted in the Header i.e.<br/>`Authorization: 'Bearer ' + privateKey `                                                                                                                                                                                                                     |
| countryCode          | string | REQUIRED | Country code Name i.e `UG, KE, TZ` etc                                                                                                                                                                                                                                                                                                                        |
| vehicleType          | string | REQUIRED | The type of carrier to take the package i.e. `DELIVERY_BIKE, DELIVERY_10_20_TON_TRUCK, DELIVERY_3_TON_TRUCK, DELIVERY_5_10_TON_TRUCK, DELIVERY_BIKE_BOX, DELIVERY_CAB, DELIVERY_PICKUP_TRUCK, DELIVERY_PICKUP_TRUCK_OPENED, DELIVERY_TRUCK`                                                                                                                   |
| deliveryInstructions | string | OPTIONAL | Instructions of the delivery request, i.e Clear description of the intended delivery destination                                                                                                                                                                                                                                                              |
| packageWeight        | double | OPTIONAL | Weight of the Package in Kilograms(kg)                                                                                                                                                                                                                                                                                                                        |
| packageDescription   | string | OPTIONAL | Description of the package                                                                                                                                                                                                                                                                                                                                    |
| pickupContactInfo    | object | REQUIRED | Details of the sender i.e.<br/> `{fullName: String (REQUIRED), phoneNumber: String (REQUIRED), email: String (OPTIONAL), gender: String (OPTIONAL), description: String (OPTIONAL), addressLatLng: Array (REQUIRED) i.e. [lat, long], addressLabel: String (REQUIRED), city: String (OPTIONAL), building: String (OPTIONAL), plotNumber: String (OPTIONAL)}`  |
| dropOffContactInfo   | object | REQUIRED | Details of the receiver i.e.<br/>`{fullName: String (REQUIRED), phoneNumber: String (REQUIRED), email: String (OPTIONAL), gender: String (OPTIONAL), description: String (OPTIONAL), addressLatLng: Array (REQUIRED) i.e. [lat, long], addressLabel: String (REQUIRED), city: String (OPTIONAL), building: String (OPTIONAL), plotNumber: String (OPTIONAL)}` |
| paymentMode          | string | REQUIRED | Mode of payment , see Payment Modes for details                                                                                                                                                                                                                                                                                                               |

### 2. Payement Modes

| Mode                        | Description                 |
| :-------------------------- | :-------------------------- |
| CASH                        | CASH                        |
| MOBILE_MONEY                | MOBILE MONEY                |
| BANK_TRANSFER               | BANK TRANSFER               |
| CREDIT_CARD                 | CREDIT CARD                 |
| CASH_BY_SENDER              | CASH BY SENDER              |
| CASH_BY_RECIPIENT           | CASH BY RECEIVER            |
| CASH_ON_DELIVERY            | CASH ON DELIVERY            |
| YELLOW_PAY                  | YELLOW PAY                  |
| MOBILE_WALLET               | MOBILE WALLET               |
| BUSINESS_ACCOUNT            | BUSINESS ACCOUNT            |
| AUTO_RECOVERY               | AUTO RECOVERY               |
| POSTPAID_E_COMMERCE_PARTNER | POSTPAID E COMMERCE PARTNER |
| PREPAID_E_COMMERCE_PARTNER  | PREPAID E COMMERCE PARTNER  |

### 3. Sample delivery Request
#### 3.1 Sample delivery Request Zone

##### Post Request
```js
let config = {
    headers: {
        Authorization: 'Bearer ' + privateKey
    }
}

let data = {
    "action": "directRequestDelivery",
    "countryCode": "UG",
    "vehicleType": "DELIVERY_CAB",
    "paymentMode": "CASH",
    "pickupContactInfo": {
        "fullName": "Isaac Stores",
        "phoneNumber": "77900000",
        "countryCode": "+256",
        "email": "isaacopiow@email.com",
        "gender": "",
        "description": "String (OPTIONAL)",
        "addressLatLng": "[0.29, 32.62]",
        "addressLabel": "Acacia Mall",
        "city": "Kampala",
        "building": "String (OPTIONAL)",
        "plotNumber": "String (OPTIONAL)"
    },
    "dropOffContactInfo": {
        "fullName": "Philip Akol",
        "phoneNumber": "77900000",
        "countryCode": "+256",
        "email": "",
        "gender": "",
        "description": "String (OPTIONAL)",
        "addressLatLng": "[0.33, 32.58]",
        "addressLabel": "Unknown",
        "city": "Kampala",
        "building": "String (OPTIONAL)",
        "plotNumber": "String (OPTIONAL)"
    }
}
axios.post('https://logistic.groupngs.com/api/', data, config)
.then(...)
.catch(...)
```

##### Sample Response Zone

```json
{
  "estimatedDistance": 6.290088835583491,
  "estimatedDuration": 29.21120981731499,
  "estimatedFee": 8155.734773947344,
  "requestID": "38fa6ce0525cb...3be0183cc2f5ca73",
  "currency": "UGX",
  "message": "Delivery request sent"
}
```


#### 3.2 Sample delivery Request Range
##### Post Request

```json
{
    "action": "directRequestDeliveryRange",
    "countryCode": "UG",
    "vehicleType": "DELIVERY_MOTORBIKE",
    "paymentMode": "POSTPAID_E_COMMERCE_PARTNER",
    "pickupContactInfo": {
        "fullName": "Liquor Barr 21",
        "phoneNumber": "+256712345678",
        "countryCode": "256",
        "email": "WD",
        "gender": null,
        "description": null,
        "addressLatLng": "[0.32676, 32.58026]",
        "addressLabel": "Kampala",
        "city": "Kampala",
        "building": null,
        "plotNumber": null
    },
    "dropOffContactInfo": {
        "fullName": "John Mugabe",
        "phoneNumber": "+256723456789",
        "countryCode": "256",
        "email": null,
        "gender": null,
        "description": null,
        "addressLatLng": "[0.31516919991087, 32.58163130000001]",
        "addressLabel": "5B Speke Road, Kampala, Uganda",
        "city": "Kampala",
        "building": null,
        "plotNumber": null
    },
    "packageDetails": {
        "packageWeightKg": 1.0,
        "packageHeightCm": 15.7,
        "packageWidthCm": 4.3,
        "packageTotalCost": 68800.0,
        "itemLabel": "Johnnie Walker Red Label",
        "itemDescription": "Crafted from the four corners of Scotland, it crackles with spice and is bursting with vibrant, smoky flavours – followed by a mellow bed of vanilla, a fresh zestiness and the Johnnie Walker signature of a long, lingering, smoky finish.",
        "itemColor": null,
        "itemImageUrl": "https://ke-thebar-business.agiza.io/rails/active_storage/blobs/redirect/eyJfcmFpbHMiOnsibWVzc2FnZSI6IkJBaHBBbXdGIiwiZXhwIjpudWxsLCJwdXIiOiJibG9iX2lkIn19--9d3b5390a7b89263e2caec17eda0706bfbc24b26/JW%20Red%20250ml.png",
        "itemQuantity": 2
    },
    "packagesMultiple" : [{
        "packageWeightKg": 1.0,
        "packageHeightCm": 12.7,
        "packageWidthCm": 4.7,
        "packageTotalCost": 72800.0,
        "itemLabel": "Dimple 15 Year Old Blended Scotch Whisky",
        "itemDescription": "Dimple offers an inviting flavour from 15 Years Old Supreme Quality Malt and Grain Whiskies which result in a delightful, smooth flavoured whisky with notes of mango, mocha and mixed nuts.",
        "itemColor": null,
        "itemImageUrl": "https://ke-thebar-business.agiza.io/rails/active_storage/blobs/redirect/eyJfcmFpbHMiOnsibWVzc2FnZSI6IkJBaHBBaE1FIiwiZXhwIjpudWxsLCJwdXIiOiJibG9iX2lkIn19--d262e837ca2745cda1d9343b7eb8b1963b31ad45/John%20Walker%20&%20Sons%20Odyssey.png",
        "itemQuantity": 1
    },{
        "packageWeightKg" : 1.4,
        "packageHeightCm" : 10,
        "packageWidthCm" : 5,
        "packageLenghtCm" :2.1,
        "packageTotalCost" : 7000,
        "itemLabel" : "Alvaro Pineapple",
        "itemDescription" : "A unique refreshing non alcoholic natural malt drink",
        "itemColor" : "GREEN",
        "itemImageUrl" : "https://ke-thebar-business.agiza.io/rails/active_storage/blobs/redirect/eyJfcmFpbHMiOnsibWVzc2FnZSI6IkJBaHBBanNDIiwiZXhwIjpudWxsLCJwdXIiOiJibG9iX2lkIn19--0b274bc24be974aff726b9d332dfb183ffc88bbf/Alvaro-20Pineapple.png",
        "itemQuantity" : 1
    }],
    "pickupCheckList" : ["Plastic cups added","The item is new",".."],
    "orderId":"U1234",
    "suborderID":"U5678",
    "DeliveryOption":"STANDARD",
    "returnDelivery":false,
    "returnItemReason":null,
    "returnOriginalRequestIdHash":null,
    "orderPlacedDateTimeMills":"12345678"
}
```

##### Sample Response Range
```json
{
    "env": "UAT",
    "logisticsPricingLabel": "Distance Based Pricing intouch",
    "estimatedDistance": 1.8169571553861763,
    "estimatedDuration": 9.841851258341787,
    "estimatedFare": 3000,
    "estimatedFee": 3000,
    "currency": "UGX",
    "zoneLabel": "",
    "deliveryOption": "STANDARD",
    "totalWeighInKg": 4.4,
    "volumetricWeight": 0,
    "feeWithVolumetricWeight": 0,
    "totalPackageAmount": 68800,
    "totalPackageQuantity": 2,
    "minimumFare": 2500,
    "requestID": "YiwRKNWwjc1Cj469A6pkfkHH7bINL22E9KLEIwU_xKo",
    "requestIdHash": "YiwRKNWwjc1Cj469A6pkfkHH7bINL22E9KLEIwU_xKo",
    "orderId": "U1234",
    "countryCode": "UG",
    "returnDelivery": false,
    "message": "Ok"
}
```



### Price Estimation

#### Parameters

| Parameter   | Type   | Status   | Description                                                                                                                                                                                                                                 |
| :---------- | :----- | :------- | :------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| action      | string | REQUIRED | directRequestDelivery, directRequestDeliveryRange                                                                                                                                                                                                                       |
| privateKey  | string | REQUIRED | Basic authentication Key obtained from the dashboard credentials inserted in the Header i.e.<br/>`Authorization: 'Bearer ' + privateKey `                                                                                                   |
| countryCode | string | REQUIRED | Country code Name i.e `UG, KE, TZ` etc                                                                                                                                                                                                      |
| vehicleType | string | REQUIRED | The type of carrier to take the package i.e. `DELIVERY_BIKE, DELIVERY_10_20_TON_TRUCK, DELIVERY_3_TON_TRUCK, DELIVERY_5_10_TON_TRUCK, DELIVERY_BIKE_BOX, DELIVERY_CAB, DELIVERY_PICKUP_TRUCK, DELIVERY_PICKUP_TRUCK_OPENED, DELIVERY_TRUCK` |
| origin      | array  | REQUIRED | Latitude and Longitude i.e. `[lat, lng]`                                                                                                                                                                                                    |
| destination | array  | REQUIRED | Latitude and Longitude i.e. `[lat, lng]`                                                                                                                                                                                                    |

```js
let config = {
    headers: {
        Authorization: 'Bearer ' + privateKey
    }
}

let data = {
    "action": "estimateDeliveryFeesAndTime",
    "countryCode": "UG",
    "vehicleType": "DELIVERY_CAB",
    "origin": "[0.29, 32.62]",
    "destination": "[0.33, 32.58]"
}
axios.post('https://logistic.groupngs.com/api/', data, config)
.then(...)
.catch(...)
```

#### Sample Response

```json
{
  "estimatedDistance": 6.290088835583491,
  "estimatedDuration": 29.21120981731499,
  "currency": "UGX",
  "estimatedFee": 8155.734773947344,
  "message": "OK"
}
```

### Nearest Driver Distance

#### Parameters

| Parameter   | Type   | Status   | Description                                                                                                                                                                                                                                 |
| :---------- | :----- | :------- | :------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| action      | string | REQUIRED | directRequestDelivery                                                                                                                                                                                                                       |
| privateKey  | string | REQUIRED | Basic authentication Key obtained from the dashboard credentials inserted in the Header i.e.<br/>`Authorization: 'Bearer ' + privateKey `                                                                                                   |
| countryCode | string | REQUIRED | Country code Name i.e `UG, KE, TZ` etc                                                                                                                                                                                                      |
| vehicleType | string | REQUIRED | The type of carrier to take the package i.e. `DELIVERY_BIKE, DELIVERY_10_20_TON_TRUCK, DELIVERY_3_TON_TRUCK, DELIVERY_5_10_TON_TRUCK, DELIVERY_BIKE_BOX, DELIVERY_CAB, DELIVERY_PICKUP_TRUCK, DELIVERY_PICKUP_TRUCK_OPENED, DELIVERY_TRUCK` |
| position    | array  | REQUIRED | Latitude and Longitude i.e. `[lat, lng]`                                                                                                                                                                                                    |

```js
let config = {
    headers: {
        Authorization: 'Bearer ' + privateKey
    }
}

let data = {
    "action": "nearestDriverDistance",
    "countryCode": "UG",
    "vehicleType": "DELIVERY_CAB",
    "position": "[0.315133, 32.576353]"
}
axios.post('https://logistic.groupngs.com/api/', data, config)
.then(...)
.catch(...)
```

#### Sample Response

```json
{
  "distance": 456
}
```

### Current Request Status

#### Parameters

| Parameter  | Type   | Status   | Description                                                                                                                               |
| :--------- | :----- | :------- | :---------------------------------------------------------------------------------------------------------------------------------------- |
| action     | string | REQUIRED | directRequestDelivery                                                                                                                     |
| privateKey | string | REQUIRED | Basic authentication Key obtained from the dashboard credentials inserted in the Header i.e.<br/>`Authorization: 'Bearer ' + privateKey ` |
| requestID  | string | REQUIRED | Id of the request                                                                                                                         |

```js
let config = {
    headers: {
        Authorization: 'Bearer ' + privateKey
    }
}

let data = {
    "action": "requestStatus",
    "requestID": "7638bae085e...1f24cf80edff1213"
}
axios.post('https://logistic.groupngs.com/api/', data, config)
.then(...)
.catch(...)
```

#### Sample Request Statuses

##### Common

| Status                       | Description                   |
| :--------------------------- | :---------------------------- |
| DRAFT                        | DRAFT                         |
| WAITING_FOR_DRIVER_TO_ACCEPT | WAITING FOR DRIVER TO ACCEPT  |
| ACCEPTED                     | REQUEST ACCEPTED              |
| REJECTED                     | REQUEST REJECTED              |
| EXPIRED                      | REQUEST EXPIRED               |
| STARTED                      | REQUEST STARTED               |
| COMPLETED                    | REQUEST COMPLETED             |
| DRIVER_ENDING_TRIP           | DRIVER ENDING TRIP            |
| SEARCHING_DRIVER             | SEARCHING DRIVER              |
| DRIVER_ARRIVING              | DRIVER ARRIVING               |
| DRIVER_ARRIVED               | DRIVER ARRIVED                |
| PAYMENT_CONFIRMATION         | PAYMENT RECEIVED FROM CLIENT  |
| NO_DRIVER_FOUND              | NO DRIVER FOUND AT THE MOMENT |

##### Specific to deliveries

| Status                        | Description                            |
| :---------------------------- | :------------------------------------- |
| DRIVER_ON_THE_WAY_TO_PICKUP   | DRIVER ON THE WAY TO PICKUP POINT      |
| DRIVER_ARRIVED_AT_PICKUP      | DRIVER ARRIVED AT PICKUP POINT         |
| DRIVER_ON_THE_WAY_TO_DROP_OFF | DRIVER ON THE WAY TO DROP OFF LOCATION |
| DRIVER_ARRIVED_AT_DROP_OFF    | DRIVER ARRIVED AT DROP OFF LOCATION    |
| ORDER_DELIVERED               | ORDER DELIVERED TO CUSTOMERS           |

##### Cancellations

| Status                           | Description                      |
| :------------------------------- | :------------------------------- |
| USER_CANCELED_ACCEPTED_REQUEST   | USER CANCELED ACCEPTED REQUEST   |
| DRIVER_CANCELED_ACCEPTED_REQUEST | DRIVER CANCELED ACCEPTED REQUEST |
| REQUEST_EVENT_CANCELED           | REQUEST EVENT CANCELED           |
| USER_CANCELED_STARTED_TRIP       | USER CANCELED STARTED TRIP       |
| DRIVER_CANCELED_STARTED_TRIP     | DRIVER CANCELED STARTED TRIP     |
| REQUEST_DELETED_BY_DRIVER        | REQUEST DELETED BY DRIVER        |
| CLIENT_CANCELED_ACCEPTED_REQUEST | REQUEST CANCELLED BY CLIENT VIA API|

##### Others

| Status                    | Description               |
| :------------------------ | :------------------------ |
| CLIENT_PICKUP_CONFIRMED   | CLIENT PICKUP CONFIRMED   |
| CLIENT_DROP_OFF_CONFIRMED | CLIENT DROP OFF CONFIRMED |
| RATING_DONE               | RATING DONE               |
| RATING_DONE_CLIENT        | RATING DONE CLIENT        |
| RATING_DONE_FREELANCER    | RATING DONE FREELANCER    |

### Current Driver Location

#### Parameters

| Parameter  | Type   | Status   | Description                                                                                                                               |
| :--------- | :----- | :------- | :---------------------------------------------------------------------------------------------------------------------------------------- |
| action     | string | REQUIRED | directRequestDelivery                                                                                                                     |
| privateKey | string | REQUIRED | Basic authentication Key obtained from the dashboard credentials inserted in the Header i.e.<br/>`Authorization: 'Bearer ' + privateKey ` |
| requestID  | string | REQUIRED | Id of the request                                                                                                                         |

```js
let config = {
    headers: {
        Authorization: 'Bearer ' + privateKey
    }
}

let data = {
    "action": "driverLocation",
    "requestID": "7638bae085e...1f24cf80edff1213"
}
axios.post('https://logistic.groupngs.com/api/', data, config)
.then(...)
.catch(...)
```

#### Sample Response

```json
{
  "position": "[0.3501982, 32.6146048]",
  "name": "Owen",
  "phone": "+2567077...00",
  "drivingStatus": "ONLINE",
  "serialNumber": "B..7",
  "profilePictureUrl": "https://firebasestorage.googleapis.com/v0/b/....e-f04f17fda7cd.jpeg",
  "message": "OK",
  "status": "200"
}
```



### Request Cancellation 

#### Parameters

| Parameter  | Type   | Status   | Description                                                                                                                               |
| :--------- | :----- | :------- | :---------------------------------------------------------------------------------------------------------------------------------------- |
| action     | string | REQUIRED | cancelRequest                                                                                                                     |
| privateKey | string | REQUIRED | Basic authentication Key obtained from the dashboard credentials inserted in the Header i.e.<br/>`Authorization: 'Bearer ' + privateKey ` |
| requestID  | string | REQUIRED | Hash Id of the request                                                                                                                    |

```js
let config = {
    headers: {
        Authorization: 'Bearer ' + privateKey
    }
}

let data = {
    "action": "cancelRequest",
    "requestID": "arUVHjbkPJamkKlMuplSny/...pU=",
    "comment" : "Client changed his mind"
}
axios.post('https://logistic.groupngs.com/api/', data, config)
.then(...)
.catch(...)
```

#### Sample Response

```json
{
    "action": "cancelRequest",
    "message": "Status updated successfully",
    "oldStatus": "DRAFT",
    "status": "CLIENT_CANCELED_ACCEPTED_REQUEST",
    "requestId": "2026-02-15T23-25-53-169Z-...1652355187233-790",
    "requestID": "arUVHjbkPJamkKlMuplSny/0dkrQMRreMLZgeP4fEpU=",
    "requestIdHash": "arUVHjbkPJamkKlMuplSny/0dkrQMRreMLZgeP4fEpU=",
    "updateDateTimeMillis": 1771198055564,
    "updateDateTimeEAT": "2/15/2026, 11:27:35 PM"
}
```
Please note the request can only be cancelled via this api if the Item has been picked up yet. Otherwise, the cancellation will be performed upon request by YellowBIRD



