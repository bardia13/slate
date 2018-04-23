---
title: Rondino API Reference


toc_footers:
  - <a href='https://www.rondino.ir/getStart/'>Sign up now !</a>
  - <a href='https://www.rondino.ir/aboutUs/'>More about Rondino !</a>

includes:
  - errors

search : true
---

# Introduction

Welcome to Rondino API Refrence. This document is gathered to provide ease of usage of Rondino APIs. For more information about what we do checkout our [homepage](https://www.rondino.ir). If you have any question please feel free to [contact us](https://www.rondino.ir).

Please consider following notes before you continue: 

* Rondino APIs are Restfull and all requests and responses are in JSON format. 

* `"Rondino_ID"` is a unique key that we provide our users upon registering a new website in our panel after login. Please keep this key safe.

* In case you got error `401`; please check the `"Rondino_ID"` that you are using in your requests and check the IP Address that you have entered for your website in our panel. If there still was a problem , please [contact us](https://www.rondino.ir).

* All Urls here are under `https://sandbox-api.rondino.ir`. Sandbox is developed for you in order to test all APIs and features so that these changes does not effect your records in Rondino. 

* When using our Sandbox, it is not necessary that your IP matches the registered IP for your website in Rondino Panel.

* In case you are ready to use Rondino service, make all requests to `https://api.rondino.ir`.

* If you got any `500` Error, make sure of your inputs and read this document carefully. If there still was a problem, please [contact us](https://www.rondino.ir/).

# Authentication

For authentication in Rondino you have to include `Rondino_id` in your requests. You can find it in your panel area.

> Every request you pass to Rondino should include `Rondino_id`:

```json
{
	"rondino_id" : "Your Rondino_id",
	// Other fields
}
```


# RoundUp Operation

In this section we explain our API's that are used for roundup operation 

## Simple RoundUp

This endpoint just round's up your given value 


> Simple RoundUp Example Request

```curl
curl -X POST \
  http://0.0.0.0:8000/client/roundup/ \
  -F rondino_id=7dc9610cf25e4c0192867827c9e44a \
  -F value=197000
  ```

> Request Body:

```json
{
	"rondino_id" : "Your rondino_id",
	"value " : 197000
}
```

> Response Body:

```json
{
	"new_value" : 200000
}
```

### HTTP Request

`POST https://sandbox-api.rondino.ir/client/roundup/`

### Request Body

Parameter | Default | Description
--------- | ------- | -----------
rondino_id |  | Your given rondino_id
value| | Amount of money that you like to be rounded in `Rials`

### Response Body

Parameter | Description
--------- | -----------
new_value | The rounded up value in `Rials`

## RoundUp with Transaction Create

This endpoint round's up your given value and creates a transaction and returns a transaction Id and an order Id which is actually your own order id (in your own shopping system) that we associate with the created transaction for your ease of use. Sending your order id is optional.


> `RoundUp with Transaction Create` Example Request

```curl
curl -X POST \
  http://0.0.0.0:8000/client/roundup/ \
  -F rondino_id=7dc9610cf25e4c0192867827c9e44a \
  -F value=197000 \
  -F order_id=1234567 \
  -F create=1
  ```

> Request Body:

```json
{
	"rondino_id" : "Your rondino_id",
	"value " : 197000,
	"order_id" : "1234567",
	"create" : 1
}
```

> Response Body:

```json
{
	"new_value" : 200000,
	"transaction_id" : 11,
	"order_id" : "1234567"
}
```

### HTTP Request

`POST https://sandbox-api.rondino.ir/client/roundup/`

### Request Body

Parameter | Default | Description
--------- | ------- | -----------
rondino_id |  | Your given rondino_id
value| | Amount of money that you like to be rounded in `Rials`
create|  | Option that differs this endpoint from `Simple Roundup` (Should be 1)
order_id | | Your client's order Id (This field is optional)

### Response Body

Parameter | Description
--------- | -----------
new_value | The rounded up value in `Rials`
transaction_id | Id for the created transaction 
order_id | Your own order_Id (If you do not send order Id, this field will be empty)

<aside class="warning">
Remember — For the `RoundUp with Transaction Create` you have to set the `create` field to 1
</aside>
<aside class="notice">
Order Id is completely optional ! You do not have to send it.
</aside>


# Transaction Operations

In this section we explain API's for commiting and confirming transaction. After confirmation the transaction is final. You can monitor these transactions in your panel area.

## Commit Transaction

This endpoint is for creating a new transactions

<aside class="notice">
If you are using `RoudUp with Transaction Create`, you do not need to use this endpoint
</aside>

> `Commit Transaction` Example Request

```curl
curl -X POST \
  http://0.0.0.0:8000/client/commit/ \
  -F rondino_id=7dc9610cf25e4c0192867827c9e44a \
  -F value=175000 \
  -F order_id=
  ```

> Request Body:

```json
{
	"rondino_id" : "Your rondino_id",
	"value " : 180000,
	"order_id" : "",
}
```

> Response Body:

```json
{
	"transaction_id" : 11,
	"order_id" : ""
}
```

### HTTP Request

`POST https://sandbox-api.rondino.ir/client/commit/`

### Request Body

Parameter | Default | Description
--------- | ------- | -----------
rondino_id |  | Your given rondino_id
value| | Amount of money that you like to be rounded in `Rials`
order_id | | Your client's order Id (This field is optional)

### Response Body

Parameter | Description
--------- | -----------
transaction_id | Id for the created transaction 
order_id | Your own order_Id (If you do not send order Id, this field will be empty)

<aside class="notice">
Order Id is completely optional ! You do not have to send it.
</aside>

## Confirm Transaction

This endpoint is for confirming your transaction status; whether it has failed or succeeded. This endpoint will finalize your transaction.

> `Confirm Transaction` Example Request

```curl
curl -X POST \
  http://0.0.0.0:8000/client/confirm/ \
  -F transaction_id=12 \
  -F transaction_status=1 \
  -F rondino_id=7dc9610cf25e4c0192867827c9e44a
  ```

> Request Body:

```json
{
	"rondino_id" : "Your rondino_id",
	"transaction_id" : 12,
	"transaction_status" : 1,
}
```

> Response Body:

```json
{
	"transaction_id" : 12,
	"transaction_status" : 1
}
```

### HTTP Request

`POST https://sandbox-api.rondino.ir/client/confirm/`

### Request Body

Parameter | Default | Description
--------- | ------- | -----------
rondino_id |  | Your given rondino_id
transaction_id| | Your previuosly created transaction Id
transaction_status | | The status of your transaction: 1 = successfull , 0 = failed

### Response Body

Parameter | Description
--------- | -----------
transaction_id | Id for the confirmed transaction 
transaction_status | | The status of your transaction: 1 = successfull , 0 = failed

## Get Transaction Id

This endpoint is for retrieving your transaction Id using your order Id if you have provided one for your transaction.The perpose of this endpoint and order Id in general is to make life easier for our users ! By using order Id you do not need to save transaction Id between page refreshes in your website.


> `Get Transaction Id` Example Request

```curl
curl -X POST \
  http://0.0.0.0:8000/client/getTransactionId/ \
  -F rondino_id=7dc9610cf25e4c0192867827c9e44a \
  -F order_id=1234567  
  ```


> Request Body:

```json
{
	"rondino_id" : "Your rondino_id",
	"order_id" : "1234567"
}
```

> Response Body:

```json
{
	"transaction_id" : 12,
}
```

### HTTP Request

`POST https://sandbox-api.rondino.ir/client/getTransactionId/`

### Request Body

Parameter | Default | Description
--------- | ------- | -----------
rondino_id |  | Your given rondino_id
order_id | | The order Id you provided upon transaction creation

### Response Body

Parameter | Description
--------- | -----------
transaction_id | Id for the requested transaction 
