---
title: Zeemart APIs

language_tabs: # must be one of https://git.io/vQNgJ
  - shell

toc_footers:
  - <a href='https://github.com/lord/slate'>Documentation Powered by Slate</a>

includes:
  - errors

search: true
---

# Introduction

Welcome to the Zeemart API! You can use our API to access Zeemart API endpoints, which can get information on various products related to buyers and suppliers in our database.

We have language bindings in Java! You can view code examples in the dark area to the right, and you can switch the programming language of the examples with the tabs in the top right.
# Users

## Create User
```shell
curl "https://staging-zm-authserv.herokuapp.com/services/user"
  -H "authType: Zeemart" -H "mudra: mudra-token" -H "supplierId: SupplierId" //Dicussion for param name
```
Allows authorized admins to create new users for their Outlets or Suppliers. Takes users basic details and register email as ZeemartId. If user’s email is already available as ZeemartId, it will update the basic details and add the Outlet/Supplier code. By default, the newly created user is Deactivated.

If the creation is successful, the system will send an email to registered email with the verification code to activate the new user account.  

### HTTP Request
`POST https://staging-zm-authserv.herokuapp.com/services/user`
> Request body for the above request is structured like this (JSON):

```json
{
  "ZeemartId": "newuser@zeemart.asia",
  "outletId": ["OAAAA"],
  "supplierId": ["SAAAA"],
  "communicationId": "communicate@supplier.com",
  "firstName": "Roy",
  "lastName": "Wu",
  "position": "Outlet manager",
  "phoneNumber": "+1234567890",
  "imageURL": "https://www.clinicaledge.co/img/blank_user.jpg"
}
```
#### Request Headers
Header | Value | Description
--------- | -------- | ------------
Content-Type | application/json | Fixed value
authType | Zeemart | Fixed Value
mudra | mudra-token | Taken from Login
supplierId | SupplierId | OutletId or Supplier TeamId

#### Request Body description
Parameter Name | Value | Mandatory? | Description
--------- | --------- | ---------- | --------
ZeemartId | newuser@zeemart.asia | Y | new User's ZeemartId <email>
outletId | ["OAAAA"] | Y | if Account type is selected as Buyer then mandatory
supplierId | ["SAAAA"]	| Y |	if Account type is selected as Supplier then mandatory
firstName | "Roy" |	Y	| First name of the new user
lastName | "Wu" | Y |	Last name of new user
position | "Assistant Manager" | Y | Position of the user
phoneNumber |	"+1234567890" | N | Mobile phone
imageURL | "https://www.clinicaledge.co/img/blank_user.jpg" | N | Link to uploaded profile image

### Response
> Success response body for the above request is structured like this (JSON):

```json
{
  "status": "success"
}
```

> Failure response body for the above request is structured like this (JSON):

```json
{
	"status": "error",
	"reason": "Reason of failure"
}
```

#### Error codes

Error Code | Reason
----------- | -----------
401 | if authentication token not matched
403 | If permissions are denied for this user for the action


## View Users
```shell
curl "https://staging-zm-authserv.herokuapp.com/services/users"
  -H "authType: Zeemart" -H "mudra: mudra-token" -H "supplierId: SupplierId" //Dicussion for param name
```
To list down all the users or filter out the users or paginate the users.  

### HTTP Request
Without query parameters
`GET https://staging-zm-authserv.herokuapp.com/services/users`

with sample query parameters
`GET https://staging-zm-authserv.herokuapp.com/services/users?searchName=ajan&outlets=outletId1,outletId2&teams=suplierId1,supplierId2`

#### Request Headers
Header | Value | Description
------ | ----- | -----------
Content-Type | application/json | Fixed value
authType | Zeemart | Fixed Value
mudra | mudra-token | Taken from Login
supplierId | SupplierId | OutletId or SupplierId

#### Request Query description
Parameter Name | Value | Mandatory? | Description
-------------- | ----- | ---------- | -----------
searchName | "Ajan" | N | Search key for search by firstName and lastName
outlets | "OAAAA,OAAAB" | N | Comma separated OutletIds
suppliers | "SAAAA,SAAAB"  | N | Comma separated SupplierIds
status | "A" | N | Values ["A", "D", "I"]
orderBY | "firstName"	| N |	OrderBy column [firstName,lastName]
sortOrder | "asc" |	N	| Values [asc,desc]
startPage | 1 | N |	If pagination start page number
pageSize | 25 | N | Number of records per page

### Response
> Success response body for the above request is structured like this (JSON):

```json
[{
		"_id": {
			"id": "59d1f28a8f715ee9af02dfba"
		},
		"authType": "Zeemart",
		"outletId": ["OAAAA"],
		"supplierId": ["SAAAA"],
		"firstName": "Roy",
		"lastName": "Wu",
		"status": "A",
		"phoneNumber": "+1234567890",
		"position": "Outlet manager",
		"imageURL": "https://www.clinicaledge.co/img/blank_user.jpg"
	},
	{
		"_id": {
			"id": "59d1f28a8f715ee9af02dfbc"
		},
		"authType": "Zeemart",
		"outletId": ["OAAAA"],
		"supplierId": ["SAAAA"],
		"firstName": "Ajay",
		"lastName": "Siva",
		"status": "A",
		"phoneNumber": "+1234567890",
		"position": "Outlet manager",
		"imageURL": "https://www.clinicaledge.co/img/blank_user.jpg"
	}
]
```

> Failure response body for the above request is structured like this (JSON):

```json
{
	"status": "error",
	"reason": "Reason of failure"
}
```

#### Error codes

Error Code | Reason
----------- | -----------
401 | if authentication token not matched
403 | If permissions are denied for this user for the action



## View Specific User
```shell
curl "https://staging-zm-authserv.herokuapp.com/services/user/ZeemartId=user@zeemart.asia"
  -H "authType: Zeemart" -H "mudra: mudra-token" -H "supplierId: SupplierId" //Dicussion for param name
```
To get the specific User details

### HTTP Request

`GET https://staging-zm-authserv.herokuapp.com/services/user/ZeemartId=user@zeemart.asia`

#### Request Headers
Header | Value | Description
------ | ----- | -----------
Content-Type | application/json | Fixed value
authType | Zeemart | Fixed Value
mudra | mudra-token | Taken from Login
supplierId | SupplierId | OutletId or SupplierId

#### Request Query description
Parameter Name | Value | Mandatory? | Description
-------------- | ----- | ---------- | -----------
ZeemartId | "user@zeemart.asia" | Y | Get userby Zeemart Id

### Response
> Success response body for the above request is structured like this (JSON):

```json
{
		"_id": {
			"id": "59d1f28a8f715ee9af02dfba"
		},
		"authType": "Zeemart",
		"outletId": ["OAAAA"],
		"supplierId": ["SAAAA"],
		"firstName": "Userwith ZeemartID",
		"lastName": "Wu",
		"status": "A",
		"phoneNumber": "+1234567890",
		"position": "Outlet manager",
		"imageURL": "https://www.clinicaledge.co/img/blank_user.jpg"
	}
```

> Failure response body for the above request is structured like this (JSON):

```json
{
	"status": "error",
	"reason": "Reason of failure"
}
```

#### Error codes

Error Code | Reason
----------- | -----------
401 | if authentication token not matched
403 | If permissions are denied for this user for the action


## Update / Edit User
```shell
curl "https://staging-zm-authserv.herokuapp.com/services/user"
  -H "authType: Zeemart" -H "mudra: mudra-token" -H "supplierId: SupplierId" //Dicussion for param name
```
Update the existing user's details  

### HTTP Request
`PUT https://staging-zm-authserv.herokuapp.com/services/user`
> Request body for the above request is structured like this (JSON):

```json
{
  "ZeemartId": "newuser@zeemart.asia",
  "outletId": ["OAAAA"],
  "supplierId": ["SAAAA"],
  "communicationId": "communicate@supplier.com",
  "firstName": "Roy",
  "lastName": "Wu",
  "position": "Outlet manager",
  "phoneNumber": "+1234567890",
  "imageURL": "https://www.clinicaledge.co/img/blank_user.jpg"
}
```
#### Request Headers
Header | Value | Description
--------- | -------- | ------------
Content-Type | application/json | Fixed value
authType | Zeemart | Fixed Value
mudra | mudra-token | Taken from Login
supplierId | SupplierId | OutletId or Supplier TeamId

#### Request Body description
Parameter Name | Value | Mandatory? | Description
--------- | --------- | ---------- | --------
ZeemartId | newuser@zeemart.asia | Y | new User's ZeemartId <email>
outletId | ["OAAAA"] | Y | Linked OutletIds
supplierId | ["SAAAA"]	| Y |	Linked SupplierIds
firstName | "Roy1" |	Y	| First name of the new user
lastName | "Wu1" | Y |	Last name of new user
position | "Assistant Manager" | Y | Position of the user
phoneNumber |	"+1234567890" | N | Mobile phone
imageURL | "https://www.clinicaledge.co/img/blank_user.jpg" | N | Link to uploaded profile image

### Response
> Success response body for the above request is structured like this (JSON):

```json
{
  "status": "success"
}
```

> Failure response body for the above request is structured like this (JSON):

```json
{
	"status": "error",
	"reason": "Reason of failure"
}
```

#### Error codes

Error Code | Reason
----------- | -----------
401 | if authentication token not matched
403 | If permissions are denied for this user for the action


## Delete User
```shell
curl "https://staging-zm-authserv.herokuapp.com/services/user?ZeemartID=user@zeemart.asia"
  -H "authType: Zeemart" -H "mudra: mudra-token" -H "supplierId: SupplierId" //Dicussion for param name
```
Update the existing user's details  

### HTTP Request
`DELETE https://staging-zm-authserv.herokuapp.com/services/user?ZeemartID=user@zeemart.asia`
> Request body for the above request is structured like this (JSON):

#### Request Headers
Header | Value | Description
--------- | -------- | ------------
Content-Type | application/json | Fixed value
authType | Zeemart | Fixed Value
mudra | mudra-token | Taken from Login
supplierId | SupplierId | OutletId or Supplier TeamId

#### Request Query description
Parameter Name | Value | Mandatory? | Description
--------- | --------- | ---------- | --------
ZeemartId | newuser@zeemart.asia | Y | new User's ZeemartId <email>

### Response
> Success response body for the above request is structured like this (JSON):

```json
{
  "status": "success"
}
```

> Failure response body for the above request is structured like this (JSON):

```json
{
	"status": "error",
	"reason": "Reason of failure"
}
```

#### Error codes

Error Code | Reason
----------- | -----------
401 | if authentication token not matched
403 | If permissions are denied for this user for the action







# Products

## Create Product

```shell
curl "https://staging-zm-inventory.herokuapp.com/services/product"
  -H "authType: Zeemart" -H "mudra: mudra-token" -H "supplierId: SupplierId"

```

This endpoint creates new Product(s).

### HTTP Request

`POST https://staging-zm-inventory.herokuapp.com/services/product`

> Request body for the above command structured like this (JSON):

```json

[
    {
        "productName": "Best Beef Ribeye",
        "brand": "Best",
        "supplierProductCode": "AC1234567",
        "imageFileNames": [
            "abc.jpg",
            "xyz.jpg"
        ],
        "imageURL": <imageServerURL> + "/uploads/<supplierId>/Zeemart-<unixTimeStamp>/",
        "chargeBy": {
            "unitSize": "Kilogram",
            "guidedPrice": {
                "comparison": "Range",
                "minPrice": 12.5,
                "maxPrice": 23.6
            }
        },
        "orderBy": [
            {
                "unitSize": "Kilogram",
                "moq": 34
            },
            {
                "unitSize": "Piece",
                "moq": 30
            },
            {
                "unitSize": "Pack",
                "subUnitSize": "Piece",
                "subUnitQuantity": 10,
                "moq": 10
            }
        ],
		"mainCategoryId": "1000001",
        "categoryId": "3000001",
        "categoryPath": "Food:Vegetables:Pumpkin",
        "attributes": [
			{"cut": ["Whole"]},
            {"Halal": ["Halal"]},
			{"State": ["Roasted","InstantPowder"]},
			{"countryOfOrigin": ["Singapore"]},
        	{"regionOfOrigin": ["Singapore"]}
        ],
        "description": "Best Product in the world",
        "upcEanNumber": "Z12345678",
		"tags": [
            "Food",
            "Vegetables"
        ]
    }
]

```

### Headers

Header | Value
--------- | -------
Content-Type | application/json
authType | Zeemart
mudra | mudra-token
supplierId | SupplierId

mudra-token would be taken from login

<b>Other options for charge By are</b>

Exactly:

"chargeBy": {
    "unitSize": "Kilogram",
    "guidedPrice": {
        "comparison": "Exactly",
        "price": 12.5
    }
}

Don’t Show:

"chargeBy": {
    "unitSize": "Kilogram",
    "guidedPrice": {
        "comparison": "Dontshow"
    }
}

## Edit Product

```shell
curl "https://staging-zm-inventory.herokuapp.com/services/product"
  -H "authType: Zeemart" -H "mudra: mudra-token" -H "supplierId: SupplierId"

```

This endpoint edits Product(s).

### HTTP Request

`PUT https://staging-zm-inventory.herokuapp.com/services/product`

> Request body for the above command structured like this (JSON):

```json

[
    {
        "sku": "<sku>",
        "supplierId": "SAAAA",
        "productName": "Best Beef Ribeye",
        "brand": "Best",
        "supplierProductCode": "AC1234567",
        "imageFileNames": [
            "abc.jpg",
            "xyz.jpg"
        ],
        "imageURL": <imageServerURL> + "/uploads/<supplierId>/Zeemart-<unixTimeStamp>/",
        "chargeBy": {
            "unitSize": "Kilogram",
            "guidedPrice": {
                "comparison": "Range",
                "minPrice": 12.5,
                "maxPrice": 23.6
            }
        },
        "orderBy": [
            {
                "unitSize": "Kilogram",
                "moq": 34
            },
            {
                "unitSize": "Piece",
                "moq": 30
            },
            {
                "unitSize": "Pack",
                "subUnitSize": "Piece",
                "subUnitQuantity": 10,
                "moq": 10,
            }
        ],
		"mainCategoryId": "1000001",
        "categoryId": "3000001",
        "categoryPath": "Food:Vegetables:Pumpkin",
        "attributes": [
            {"cut": ["Whole"]},
            {"Halal": ["Halal"]},
			{"State": ["Roasted","InstantPowder"]},
			{"countryOfOrigin": ["Singapore"]},
        	{"regionOfOrigin": ["Singapore"]}
        ],
        "description": "Best Product in the world",
        "upcEanNumber": "Z12345678",
        "tags": [
            "Food",
            "Vegetables"
        ],
		"status": "<status>"
    }
]

```

### Headers

Header | Value
--------- | -------
Content-Type | application/json
authType | Zeemart
mudra | mudra-token
supplierId | SupplierId

mudra-token would be taken from login

<b>Other options for charge By are</b>

Exactly:

"chargeBy": {
    "unitSize": "Kilogram",
    "guidedPrice": {
        "comparison": "Exactly",
        "price": 12.5
    }
}

Don’t Show:

"chargeBy": {
    "unitSize": "Kilogram",
    "guidedPrice": {
        "comparison": "Dontshow"
    }
}

## Delete Product

```shell
curl "https://staging-zm-inventory.herokuapp.com/services/product"
  -H "authType: Zeemart" -H "mudra: mudra-token" -H "supplierId: SupplierId"

```

This endpoint deletes Product(s).

### HTTP Request

`DELETE https://staging-zm-inventory.herokuapp.com/services/product`

> Request body for the above command structured like this (JSON):

```json

[
    {
        "sku": "<sku>",
    }
]

```

### Headers

Header | Value
--------- | -------
Content-Type | application/json
authType | Zeemart
mudra | mudra-token
supplierId | SupplierId

mudra-token would be taken from login

## Visible/ Invisible Product

```shell
curl "https://staging-zm-inventory.herokuapp.com/services/product/status"
  -H "authType: Zeemart" -H "mudra: mudra-token" -H "supplierId: SupplierId"

```

This endpoint deletes Product(s).

### HTTP Request

`PUT https://staging-zm-inventory.herokuapp.com/services/product/status`

> Request body for the above command structured like this (JSON):

```json

[
    {
        "sku": "<sku>",
        "status": "invisible"
    }
]

```

### Headers

Header | Value
--------- | -------
Content-Type | application/json
authType | Zeemart
mudra | mudra-token
supplierId | SupplierId

mudra-token would be taken from login

<b>Note:</b> Status could be “invisible/visible”

## Get All Products list view

```shell
curl "https://staging-zm-inventory.herokuapp.com/services//services/products?status=<productStatus>&pageNumber=<pageNumber>&pageSize=<pageSize>&sortField=<sortField>&sortOrder=<sortOrder>"
  -H "authType: Zeemart" -H "mudra: mudra-token" -H "supplierId: SupplierId"

```

This endpoint retrieves all Products.

### HTTP Request

`GET https://staging-zm-inventory.herokuapp.com/services//services/products?status=<productStatus>&pageNumber=<pageNumber>&pageSize=<pageSize>&sortField=<sortField>&sortOrder=<sortOrder>`

> The above command returns JSON structured like this:

```json

{
    "numberOfPages": 33,
    "numberOfRecords": 3212,
    "products": [
    {
        "sku": "<sku>",
        "productName": "Best Beef Ribeye",
        "supplierProductCode": "AC1234567",
        "chargeBy": {
            "unitSize": "Kilogram",
            "guidedPrice": {
                "comparison": "Range",
                "minPrice": 12.5,
                "maxPrice": 23.6
            }
        },
        "categoryPath": "Food:Vegetables:Pumpkin",
	  "status": "<status>"
        ]
    },
    {
        "sku": "<sku>",
        "productName": "Best Beef Ribeye",
        "supplierProductCode": "AC1234567",
        "chargeBy": {
            "unitSize": "Kilogram",
            "guidedPrice": {
                "comparison": "Range",
                "minPrice": 12.5,
                "maxPrice": 23.6
            }
        },
        "categoryPath": "Food:Vegetables:Pumpkin",
	  "status": "<status>"
        ]
    }
    ]
}

```

### Headers

Header | Value
--------- | -------
Content-Type | application/json
authType | Zeemart
mudra | mudra-token
supplierId | SupplierId

mudra-token would be taken from login

### Query Parameters

Parameter | Default | Description
--------- | ------- | -----------
pageNumber | 1 | Page number for which data needs to retrieve.
pageSize | 100 | Page size.
sortField | productCode | field on which user want to sort the list, e.g. supplierProductCode or productName.
sortOrder | desc | order of sorting “asc/desc”.
productStatus | ALL | if “ALL”, API will return all the products which are not deleted. If “Deleted”, then only deleted products would be returned.


## Get a Specific Product

```shell
curl "https://staging-zm-inventory.herokuapp.com/services/product?sku=<sku>"
-H "authType: Zeemart" -H "mudra: mudra-token" -H "supplierId: SupplierId"

mudra-token would be replaced by the mudra coming from login
```

This endpoint retrieves all Products.

### HTTP Request

`GET https://staging-zm-inventory.herokuapp.com/services/product?sku=<sku>`

> The above command returns JSON structured like this:

```json

{
	"supplierId": "SAAAA",
	"productName": "Best Beef Ribeye",
	"brand": "Best",
	"supplierProductCode": "AC1234567",
	"imageFileNames": [
		"abc.jpg",
		"xyz.jpg"
	],
	"imageURL": "https://images.zeemart.asia/uploads/<supplierId>/Zeemart-<unixTimeStamp>/",
	"chargeBy": {
		"unitSize": "Kilogram",
		"guidedPrice": {
			"comparison": "Range",
			"MinPrice": 12.5,
			"MaxPrice": 23.6,
		}
	},
	"orderBy": [
		{
			"unitSize": "Kilogram",
			"moq": 34,
		},
		{
			"unitSize": "Piece",
			"moq": 30,
		},
		{
			"unitSize": "Pack",
			"subUnitSize": "Piece",
			"subUnitQuantity": 10,
			"moq": 10,
		}
	],
	"categoryId": "3000001",
	"categoryPath": "Food/Vegetables/Pumpkin",
	"attributes": {
		"cut": ["Whole"],
		"Halal": ["https://image.zeemart.asia/uploads/<supplierId>/Zeemart-<unixTimeStamp>/HalalCertificate.jpg"],
		"countryOfOrigin": ["Singapore"],
		"regionOfOrigin": ["Singapore"]
	},
	"descripion": "Best Product in the world",
	"upcEanNumber": "Z12345678",
	"tags": [
		"Food",
		"Vegetables"
	]
}

```

### Headers

Header | Value
--------- | -------
Content-Type | application/json
authType | Zeemart
mudra | mudra-token
supplierId | SupplierId

mudra-token would be taken from login

### Query Parameters

Parameter | Default | Description
--------- | ------- | -----------
sku | null | sku for which details needed

# Market List
