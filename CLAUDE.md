# Crown Trophy Project - Claude Code Configuration

## Project Overview
Crown Trophy automation system is an AI-powered solution that transforms manual engraving processes into automated workflows. The project features document intelligence for data extraction, layout optimization for laser engraving, integrated order management, and multi-location inventory tracking.

## Technology Stack
- **Framework**: Next.js 14 with App Router + Desktop App (Electron/Tauri)
- **Language**: TypeScript 5 + Python 3.11 (AI services)
- **AI/ML**: OpenAI GPT-5, Claude Opus 4 (claude-opus-4-20250514), LangChain 2.0
- **Database**: PostgreSQL 15 with Prisma ORM
- **Document Processing**: PyMuPDF, OpenPyXL, Unstructured.io
- **Integrations**: QuickBooks Desktop, Corel Draw, Email monitoring
- **Caching**: Redis (Upstash)
- **Infrastructure**: AWS/Vercel + Local deployment options

## Core Business Logic
The application centers around:
- **45-minute → 1-minute** engraving data processing
- **AI-powered document extraction** from Excel/PDF/Word
- **Optimized layout generation** for laser cutting
- **Multi-vendor freight optimization** 
- **Real-time inventory tracking** across locations

## Agent Configurations

### 1. Document Intelligence Specialist (document-intelligence)
**Purpose**: Extract and process customer engraving data
**Focus Areas**:
- `/services/document-processor/` - Core extraction engine
- `/lib/ai/gpt5-integration.ts` - AI model integration
- `/lib/parsers/` - File format parsers
- `/tests/extraction-accuracy/` - Accuracy validation

**Key Tasks**:
- Implement GPT-5 document analysis
- Handle Excel name list extraction
- Process PDF award templates
- Optimize extraction accuracy to 99%+
- Handle edge cases (reversed names, special characters)

### 2. Layout Optimization Engineer (layout-optimizer)
**Purpose**: Generate efficient material layouts for laser cutting
**Focus Areas**:
- `/lib/layout-engine/` - Bin packing algorithms
- `/lib/material-optimizer/` - Waste reduction logic
- `/services/pdf-generator/` - Export to laser formats
- `/components/layout-preview/` - Visual preview

**Key Tasks**:
- Implement 2D bin packing for material optimization
- Generate PDF with vector cut lines
- Support multiple material sizes (12x18, 12x24)
- Minimize material waste
- Export to Corel Draw compatible formats

### 3. Order Management Specialist (order-management)
**Purpose**: Handle order lifecycle and workflows
**Focus Areas**:
- `/app/api/orders/` - Order API endpoints
- `/lib/services/order-service.ts` - Business logic
- `/components/orders/` - Order UI components
- `/lib/db/schemas/orders.prisma` - Database models

**Key Tasks**:
- Build CRUD operations for orders
- Implement status tracking workflow
- Create multi-location visibility
- Handle order-to-inventory depletion
- Generate QuickBooks-ready invoices

### 4. Integration Architect (integration-architect)
**Purpose**: Connect with external systems
**Focus Areas**:
- `/lib/integrations/quickbooks/` - QuickBooks Desktop SDK
- `/lib/integrations/corel-draw/` - COM automation
- `/services/email-monitor/` - Email ingestion
- `/lib/integrations/laser-software/` - Various laser formats

**Key Tasks**:
- Implement QuickBooks Desktop connector
- Build Corel Draw automation
- Create email monitoring service
- Handle multiple laser software formats
- Ensure reliable sync mechanisms

### 5. Inventory & Vendor Specialist (inventory-vendor)
**Purpose**: Manage stock levels and optimize vendor selection
**Focus Areas**:
- `/lib/inventory/` - Stock tracking system
- `/lib/vendor-optimizer/` - Freight optimization AI
- `/app/api/inventory/` - Inventory endpoints
- `/components/vendor-selection/` - Vendor UI

**Key Tasks**:
- Build real-time inventory tracking
- Implement vendor freight optimization
- Create reorder point calculations
- Handle inter-location transfers
- Optimize vendor selection for cost

### 6. Performance & Infrastructure Agent (performance-infra)
**Purpose**: Ensure system performance and reliability
**Focus Areas**:
- `/lib/cache/` - Redis caching strategies
- `/infrastructure/` - Deployment configurations
- `/monitoring/` - Performance metrics
- `/lib/queue/` - Background job processing

**Key Tasks**:
- Optimize document processing speed
- Implement efficient caching
- Set up monitoring and alerts
- Handle offline capability
- Ensure sub-second response times

## Development Workflow

### Running the Project
```bash
# Install dependencies
npm install
pip install -r requirements.txt

# Set up databases
docker-compose up -d

# Run development servers
npm run dev              # Next.js app
python services/app.py   # Python AI services
```

### Testing
```bash
# Run all tests
npm test
pytest

# Test specific features
npm run test:extraction
npm run test:layout
npm run test:integration
```

### Building for Production
```bash
# Build web app
npm run build

# Build desktop app
npm run build:desktop

# Package for distribution
npm run package:windows
```

## Key Files and Directories

### Core Application
- `/app/` - Next.js pages and API routes
- `/components/` - React components
- `/lib/` - Shared business logic
- `/services/` - Python AI services
- `/desktop/` - Electron/Tauri desktop app

### Document Processing
- `/services/document-processor/` - Main extraction service
- `/lib/parsers/` - File format parsers
- `/lib/ai/` - AI model integrations
- `/test-data/` - Sample customer files

### Configuration
- `next.config.js` - Next.js configuration
- `tsconfig.json` - TypeScript settings
- `requirements.txt` - Python dependencies
- `.env.local` - Environment variables

### Documentation
- `README.md` - Project overview
- `/docs/` - Detailed documentation
- `/examples/` - Implementation examples
- `/specs/` - Technical specifications

## Critical Considerations

### Accuracy Requirements
- 99%+ accuracy on name extraction
- 100% accuracy on material layout
- Zero data loss during processing
- Validate against original files

### Performance Targets
- < 1 minute for 300-item job
- < 500ms layout generation
- < 200ms order creation
- Real-time inventory updates

### Integration Reliability
- Handle QuickBooks connection failures
- Queue failed syncs for retry
- Maintain local state during outages
- Graceful degradation

### Security Requirements
- Encrypt customer data at rest
- Secure API endpoints
- Role-based access control
- Audit trail for all actions

## Common Tasks

### Processing a New Order
1. Receive customer file via email/upload
2. Extract engraving data with AI
3. Generate optimized layout
4. Create order in system
5. Sync to QuickBooks
6. Update inventory levels

### Adding a New Vendor
1. Add vendor to database
2. Import product catalog
3. Set freight thresholds
4. Configure pricing rules
5. Test optimization algorithm

### Debugging Extraction Issues
1. Review extraction confidence scores
2. Check AI model responses
3. Validate parser output
4. Test with similar files
5. Adjust prompts if needed

## Project-Specific Patterns

### File Processing Pipeline
```typescript
// Standardized processing flow
const pipeline = new ProcessingPipeline()
  .addStep(FileParser)
  .addStep(AIExtractor)
  .addStep(DataValidator)
  .addStep(LayoutGenerator)
  .addStep(PDFExporter)
```

### Error Handling
- Graceful failures with user feedback
- Automatic retries for transient errors
- Manual review queue for low confidence
- Comprehensive error logging

### State Management
- Server state source of truth
- Optimistic UI updates
- Background sync for offline
- Conflict resolution strategies

## Crown Trophy Specific Context

### Business Rules
- Engraving costs $1 per item
- Material sheets are 12x24 or 12x18
- 75% of orders come via email
- QuickBooks Desktop (not Online)
- No shopping cart (franchise restriction)

### Vendor Information
- Crown Awards (primary)
- JDS, Marco, Leonard's, SmartD, PDU
- Freight minimums: $200-$1000
- Promotions change infrequently

### Location Details
- Littleton (primary)
- Arvada (secondary)
- DTC (aunt's location)
- Inter-store transfers common

## Contact and Resources
- **Customer Examples**: Golden Marlins (276 items)
- **QuickBooks Version**: Desktop 2024
- **Corel Draw Version**: X8 or newer
- **Test Files**: Available in `/test-data/`
- **Support**: Slack channel (to be created)