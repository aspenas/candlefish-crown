# Action Items & Team Assignments

## Immediate Actions (This Week)

### For Tyler
- [ ] **Create Slack channel** for Crown Trophy project team
- [ ] **Set up Aaron's AWS IAM account** with appropriate permissions
  - AdministratorAccess similar to your current setup
  - Store credentials in AWS Secrets Manager
  - Send credentials securely to Aaron
- [ ] **Review candlefish-crown GitHub repo** and add Aaron as collaborator
- [ ] **Set up development infrastructure**:
  - PostgreSQL database
  - Redis instance (Upstash)
  - S3 bucket for document storage

### Development Track (Aaron's Expertise)
- [ ] **Review all documentation** in this repository
- [ ] **Set up local development environment**
- [ ] **Schedule joint call with Marshall** (Patrick, Aaron, Marshall) to discuss:
  - Corel Draw version and specifics
  - Sample engraving files
  - Most complex use cases
- [ ] **Make architecture decisions**:
  - Desktop framework (Electron vs Tauri)
  - Monolith vs microservices
  - Development approach preference
- [ ] **Start document extraction prototype** with sample files

### For Patrick
- [ ] **Send intro message** to Aaron and Tyler with repo link
- [ ] **Schedule weekly standup** meetings
- [ ] **Schedule joint requirements session** with Aaron and Marshall
- [ ] **Review and approve** architecture decisions

## Week 1 Deliverables

### Development Environment (Aaron + Tyler)
- [ ] Local dev setup confirmed working
- [ ] Database schemas created
- [ ] Basic API structure in place
- [ ] CI/CD pipeline configured

### Document Intelligence (Aaron)
- [ ] GPT-5 API integrated
- [ ] Basic file upload endpoint
- [ ] Initial extraction tests with Golden Marlins file
- [ ] Accuracy benchmarks established

### Infrastructure (Tyler)
- [ ] AWS resources provisioned
- [ ] Secrets management configured
- [ ] Monitoring setup started
- [ ] Backup strategy defined

## Week 2-4 Goals

### MVP Features (Aaron)
- [ ] Complete document extraction pipeline
- [ ] Layout generation algorithm
- [ ] PDF export functionality
- [ ] Basic web interface
- [ ] Demo ready for Marshall

### Integrations (Aaron)
- [ ] QuickBooks Desktop research
- [ ] Corel Draw automation POC
- [ ] Email monitoring setup
- [ ] File format compatibility tests

### Testing & Quality (All)
- [ ] Unit test coverage >80%
- [ ] Integration tests for critical paths
- [ ] Performance benchmarks
- [ ] Security audit checklist

## Ongoing Responsibilities

### Aaron (Development Partner)
- Lead technical architecture decisions
- Drive core feature implementation
- Maintain code quality standards
- Collaborate in requirements sessions with Marshall

### Tyler (Infrastructure & DevOps)
- Manage AWS resources
- Handle deployments
- Monitor system health
- Manage secrets and credentials

### Patrick (Project Lead)
- Stakeholder communication
- Strategic decisions
- Remove blockers
- Resource allocation

### Marshall (Domain Expert)
- Provide sample data
- Test features
- Give feedback
- Connect with other franchises

## Communication Protocol

### Daily
- Update progress in Slack
- Flag any blockers immediately

### Weekly
- Monday: Sprint planning
- Wednesday: Progress check
- Friday: Demo and retrospective

### Monthly
- Stakeholder update
- Budget review
- Roadmap adjustment

## Success Metrics

### Week 4 (MVP)
- [ ] Process 300 names in <60 seconds
- [ ] 95%+ extraction accuracy
- [ ] PDF generation working
- [ ] Marshall approval obtained

### Week 8
- [ ] Full order workflow
- [ ] QuickBooks integration
- [ ] 99%+ accuracy
- [ ] <30 second processing

### Week 16
- [ ] 2 locations deployed
- [ ] 90% time reduction achieved
- [ ] Positive user feedback
- [ ] Ready for scale

## Key Contacts

- **Patrick Smith**: patrick@candlefish.ai
- **Tyler Robinson**: tyler@candlefish.ai
- **Aaron Westphal**: [To be provided]
- **Marshall**: [Via Patrick]

## Important Links

- **GitHub Repo**: https://github.com/aspenas/candlefish-crown
- **Slack Channel**: [To be created by Tyler]
- **AWS Console**: https://207567767039.signin.aws.amazon.com/console
- **Documentation**: See `/docs` folder

---

*Last Updated: July 2025*
*Next Review: End of Week 1*