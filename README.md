# Lead Management Backend – Strapi v5

This is a backend service built using **Strapi** (Node.js Headless CMS) for managing leads, sources, campaign mappings, and analytics. It includes custom APIs, lifecycles, services, and plugins to support robust lead operations.

---

## 🚀 Features

- Modular content-type API architecture
- Custom routes/controllers for external integrations (e.g., Facebook, Google, LinkedIn webhooks)
- Lifecycle hooks and services for data preprocessing
- GraphQL and REST API support
- Audit logging, analytics, and export/import tools
- Role-based access with Users & Permissions plugin
- Docker-ready and configurable via environment variables

---

## ⚙️ Setup Instructions

### 1. Clone & Install
```bash
git clone <repo-url>
cd lead-mgmt-backend-dev
npm install
```

### 2. Environment Variables
- Create a `.env` file at the project root (copy from `.env.example`) and configure the following:
  
| Variable              | Description                                                        | Example                  |
|-----------------------|--------------------------------------------------------------------|--------------------------|
| `HOST`                | Server bind interface                                              | `0.0.0.0`                |
| `PORT`                | Server listening port                                              | `1337`                   |
| `APP_KEYS`            | Comma-separated keys for signing cookies and crypto operations     | `key1,key2,key3`         |
| `API_TOKEN_SALT`      | Salt for generating API tokens                                     | `randomSaltValue`        |
| `ADMIN_JWT_SECRET`    | JWT secret for admin panel                                         | `strongAdminSecret`      |
| `TRANSFER_TOKEN_SALT` | Salt for secure data-transfer tokens                              | `randomSaltValue2`       |
| `JWT_SECRET`          | JWT secret for user authentication                                 | `strongUserSecret`       |

---

### Sample `.env` Configuration

Below is a sample `.env` file.  
**Copy this template, fill in your own secrets and credentials, and place it at the project root.**

```env
# Server
HOST=0.0.0.0
PORT=1337

# Secrets
APP_KEYS=your_app_key1,your_app_key2,your_app_key3
API_TOKEN_SALT=your_api_token_salt
ADMIN_JWT_SECRET=your_admin_jwt_secret
TRANSFER_TOKEN_SALT=your_transfer_token_salt
JWT_SECRET=your_jwt_secret

# Database
DATABASE_CLIENT=postgres
DATABASE_HOST=localhost
DATABASE_PORT=5432
DATABASE_NAME=your_database_name
DATABASE_USERNAME=your_database_user
DATABASE_PASSWORD=your_database_password
DATABASE_SCHEMA=public
DATABASE_SSL=false
DATABASE_POOL_MIN=2
DATABASE_POOL_MAX=10
DATABASE_CONNECTION_TIMEOUT=60000
DATABASE_SSL_REJECT_UNAUTHORIZED=false

# Facebook Integration
FB_VERIFY_TOKEN=your_fb_verify_token
FB_PAGE_ACCESS_TOKEN=your_fb_page_access_token

# SMTP (Brevo/Sendinblue)
BRAVO_SMTP_HOST=smtp-relay.brevo.com
BRAVO_SMTP_PORT=587
BRAVO_SMTP_USER=your_smtp_user
BRAVO_SMTP_PASS=your_smtp_password

# Public URL
PUBLIC_URL=http://localhost:1337
```

> **Important:**  
> - Never commit your real `.env` file to version control.  
> - Only share `.env.example` with placeholders.
> - Always use strong, unique secrets in production.
> - Restart the server after changing environment


### 5. Folder Structure
```
lead-mgmt-backend-dev/
├── src/
│   ├── api/                 # Content-type APIs (lead, call, etc.)
│   ├── admin/               # Admin panel customizations
│   ├── components/          # Reusable components & dynamic zones
│   ├── extensions/          # Custom GraphQL resolvers, permission logic
│   └── index.ts             # Entry point
├── config/                  # Strapi configuration (database, server, plugins)
├── database/                # Migrations & seed files (future use)
├── public/                  # Static assets and uploads
├── types/                   # Custom and generated TypeScript definitions
├── utils/                   # Utility modules
├── interfaces/              # TypeScript interfaces
├── assets/                  # SSL/TLS certificates and assets
└── .strapi/                 # Internal admin build artifacts
```

### 7. Testing
- **Admin Panel**: `http://localhost:1337/admin`
- **REST API**: `http://localhost:1337/api`
- **GraphQL**: `http://localhost:1337/graphql`

### 8. Deployment Notes
- Ensure all environment variables are set in production.
- Run `npm run build` before `npm run start`.
- Secure the admin panel with HTTPS and a strong JWT secret.
- Use [Strapi Cloud](https://cloud.strapi.io) or other deployment options (see [Strapi Deployment Docs](https://docs.strapi.io/dev-docs/deployment)).
```bash
yarn strapi deploy
```

---

## 📚 Getting Started with Strapi

Strapi provides a powerful [Command Line Interface](https://docs.strapi.io/dev-docs/cli) (CLI) to scaffold and manage your project efficiently.

### `develop`
Start Strapi with auto-reload enabled for development. [Learn more](https://docs.strapi.io/dev-docs/cli#strapi-develop)
```bash
npm run develop
# or
yarn develop
```

### `start`
Start Strapi without auto-reload for production. [Learn more](https://docs.strapi.io/dev-docs/cli#strapi-start)
```bash
npm run start
# or
yarn start
```

### `build`
Build the Strapi admin panel for production. [Learn more](https://docs.strapi.io/dev-docs/cli#strapi-build)
```bash
npm run build
# or
yarn build
```

---

## 📖 Learn More
- [Resource Center](https://strapi.io/resource-center) - Strapi tutorials and guides.
- [Strapi Documentation](https://docs.strapi.io) - Official documentation.
- [Strapi Tutorials](https://strapi.io/tutorials) - Community and core team tutorials.
- [Strapi Blog](https://strapi.io/blog) - Articles from the Strapi team and community.
- [Changelog](https://strapi.io/changelog) - Updates, new features, and improvements.
- [Strapi GitHub](https://github.com/strapi/strapi) - Contribute or provide feedback.

---

## 🛠 Configuration Directory (`config/`)

This folder contains environment-specific and global configuration files for the Lead Management Backend.

### Structure
| File             | Purpose                                                                                   |
|------------------|-------------------------------------------------------------------------------------------|
| `admin.ts`       | Admin panel authentication, API token, and feature flag configuration.                     |
| `api.ts`         | REST API pagination and count settings.                                                   |
| `cron.ts`        | Scheduled (cron) jobs for background tasks (e.g., LinkedIn token management).             |
| `database.ts`    | Database connection settings (supports SQLite, MySQL, PostgreSQL).                        |
| `middlewares.ts` | Global middleware stack (CORS, security, logging, etc.).                                  |
| `plugins.ts`     | Plugin-specific configuration (GraphQL, email, etc.).                                     |
| `server.ts`      | Core server settings (host, port, app keys, cron enablement).                             |

---

## Other Modules in the Home Directory

### 1. `interfaces/`
- **Purpose**: Defines TypeScript interfaces for strong typing and contract enforcement.
- **Contents**:
  - [`linkedin-interfaces.ts`](../interfaces/linkedin-interfaces.ts):
    - `LinkedInResponse`: Models LinkedIn webhook payloads.
    - `LinkedInFormData`: Represents LinkedIn form metadata and questions.
    - `TokenData`, `LinkedInTokenResponse`: Handles OAuth token management and refresh logic.
- **Best Practices**:
  - Keep interfaces focused and descriptive.
  - Sync with external API changes.
  - Use interface extension to avoid duplication.

### 2. `types/`
- **Purpose**: Centralizes TypeScript type definitions.
- **Contents**:
  - `generated/`:
    - [`components.d.ts`](../types/generated/components.d.ts): Auto-generated types for Strapi components.
    - [`contentTypes.d.ts`](../types/generated/contentTypes.d.ts): Auto-generated types for Strapi content types.
- **Best Practices**:
  - Do not edit files in `generated/`; regenerate using Strapi or codegen tools.
  - Use for type-safe access to Strapi entities.
  - Place custom types outside `generated/` to avoid overwrites.

### 3. `assets/`
- **Purpose**: Stores static assets for secure backend operations.
- **Contents**:
  - `finspring_in.ca-bundle`, `finspring_in.crt`, `finspring_in.key`, `fullchain.pem`: SSL/TLS certificates and keys for HTTPS and third-party integrations.
- **Security Guidelines**:
  - Never commit or share private keys.
  - Restrict file permissions to necessary users/processes.
  - Rotate certificates periodically.
  - Ensure certificates are valid to avoid interruptions.

### 4. `utils/`
- **Purpose**: Contains stateless utility modules for cross-cutting concerns.
- **Contents**:
  - [`linkedin-webhook-helper.ts`](../utils/linkedin-webhook-helper.ts):
    - Helper functions for LinkedIn integration (token refresh, cleanup, webhook payload handling).

---

## Source Code Documentation (`src/`)

The `src/` directory contains all backend logic, APIs, components, and extensions. Below is a detailed breakdown of each module.

---

### 1. `src/index.ts`
- **Purpose**: Entry point for custom Strapi bootstrap logic or server extensions.

---

### 2. `src/api/`

Contains all API modules, each following Strapi’s modular structure:
- `content-types/`: Defines entity schemas/models.
- `controllers/`: Contains business logic for requests.
- `routes/`: Declares REST endpoints.
- `services/`: Encapsulates reusable data access logic.

#### 2.1. `audit-log/`
- **Purpose**: Tracks and exposes system action logs.
- **Routes**:
  - `GET /audit-logs`: List all logs.
  - `GET /audit-logs/:id`: Get a specific log.
- **Functionality**: Tracks changes, user actions, and events for compliance/debugging.

#### 2.2. `call/`
- **Purpose**: Manages call records linked to leads, with analytics for VAS, products, and call stats.

##### Folder Structure
- `content-types/`: Schema and lifecycle hooks for audit logging.
- `controllers/`: Call management and analytics logic.
- `routes/`: REST and custom endpoints.
- `services/`: Reusable data logic.

##### Standard REST Endpoints (`routes/call.ts`)
- `GET /calls`: List calls (supports filtering, pagination).
- `POST /calls`: Create a call.
- `GET /calls/:id`: Get call details.
- `PUT /calls/:id`: Update a call.
- `DELETE /calls/:id`: Delete a call.

##### Custom Routes (`routes/custom.ts`)
| Method | Path                     | Handler                      | Description                                      |
|--------|--------------------------|------------------------------|--------------------------------------------------|
| GET    | `/call/config`           | `call.customContentTypeConfig`| Get call content-type config (UI metadata)        |
| GET    | `/calls/vas-analytics`    | `call.vasAnalytics`           | VAS-wise lead analytics for calls                |
| GET    | `/calls/product-analytics`| `call.productAnalytics`    | Product-wise lead analytics for calls             |
| GET    | `/calls/called-by-stats` | `call.calledByStats`         | Call counts grouped by user (CalledBy)           |

##### Controller Responsibilities
- **customContentTypeConfig**: Returns schema with UI metadata (labels, order, components).
- **vasAnalytics**: Aggregates unique leads by VAS for a date range.
- **productAnalytics**: Aggregates leads by product for a date range.
- **calledByStats**: Returns call counts by user for a date range.
- **Lifecycle Hooks** (`content-types/call/lifecycles.ts`):
  - Logs create/update/delete actions with user attribution and field changes.
  - Ensures referential integrity.

##### Data Model Highlights (`content-types/call/schema.json`)
- **Fields**:
  - `MeetingAgenda`: Enum (Courtesy, Call for meeting, Portfolio review, Followup, None).
  - `Mode`: Enum (Email, In person, Online, Phone, Chatty, None).
  - `CalledBy`: User relation.
  - `LeadName`: Lead relation.
  - `ComponetVas`: Repeatable VAS component.
  - `ComponentProductModule`: Repeatable product component.
  - `CallPicked`: Boolean.

##### Example: VAS Analytics Flow
- `GET /call/vas-analytics?startDate=YYYY-MM-DD&endDate=YYYY-MM-DD`
- Aggregates unique leads per VAS and returns a summary.

##### Security & Validation
- Analytics endpoints are public by default (`auth: false`); restrict as needed.
- Lifecycle hooks sanitize user data and log valid changes.

**Details**: See [`src/api/call/controllers/`](src/api/call/controllers/) and [`src/api/call/routes/`](src/api/call/routes/).

#### 2.3. `campaign-mapping/`
- **Purpose**: Maps campaignName field to subLead Source.
- **Routes**:
  - `GET /campaign-mappings`: List mappings.
  - `POST /campaign-mappings`: Create a mapping.
  - `GET /campaign-mappings/:id`: Get mapping details.
  - `PUT /campaign-mappings/:id`: Update a mapping.
  - `DELETE /campaign-mappings/:id`: Delete a mapping.
- **Functionality**: Tracks campaign-driven leads.

#### 2.4. `city/`
- **Purpose**: Manages city master data.
- **Routes**:
  - `GET /cities`: List cities.
  - `POST /cities`: Create a city.
  - `GET /cities/:id`: Get city details.
  - `PUT /cities/:id`: Update a city.
  - `DELETE /cities/:id`: Delete a city.
- **Functionality**: Supports location-based filtering/reporting.

#### 2.5. `form-mapping/`
- **Purpose**: Maps formName field to subLead Source.
- **Routes**:
  - `GET /form-mappings`: List mappings.
  - `POST /form-mappings`: Create a mapping.
  - `GET /form-mappings/:id`: Get mapping details.
  - `PUT /form-mappings/:id`: Update a mapping.
  - `DELETE /form-mappings/:id`: Delete a mapping.
- **Functionality**: Integrates with external lead sources (e.g., Facebook, Google).

#### 2.6. `lead/`
- **Purpose**: Core module for lead management, supporting CRUD, analytics, bulk operations, CSV import/export, and integrations (Meta/Facebook, Google, LinkedIn, etc.).

##### Folder Structure
- `controllers/`: Lead management and integration logic.
- `routes/`: REST and custom endpoints.
- `services/`: Reusable data logic.
- `content-types/`: Lead schema/model.

##### Standard REST Endpoints (`routes/lead.ts`)
- `GET /leads`: List leads (filtering, pagination).
- `POST /leads`: Create a lead.
- `GET /leads/:id`: Get lead details.
- `PUT /leads/:id`: Update a lead.
- `DELETE /leads/:id`: Delete a lead.

##### Custom Routes (`routes/custom.ts`)
| Method | Path                            | Handler                            | Description                                     |
|--------|---------------------------------|------------------------------------|-------------------------------------------------|
| GET    | `/leads/facebook-webhook`       | `facebook-webhook.verifyWebhook`   | Verify Facebook webhook (GET challenge)         |
| POST   | `/leads/facebook-webhook`       | `facebook-webhook.receiveLead`     | Receive Facebook/Meta lead (webhook)            |
| POST   | `/leads/google-webhook`         | `wn-website-webhook.receiveLead`   | Receive Google Ads/website lead (webhook)       |
| POST   | `/leads`                        | `common-leads-webhook.receiveLead` | Receive generic lead from external source       |
| POST   | `/leads/mmp-leads`              | `mmp-leads.receiveLead`            | Receive MyMoneyPanda lead (webhook)             |
| GET    | `/leads/linkedin-webhook`       | `linkedin-webhook.verifyWebhook`   | Verify LinkedIn webhook (GET challenge)         |
| POST   | `/leads/linkedin-webhook`       | `linkedin-webhook.receiveLead`     | Receive LinkedIn lead (webhook)                 |
| POST   | `/leads/import-csv`             | `import.importCSV`                 | Bulk import leads from CSV                      |
| PUT    | `/leads/bulk-update-leads`      | `lead.bulkUpdateLeads`             | Bulk update fields for multiple leads           |
| GET    | `/leads/config`                 | `lead.customContentTypeConfig`     | Get lead content-type config (UI metadata)      |
| GET    | `/leads/form-config`            | `lead.formMappingContentTypeConfig`| Get form-mapping config                         |
| GET    | `/leads/campaign-config`        | `lead.campaignMappingContentTypeConfig`| Get campaign-mapping config                  |
| GET    | `/leads/analytics`              | `lead.analytics`                   | Get lead analytics (count in date range)        |
| POST   | `/leads/rm-stats`               | `lead.rmStats`                     | Get RM/source/subsource stats (basic)           |
| POST   | `/leads/bulk-delete`            | `lead.bulkDeleteLeads`             | Bulk delete leads by document_id                |
| POST   | `/leads/exportCSV`              | `export.exportCSV`                 | Export filtered leads as CSV                    |
| POST   | `/leads/rm-status`              | `reports.rmStats`                  | Advanced RM/source/subsource stats (filtered)   |
| POST   | `/leads/update-created-at`      | `update-createdat.update`          | Update `created_at` timestamp for a lead        |
| POST   | `/leads/filters`                | `reports.createFilter`             | Create a saved filter for lead stats            |
| GET    | `/leads/filters`                | `reports.getAllFilters`            | List all saved filters                          |
| PUT    | `/leads/filters/:id`            | `reports.updateFilter`             | Update a saved filter                           |
| DELETE | `/leads/filters/:id`            | `reports.deleteFilter`             | Delete a saved filter                           |

##### Controller Responsibilities
- **`lead.ts`**: Handles CRUD, bulk operations, analytics, CSV import/export, and UI config.
- **`facebook-webhook.ts`**: Verifies and receives Facebook/Meta leads.
- **`linkedin-webhook.ts`**: Verifies and receives LinkedIn leads.
- **`wn-website-webhook.ts`**: Receives Google Ads/website leads.
- **`common-leads-webhook.ts`**: Receives generic external leads.
- **`mmp-leads.ts`**: Receives MyMoneyPanda leads.
- **`import.ts`**: Validates and imports CSV leads.
- **`export.ts`**: Exports filtered leads as CSV.
- **`reports.ts`**: Manages filters and advanced stats.
- **`update-createdat.ts`**: Updates lead `created_at` timestamp.

##### Example Flows
- **Facebook Webhook**:
  1. GET `/leads/facebook-webhook` for verification.
  2. POST `/leads/facebook-webhook` with lead data.
  3. Fetches full lead from Graph API, maps fields, and creates lead.
- **Bulk Update**:
  - `PUT /leads/bulk-update-leads` with `{ leadIds: [...], field: "AssignedRM", value: "<document_id>" }`.
  - Resolves relations and updates leads.
- **CSV Import**:
  - `POST /leads/import-csv` with CSV file.
  - Validates fields, resolves relations, and inserts leads transactionally.

##### Security & Validation
- Webhooks require API key validation.
- CSV imports validate enums and relations.
- Bulk operations validate inputs.
- Most endpoints are `auth: false` for integrations; secure with network-level controls or API keys.

##### Extending the Module
- Add new integrations via new controllers/routes.
- Add analytics/reporting in `lead.ts` or `reports.ts`.
- Update UI config for new requirements.

**Details**: See [`src/api/lead/controllers/`](src/api/lead/controllers/) and [`src/api/lead/routes/`](src/api/lead/routes/).

#### 2.7. `lead-source/`
- **Purpose**: Manages lead sources.
- **Routes**:
  - `GET /lead-sources`: List sources.
  - `POST /lead-sources`: Create a source.
  - `GET /lead-sources/:id`: Get source details.
  - `PUT /lead-sources/:id`: Update a source.
  - `DELETE /lead-sources/:id`: Delete a source.
- **Functionality**: Supports source-based analytics.

#### 2.8. `location/`
- **Purpose**: Manages location master data with city linkage.

##### Folder Structure
- `content-types/`: Location schema.
- `controllers/`: Location logic and custom queries.
- `routes/`: REST and custom endpoints.
- `services/`: Reusable data logic.

##### Standard REST Endpoints (`routes/location.ts`)
- `GET /locations`: List locations.
- `POST /locations`: Create a location.
- `GET /locations/:id`: Get location details.
- `PUT /locations/:id`: Update a location.
- `DELETE /locations/:id`: Delete a location.

##### Custom Routes (`routes/custom-location.ts`)
| Method | Path                 | Handler            | Description                              |
|--------|----------------------|--------------------|------------------------------------------|
| GET    | `/locations/by-city` | `location.findByCity`| List locations by city (query: `city`)   |

##### Controller Responsibilities
- **findByCity**: Returns locations for a given city ID (requires `city` query param).

##### Data Model Highlights (`content-types/location/schema.json`)
- **Fields**:
  - `name`: String (location name).
  - `city`: Relation to city (many-to-one).

##### Example: Find Locations by City
- `GET /locations/by-city?city=<cityId>`
- Validates `city` param and returns locations.

##### Security & Validation
- `findByCity` is `auth: false` by default; restrict as needed.
- Returns `400` if `city` param is missing.

**Details**: See [`src/api/location/controllers/`](src/api/location/controllers/) and [`src/api/location/routes/`](src/api/location/routes/).

#### 2.9. `outside-investment/`
- **Purpose**: Tracks external investments.
- **Routes**:
  - `GET /outside-investments`: List records.
  - `POST /outside-investments`: Create a record.
  - `GET /outside-investments/:id`: Get details.
  - `PUT /outside-investments/:id`: Update a record.
  - `DELETE /outside-investments/:id`: Delete a record.
- **Functionality**: Supports financial analytics/compliance.

#### 2.10. `relevant-product-master/`
- **Purpose**: Manages product master data.
- **Routes**:
  - `GET /relevant-product-masters`: List products.
  - `POST /relevant-product-masters`: Create a product.
  - `GET /relevant-product-masters/:id`: Get product details.
  - `PUT /relevant-product-masters/:id`: Update a product.
  - `DELETE /relevant-product-masters/:id`: Delete a product.
- **Functionality**: Maps leads to products.

#### 2.11. `relevant-vas-master/`
- **Purpose**: Manages VAS master data.
- **Routes**:
  - `GET /relevant-vas-masters`: List VAS.
  - `POST /relevant-vas-masters`: Create a VAS.
  - `GET /relevant-vas-masters/:id`: Get VAS details.
  - `PUT /relevant-vas-masters/:id`: Update a VAS.
  - `DELETE /relevant-vas-masters/:id`: Delete a VAS.
- **Functionality**: Supports VAS mapping/analytics.

#### 2.12. `sub-lead-source/`
- **Purpose**: Manages sub-categories of lead sources.
- **Routes**:
  - `GET /sub-lead-sources`: List sub-sources.
  - `POST /sub-lead-sources`: Create a sub-source.
  - `GET /sub-lead-sources/:id`: Get sub-source details.
  - `PUT /sub-lead-sources/:id`: Update a sub-source.
  - `DELETE /sub-lead-sources/:id`: Delete a sub-source.
- **Functionality**: Enables granular lead origin tracking.

---

### 3. `src/components/`
- **Purpose**: Stores reusable components and dynamic zones for content-types.
- **Example**: `test/` for test components.

---

### 4. `src/extensions/`
- **Purpose**: Contains Strapi extensions and plugins.
- **Subfolders**:
  - `documentation/`: Swagger/OpenAPI docs (`/documentation` endpoint).
  - `graphql/`: Custom GraphQL resolvers and schema extensions.
  - `users-permissions/`: Custom authentication, roles, and permissions.

---

### 5. `src/admin/`
- **Purpose**: Customizes Strapi admin panel UI and logic.
- **Files**: App files, TypeScript config, and Vite config for admin development.

---

### 6. General Notes
- **Services**: Reusable business logic in each module’s `services/` folder.
- **Controllers**: Handle HTTP requests and call services.
- **Routes**: Define REST endpoints and link to controllers.
- **Content-types**: Define data models and relationships.
- **Lifecycle Hooks**: Used for validation, transformation, and side effects.
- **Security**: Protected by Strapi’s RBAC; customizable in `users-permissions`.

---

### 7. API Documentation
- **Swagger/OpenAPI**: Auto-generated at `/documentation` (see [`src/extensions/documentation/`](src/extensions/documentation/)).
- **GraphQL**: Available at `/graphql`.

---

### 8. Adding New Modules
1. Use Strapi CLI or create a folder under `src/api/`.
2. Define `content-types`, `controllers`, `routes`, and `services`.
3. Update permissions in the admin panel.

---

For more details, refer to [`src/api/`](src/api/) and the Swagger docs.

---
##  Docker Support (`Dockerfile`)

A `Dockerfile` is provided for containerized deployments.

### Key Steps

1. Uses official Node.js 20 LTS Alpine image.
2. Sets `/app` as the working directory.
3. Copies `package.json` and installs dependencies.
4. Copies the rest of the application code.
5. Exposes port `1337` (default Strapi port).
6. Starts Strapi in development mode.

**Build and Run Example:**
```bash
docker build -t lead-mgmt-backend .
docker run -p 1337:1337 --env-file .env lead-mgmt-backend
```

> **Note:**  
> For production, modify the `CMD` in the Dockerfile to use `npm run start` after building.



## 🚀 Deployment Scripts

### `deploy.sh`

A shell script to automate deployment steps on Linux servers:

- Loads Node Version Manager (NVM) environment.
- Installs dependencies.
- Builds the Strapi admin panel.
- Restarts all PM2-managed processes and saves the PM2 process list.

**Usage:**
```bash
sh deploy.sh
```
> **Note:**  
> Ensure `pm2` and `nvm` are installed and configured on your server.

---


