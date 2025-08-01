# Aaron Westphal - Crown Trophy Project Onboarding

Welcome to the Crown Trophy automation project, Aaron! This document provides everything you need to get up to speed and start contributing effectively.

## 🎯 Project Overview

### The Vision
We're building an AI-powered automation system for Crown Trophy that will revolutionize how they handle engraving orders, inventory, and vendor management. This isn't just another CRUD app - it's a sophisticated document intelligence system that will save franchises hundreds of thousands of dollars annually.

### Your Role
As our full-stack developer, you'll have the flexibility to either:
1. Take the first stab at the entire development (recommended if you prefer ownership)
2. Refine and enhance the initial scope we create

You have **root access** to the project and all API keys - we trust you to make architectural decisions that align with our goals.

## 🚨 Critical Context

### Company Status
- **Candlefish AI LLC** was just approved by Morgan Stanley last week
- Tyler is currently migrating all projects, credentials, and services
- There will be some lag on infrastructure items initially
- A business operations specialist will join soon to handle back-office tasks

### Team Collaboration
- **Slack Channel**: Tyler will create and invite you (ACTION ITEM)
- **AWS Access**: Tyler will set up your IAM account with appropriate permissions (ACTION ITEM)
- **GitHub**: You have access to this repository
- **Communication**: Direct access to Patrick, Tyler, and Marshall

## 💡 The Business Problem

### Current Pain Points at Crown Trophy

1. **Manual Engraving Data Entry (HIGHEST PRIORITY)**
   - Currently takes **45+ minutes** for jobs like Golden Marlins (276 items)
   - Revenue: Only $1 per engraved item ($276 for 45 minutes work)
   - Process: Manual copy/paste from Excel to Corel Draw
   - **Our Goal**: Reduce to under 1 minute with AI

2. **Order Management Chaos**
   - Using Google Sheets with color coding (white/yellow/blue)
   - 10 users with write access, no version control
   - No integration between orders and inventory
   - **Our Goal**: Unified system with real-time updates

3. **Inventory Nightmare**
   - 2,000+ SKUs across 3 locations
   - Manual counting and spreadsheet entry
   - No automated reordering or tracking
   - **Our Goal**: Automated tracking with predictive reordering

4. **Vendor Complexity**
   - 6+ vendors with different freight minimums ($200-$1000)
   - Marshall manually optimizes to hit freight minimums
   - Promotions change every few years
   - **Our Goal**: AI-powered vendor selection

## 🏗️ Technical Architecture

### Core Technology Stack
```
Frontend:
- Next.js 14 (App Router)
- TypeScript 5
- Tailwind CSS
- Framer Motion

Backend:
- Node.js + Express/Fastify
- PostgreSQL + Prisma ORM
- Redis (Upstash) for caching
- WebSocket for real-time updates

AI/ML:
- OpenAI GPT-5 (primary for document extraction)
- Claude Opus 4 (claude-opus-4-20250514) - context understanding
- LangChain 2.0 (orchestration)
- Unstructured.io (document parsing)

Document Processing:
- PyMuPDF 2.0 (PDF generation)
- OpenPyXL 4.0 (Excel processing)
- Custom layout optimization algorithms

Integrations:
- QuickBooks Desktop (bidirectional sync)
- Corel Draw (via PDF export)
- Email monitoring (customer orders)
```

### System Architecture
```
┌─────────────────┐     ┌─────────────────┐     ┌─────────────────┐
│  Desktop App    │     │ Email Ingestion │     │  Web Dashboard  │
└────────┬────────┘     └────────┬────────┘     └────────┬────────┘
         │                       │                         │
         └───────────────────────┴─────────────────────────┘
                                 │
                        ┌────────▼────────┐
                        │   API Gateway   │
                        │   (FastAPI)     │
                        └────────┬────────┘
                                 │
      ┌──────────────────────────┼──────────────────────────┐
      │                          │                          │
┌─────▼─────┐          ┌────────▼────────┐        ┌────────▼────────┐
│ Document  │          │     Order       │        │   Inventory     │
│Intelligence│          │  Management     │        │  Management     │
│  Service  │          │    Service      │        │    Service      │
└─────┬─────┘          └────────┬────────┘        └────────┬────────┘
      │                          │                          │
      └──────────────────────────┴──────────────────────────┘
                                 │
                        ┌────────▼────────┐
                        │  PostgreSQL DB  │
                        └─────────────────┘
```

## 🚀 Development Roadmap

### Phase 1: Foundation (Weeks 1-4) - CURRENT PHASE
- [x] Requirements gathering
- [ ] Development environment setup
- [ ] Core infrastructure (database, API, auth)
- [ ] Document intelligence integration
- [ ] Basic engraving workflow MVP

### Phase 2: Core Features (Weeks 5-8)
- [ ] Advanced extraction (patterns, confidence scoring)
- [ ] Layout optimization algorithms
- [ ] Order management CRUD
- [ ] QuickBooks integration

### Phase 3: Advanced Features (Weeks 9-12)
- [ ] Inventory tracking system
- [ ] Vendor optimization AI
- [ ] Multi-location support
- [ ] Advanced analytics dashboard

### Phase 4: Deployment & Scale (Weeks 13-16)
- [ ] Production deployment
- [ ] User training
- [ ] Multi-franchise rollout
- [ ] Market packaging

## 🔧 Getting Started

### 1. Environment Setup
```bash
# Clone the repo
git clone https://github.com/aspenas/candlefish-crown.git
cd candlefish-crown

# Install dependencies
npm install
pip install -r requirements.txt

# Set up databases
docker-compose up -d postgres redis

# Run migrations
npx prisma migrate dev
```

### 2. API Keys & Secrets
Contact Tyler for:
- AWS IAM credentials
- Access to Infisical project
- API keys for OpenAI, Anthropic, etc.

### 3. Local Development
```bash
# Start the development server
npm run dev

# In another terminal, start the Python services
python services/document_processor.py
```

## 📊 Business Metrics & Goals

### Success Criteria
1. **Time Reduction**: 45 minutes → <1 minute for engraving data entry
2. **Accuracy**: 99%+ data extraction accuracy
3. **Cost Savings**: $30K+ annually per location
4. **User Adoption**: 100% within 30 days

### Industry Impact
- 150 Crown Trophy franchises nationwide
- Potential market: $7.67M in annual savings
- Goal: Become the industry standard solution

## 🎨 UI/UX Priorities

### Design Principles
1. **Faster than Google Sheets** - Performance is critical
2. **Desktop-first** - Windows app, not just web
3. **Minimal training** - Intuitive for non-technical users
4. **Offline capability** - Must work without internet

### Key Workflows
1. **Email → Engraving**: Automatic extraction and layout
2. **Order → Inventory**: Real-time depletion tracking
3. **Vendor Selection**: One-click optimization
4. **Multi-location**: Seamless transfers

## 🔐 Security & Compliance

### Requirements
- All customer data encrypted at rest
- API keys in AWS Secrets Manager/Infisical
- Audit trail for all actions
- QuickBooks compliance for financial data

### Access Control
- Role-based permissions (Admin, User, Viewer)
- Multi-location data isolation
- API rate limiting per franchise

## 📚 Resources & Documentation

### Project Documents
1. [Initial Meeting Transcript](../inputs/InitialMeeting_TranscriptedNotes.txt) - Raw notes from Marshall
2. [Crown Trophy Assessment](../Crown%20Trophy%20-%20Initial%20Project%20Assessment.pdf) - Our initial proposal
3. [Comprehensive Analysis](../openai/comprehensive_crown_trophy_analysis.md) - Deep dive into solutions
4. [Marshall's Notes](../inputs/marshallnotes_EtchXpress_250724_002428.pdf) - Feature ideas

### External Resources
- [Corel Draw API Docs](https://www.coreldraw.com/en/pages/api/)
- [QuickBooks Desktop SDK](https://developer.intuit.com/app/developer/qbdesktop/docs)
- [LangChain Documentation](https://python.langchain.com/)

## 🤔 Key Decisions for Aaron

### Immediate Priorities
1. **Architecture**: Monolith vs microservices for initial MVP?
2. **Desktop Framework**: Electron vs Tauri for Windows app?
3. **AI Integration**: Direct API vs LangChain abstraction?
4. **Database**: Single vs separate databases per location?

### Development Approach
Would you prefer to:
- A) Build the initial architecture and let us refine?
- B) Review our initial implementation and improve?
- C) Collaborate on architecture then divide features?

## 💬 Questions for Marshall

When you connect with Marshall, key areas to explore:
1. Exact Corel Draw version and export formats
2. Laser engraver software compatibility
3. Peak season timing and volume
4. Most complex engraving job examples
5. QuickBooks Desktop integration specifics

## 🚦 Next Steps

### Week 1 Tasks
1. [ ] Set up development environment
2. [ ] Review all project documentation
3. [ ] Connect with Tyler for AWS access
4. [ ] Initial architecture decisions
5. [ ] Start document intelligence prototype

### Action Items for Team
- **Tyler**: Create Slack channel, set up Aaron's AWS IAM
- **Patrick**: Schedule architecture review meeting
- **Aaron**: Review docs, set up environment, initial questions

## 🎯 Final Thoughts

This project has massive potential - both for Crown Trophy and as a product we can sell to the entire industry. Your expertise will be crucial in making this vision a reality.

We believe in giving you autonomy to make technical decisions while staying aligned with business goals. Don't hesitate to challenge assumptions or propose better approaches.

Welcome to the team! Let's build something amazing together.

---

**Questions?** Reach out on Slack or email:
- Patrick: patrick@candlefish.ai
- Tyler: tyler@candlefish.ai