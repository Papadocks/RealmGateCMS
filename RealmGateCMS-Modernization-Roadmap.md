# RealmGateCMS — Product & Modernization Roadmap

## Delivery roadmap at a glance

This is a plan, not a completion report. Editing this document does not mean the application meets its acceptance criteria. Use the short roadmap below to plan work; keep the numbered sections as the detailed specification and backlog. Deferred features remain outside the release gate until explicitly scheduled.

| Milestone | Deliveries (section 106) | Observable result required to finish |
| --- | --- | --- |
| Foundation | 1–3 | Legacy inventory and critical tests exist; a clean Laravel/Inertia setup builds and passes CI with pinned dependencies |
| v1.0 core | 4–6 | Scoped game identities, independent CMS authentication, protected user/admin flows and basic content work through the base theme |
| v1.0 release | 7 | Clean installation, repeatable import, no-Redis operation and a measured restore pass; required legacy transitions are documented |
| v1.1 commerce | 8 | Verified payments create durable entitlements; duplicate events and ambiguous game results cannot trigger unsafe repeated delivery |
| v1.2 community | 9 | Voting, basic armory and rankings pass realm isolation, reward and query-budget checks |
| Later ecosystem | 10 | Each selected extension or emulator passes its own compatibility and lifecycle criteria; the entire backlog is not one release gate |

Section 2 governs release scope; section 106 governs dependency order and replacement boundaries; section 112 provides the acceptance matrix and evidence rules. Update all three when a scope decision changes. The remaining detailed sections refine these requirements without adding deferred features to v1.0.

---

## 1. Objective

RealmGateCMS is a World of Warcraft server CMS developed from SahtoutCMS, with a modular target architecture and initial support for AzerothCore.

RealmGateCMS is the definitive product identity. SahtoutCMS identifies the upstream project, inherited code, attribution, and migration sources only.

The objective should be to use SahtoutCMS as a **starting reference implementation for AzerothCore/WoW-specific functionality**, while progressively transforming the project into a modern, maintainable, modular and extensible World of Warcraft private-server CMS.

The end product should be:

- Modern and secure
- Easy to install
- Easy to extend
- Multi-realm capable
- Multi-emulator capable
- API-first
- Themeable
- Module/plugin based
- Suitable for community contributions
- Suitable for long-term maintenance
- Independent from one specific WoW core
- Able to support websites, launchers, Discord bots and external apps through the same API

---

# 2. Overall Strategy

The migration should be gradual.

## Release scope

The following release plan defines delivery scope. The feature sections below describe the product backlog and target architecture; they are not all requirements for v1.0. Section 112 defines the release acceptance matrix and v1.0 boundary. Release labels express planned scope, not delivery dates or completed work.

| Delivery | Planned scope |
| --- | --- |
| Foundation | Rebranding, legacy feature inventory, characterization tests for critical flows, CI and Laravel application structure |
| v1.0 | AzerothCore, multi-realm support, CMS identity, game-account linking, account security, basic character list, realm status, administration, news/pages, one base theme, installation and migration |
| v1.1 | Store, one payment provider, queued deliveries, transaction history and reconciliation |
| v1.2 | Voting, basic armory and rankings |
| Later, unversioned backlog | External plugins, advanced themes, additional payment providers, referrals, Discord, additional emulators and other optional integrations |

The initial API covers the capabilities actually delivered in v1.0. Add endpoints as their features ship; the full endpoint catalog is not a launch requirement.

Commerce is planned for v1.1. If it becomes essential to launch, explicitly revise the v1.0 scope and remove comparable optional work rather than adding it to the existing commitment.

Inherited functionality may remain available during migration, but its presence does not make a modernized replacement a v1.0 requirement. Any deployment depending on deferred functionality needs a documented transition path before switching to the new release.

## Migration approach

Do **not** rewrite everything at once.

The dependency order is:

1. Inventory inherited functionality, data and risks while establishing the RealmGateCMS identity.
2. Add characterization tests for critical legacy behavior before replacing it.
3. Introduce Laravel, environment configuration, the CMS database and CI.
4. Implement the multi-realm model and the minimal AzerothCore account adapter.
5. Implement CMS identity, authentication, authorization and audit logging together.
6. Migrate features as small, complete deliveries including their UI, services, permissions, tests and documentation.
7. Validate installation, import, production operation and recovery before releasing v1.0.
8. Deliver commerce and community features in their assigned releases; add other emulators after AzerothCore is stable.

Section 106 defines dependencies, acceptance criteria and legacy replacement boundaries for each delivery. Tests and security controls precede route replacement rather than being added at the end.

The project should evolve from:

```text
SahtoutCMS
├── includes/
├── pages/
├── assets/
├── SQL/
└── index.php
```

into the following modular monolith (one Laravel application and deployment):

```text
RealmGateCMS/
├── app/
│   ├── Http/                 # Web/API controllers and request validation
│   ├── Modules/              # Identity, Realms, Content; later Store, Voting, Armory
│   ├── WoW/
│   │   ├── Contracts/
│   │   └── Adapters/         # AzerothCore first
│   ├── Providers/            # Dependency bindings and module registration
│   └── Support/              # Small, genuinely shared utilities
├── config/
├── database/                 # CMS migrations and seeders
├── routes/                   # Web, API and console entry points
├── resources/
│   ├── js/
│   │   ├── Pages/
│   │   ├── Components/
│   │   └── Themes/default/
│   ├── css/
│   └── views/                # Inertia root and email templates
├── lang/
├── public/
├── storage/
└── tests/
```

`app/Modules` is the single location for first-party feature logic. Do not introduce parallel root `modules/` or `extensions/` trees. Frontend themes live under `resources/js/Themes`; future external packages use normal dependency packaging after the extension contract is stable. This is a target structure for migrated code, not an instruction to move unconverted legacy files immediately.

---

# 3. First Steps After Forking

Immediately after forking:

## 3.1 Establish the RealmGateCMS identity

The product name is **RealmGateCMS**; the npm package name is `realmgatecms` and the Composer package name is `realmgatecms/realmgatecms`.

The project repository is https://github.com/Papadocks/RealmGateCMS. Project links, issue links and installation instructions must point to this repository. Do not present upstream community, donation or demo links as RealmGateCMS resources.

Use SahtoutCMS only for historical provenance, inherited code, copyright attribution and migration sources. In particular, `php artisan migrate:sahtout` identifies the import source and should retain its name. Existing database names, SQL filenames and compatibility identifiers must not be renamed without a migration plan.

Rebranding acceptance checklist:

- [x] Rename this file to `RealmGateCMS-Modernization-Roadmap.md` and identify RealmGateCMS in its title and objective.
- [x] Update the README product name, description, installation repository and project links.
- [x] Distinguish the fork's development status from SahtoutCMS release history.
- [x] Update npm and Composer metadata; keep npm lockfile root metadata consistent.
- [x] Align the project metadata with the existing MIT license and preserve the original copyright notice.
- [x] Add `NOTICE.md` with explicit SahtoutCMS provenance.
- [x] Label inherited screenshots as upstream references; remove upstream branding and promotional links from the README introduction.
- [ ] Replace the application's default logo and favicon with RealmGateCMS assets.
- [ ] Update default website/footer, installer, email and translated UI branding to RealmGateCMS.
- [ ] Replace inherited screenshots with verified RealmGateCMS screenshots.
- [ ] Review internal namespaces and identifiers; preserve compatibility identifiers until their migrations exist.
- [ ] Verify the rendered website, installer and administration interface in each supported language.

The checked items cover the documentation and package identity changes. The remaining application and visual branding work must be completed before declaring the full product rebrand finished.

---

## 3.2 Clean the repository

Remove:

- Unused screenshots
- Temporary files
- Old backups
- Generated files
- Debug files
- Hardcoded test credentials
- Local development configuration
- Redundant assets
- Dead code

Add/update:

```text
.gitignore
.editorconfig
.env.example
LICENSE
NOTICE.md
CONTRIBUTING.md
SECURITY.md
CODE_OF_CONDUCT.md
CHANGELOG.md
```

---

## 3.3 Establish coding standards

Adopt:

- PSR-12 for PHP
- Strict typing where possible
- TypeScript for frontend code
- ESLint
- Prettier
- PHPStan or Psalm
- Laravel Pint or PHP-CS-Fixer

Introduce automated checks in CI.

---

# 4. Replace the Current Application Structure

One of the largest problems to solve is the current architecture where pages contain multiple concerns at once.

Avoid files such as:

```text
pages/login.php
pages/register.php
pages/account.php
pages/shop.php
```

handling:

- HTTP request parsing
- validation
- authentication
- database access
- rendering
- email delivery
- security logic
- business rules

Replace this with clear application layers.

Example:

```text
HTTP Request
    ↓
Controller
    ↓
Service
    ↓
Repository
    ↓
WoW Adapter / Database
```

Example authentication structure:

```text
AuthController
    ↓
CmsIdentityService / GameAccountService
    ↓
GameAccountGateway (game operations only)
    ↓
AzerothCore implementation
```

---

# 5. Move to a Modern PHP Framework

Recommended backend:

## Laravel

Laravel should become the application foundation.

Reasons:

- Routing
- Middleware
- CSRF protection
- Sessions
- Authentication
- Authorization
- Validation
- Caching
- Queues
- Email
- Events
- CLI commands
- Database migrations
- Database query builder
- ORM where appropriate
- API resources
- Rate limiting
- Logging
- Testing utilities
- Background jobs

This avoids maintaining custom implementations of common web infrastructure.

Selected framework baseline: **Laravel 13.x**. Use the explicit compatibility matrix in section 111 rather than “latest stable” or an unspecified LTS-compatible release.

Laravel's published support policy uses annual major releases, with 18 months of bug fixes and two years of security fixes. Laravel 13 supports PHP 8.3–8.5 according to the [official release policy](https://laravel.com/framework/docs/13.x/releases). RealmGateCMS selects a narrower initial runtime profile in section 111 and must validate its own dependencies and emulator integration before claiming support.

---

# 6. Configuration & Environment Management

Remove database credentials and secrets from PHP source files.

Use:

```text
.env
```

Single-auth-source, single-realm bootstrap example (not the multi-realm storage model):

```env
APP_NAME=RealmGateCMS
APP_URL=
APP_ENV=
APP_KEY=

CMS_DB_HOST=
CMS_DB_PORT=
CMS_DB_DATABASE=
CMS_DB_USERNAME=
CMS_DB_PASSWORD=

WOW_AUTH_HOST=
WOW_AUTH_DATABASE=
WOW_AUTH_USERNAME=
WOW_AUTH_PASSWORD=

WOW_CHARACTERS_HOST=
WOW_CHARACTERS_DATABASE=

WOW_WORLD_HOST=
WOW_WORLD_DATABASE=

WOW_SOAP_HOST=
WOW_SOAP_PORT=
WOW_SOAP_USERNAME=
WOW_SOAP_PASSWORD=
```

Never commit real secrets.

For multiple realms, resolve named connection/secret references from `AuthSource` and `Realm` records (section 23). Do not reuse one global `WOW_AUTH_*` or characters connection for every request. Store authentication connection configuration once per auth source; realms reference that source and their own game-data/command connections. Do not include secret values in public API resources.

Support environment-specific configuration:

```text
development
testing
staging
production
```

---

# 7. Separate CMS Data From WoW Data

The CMS should have its own database.

Do not treat the WoW `auth` database as the CMS database.

Recommended separation:

```text
CMS Database
├── cms_users
├── roles
├── permissions
├── role_permissions
├── news
├── pages
├── modules
├── settings
├── store_products
├── store_orders
├── transactions
├── votes
├── referrals
├── audit_logs
├── api_tokens
├── notifications
├── auth_sources
├── game_accounts
├── game_account_links
└── realms

WoW Auth Database
├── account
├── account_access
├── realmcharacters
└── ...

WoW Characters Database
├── characters
├── guild
├── item_instance
└── ...

WoW World Database
├── creature_template
├── item_template
├── quest_template
└── ...
```

The CMS should interact with WoW databases through dedicated repositories/adapters.

`game_accounts` stores CMS references to external accounts, not copies of game credentials. `game_account_links` connects those references to CMS users. `auth_sources` represents shared authentication services; multiple realms may refer to one source. The uniqueness and ownership rules in sections 23 and 87 apply to imports, queries and writes.

---

# 8. Create a WoW Core Abstraction Layer

Use small capability-oriented contracts, not one universal emulator interface. Application services depend on the contract they need; an adapter registry resolves implementations from the trusted `AuthSource` or `Realm` profile.

| Contract | Context | Responsibility |
| --- | --- | --- |
| `GameAccountGateway` | Auth source | Verify credentials, create accounts and change/reset credentials |
| `CharacterReader` | Realm | List and retrieve scoped character data |
| `RealmStatusReader` | Realm | Read game status without assuming all supporting services are healthy |
| `GameCommandGateway` | Realm | Execute typed, validated game actions supported by that profile |
| `CapabilityProvider` | Auth source or realm profile | Report supported operations and optional data features |

Use typed contexts, scoped references and explicit input/result DTOs in implementations. Account operations take an auth-source context; character/status/command operations take a realm context. Validate account-source/realm compatibility and ownership before reaching a gateway. No gateway infers a connection from a bare external ID.

Capability metadata exists from the first AzerothCore adapter. Examples include account creation, credential reset, item delivery, achievements and arena teams. A capability unavailable in the configured profile must return a defined unsupported-operation result; do not fake an empty successful response. Distinguish unsupported operations, missing records, connectivity failures and uncertain command outcomes.

The backend validates capabilities as well as permissions, and the UI uses a sanitized capability projection to show available actions. Do not base support solely on expansion names. Tests cover both supported and unsupported paths; each profile passes the common contracts it advertises.

Implement the minimal AzerothCore account and status gateways first, then character reads and command capabilities when their owning delivery requires them. Later emulator profiles can implement the same small contracts independently. Avoid speculative abstractions for unimplemented emulator features.

Domain services enforce business rules, authorization and audit behavior. Gateways isolate external schema/protocol knowledge. CMS data can use Eloquent directly inside its owning module; add repositories only where they create a useful persistence or external-system boundary.

---

# 9. Authentication Refactor

Separate the RealmGateCMS website identity from external game credentials and permissions. A CMS user can exist without a linked game account. A game account can remain usable in the game without a CMS association.

## Service and credential boundaries

```text
CmsIdentityService
├── login / logout / session revocation
├── register / verifyEmail
├── changeCmsPassword / resetCmsPassword
└── CMS MFA and recovery

GameAccountLinkService
├── proveControl / link
├── unlink
└── association history

GameAccountService → AuthSource adapter
├── verifyGameCredentials
├── createGameAccount
└── changeGamePassword / resetGamePassword
```

These are proposed service boundaries, not existing APIs. CMS passwords use the framework password hasher and its upgrade policy. SRP6 and other emulator-specific credential formats belong only inside the game adapter. Do not copy game verifiers into CMS password fields, derive a website password from a verifier or persist submitted game passwords in CMS records, queues or logs.

Do not synchronize CMS and game passwords. Even if a user chooses identical text, each credential has a separate purpose, storage mechanism and lifecycle. CMS roles never follow automatically from game GM/security levels, and CMS administration does not implicitly grant game privileges.

## Existing users and first access

Import an identity as pending activation where the source does not provide a separately validated CMS credential and verified recovery channel. Preserve the source-to-target mapping and historical data without enabling an unproven login or account link.

For the inherited game-credential login flow, first access requires explicit proof of control through the selected auth-source adapter, verification of the new CMS email and selection of a separate CMS password. Consume a short-lived, single-use activation context bound to the source identity and external account. Then activate the CMS identity and establish the eligible link. Do not automatically log users into the new CMS using old PHP sessions.

If the source contains an independently verified website identity, use an explicit, tested import mapping and the documented CMS activation/recovery path. Never treat an imported email string as verified merely because it exists. Duplicate emails, missing credentials, an account already linked elsewhere or ambiguous mappings require conflict resolution; do not merge accounts or create a new owner silently.

Imported privileged roles require operator review and the administrator MFA policy before privileged access. Users unable to prove control enter the documented operator-assisted recovery process; importing a row is not proof of ownership.

## Password and recovery behavior

| Action | Required authorization | Effect |
| --- | --- | --- |
| Change CMS password | Recent CMS re-authentication and enrolled MFA | Update the CMS credential and revoke other CMS sessions/tokens according to policy; game credentials remain unchanged |
| Recover CMS access | Single-use, expiring token to the verified CMS recovery address; satisfy MFA or its recovery procedure | Reset the CMS credential and revoke existing sessions/tokens; do not reset game passwords or silently remove MFA |
| Change game password | Recent CMS re-authentication/MFA, active verified link and proof of the current game credential | Update only the selected external account through its auth-source adapter |
| Recover a linked game account | Recent CMS re-authentication/MFA, an established verified link and confirmation through the verified CMS recovery address | Reset only that external credential through the adapter; a newly asserted link or matching email alone is insufficient |
| Recover an unlinked/disputed game account | Operator-assisted proof review under a documented policy | No automatic claim, reassignment or password reset based solely on email, username or CMS access |

Any cooldown after CMS recovery, email change or linking must be defined in configuration and tested before enabling self-service game recovery. Until an installation has an approved recovery policy, use operator-assisted game recovery. Do not require the old game password for a lost-password recovery flow; distinguish it from an ordinary password change.

Recovery tokens are bound to their purpose, identity and, for game operations, the exact game-account reference. They expire, are consumed once and cannot be reused across CMS and game endpoints. Return non-enumerating recovery responses and rate-limit attempts. Never email plaintext passwords.

CMS MFA protects website operations; it does not establish MFA for the game client. The adapter must report whether a game credential change can revoke existing game sessions. Do not claim that players were disconnected unless that operation was supported and confirmed. Failed or uncertain adapter writes must be reported and reconciled without changing CMS credentials or claiming success.

## Email changes and suspensions

CMS email changes require recent re-authentication/MFA, verification of the new address and a notice to the previous verified address. They do not modify game-account email fields or transfer account links. Game email changes, if supported later, are separate adapter operations with their own authorization policy.

| State | Default behavior |
| --- | --- |
| CMS suspended | Deny authenticated CMS access, token use, linking and game-account management; revoke CMS sessions/tokens, including the legacy bridge where applicable |
| Game account banned | Preserve its CMS association and permit permitted CMS access; display the restriction and prohibit CMS actions that would bypass the ban |
| Realm unavailable/disabled | Block affected realm operations; preserve the user, shared account and sibling realms |

CMS suspension does not automatically ban the player in the emulator. A game ban does not automatically suspend the website identity. Any combined administrative action needs explicit permissions, defined scope, audit records and separate results for each system; partial success must remain visible. Recovery, relinking or password changes must never clear a suspension or game ban implicitly.

## Identity acceptance scenarios

Delivery 5 must demonstrate that:

- An existing game-login user can activate a separate CMS identity only after completing the required proofs.
- An imported or matching email alone cannot activate, merge or claim accounts.
- Two concurrent link attempts cannot assign one game account to two CMS owners.
- Changing or recovering one credential leaves the other credential unchanged.
- A CMS recovery token cannot reset a game password, bypass MFA or remove a suspension.
- A stolen/stale link challenge, unlinked account or cross-auth-source identifier cannot authorize game recovery.
- Unlinking preserves the external account and characters and revokes the old owner's CMS management access.
- CMS suspension and game bans apply independently and cannot be bypassed through another route or relinking.
- Adapter failure does not produce a false password-change success or expose credentials in logs.

---

# 10. Add Strong Authentication Security

Implement:

- Secure cookies
- HttpOnly cookies
- SameSite cookies
- HTTPS-only production mode
- Session ID rotation
- Session invalidation
- Session timeout
- Device/session management
- Login history
- IP history
- Failed login tracking
- Configurable account lockout
- Rate limiting
- Optional 2FA/TOTP
- Email verification
- Password reset tokens
- CSRF middleware
- Anti-replay protections where relevant

Admin accounts should support mandatory 2FA.

---

# 11. Centralized CSRF Protection

Remove page-level manual CSRF logic.

Use framework middleware globally for browser forms.

All state-changing actions should require CSRF protection:

```text
POST
PUT
PATCH
DELETE
```

API clients should use bearer tokens instead.

---

# 12. Authorization & RBAC

Replace simple role checks such as:

```text
admin
moderator
player
```

with a proper RBAC system.

Recommended model:

```text
Users
↓
Roles
↓
Permissions
```

Example permissions:

```text
accounts.view
accounts.edit
accounts.ban

characters.view
characters.edit
characters.teleport
characters.rename

news.create
news.edit
news.delete

store.manage
store.refund

realms.manage

users.manage
roles.manage
permissions.manage

server.execute_command

logs.view
```

Support:

- Custom roles
- Per-role permissions
- Per-user overrides
- Module permissions

---

# 13. Audit Logging

Every sensitive administrative action should generate an audit log.

Examples:

```text
Admin changed user email
Admin banned WoW account
Admin changed role
Admin executed SOAP command
Admin refunded store purchase
Admin edited realm configuration
Admin deleted news article
```

Log:

```text
actor
action
target
before state
after state
timestamp
IP
user agent
```

Audit logs should be immutable through the normal admin UI.

---

# 14. API-First Design

Create a versioned API:

```text
/api/v1/
```

Target endpoint catalog, delivered incrementally with the corresponding features. Store, voting, guild and full armory endpoints are not v1.0 requirements:

```text
/api/v1/auth
/api/v1/users
/api/v1/accounts
/api/v1/characters
/api/v1/realms
/api/v1/guilds
/api/v1/armory
/api/v1/rankings
/api/v1/store
/api/v1/news
/api/v1/votes
```

Benefits:

- Website frontend
- Desktop launcher
- Mobile application
- Discord bot
- Admin dashboard
- External integrations

can all use the same backend.

---

# 15. API Authentication

Support:

- Web sessions
- API tokens
- Personal access tokens
- Optional OAuth2 later

API tokens should have scopes:

```text
characters:read
account:read
account:write
store:read
admin:read
admin:write
```

---

# 16. Module System

Build a modular monolith: one Laravel application, one release artifact and shared infrastructure, with first-party feature logic under `app/Modules`.

Initial modules are Identity, Realms and Content. Add Store in v1.1 and Voting/Armory/Rankings in v1.2. Each module owns its use cases, models, policies, events and internal helpers. Use simple folders; do not require every module to implement a full controller/service/repository stack if it adds no useful boundary.

Keep HTTP entry points in `app/Http`, route registration in `routes`, CMS migrations in `database`, translations in `lang` and React views in `resources/js`, grouped by feature. Module ownership is logical and documented; it does not require duplicate migration, route or asset loaders.

Modules collaborate through explicit application services/contracts or events. Do not directly modify another module's tables or depend on its private implementation classes. Game database access always uses the scoped gateways in section 8.

For v1.0, modules are shipped and registered with the application. Feature toggles may hide supported optional functionality but must not disable required identity/security dependencies. Installing executable plugins, public lifecycle hooks and external package discovery remain later ecosystem work.

Distinguish deployment from activation: an authorized administrator may enable/disable only optional features whose code and compatible schema are already deployed. Check dependencies and pending work before disabling; disabling a store must preserve reconciliation and existing orders. Neither activation nor deactivation installs code, runs migrations or deletes records. Core identity, authorization and audit controls cannot be disabled. Section 48 governs later distribution.

---

# 17. Module Manifest

A future external plugin should include a manifest. This example is an ecosystem design sketch, not a v1.0 loader requirement. First-party modules are registered through Laravel providers.

Example:

```json
{
  "name": "Armory",
  "slug": "armory",
  "version": "1.0.0",
  "author": "RealmGateCMS Team",
  "requires": {
    "cms": ">=1.0.0"
  },
  "permissions": [
    "armory.view",
    "armory.manage"
  ]
}
```

---

# 18. Theme System

Ship one responsive React base theme in v1.0 under `resources/js/Themes/default`. Themes own presentation: layouts, reusable visual components, styles, assets and validated configuration metadata. Feature pages keep their use-case behavior and consume theme components through a small typed presentation contract.

```text
resources/js/Themes/default/
├── Layouts/
├── Components/
├── assets/
├── tokens.css
├── theme.json
└── index.ts
```

Laravel prepares authorized page props. Themes receive presentation data, never database connections, secrets or permission-enforcement responsibility. Backend policies remain authoritative regardless of whether an action is visible in the theme.

Build React/TypeScript and CSS assets with Vite as part of the release. Theme component changes require a new build/deployment. Basic branding values such as validated colors and asset URLs can be supplied as runtime settings/CSS custom properties without compiling administrator-supplied code. Use an explicit registry for compiled theme components rather than importing arbitrary filesystem paths from settings.

The admin interface uses a stable shared layout in v1.0. Advanced theme switching, third-party theme installation and live preview remain deferred. When multiple themes are introduced, require compatibility checks against the presentation contract and ship their compiled assets with the release.

# 19. Theme Configuration

v1.0 settings cover logo, favicon, primary/secondary colors, approved background/hero assets, footer text and social links. Validate values and escape displayed content; settings are data, not JavaScript, raw CSS or executable templates.

Use a documented schema in `theme.json`, with types, defaults and allowed values. Resolve media through the CMS media/storage layer. Keep secrets and game configuration outside theme settings. Navigation layout variants and live preview are later enhancements.

---

# 20. Modern Frontend

Selected integration: **Laravel + Inertia + React + TypeScript**, with Tailwind CSS and Vite. Website and administration live in the same repository and release as the backend; a standalone Next.js application is not part of v1.0.

Web controllers validate requests and call application services, then return Inertia pages with authorized props. API controllers call the same services and return versioned JSON resources. Business rules and policy checks must not be duplicated or bypassed between these entry points. The website does not make internal HTTP calls to its own REST API to execute these use cases.

This matches Inertia's server-driven model, which uses backend routing/controllers and does not require a separate API for its pages. See the [official Inertia introduction](https://inertiajs.com/docs/v2/getting-started). External clients use `/api/v1` for the features actually delivered, with their appropriate token permissions; browser pages use sessions and CSRF protection.

Use typed page props, accessible shared components and responsive layouts. Validate forms on the server even where the client provides feedback. Define asset versioning and test navigation, forms and authorization failures in the foundation prototype.

CSR is the initial deployment baseline. Public-page metadata and indexing requirements must be checked during delivery 6; if SSR is necessary, record and test the additional runtime/deployment requirement before release rather than implying that Inertia automatically provides server-rendered page content.

---

# 21. Admin Panel Redesign

Create a proper admin application.

Suggested navigation:

```text
Dashboard

Users
├── Website users
├── Game accounts
├── Roles
└── Permissions

Content
├── News
├── Pages
└── Media

Game
├── Realms
├── Characters
├── Guilds
└── Commands

Commerce
├── Products
├── Orders
├── Payments
└── Coupons

Community
├── Voting
├── Referrals
└── Events

System
├── Modules
├── Themes
├── Settings
├── Logs
├── Jobs
└── Updates
```

---

# 22. Dashboard Improvements

Admin dashboard should show:

- Realm status
- Current online players
- New registrations
- Store revenue
- Failed login attempts
- Pending orders
- Recent admin actions
- Queue failures
- Server alerts
- Database connectivity
- SOAP connectivity

---

# 23. Realm Management

Model the authentication service separately from a realm. Several realms can share the same authentication database and account population, while using different characters databases and command endpoints.

| Entity | Purpose | Required identity and relationships |
| --- | --- | --- |
| `AuthSource` | One logical game authentication service/account namespace | CMS `id`, name, authentication adapter/profile, auth connection secret reference, enabled state |
| `GameAccount` | CMS reference to an existing external game account | CMS `id`, `auth_source_id`, `external_account_id`; unique pair `(auth_source_id, external_account_id)` |
| `Realm` | One playable realm registered under an auth source | CMS `id`, `auth_source_id`, `external_realm_id`, name, emulator/profile, expansion, characters/world connection references, command endpoint reference, address/port, enabled state and display order |
| `GameAccountLink` | Association between a CMS user and a game-account reference | CMS `id`, `cms_user_id`, `game_account_id`, verified/link timestamp; one current owner per game account |

`AuthSource.id`, `Realm.id` and `GameAccount.id` are CMS identifiers. They are not the external IDs stored in the emulator. Enforce uniqueness of `(auth_source_id, external_realm_id)` for registered realms. Configure the auth source and realm adapter profiles compatibly and validate the selected connections before enabling a realm.

A CMS user can link multiple game accounts, including accounts from different auth sources. An external account shared by several realms has one `GameAccount` reference and one ownership link, not one copy per realm. Keep link history separately from the current association if needed; enforce the current-owner constraint in the database, not only in application checks.

```text
CMS user
    → GameAccountLink
        → GameAccount (AuthSource A, external account 42)
            → characters in Realm X (AuthSource A)
            → characters in Realm Y (AuthSource A)

Another GameAccount (AuthSource B, external account 42)
    → a distinct account, despite the same external ID
```

The relation to characters is resolved through the selected realm's character repository; characters are not required to be copied into the CMS database. An account's auth source must match the realm's source before resolving its characters. A match establishes the correct namespace, not permission to view or modify that account.

Register a shared authentication service once and reference it from its realms. Connection credential rotation updates its secret reference without changing account identity. Repointing a source to a different account namespace, reassigning a realm to another source or restoring unrelated data under an existing identity requires an explicit migration and revalidation of affected links and records.

Support this model from the first AzerothCore release, including installations with one auth source and one realm.

---

# 24. Realm Health Checks

Automatically check:

```text
Auth DB connection
Characters DB connection
World DB connection
SOAP connection
Game server port
Realm online status
```

Check each shared auth source once per health-check cycle and show its failure on all affected realms. Check characters/world connections and command endpoints in their realm context. Disabling one realm must not disable sibling realms or delete shared accounts.

Display failures clearly in admin.

---

# 25. Server Command Layer

Do not call SOAP commands from arbitrary controllers.

Create:

```text
ServerCommandService
```

Example:

```php
execute()
teleport()
renameCharacter()
sendItem()
sendMail()
banAccount()
unbanAccount()
```

Validate all commands and arguments.

Every command should be audit logged.

---

# 26. Character Service

Create a dedicated character domain.

Possible features:

- Character profile
- Equipment
- Inventory
- Achievements
- PvP stats
- Guild
- Reputation
- Professions
- Talents
- Mounts
- Recent activity

Access WoW tables only through character repositories.

---

# 27. Armory Redesign

Armory should become a first-class module.

Features:

```text
Character search
Character profiles
Equipment
Talents
Achievements
Guild pages
Arena/PvP rankings
PvE progress
Realm filters
```

Cache expensive queries.

---

# 28. Store Redesign

Keep commerce rules separate from game integration. Target release: v1.1, with one payment provider, queued delivery, transaction history and reconciliation. The wallet remains deferred.

Persist `Product`, `Order`, `OrderItem`, `PaymentAttempt`, `PaymentEvent`, `Transaction`, `DeliveryIntent`, `DeliveryAttempt` and outbox records. A queue job is an execution mechanism, not the authoritative record of whether a reward is owed or delivered.

At checkout, snapshot product/reward definitions, quantity, price, currency and the scoped account/realm/character destination. Calculate totals server-side using integer minor units appropriate to the currency. Later product edits must not change an existing order's entitlement. Prevent replayed checkout requests from creating unintended duplicate orders using a user-scoped idempotency key and request fingerprint; reject reuse with different input.

## Independent state machines

| Aggregate | States / recorded outcomes | Rules |
| --- | --- | --- |
| Payment attempt | `pending`, `confirmed`, `failed`, `cancelled` | Only verified provider evidence confirms payment; retain each attempt and its provider references |
| Order | `awaiting_payment`, `ready`, `fulfilling`, `completed`, `cancelled`, `needs_attention` | Ready requires payment eligibility; completed requires every required delivery intent to be confirmed |
| Delivery intent | `pending`, `in_progress`, `delivered`, `retryable_failure`, `unknown`, `cancelled` | Unknown is not a failed or successful delivery; it blocks automatic re-execution without safe evidence |
| Refund/dispute | Separate linked records and status history | Do not rewrite a successful payment or delivery as if it never happened |

Record allowed transitions explicitly and enforce them transactionally with row locks or conditional updates. Keep state history, timestamps and evidence references. A delayed pending/failed notification must not undo a verified payment confirmation. Multiple payment attempts can exist, but fulfillment entitlement is created once per order item/reward unit; an extra confirmed payment is a reconciliation case, not permission for duplicate rewards.

Cancellation stops only unexecuted work under a defined policy. Partial delivery, refund requests and disputes remain visible as separate facts. Never claim the game-side reward has been revoked merely because an order is cancelled or money was refunded.

## Durable delivery entitlement

Within one CMS transaction, accept the verified payment state, create uniquely keyed delivery intents and write their outbox records. Use a stable uniqueness constraint such as `(order_item_id, reward_unit)` so duplicate or concurrent event processing cannot create a second entitlement.

An outbox dispatcher publishes pending work and can safely publish it again after a crash. Workers load the persisted intent and check its state before doing anything. If payment confirmation commits but the queue is unavailable, the outbox retains the work for later dispatch; queue availability must not determine whether the entitlement is recorded.

The CMS database transaction cannot atomically commit a remote game command. The delivery protocol in section 30 handles that boundary explicitly.

---

# 29. Payment Provider Abstraction

Define a `PaymentProviderInterface` for checkout creation, webhook verification, payment-status lookup and supported refund operations. Implement one provider in v1.1; PayPal, Stripe and other providers are future choices rather than simultaneous requirements. Provider-specific status mappings and verification rules belong inside its adapter.

## Webhook processing

1. Verify authenticity using the provider's documented mechanism and the original request bytes where required. Validate environment, merchant/recipient, provider transaction reference, expected order, amount and currency. Browser redirects and client-supplied success flags never confirm payment.
2. Apply the provider's timestamp/replay rules where supported. Persist the verified event in a durable inbox with a unique key scoped by provider, merchant/environment and event ID. Where event IDs are unavailable, define and test a provider-specific deduplication key before enabling that integration.
3. Acknowledge only after durable recording. A valid already-recorded event can be acknowledged without repeating its effects. Return the appropriate failure response if recording fails so the provider can retry according to its protocol. Invalid events must not change business state.
4. Process the inbox asynchronously. Deduplicate business effects by payment reference and delivery entitlement as well as event ID: two distinct events can describe the same payment. Resolve reordered/conflicting events through the allowed state transitions and an authoritative provider lookup where necessary.
5. Atomically update payment/order state and create the outbox work described in section 28. Do not send game commands in the callback handler.

Persist only the event data/evidence needed for reconciliation, with access controls, redaction and retention rules. Never put payment secrets or full sensitive payloads into general logs.

## Reconciliation

Run scheduled reconciliation for pending/ambiguous payments, unprocessed inbox records, undispatched outbox records, stalled deliveries, refunds and disputes. Compare provider state with local references and surface mismatches to administrators. An unavailable provider yields an unresolved case, not an assumed success or failure.

Record every automated or administrative correction with actor, reason, previous/new state and evidence. Administrative intervention must use controlled service operations, not direct database edits. Provider refund initiation also uses a stable request key/status lookup where supported so retrying an uncertain request does not issue another refund.

---

# 30. Delivery Queue

Execute rewards through the typed, scoped game-command gateway and persisted delivery intents. Assume queue messages may be delivered more than once. A queue and a CMS idempotency key alone do not guarantee exactly-once game execution.

## Worker protocol

1. Claim the intent atomically and record a delivery attempt with a unique operation reference. A duplicate worker must not claim the same active intent. Revalidate payment eligibility, the snapshotted destination, link ownership, realm mapping and required capabilities before execution; invalid or changed destinations require review rather than silent rerouting.
2. Persist `in_progress` and the attempt reference before sending the command. Record whether the adapter supports downstream idempotency and an authoritative operation-status query. Do not put credentials in the job payload.
3. Send the command with the stable intent/operation key if the game endpoint supports durable deduplication. Record the returned receipt and verification evidence. Mark `delivered` only on evidence that the defined reward was applied, not merely that a command was accepted for later processing.
4. Update order progress from confirmed intents. An order completes only when all required rewards are confirmed. Previously delivered intents remain delivered when another unit fails.

Claim leases or worker locks prevent local concurrent execution; their expiry is not proof that the game command never ran. A restart with an unfinished attempt must enter reconciliation before any unsafe resend.

## Retry and unknown-result policy

| Outcome | Required action |
| --- | --- |
| Failure proven to occur before command submission | Bounded retry with backoff, after revalidation |
| Explicit rejection with guaranteed no side effect | Retry only if the documented cause is transient; otherwise require correction/review |
| Confirmed application with an attributable receipt/status | Persist `delivered`; repeated queue messages become no-ops |
| Timeout, lost response, crash after submission or ambiguous partial effect | Persist `unknown` and reconcile; do not blindly resend |
| Downstream operation has durable deduplication/status lookup | Reconcile or retry with the same stable key within the endpoint's documented guarantee |

The critical case is **the server delivered the item, but the response was lost**. Without downstream deduplication or attributable status evidence, automatic retry can duplicate the reward. The default for an ordinary command endpoint without those guarantees is to stop automatic execution and open an administrative case.

Current inventory alone is not reliable proof of a particular delivery: an item may have existed before or been consumed/transferred. Use a command receipt, operation-specific server record or another documented authoritative check. If none exists, keep the result unresolved until an operator records sufficient evidence and an explicit resolution. Do not advertise exactly-once delivery for that adapter.

The admin view shows the order/item, destination, attempts, receipts, timestamps, errors and reconciliation status. Permit evidence-backed confirmation, proven-safe retry or a recorded cancellation/refund resolution through separate permissions and audit logs. A generic “retry all failed jobs” action must never resend `unknown` game operations automatically.

## Commerce acceptance tests

Delivery 8 must cover:

- Invalid signatures, wrong merchant/environment, amount or currency cannot confirm an order.
- Duplicate and concurrent callbacks, including different event IDs for one payment, create one entitlement per reward unit.
- Reordered events and multiple successful payment attempts do not regress state or duplicate fulfillment.
- A crash after payment commit but before queue publication is recovered from the outbox.
- Duplicate queue messages and concurrent workers do not execute an already confirmed intent again.
- A crash before submission is handled conservatively unless non-submission can be proven.
- A command applies a reward and its response is lost: the intent becomes `unknown`; no unsafe automatic resend occurs.
- A crash after game success but before persisting success follows the same reconciliation path.
- A lease expires while the first worker may still be running: a second worker does not blindly execute the command.
- Partial orders, disabled realms, changed ownership, refunds and disputes preserve accurate history and block inappropriate delivery.
- Provider/queue/game outages, exhausted retries and manual resolutions leave visible, recoverable records.

Use sandbox payment events and a controlled command adapter capable of injecting failures at these boundaries. Do not test fulfillment against live player accounts.

---

# 31. Wallet System

Optional wallet system:

```text
Balance
Transactions
Credits
Bonuses
Refunds
Promotional currency
```

Keep financial records immutable.

---

# 32. Voting System

Refactor voting into a module.

Target release: v1.2, alongside basic armory and rankings. Advanced community features remain optional backlog work.

Support:

- Multiple vote sites
- Cooldowns
- Vote verification
- Reward configuration
- Fraud detection
- Vote history
- Realm-specific rewards

---

# 33. Referral System

Create a proper referrals module.

Features:

- Referral codes
- Referral links
- Signup attribution
- Milestones
- Rewards
- Fraud protection
- Referral statistics

---

# 34. News & Content CMS

Build a modern content editor.

Support:

- Drafts
- Scheduled publishing
- Categories
- Tags
- Featured image
- SEO metadata
- Markdown or rich text
- Revision history
- Author attribution

---

# 35. Static Pages

Support custom pages from admin:

```text
About
Rules
Connection Guide
Downloads
Support
FAQ
Terms
Privacy
```

Pages should use slug-based routes.

---

# 36. Media Library

Create a central media manager.

Support:

- Image upload
- File upload
- Validation
- Size limits
- Thumbnails
- Alt text
- Folder organization
- Storage abstraction

Potential storage:

```text
Local
S3
Cloudflare R2
```

---

# 37. Localization

Move language strings into proper translation files.

Support:

```text
English
Portuguese
Spanish
French
German
Russian
etc.
```

Modules and themes should be able to provide translations.

Avoid hardcoded UI strings.

---

# 38. Timezone & Locale Handling

Store dates in UTC.

Display them according to:

```text
site timezone
user timezone
user locale
```

Use proper date/time libraries.

---

# 39. Caching

Introduce configurable caching.

Potential cache targets:

```text
realm status
online players
rankings
armory
guild pages
item data
news
configuration
```

Support:

```text
filesystem
Redis
```

---

# 40. Redis Support

Redis is optional, but durable asynchronous processing is required in production.

The baseline without Redis is a single application host using the CMS database for durable queues, sessions and shared cache/lock/rate-limit state. Provision the required framework tables through migrations. Local filesystem caching may serve noncritical, host-local content; it must not become the source of delivery state or coordination across workers.

Redis can replace selected queue/cache/session backends after configuration and recovery tests. Losing cache data must not erase orders, delivery intents, inbox/outbox records or audit evidence. Those remain in the CMS database.

Multiple application hosts require shared sessions, rate limits and locking, plus shared media storage. They are an additional deployment profile to validate, not an implicit capability of a local-file configuration. Test the no-Redis baseline in CI and release validation rather than only the Redis profile.

---

# 41. Queue System

Use durable queues for emails, notifications, large background work and, from v1.1, commerce processing. Run supervised long-lived workers with the same application release and configuration as the web process. Redis is not required; the CMS database queue is the baseline.

Document queue names, worker concurrency, job timeouts, backend visibility/retry settings, maximum attempts and backoff. Verify that an active job is not reclaimed under normal timeout settings. These settings do not replace the unknown-delivery safeguards in section 30.

Track worker heartbeat, queue depth, oldest pending job age and failed jobs. Define alert thresholds in the deployment runbook. A successful web request or writable queue table does not prove that workers are processing jobs.

Restart workers gracefully during deployment so old code does not execute against an incompatible schema. Reserve worker capacity for identity emails and operational tasks; expensive snapshots/imports must not starve them. Limit game-facing concurrency per realm.

A missing worker makes dependent operations unavailable or visibly pending; do not silently fall back to synchronous reward delivery or in-request bulk work. Failed-job actions require permissions and the owning operation's retry policy, especially for unknown game outcomes.

---

# 42. Scheduler

Production requires an operating-system scheduler invoking the application schedule every minute. The deployment runbook supplies the actual command, application path and service user for the selected release.

Schedule realm health checks, bounded cache/snapshot refresh, expired-token cleanup and backup checks. Enable scheduled publishing, voting maintenance, statistics and commerce reconciliation only when those features ship.

Record scheduler heartbeat and per-task last success/failure. Prevent overlapping runs through shared coordination; multi-host deployments designate one scheduler or use tested shared locks. Define missed-run behavior for each task rather than replaying all tasks blindly after downtime.

Release diagnostics must prove an actual scheduled run and an actual queued job completed. Page traffic is not a substitute for cron or workers.

---

# 43. Email System

Centralize email handling.

Support:

```text
SMTP
Mailgun
Postmark
Amazon SES
SendGrid
```

Templates:

```text
Verify email
Password reset
Login alert
Purchase confirmation
Account security notification
Admin alert
```

---

# 44. Notifications

Create a notification system supporting:

```text
Email
In-app
Discord webhook
Optional push notifications
```

Examples:

- Account login
- Password change
- Purchase complete
- Realm offline
- Admin warning

---

# 45. Discord Integration

Optional module.

Features:

```text
Discord OAuth
Role synchronization
Server status webhook
News notifications
Store purchase notifications
Account linking
```

---

# 46. Installer Redesign

Create a proper web installer or CLI installer.

Installer should:

1. Check PHP version
2. Check extensions
3. Check directory permissions
4. Configure CMS database
5. Configure WoW database
6. Detect emulator/core
7. Test SOAP
8. Run migrations
9. Create admin user
10. Generate application key
11. Write `.env`
12. Lock installer

Installation is not production-ready until the CLI/worker/scheduler requirements in section 67 and compatibility diagnostics in section 47 pass. A web installer cannot bypass those prerequisites.

---

# 47. Core Detection and Tested Compatibility

Automatic schema detection is advisory. It can suggest an adapter, but it cannot certify compatibility or enable unverified write capabilities. Permit explicit profile selection and run read-only compatibility diagnostics before activation.

Maintain a version-controlled compatibility manifest for each advertised emulator profile. Record:

- Emulator name, expansion/client build and exact tested core commit or release.
- Auth, characters and world schema revisions, including relevant custom modules/schema changes.
- Database engine/version, required privileges, transport configuration and tested command capabilities.
- Adapter version, fixture/snapshot provenance, test date and linked integration/contract-test results.
- Known limitations and operations that are read-only, unsupported or require operator review.

AzerothCore support remains a target until this evidence exists; a broad name such as “AzerothCore WotLK” is not enough to claim every revision works. Unknown or mismatched schemas fail closed for writes and show a diagnostic; read-only access is allowed only where its compatibility checks pass. Manual selection does not bypass these checks.

Verify account operations, scoped identifiers, status reads and applicable command behavior against isolated fixtures. Re-run the relevant checks when a core, schema, adapter or command profile changes. Do not modify live game schemas to force a detection match.

---

# 48. Update and Extension Distribution

For v1.0/v1.1, updates are operator-managed releases: obtain the reviewed release artifact and locked dependencies, rehearse migrations and recovery, then deploy through the documented maintenance process. The web/admin process does not download, install or execute update packages.

Automatic updates and remote plugin/theme installation are deferred. Do not expose a one-click downloader, arbitrary package URL or archive upload as an early substitute for a distribution design.

Before enabling a later update system, define publisher trust, verified artifact integrity/authenticity, dependency and CMS compatibility, migration ordering, interruption recovery and rollback limits. Verification of a signature establishes origin/integrity, not that code is safe. Test the process with revoked/untrusted artifacts, incompatible versions and interrupted installations.

External PHP plugins run with application privileges unless a separate isolation mechanism is deliberately implemented. Permission declarations and module manifests are not a sandbox. Install only operator-approved code through the deployment pipeline; the admin panel may enable already-deployed, compatible optional features under the rules in section 16.

---

# 49. Database Migrations

All CMS schema changes should use migrations.

Avoid manually telling users to import random SQL files.

Use:

```bash
php artisan migrate
```

First-party CMS migrations remain in the central `database` tree with documented module ownership (section 16). Future external package migrations require a compatible, operator-run deployment lifecycle; enabling a feature in admin must not silently run schema changes.

---

# 50. Seeders

Provide seeders for:

```text
default roles
permissions
settings
demo content
default modules
```

---

# 51. Testing

This must become a major part of the project.

## Unit tests

Test:

- SRP6 logic
- Services
- Permissions
- Validation
- Core adapters
- Payment calculations

## Integration tests

Test:

- AzerothCore DB
- SOAP
- Registration
- Authentication
- Password changes
- Store delivery

## Feature tests

Test:

- Login
- Registration
- Admin permissions
- Purchases
- Voting
- Realm switching

---

# 52. Emulator Adapter Test Suite

Each emulator profile must pass the shared contract tests for the small gateways and capabilities it advertises (section 8), including unsupported-operation cases.

Example:

```text
createAccount()
changePassword()
getCharacters()
getRealmStatus()
executeCommand()
```

This ensures consistent behavior for supported capabilities without requiring every core to implement every feature.

---

# 53. Continuous Integration

GitHub Actions should run on every PR.

Suggested pipeline:

```text
Composer install
NPM install
Lint PHP
Lint TypeScript
Static analysis
Unit tests
Integration tests
Build frontend
Security audit
```

Do not merge failing builds.

---

# 54. Static Analysis

Use:

```text
PHPStan
ESLint
TypeScript strict mode
```

Gradually increase PHPStan level as legacy code is migrated.

---

# 55. Dependency Security

Automate:

```text
composer audit
npm audit
Dependabot
```

Review dependencies regularly.

---

# 56. Logging

Use structured logging.

Log channels:

```text
application
authentication
payments
server commands
database
security
jobs
API
```

Do not log passwords, tokens or private credentials.

---

# 57. Error Handling

Production should never show raw PHP errors or SQL errors.

Display generic messages to users.

Log full exceptions server-side.

---

# 58. Rate Limiting

Rate limit:

```text
Login
Registration
Password reset
API
Search
Voting
Store checkout
SOAP-related actions
```

Support configurable limits.

---

# 59. Input Validation

All user input should go through centralized validation rules.

Never trust:

```text
GET
POST
JSON API
headers
uploaded files
```

Use request DTOs or framework request validators.

---

# 60. Output Escaping

Ensure all user-generated content is escaped by default.

For rich text:

- sanitize HTML
- whitelist allowed tags
- remove scripts
- remove dangerous attributes

---

# 61. File Upload Security

Validate:

```text
MIME type
extension
size
dimensions
filename
```

Generate server-side filenames.

Store uploads outside executable paths where possible.

---

# 62. Content Security Policy

Add a proper CSP.

Restrict:

```text
scripts
styles
frames
images
fonts
connections
```

Avoid inline scripts where possible.

---

# 63. Security Headers

Enable:

```text
Content-Security-Policy
X-Content-Type-Options
Referrer-Policy
Permissions-Policy
HSTS
frame-ancestors
```

---

# 64. Backup Strategy

The production runbook must assign an operator and specify backup frequency, retention, recovery-point objective (maximum acceptable data loss) and recovery-time objective (target restoration time). Set numeric objectives for the actual installation before launch and measure them in restore drills.

Back up the CMS database, uploads, configuration/secret material needed for recovery, and the matching application release/lockfiles. Protect application encryption keys: recreating a key is not a substitute for restoring the one needed to decrypt existing data. Restrict and encrypt backup storage and keep a copy outside the application host.

Game database backups remain the game operator's responsibility; do not give the CMS broad backup privileges merely for convenience. Coordinate consistent recovery points where CMS/game operations span both systems. Document which snapshots can be restored together and which external effects require reconciliation.

Record backup completion and integrity checks, alert on stale/failed backups, and verify restoration under section 65. Optional backup tooling does not remove this operational requirement. Backups do not roll back payment-provider or already-applied game actions.

---

# 65. Restore Validation

Provide and rehearse a restore runbook before v1.0, after material schema/deployment changes and at the installation's scheduled drill interval.

1. Restore the matching application release, CMS database, uploads, configuration and encryption keys into an isolated environment.
2. Keep workers/scheduler disabled and block outbound email, payment and game commands until the environment is safe. Restored jobs must not replay production effects during a drill.
3. Rebuild disposable caches and verify permissions, decryption and database consistency.
4. Check login/recovery in a test context, identity links, content/media, realm mapping and migration checkpoints. For commerce releases, validate inbox/outbox, orders and delivery evidence without replaying rewards.
5. Reconcile external effects and post-backup changes before enabling production writers. Unknown command/refund results follow sections 29–30 and 100 rather than being resent automatically.
6. Record elapsed recovery time, recoverable data timestamp, failed checks and remediation. Compare measured results with the declared recovery objectives.

A backup passes only when a restoration and application checks succeed. A successful file copy or archive checksum alone is insufficient. Link restore evidence in delivery 7's release checklist.

---

# 66. Docker Support

Provide optional Docker setup.

Example:

```text
nginx
php-fpm
mysql/mariadb
redis
queue worker
scheduler
```

Docker should remain optional.

---

# 67. Production Hosting Contract

The baseline is one managed Linux host or equivalent service arrangement running the selected PHP/Laravel profile, Nginx/PHP-FPM, the CMS database, supervised queue workers and a scheduler. Redis and Docker remain optional. Document the exact tested OS/service versions with the runtime matrix in section 111.

Production requires:

- Operator CLI/deployment access for installation, migrations, diagnostics and recovery.
- Persistent supervised workers and a scheduler running independently of website requests.
- HTTPS, writable storage/cache directories and a web document root restricted to `public/` for migrated Laravel routes; transitional legacy routes follow the explicit ownership manifest.
- Persistent media and protected configuration/secrets, with least-privilege CMS/game connections.
- Reachable mail and required game services, observable jobs/scheduler and the backup/restore process in sections 64–65.

Shared hosting without workers, scheduler or the required deployment controls is outside the supported production profile. Do not advertise it as supported through synchronous job fallbacks. The existing XAMPP instructions describe inherited/local development, not certification of production readiness.

Build assets in CI or a build environment and deploy the artifact; Node need not run persistently under the selected CSR baseline. Document service users, paths, process supervision, restart order, permissions, secret provisioning, deployment/rollback commands and alert destinations for the actual environment.

Delivery 7 must demonstrate clean deployment, worker/scheduler completion, restart behavior and restoration without Redis. Docker uses the same operational requirements; it does not replace persistent storage, supervision or recovery planning.

---

# 68. Development Environment

Provide easy local setup:

```bash
git clone
composer install
npm install
cp .env.example .env
php artisan key:generate
php artisan migrate
npm run dev
```

Optional:

```text
Laravel Sail
Docker Compose
```

---

# 69. CLI Tools

Add CMS CLI commands.

Examples:

```bash
php artisan cms:install
php artisan cms:status
php artisan realm:test
php artisan realm:add
php artisan account:create
php artisan module:list
php artisan module:enable
php artisan module:disable
php artisan theme:list
```

---

# 70. System Diagnostics

Admin diagnostics page:

```text
PHP version
extensions
database connections
WoW DB status
SOAP status
cache
queue
storage
cron
permissions
latest errors
CMS version
module versions
```

Include the tested compatibility profile (section 47), worker/scheduler evidence, stale backups and latest restore-drill result. Distinguish configured, reachable and operational states. Allow exporting a sanitized diagnostics report.

---

# 71. Developer SDK

Create stable interfaces for external module developers in the later ecosystem delivery, after first-party contracts have been exercised. The SDK documents compatibility and trusted-code boundaries; it does not promise plugin sandboxing. Installation and activation remain separate under sections 16 and 48.

Document:

```text
events
hooks
services
repositories
API
permissions
UI extension points
module lifecycle
```

---

# 72. Event System

Expose internal events.

Examples:

```text
UserRegistered
GameAccountLinked
CharacterCreated
PurchaseCompleted
VoteConfirmed
RealmWentOffline
AdminCommandExecuted
```

Modules can subscribe without modifying core code.

---

# 73. Hooks / Extension Points

Provide documented extension points.

Avoid uncontrolled monkey-patching.

Examples:

```text
dashboard widgets
navigation items
profile tabs
admin menu
character actions
store providers
payment providers
```

---

# 74. Semantic Versioning

Use:

```text
MAJOR.MINOR.PATCH
```

Example:

```text
1.4.2
```

Define backward compatibility rules.

---

# 75. Deprecation Policy

When changing public APIs/interfaces:

1. Mark deprecated
2. Document replacement
3. Keep for at least one major/minor cycle where practical
4. Remove only in documented breaking release

---

# 76. Documentation Website

Create proper docs.

Sections:

```text
Installation
Upgrade
Configuration
Realms
Emulator adapters
Modules
Themes
API
Security
Deployment
Developer guide
Troubleshooting
```

---

# 77. API Documentation

Generate OpenAPI documentation.

Expose:

```text
/api/docs
```

Document:

- authentication
- endpoints
- payloads
- errors
- pagination
- rate limits

---

# 78. Error Response Standard

API errors should be consistent.

Example:

```json
{
  "error": {
    "code": "ACCOUNT_NOT_FOUND",
    "message": "The requested account was not found."
  }
}
```

---

# 79. Pagination Standard

All large API collections should support:

```text
page
per_page
sort
filters
```

Avoid returning thousands of characters/accounts at once.

---

# 80. Search

Provide centralized search for:

```text
users
accounts
characters
guilds
news
store products
```

Admin search should support IDs as well as names.

---

# 81. Observability

Optional advanced support:

```text
Sentry
OpenTelemetry
Prometheus
Grafana
```

At minimum provide:

- structured logs
- health endpoint
- queue status
- failed job history

---

# 82. Health Endpoints

Separate lightweight process liveness from application readiness and authenticated diagnostics. A public liveness response must not disclose internal hosts, secrets or dependency details.

Readiness reports whether the CMS can serve its intended workload. Use cached worker/scheduler heartbeats and bounded dependency checks rather than expensive synchronous scans on every probe. An individual realm outage is normally a degraded game feature, not a reason to restart a healthy CMS process repeatedly.

Detailed CMS database, queue, scheduler, auth-source and realm status belongs in permission-protected diagnostics. Include last-check time, last successful job/schedule execution and stale status. Define feature-specific degraded behavior and operational alert thresholds in the runbook.

---

# 83. Performance Profiling

Profile armory, rankings, online counts, guild pages and reports against representative isolated data before enabling them. For each game-facing query/refresh, record a numeric timeout, maximum result size, pagination limit, concurrency limit and refresh interval in the tested profile.

Choose limits from measured plans/latency and the game operator's load budget; the feature is not release-ready while these remain unspecified. Apply per-realm rate/concurrency limits and monitor slow-query/timeout frequency and scheduler overlap. Test cold-cache and game-database outage behavior.

CMS-owned indexes are managed through CMS migrations. Any proposed game-schema index/change requires a separately reviewed game-operator migration; never apply it automatically from the CMS installer. Do not add heavy joins to every web request as a cache-miss fallback.

---

# 84. Read Models for Expensive Game Data

Use bounded scheduled snapshots/read models for expensive rankings and aggregate game data. Store derived results in CMS-owned storage, scoped by realm and profile, with refresh timestamp and last successful status. Read-only replicas may be an additional tested profile, with lag explicitly accounted for.

Serve a last-known-good snapshot within a documented staleness limit, or display temporary unavailability. Show freshness where relevant. Do not turn a failed refresh into an unbounded live query against the game database. Coalesce concurrent refresh requests and back off after connection failures.

Snapshots are presentation data, not authoritative evidence for ownership, payment eligibility or game-command execution. Revalidate sensitive operations against their designated authoritative source. Test that the configured query budgets hold during simultaneous requests, cold cache and scheduled refreshes.

---

# 85. Game Database Safety

The CMS should avoid writing directly to world/character tables wherever possible.

Prefer:

```text
SOAP / server commands
```

for operations that the emulator expects to perform itself.

Direct writes should be isolated and documented.

---

# 86. Read/Write Database Credentials

Use least privilege.

Example:

```text
world DB → read-only CMS user
characters DB → read-only where possible
auth DB → limited account operations
CMS DB → normal CMS account
```

Never use `root`.

---

# 87. Multi-Realm Architecture

Use the correct identity scope rather than adding `realm_id` to every record:

| Record or operation | Required scope |
| --- | --- |
| CMS user, site-wide content and roles | CMS identity; no artificial realm assignment |
| Game-account lookup or password operation | `(auth_source_id, external_account_id)`, or the CMS `game_account_id` resolving that pair |
| Registered emulator realm | CMS `realm_id`, mapping to `(auth_source_id, external_realm_id)` |
| Character lookup or action | `(realm_id, external_character_id)` |
| Guild lookup | `(realm_id, external_guild_id)` |
| Rankings, realm notices, realm status and character services | CMS `realm_id` plus the relevant scoped external identifiers |
| Store/vote reward destination | Explicit realm and account/character destination; validate their relationship before delivery |

External IDs and names are not globally unique. Never use an account ID, character GUID, guild ID or character name alone as a cross-realm lookup, cache key, job destination or import identity. Do not hardcode realm `1` or treat an emulator realm ID as a CMS realm ID.

Enforce composite uniqueness for CMS references/read models where stored. Validate cross-context relationships in services: the selected character belongs to the intended game account in that realm, the account belongs to the realm's auth source, and the acting CMS user has the required ownership or administrative permission. A supplied realm ID is not authorization.

Include the relevant auth-source/realm context in API resource identifiers, cache keys, search indexes, audit targets, queued jobs and import mappings. Resolve credentials from trusted configuration on the server. Jobs must revalidate the destination and authorization policy applicable to the operation before execution; do not silently reroute a job if a realm mapping changed after enqueueing.

Acceptance scenarios for delivery 4 and dependent features:

- Two realms share one auth source: account 42 is represented once and exposes the correct, separate character lists through each realm.
- Two auth sources both contain account 42: they remain separate `GameAccount` references and ownership links.
- Two realms both contain character 100: searches, caches, API responses and jobs do not mix them.
- A request combines an account from source A with a realm from source B: it is rejected before a game-data read or write.
- A user supplies another user's valid scoped identifiers: ownership checks still deny unauthorized access.
- Disabling one realm leaves sibling realms and shared account associations intact.
- An import is repeated with colliding external IDs across sources/realms: mappings remain distinct and no duplicate links are created.

---

# 88. Multi-Expansion Support

Adapters should expose expansion metadata.

Examples:

```text
Vanilla
TBC
WotLK
Cataclysm
MoP
etc.
```

Frontend modules can adjust available data accordingly.

---

# 89. Expansion-Specific Capabilities

Use the `CapabilityProvider` from section 8 for profile-specific capability checks:

```text
supportsAchievements()
supportsTransmog()
supportsArenaTeams()
supportsCollections()
```

Do not assume all cores expose the same data.

---

# 90. Feature Flags

Introduce feature flags.

Examples:

```text
ENABLE_STORE
ENABLE_VOTING
ENABLE_ARMORY
ENABLE_DISCORD
ENABLE_REFERRALS
```

Later use DB-managed feature flags from admin.

---

# 91. Maintenance Mode

Support:

```text
site maintenance
admin bypass
custom message
scheduled maintenance
```

API should return appropriate status codes.

---

# 92. Announcement System

Provide global notices:

```text
server maintenance
realm restart
promotion
security notice
```

Support expiry dates and targeting.

---

# 93. User Account Dashboard

Modern user dashboard:

```text
Profile
Game accounts
Characters
Security
Sessions
Purchases
Wallet
Votes
Referrals
Notifications
Support
```

---

# 94. Game Account Linking

Use `GameAccountLink` to associate a CMS user with a `GameAccount` reference as defined in section 23. Support multiple game accounts per CMS user, including accounts from different auth sources, while enforcing one current CMS owner per game account.

## Linking procedure

1. Require an active CMS identity, verified CMS email and recent CMS re-authentication, including enrolled MFA.
2. Select the auth source and identify the external account within that source. Verify the submitted game credentials through its adapter. Never use a matching email or bare account ID as proof.
3. Issue a short-lived, single-use proof context bound to the CMS user, session, auth source and external account. Store no plaintext game password in that context. Apply CSRF protection and rate limits to the browser flow.
4. In a CMS transaction, consume the proof and create the verified association while enforcing the database's unique current-owner constraint. Check suspension/restriction policies again before linking. If the account is linked elsewhere, report a conflict without revealing the other owner's identity.
5. Audit the association and notify the CMS owner. If an external notice is supported, send it only to a validated destination; do not disclose credentials.

A link covers the same game account across realms using its auth source. It does not create an account per realm, grant administrative rights or bypass realm-specific access restrictions. Resolve characters through each selected realm using section 87.

## Unlinking and disputed ownership

Unlinking requires recent CMS re-authentication/MFA and explicit confirmation naming the affected account and auth source. Keep at least one working CMS login/recovery method. Revoke the old association's management access and pending link/recovery proofs; preserve the external account, game credentials, characters and audit history.

Pending game actions must revalidate association ownership before execution. Block unlinking until unresolved deliveries or sensitive operations are reconciled, or apply a documented cancellation/settlement policy; do not let unlinking redirect a purchased reward to a new owner.

Unlinking is not a transfer operation and does not reset passwords or bans. A later owner must complete the normal proof procedure. Ownership disputes and transfers need a separate operator-reviewed procedure with recorded evidence, authorization and notifications; never replace a current link automatically, even if another CMS user presents valid game credentials.

Recovery follows section 9. Removing a CMS link must not remove the emulator's restrictions or provide a shortcut around a suspended CMS identity; attempted reassociation of a restricted account requires policy review.

---

# 95. Account Security Page

Show:

```text
Active CMS sessions
Recent CMS logins
CMS password and recovery
Verified CMS email
CMS 2FA and recovery codes
API tokens
Linked game accounts (auth source and restriction status)
Separate game-password actions for each eligible account
```

Allow revoking sessions.

---

# 96. GDPR / Privacy Basics

Add:

- Privacy policy
- Cookie information
- Data export
- Account deletion request
- Consent logging where relevant

Keep logs according to configurable retention policies.

---

# 97. Terms & Rules Acceptance

Track versioned acceptance.

Example:

```text
terms_version
accepted_at
IP
```

When terms materially change, require re-acceptance.

---

# 98. Support/Ticket Module

Optional:

```text
Tickets
Categories
Priority
Assignments
Internal notes
Attachments
Status
```

This can replace external support tools for smaller servers.

---

# 99. User Impersonation

Admin impersonation can be useful, but should be heavily controlled.

Requirements:

- explicit permission
- visible impersonation banner
- audit log
- no impersonating higher-privileged admins
- easy exit

---

# 100. Store Refunds

Never delete financial transactions. Record refunds as compensating transactions linked to the original payment, with amount, currency, provider reference, status and audit history. Prevent concurrent requests from refunding more than the remaining refundable amount.

A refund request is not a confirmed refund. Reconcile uncertain provider responses using the same operation reference; do not create a second refund simply because the first call timed out.

Before refunding, coordinate with pending/in-progress delivery intents under the state rules in sections 28–30. Cancel unexecuted work atomically where possible. An in-flight or unknown command may already have applied a reward and requires reconciliation. A completed refund does not prove that the game-side reward was removed.

If reward reversal is supported, treat it as a separate authorized, audited game operation with its own evidence and failure handling. Preserve partial-delivery and dispute history even when money has been returned.

---

# 101. Analytics

CMS analytics:

```text
registrations
active users
online peaks
store revenue
conversion
vote activity
referrals
realm uptime
```

Avoid unnecessary privacy-invasive tracking.

---

# 102. Admin Export

Allow CSV export for:

```text
users
orders
transactions
votes
referrals
audit logs
```

Require appropriate permissions.

---

# 103. Import Tools

Support controlled imports from:

- SahtoutCMS
- Legacy CMS databases
- CSV

Import should validate and preview changes before applying them.

---

# 104. Migration Tool From SahtoutCMS

Create an official, versioned migration path. The following CLI syntax is a proposed interface to implement, not an available command:

```bash
php artisan migrate:sahtout --source=legacy --dry-run
php artisan migrate:sahtout --source=legacy --run=<run-id>
php artisan migrate:sahtout --source=legacy --resume=<run-id>
```

Source names resolve configured connections; do not pass secrets in command-line arguments. Each supported source schema/version must have a tested mapping to the target schema.

## Scope and identity mapping

For v1.0, import supported website identities, news/pages, settings and required media references. Resolve game-account associations only through the activation and proof procedures in sections 9 and 94; import unproven identities as pending activation. An email match alone is not proof of account ownership. Preserve existing game accounts in their original game databases rather than recreating them during a CMS import.

Store products and financial history target v1.1; vote configuration/history target v1.2. Report and preserve deferred data in the source and backup rather than treating it as successfully imported. Unsupported settings, credentials and authentication formats require an explicit operator action or recovery path.

Maintain a persistent mapping keyed by source installation, entity type and source ID, with source realm/auth context wherever needed to disambiguate IDs. Record the target ID, import version, source fingerprint and last successful run. Enforce uniqueness so a restart or second import cannot silently create a second target record for the same source entity.

## Preview, execution and restart

- Dry-run validates the source schema, required fields, relationships, identity collisions and media availability. Report planned creates, updates, skips, conflicts and unsupported records without writing target application data or triggering external effects.
- Bind an execution plan to its source snapshot/fingerprint and mapping version. If the source changed after preview, revalidate and issue an updated report before applying it.
- Import in bounded transactions with durable checkpoints. A committed batch includes its target records and mapping/checkpoint changes; a failed batch can be retried without duplicating records.
- Default to preserving target edits made since a previous import. Report conflicts instead of silently overwriting them; any explicit resolution policy is recorded with the run.
- Resolve relationships through the mapping, not by assuming that source and destination IDs are equal. Validate counts, required relationships and representative records after import.
- Disable normal side effects during import: no welcome/reset emails, payment calls, vote rewards or game commands. Importing historical transactions must never initiate fulfillment.
- Produce a sanitized per-run report with counts, failures, conflicts, deferred data, mapping version, checkpoint and completion state. Never mark a partial import as complete.

Acceptance includes dry-run with no application writes, an interrupted/resumed import, a repeated completed import with no duplicates, target-edit conflicts and relationship validation against representative sanitized fixtures. Final production import uses the controlled write freeze described in section 105.

---

# 105. Compatibility Layer During Rewrite

Use a staged route migration with one authoritative writer for each operation and data set. Legacy and Laravel code may coexist temporarily; they must not independently mutate the same records without an explicit ownership rule. The rules below are implementation requirements, not a claim that the compatibility layer already exists.

## Feature migration matrix

Create and maintain a version-controlled migration register during delivery 1. The following is its initial scope, not a completed route inventory:

| Feature | Decision | Target | Replacement boundary |
| --- | --- | --- | --- |
| Login, registration and recovery | Migrate | v1.0 | Identity routes, session lifecycle and account adapter |
| Account dashboard and basic characters | Migrate | v1.0 | User routes and realm-aware reads |
| Administration and permissions | Replace | v1.0 | Admin routes, policies and audit records |
| News/pages and required media | Migrate | v1.0 | Content routes, records and media references |
| Setup and configuration | Replace | v1.0 | Installer, environment configuration and diagnostics |
| Game accounts and game data | Retain in game databases | v1.0 onward | Access through adapters; no blanket data copy |
| Store and payment callbacks | Retain temporarily, then migrate | v1.1 | Checkout, callback processing, orders and delivery jobs |
| Voting and reward callbacks | Retain temporarily, then migrate | v1.2 | Vote verification and reward processing |
| Armory and rankings | Retain temporarily, then migrate | v1.2 | Read routes and cached queries |
| Other inherited functionality | Inventory and classify individually | Before affected cutover | Explicit retain, migrate, replace or discontinue decision |

For each concrete feature, record exact public URLs and HTTP methods, direct PHP entry points, callbacks, CLI jobs, scheduled jobs, data read/written, current owner, replacement owner, release, migration status and rollback procedure. A decision to discontinue must document affected data and users; absence from v1.0 is not permission to delete the feature or its data.

## Route ownership and execution

Maintain an explicit route ownership manifest: `legacy`, `realmgate` or `disabled`, with the replacement entry point and cutover state. The web-server/router configuration must enforce this assignment, including direct requests to old PHP files. Default unmigrated routes to their recorded legacy handler; unknown routes must not become an unrestricted legacy fallback.

For each migration, build and test the replacement before switching ownership. The switch covers all methods, form actions, API equivalents and background entry points for that operation. Old browser GET URLs can redirect to the replacement. State-changing requests and callbacks must use a controlled handler that preserves method/payload and runs the new validation and authorization, or fail explicitly; do not rely on a generic redirect to replay them.

Legacy views may temporarily call a new application service only through a defined bridge that initializes the required framework context and applies the same security checks. Never treat a legacy session variable or request parameter as trusted authorization. There must be no bypass route that still invokes the previous write logic.

## Session transition

Use separate cookie names and session stores for legacy and Laravel sessions. Do not copy serialized session payloads or automatically trust legacy role flags in Laravel.

The default cutover policy is explicit re-authentication into RealmGateCMS. Switch the identity route family together, invalidate legacy authentication for migrated protected routes, rotate the new session ID on login and rebuild authorization from the new identity system. Document the login interruption in the deployment notice.

During temporary coexistence, a separately authenticated legacy area may remain only where its account operations are compatible with the ownership register. Sensitive account changes, bans and logout must invalidate both session systems through a tested revocation bridge, or the affected legacy protected area must be disabled before cutover. Password recovery and account updates must not have two independent authorities.

Transparent single sign-on is outside the initial migration scope; do not invent an implicit session-sharing mechanism to avoid re-authentication.

## Data ownership during coexistence

For each writable entity or operation, record one owner and permitted readers. Before cutover the legacy feature owns its writes; after cutover the new service owns them. An old UI retained after cutover must delegate to that service or become read-only/disabled.

Read-only shadow comparisons are allowed. Independent dual writes to legacy and new CMS databases are not the default migration mechanism. Background workers, webhooks and cron jobs follow the same ownership rules as browser requests.

Game database writes remain restricted to the designated adapter/command path. CMS migrations must not modify game schemas as a side effect. Switching one feature must not change ownership of unrelated deferred features.

## Production cutover and recovery

1. Rehearse installation, import, ownership switching and recovery in an isolated environment using a representative sanitized source copy. Record accepted validation results and rollback triggers.
2. Back up the affected databases, uploads and configuration; verify restoration. Record source/target versions and the route/data ownership manifest for the release.
3. Announce the maintenance window and re-authentication requirement. Freeze affected writes, including administrative actions, callbacks, workers and scheduled jobs. Drain in-flight operations or record unresolved ones. Buffer callbacks durably or rely on a verified provider retry mechanism; never acknowledge an event that has not been safely recorded.
4. Run the final validated import against a stable source snapshot, reconcile its report and confirm that deferred data remains preserved. Do not open the new write path while the source still accepts conflicting writes.
5. Switch route and data ownership together, apply the session transition and run smoke checks for login, permissions, account links, content and realm selection. Initially keep business writes paused so rollback is still straightforward.
6. Enable the designated writers and workers, then resume callback processing through the single owner. Monitor authentication failures, server errors, import discrepancies and job failures against thresholds defined in the deployment runbook.
7. If validation fails before new business writes, restore the previous routing/configuration and recover the target import state as rehearsed. If new writes or external effects have occurred, freeze affected operations and reconcile post-cutover changes before rollback; never blindly restore an old snapshot and lose new data or replay game rewards. Use forward recovery where a safe reverse transition is unavailable.

The operator runbook must identify rollback triggers, the responsible operator, commands for the actual environment and the point after which simple rollback is no longer safe. An unresolved transition for a required deferred feature blocks that installation's cutover, not an automatic expansion of v1.0 scope.

## Legacy retirement gate

Remove a legacy feature only when:

- Its replacement passes behavior, permission and data-integrity checks, including intentional corrections to legacy defects.
- Every related route, form, direct PHP entry point, callback and background job is switched, redirected safely or explicitly disabled.
- Import/mapping results are reconciled and ownership rules prevent further legacy writes.
- Session transition and a documented recovery procedure have been tested.
- Monitoring over an observation window defined before deployment confirms no required traffic or jobs still depend on that implementation.
- Documentation and the migration register identify the replacement and preserved/deferred data.

Retire executable code separately from historical data. Keep required backups, attribution and compatibility identifiers under their documented retention/migration rules. Do not delete shared helpers while another recorded legacy feature still depends on them.

---

# 106. Rewrite Order

Implement the following deliveries in dependency order. Deliveries 1–3 establish the foundation; deliveries 4–7 complete v1.0. Delivery 8 targets v1.1 and delivery 9 targets v1.2. Delivery 10 is the later, unversioned backlog. This sequence preserves the release scope in sections 2 and 112.

Each implementation task must identify its dependencies, applicable acceptance IDs from section 112, acceptance evidence and exact legacy routes, services or data flows being replaced. A delivery is complete only when those criteria are met; a new directory or service class alone is not a completed migration. Keep legacy behavior available until its replacement has passed the relevant checks, and document any intentional behavior change.

## Delivery 1 — Inventory and Rebranding

**Target:** Foundation.

**Dependencies:** Access to the inherited source and representative, sanitized schema/configuration information.

**Work:** Complete the rebranding checklist; inventory features, entry points, data stores, external integrations and critical risks. Maintain the feature, route and data ownership register specified in section 105; mark features for migration, retention, replacement or explicit discontinuation according to release scope.

**Acceptance:** Every feature selected for v1.0 has identified entry points, data dependencies and a planned replacement. Deferred features and installations that depend on them are explicitly recorded. No secrets or live personal data enter the inventory.

**Legacy replacement:** Documentation and product metadata only at this stage; no application flow is retired by the inventory.

## Delivery 2 — Characterization Tests

**Target:** Foundation.

**Dependencies:** Delivery 1 identifies critical flows and available fixtures.

**Work:** Capture current SRP6/account behavior, registration and password changes, permission checks, realm selection and relevant game queries. Record known security defects as behavior to correct rather than preserve. Add regression coverage for inherited payment/delivery flows before changing those flows in v1.1.

**Acceptance:** Critical v1.0 flows have repeatable tests with isolated fixtures and explicit expected results. Tests do not require production accounts or databases. Known defects have documented corrected expectations.

**Legacy replacement:** None; establish evidence against which replacements will be checked.

## Delivery 3 — Laravel and CMS Foundation

**Target:** Foundation.

**Dependencies:** Deliveries 1–2.

**Work:** Bootstrap the modular Laravel/Inertia application using the compatibility matrix in section 111, environment configuration, CMS migrations, logging, coding standards and CI. Establish the application test environment and internal feature boundaries.

**Acceptance:** A clean development setup boots from documented configuration, creates the CMS schema through migrations and runs the baseline checks in CI. The pinned toolchain and Inertia login/form/navigation prototype satisfy section 111; web and API entry points reuse the same application services. Secrets remain outside version control. The legacy baseline remains testable.

**Legacy replacement:** Shared infrastructure for migrated routes only. Existing routes remain operational until their owning delivery replaces them; introducing Laravel does not retire authentication or administrative controls.

## Delivery 4 — Multi-Realm Model and Minimal AzerothCore Adapter

**Target:** v1.0.

**Dependencies:** Delivery 3 and the account/realm fixtures from delivery 2.

**Work:** Implement `AuthSource`, `GameAccount`, `Realm` and `GameAccountLink` with the scoped identities in sections 23 and 87. Define realm-aware connections and the minimal account adapter needed for account verification, creation and password changes. Isolate SRP6 and account database access behind that adapter. Add basic realm status access. Extend character and command operations only when required by subsequent deliveries.

**Acceptance:** Account adapter contract tests pass against the supported AzerothCore test schema, including failed verification and password changes. Realm selection resolves the intended connections; shared-auth and colliding-ID scenarios in section 87 pass. Unavailable connections produce controlled failures. No production game data is modified by tests.

**Legacy replacement:** Direct account access and SRP6 implementation calls in the flows selected for migration. Public login/registration routes are switched only after delivery 5 provides their complete security controls.

## Delivery 5 — Identity, Authentication, Authorization and Audit

**Target:** v1.0.

**Dependencies:** Delivery 4 for game-account operations and delivery 3 for CMS persistence and framework middleware.

**Work:** Implement CMS identity, login, registration, email verification, recovery, game-account linking, sessions, CSRF protection and rate limiting. Establish RBAC and audit logging now, together with account security and administrator 2FA policy support. Implement the separate credential, activation, recovery, suspension and link lifecycles in sections 9 and 94.

**Acceptance:** Authentication and recovery flows pass integration tests; unauthorized requests and attempts to access another user's linked accounts are rejected. Session invalidation works. Sensitive changes produce audit records without credentials or tokens. No migrated administrative action is exposed without authorization and audit coverage. All identity acceptance scenarios in section 9 pass, including concurrent linking and independent credential/restriction behavior.

**Legacy replacement:** Selected login, registration, password recovery and account-linking flows, their session handling and ad-hoc permission checks. Retire each old entry point only once its replacement passes the same behavioral checks and the intended security corrections.

## Delivery 6 — Complete User, Admin and Content Features

**Target:** v1.0.

**Dependencies:** Delivery 5. Add tested character repositories or narrowly scoped server-command operations before any feature that uses them.

**Work:** Migrate small end-to-end features: account dashboard, linked-account character list, realm status, user/game-account/realm administration, audit views, basic news/pages and required media support. Include the responsive base theme, translated strings and the corresponding initial API in these deliveries.

**Acceptance:** Each feature has a working UI or API entry point, validation, service integration, appropriate permissions, regression coverage and updated usage documentation. Users cannot view another account's private data or bypass permissions through the API. Realm-aware views use the selected realm consistently. Content renders through the base theme.

**Legacy replacement:** The specific user, administration and content pages and queries covered by each completed feature. Record the exact routes retired with that feature; store, voting and other deferred routes are not implicitly considered migrated.

## Delivery 7 — Installation, Migration and Release Validation

**Target:** v1.0 release gate.

**Dependencies:** Deliveries 1–6. Develop installer/import tooling and operational documentation alongside earlier deliveries; perform this integrated validation after the v1.0 features are available.

**Work:** Complete the documented installer, diagnostics and supported SahtoutCMS imports. Validate production configuration, required background jobs, deployment and recovery procedures. Document the transition for installations using deferred features.

**Acceptance:** A clean installation and an import from representative sanitized legacy fixtures both produce a usable v1.0 site. Imported records and relationships are checked, limitations are reported, and required background jobs run. A backup is restored into a clean test environment and core flows work afterward. Release checks pass with documented evidence, including the no-Redis hosting profile, numeric recovery objectives and measured restore results (sections 40–42 and 64–67). Record the tested emulator/schema manifest from section 47.

**Legacy replacement:** The inherited setup process and supported imported data flows for installations switching to v1.0. A production switch requires the cutover, session transition and recovery checks in section 105; imports must satisfy section 104. Data belonging to deferred features must not be silently discarded.

## Delivery 8 — Commerce

**Target:** v1.1.

**Dependencies:** Delivery 7, payment/delivery regression coverage from delivery 2, and tested queue and server-command integrations.

**Work:** Products and immutable checkout snapshots; independent payment/order/delivery states; one verified payment provider; durable inbox, delivery intents and outbox; queued rewards, transaction history and reconciliation as specified in sections 28–30 and 100.

**Acceptance:** All commerce acceptance scenarios in section 30 pass, including concurrent/reordered callbacks, publication failures, worker restarts and lost responses after actual game delivery. Unknown outcomes cannot be blindly retried. Refund reconciliation in section 100 is covered, and purchase/delivery history remains traceable. Document the selected adapter's actual deduplication and verification guarantees.

**Legacy replacement:** Legacy checkout, payment callbacks and direct reward delivery for the provider and products covered by this release. Document unsupported payment methods and their transition before retiring them.

## Delivery 9 — Community

**Target:** v1.2.

**Dependencies:** Delivery 7, tested realm-aware character/read queries, and verified reward delivery for voting rewards (reuse delivery 8 where applicable).

**Work:** Voting, basic armory and rankings. Referrals and Discord remain outside this delivery.

**Acceptance:** Voting verification and cooldowns are tested; repeated confirmations do not duplicate rewards. Armory and ranking results respect realm selection and visibility rules, and pass the numeric query budgets and cold-cache/outage checks in sections 83–84.

**Legacy replacement:** Corresponding vote, armory and ranking routes after their replacements pass checks; no implicit retirement of other community features.

## Delivery 10 — Ecosystem and Additional Emulators

**Target:** Later, unversioned backlog.

**Dependencies:** Stable core interfaces for external plugins and themes; stable AzerothCore behavior and shared adapter contract tests before adding another emulator. Each optional integration declares its own required services.

**Work:** External plugins and public SDK, advanced/installable themes, extended integration APIs, ecosystem documentation, referrals, Discord and additional emulators. A marketplace follows stable plugin interfaces rather than preceding them.

**Acceptance:** Each separately scoped delivery declares its compatibility requirements and passes relevant contract, permission and upgrade checks. Additional emulators pass the supported capability contracts before being advertised as supported.

**Legacy replacement:** Specify per feature; these are generally extensions of the released product and do not automatically replace inherited functionality.

---

# 107. What to Keep From SahtoutCMS Initially

Useful existing logic to preserve/reuse while migrating:

- SRP6 implementation
- AzerothCore schema knowledge
- SOAP integration knowledge
- Registration flow behaviour
- Character queries
- Item/game-data mappings
- Realm status logic
- Vote logic
- Shop concepts
- Localization content
- Existing game-related frontend assets where license permits

Do not rewrite working WoW-specific code until equivalent tests exist.

---

# 108. What to Replace Early

Prioritize replacing:

- Direct page-based architecture
- Direct `mysqli` usage spread throughout pages
- Hardcoded DB configuration
- Mixed HTML/business logic
- Manual routing
- Ad-hoc permission checks
- Page-specific security logic
- Direct payment/store execution
- Direct SOAP calls from UI controllers
- Large multi-thousand-line page files

---

# 109. What Not to Do

Avoid:

- Rewriting everything in one release
- Supporting many emulators immediately
- Building a plugin marketplace before stable APIs exist
- Letting modules query arbitrary databases directly
- Letting themes execute business logic
- Using WoW auth DB as the entire CMS identity system
- Storing secrets in source control
- Using database root credentials
- Hardcoding realm IDs
- Sending store rewards during the payment callback request itself
- Allowing arbitrary server commands without RBAC/audit logging

---

# 110. Target Architecture

One modular Laravel application exposes two entry points over shared application services:

```text
Browser: website/admin                External clients (later integrations)
         |                                       |
Laravel web routes                         /api/v1 routes
Inertia controllers                        API controllers
         |                                       |
         +------- Shared application services ---+
                     | validation / policies / audit
                     |
        First-party modules in app/Modules
        Identity | Realms | Content | later features
              |                        |
       CMS persistence           Scoped game contracts
              |                  Accounts | Characters
         CMS database            Status | Commands | Capabilities
                                          |
                                AzerothCore implementations
                                (other profiles later)
                                          |
                                  Game DBs / commands

Web responses → typed Inertia props → React pages → base theme
API responses → versioned JSON resources
Jobs/CLI → the same application services and scoped gateways
```

Browser and API controllers have different response formats, not different business rules. Queue workers deploy the same application code. Later integrations or plugins do not require splitting the core into separate services.

---

# 111. Selected Technology Stack and Compatibility Matrix

Decision recorded on 2026-09-11. This is the target modernization profile, not a statement that the inherited application or its current lockfiles have been upgraded or tested on it.

| Component | Selected target | Validation requirement |
| --- | --- | --- |
| Laravel | 13.x (`^13.0`) | Resolve dependencies and run foundation/integration checks |
| PHP | 8.4.x initial deployment/CI profile | Test required extensions and SRP6; add other runtimes only after passing CI |
| Composer | 2.x | Pin the actual tool version in CI and commit the resolved lockfile |
| Node.js | 24.x LTS for asset builds | Pin patch/toolchain version in CI; build from the npm lockfile |
| Inertia | 3.x Laravel and React adapters | Validate their peer constraints together with Laravel 13 in the prototype |
| React / React DOM | 19.x, matching versions | Confirm adapter compatibility and browser smoke tests |
| TypeScript | 5.x, strict mode | Type-check page props and theme contracts |
| Tailwind CSS | 4.x | Build the base theme and verify generated styles |
| Vite | Compatible version selected by the Laravel/Inertia bootstrap | Record exact resolved version and Node requirements before accepting delivery 3 |
| CMS database | MySQL 8.4 initial target | Run migrations/import/integration tests; MariaDB is an additional profile only after equivalent checks |
| Game databases | Versions required by the tested AzerothCore profile | Record core/schema/database versions separately; do not upgrade game databases to match the CMS profile |

Laravel 13's supported PHP range is broader than this project's initial profile; framework support alone does not establish CMS compatibility. Use the [Laravel support policy](https://laravel.com/framework/docs/13.x/releases) and [Node.js release schedule](https://nodejs.org/en/about/previous-releases) when reviewing runtime upgrades. The Inertia version decision follows the official documentation's [v3 release notice](https://inertiajs.com/docs/v2/getting-started); dependency resolution and smoke tests remain required, not assumed.

Delivery 3 must produce reproducible lockfiles, pinned CI tooling and a minimal login/form/navigation prototype on the selected combination. If dependency constraints conflict, document a revised matrix before accepting that delivery. Do not silently fall back to floating “latest” versions or advertise untested combinations.

Production uses Nginx/PHP-FPM, supervised workers and scheduler/cron. Redis and Docker remain optional. Node is required for asset builds, not a persistent production frontend process under the CSR baseline; SSR would change that requirement.

Quality tooling: PHPUnit for automated tests, PHPStan, Laravel Pint, ESLint, Prettier, GitHub Actions and dependency update checks. Select compatible tool versions during bootstrap and commit their resolved dependencies. Revisit the matrix before upstream support expires or a major dependency changes.

---

# 112. Release Acceptance Criteria

A release is complete when its included behavior is demonstrated with recorded evidence, not when a list of components exists. The matrix below consolidates release gates; detailed scenarios in the referenced sections remain mandatory for their scheduled features.

## Acceptance matrix

| ID | Release / area | Observable acceptance criterion | Minimum evidence / detail |
| --- | --- | --- | --- |
| FND-01 | Foundation / inventory | Every v1.0 feature has identified routes, data owners, dependencies and legacy replacement boundaries; deferred features are explicitly classified | Reviewed migration register, section 105 |
| FND-02 | Foundation / baseline | Critical inherited behavior has repeatable characterization tests; known security defects have explicit corrected expectations | Isolated fixtures and test results, deliveries 1–2 |
| FND-03 | Foundation / stack | A clean checkout installs locked dependencies, builds assets, boots Laravel/Inertia and passes the selected checks | CI run, exact revision/tool versions and prototype checks, section 111 |
| V1-01 | v1.0 / multi-realm | Account IDs shared across auth sources and character IDs shared across realms never mix data; shared-auth accounts are represented once | Collision, shared-source and mismatched-context tests, sections 23 and 87 |
| V1-02 | v1.0 / identity | Registration, activation, email verification and recovery work; CMS and game password changes remain independent | Identity integration tests, section 9 |
| V1-03 | v1.0 / linking | A verified game account has one current CMS owner, including concurrent requests; unlinking preserves the external account and revokes CMS management access | Concurrency, proof-expiry and unlink tests, sections 9 and 94 |
| V1-04 | v1.0 / permissions | A user cannot read or modify another user's accounts through pages, API endpoints or old direct entry points; unauthorized admin actions fail | Positive and negative authorization tests, sections 12, 87 and 105 |
| V1-05 | v1.0 / sessions and security | Session revocation, CSRF protection, rate limits and administrator MFA policy work; recovery does not remove MFA, bans or suspensions implicitly | Security/identity flow tests and legacy-session transition check, sections 9–11 and 105 |
| V1-06 | v1.0 / game access | The supported AzerothCore profile passes account/status/character contracts; unsupported capabilities fail explicitly | Tested core/schema manifest and adapter results, sections 8 and 47 |
| V1-07 | v1.0 / user and administration | The account dashboard, scoped character list, realm status and basic user/account/realm administration complete their intended flows | End-to-end checks on representative linked accounts and realms, delivery 6 |
| V1-08 | v1.0 / audit | Sensitive actions identify actor, scoped target, outcome and time without exposing passwords, recovery tokens or credentials | Audit assertions and sanitized sample records, section 13 |
| V1-09 | v1.0 / content and theme | An authorized editor publishes news/pages with required media; public pages render correctly and stored content cannot execute injected scripts | Publishing/escaping tests and responsive/keyboard visual checks of the base theme, sections 18–20 and 34–36 |
| V1-10 | v1.0 / API | Delivered API resources use the same business rules as web flows, enforce authorization and return documented errors/pagination | API contract tests and matching documentation, sections 14–15 and 77–79 |
| V1-11 | v1.0 / installation | A clean installation creates the CMS schema and usable admin access, locks setup and passes operational diagnostics | Recorded clean-install rehearsal on the declared profile, sections 46–47 and 67 |
| V1-12 | v1.0 / migration | Dry-run changes no target application data; repeated or resumed imports produce no duplicates, preserve relationships and report conflicts/deferred records | Import reports and fixture comparisons, section 104 |
| V1-13 | v1.0 / operation | Without Redis, an actual queued job and scheduled task complete; stopped workers/scheduler are detected and dependent work stays visibly pending | Operational rehearsal and diagnostic output, sections 40–42 and 67 |
| V1-14 | v1.0 / recovery | A backup restores into a clean environment, key application flows pass and measured data loss/recovery time meet the declared objectives | Restore-drill report, sections 64–65 |
| V1-15 | v1.0 / cutover | Route/data ownership switches without competing writers; re-authentication and recovery are rehearsed; required deferred data is preserved | Cutover/recovery rehearsal and retirement register, section 105 |
| V11-01 | v1.1 / payment | Invalid payment evidence is rejected; duplicate, concurrent or reordered notifications create one entitlement per reward unit | Provider sandbox and state-transition tests, sections 28–29 |
| V11-02 | v1.1 / delivery | Queue outages/restarts do not lose entitlements; a lost response after game delivery becomes unknown and is not blindly resent | Failure-injection tests at each command/commit boundary, section 30 |
| V11-03 | v1.1 / reconciliation | Partial deliveries, extra payments, disputes and uncertain refunds remain traceable and recoverable without repeated effects | Reconciliation/refund scenarios and audit evidence, sections 29–30 and 100 |
| V12-01 | v1.2 / voting | Verified votes respect cooldowns and repeated callbacks do not duplicate rewards | Vote/reward tests with isolated provider fixtures, section 32 |
| V12-02 | v1.2 / armory and rankings | Results respect realm/visibility boundaries and measured query, concurrency and staleness limits, including cold cache and outages | Query plans, load results and freshness/failure checks, sections 83–84 |
| FUT-01 | Later / extensions | Each scheduled plugin/theme activates only with compatible deployed code/schema and preserves pending work when disabled | Dependency, activation/deactivation and upgrade tests, sections 16–18 and 48 |
| FUT-02 | Later / emulators | Each advertised profile passes the contracts for its declared capabilities against identified core/schema versions | Compatibility manifest and adapter test evidence, sections 8, 47 and 52 |

The v1.0 gate includes FND-01–03 and V1-01–15. v1.1 adds V11-01–03; v1.2 adds V12-01–02. Later releases retain the applicable baseline checks and add criteria for their selected work. A code change that affects earlier guarantees requires relevant regression checks on the new release candidate.

## Evidence and completion rules

Maintain a release acceptance record keyed by these IDs. For each applicable criterion, record the responsible owner, release candidate/commit, tested runtime and emulator profile, fixture or rehearsal setup, evidence link, result and any open issue. Use `not started`, `in progress`, `passed`, `failed` or `blocked`; no criteria are marked passed by this roadmap edit.

Results must be reproducible and attributable to the release candidate. Reuse a single relevant test run for several criteria rather than duplicating tests. Use automated checks for repeatable behavioral guarantees and recorded manual/operational evidence where a test suite alone cannot establish the outcome. Do not substitute overall coverage percentages or a generic green CI badge for an untested required behavior.

Only claim completion when all applicable gates pass and blocking issues are resolved. A deferred feature can be outside scope, but a failed included security, data-integrity or recovery criterion cannot be relabeled as optional. Document any scope change and its effects in sections 2, 106 and this matrix before changing applicability.

Before release, review the supported configuration matrix, migration limitations, user/admin instructions and recovery runbook against the actual behavior. Do not advertise functionality or compatibility supported only by an unimplemented plan.

## v1.0 scope boundary

v1.0 includes the focused CMS identity, AzerothCore/multi-realm access, basic user/admin experience, news/pages and required media, one responsive base theme, internal module boundaries, the corresponding API, installation, migration and production recovery covered above. It does not require an external plugin platform or advanced theme system.

Explicitly deferred:

- v1.1: store, one payment provider, queued rewards, transaction history and reconciliation.
- v1.2: voting, basic armory and rankings.
- Later: wallet, additional payment providers, external plugins/marketplace, advanced themes, referrals, Discord, additional emulators, launchers, mobile apps and other optional integrations.

Advanced publishing workflows, a standalone media management suite and additional dashboard features remain backlog enhancements. Installations relying on deferred features require the documented transition path in section 105; meeting a release gate never permits silent loss of their data.

---

# 113. Long-Term Product Vision

The project should aim to become more than "a WoW website template".

It should become a platform.

Conceptually:

```text
WoW Server Platform

Core
├── Identity
├── Security
├── Realms
├── Emulator SDK
├── API
└── Administration

Modules
├── Armory
├── Store
├── Voting
├── Rankings
├── Referrals
├── Discord
└── Support

Integrations
├── Launcher
├── Mobile app
├── Discord bot
├── Payment providers
└── Analytics

Presentation
├── Themes
├── Components
└── Brand customization
```

This gives RealmGateCMS an independent product direction and a foundation for long-term development.

---

# 114. Final Recommendation

Develop RealmGateCMS from the inherited SahtoutCMS implementation, preserving its provenance and useful AzerothCore/WoW-specific logic.

However:

> **Treat the fork as a migration source, not as the final architecture.**

The strongest approach is to preserve the useful game-specific knowledge while rebuilding the surrounding CMS as a clean, modular and framework-based application.

The most important decisions are:

1. Laravel foundation
2. Separate CMS and WoW domains
3. Emulator adapter layer
4. API-first backend
5. RBAC
6. Audit logging
7. Module system
8. Theme system
9. Queue-based store/server actions
10. Automated tests
11. Multi-realm support
12. Long-term multi-emulator support

If these foundations are implemented well, the result can become a substantially more maintainable and extensible product than the current generation of WoW private-server CMS projects.
