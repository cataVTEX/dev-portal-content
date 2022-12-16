---
title: "Updating SKU Specifications with Catalog API"
slug: "updating-sku-specifications-with-catalog-api"
hidden: false
createdAt: "2020-10-22T22:10:57.479Z"
updatedAt: "2020-10-28T23:18:46.756Z"
---
In our Catalog, [specifications](https://help.vtex.com/tracks/catalog-101--5AF0XfnjfWeopIFBgs3LIQ/2NQoBv8m4Yz3oQaLgDRagP#) are created in the category level. Products and SKUs inherit their specifications from the category they are placed in and all its parent categories. This may lead to some confusion when using our API to create and update SKUs. 

In this article, we share a few tips to help you update an [SKU Specification](https://help.vtex.com/tracks/catalog-101--5AF0XfnjfWeopIFBgs3LIQ/2NQoBv8m4Yz3oQaLgDRagP#sku-specifications).

### SKU and its Specification
First, you need to select an SKU and its Specification to update. If you don’t know the Specifications of the SKU, use the [Get SKU Specifications](ref:catalog-api-get-sku-specification) endpoint. It will retrieve all Specifications indexed on the SKU.

In the example below, we show what the response would look like if your SKU had two specifications to fill in: 

- Color (`"FieldId": 271`)
- Size (`"FieldId": 40`)

If needed, you would be able to get more information about each specification using the [Get Specification Field](ref:catalog-api-get-specification-field) endpoint.
[block:code]
{
  "codes": [
    {
      "code": "[\n    {\n        \"Id\": 472,\n        \"SkuId\": 17784,\n        \"FieldId\": 271,\n        \"FieldValueId\": null,\n        \"Text\": \"\"\n    },\n  \t{\n        \"Id\": 528,\n        \"SkuId\": 17784,\n        \"FieldId\": 40,\n        \"FieldValueId\": 147,\n        \"Text\": \"L\"\n    }\n]",
      "language": "json",
      "name": "Example: Get SKU Specifications"
    }
  ]
}
[/block]

[block:callout]
{
  "type": "warning",
  "body": "Notice that the response body from this endpoint is an array but you can’t update all at once. You can update only one SKU Specification at a time."
}
[/block]
### Knowing the `FieldValueId`

After selecting the one Specification to update, use the [Get Specifications Values By Field Id](ref:catalog-api-get-specification-field-value-fieldid) endpoint. It allows you to know all the possible values from this Specification as you can only update the `FieldValueId` from an SKU Specification.

In the example below, we show what the response would look like if your specification had three possible values: 

- S (`"FieldValueId": 144`)
- M (`"FieldValueId": 145`)
- L (`"FieldValueId": 146`)
[block:code]
{
  "codes": [
    {
      "code": "[\n    {\n        \"FieldValueId\": 144,\n        \"Value\": \"S\",\n        \"IsActive\": true,\n        \"Position\": 1\n    },\n    {\n        \"FieldValueId\": 145,\n        \"Value\": \"M\",\n        \"IsActive\": true,\n        \"Position\": 2\n    },\n    {\n        \"FieldValueId\": 147,\n        \"Value\": \"L\",\n        \"IsActive\": true,\n        \"Position\": 3\n    }\n]",
      "language": "json",
      "name": "Example: Get Specifications Values By Field Id"
    }
  ]
}
[/block]
### Updating the SKU Specification `FieldValueId`

Finally, use the [Update SKU Specification](ref:put_api-catalog-pvt-stockkeepingunit-skuid-specification) endpoint to change the `FieldValueId` from the SKU Specification to the desired new value.
[block:callout]
{
  "type": "warning",
  "body": "SKU Specifications can only have the `FieldType` as `5`(Combo) or `6`(Radio). You cannot change the `Text` field without updating the `FieldValueId`. After this update, the `Text` field will be automatically modified."
}
[/block]