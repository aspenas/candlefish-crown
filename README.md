# Crown Trophy Automation Project - Candlefish AI

Welcome to the Crown Trophy automation project! This repository contains all documentation, code, and resources for automating Crown Trophy's engraving and order management processes.

## 🎯 Project Vision

Transform Crown Trophy's manual processes into an AI-powered automation system that:
- Reduces engraving data entry from 45+ minutes to under 1 minute
- Optimizes vendor selection and freight costs
- Streamlines order management across multiple locations
- Potentially saves $4.29M annually across 150 Crown Trophy franchises

## 📋 Quick Links

- [Comprehensive Onboarding Guide](./docs/AARON_ONBOARDING.md) - Start here!
- [Technical Architecture](./docs/TECHNICAL_ARCHITECTURE.md)
- [Project Roadmap](./docs/ROADMAP.md)
- [API Documentation](./docs/API_REFERENCE.md)
- [Meeting Notes & Analysis](./docs/MEETING_ANALYSIS.md)

## 🚀 Quick Start

```bash
# Clone the repository
git clone https://github.com/aspenas/candlefish-crown.git
cd candlefish-crown

# Install dependencies (once we have code)
npm install

# Set up environment variables
cp .env.example .env.local
# Edit .env.local with your credentials

# Run development server
npm run dev
```

## 🏗️ Project Status

**Current Phase**: Planning & Architecture
**Next Milestone**: MVP with engraving automation (Week 4)

### Completed ✅
- [x] Initial requirements gathering with Marshall
- [x] Technology stack selection
- [x] Architecture planning
- [x] Team onboarding documentation

### In Progress 🔄
- [ ] Development environment setup
- [ ] Core infrastructure implementation
- [ ] Document intelligence integration

### Upcoming 📅
- [ ] Engraving automation MVP
- [ ] Order management system
- [ ] QuickBooks integration

## 👥 Team

- **Patrick Smith** - Project Lead & Architecture
- **Tyler Robinson** - Infrastructure & DevOps
- **Aaron Westphal** - Full-Stack Development
- **Marshall** - Crown Trophy Domain Expert

## 📞 Communication

- **Slack Channel**: To be created by Tyler
- **GitHub Issues**: For tracking development tasks
- **Weekly Standups**: TBD

## 🔐 Access & Credentials

All credentials are managed through:
- **AWS Secrets Manager**: Primary secrets storage
- **Infisical**: Environment variable management
- **GitHub Secrets**: CI/CD credentials

Contact Tyler for AWS IAM access setup.

## 📚 Documentation Structure

```
docs/
├── AARON_ONBOARDING.md      # Start here for new team members
├── TECHNICAL_ARCHITECTURE.md # System design and tech stack
├── ROADMAP.md               # Development phases and timeline
├── API_REFERENCE.md         # API endpoints and integration
├── MEETING_ANALYSIS.md      # Marshall's requirements analysis
└── CLAUDE_AGENT_STRUCTURE.md # AI agent configuration
```

## 🛠️ Technology Stack

- **Frontend**: Next.js 14, TypeScript, Tailwind CSS
- **Backend**: Node.js, Express/Fastify
- **AI/ML**: OpenAI GPT-5, Claude Opus 4 (claude-opus-4-20250514), LangChain
- **Database**: PostgreSQL with Prisma ORM
- **Document Processing**: PyMuPDF, OpenPyXL, Unstructured.io
- **Integrations**: QuickBooks Desktop, Corel Draw
- **Infrastructure**: AWS, Render/Netlify (excellent CLI deployment), Docker

## 📈 Business Impact

This project addresses critical pain points:
1. **Manual Data Entry**: 45+ minutes → <1 minute
2. **Order Management**: Unified system replacing Google Sheets
3. **Inventory Tracking**: Real-time visibility across locations
4. **Vendor Optimization**: AI-powered freight savings

Potential annual savings: **$7.67M** across the Crown Trophy network

## 🤝 Contributing

Please read our [Contributing Guide](./CONTRIBUTING.md) before submitting PRs.

## 📄 License

This project is proprietary to Candlefish AI LLC.

---

**Questions?** Contact Patrick at patrick@candlefish.ai