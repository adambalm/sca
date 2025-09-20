# Springfield Academy CMS Implementation Plan

## Document Type: Build Plan / Implementation Roadmap
## Version: 1.0
## Last Updated: September 2024

---

## Executive Overview

This Implementation Plan provides the detailed execution strategy for the Springfield Academy CMS project, breaking down the work into sprints, defining dependencies, and establishing clear success metrics for each phase.

### Plan Structure
- **Phase 0**: Foundation (48-72 hours) - Template selection and setup
- **Phase 1**: Core CMS (8 weeks) - Full implementation
- **Phase 2**: Enhancements (Optional, 3-6 months) - AI and advanced features
- **Phase 3**: Future Roadmap (6-18 months) - Major expansions

---

## Phase 0: Foundation Sprint (48-72 Hours)

### Objectives
1. Select optimal Wagtail template through data-driven comparison
2. Establish development environment with GitPod/Ona
3. Capture and inventory all existing content
4. Lock design tokens and information architecture

### Hour-by-Hour Breakdown

#### Hours 0-4: Environment Initialization
```bash
# Task 0.1: Repository Setup
- Create GitHub repository
- Configure GitPod environment
- Set up branch structure for bake-off
- Initialize both template candidates

# Task 0.2: Team Coordination
- Assign developers to branches
- Set up communication channels
- Share GitPod workspace links
- Define success criteria
```

#### Hours 4-24: Parallel Development

**Branch A: News Template Path**
| Hour | Task | Deliverable | Success Metric |
|------|------|------------|----------------|
| 4-8 | Setup & explore | Running site | Admin accessible |
| 8-12 | Add admissions header | Sticky CTAs | Visible on all pages |
| 12-16 | Create programs stub | Program model | CRUD working |
| 16-20 | Implement program finder | Filter UI | Returns results |
| 20-24 | Mobile optimization | Responsive design | Lighthouse >85 |

**Branch B: Starter Kit Path**
| Hour | Task | Deliverable | Success Metric |
|------|------|------------|----------------|
| 4-8 | Setup & import news | Running site | News app integrated |
| 8-12 | Build admissions nav | Custom navigation | CTAs prominent |
| 12-16 | Create programs app | Program model | Matches Branch A |
| 16-20 | Program finder | Same as Branch A | Feature parity |
| 20-24 | Performance testing | Metrics collected | Comparable data |

#### Hours 24-48: Content Capture & Analysis

```python
# Content Scraping Tasks
1. Configure scraper for WordPress site
2. Run extraction (4-hour process)
3. Generate inventory CSV
4. Clean HTML artifacts
5. Categorize content types
6. Create redirect mapping
7. Identify rewrite needs
```

#### Hours 48-72: Decision & Consolidation

**Decision Matrix Scoring**
```
Score = (Admissions * 0.30) + (Finder * 0.25) + (Performance * 0.15) + 
        (Effort * 0.15) + (i18n * 0.10) + (Clarity * 0.05)

If Score_A > Score_B: Use News Template
Else: Use Starter Kit
```

### Phase 0 Deliverables Checklist
- [ ] Selected template with documented rationale
- [ ] GitPod environment configured and shareable
- [ ] Content inventory (CSV) with classifications
- [ ] Redirect map for top 100 pages
- [ ] Design tokens locked in Tailwind config
- [ ] IA diagram with admissions-first navigation
- [ ] Stakeholder sign-off on direction

---

## Phase 1: Core CMS Implementation (8 Weeks)

### Sprint Structure
- **Sprint 1-2**: Models & Admin (Weeks 1-2)
- **Sprint 3-4**: Templates & Frontend (Weeks 3-4)
- **Sprint 5-6**: Content Migration (Weeks 5-6)
- **Sprint 7-8**: Testing & Launch Prep (Weeks 7-8)

### Sprint 1-2: Models & Admin Configuration

#### Week 1: Core Models
```python
# Development Priority Order
Day 1-2: Program model + admin
  - Fields: name, grades, type, overview, curriculum
  - StreamFields for flexible content
  - CTAs: apply_url, visit_url
  - SEO: meta_description, keywords

Day 3-4: Faculty model + admin
  - Fields: name, title, department, bio
  - Relations: programs taught
  - Media: profile photo
  - Contact: email, phone extension

Day 5: Event model + admin
  - Fields: title, date, location, description
  - Registration: capacity, deadline, URL
  - Recurrence: support for repeating events
```

#### Week 2: Supporting Models
```python
Day 1-2: Forms and CTAs
  - InquiryForm: capture leads
  - VisitRequestForm: schedule tours
  - NewsletterSignup: email collection

Day 3-4: Navigation and Settings
  - Menu system for main nav
  - Footer configuration
  - Global settings (contact, social)

Day 5: Integration Points
  - Email configuration (SMTP/API)
  - Analytics setup (GA4)
  - Search configuration
```

### Sprint 3-4: Frontend Development

#### Week 3: Template Architecture
```
/templates/
  /base/
    - master.html (with admissions header)
    - navigation.html
    - footer.html
  /pages/
    - home.html
    - program_detail.html
    - faculty_list.html
    - faculty_detail.html
    - event_list.html
  /components/
    - card.html
    - cta_block.html
    - form_inquiry.html
```

#### Week 4: Styling & Interactions
```css
/* Tailwind Configuration */
- Color palette (academy blue, gold)
- Typography scale
- Spacing system
- Component classes
- Responsive breakpoints

/* JavaScript Features */
- Mobile navigation
- Form validation
- Program finder filters
- Smooth scrolling
- Analytics events
```

### Sprint 5-6: Content Migration

#### Week 5: Automated Migration
```python
# Migration Pipeline
1. Parse WordPress export/scrape
2. Map content to Wagtail models
3. Clean HTML and fix links
4. Import media assets
5. Create page hierarchy
6. Generate redirects

# Priority Order
1. Homepage content
2. Program pages (20)
3. Faculty profiles (50)
4. News articles (last 50)
5. Static pages (About, Contact)
6. Events (upcoming only)
```

#### Week 6: Manual Refinement
- Review automated imports
- Fix formatting issues
- Update images and captions
- Write missing meta descriptions
- Test all forms
- Verify CTAs work

### Sprint 7-8: Testing & Launch Preparation

#### Week 7: Quality Assurance

**Testing Checklist**
| Test Type | Target | Tool | Pass Criteria |
|-----------|--------|------|---------------|
| Performance | Mobile speed | Lighthouse | Score ≥ 90 |
| Accessibility | WCAG AA | axe DevTools | No critical issues |
| Forms | All submissions | Manual | Email received |
| SEO | Meta tags | Screaming Frog | All pages have meta |
| Mobile | Responsive | BrowserStack | Looks good on all |
| Search | Result relevance | Manual | Correct results |
| CTAs | Click tracking | GA4 | Events firing |

#### Week 8: Deployment

**Launch Checklist**
```bash
# Pre-launch (Days 1-3)
□ Final content review
□ Backup current site
□ DNS preparation
□ SSL certificates ready
□ Monitoring setup
□ Team training complete

# Launch Day (Day 4)
□ Deploy to production
□ DNS cutover
□ Test all critical paths
□ Monitor error logs
□ Verify analytics
□ Check email flow

# Post-launch (Days 5-7)
□ Monitor performance
□ Fix any issues
□ Gather feedback
□ Document lessons learned
□ Plan Phase 2 features
```

---

## Phase 2: Optional Enhancements (Months 3-6)

### Feature Prioritization Matrix

| Feature | Effort | Impact | Priority | Dependencies |
|---------|--------|--------|----------|--------------|
| RAG Search | High | High | 1 | Webhook system |
| Event Calendar | Medium | Medium | 2 | JS framework |
| Workflows | Low | Low | 3 | Config only |
| Performance | Medium | High | 4 | Monitoring data |

### RAG Integration Plan (If Implemented)

#### Month 3: Setup
1. Deploy RAG service container
2. Configure webhooks
3. Index existing content
4. Test search quality

#### Month 4: Enhancement
1. Tune search relevance
2. Add chat interface
3. Implement fallback
4. Monitor usage

---

## Risk Management

### Technical Risks

| Risk | Probability | Impact | Mitigation |
|------|------------|--------|------------|
| Template choice wrong | Low | High | 48-hour bake-off with data |
| Migration data loss | Medium | High | Multiple backups, validation |
| Performance issues | Low | Medium | Early testing, CDN ready |
| Team unavailable | Medium | Medium | Documentation, pair work |

### Business Risks

| Risk | Probability | Impact | Mitigation |
|------|------------|--------|------------|
| Scope creep | High | Medium | Written sign-offs, phases |
| Budget overrun | Low | Medium | Fixed phases, clear boundaries |
| Stakeholder changes | Medium | High | Regular demos, involvement |

---

## Success Metrics

### Phase 1 Success Criteria (Must Have)
- [ ] All content migrated (100%)
- [ ] Forms working (100% delivery rate)
- [ ] Mobile Lighthouse ≥ 90
- [ ] Zero broken links
- [ ] 301 redirects for all old URLs
- [ ] CTAs on every page
- [ ] Search returning relevant results

### Phase 2 Success Metrics (If Implemented)
- [ ] RAG search <800ms response time
- [ ] 50% of searches use AI
- [ ] Event registrations working
- [ ] Workflow reducing publish time by 30%

---

## Resource Allocation

### Team Structure
```
Project Lead (You)
  ├── Development (Cursor + Claude)
  │   ├── Backend (Django/Wagtail)
  │   ├── Frontend (Templates/CSS)
  │   └── Infrastructure (GitPod/Fly)
  ├── Content (Manual work)
  │   ├── Migration review
  │   ├── Meta descriptions
  │   └── Image optimization
  └── Testing (Automated + Manual)
      ├── Performance
      ├── Functionality
      └── Accessibility
```

### Time Allocation (Phase 1)
```
Development: 60% (48 hours/week)
  - Models: 15%
  - Templates: 20%
  - Migration: 15%
  - Integration: 10%

Content Work: 25% (20 hours/week)
  - Review: 10%
  - Writing: 10%
  - Images: 5%

Testing: 15% (12 hours/week)
  - Automated: 5%
  - Manual: 10%
```

---

## Communication Plan

### Stakeholder Updates
- **Weekly**: Progress email with demos
- **Bi-weekly**: Video call review
- **Phase gates**: Sign-off meetings

### Documentation
- **Daily**: Git commits with clear messages
- **Weekly**: Update this plan
- **Per sprint**: Retrospective notes

---

## Appendix: Quick Reference

### Critical Commands
```bash
# Most used during development
python manage.py runserver
python manage.py makemigrations && python manage.py migrate
python manage.py test apps.programs
npx tailwindcss -w

# Deployment
fly deploy
fly logs -a springfield-academy
```

### Key Files to Edit
```
apps/programs/models.py     # Program page model
templates/base.html          # Master template
static/css/input.css        # Tailwind source
fly.toml                    # Deployment config
```

### Emergency Contacts
- Wagtail Slack: #help channel
- Fly.io Status: status.fly.io
- Cloudflare: 1-888-99-FLARE

---

*This plan is a living document. Update it as the project evolves.*
