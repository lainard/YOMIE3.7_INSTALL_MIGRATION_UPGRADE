# Project Agreement: YomieCRM Platform Upgrade

## PHP 8.4 & CodeIgniter 4.7 Migration

---

## 1. Parties

**Service Provider:**
ADITUM IT
VAT number: BE 1019.631.039

**Client:** Yomie

---

## 2. Project Summary

This agreement covers the full migration of the YomieCRM platform from the current stack to a modern, supported stack:

| Component | Current | Target |
|-----------|---------|--------|
| PHP Version | 7.4 (End-of-Life) | 8.4 |
| Framework | CodeIgniter 3.x | CodeIgniter 4.7 |
| Infrastructure | Docker (php:7.4-apache) | Docker (php:8.4-apache) |

The full scope of work is documented in [TODO.md](./TODO.md).

---

## 3. Reason for Upgrade

- **PHP 7.4** reached end-of-life on November 28, 2022. It no longer receives security patches, leaving the platform vulnerable.
- **CodeIgniter 3** is in maintenance-only mode with no new features or active development.
- The upgrade brings significant performance improvements (PHP 8.4 JIT), modern security practices, long-term support, and maintainability.

---

## 4. Scope of Work

The project consists of 12 phases as detailed in [TODO.md](./TODO.md):

1. Preparation & Audit
2. PHP 8.4 Compatibility Fixes
3. CodeIgniter 4.7 Project Setup
4. Migrate Models (15+ models)
5. Migrate Controllers (30+ controllers across admin/api/client/group)
6. Migrate Custom Libraries (20+ libraries)
7. Migrate Views
8. Migrate Helpers & Hooks
9. Cron Jobs & CLI Commands
10. Docker & Infrastructure Update
11. Testing & Quality Assurance
12. Staged Deployment & Go-Live

---

## 5. Timeline

Estimated duration: **26 weeks (approximately 6 months)** from the signed date of this agreement.

| Milestone | Target Week | Deliverable |
|-----------|-------------|-------------|
| Audit Complete | Week 2 | Codebase inventory & compatibility report |
| PHP 8.4 Running (CI3) | Week 4 | App running on PHP 8.4 with existing framework |
| CI4 Skeleton Ready | Week 6 | New project structure, routing, auth working |
| Models Migrated | Week 9 | All database models ported and tested |
| Controllers Migrated | Week 13 | All endpoints functional in CI4 |
| Libraries & Views Done | Week 18 | Full feature parity |
| Testing Complete | Week 24 | All integrations verified |
| Production Go-Live | Week 26 | Deployed with rollback plan |

Timelines are estimates and may shift based on unforeseen complexity. Any significant delay (more than 2 weeks) will be communicated promptly.

---

## 6. Deliverables

- Fully functional YomieCRM running on PHP 8.4 + CodeIgniter 4.7
- Updated Docker infrastructure (Dockerfile, docker-compose, cron configuration)
- All existing features preserved and functional:
  - Authentication & SSO
  - Client portal
  - Invoicing & billing
  - Exact Online integration
  - Zoho CRM sync
  - Twikey / Mollie payment processing
  - Peppol invoice processing
  - Email (Brevo/SendInBlue)
  - Helpdesk & Knowledge Base
  - API endpoints (mobile, web, printer)
  - Cron/scheduled tasks
- Updated documentation (README, INSTALL)
- Rollback plan in case of critical issues post-deployment

---

## 7. What Is NOT Included

Unless explicitly agreed upon in writing:

- New feature development during the migration period
- UI/UX redesign or frontend framework changes
- Database schema redesign or data model restructuring
- Migration to a different database engine
- Migration to a different hosting provider or cloud platform
- Mobile app changes beyond API compatibility

---

## 8. Client Responsibilities

Yomie agrees to:

- Provide access to all necessary environments (staging, production, databases)
- Provide documentation or clarification on business logic where code is unclear
- Designate a point of contact available for questions during the project
- Participate in testing and sign off on each milestone
- Avoid major feature requests during the migration to prevent scope creep
- Provide a maintenance window for the final production cutover

---

## 9. Pricing & Payment

| Item | Amount |
|------|--------|
| Total Project Cost | EUR __________ |
| Payment Schedule | As below |

**Payment Milestones:**

| Milestone | Percentage | Amount | Due |
|-----------|-----------|--------|-----|
| Agreement signed (start) | 20% | EUR ____ | Upon signing |
| Phase 2 complete (PHP 8.4 compatible) | 20% | EUR ____ | ~Week 4 |
| Phase 5 complete (Controllers migrated) | 20% | EUR ____ | ~Week 13 |
| Phase 11 complete (Testing done) | 20% | EUR ____ | ~Week 24 |
| Production go-live (Final delivery) | 20% | EUR ____ | ~Week 26 |

---

## 10. Warranty & Support

- **30-day warranty period** after go-live for bug fixes related to the migration
- Bugs caused by the migration will be fixed at no additional cost during the warranty period
- New feature requests or changes outside the migration scope are billed separately
- After the warranty period, standard support/maintenance terms apply

---

## 11. Risk & Rollback

- The old CI3 system will remain deployable for at least 2 weeks after go-live as a rollback option
- If critical issues are found that cannot be resolved within 48 hours of go-live, the system will be rolled back to the previous version
- Data created during the CI4 period will be preserved (same database)

---

## 12. Intellectual Property

- All code produced during this project is owned by Yomie
- The Service Provider retains no rights to the custom code
- Open-source libraries used remain under their respective licenses

---

## 13. Confidentiality

Both parties agree to treat all business information, source code, customer data, and internal processes as confidential and will not share them with third parties without written consent.

---

## 14. Termination

- Either party may terminate this agreement with 14 days written notice
- In case of termination, work completed up to that point will be delivered and billed proportionally
- Any advance payments for undelivered milestones will be refunded

---

## 15. Acceptance & Signatures

By signing below, both parties agree to the terms outlined in this document and the scope defined in [TODO.md](./TODO.md).

---

**For Yomie:**

Name: ___________________________________

Role: ___________________________________

Signature: ___________________________________

Date: ___________________________________

---

**For Service Provider:**

Name: ___________________________________

Role: ___________________________________

Signature: ___________________________________

Date: ___________________________________

VAT number: BE 1019.631.039

---

*This document was generated on August 7, 2026. Both parties should retain a signed copy.*
