# Crown Trophy - Technical Architecture

## System Overview

The Crown Trophy automation system is designed as a hybrid desktop/cloud application that processes customer orders, automates engraving layouts, manages inventory, and optimizes vendor selection using AI-powered document intelligence.

## Architecture Principles

1. **Desktop-First**: Windows desktop application with cloud sync
2. **AI-Powered**: Leverage latest LLMs for document understanding
3. **Real-Time**: Instant updates across locations
4. **Offline-Capable**: Core functions work without internet
5. **Scalable**: Support 150+ franchise locations

## High-Level Architecture

```mermaid
graph TB
    subgraph "Client Layer"
        DA[Desktop App<br/>Electron/Tauri]
        WD[Web Dashboard<br/>Next.js]
        EM[Email Monitor<br/>Python Service]
    end
    
    subgraph "API Gateway"
        AG[FastAPI Gateway<br/>Rate Limiting, Auth]
    end
    
    subgraph "Core Services"
        DS[Document Service<br/>GPT-5, LangChain]
        OS[Order Service<br/>Business Logic]
        IS[Inventory Service<br/>Stock Management]
        VS[Vendor Service<br/>Optimization AI]
    end
    
    subgraph "Data Layer"
        PG[(PostgreSQL<br/>Primary DB)]
        RD[(Redis<br/>Cache)]
        S3[S3 Compatible<br/>File Storage]
    end
    
    subgraph "Integrations"
        QB[QuickBooks<br/>Desktop]
        CD[Corel Draw<br/>Automation]
        LS[Laser Software<br/>Various]
    end
    
    DA --> AG
    WD --> AG
    EM --> AG
    
    AG --> DS
    AG --> OS
    AG --> IS
    AG --> VS
    
    DS --> PG
    OS --> PG
    IS --> PG
    VS --> PG
    
    DS --> RD
    OS --> RD
    
    DS --> S3
    
    OS --> QB
    DS --> CD
    DS --> LS
```

## Component Details

### 1. Document Intelligence Service

**Purpose**: Extract and understand data from customer files

**Technology Stack**:
- **Primary AI**: OpenAI GPT-5 API ($0.02/1000 pages)
- **Framework**: LangChain 2.0 for orchestration
- **Parsers**: 
  - PyMuPDF for PDFs
  - OpenPyXL for Excel
  - python-docx for Word
- **Storage**: S3 for processed documents

**Key Features**:
- Multi-format support (PDF, Excel, Word, Images)
- Pattern recognition for engraving data
- Confidence scoring for extractions
- Automatic layout optimization

**Data Flow**:
```python
# Pseudo-code for document processing pipeline
async def process_document(file: UploadFile):
    # 1. Extract content
    content = await extract_content(file)
    
    # 2. Send to GPT-5 for understanding
    analysis = await gpt5_analyze(content, 
        prompt="Extract engraving data...")
    
    # 3. Validate and structure data
    structured_data = validate_extraction(analysis)
    
    # 4. Generate optimized layout
    layout = optimize_layout(structured_data, 
        material_size="12x24")
    
    # 5. Create PDF for laser
    pdf = generate_pdf(layout)
    
    return {
        "data": structured_data,
        "layout": layout,
        "pdf_url": upload_to_s3(pdf)
    }
```

### 2. Order Management Service

**Purpose**: Handle order lifecycle from creation to completion

**Technology Stack**:
- **Framework**: Node.js with Express/Fastify
- **ORM**: Prisma for PostgreSQL
- **Queue**: Bull for job processing
- **WebSocket**: Socket.io for real-time updates

**Database Schema**:
```sql
-- Core order tables
CREATE TABLE orders (
    id UUID PRIMARY KEY,
    customer_id UUID REFERENCES customers(id),
    location_id UUID REFERENCES locations(id),
    status ENUM('draft', 'pending', 'processing', 'complete'),
    total_amount DECIMAL(10,2),
    created_at TIMESTAMP,
    updated_at TIMESTAMP
);

CREATE TABLE order_items (
    id UUID PRIMARY KEY,
    order_id UUID REFERENCES orders(id),
    product_id UUID REFERENCES products(id),
    quantity INTEGER,
    engraving_data JSONB,
    unit_price DECIMAL(10,2)
);

CREATE TABLE order_status_history (
    id UUID PRIMARY KEY,
    order_id UUID REFERENCES orders(id),
    status VARCHAR(50),
    changed_by UUID REFERENCES users(id),
    changed_at TIMESTAMP,
    notes TEXT
);
```

### 3. Inventory Management Service

**Purpose**: Track stock levels and automate reordering

**Key Features**:
- Real-time stock tracking
- Multi-location inventory
- Predictive reordering
- Transfer management

**Architecture**:
```typescript
interface InventoryService {
  // Track stock changes
  updateStock(sku: string, location: string, change: number): Promise<void>
  
  // Check availability across locations
  checkAvailability(sku: string, quantity: number): Promise<LocationStock[]>
  
  // Generate reorder suggestions
  getReorderSuggestions(): Promise<ReorderSuggestion[]>
  
  // Handle inter-location transfers
  createTransfer(from: string, to: string, items: TransferItem[]): Promise<Transfer>
}
```

### 4. Vendor Optimization Service

**Purpose**: AI-powered vendor selection for freight optimization

**Algorithm**:
```python
def optimize_vendor_selection(order_items, vendor_catalog):
    """
    Uses dynamic programming to find optimal vendor mix
    that minimizes total cost including freight
    """
    vendors = load_vendor_data()
    freight_rules = load_freight_rules()
    
    # Build optimization model
    model = VendorOptimizationModel(
        items=order_items,
        vendors=vendors,
        freight_rules=freight_rules
    )
    
    # Solve using constraint satisfaction
    solution = model.solve()
    
    return {
        "vendor_splits": solution.splits,
        "total_cost": solution.cost,
        "freight_savings": solution.savings
    }
```

## Data Architecture

### Primary Database (PostgreSQL)

**Schema Design Principles**:
- Normalized for consistency
- JSONB for flexible engraving data
- UUID primary keys for distributed systems
- Audit trails on all tables

**Key Tables**:
- `customers` - Customer information
- `locations` - Franchise locations
- `orders` - Order headers
- `order_items` - Line items with engraving data
- `inventory` - Stock levels by location/SKU
- `vendors` - Vendor information
- `freight_rules` - Dynamic freight thresholds
- `users` - System users with roles

### Caching Layer (Redis)

**Cache Strategy**:
```javascript
// Cache keys structure
const cacheKeys = {
  // Document processing results (1 hour TTL)
  documentResult: `doc:${hash}:result`,
  
  // Inventory levels (5 minute TTL)
  inventory: `inv:${location}:${sku}`,
  
  // Vendor freight rules (24 hour TTL)
  freightRules: `freight:${vendor}:rules`,
  
  // User sessions
  session: `session:${userId}`
}
```

### File Storage (S3)

**Bucket Structure**:
```
crown-trophy-files/
├── documents/
│   ├── incoming/      # Raw customer files
│   ├── processed/     # Extracted data
│   └── generated/     # PDFs for laser
├── designs/
│   ├── templates/     # Reusable templates
│   └── customer/      # Custom designs
└── backups/
    └── daily/         # Automated backups
```

## Security Architecture

### Authentication & Authorization

**Multi-Layer Security**:
1. **Desktop App**: Local Windows credentials + API key
2. **Web Dashboard**: JWT tokens with refresh
3. **API Gateway**: Rate limiting per franchise
4. **Services**: Internal service mesh authentication

**Permission Model**:
```typescript
enum Role {
  FRANCHISE_OWNER = 'owner',
  MANAGER = 'manager', 
  EMPLOYEE = 'employee',
  VIEWER = 'viewer'
}

interface Permissions {
  orders: {
    create: boolean
    edit: boolean
    delete: boolean
    view: boolean
  }
  inventory: {
    adjust: boolean
    transfer: boolean
    view: boolean
  }
  vendors: {
    manage: boolean
    view: boolean
  }
}
```

### Data Encryption

- **At Rest**: AES-256-GCM for database fields
- **In Transit**: TLS 1.3 for all connections
- **File Storage**: S3 server-side encryption
- **Sensitive Data**: Field-level encryption for PII

## Integration Architecture

### QuickBooks Desktop Integration

**Approach**: QuickBooks SDK with local connector

```csharp
// C# QuickBooks connector service
public class QuickBooksConnector {
    public async Task<Invoice> CreateInvoice(Order order) {
        var invoice = new QBInvoice {
            CustomerRef = order.CustomerId,
            LineItems = order.Items.Select(MapToQBLineItem),
            DueDate = DateTime.Now.AddDays(30)
        };
        
        return await qbSession.AddInvoice(invoice);
    }
}
```

### Corel Draw Automation

**Approach**: COM automation for direct control

```python
# Python Corel Draw automation
import win32com.client

class CorelDrawAutomation:
    def __init__(self):
        self.app = win32com.client.Dispatch("CorelDRAW.Application")
    
    def import_pdf(self, pdf_path):
        self.app.ActiveDocument.Import(pdf_path)
    
    def export_for_laser(self, output_path):
        export_filter = self.app.ActiveDocument.ExportEx(
            output_path,
            cdrEPS,  # Or other laser format
            cdrExportOpt
        )
```

## Deployment Architecture

### Development Environment
```yaml
# docker-compose.yml
version: '3.8'
services:
  postgres:
    image: postgres:15
    environment:
      POSTGRES_DB: crown_trophy
      POSTGRES_USER: crown
      POSTGRES_PASSWORD: secure_password
    
  redis:
    image: redis:7-alpine
    
  api:
    build: ./api
    depends_on:
      - postgres
      - redis
    environment:
      DATABASE_URL: postgresql://crown:password@postgres:5432/crown_trophy
      REDIS_URL: redis://redis:6379
```

### Production Deployment

**Option 1: Cloud Native**
- API: AWS ECS or Google Cloud Run
- Database: Amazon RDS or Google Cloud SQL
- Cache: ElastiCache or Memorystore
- Files: S3 or Google Cloud Storage

**Option 2: On-Premise**
- Single server per franchise
- Docker Compose deployment
- Automated backups to cloud
- VPN for multi-location sync

## Performance Targets

### Response Times
- Document processing: < 5 seconds for 100 pages
- Layout generation: < 500ms
- Order creation: < 200ms
- Inventory check: < 100ms

### Throughput
- 1000 concurrent users across all franchises
- 10,000 documents/day processing capacity
- 100,000 orders/day transaction capacity

### Scalability
- Horizontal scaling for API servers
- Read replicas for database
- CDN for static assets
- Queue-based processing for heavy tasks

## Monitoring & Observability

### Metrics Collection
```typescript
// OpenTelemetry instrumentation
import { metrics } from '@opentelemetry/api-metrics';

const meter = metrics.getMeter('crown-trophy');

const documentCounter = meter.createCounter('documents_processed');
const processingTime = meter.createHistogram('processing_duration_ms');
const errorRate = meter.createCounter('processing_errors');
```

### Key Dashboards
1. **Business Metrics**: Orders/day, revenue, savings
2. **Technical Metrics**: API latency, error rates
3. **AI Metrics**: Extraction accuracy, token usage
4. **Infrastructure**: CPU, memory, disk usage

## Development Workflow

### Git Branching Strategy
```
main
├── develop
│   ├── feature/engraving-automation
│   ├── feature/order-management
│   └── feature/inventory-tracking
├── release/v1.0
└── hotfix/critical-bug
```

### CI/CD Pipeline
1. **Test**: Jest/Pytest unit tests
2. **Build**: TypeScript compilation, Docker images
3. **Deploy**: Staged rollout to test franchise first
4. **Monitor**: Automated health checks

## Technology Decisions

### Why These Choices?

**GPT-5 over alternatives**:
- Best accuracy for unstructured documents
- Cost-effective at scale
- Extensive fine-tuning capabilities

**PostgreSQL over NoSQL**:
- Strong consistency requirements
- Complex relational data
- Excellent JSON support via JSONB

**Desktop app over pure web**:
- Offline requirements
- Local printer/hardware integration
- Better performance for heavy processing

**Python for document processing**:
- Best libraries for PDF/Excel manipulation
- Native AI/ML ecosystem
- Easy integration with Node.js via API

This architecture provides a solid foundation that can scale from MVP to enterprise deployment while maintaining flexibility for future enhancements.