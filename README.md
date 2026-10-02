# YellowBIRD Delivery API

Before using the API, [sign up](https://logistics.groupngs.com) to obtain an API Key (Private Key). The Private Key is used for authorization.

## Authentication

Include the Private Key in the request header:

```http
Authorization: Bearer <privateKey>
```

**API endpoint used in the examples:**

```text
https://logistics.groupngs.com/api/
```

## Table of Contents

- [Authentication](#authentication)
- [Usage](#usage)
- [1. Price Estimation](#1-price-estimation)
  - [1.1 Parameters](#11-parameters)
  - [1.2 Sample Price Estimation](#12-sample-request)
  - [1.3 Sample Response (Price Estimation)](#13-sample-response)
- [2. Delivery Request](#2-delivery-request)
  - [2.1 Parameters](#21-parameters)
  - [2.2 Sample Delivery Request (Distance Range)](#22-sample-delivery-request-distance-range)
    - [2.2.1 Post Request](#221-request)
    - [2.2.2 Sample Response](#222-response)
- [3. Nearest Driver Distance](#3-nearest-driver-distance)
  - [3.1 Parameters](#31-parameters)
  - [3.2 Sample Response](#32-sample-response)
- [4. Current Request Status](#4-current-request-status)
  - [4.1 Parameters](#41-parameters)
  - [4.2 Request Statuses](#42-request-statuses)
    - [Common](#common)
    - [Delivery Statuses](#delivery-statuses)
    - [Cancellation Statuses](#cancellation-statuses)
    - [Other Statuses](#other-statuses)
- [5. Current Driver Location](#5-current-driver-location)
  - [5.1 Parameters](#51-parameters)
  - [5.2 Sample Response](#52-sample-response)
- [6. Request Cancellation](#6-request-cancellation)
  - [6.1 Parameters](#61-parameters)
  - [6.2 Sample Response](#62-sample-response)
- [7. Payment Modes](#7-payment-modes)

---

## Usage

The following examples show how to consume different API actions

## 1. Price Estimation
### 1.1 Parameters

| Parameter   | Type   | Status   | Description                                                                                                                                                                                                                                 |
| :---------- | :----- | :------- | :------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| action      | string | REQUIRED | directRequestDelivery, directRequestDeliveryRange                                                                                                                                                                                                                       |
| privateKey  | string | REQUIRED | Basic authentication Key obtained from the dashboard credentials inserted in the Header i.e.<br/>`Authorization: 'Bearer ' + privateKey `                                                                                                   |
| countryCode | string | REQUIRED | Country code Name i.e `UG, KE, TZ` etc                                                                                                                                                                                                      |
| vehicleType | string | REQUIRED | The type of carrier to take the package i.e. `DELIVERY_BIKE, DELIVERY_10_20_TON_TRUCK, DELIVERY_3_TON_TRUCK, DELIVERY_5_10_TON_TRUCK, DELIVERY_BIKE_BOX, DELIVERY_CAB, DELIVERY_PICKUP_TRUCK, DELIVERY_PICKUP_TRUCK_OPENED, DELIVERY_TRUCK` |
| origin      | array  | REQUIRED | Latitude and Longitude i.e. `[lat, lng]`                                                                                                                                                                                                    |
| destination | array  | REQUIRED | Latitude and Longitude i.e. `[lat, lng]`                                                                                                                                                                                                    |
### 1.2 Sample Request (Price estimation)
```js
let config = {
    headers: {
        Authorization: 'Bearer ' + privateKey
    }
}
let data = {
    "action": "estimateDeliveryFeesRange",
    "countryCode": "UG",
    "orderId":"123455",
    "origin": "[0.3184833930051751, 32.62019266226217]",
    "destination": "[0.31934915908063444, 32.61512999584997]",
    "deliveryOption" : "STANDARD",
    "packageDetails" :{
        "packageWeightKg" : 1.2,
        "packageHeightCm" : 13.7,
        "packageWidthCm" : 17.1,
        "packageLenghtCm" : 27.1,
        "packageTotalCost" : 6356000,
        "itemQuantity" : 2
    },
    "packagesMultiple" : [{
        "packageWeightKg" : 1.6,
        "packageHeightCm" : 9,
        "packageWidthCm" : 11,
        "packageLenghtCm" : 25,
        "packageTotalCost" : 2339000,
        "itemQuantity" : 1
    },{
        "packageWeightKg" : 0.4,
        "packageHeightCm" : 10,
        "packageWidthCm" : 5,
        "packageLenghtCm" : 15,
        "packageTotalCost" : 57000,
        "itemQuantity" : 2
    }]
}
axios.post('https://logistics.groupngs.com/api/', data, config)
.then(...)
.catch(...)
```

### 1.3 Sample Response (Price estimation)

```json
{
    "env": "UAT",
    "orderId": "123455",
    "companyName": "Moja Express",
    "logisticsPricingLabel": "Distance Based Pricing",
    "estimatedDistance": 0.7995489128928784,
    "estimatedDuration": 4.330889944836425,
    "currency": "UGX",
    "countryCode": "UG",
    "zoneLabel": "undefined",
    "deliveryOption": "STANDARD",
    "totalWeighInKg": 4.800000000000001,
    "volumetricWeight": 16672.434,
    "totalPackageAmount": 6356000,
    "totalPackageQuantity": 2,
    "minimumFare": 2500,
    "estimatedFare": 2500,
    "estimatedFee": 2500,
    "message": "OK"
}
```

## 2. Delivery Request
### 2.1 Parameters

| Parameter            | Type   | Status   | Description                                                                                                                                                                                                                                                                                                                                                   |
| :------------------- | :----- | :------- | :------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| action               | string | REQUIRED | directRequestDeliveryZone, directRequestDeliveryRange                                                                                                                                                                                                                                                                                                                                       |
| privateKey           | string | REQUIRED | Basic authentication Key obtained from the dashboard credentials inserted in the Header i.e.<br/>`Authorization: 'Bearer ' + privateKey `                                                                                                                                                                                                                     |
| countryCode          | string | REQUIRED | Country code Name i.e `UG, KE, TZ` etc                                                                                                                                                                                                                                                                                                                        |
| vehicleType          | string | REQUIRED | The type of carrier to take the package i.e. `DELIVERY_BIKE, DELIVERY_10_20_TON_TRUCK, DELIVERY_3_TON_TRUCK, DELIVERY_5_10_TON_TRUCK, DELIVERY_BIKE_BOX, DELIVERY_CAB, DELIVERY_PICKUP_TRUCK, DELIVERY_PICKUP_TRUCK_OPENED, DELIVERY_TRUCK`                                                                                                                   |
| deliveryInstructions | string | OPTIONAL | Instructions of the delivery request, i.e Clear description of the intended delivery destination                                                                                                                                                                                                                                                              |
| packageWeight        | double | OPTIONAL | Weight of the Package in Kilograms(kg)                                                                                                                                                                                                                                                                                                                        |
| packageDescription   | string | OPTIONAL | Description of the package                                                                                                                                                                                                                                                                                                                                    |
| pickupContactInfo    | object | REQUIRED | Details of the sender i.e.<br/> `{fullName: String (REQUIRED), phoneNumber: String (REQUIRED), email: String (OPTIONAL), gender: String (OPTIONAL), description: String (OPTIONAL), addressLatLng: Array (REQUIRED) i.e. [lat, long], addressLabel: String (REQUIRED), city: String (OPTIONAL), building: String (OPTIONAL), plotNumber: String (OPTIONAL)}`  |
| dropOffContactInfo   | object | REQUIRED | Details of the receiver i.e.<br/>`{fullName: String (REQUIRED), phoneNumber: String (REQUIRED), email: String (OPTIONAL), gender: String (OPTIONAL), description: String (OPTIONAL), addressLatLng: Array (REQUIRED) i.e. [lat, long], addressLabel: String (REQUIRED), city: String (OPTIONAL), building: String (OPTIONAL), plotNumber: String (OPTIONAL)}` |
| paymentMode          | string | REQUIRED | Mode of payment, see Payment Modes for details                                                                                                                                                                                                                                                                                                               |

### 2.2 Sample Request (Delivery Request)
#### 2.2.1 Request
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
        "itemQuantity": 2,
        "handlingRequirements": {
            "fragile": false,
            "hazardous": false,
            "medicalItem": false,
            "prescriptionRequired": false,
            "specialPackagingRequired": false,
            "uprightPositionRequired": false,
            "refrigerationRequired": false,    
            "ageRestriction": 18,
            "temperatureRange": { 
                "min": 5,
                "max": 30
            },
            "fragilityLevel": "None",  // Options: None, Low, Medium, High
            "isLightSensitive": false,
            "isWaterSensitive": true,
            "handlingInstructions": "Maintain temperature between 5°C and 30°C to ensure product quality."
        },
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
    "itemPaymentMode": "CASH_ON_DELIVERY",
    "deliveryPaymentMode": "CASH_ON_DELIVERY",
    "deliveryFeeFromPartner": null, //This is in case the partner may want to charge a different delivery fee to the customer. (From the driver App)
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

#### 2.2.2 Sample Response (Delivery Request)
```js
  {
    "env": "UAT",
    "logisticsPricingLabel": "Distance Based Pricing",
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

## 3. Nearest Driver Distance
### 3.1 Parameters

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
axios.post('https://logistics.groupngs.com/api/', data, config)
.then(...)
.catch(...)
```

### 3.2 Sample Response

```json
{
  "distance": 456
}
```

## 4. Current Request Status

### 4.1 Parameters

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
axios.post('https://logistics.groupngs.com/api/', data, config)
.then(...)
.catch(...)
```

### 4.2 Request Statuses

#### Common

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

#### Delivery Statuses

| Status                        | Description                            |
| :---------------------------- | :------------------------------------- |
| DRIVER_ON_THE_WAY_TO_PICKUP   | DRIVER ON THE WAY TO PICKUP POINT      |
| DRIVER_ARRIVED_AT_PICKUP      | DRIVER ARRIVED AT PICKUP POINT         |
| DRIVER_ON_THE_WAY_TO_DROP_OFF | DRIVER ON THE WAY TO DROP OFF LOCATION |
| DRIVER_ARRIVED_AT_DROP_OFF    | DRIVER ARRIVED AT DROP OFF LOCATION    |
| ORDER_DELIVERED               | ORDER DELIVERED TO CUSTOMERS           |

#### Cancellation Statuses

| Status                           | Description                      |
| :------------------------------- | :------------------------------- |
| USER_CANCELED_ACCEPTED_REQUEST   | USER CANCELED ACCEPTED REQUEST   |
| DRIVER_CANCELED_ACCEPTED_REQUEST | DRIVER CANCELED ACCEPTED REQUEST |
| REQUEST_EVENT_CANCELED           | REQUEST EVENT CANCELED           |
| USER_CANCELED_STARTED_TRIP       | USER CANCELED STARTED TRIP       |
| DRIVER_CANCELED_STARTED_TRIP     | DRIVER CANCELED STARTED TRIP     |
| REQUEST_DELETED_BY_DRIVER        | REQUEST DELETED BY DRIVER        |
| CLIENT_CANCELED_ACCEPTED_REQUEST | REQUEST CANCELLED BY CLIENT VIA API|

#### Other Statuses

| Status                    | Description               |
| :------------------------ | :------------------------ |
| CLIENT_PICKUP_CONFIRMED   | CLIENT PICKUP CONFIRMED   |
| CLIENT_DROP_OFF_CONFIRMED | CLIENT DROP OFF CONFIRMED |
| RATING_DONE               | RATING DONE               |
| RATING_DONE_CLIENT        | RATING DONE CLIENT        |
| RATING_DONE_FREELANCER    | RATING DONE FREELANCER    |

## 5. Current Driver Location

### 5.1 Parameters

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
axios.post('https://logistics.groupngs.com/api/', data, config)
.then(...)
.catch(...)
```

### 5.2 Sample Response

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

## 6. Request Cancellation

### 6.1 Parameters

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
axios.post('https://logistics.groupngs.com/api/', data, config)
.then(...)
.catch(...)
```

### 6.2 Sample Response

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

## 7.Delivery Payment Modes

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
