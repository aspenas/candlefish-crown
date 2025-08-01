# Crown Trophy API Reference

## Base URL
- Development: `http://localhost:3000/api`
- Production: `https://api.crowntrophy.candlefish.ai`

## Authentication
All API requests require authentication via API key:
```
X-API-Key: your-api-key-here
```

## Endpoints

### Document Processing

#### POST /documents/extract
Extract engraving data from uploaded files

**Request:**
```json
{
  "file": "base64-encoded-file",
  "filename": "golden_marlins.xlsx",
  "type": "engraving_list",
  "options": {
    "confidence_threshold": 0.95,
    "validate_names": true
  }
}
```

**Response:**
```json
{
  "extraction_id": "ext_123456",
  "status": "success",
  "confidence": 0.98,
  "data": {
    "total_items": 276,
    "constant_text": ["Golden Marlins", "2025"],
    "variable_data": [
      {
        "line1": "Anna Grace Chapman",
        "line2": "Golden Marlins",
        "line3": "2025"
      }
    ]
  },
  "processing_time_ms": 2341
}
```

#### POST /layout/generate
Generate optimized layout for laser cutting

**Request:**
```json
{
  "extraction_id": "ext_123456",
  "material": {
    "type": "acrylic",
    "size": "12x24",
    "thickness": "1/8"
  },
  "plate_size": {
    "width": 3.25,
    "height": 0.75
  }
}
```

**Response:**
```json
{
  "layout_id": "lay_789012",
  "sheets_required": 3,
  "utilization_percent": 94.5,
  "pdf_url": "https://storage.../layout_789012.pdf",
  "waste_area": 1.38
}
```

### Order Management

#### POST /orders
Create a new order

**Request:**
```json
{
  "customer_id": "cust_123",
  "location_id": "loc_littleton",
  "items": [
    {
      "product_id": "prod_trophy_8inch",
      "quantity": 50,
      "engraving_data": {
        "extraction_id": "ext_123456"
      }
    }
  ]
}
```

#### GET /orders/{id}
Get order details

#### PUT /orders/{id}/status
Update order status

**Request:**
```json
{
  "status": "processing",
  "notes": "Sent to laser engraver"
}
```

### Inventory Management

#### GET /inventory
Get current inventory levels

**Query Parameters:**
- `location_id`: Filter by location
- `sku`: Filter by SKU
- `low_stock`: Show only low stock items

#### POST /inventory/adjust
Adjust inventory levels

**Request:**
```json
{
  "adjustments": [
    {
      "sku": "PLATE_CHERRY_8X10",
      "location_id": "loc_littleton",
      "quantity_change": -10,
      "reason": "order_fulfillment",
      "order_id": "ord_123456"
    }
  ]
}
```

#### POST /inventory/transfer
Transfer between locations

**Request:**
```json
{
  "from_location": "loc_arvada",
  "to_location": "loc_littleton",
  "items": [
    {
      "sku": "CRYSTAL_APEX",
      "quantity": 5
    }
  ]
}
```

### Vendor Management

#### POST /vendors/optimize
Get optimal vendor selection for items

**Request:**
```json
{
  "items": [
    {
      "sku": "PLATE_CHERRY_8X10",
      "quantity": 50
    }
  ],
  "location_id": "loc_littleton",
  "consider_freight": true
}
```

**Response:**
```json
{
  "recommendations": [
    {
      "vendor": "JDS",
      "items": ["PLATE_CHERRY_8X10"],
      "subtotal": 125.00,
      "freight": 0,
      "total": 125.00,
      "freight_minimum_met": true
    }
  ],
  "total_cost": 125.00,
  "savings": 15.00
}
```

## Webhooks

### Order Status Updates
```json
{
  "event": "order.status_changed",
  "order_id": "ord_123456",
  "old_status": "pending",
  "new_status": "processing",
  "timestamp": "2025-07-25T10:30:00Z"
}
```

### Inventory Alerts
```json
{
  "event": "inventory.low_stock",
  "sku": "PLATE_CHERRY_8X10",
  "location_id": "loc_littleton",
  "current_quantity": 5,
  "reorder_point": 10
}
```

## Error Responses

### 400 Bad Request
```json
{
  "error": "INVALID_REQUEST",
  "message": "Missing required field: customer_id",
  "details": {
    "field": "customer_id",
    "required": true
  }
}
```

### 401 Unauthorized
```json
{
  "error": "UNAUTHORIZED",
  "message": "Invalid API key"
}
```

### 429 Rate Limited
```json
{
  "error": "RATE_LIMITED",
  "message": "Too many requests",
  "retry_after": 60
}
```

### 500 Internal Server Error
```json
{
  "error": "INTERNAL_ERROR",
  "message": "An unexpected error occurred",
  "request_id": "req_abc123"
}
```

## Rate Limits
- Document processing: 100 requests/minute
- API calls: 1000 requests/minute
- File uploads: 10GB/day

## SDKs
Coming soon:
- JavaScript/TypeScript
- Python
- C# (for QuickBooks integration)