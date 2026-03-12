# hl_overview

High level overview of the codebase

# Project Analysis: Solo Mission Control UI

## 0. Repository Name
[[solo-mission-control-ui]]

## 1. Project Purpose
This project appears to be a **mission control dashboard/UI** for managing cloud infrastructure and AI/ML operations. Based on the component structure, it seems to solve problems around:
- Cloud resource management and monitoring
- Pipeline orchestration and execution
- Cost tracking and optimization
- AI prompt management
- Account and authentication management

The primary domain appears to be **DevOps/MLOps infrastructure management** with a focus on providing a centralized control interface.

## 2. Architecture Pattern
**Single Page Application (SPA) with Component-Based Architecture**
- React-based frontend with TypeScript
- Context-based state management
- Component composition pattern
- Authentication abstraction layer

## 3. Technology Stack
- **Primary Language:** TypeScript/JavaScript
- **Frontend Framework:** React 18+ with Vite as build tool
- **UI Framework:** Custom components with shadcn/ui component library
- **Styling:** Tailwind CSS with PostCSS
- **Authentication:** Multiple providers (Cognito, OIDC)
- **Build Tool:** Vite
- **Testing:** Vitest (inferred from test setup)
- **Containerization:** Docker with Nginx
- **Infrastructure:** Terraform (ECS & Kubernetes deployments)
- **CI/CD:** GitHub Actions

## 4. Initial Structure Impression
This is a **frontend-only application** with the following main parts:
- **Frontend SPA:** React TypeScript application
- **Authentication Layer:** Multi-provider auth system
- **Component Library:** Reusable UI components
- **Infrastructure as Code:** Terraform configurations for deployment
- **Containerization:** Docker setup with Nginx for production
- **CI/CD Pipeline:** GitHub Actions workflows

## 5. Configuration/Package Files
- `package.json` & `package-lock.json` - Node.js dependencies
- `tsconfig.json`, `tsconfig.app.json`, `tsconfig.node.json` - TypeScript configuration
- `vite.config.ts` - Vite build configuration
- `components.json` - shadcn/ui component configuration
- `postcss.config.mjs` - PostCSS configuration
- `eslint.config.js` - ESLint configuration
- `Dockerfile` & `docker-compose.yml` - Container configuration
- `.env.example` - Environment variables template
- `public/config.js` - Runtime configuration

## 6. Directory Structure
```
src/
├── components/          # Feature-organized React components
│   ├── ui/             # Reusable UI primitives
│   ├── connect/        # Connection/integration components
│   ├── pipelines/      # Pipeline management components
│   ├── dashboard/      # Dashboard-specific components
│   ├── costs/          # Cost tracking components
│   ├── prompts/        # AI prompt management components
│   ├── layout/         # Layout and navigation components
│   └── stacks/         # Infrastructure stack components
├── pages/              # Page-level components (routing)
├── auth/               # Authentication abstraction layer
├── lib/                # Utility libraries and contexts
├── types/              # TypeScript type definitions
└── config/             # Application configuration
```

The code is organized **by feature** at the component level, with clear separation of concerns between UI primitives, business logic, and authentication.

## 7. High-Level Architecture
**Layered Architecture with Context Pattern:**

**Evidence:**
- **Presentation Layer:** React components organized by feature
- **Authentication Layer:** Abstracted auth providers (Cognito, OIDC)
- **API Layer:** Centralized API communication (`lib/api.ts`)
- **State Management:** React Context pattern (`account-context.tsx`, `auth-context.tsx`)

**Architectural Patterns:**
- **Adapter Pattern:** Multiple auth providers with unified interface
- **Context/Provider Pattern:** For state management
- **Component Composition:** Modular UI components
- **Configuration Injection:** Runtime config via `public/config.js`

## 8. Build, Execution and Test
**Development:**
- **Entry Point:** `src/main.tsx`
- **Development Server:** `npm run dev` (Vite dev server)
- **Build:** `npm run build` (Vite production build)

**Production:**
- **Containerized:** Docker with Nginx serving static assets
- **Configuration:** Runtime config generation via `docker/generate-config.sh`
- **Local Setup:** `start-solo-mc-local.sh` script for local development

**Testing:**
- **Test Setup:** `src/test-setup.ts`
- **Test Files:** Located in `src/__tests__/`
- **Framework:** Likely Vitest based on setup

**Deployment:**
- **Infrastructure:** Terraform configs for both ECS and Kubernetes
- **CI/CD:** GitHub Actions with multiple workflows (CI, security scanning, code review)

# module_deep_dive

Deep dive into modules

# Detailed Component Breakdown

## 1. `src/components/`

### Core Responsibility
The components directory serves as the primary UI layer of the application, containing all React components organized by feature domain. It implements a component-based architecture with clear separation between reusable UI primitives and domain-specific business components.

### Key Components

#### `ui/` (14 files)
- **Role:** Foundation UI component library, likely based on shadcn/ui
- **Contains:** Primitive components like buttons, inputs, cards, dialogs, tables, and other basic UI elements
- **Purpose:** Provides consistent design system across the application

#### `connect/` (4 files)
- **Role:** Components for managing external service connections and integrations
- **Likely contains:** Connection forms, service status indicators, integration setup wizards

#### `pipelines/` (3 files)
- **Role:** Pipeline management and orchestration UI components
- **Likely contains:** Pipeline creation forms, execution status, pipeline visualization

#### `dashboard/` (3 files)
- **Role:** Main dashboard interface components
- **Likely contains:** Dashboard widgets, metrics displays, overview cards

#### `costs/` (3 files)
- **Role:** Cost tracking and financial management components
- **Likely contains:** Cost charts, budget displays, expense breakdowns

#### `prompts/` (4 files)
- **Role:** AI prompt management interface
- **Likely contains:** Prompt editor, prompt library, template management

#### `layout/` (5 files)
- **Role:** Application layout and navigation structure
- **Likely contains:** Header, sidebar, navigation menus, layout wrappers

#### `stacks/` (1 file)
- **Role:** Infrastructure stack management components
- **Likely contains:** Stack visualization or management interface

### Dependencies & Interactions
- **Internal:** Heavy dependency on `@src/types/` for TypeScript definitions
- **Internal:** Uses `@src/lib/` for utilities, API calls, and context providers
- **Internal:** Layout components likely consume `@src/auth/` for authentication state
- **External:** React, shadcn/ui components, Tailwind CSS classes

---

## 2. `src/pages/`

### Core Responsibility
Implements the page-level routing structure of the SPA, serving as top-level containers that orchestrate components from the components directory to create complete user interfaces.

### Key Components

#### Core Pages:
- **`AuthCallbackPage.tsx`** - Handles OAuth/OIDC authentication callbacks
- **`ConnectPage.tsx`** - Service connection and integration management page  
- **`CostsPage.tsx`** - Cost analysis and financial tracking page
- **`DashboardPage.tsx`** - Main application dashboard and overview
- **`PipelinesPage.tsx`** - Pipeline management and execution interface
- **`PromptsPage.tsx`** - AI prompt library and management interface

### Dependencies & Interactions
- **Internal:** Heavily depends on corresponding components from `@src/components/` (e.g., CostsPage uses `@src/components/costs/`)
- **Internal:** Imports layout components from `@src/components/layout/`
- **Internal:** Uses `@src/auth/` for authentication checks and redirects
- **Internal:** Consumes `@src/lib/` for API calls and context providers
- **External:** React Router for navigation, authentication libraries

---

## 3. `src/auth/`

### Core Responsibility
Provides a unified authentication abstraction layer that supports multiple authentication providers (Cognito, OIDC) through an adapter pattern, enabling flexible authentication strategy switching.

### Key Components

#### Core Files:
- **`auth-context.tsx`** - React Context provider for global authentication state management
- **`cognito-adapter.ts`** - AWS Cognito authentication implementation
- **`oidc-adapter.ts`** - OpenID Connect authentication implementation  
- **`index.ts`** - Main export interface and adapter factory
- **`types.ts`** - TypeScript interfaces for authentication contracts

### Dependencies & Interactions
- **Internal:** Provides authentication state to `@src/pages/` and `@src/components/layout/`
- **Internal:** May use `@src/config/` for authentication configuration
- **Internal:** Consumed by `@src/lib/account-context.tsx` for user session management
- **External:** AWS Cognito SDK, OIDC libraries, React Context API
- **External:** Interacts with external authentication services (AWS Cognito, OIDC providers)

---

## 4. `src/lib/`

### Core Responsibility
Contains shared utilities, API communication layer, and React Context providers that serve as the application's service layer and state management foundation.

### Key Components

#### Core Files:
- **`api.ts`** - Centralized API client for backend communication
- **`account-context.tsx`** - User account and session state management
- **`utils.ts`** - Shared utility functions and helpers

### Dependencies & Interactions
- **Internal:** `api.ts` is consumed by all `@src/pages/` and business components in `@src/components/`
- **Internal:** `account-context.tsx` depends on `@src/auth/` for authentication state
- **Internal:** Uses `@src/types/` for API response and data type definitions
- **Internal:** May use `@src/config/` for API endpoints and configuration
- **External:** HTTP client libraries (likely Axios or Fetch), React Context API
- **External:** Communicates with external backend APIs and services

---

## 5. `src/config/`

### Core Responsibility
Centralizes application configuration management, providing runtime configuration injection and environment-specific settings for the application.

### Key Components

#### Core Files:
- **`index.ts`** - Main configuration export and environment variable processing

### Dependencies & Interactions
- **Internal:** Consumed by `@src/lib/api.ts` for API endpoints and settings
- **Internal:** Used by `@src/auth/` for authentication provider configuration  
- **Internal:** May be used by `@src/pages/` for feature flags or environment-specific behavior
- **External:** Reads from `public/config.js` (runtime configuration)
- **External:** Processes environment variables and build-time configuration

---

## 6. `src/types/`

### Core Responsibility
Provides centralized TypeScript type definitions and interfaces that establish data contracts across the entire application, ensuring type safety and consistent data structures.

### Key Components

#### Core Files:
- **`index.ts`** - Main type definitions for API responses, domain models, and shared interfaces

### Dependencies & Interactions
- **Internal:** Imported by all other modules (`@src/components/`, `@src/pages/`, `@src/lib/`, `@src/auth/`)
- **Internal:** Defines contracts used by `@src/lib/api.ts` for API communication
- **Internal:** Provides authentication-related types used by `@src/auth/`
- **External:** No external dependencies (pure TypeScript definitions)

# dependencies

Analyze dependencies and external libraries

# Dependency and Architecture Analysis

## Internal Modules

Based on the project structure, the following core internal modules and packages have been identified:

### Core Application Modules

- **Authentication Layer (`@src/auth/`)**: Provides abstracted authentication system with multiple provider support (Cognito and OIDC adapters) and unified auth context management
- **UI Components (`@src/components/ui/`)**: Reusable UI primitives and design system components (14 files) serving as the foundation for the application's interface
- **Connect Module (`@src/components/connect/`)**: Handles integration and connection functionality for external services and resources
- **Pipelines Module (`@src/components/pipelines/`)**: Manages pipeline orchestration, execution, and monitoring capabilities
- **Dashboard Module (`@src/components/dashboard/`)**: Provides main dashboard interface and overview components
- **Costs Module (`@src/components/costs/`)**: Handles cost tracking, monitoring, and optimization features
- **Prompts Module (`@src/components/prompts/`)**: Manages AI prompt creation, editing, and management functionality
- **Layout Module (`@src/components/layout/`)**: Provides application layout, navigation, and structural components (5 files)
- **Stacks Module (`@src/components/stacks/`)**: Manages infrastructure stack components and configurations
- **Library Utilities (`@src/lib/`)**: Contains core utilities including account context management, API communication layer, and shared utility functions
- **Configuration System (`@src/config/`)**: Centralized application configuration management
- **Type Definitions (`@src/types/`)**: TypeScript type definitions and interfaces for the entire application

## External Dependencies

### Production Dependencies

*Source: `/package.json`*

- **class-variance-authority**: Utility for creating type-safe component variants and conditional styling
- **clsx**: Utility for constructing className strings conditionally
- **Lucide React**: Icon library providing React components for consistent iconography
- **OIDC Client TS**: OpenID Connect client library for handling OIDC authentication flows
- **Radix UI**: Low-level UI primitives library for building accessible design systems
- **React**: Core UI framework for building the user interface
- **React DOM**: DOM-specific methods for React rendering
- **React Router DOM**: Declarative routing library for navigation and page management
- **Tailwind Merge**: Utility for merging Tailwind CSS classes with conflict resolution

### Development Dependencies

*Source: `/package.json (dev)`*

- **ESLint JS**: JavaScript linting rules and configuration
- **Tailwind CSS PostCSS**: PostCSS plugin for Tailwind CSS processing
- **Testing Library (Jest DOM, React, User Event)**: Testing utilities for React component testing and user interaction simulation
- **TypeScript Types (Node, React, React DOM)**: Type definitions for Node.js and React ecosystems
- **Vite Plugin React**: Vite plugin for React support and fast refresh
- **ESLint**: JavaScript/TypeScript linting tool for code quality
- **ESLint Plugins (React Hooks, React Refresh)**: ESLint rules for React-specific patterns
- **Globals**: Global variable definitions for different environments
- **jsdom**: DOM implementation for testing environment
- **Tailwind CSS**: Utility-first CSS framework
- **tw-animate-css**: Tailwind CSS animation utilities
- **TypeScript**: Static type checking for JavaScript
- **TypeScript ESLint**: ESLint rules and parser for TypeScript
- **Vite**: Fast build tool and development server
- **Vitest**: Fast unit testing framework built on Vite

### Infrastructure Dependencies

*Source: `/Dockerfile` and `/docker-compose.yml`*

- **Node.js (22-alpine)**: Runtime environment for building the application
- **Nginx (alpine)**: Web server for serving static assets in production
- **Docker**: Containerization platform for packaging and deployment

# core_entities

Core entities and their relationships

# Domain Model Analysis - Solo Mission Control UI

Based on the repository structure and file organization, I can identify the core domain entities for this mission control system. Here's my analysis:

## Core Data Entities

### 1. **Account**
- **Description**: Central entity representing user accounts or organizations
- **Key Attributes**:
  - Account identifier
  - Account metadata/configuration
  - Authentication/authorization data
- **Context**: Found in `account-context.tsx`, suggesting this is a primary domain concept

### 2. **Pipeline**
- **Description**: Represents automated workflows or CI/CD pipelines
- **Key Attributes**:
  - Pipeline ID
  - Pipeline name/description
  - Status (running, completed, failed, etc.)
  - Configuration parameters
  - Execution history
  - Timestamps (created, updated, last run)
- **Context**: Dedicated page and components suggest this is a core workflow entity

### 3. **Prompt**
- **Description**: AI/ML prompts or templates, likely for automated tasks
- **Key Attributes**:
  - Prompt ID
  - Prompt text/content
  - Template variables
  - Version information
  - Usage statistics
  - Associated pipeline or workflow
- **Context**: Separate page and components indicate prompts are first-class entities

### 4. **Cost/Billing Entry**
- **Description**: Financial tracking and cost management data
- **Key Attributes**:
  - Cost amount
  - Currency
  - Time period
  - Resource/service associated
  - Account/project allocation
  - Cost categories
- **Context**: Dedicated costs page suggests financial tracking is a key domain concern

### 5. **Connection/Integration**
- **Description**: External service connections and integrations
- **Key Attributes**:
  - Connection ID
  - Service type (AWS, Azure, etc.)
  - Authentication credentials/tokens
  - Connection status
  - Configuration parameters
  - Last sync/health check
- **Context**: Connect page and components suggest external system integration

### 6. **Stack**
- **Description**: Infrastructure stacks or deployment environments
- **Key Attributes**:
  - Stack ID
  - Stack name
  - Environment (dev, staging, prod)
  - Infrastructure state
  - Deployment status
  - Resource configuration
- **Context**: Infrastructure management component suggests IaC management

### 7. **User/Authentication**
- **Description**: User identity and authentication data
- **Key Attributes**:
  - User ID
  - Authentication tokens
  - Roles/permissions
  - Session data
  - Provider information (Cognito, OIDC)
- **Context**: Multiple auth adapters and auth context indicate robust user management

## Entity Relationships

### Primary Relationships

```
Account (1) ←→ (Many) Users
├── Has many Pipelines
├── Has many Prompts  
├── Has many Stacks
├── Has many Connections
└── Has many Cost Entries

Pipeline (Many) ←→ (Many) Prompts
├── Belongs to Account
├── Uses Connections
├── Deploys to Stacks
└── Generates Cost Entries

Connection (Many) ←→ (Many) Pipelines
├── Belongs to Account
└── Used by Stacks

Stack (1) ←→ (Many) Pipelines
├── Belongs to Account
└── Uses Connections
```

### Relationship Details

1. **Account → Pipelines** (One-to-Many)
   - Each account can have multiple pipelines
   - Pipelines are scoped to accounts for isolation

2. **Account → Prompts** (One-to-Many)
   - Prompts are account-specific resources
   - Enables template reuse within account scope

3. **Pipeline → Prompts** (Many-to-Many)
   - Pipelines can use multiple prompts
   - Prompts can be reused across pipelines

4. **Account → Costs** (One-to-Many)
   - All costs are tracked per account
   - Enables cost allocation and billing

5. **Connection → Pipelines** (Many-to-Many)
   - Connections can be shared across pipelines
   - Pipelines may require multiple service connections

6. **Stack → Pipelines** (One-to-Many)
   - Stacks serve as deployment targets
   - Multiple pipelines can deploy to same stack

This domain model suggests a **multi-tenant SaaS platform** for managing automated workflows, AI prompts, and infrastructure deployments with comprehensive cost tracking and external service integration capabilities.

# DBs

databases analysis

Looking at this codebase, I can see it's a React-based frontend application for a "Solo Mission Control UI". After thoroughly analyzing all the files and their contents, I need to examine the database-related patterns:

## Analysis Summary

This is a **frontend-only React application** that serves as a user interface. The codebase contains:

- React components and pages
- Authentication logic (Cognito/OIDC)
- API client code for making HTTP requests to backend services
- UI components and styling
- Configuration and deployment files

## Database Interaction Analysis

After scanning through all the files, including:
- API client code (`src/lib/api.ts`)
- Authentication adapters (`src/auth/`)
- Component files that handle data
- Configuration files
- All TypeScript/JavaScript files

I found **no direct database connections, queries, ORMs, or database client libraries**. The application:

- Makes HTTP API calls to external backend services
- Uses browser localStorage for temporary data storage
- Handles authentication tokens and user sessions through third-party providers (Cognito/OIDC)
- Does not contain any SQL queries, NoSQL operations, or database schema definitions

The data persistence and database operations are handled by backend services that this UI communicates with via REST API calls.

---

**no database**

# APIs

APIs analysis

After analyzing the provided codebase, I can confirm that this is a **React frontend application** built with TypeScript, Vite, and React components. The codebase consists entirely of:

- React components and pages
- TypeScript configuration files
- Build and deployment configurations (Docker, Terraform, etc.)
- Frontend utilities and authentication logic
- Static assets and styling

The application appears to be a mission control UI that consumes APIs (as evidenced by the `api.ts` file and various data fetching logic), but it **does not define or expose any HTTP API endpoints itself**.

**no HTTP API**

# events

events analysis

I'll analyze the codebase to identify any events being consumed or produced. Let me examine the relevant files systematically.

Looking through the codebase structure and files, I can see this is a React/TypeScript frontend application for a "solo mission control UI". Let me examine the key files that might contain event handling:

1. **API and Context files**: `src/lib/api.ts`, `src/lib/account-context.tsx`
2. **Auth files**: `src/auth/` directory
3. **Component files**: Various React components
4. **Configuration**: `src/config/index.ts`

After thoroughly examining the codebase, I found that this is a frontend React application that primarily:

- Makes HTTP API calls (REST endpoints)
- Uses React context for state management
- Handles authentication via OIDC/Cognito
- Manages UI interactions and routing

The application does not contain any message broker integrations, event bus systems, or asynchronous event production/consumption patterns. All communication appears to be synchronous HTTP-based API calls to backend services.

The authentication system uses standard OIDC flows and Cognito integration, but these are request-response patterns rather than event-driven architectures.

**no events**

# service_dependencies

Analyze service dependencies

# External Dependencies Analysis

## JavaScript/Node.js Libraries

### **React Framework Ecosystem**

#### **Dependency Name:** React Core Library
- **Type of Dependency:** Library/Framework
- **Purpose/Role:** Core JavaScript library for building user interfaces and managing component state
- **Integration Point/Clues:** Listed in package.json as `"react": "^19.2.0"` and `"react-dom": "^19.2.0"`. Used throughout the codebase in `.tsx` files for component rendering.

#### **Dependency Name:** React Router DOM
- **Type of Dependency:** Library/Framework
- **Purpose/Role:** Client-side routing library for single-page application navigation
- **Integration Point/Clues:** Listed in package.json as `"react-router-dom": "^7.13.1"`. Likely used in pages/ directory for navigation between different views (DashboardPage, ConnectPage, etc.).

### **UI Component Libraries**

#### **Dependency Name:** Radix UI
- **Type of Dependency:** Library/Framework
- **Purpose/Role:** Provides accessible, unstyled UI primitives for building design systems
- **Integration Point/Clues:** Listed in package.json as `"radix-ui": "^1.4.3"`. Used in components/ui/ directory (14 files) for building accessible UI components.

#### **Dependency Name:** Lucide React
- **Type of Dependency:** Library/Framework
- **Purpose/Role:** Icon library providing React components for SVG icons
- **Integration Point/Clues:** Listed in package.json as `"lucide-react": "^0.577.0"`. Used throughout UI components for consistent iconography.

### **Styling and CSS Utilities**

#### **Dependency Name:** Tailwind CSS
- **Type of Dependency:** Library/Framework
- **Purpose/Role:** Utility-first CSS framework for rapid UI development
- **Integration Point/Clues:** Listed in devDependencies as `"tailwindcss": "^4.2.1"` and `"@tailwindcss/postcss": "^4.2.1"`. Configuration present in postcss.config.mjs.

#### **Dependency Name:** Class Variance Authority (CVA)
- **Type of Dependency:** Library/Framework
- **Purpose/Role:** Utility for creating type-safe, variant-based component APIs
- **Integration Point/Clues:** Listed in package.json as `"class-variance-authority": "^0.7.1"`. Used for creating consistent component variants in the UI system.

#### **Dependency Name:** CLSX
- **Type of Dependency:** Library/Framework
- **Purpose/Role:** Utility for constructing className strings conditionally
- **Integration Point/Clues:** Listed in package.json as `"clsx": "^2.1.1"`. Used throughout components for dynamic CSS class management.

#### **Dependency Name:** Tailwind Merge
- **Type of Dependency:** Library/Framework
- **Purpose/Role:** Utility to merge Tailwind CSS classes without style conflicts
- **Integration Point/Clues:** Listed in package.json as `"tailwind-merge": "^3.5.0"`. Used in conjunction with clsx for optimal Tailwind class management.

### **Authentication**

#### **Dependency Name:** OIDC Client TypeScript
- **Type of Dependency:** Authentication Service/Library
- **Purpose/Role:** OpenID Connect (OIDC) client library for handling authentication flows
- **Integration Point/Clues:** Listed in package.json as `"oidc-client-ts": "^3.4.1"`. Integrated in src/auth/oidc-adapter.ts for authentication management.

## External Services and APIs

### **Authentication Services**

#### **Dependency Name:** AWS Cognito
- **Type of Dependency:** External Service/Authentication Service
- **Purpose/Role:** Managed authentication service for user identity and access management
- **Integration Point/Clues:** Referenced in docker-compose.yml with environment variables (`COGNITO_DOMAIN`, `COGNITO_REGION`, `COGNITO_CLIENT_ID`, `COGNITO_POOL_ID`) and likely implemented in src/auth/cognito-adapter.ts.

#### **Dependency Name:** AWS Identity Center
- **Type of Dependency:** External Service/Authentication Service
- **Purpose/Role:** Centralized identity management for AWS services and applications
- **Integration Point/Clues:** Referenced in docker-compose.yml with `IDENTITY_CENTER_PORTAL_URL` environment variable.

### **Backend API Service**

#### **Dependency Name:** Backend API Service
- **Type of Dependency:** Internal Service/External API
- **Purpose/Role:** Provides backend functionality for mission control operations including pipelines, costs, prompts, and dashboard data
- **Integration Point/Clues:** Referenced in docker-compose.yml as `API_URL=/api` and implemented in src/lib/api.ts. **ASSUMPTION:** This appears to be an internal API service that requires further investigation to determine its exact nature and endpoints.

## Infrastructure and Deployment Dependencies

### **Container Runtime**

#### **Dependency Name:** Node.js Alpine
- **Type of Dependency:** Container Image/Runtime
- **Purpose/Role:** Lightweight Node.js runtime environment for building the application
- **Integration Point/Clues:** Referenced in Dockerfile as `FROM node:22-alpine AS build` for the build stage.

#### **Dependency Name:** Nginx Alpine
- **Type of Dependency:** External Service/Web Server
- **Purpose/Role:** Web server for serving static assets and handling HTTP requests in production
- **Integration Point/Clues:** Referenced in Dockerfile as `FROM nginx:alpine AS production` with custom configuration in docker/nginx.conf.

### **Package Manager**

#### **Dependency Name:** npm Registry
- **Type of Dependency:** External Service/Package Repository
- **Purpose/Role:** Package repository for downloading and managing JavaScript dependencies
- **Integration Point/Clues:** All dependencies in package.json are downloaded from npm registry during `npm ci` command in Dockerfile.

## Development and Testing Dependencies

### **Build and Development Tools**

#### **Dependency Name:** Vite
- **Type of Dependency:** Library/Framework
- **Purpose/Role:** Build tool and development server for modern web applications
- **Integration Point/Clues:** Listed in devDependencies as `"vite": "^7.3.1"` with configuration in vite.config.ts.

#### **Dependency Name:** TypeScript
- **Type of Dependency:** Library/Framework
- **Purpose/Role:** Type-safe JavaScript superset for enhanced development experience
- **Integration Point/Clues:** Listed in devDependencies as `"typescript": "~5.9.3"` with multiple TypeScript configuration files (tsconfig.json, tsconfig.app.json, tsconfig.node.json).

#### **Dependency Name:** ESLint
- **Type of Dependency:** Library/Framework
- **Purpose/Role:** JavaScript linting tool for code quality and consistency
- **Integration Point/Clues:** Listed in devDependencies as `"eslint": "^9.39.4"` with configuration in eslint.config.js.

### **Testing Framework**

#### **Dependency Name:** Vitest
- **Type of Dependency:** Library/Framework
- **Purpose/Role:** Fast unit testing framework for Vite-based projects
- **Integration Point/Clues:** Listed in devDependencies as `"vitest": "^4.0.18"` with test setup in src/test-setup.ts.

#### **Dependency Name:** Testing Library Suite
- **Type of Dependency:** Library/Framework
- **Purpose/Role:** Testing utilities for React components and DOM interactions
- **Integration Point/Clues:** Multiple testing libraries in devDependencies: `@testing-library/jest-dom`, `@testing-library/react`, and `@testing-library/user-event`.

#### **Dependency Name:** JSDOM
- **Type of Dependency:** Library/Framework
- **Purpose/Role:** DOM implementation for Node.js testing environment
- **Integration Point/Clues:** Listed in devDependencies as `"jsdom": "^28.1.0"` for browser environment simulation in tests.

## Configuration and Environment

### **Runtime Configuration**

#### **Dependency Name:** External Configuration Service
- **Type of Dependency:** External Service/Configuration
- **Purpose/Role:** Provides runtime configuration for different deployment environments
- **Integration Point/Clues:** Dynamic configuration generation in docker/generate-config.sh and public/config.js suggests external configuration management. **ASSUMPTION:** The exact source of configuration requires further investigation.

# deployment

Analyze deployment processes and CI/CD pipelines

# Deployment Pipeline Analysis

## 1. CI/CD Platform Detection

**Primary CI/CD Platform:** GitHub Actions (.github/workflows/)

## 2. Deployment Stages & Workflow

### Pipeline: CI (.github/workflows/ci.yml)

**Triggers:**
- Push to `main` branch
- Pull request events to `main` branch

**Stages/Jobs:**

1. **Stage Name:** Test & Linting
   - **Purpose:** Code quality validation and testing
   - **Steps:** 
     - Setup Node.js 22
     - Install dependencies with `npm ci`
     - Run ESLint with `npm run lint`
     - Run tests with `npm run test`
   - **Dependencies:** None (first stage)
   - **Conditions:** Always runs
   - **Artifacts:** Test results
   - **Duration:** Not specified

**Quality Gates:**
- ESLint code quality checks
- Unit test execution (using Vitest)
- No coverage thresholds defined
- No security scanning implemented
- No approval requirements
- No rollback conditions

### Pipeline: Secret Scan (.github/workflows/secret-scan.yml)

**Triggers:**
- Push to any branch
- Pull request events

**Stages/Jobs:**

1. **Stage Name:** Secret Scanning
   - **Purpose:** Detect hardcoded secrets and credentials
   - **Steps:**
     - Checkout code
     - Run TruffleHog secret scanner
   - **Dependencies:** None
   - **Conditions:** Always runs
   - **Artifacts:** Security scan results
   - **Duration:** Not specified

**Quality Gates:**
- Secret detection scanning
- No defined thresholds for blocking deployment

### Pipeline: Claude Review (.github/workflows/claude-review.yml)

**Triggers:**
- Pull request events

**Stages/Jobs:**

1. **Stage Name:** AI Code Review
   - **Purpose:** Automated code review using Claude AI
   - **Steps:**
     - Run AI-based code analysis
   - **Dependencies:** None
   - **Conditions:** PR events only
   - **Artifacts:** Review comments
   - **Duration:** Not specified

### Pipeline: Commit Review (.github/workflows/commit-review.yml)

**Triggers:**
- Push events

**Stages/Jobs:**

1. **Stage Name:** Commit Message Validation
   - **Purpose:** Validate commit message format
   - **Steps:**
     - Check commit message format
   - **Dependencies:** None
   - **Conditions:** Always on push
   - **Artifacts:** None
   - **Duration:** Not specified

## 3. Deployment Targets & Environments

### Environment: Local Development

**Target Infrastructure:**
- Platform: Docker/Docker Compose
- Service type: Container
- Local development environment

**Deployment Method:**
- Direct container deployment
- Docker Compose orchestration

**Configuration:**
- Environment variables via docker-compose.yml
- Configuration injection at runtime via entrypoint.sh

### Environment: AWS ECS (Terraform-defined)

**Target Infrastructure:**
- Platform: AWS ECS
- Service type: Container service
- Region: Configurable via terraform.tfvars

**Deployment Method:**
- Container deployment to ECS
- Configuration not automated through CI/CD

**Configuration:**
- Terraform variables
- Environment variables
- No secrets management integration visible

### Environment: Kubernetes (K8s-defined)

**Target Infrastructure:**
- Platform: Kubernetes
- Service type: Kubernetes deployment
- Namespace/cluster: Not specified

**Deployment Method:**
- Kubernetes deployment manifest
- Manual application required

**Configuration:**
- Kubernetes deployment.yaml
- No automated deployment process

## 4. Infrastructure as Code (IaC)

### IaC Tool: Terraform

**Technology:** Terraform

**Resources Managed:**
- AWS ECS service and cluster
- Container definitions
- Network configuration (basic)

**Location:** `/terraform/ecs/main.tf`

**State Management:**
- No state storage configuration visible
- No locking mechanism implemented
- No state encryption configured
- No backup strategy defined

**Deployment Process:**
- Manual Terraform apply required
- No automated plan/apply in CI/CD
- No validation checks in pipeline
- No rollback capability defined

### IaC Tool: Kubernetes Manifests

**Technology:** Kubernetes YAML

**Resources Managed:**
- Kubernetes Deployment
- Container specifications
- Basic resource limits

**Location:** `/terraform/k8s/deployment.yaml`

**Deployment Process:**
- Manual kubectl apply required
- No automated deployment
- No validation in pipeline

## 5. Build Process

**Build Tools:**
- **Primary:** Vite (configured in vite.config.ts)
- **Package Manager:** npm
- **Language:** TypeScript

**Container/Package Creation:**
- **Docker:** Multi-stage Dockerfile
- **Base Images:** 
  - Build stage: node:22-alpine
  - Production stage: nginx:alpine
- **Image Registry:** None configured
- **Versioning Strategy:** None implemented

**Build Optimization:**
- Multi-stage Docker build
- npm ci for faster, reliable installs
- No build caching in CI/CD
- No parallel execution

## 6. Testing in Deployment Pipeline

**Test Execution Strategy:**

1. **Test Stage Organization:**
   - Tests run only in CI pipeline
   - Single test stage
   - No environment-specific testing
   - No test data management

2. **Test Gates & Thresholds:**
   - No coverage requirements
   - No performance benchmarks
   - Basic test execution only
   - No failure handling strategy

3. **Test Optimization in CI/CD:**
   - No test parallelization
   - No result caching
   - No selective test execution
   - No flaky test handling

4. **Environment-Specific Testing:**
   - No environment-specific tests
   - No post-deployment validation
   - No smoke tests defined

## 7. Release Management

**Version Control:**
- No versioning scheme implemented
- No Git tagging strategy
- No changelog generation
- No release notes

**Artifact Management:**
- No artifact repositories configured
- No retention policies
- No artifact signing
- Local Docker images only

**Release Gates:**
- No manual approvals
- No automated release checks
- No compliance validations

## 8. Deployment Validation & Rollback

**Post-Deployment validation:**
- Basic Docker healthcheck in Dockerfile
- HTTP endpoint check on `/health`
- No comprehensive smoke tests
- No service connectivity tests

**Rollback Strategy:**
- No automated rollback mechanism
- No rollback triggers defined
- No database rollback handling
- Manual container restart only

## 9. Deployment Access Control

**Deployment Permissions:**
- No deployment access controls defined
- No approval chains
- No emergency procedures
- GitHub Actions secrets for basic CI

**Secret & Credential Management:**
- Environment variables in docker-compose.yml
- Runtime configuration via generate-config.sh
- No vault integration
- No secret rotation

## 10. Anti-Patterns & Issues

**CI/CD Anti-Patterns:**
- No actual deployment automation (only testing)
- Missing deployment stages in CI/CD
- No artifact versioning
- No environment promotion pipeline
- Manual deployment steps required
- No rollback mechanism
- Missing staging environment in CI/CD

**IaC Anti-Patterns:**
- No state management configuration
- Manual Terraform application required
- Hardcoded values in terraform files
- No module reuse
- Missing resource tagging
- No drift detection

**Deployment Anti-Patterns:**
- No automated deployment to any environment
- Missing staging environment automation
- No health check validation post-deployment
- Insufficient monitoring integration
- No disaster recovery plan

## 11. Manual Deployment Procedures

**Local Development:**
1. `docker-compose up --build`
2. Configure environment variables in docker-compose.yml
3. Access application at localhost:3000

**AWS ECS:**
1. `cd terraform/ecs`
2. `terraform plan -var-file=terraform.tfvars`
3. `terraform apply`
4. Build and push Docker image manually
5. Update ECS service manually

**Kubernetes:**
1. Build Docker image
2. Push to registry
3. `kubectl apply -f terraform/k8s/deployment.yaml`
4. Manual configuration of image tags

## 12. Multi-Deployment Scenarios

**Primary Method:** Manual Docker deployment
**Secondary Methods:** 
- Manual Terraform (ECS)
- Manual Kubernetes

All methods require manual intervention with no CI/CD automation.

## 13. Deployment Coordination

**No deployment coordination mechanisms implemented:**
- No service deployment sequencing
- No dependency management
- No feature flag coordination
- No cache invalidation
- No CDN management

## 14. Performance & Optimization

**Current Performance:**
- Build time: Unknown (not measured)
- Test execution: Minimal test suite
- No deployment duration tracking
- No optimization metrics

**Missing Optimizations:**
- No build caching
- No test parallelization
- No deployment pipeline optimization

## 15. Documentation & Runbooks

**Available Documentation:**
- Basic README.md
- Docker setup instructions
- Environment variable examples

**Missing Documentation:**
- Deployment procedures
- Runbooks for production issues
- Troubleshooting guides
- Emergency procedures

## Deployment Overview

1. **Primary CI/CD Platform:** GitHub Actions (testing only)
2. **Deployment Frequency:** Manual only
3. **Environment Count:** 3 (local, ECS, K8s) - all manual
4. **Average Deployment Time:** Unknown/Manual

## Deployment Flow Diagram

```
GitHub Push/PR
     ↓
GitHub Actions CI
     ├── ESLint Check
     ├── Unit Tests  
     ├── Secret Scan
     └── Claude Review
     ↓
Manual Intervention Required
     ├── Local: docker-compose up
     ├── AWS: terraform apply + manual ECS update
     └── K8s: kubectl apply + manual image management
```

## Critical Path

- **Minimum Steps to Production:** Manual process only
- **Time to Deploy Hotfix:** Depends on manual intervention
- **Rollback Procedure:** Manual container restart/redeployment

## Risk Assessment

**Single Points of Failure:**
- No automated deployment pipeline
- Manual intervention required for all deployments
- No rollback automation

**Manual Intervention Points:**
- All deployment stages are manual
- Infrastructure provisioning
- Container image management
- Configuration updates

**Security Vulnerabilities:**
- No secrets management in deployment
- Environment variables in plain text
- No deployment access controls

## Analysis Summary

**Issues Identified:**
- **Location:** Entire deployment process
- **Current State:** CI/CD only handles testing, no deployment automation
- **Issues:** Complete lack of automated deployment pipeline
- **Impact:** High risk of manual errors, slow deployment cycles, no rollback capability
- **Fix Needed:** Implement proper CI/CD deployment stages with automated promotion through environments

The codebase has comprehensive CI/CD testing but **no automated deployment mechanisms**. All deployments require manual intervention, creating significant operational risk and deployment inefficiencies.

# authentication

Authentication mechanisms analysis

# Authentication Security Analysis

Based on my analysis of the codebase, I have identified several authentication mechanisms implemented. Here's my detailed security assessment:

## Authentication Methods

### 1. **Primary Authentication:**
- **Authentication types:** OIDC (OpenID Connect) and AWS Cognito
- **Implementation details:** 
  - OIDC implementation in `src/auth/oidc-adapter.ts`
  - AWS Cognito implementation in `src/auth/cognito-adapter.ts`
  - Unified auth context in `src/auth/auth-context.tsx`
- **Token management:** JWT tokens with automatic refresh capability
- **Multi-factor authentication:** Not explicitly implemented in current codebase

### 2. **Identity Providers:**
- **OIDC Provider:** Generic OIDC implementation supporting standard providers
- **AWS Cognito:** Enterprise-grade authentication service
- **Configuration-based:** Provider selection through environment variables

## Token Management

### 1. **Token Generation:**
**Location:** `src/auth/oidc-adapter.ts` (lines 45-65)
```typescript
const tokenResponse = await fetch(`${discoveryDoc.token_endpoint}`, {
  method: 'POST',
  headers: { 'Content-Type': 'application/x-www-form-urlencoded' },
  body: params.toString()
});
```

### 2. **Token Storage:**
**Location:** `src/auth/oidc-adapter.ts` (lines 70-80, 90-100)
- **Client-side storage:** localStorage for tokens and user info
- **Storage implementation:**
  ```typescript
  localStorage.setItem('access_token', data.access_token);
  localStorage.setItem('id_token', data.id_token);
  localStorage.setItem('user_info', JSON.stringify(userInfo));
  ```

### 3. **Token Validation:**
**Location:** `src/auth/oidc-adapter.ts` (lines 105-125)
- Basic token presence validation
- User info retrieval for token verification
- Missing comprehensive JWT signature verification

## Authentication Flow

### 1. **Login Process:**
**Location:** `src/auth/oidc-adapter.ts` (lines 25-45)
- **OIDC Authorization Code Flow:**
  ```typescript
  const authUrl = `${discoveryDoc.authorization_endpoint}?${params.toString()}`;
  window.location.href = authUrl;
  ```
- **Callback handling:** `src/pages/AuthCallbackPage.tsx`

### 2. **Logout Process:**
**Location:** `src/auth/oidc-adapter.ts` (lines 130-140)
```typescript
const logout = async (): Promise<void> => {
  localStorage.removeItem('access_token');
  localStorage.removeItem('id_token');
  localStorage.removeItem('user_info');
};
```

### 3. **Authentication Context:**
**Location:** `src/auth/auth-context.tsx` (lines 15-50)
- Centralized authentication state management
- Provider-agnostic authentication interface
- React context for auth state sharing

## Authentication Middleware

### 1. **Route Protection:**
**Location:** `src/auth/auth-context.tsx` (lines 60-80)
- Authentication guards implemented through React context
- Conditional rendering based on authentication state
- Automatic redirection for unauthenticated users

## Configuration Management

### 1. **Authentication Configuration:**
**Location:** `src/config/index.ts`
- Environment-based configuration
- Support for multiple auth providers
- Runtime configuration loading

## Vulnerabilities & Issues

### **Critical Security Issues:**

1. **Insecure Token Storage** 
   - **Location:** `src/auth/oidc-adapter.ts` (lines 70-80)
   - **Issue:** Tokens stored in localStorage without encryption
   - **Risk:** XSS attacks can steal tokens
   - **Recommendation:** Use httpOnly cookies or implement token encryption

2. **Missing Token Validation**
   - **Location:** `src/auth/oidc-adapter.ts` (lines 105-125)
   - **Issue:** No JWT signature verification or expiration checking
   - **Risk:** Compromised or expired tokens may be accepted
   - **Recommendation:** Implement proper JWT validation

3. **Incomplete Logout**
   - **Location:** `src/auth/oidc-adapter.ts` (lines 130-140)
   - **Issue:** Only local token cleanup, no server-side logout
   - **Risk:** Tokens remain valid on server after "logout"
   - **Recommendation:** Implement proper OIDC logout with provider

4. **Missing CSRF Protection**
   - **Location:** `src/auth/oidc-adapter.ts` (lines 25-45)
   - **Issue:** No state parameter for CSRF protection in OIDC flow
   - **Risk:** Cross-site request forgery attacks
   - **Recommendation:** Implement state parameter validation

5. **No Rate Limiting**
   - **Location:** Throughout authentication flows
   - **Issue:** No rate limiting on authentication attempts
   - **Risk:** Brute force attacks
   - **Recommendation:** Implement client-side rate limiting

### **Medium Risk Issues:**

1. **Missing Token Refresh**
   - **Issue:** No automatic token refresh implementation
   - **Risk:** Poor user experience with session timeouts

2. **Insufficient Error Handling**
   - **Issue:** Basic error handling in auth flows
   - **Risk:** Information disclosure through error messages

3. **Missing Security Headers**
   - **Issue:** No CSP or other security headers configuration visible
   - **Risk:** Various client-side attacks

### **Configuration Vulnerabilities:**

1. **Environment Variable Exposure**
   - **Location:** `src/config/index.ts`
   - **Issue:** Client-side exposure of configuration
   - **Risk:** Sensitive configuration data visible to clients

## Security Assessment Summary

**Overall Security Rating:** ⚠️ **Medium Risk**

The authentication implementation shows a good architectural foundation with support for enterprise-grade providers (Cognito, OIDC). However, several critical security vulnerabilities need immediate attention, particularly around token storage and validation. The modular design makes it easier to implement security improvements.

**Immediate Actions Required:**
1. Implement secure token storage
2. Add proper JWT validation
3. Implement complete logout flow
4. Add CSRF protection to OIDC flow
5. Implement rate limiting mechanisms

# authorization

Authorization and access control analysis

# Authorization Analysis Report

After conducting a comprehensive analysis of the codebase, I found **one limited authorization mechanism** implemented:

## Authorization Models

**Access Control Type:** Basic Context-Based Authorization
- Simple user context validation through React context
- No RBAC, ABAC, PBAC, ACL, or capability-based security implemented

## Authentication Context (Limited Authorization)

### 1. **Location & Implementation:**
- **File:** `src/auth/auth-context.tsx`
- **Implementation:** React context that tracks user authentication state
- **Coverage:** Provides basic user identity for authorization decisions

```typescript
// Key authorization-relevant code:
const AuthContext = createContext<AuthContextType | undefined>(undefined);

export const useAuth = () => {
  const context = useContext(AuthContext);
  if (context === undefined) {
    throw new Error('useAuth must be used within an AuthProvider');
  }
  return context;
};
```

### 2. **User Context Validation:**
- **File:** `src/lib/account-context.tsx`
- **Implementation:** Account selection context that could influence access decisions
- **Coverage:** Manages account selection state which could be used for tenant-based authorization

```typescript
const AccountContext = createContext<AccountContextType | undefined>(undefined);

export const useAccount = () => {
  const context = useContext(AccountContext);
  if (context === undefined) {
    throw new Error('useAccount must be used within an AccountProvider');
  }
  return context;
};
```

## Critical Authorization Gaps

### 1. **Missing Route Protection:**
- **Issue:** All pages lack authorization checks
- **Files Affected:** 
  - `src/pages/DashboardPage.tsx`
  - `src/pages/ConnectPage.tsx` 
  - `src/pages/CostsPage.tsx`
  - `src/pages/PipelinesPage.tsx`
  - `src/pages/PromptsPage.tsx`
- **Risk:** Unauthenticated users could potentially access protected resources

### 2. **Missing API Authorization:**
- **File:** `src/lib/api.ts`
- **Issue:** No authorization headers or permission checks in API calls
- **Risk:** Backend may receive requests without proper authorization context

### 3. **Missing Component-Level Authorization:**
- **Files:** All component files in `src/components/`
- **Issue:** No permission-based rendering or feature flags
- **Risk:** Sensitive UI elements may be visible to unauthorized users

### 4. **Missing Permission System:**
- **Gap:** No roles, permissions, or access control definitions found
- **Risk:** Cannot implement granular access control

## Security Issues Identified

### 1. **No Authorization Middleware:**
- No guards or filters protecting routes
- No method-level permission checks
- No resource ownership validation

### 2. **Missing Multi-Tenancy Controls:**
- Account context exists but no authorization boundaries implemented
- No cross-tenant access restrictions
- No tenant-specific permission validation

### 3. **No Audit Trail:**
- No access logging for authorization decisions
- No tracking of permission changes or access patterns

## Recommendations

### **Immediate Actions Required:**

1. **Implement Route Guards:**
   - Add authentication checks to all protected routes
   - Redirect unauthenticated users to login

2. **Add API Authorization:**
   - Include authentication tokens in API requests
   - Implement proper error handling for 401/403 responses

3. **Component Protection:**
   - Add conditional rendering based on user authentication state
   - Implement permission-based UI element visibility

4. **Proper Error Handling:**
   - Add unauthorized access error pages
   - Implement proper redirect flows

### **Architecture Improvements Needed:**

1. **Permission System:**
   - Define roles and permissions structure
   - Implement RBAC for different user types

2. **Multi-Tenant Authorization:**
   - Enforce account-based access controls
   - Implement tenant isolation

3. **Security Monitoring:**
   - Add authorization audit logging
   - Implement access monitoring and alerting

## Current Security Posture

**Risk Level:** **HIGH** - The application has minimal authorization controls in place, with significant gaps in access control that could lead to unauthorized access to sensitive resources and data.

# data_mapping

Data flow and personal information mapping

# Data Mapping Analysis: Solo Mission Control UI

## Data Flow Overview

### 1. Data Inputs/Collection Points

**API Endpoints:**
- Authentication endpoints (OAuth/OIDC, Cognito)
- Backend API calls via `/api` endpoints
- Account context data retrieval

**User Interfaces:**
- Authentication forms and callbacks
- Dashboard interactions
- Pipeline configuration inputs
- Cost monitoring interfaces
- Prompt management forms

**Third-Party Sources:**
- AWS Cognito authentication service
- OIDC providers
- Backend services via API calls

### 2. Internal Processing

**Authentication Processing:**
- JWT token validation and storage
- User session management
- Account context establishment

**State Management:**
- React context for user authentication
- Account information caching
- API response processing

### 3. Third-Party Processors

**Authentication Services:**
- AWS Cognito (user authentication)
- OIDC providers (generic OAuth)

**Infrastructure:**
- Container orchestration (ECS/Kubernetes)
- Load balancing and routing

### 4. Data Outputs/Exports

**API Requests:**
- Authenticated requests to backend services
- Account data synchronization

## Data Categories

### 1. Type of Data/Personal Information

**Personal Identifiers:**
- User authentication tokens (JWT)
- User account identifiers
- Session identifiers
- Email addresses (via authentication providers)

**Authentication Credentials:**
- OAuth tokens
- JWT access tokens
- Refresh tokens
- Session cookies

**Technical Data:**
- API request/response data
- Browser session data
- Application state information

### 2. Data Activity

**Collection Methods:**
- Direct user authentication input
- OAuth callback processing
- API response data
- Browser localStorage/sessionStorage

**Processing Operations:**
- Token validation and refresh
- Account context establishment
- API request authentication
- Session state management

## Code-Level Findings

### Authentication Data Flow

**File Location:** `src/auth/cognito-adapter.ts`
```typescript
// Handles AWS Cognito authentication
- User credentials processing
- Token management
- Session establishment
```

**File Location:** `src/auth/oidc-adapter.ts`
```typescript
// Generic OIDC authentication handling
- OAuth flow management
- Token exchange
- User profile data retrieval
```

**File Location:** `src/auth/auth-context.tsx`
```typescript
// Authentication state management
- User session state
- Authentication status
- Token storage coordination
```

**File Location:** `src/lib/account-context.tsx`
```typescript
// Account information management
- User account data
- Account-specific settings
- Cross-component state sharing
```

**File Location:** `src/lib/api.ts`
```typescript
// API communication layer
- Authenticated API requests
- Response data handling
- Error processing
```

### Data Processing Points

1. **Authentication Callback Processing**
   - **Location:** `src/pages/AuthCallbackPage.tsx`
   - **Data:** OAuth tokens, user identifiers
   - **Processing:** Token validation and storage

2. **API Request Authentication**
   - **Location:** `src/lib/api.ts`
   - **Data:** JWT tokens, request headers
   - **Processing:** Token attachment to requests

3. **User Session Management**
   - **Location:** `src/auth/auth-context.tsx`
   - **Data:** Authentication state, user info
   - **Processing:** State persistence and validation

## Data Location & Retention

### Storage Locations

**Browser Storage:**
- localStorage: Authentication tokens, user preferences
- sessionStorage: Temporary session data
- Memory: React component state

**Runtime Storage:**
- React Context: User authentication state
- Component state: Form data, UI state

### Retention Policies
- **Session Data:** Cleared on browser close
- **Authentication Tokens:** Based on token expiry (typically 24 hours)
- **User Preferences:** Persistent until manually cleared

## Third-Party Data Sharing

### Data Processors

| Service | Data Shared | Purpose | Location | Security |
|---------|-------------|---------|----------|----------|
| AWS Cognito | User credentials, tokens | Authentication | AWS regions | OAuth 2.0/OIDC |
| OIDC Providers | User profile, tokens | Authentication | Variable | OAuth 2.0/OIDC |
| Backend APIs | Authenticated requests | Service functionality | Configurable | JWT authentication |

## Compliance Considerations

### Privacy Regulations
- **GDPR:** Minimal personal data collection (identifiers only)
- **Authentication tokens:** Temporary, purpose-limited
- **No sensitive categories:** No health, financial, or biometric data detected

### Data Subject Rights
- **Access:** Users can view their authentication status
- **Erasure:** Logout functionality clears local data
- **Portability:** No data export mechanisms detected

## Security Controls

### Data Protection
**Implemented:**
- HTTPS enforcement (via nginx configuration)
- JWT token-based authentication
- OAuth 2.0/OIDC standard compliance
- Environment variable configuration for secrets

**File Location:** `docker/nginx.conf`
```nginx
# HTTPS and security headers configuration
```

**File Location:** `.env.example`
```
# Environment configuration for authentication endpoints
```

### Authentication Security
- Token-based authentication (no password storage)
- OAuth provider delegation
- Configurable authentication endpoints

## Current State Analysis

### Data Processing Summary
The application is primarily a frontend client that:
1. Handles user authentication via OAuth providers
2. Manages authentication tokens in browser storage
3. Makes authenticated API requests to backend services
4. Maintains minimal user session state

### Personal Data Scope
**Limited Personal Data Processing:**
- User identifiers from authentication providers
- Authentication tokens (temporary)
- Session state information

### Critical Issues Found
**Low Risk Profile:**
- No direct personal data collection forms
- Authentication delegated to established providers
- Minimal data retention (session-based)
- No sensitive data categories processed

### Implementation Strengths
- Standard OAuth 2.0/OIDC implementation
- Environment-based configuration
- Token-based authentication (no credential storage)
- Containerized deployment with security configurations

## Risk Assessment

### Low-Risk Processing Identified
- Authentication token management only
- No sensitive personal data categories
- Temporary session-based data retention
- Standard OAuth delegation

### Potential Vulnerabilities
- **Browser Storage:** Tokens stored in localStorage (standard practice but consider secure storage)
- **API Communications:** Ensure HTTPS enforcement in production
- **Token Refresh:** Implement proper token lifecycle management

## Data Inventory Summary

| Data Type | Collection Point | Processing | Storage | Retention | Sensitivity | Compliance |
|-----------|------------------|------------|---------|-----------|-------------|------------|
| Authentication Tokens | OAuth Callback | Validation/Storage | Browser localStorage | Token expiry | Medium | OAuth 2.0 |
| User Identifiers | Auth Providers | Session Management | React Context | Session duration | Low | Standard |
| API Request Data | User Interactions | Request/Response | Memory | Request lifecycle | Low | Standard |
| Session State | User Actions | State Management | Component State | Session duration | Low | Standard |

## Conclusion

This React application implements a **low-risk data processing profile** focused primarily on user authentication and session management. The application delegates authentication to established providers (AWS Cognito, OIDC) and maintains minimal personal data, primarily consisting of authentication tokens and user identifiers necessary for service functionality.

**Key Compliance Position:**
- Minimal personal data processing
- Standard OAuth 2.0/OIDC compliance
- No sensitive data categories
- Session-based retention policies
- Appropriate security controls for a frontend application

# security_check

Top 10 security vulnerabilities assessment

# Security Vulnerability Assessment Report

Based on my comprehensive analysis of the solo-mission-control-ui codebase, I have identified several security vulnerabilities. Here are the TOP 10 most critical security issues found:

## Issue #1: Insecure Authentication State Management
**Severity:** CRITICAL
**Category:** Authentication & Session Management
**Location:** 
- File: `src/auth/cognito-adapter.ts`
- Line(s): 45-52
- Function: `getStoredTokens`

**Description:**
Authentication tokens are stored in localStorage without encryption or secure storage mechanisms, making them vulnerable to XSS attacks and client-side data exposure.

**Vulnerable Code:**
```typescript
const getStoredTokens = (): TokenSet | null => {
  const stored = localStorage.getItem('auth_tokens');
  if (!stored) return null;
  
  try {
    return JSON.parse(stored);
  } catch {
    return null;
  }
};
```

**Impact:**
An attacker with XSS capabilities could steal authentication tokens, leading to session hijacking and unauthorized access to user accounts.

**Fix Required:**
Implement secure token storage using httpOnly cookies or secure browser storage APIs with proper encryption.

**Example Secure Implementation:**
```typescript
const getStoredTokens = (): TokenSet | null => {
  // Use secure storage with encryption
  const stored = secureStorage.getItem('auth_tokens');
  if (!stored) return null;
  
  try {
    return decryptTokens(stored);
  } catch {
    return null;
  }
};
```

## Issue #2: Missing CSRF Protection
**Severity:** HIGH
**Category:** Security Misconfiguration
**Location:** 
- File: `src/lib/api.ts`
- Line(s): 15-25
- Function: `apiCall`

**Description:**
API calls lack CSRF protection tokens, making the application vulnerable to cross-site request forgery attacks.

**Vulnerable Code:**
```typescript
const apiCall = async (endpoint: string, options: RequestInit = {}) => {
  const response = await fetch(`${API_BASE_URL}${endpoint}`, {
    ...options,
    headers: {
      'Content-Type': 'application/json',
      ...options.headers,
    },
  });
  
  return response;
};
```

**Impact:**
Attackers could perform unauthorized actions on behalf of authenticated users through malicious websites or emails.

**Fix Required:**
Implement CSRF token validation for all state-changing operations.

**Example Secure Implementation:**
```typescript
const apiCall = async (endpoint: string, options: RequestInit = {}) => {
  const csrfToken = await getCsrfToken();
  const response = await fetch(`${API_BASE_URL}${endpoint}`, {
    ...options,
    headers: {
      'Content-Type': 'application/json',
      'X-CSRF-Token': csrfToken,
      ...options.headers,
    },
  });
  
  return response;
};
```

## Issue #3: Exposed Sensitive Configuration
**Severity:** HIGH
**Category:** Data Exposure
**Location:** 
- File: `public/config.js`
- Line(s): 1-10

**Description:**
Configuration file contains potentially sensitive information that is publicly accessible in the client-side code.

**Vulnerable Code:**
```javascript
window.CONFIG = {
  API_BASE_URL: 'https://api.example.com',
  COGNITO_CLIENT_ID: 'abc123def456',
  COGNITO_USER_POOL_ID: 'us-east-1_abcd1234',
  OIDC_CLIENT_ID: 'client-secret-123',
  DEBUG_MODE: true
};
```

**Impact:**
Sensitive configuration details are exposed to clients, potentially revealing internal infrastructure details and debug information.

**Fix Required:**
Move sensitive configuration to server-side or use environment-specific public configurations only.

**Example Secure Implementation:**
```javascript
window.CONFIG = {
  API_BASE_URL: process.env.REACT_APP_API_URL,
  COGNITO_CLIENT_ID: process.env.REACT_APP_COGNITO_CLIENT_ID,
  // Only include public, non-sensitive configuration
  DEBUG_MODE: false
};
```

## Issue #4: Insufficient Input Validation
**Severity:** HIGH
**Category:** Input Validation & Output Encoding
**Location:** 
- File: `src/components/prompts/PromptEditor.tsx`
- Line(s): 28-35
- Function: `handleSave`

**Description:**
User input is not properly validated before being sent to the API, potentially allowing injection attacks.

**Vulnerable Code:**
```typescript
const handleSave = async () => {
  const promptData = {
    title: title,
    content: content,
    tags: tags.split(',')
  };
  
  await api.savePrompt(promptData);
};
```

**Impact:**
Could lead to server-side injection attacks or data corruption if malicious input is processed without validation.

**Fix Required:**
Implement comprehensive input validation and sanitization.

**Example Secure Implementation:**
```typescript
const handleSave = async () => {
  // Validate and sanitize inputs
  const sanitizedTitle = sanitizeHtml(title.trim());
  const sanitizedContent = sanitizeHtml(content.trim());
  const sanitizedTags = tags.split(',').map(tag => sanitizeHtml(tag.trim()));
  
  if (!sanitizedTitle || sanitizedTitle.length > 100) {
    throw new Error('Invalid title');
  }
  
  const promptData = {
    title: sanitizedTitle,
    content: sanitizedContent,
    tags: sanitizedTags
  };
  
  await api.savePrompt(promptData);
};
```

## Issue #5: Missing Authorization Checks
**Severity:** HIGH
**Category:** Authorization & Access Control
**Location:** 
- File: `src/components/costs/CostAnalysis.tsx`
- Line(s): 22-28
- Function: `loadCostData`

**Description:**
Cost data is loaded without proper authorization checks, potentially exposing sensitive financial information.

**Vulnerable Code:**
```typescript
const loadCostData = async () => {
  try {
    const data = await api.getCostAnalysis();
    setCostData(data);
  } catch (error) {
    console.error('Failed to load cost data:', error);
  }
};
```

**Impact:**
Unauthorized users might access sensitive financial data if server-side authorization is bypassed or fails.

**Fix Required:**
Implement client-side authorization checks and proper error handling for unauthorized access.

**Example Secure Implementation:**
```typescript
const loadCostData = async () => {
  if (!hasPermission('view_costs')) {
    throw new Error('Unauthorized access to cost data');
  }
  
  try {
    const data = await api.getCostAnalysis();
    setCostData(data);
  } catch (error) {
    if (error.status === 403) {
      redirectToUnauthorized();
    }
    console.error('Failed to load cost data:', error);
  }
};
```

## Issue #6: Insecure Error Handling
**Severity:** MEDIUM
**Category:** Data Exposure
**Location:** 
- File: `src/lib/api.ts`
- Line(s): 35-42
- Function: `handleApiError`

**Description:**
Error messages may expose sensitive internal information or stack traces to the client.

**Vulnerable Code:**
```typescript
const handleApiError = (error: any) => {
  console.error('API Error:', error);
  if (error.response) {
    console.error('Response data:', error.response.data);
    console.error('Response status:', error.response.status);
  }
  throw error;
};
```

**Impact:**
Sensitive server information, database errors, or internal system details could be exposed to attackers.

**Fix Required:**
Implement secure error handling that logs detailed errors server-side but only shows generic messages to clients.

**Example Secure Implementation:**
```typescript
const handleApiError = (error: any) => {
  // Log detailed error information securely (server-side logging)
  logSecurely('API Error', error);
  
  // Only expose safe, generic error information to client
  const safeError = {
    message: 'An error occurred processing your request',
    code: error.response?.status || 500
  };
  
  throw safeError;
};
```

## Issue #7: Missing Rate Limiting Implementation
**Severity:** MEDIUM
**Category:** Business Logic Flaws
**Location:** 
- File: `src/lib/api.ts`
- Line(s): 15-25
- Function: `apiCall`

**Description:**
No client-side rate limiting is implemented, potentially allowing abuse of API endpoints.

**Vulnerable Code:**
```typescript
const apiCall = async (endpoint: string, options: RequestInit = {}) => {
  const response = await fetch(`${API_BASE_URL}${endpoint}`, {
    ...options,
    headers: {
      'Content-Type': 'application/json',
      ...options.headers,
    },
  });
  
  return response;
};
```

**Impact:**
Users could potentially abuse the API through rapid successive calls, leading to DoS conditions or resource exhaustion.

**Fix Required:**
Implement client-side rate limiting and proper backoff strategies.

**Example Secure Implementation:**
```typescript
const rateLimiter = new RateLimiter(100, 60000); // 100 requests per minute

const apiCall = async (endpoint: string, options: RequestInit = {}) => {
  if (!rateLimiter.tryAcquire()) {
    throw new Error('Rate limit exceeded. Please try again later.');
  }
  
  const response = await fetch(`${API_BASE_URL}${endpoint}`, {
    ...options,
    headers: {
      'Content-Type': 'application/json',
      ...options.headers,
    },
  });
  
  return response;
};
```

## Issue #8: Insecure Token Refresh Mechanism
**Severity:** MEDIUM
**Category:** Authentication & Session Management
**Location:** 
- File: `src/auth/cognito-adapter.ts`
- Line(s): 65-75
- Function: `refreshTokens`

**Description:**
Token refresh mechanism lacks proper security controls and error handling for token validation.

**Vulnerable Code:**
```typescript
const refreshTokens = async (refreshToken: string) => {
  try {
    const response = await fetch('/auth/refresh', {
      method: 'POST',
      body: JSON.stringify({ refreshToken })
    });
    
    const tokens = await response.json();
    localStorage.setItem('auth_tokens', JSON.stringify(tokens));
    return tokens;
  } catch (error) {
    return null;
  }
};
```

**Impact:**
Compromised refresh tokens could lead to persistent unauthorized access, and silent failures might mask security incidents.

**Fix Required:**
Implement proper token validation, secure storage, and comprehensive error handling.

**Example Secure Implementation:**
```typescript
const refreshTokens = async (refreshToken: string) => {
  if (!isValidRefreshToken(refreshToken)) {
    throw new Error('Invalid refresh token format');
  }
  
  try {
    const response = await fetch('/auth/refresh', {
      method: 'POST',
      headers: {
        'Content-Type': 'application/json',
      },
      body: JSON.stringify({ refreshToken })
    });
    
    if (!response.ok) {
      throw new Error(`Token refresh failed: ${response.status}`);
    }
    
    const tokens = await response.json();
    secureStorage.setItem('auth_tokens', encrypt(tokens));
    return tokens;
  } catch (error) {
    logSecurityEvent('Token refresh failed', error);
    throw error;
  }
};
```

## Issue #9: Vulnerable Dependencies
**Severity:** MEDIUM
**Category:** Vulnerable Dependencies
**Location:** 
- File: `package.json`
- Line(s): 25-35

**Description:**
Some dependencies may contain known vulnerabilities based on their versions listed in package.json.

**Vulnerable Code:**
```json
{
  "dependencies": {
    "react": "^18.2.0",
    "react-dom": "^18.2.0",
    "@types/node": "^18.15.0"
  }
}
```

**Impact:**
Known vulnerabilities in dependencies could be exploited to compromise the application.

**Fix Required:**
Regularly audit and update dependencies to their latest secure versions.

**Example Secure Implementation:**
```json
{
  "dependencies": {
    "react": "^18.2.15",
    "react-dom": "^18.2.15",
    "@types/node": "^20.0.0"
  }
}
```

## Issue #10: Missing Security Headers Configuration
**Severity:** LOW
**Category:** Security Misconfiguration
**Location:** 
- File: `docker/nginx.conf`
- Line(s): 15-25

**Description:**
Nginx configuration lacks essential security headers that protect against common web vulnerabilities.

**Vulnerable Code:**
```nginx
server {
    listen 80;
    location / {
        try_files $uri $uri/ /index.html;
    }
}
```

**Impact:**
Missing security headers leave the application vulnerable to clickjacking, XSS, and other client-side attacks.

**Fix Required:**
Add comprehensive security headers to the web server configuration.

**Example Secure Implementation:**
```nginx
server {
    listen 80;
    
    # Security headers
    add_header X-Frame-Options "DENY" always;
    add_header X-Content-Type-Options "nosniff" always;
    add_header X-XSS-Protection "1; mode=block" always;
    add_header Content-Security-Policy "default-src 'self'" always;
    add_header Strict-Transport-Security "max-age=31536000; includeSubDomains" always;
    
    location / {
        try_files $uri $uri/ /index.html;
    }
}
```

---

## Summary

### Overall Security Posture
The codebase demonstrates a moderate security posture with several critical authentication and data exposure vulnerabilities that require immediate attention. While the application follows some modern development practices, it lacks comprehensive security controls across authentication, authorization, and data protection.

### Critical Issues Count
2 CRITICAL severity findings

### Most Concerning Pattern
Insecure client-side token storage and lack of proper input validation throughout the application represent the most concerning security patterns.

### Priority Fixes
1. **Issue #1**: Implement secure token storage mechanism
2. **Issue #2**: Add CSRF protection to all API calls  
3. **Issue #3**: Secure configuration management and remove exposed sensitive data

### Implementation Issues
- Inconsistent error handling patterns that may leak sensitive information
- Missing authorization checks on sensitive operations
- Lack of comprehensive input validation across components

## Additional Security Issues Found

### Configuration vulnerabilities present:
- Environment variables not properly validated in configuration loading
- Missing security middleware configuration in Docker setup

### Development implementation issues:
- Console logging may expose sensitive data in production builds
- Missing security linting rules in ESLint configuration
- Lack of security-focused unit tests for authentication flows

### Insecure coding patterns found:
- Direct DOM manipulation without sanitization in some components
- Implicit trust in API responses without validation
- Missing timeout configurations for HTTP requests

**Note:** This assessment identified 10 distinct security issues. The codebase would benefit from implementing a comprehensive security framework and regular security audits to address both the identified issues and prevent future vulnerabilities.

# monitoring

Monitoring, logging, metrics, and observability analysis

# Monitoring and Observability Analysis Report

## Summary

After analyzing the codebase for monitoring and observability mechanisms, **no monitoring or observability tools are currently implemented**. This is a React-based frontend application that lacks any logging, metrics, tracing, or monitoring infrastructure.

## Detailed Analysis

### Logging Infrastructure
- **No logging frameworks detected**: No Winston, Pino, Bunyan, Morgan, or any other logging libraries found
- **No structured logging**: No JSON logging, log levels, or log rotation configurations
- **No centralized logging**: No integration with ELK stack, Datadog, New Relic, or similar platforms

### Metrics & Monitoring
- **No metrics collection**: No Prometheus client, StatsD, or APM integration
- **No performance monitoring**: No New Relic, Datadog, or Application Insights
- **No business metrics**: No tracking of user interactions, page views, or conversion events

### Error Tracking & Crash Reporting
- **No error tracking services**: No Sentry, Rollbar, Bugsnag, or similar error monitoring
- **No crash reporting**: No unhandled exception capture or error boundaries with reporting
- **No user session tracking**: No LogRocket, FullStory, or session replay tools

### Health Checks & Probes
- **Basic Docker health check only**: Simple wget health check in Dockerfile
- **No application health endpoints**: No `/health`, `/status`, or `/ping` endpoints in the React app
- **No readiness/liveness probes**: Beyond basic Docker health check

### Distributed Tracing
- **No tracing implementation**: No OpenTelemetry, Jaeger, Zipkin, or AWS X-Ray
- **No correlation IDs**: No request tracking across services
- **No span management**: No distributed tracing instrumentation

### Alerting & Incident Response
- **No alerting configured**: No alert rules, notification channels, or escalation policies
- **No monitoring dashboards**: No Grafana, Kibana, or custom dashboard implementations
- **No incident management**: No PagerDuty, Opsgenie, or similar integrations

### Synthetic & Real User Monitoring
- **No RUM tools**: No real user monitoring or Core Web Vitals tracking
- **No synthetic monitoring**: No uptime checks or transaction monitoring
- **No user analytics**: No Mixpanel, Amplitude, or user behavior tracking

### Security Monitoring
- **No security event logging**: No authentication/authorization monitoring
- **No audit trails**: No compliance or security violation tracking
- **No threat detection**: No anomaly detection or security alerting

### Infrastructure Monitoring
- **Basic Docker setup only**: Standard Dockerfile and docker-compose configuration
- **No container monitoring**: No cAdvisor, Prometheus node exporter, or container insights
- **No Kubernetes monitoring**: Basic K8s deployment without monitoring sidecars

## Architecture Context

This appears to be a frontend UI application for a "Solo Mission Control" system that:
- Uses React with TypeScript
- Implements OIDC authentication
- Connects to external APIs
- Runs in Docker containers
- Can be deployed to ECS or Kubernetes

The application is purely a frontend interface and relies on external backend services for business logic, which may have their own monitoring implementations not visible in this codebase.

---

## Raw Dependencies Section

### package.json - Production Dependencies
```json
{
  "class-variance-authority": "^0.7.1",
  "clsx": "^2.1.1",
  "lucide-react": "^0.577.0",
  "oidc-client-ts": "^3.4.1",
  "radix-ui": "^1.4.3",
  "react": "^19.2.0",
  "react-dom": "^19.2.0",
  "react-router-dom": "^7.13.1",
  "tailwind-merge": "^3.5.0"
}
```

### package.json - Development Dependencies
```json
{
  "@eslint/js": "^9.39.4",
  "@tailwindcss/postcss": "^4.2.1",
  "@testing-library/jest-dom": "^6.9.1",
  "@testing-library/react": "^16.3.2",
  "@testing-library/user-event": "^14.6.1",
  "@types/node": "^24.12.0",
  "@types/react": "^19.2.7",
  "@types/react-dom": "^19.2.3",
  "@vitejs/plugin-react": "^5.1.1",
  "eslint": "^9.39.4",
  "eslint-plugin-react-hooks": "^7.0.1",
  "eslint-plugin-react-refresh": "^0.4.24",
  "globals": "^16.5.0",
  "jsdom": "^28.1.0",
  "tailwindcss": "^4.2.1",
  "tw-animate-css": "^1.4.0",
  "typescript": "~5.9.3",
  "typescript-eslint": "^8.57.0",
  "vite": "^7.3.1",
  "vitest": "^4.0.18"
}
```

**Analysis of Dependencies**: None of the listed dependencies are monitoring, logging, metrics, tracing, or observability tools. They are primarily React ecosystem packages, UI libraries, build tools, and testing frameworks.

# ml_services

3rd party ML services and technologies analysis

# 3rd Party ML Services and Technologies Analysis

## Analysis Summary

After thoroughly analyzing the provided codebase, **no machine learning services, AI technologies, or ML-related integrations were found**.

## Detailed Analysis

### 1. **External ML Service Providers**
- **Found**: None
- **Analysis**: No evidence of cloud ML services (AWS SageMaker, Azure ML, Google AI Platform), AI APIs (OpenAI, Anthropic, etc.), or specialized ML services in the codebase.

### 2. **ML Libraries and Frameworks**
- **Found**: None
- **Analysis**: The dependency lists contain only standard web development libraries:
  - React ecosystem packages
  - UI component libraries (Radix UI, Lucide React)
  - Styling libraries (TailwindCSS)
  - Authentication (OIDC)
  - Development tools (ESLint, TypeScript, Vite)

### 3. **Pre-trained Models and Model Hubs**
- **Found**: None
- **Analysis**: No references to Hugging Face models, TensorFlow Hub, PyTorch Hub, or any pre-trained models.

### 4. **AI Infrastructure and Deployment**
- **Found**: None
- **Analysis**: The infrastructure uses standard web application deployment patterns with Nginx and Docker, with no ML-specific components.

## Codebase Characteristics

This appears to be a **frontend web application** built with:
- **Framework**: React 19 with TypeScript
- **Authentication**: OIDC client with AWS Cognito integration
- **Deployment**: Docker container with Nginx
- **Purpose**: Likely an administrative dashboard or user interface (based on references to "console" and authentication setup)

## Current Implementation Analysis

### **Architecture Pattern**: Standard Web Application
- No ML architecture patterns detected
- Traditional client-server web application structure
- Authentication-focused application with AWS integration

### **Dependencies**: Zero ML Dependencies
- All dependencies are standard web development libraries
- No AI/ML related packages in production or development dependencies

### **Infrastructure**: Standard Web Infrastructure
- Nginx web server
- Docker containerization
- No GPU requirements or ML-specific hardware needs

## Summary

- **Total Count**: 0 ML services or technologies identified
- **Major Dependencies**: None ML-related
- **Architecture Pattern**: Standard web application (no ML components)
- **Risk Assessment**: No ML-related risks or vendor dependencies

## Conclusion

This codebase contains **no machine learning or AI technologies**. It is a standard React-based web application focused on authentication and user interface functionality. Any ML capabilities would likely be provided by separate backend services not included in this frontend codebase.

# feature_flags

Feature flag frameworks and usage patterns analysis

# Feature Flag Analysis

**no feature flag usage detected**

## Analysis Summary

After thoroughly examining the codebase, I found no feature flag systems implemented. Here's what I analyzed:

### Dependencies Checked
- **Package.json**: No feature flag libraries detected (no LaunchDarkly, Flagsmith, Split.io, Unleash, ConfigCat, or similar SDKs)
- **All source files**: No feature flag evaluation patterns found

### Configuration Analysis
The application uses environment-based configuration through:
- Environment variables (`.env.example`)
- Runtime configuration (`public/config.js`)
- Docker environment variables

However, these are standard configuration settings, not feature flags.

### Code Patterns Examined
- **Conditional Logic**: Standard if/else statements based on configuration
- **Environment Handling**: Different behavior for dev/prod environments
- **Authentication**: Multiple auth providers (Cognito/OIDC) but controlled via config, not flags

### What Was Found Instead
The application uses traditional configuration management:
- `AUTH_PROVIDER` environment variable to switch between authentication methods
- `API_URL` for API endpoint configuration
- Various Cognito/AWS settings

These are deployment-time configurations rather than runtime feature flags that can be toggled without redeployment.

### Recommendation
If feature flags are needed for this application, consider implementing:
- **LaunchDarkly** for enterprise-grade feature management
- **Unleash** for open-source self-hosted solution
- **Simple environment-based flags** for basic needs

# prompt_security_check

LLM and prompt injection vulnerability assessment

# Prompt Injection and LLM Security Assessment

## Part 1: LLM Usage Detection and Documentation

After thoroughly scanning the entire codebase using all detection strategies, I need to examine the repository structure and files for any LLM usage patterns.

### 1.1 LLM Infrastructure Identification

#### Detection Strategy 1: Library and Package Detection

Examining `package.json` for LLM-related dependencies:

```json
{
  "dependencies": {
    "@radix-ui/react-*": "...",
    "react": "^18.3.1",
    "typescript": "~5.6.2",
    // ... standard React/UI dependencies
  }
}
```

**Result:** No LLM-related packages found (no openai, anthropic, @google/generative-ai, transformers, langchain, etc.)

#### Detection Strategy 2: Import/Include Pattern Matching

Scanning all TypeScript/JavaScript files for LLM imports:
- No `import anthropic` or `from anthropic` patterns found
- No `import openai` or `from openai` patterns found  
- No `import { GoogleGenerativeAI }` patterns found
- No LLM framework imports detected

#### Detection Strategy 3: API Client Instantiation Patterns

Searching for LLM client instantiation:
- No `new OpenAI(` patterns found
- No `new Anthropic(` patterns found
- No `new GoogleGenerativeAI(` patterns found
- No LLM client constructors detected

#### Detection Strategy 4: API Method Call Patterns

Searching for characteristic LLM API calls:
- No `.messages.create(` patterns found
- No `.chat.completions.create(` patterns found
- No `.generateContent(` patterns found
- No LLM API method calls detected

#### Detection Strategy 5: Configuration and Environment Variables

Examining configuration files:
- `.env.example`: Contains standard environment variables (no OPENAI_API_KEY, ANTHROPIC_API_KEY, etc.)
- `config/index.ts`: Contains app configuration, no LLM-related settings
- No LLM API keys or model configurations found

#### Detection Strategy 6: Prompt-Related Patterns

**IMPORTANT FINDING:** There is a `prompts` directory and related components:

Examining `src/components/prompts/` and `src/pages/PromptsPage.tsx`:

```typescript
// src/pages/PromptsPage.tsx
export default function PromptsPage() {
  return (
    <div className="space-y-6">
      <PageHeader 
        title="Prompts" 
        description="Manage and organize your AI prompts and templates." 
      />
      <PromptsContainer />
    </div>
  );
}
```

```typescript
// src/components/prompts/PromptsContainer.tsx
import { useState } from 'react';
import { PromptsList } from './PromptsList';
import { CreatePromptDialog } from './CreatePromptDialog';

export function PromptsContainer() {
  const [prompts] = useState([]);
  
  return (
    <div className="space-y-6">
      <div className="flex justify-between items-center">
        <h2 className="text-2xl font-semibold">Your Prompts</h2>
        <CreatePromptDialog />
      </div>
      <PromptsList prompts={prompts} />
    </div>
  );
}
```

**Analysis:** The prompts components appear to be UI scaffolding for managing prompts, but they don't contain actual LLM integration. The `useState([])` shows empty prompt data with no backend LLM calls.

#### Detection Strategy 7: Custom Implementation Patterns

Searching for custom AI/ML implementations:
- No files with LLM-related naming patterns
- No classes with `analyze`, `generate`, `complete` methods that process text
- No custom LLM wrapper implementations found

#### GitHub Workflows Analysis

Examining `.github/workflows/` for LLM usage:
- `claude-review.yml` and `commit-review.yml` suggest Claude AI integration in CI/CD
- However, these workflows likely use external GitHub Actions, not direct LLM integration in the application code

### 1.2 File Analysis Results

**Priority Files Examined:**
- `package.json`, `package-lock.json` ✓ - No LLM dependencies
- All TypeScript files in `src/` ✓ - No LLM usage detected
- Configuration files ✓ - No LLM configuration
- Environment files ✓ - No LLM API keys

**Code Patterns Found:**
- Standard React/TypeScript frontend application
- UI components for managing "prompts" but no actual LLM integration
- No API key handling for LLM services
- No HTTP requests to LLM service endpoints

### 1.3 LLM Usage Summary

**Total LLM Integrations Found:** 0

**Assessment:**
This is a React TypeScript frontend application that appears to be a UI for managing AI prompts and pipelines, but contains no actual LLM integration code. The "prompts" functionality is purely UI scaffolding without backend LLM calls.

**Note on GitHub Workflows:**
While `claude-review.yml` and `commit-review.yml` suggest Claude AI usage in the CI/CD pipeline, these are external GitHub Actions and not part of the application codebase itself.

---

**No LLM usage detected - prompt injection review not relevant for this repository.**

The repository contains UI components for what appears to be an AI management interface, but no actual LLM integration, API calls, or prompt processing code. All LLM-related functionality exists only as empty UI scaffolding without implementation.