# FulfillmentEvents

**Path**: `/api/v1/fulfillmentEvents`

**Method**: `POST`

**Auth**: Requires Authorization header with API key

**Full Request**

Push a list of `fulfillmentEvents` to update the status of the identified invoices.

Each item in the array within the `fulfillmentEvents` includes a SKU and quantity.

Updating all items in the invoice to `fulfilled` will update the associated invoice to fulfilled.

You can repeatedly update the same items for the same invoice, and the latest event is considered the current status.

```javascript
{
  fulfillmentEvents: [
    {
      // ID of the invoice to update the status for
      invoiceId: 'in_1JkMYtEAvJFf3sgI0jqcy2EC',
      
      // The status to be applied to all items in the list
      status: 'fulfilled',
      
      // How it's being delivered
      carrier: 'tyltgo', // Options are 'tyltgo' | 'pickup' | 'specialDelivery' 
      
      // Array of tracking numbers (optional)
      trackingNumbers: string[];
      
      // Array of tracking URLs (optional)
      trackingUrls: string[];
      
      // Estimatated delivery date (optional)
      estimatedDeliveryDate: Date | null;
      
      items: [
        {
          sku: 'fosajifdwfedas',
          qty: 2,
        },
      ],  
    },
  ],
}
```

**Response**

```json5
{
  "code": 200,
  "status": "OK",
  "dateTime": "2021-10-21T12:59:23.680Z",
  "timestamp": 1634821163680,
}
```
