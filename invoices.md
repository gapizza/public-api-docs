# Invoices

**Path**: `/api/v1/invoices/search`

**Method**: `POST`

**Auth**: Requires Authorization header with API key

**Full Request**

```javascript
{
  // Filter by created date. Optional and all fields optional
  createdAtFilter: {
    start: '2018-01-01T00:00:00.000Z',
    end: '2018-01-01T00:00:00.000Z',
    order: 'asc' | 'desc',
  },
  // Filter by updated date. Optional and all fields optional
  updatedAtFilter: {
    start: '2018-01-01T00:00:00.000Z',
    end: '2018-01-01T00:00:00.000Z',
    order: 'asc' | 'desc',
  },
  // Cursor-based pagination. optional and all fields optional
  // - Only supply `before` or `after` to move to the previous or next page
  // - They are attached as `prevBefore` and `nextAfter` in the response if there are pages before or after the current one
  pagination: {
    before: '2018-01-01T00:00:00.000Z',
    after: '2018-01-01T00:00:00.000Z',
    limit: 20, // Defaults to 20. Max of 100.
  },
  
  // Filtering by fields. All optional
  customerId: 'some-customer-id',
  status: 'draft' | 'open' | 'paid' | 'uncollectable' | 'void'
  postalCode: 'M5A 0B1',
  postCodePrefix: 'M5A', // The postal FSA
  fulfillmentStatus: 'unfulfilled' | 'partiallyFulfilled' | 'fulfilled'
}

```

**Example request for invoices by updatedAt**

This will return all invoices updated after `2021-10-20T00:00:00.000Z`, 100 at a time.

Because there is no filter for status, you'll have to filter on the client side for invoices that are set to `paid`. 
But may still want to see the remainder in case they have been voided.

```javascript
{
  // Filter by updated date. Optional and all fields optional
  updatedAtFilter: {
    start: '2021-10-20T00:00:00.000Z',
    order: 'asc',
  },
  // Cursor-based pagination. optional and all fields optional
  // - Only supply `before` or `after` to move to the previous or next page
  // - They are attached as `prevBefore` and `nextAfter` in the response if there are pages before or after the current one
  pagination: {
    limit: 100, // Defaults to 20. Max of 100.
  },
}

```


**Response**

```json5
{
  "code": 200,
  "status": "OK",
  "dateTime": "2021-10-21T12:59:23.680Z",
  "timestamp": 1634821163680,
  "data": {
    // List of up to LIMIT orders
    "list": [
      {
        "id": "in_1Jn0lOEAvJFf3sgImgzSf8Ex",
        "amountDue": 4900,
        "amountPaid": 0,
        "amountRemaining": 4900,
        "customerId": "cus_KPjS5NjCG7M1Id",
        "discounts": [],
        "dueDate": null,
        "items": [
          {
            "id": "il_1Jn0lOEAvJFf3sgIXEmJTXst",
            "priceId": "price_1JktyAEAvJFf3sgIqockyD59",
            "qty": 1,
            "bundleDefinitionId": "bndl-def-price_1JktyAEAvJFf3sgIqockyD59",
            "bundleId": "5717ad54-c0b9-432b-8979-9d3b1005c3aa",
            "metadata": null,
            // SKU is null for non-shippable items
            "sku": null
          },
          {
            "id": "il_1Jn0lOEAvJFf3sgIg1MldzAT",
            "priceId": "price_1JktxlEAvJFf3sgI7M1frdAz",
            "qty": 4,
            "bundleDefinitionId": "bndl-def-price_1JktyAEAvJFf3sgIqockyD59",
            "bundleId": "5717ad54-c0b9-432b-8979-9d3b1005c3aa",
            "metadata": null,
            "sku": "205"
          }
        ],
        "nextPaymentAttemptAt": null,
        "paid": false,
        "pdfLink": null,
        "status": "draft",
        "subtotal": 4900,
        "tax": 0,
        "total": 4900,
        // Delivery date in UTC
        "deliveryDate": "2021-10-24T12:52:48.698Z",
        "subscriptionIds": [],
        // Destination address with name and validated phone number
        "shippingAddress": {
          "id": "ship-add-kSpIy8N5jIDd8M7IO5FgM",
          "address": {
            "city": "Toronto",
            "country": "CA",
            "line1": "13 Millington St",
            "line2": "",
            "postalCode": "M4X 3A1",
            "postalCodePrefix": "M4X",
            "state": "ontario"
          },
          "name": "Eric Hacke",
          "phone": "+16472009402",
          "customerId": "cus_KPjS5NjCG7M1Id",
          "createdAt": "2021-10-19T14:30:14.901Z",
          "updatedAt": "2021-10-19T14:30:14.901Z"
        },
        // This structure indicates whether it goes out by Tyltgo or others
        "shippingMethod": {
          "id": "ship-meth-JNeYis2GdCMAohlRsv1oR",
          "fulfillmentProvider": "dnet",
          "carrier": "tyltgo", // Options are `tyltgo` | `pickup` | `special_delivery`
          "createdAt": "2021-10-14T00:00:00.000Z",
          "updatedAt": "2021-10-14T00:00:00.000Z"
        },
        // Updated fulfillment status based on the events sent to the fulfillment API
        "fulfillmentStatus": "unfulfilled",
        "paymentMethod": {
          "id": "pm_1JmJKQEAvJFf3sgIoLHw7wDX",
          "card": {
            "brand": "visa",
            "country": "US",
            "expiryMonth": 4,
            "expiryYear": 2056,
            "last4": "4242"
          },
          "billingDetails": {
            "name": "Eric Hacke",
            "email": "olive.algae.867@example.com",
            "phone": null,
            "address": {
              "city": "Toronto",
              "country": "CA",
              "line1": "13 Millington St",
              "line2": null,
              "postalCode": "M4X 3A1",
              "postalCodePrefix": "M4X",
              "state": "ontario"
            }
          },
          "type": "card",
          "customerId": "cus_KPjS5NjCG7M1Id",
          "createdAt": "2021-10-19T14:30:15.590Z",
          "updatedAt": "2021-10-19T14:30:15.590Z"
        },
        "createdAt": "2021-10-21T12:52:53.877Z",
        "updatedAt": "2021-10-21T12:52:53.877Z"
      }
    ]
  }
}
```
