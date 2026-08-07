# Upgrade Plan: PHP 8.4 + CodeIgniter 4.7

> **Agreement:** Before starting this project, the [Upgrade Agreement](./UPGRADE_AGREEMENT.md) must be signed by Yomie and the Service Provider.
> **Overeenkomst:** [Nederlandse versie](./UPGRADE_OVEREENKOMST.md)

## Overview

Migrate YomieCRM from **PHP 7.4 + CodeIgniter 3** to **PHP 8.4 + CodeIgniter 4.7**.

This is a major migration. CI3 to CI4 is not a drop-in upgrade — it's essentially a rewrite with a new architecture (namespaces, PSR-4 autoloading, new MVC pattern, new routing, new ORM). The plan is structured in phases to minimize downtime and allow parallel development.

---

## Phase 1: Preparation & Audit (Week 1-2)

### 1.1 Codebase Inventory
- [ ] Document all controllers (admin/, api/, client/, group/ + root)
- [ ] Document all models and their database table mappings
- [ ] Document all custom libraries (Pdf, Twikey, SendInBlue, Zohoapi, Pusher, etc.)
- [ ] Document all helpers and their usage
- [ ] Document all hooks and config overrides
- [ ] Map all routes (config/routes.php) and their corresponding controller methods
- [ ] List all views and their template structure

### 1.2 Dependency Audit
- [ ] Check PHP 8.4 compatibility for each composer package:
  - `sendinblue/api-v3-sdk` ^6.1.0 — check for PHP 8.4 support or replacement (Brevo SDK)
  - `codelicious/php-coda-parser` ^1.0 — check compatibility
  - `box/spout` ^2.7 — DEPRECATED, replace with `openspout/openspout`
  - `phpmailer/phpmailer` ^6.4 — upgrade to ^6.9+ (PHP 8.4 compatible)
  - `webleit/zohocrmapi` ^5.0 — check compatibility
  - `zohocrm/php-sdk` ^3.1 — check for v4+ with PHP 8.4 support
  - `picqer/exact-php-client` ^3.40 — check for PHP 8.4 compatible version
  - `twikey/twikey-api-php` ^0.3.1 — check compatibility
  - `mollie/mollie-api-php` ^2.76 — should support PHP 8.4
  - `phpgangsta/googleauthenticator` dev-master — ABANDONED, replace with `pragmarx/google2fa`
  - `pusher/pusher-php-server` ^7.2 — check for PHP 8.4 compatible version
- [ ] Identify deprecated PHP functions used in codebase (mysql_*, each(), create_function, etc.)
- [ ] Run PHP compatibility checker: `phpcs --standard=PHPCompatibility --runtime-set testVersion 8.4 app/`

### 1.3 Database Schema Documentation
- [ ] Export full database schema (all tables, indexes, relationships)
- [ ] Document any raw SQL queries that may need updating
- [ ] Identify queries using deprecated MySQL features

### 1.4 Test Environment Setup
- [ ] Create a `docker/app-php84/` directory with PHP 8.4 Dockerfile
- [ ] Set up docker-compose profile for the new stack (PHP 8.4 + CI4)
- [ ] Create a separate database for testing migration

---

## Phase 2: PHP 8.4 Compatibility Fix (Week 3-4)

### 2.1 Fix Deprecated/Removed PHP Features
- [ ] Replace `each()` calls with `foreach` loops
- [ ] Replace deprecated `create_function()` with anonymous functions
- [ ] Fix implicit nullable parameter declarations (`function foo(Type $x = null)` → `function foo(?Type $x = null)`)
- [ ] Remove usage of `utf8_encode()` / `utf8_decode()` (removed in 8.2+)
- [ ] Fix dynamic property usage (deprecated in 8.2, fatal in 8.4) — add `#[AllowDynamicProperties]` or declare properties
- [ ] Update `strpos()`/`substr()` calls that pass null (strict type enforcement)
- [ ] Fix `${var}` string interpolation (deprecated in 8.2)
- [ ] Replace `FILTER_SANITIZE_STRING` (removed in 8.1)

### 2.2 Update Composer Dependencies
- [ ] Replace `box/spout` with `openspout/openspout` ^4.x
- [ ] Replace `phpgangsta/googleauthenticator` with `pragmarx/google2fa` ^8.0
- [ ] Replace `sendinblue/api-v3-sdk` with `getbrevo/brevo-php` (Sendinblue rebranded to Brevo)
- [ ] Update all other packages to PHP 8.4 compatible versions
- [ ] Update `composer.json` to require `"php": ">=8.4"`

### 2.3 Validate on PHP 8.4 (still CI3)
- [ ] Run the app on PHP 8.4 with CI3 to confirm basic compatibility
- [ ] Fix any remaining runtime errors/deprecations
- [ ] Test all critical flows: auth, invoicing, cron jobs, API endpoints

---

## Phase 3: CodeIgniter 4.7 Project Setup (Week 5-6)

### 3.1 Initialize CI4 Project
- [ ] Create new CI4 project: `composer create-project codeigniter4/appstarter yomiecrm-ci4`
- [ ] Configure CI4 project structure (app/, public/, writable/)
- [ ] Set up `.env` configuration (database, base URL, environment)
- [ ] Configure CI4 routing to match existing URL structure
- [ ] Set up CI4 database connection and verify access

### 3.2 Configure CI4 Core Settings
- [ ] Port `config/config.php` settings to CI4 `app/Config/App.php`
- [ ] Port `config/database.php` to CI4 `app/Config/Database.php`
- [ ] Port `config/routes.php` to CI4 `app/Config/Routes.php`
- [ ] Port `config/autoload.php` (libraries/helpers) to CI4 service registration
- [ ] Set up CI4 session handling to match current session config
- [ ] Configure CI4 CSRF protection, security headers

### 3.3 Set Up CI4 Authentication
- [ ] Implement authentication using CI4 Shield or custom auth filter
- [ ] Port login/logout/session logic from `Auth.php` controller
- [ ] Port admin auth from `admin/Auth.php`
- [ ] Port client auth from `client/Auth.php`
- [ ] Implement role-based access (admin, client, group)

---

## Phase 4: Migrate Models (Week 7-9)

### 4.1 Create CI4 Models (using CI4 Model class)
- [ ] Migrate `Admin_model.php` → `app/Models/AdminModel.php`
- [ ] Migrate `Api_model.php` → `app/Models/ApiModel.php`
- [ ] Migrate `Auth_model.php` → `app/Models/AuthModel.php`
- [ ] Migrate `Bob_model.php` → `app/Models/BobModel.php`
- [ ] Migrate `Client_model.php` → `app/Models/ClientModel.php`
- [ ] Migrate `Cron_model.php` → `app/Models/CronModel.php`
- [ ] Migrate `Helpdesk_model.php` → `app/Models/HelpdeskModel.php`
- [ ] Migrate `Import_model.php` → `app/Models/ImportModel.php`
- [ ] Migrate `Invoice_model.php` → `app/Models/InvoiceModel.php`
- [ ] Migrate `Kb_model.php` → `app/Models/KbModel.php`
- [ ] Migrate `Mandate_model.php` → `app/Models/MandateModel.php`
- [ ] Migrate `Move_model.php` → `app/Models/MoveModel.php`
- [ ] Migrate `Quote_model.php` → `app/Models/QuoteModel.php`
- [ ] Migrate `Subscriptions_model.php` → `app/Models/SubscriptionsModel.php`
- [ ] Migrate module models from `models/modules/`

### 4.2 Update Query Builder Usage
- [ ] Replace CI3 `$this->db->get()` with CI4 Query Builder syntax
- [ ] Replace `$this->db->where()` chains with CI4 equivalents
- [ ] Replace `$this->db->insert()` / `update()` / `delete()` syntax
- [ ] Replace `$this->db->query()` raw queries (ensure parameterized)
- [ ] Add model validation rules using CI4 built-in validation

---

## Phase 5: Migrate Controllers (Week 10-13)

### 5.1 Root Controllers
- [ ] Migrate `Auth.php` → `app/Controllers/Auth.php`
- [ ] Migrate `Cron.php` → `app/Controllers/Cron.php`
- [ ] Migrate `Exact.php` → `app/Controllers/Exact.php`
- [ ] Migrate `Homepage.php` → `app/Controllers/Homepage.php`
- [ ] Migrate `Import.php` → `app/Controllers/Import.php`
- [ ] Migrate `Licences.php` → `app/Controllers/Licences.php`
- [ ] Migrate `Move.php` → `app/Controllers/Move.php`
- [ ] Migrate `Pages.php` → `app/Controllers/Pages.php`
- [ ] Migrate `Pay.php` → `app/Controllers/Pay.php`
- [ ] Migrate `Payment.php` → `app/Controllers/Payment.php`
- [ ] Migrate `Sso.php` → `app/Controllers/Sso.php`
- [ ] Migrate `Table.php` → `app/Controllers/Table.php`
- [ ] Migrate `Zoho.php` → `app/Controllers/Zoho.php`

### 5.2 Admin Controllers
- [ ] Migrate `admin/Auth.php` → `app/Controllers/Admin/Auth.php`

### 5.3 API Controllers
- [ ] Migrate `api/Client.php` → `app/Controllers/Api/Client.php`
- [ ] Migrate `api/Mobile.php` → `app/Controllers/Api/Mobile.php`
- [ ] Migrate `api/Mobilev1.php` → `app/Controllers/Api/Mobilev1.php`
- [ ] Migrate `api/Printer.php` → `app/Controllers/Api/Printer.php`
- [ ] Migrate `api/V1.php` → `app/Controllers/Api/V1.php`
- [ ] Migrate `api/Webv1.php` → `app/Controllers/Api/Webv1.php`

### 5.4 Client Controllers
- [ ] Migrate `client/Auth.php` → `app/Controllers/Client/Auth.php`
- [ ] Migrate `client/Dashboard.php` → `app/Controllers/Client/Dashboard.php`
- [ ] Migrate `client/Directdebit.php` → `app/Controllers/Client/Directdebit.php`
- [ ] Migrate `client/Email.php` → `app/Controllers/Client/Email.php`
- [ ] Migrate `client/Helpdesk.php` → `app/Controllers/Client/Helpdesk.php`
- [ ] Migrate `client/Invoice.php` → `app/Controllers/Client/Invoice.php`
- [ ] Migrate `client/Kb.php` → `app/Controllers/Client/Kb.php`
- [ ] Migrate `client/Pay.php` → `app/Controllers/Client/Pay.php`

### 5.5 Group Controllers
- [ ] Inventory group/ directory controllers and migrate each

### 5.6 Controller Pattern Changes
- [ ] Replace `$this->load->model()` with CI4 dependency injection or `model()` helper
- [ ] Replace `$this->load->view()` with `return view()`
- [ ] Replace `$this->load->library()` with CI4 Services or direct instantiation
- [ ] Replace `$this->input->post()` with `$this->request->getPost()`
- [ ] Replace `$this->input->get()` with `$this->request->getGet()`
- [ ] Replace `$this->output->set_content_type()` with `$this->response->setContentType()`
- [ ] Replace `redirect('url')` with `return redirect()->to('url')`

---

## Phase 6: Migrate Libraries (Week 14-15)

### 6.1 Convert Libraries to CI4 Services/Libraries
- [ ] Migrate `Pdf.php` → `app/Libraries/Pdf.php` (update namespace, remove CI instance references)
- [ ] Migrate `Pmail.php` → `app/Libraries/Pmail.php`
- [ ] Migrate `SendInBlue.php` → `app/Libraries/Brevo.php` (rename + update to Brevo SDK)
- [ ] Migrate `Twikey.php` → `app/Libraries/Twikey.php`
- [ ] Migrate `Zohoapi.php` → `app/Libraries/Zohoapi.php`
- [ ] Migrate `Pusher.php` → `app/Libraries/Pusher.php`
- [ ] Migrate `Exportxls.php` → `app/Libraries/Exportxls.php`
- [ ] Migrate `Xlswriter.php` → `app/Libraries/Xlswriter.php`
- [ ] Migrate `Schema.php` → `app/Libraries/Schema.php`
- [ ] Migrate `Ssp.php` (DataTables server-side) → `app/Libraries/Ssp.php`
- [ ] Migrate `Wkhtmltopdf.php` → `app/Libraries/Wkhtmltopdf.php`
- [ ] Migrate `Whois.php` → `app/Libraries/Whois.php`
- [ ] Replace `$this->CI =& get_instance()` pattern with proper DI or service locator

### 6.2 Replace tcpdf Library
- [ ] Evaluate if `tcpdf` is still needed or can be replaced with a maintained alternative
- [ ] Install via composer: `tecnickcom/tcpdf` (modern version)
- [ ] Update Pdf.php to use composer-installed version

---

## Phase 7: Migrate Views (Week 16-18)

### 7.1 Port View Files
- [ ] Copy view files to CI4 `app/Views/` directory
- [ ] Update view paths to match CI4 structure
- [ ] Replace CI3 helper calls in views with CI4 equivalents:
  - `base_url()` → `base_url()` (same in CI4)
  - `site_url()` → `site_url()` (same in CI4)
  - `form_open()` → `form_open()` (load helper first)
  - `anchor()` → `anchor()` (load helper first)
- [ ] Replace `$this->session->flashdata()` with `session()->getFlashdata()`
- [ ] Update any CI3-specific view loading patterns

### 7.2 Email Templates
- [ ] Migrate email templates from `views/email/`
- [ ] Update email sending logic to use CI4 Email service

---

## Phase 8: Migrate Helpers & Hooks (Week 18-19)

### 8.1 Helpers
- [ ] Inventory all custom helpers in `application/helpers/`
- [ ] Port each helper to `app/Helpers/` with CI4 conventions
- [ ] Register helpers in BaseController or autoload config

### 8.2 CI3 Hooks → CI4 Events/Filters
- [ ] Port `config/hooks.php` to CI4 Events (`app/Config/Events.php`)
- [ ] Create CI4 Filters for before/after request processing
- [ ] Implement authentication filter (replaces hook-based auth checks)
- [ ] Implement CORS filter for API endpoints

---

## Phase 9: Cron Jobs & CLI (Week 19-20)

### 9.1 Migrate Cron to CI4 CLI Commands
- [ ] Create `app/Commands/TerminateCron.php` (spark command)
- [ ] Create `app/Commands/FixStatus.php`
- [ ] Create `app/Commands/ProcessMoveService.php`
- [ ] Create `app/Commands/GenerateReminder.php`
- [ ] Create `app/Commands/DailyInvoicing.php`
- [ ] Create `app/Commands/ZohoSync.php`
- [ ] Create `app/Commands/ProcessPeppolInvoices.php`

### 9.2 Update Docker Cron Configuration
- [ ] Update `docker/app/cronjob` to use `php spark` commands:
  ```
  0 0 * * * php /var/www/html/spark cron:terminate >> ...
  0 8 * * * php /var/www/html/spark cron:fixStatus >> ...
  ```
- [ ] Test all cron commands manually via `php spark`

---

## Phase 10: Docker & Infrastructure Update (Week 20-21)

### 10.1 Update Dockerfile
- [ ] Change base image from `php:7.4-apache` to `php:8.4-apache`
- [ ] Remove deprecated PHP extensions (mcrypt — not available for PHP 8.4)
- [ ] Replace mcrypt with openssl or sodium for any encryption
- [ ] Update PECL extensions (imagick → compatible version)
- [ ] Update composer image version
- [ ] Adjust file paths for CI4 structure (public/ as document root)

### 10.2 Update Apache Configuration
- [ ] Update document root to `/var/www/html/public` (CI4 uses public/ folder)
- [ ] Update `.htaccess` rules for CI4 routing
- [ ] Update `apache-vhost.conf` accordingly

### 10.3 Update docker-compose
- [ ] Update volume mounts for new project structure
- [ ] Update environment variables for CI4 (`.env` format)
- [ ] Test full stack startup with new configuration

---

## Phase 11: Testing & QA (Week 22-24)

### 11.1 Unit Testing
- [ ] Set up PHPUnit with CI4 testing framework
- [ ] Write tests for critical model methods (Invoice, Client, Auth)
- [ ] Write tests for API endpoints
- [ ] Write tests for cron commands

### 11.2 Integration Testing
- [ ] Test complete auth flow (login, session, logout, SSO)
- [ ] Test invoicing flow (create, send, payment, reminder)
- [ ] Test client portal (dashboard, invoices, helpdesk, KB)
- [ ] Test Exact Online integration
- [ ] Test Zoho CRM sync
- [ ] Test Twikey/Mollie payment flows
- [ ] Test Peppol invoice processing
- [ ] Test all cron jobs end-to-end

### 11.3 Performance & Security
- [ ] Run security audit (OWASP top 10 checks)
- [ ] Benchmark key endpoints vs old system
- [ ] Enable OPcache and JIT for PHP 8.4 performance gains
- [ ] Review and tighten CSP headers

---

## Phase 12: Migration & Deployment (Week 25-26)

### 12.1 Data Migration
- [ ] Verify database schema is compatible (no changes needed if same DB)
- [ ] Migrate any file storage paths if directory structure changed
- [ ] Update any hardcoded paths in database records

### 12.2 Staged Rollout
- [ ] Deploy to staging environment
- [ ] Run full regression test on staging
- [ ] Fix any issues found during staging
- [ ] Plan maintenance window for production cutover
- [ ] Deploy to production
- [ ] Monitor logs for first 48 hours
- [ ] Keep old CI3 container ready for emergency rollback

### 12.3 Cleanup
- [ ] Remove old CI3 code after 2-week stable period
- [ ] Archive old Dockerfile and configs
- [ ] Update README.md and documentation
- [ ] Update INSTALL.md for new setup process

---

## Key Risks & Mitigations

| Risk | Impact | Mitigation |
|------|--------|------------|
| Breaking change in third-party API SDK | High | Test each integration individually before full migration |
| Session/auth incompatibility | High | Run both systems side-by-side during transition |
| Database query differences CI3 vs CI4 | Medium | Keep raw queries where CI4 Query Builder doesn't fit |
| URL/route breaking changes | Medium | Map all routes first, use 301 redirects for changed URLs |
| Cron job failures during migration | High | Test all cron commands before switching over |
| Performance regression | Medium | Benchmark before/after, PHP 8.4 JIT should offset CI4 overhead |

---

## Estimated Timeline

| Phase | Duration | Cumulative |
|-------|----------|------------|
| Phase 1: Preparation & Audit | 2 weeks | Week 2 |
| Phase 2: PHP 8.4 Compatibility | 2 weeks | Week 4 |
| Phase 3: CI4 Project Setup | 2 weeks | Week 6 |
| Phase 4: Migrate Models | 3 weeks | Week 9 |
| Phase 5: Migrate Controllers | 4 weeks | Week 13 |
| Phase 6: Migrate Libraries | 2 weeks | Week 15 |
| Phase 7: Migrate Views | 3 weeks | Week 18 |
| Phase 8: Helpers & Hooks | 1 week | Week 19 |
| Phase 9: Cron Jobs & CLI | 2 weeks | Week 21 |
| Phase 10: Docker & Infra | 1 week | Week 21 |
| Phase 11: Testing & QA | 3 weeks | Week 24 |
| Phase 12: Migration & Deploy | 2 weeks | Week 26 |

**Total estimated: ~26 weeks (6 months)**

---

## References

- [CodeIgniter 4 Documentation](https://codeigniter.com/user_guide/)
- [CI3 to CI4 Upgrade Guide](https://codeigniter4.github.io/CodeIgniter4/installation/upgrade_4xx.html)
- [PHP 8.4 Migration Guide](https://www.php.net/manual/en/migration84.php)
- [PHP 8.x Deprecated Features](https://www.php.net/manual/en/migration80.deprecated.php)
