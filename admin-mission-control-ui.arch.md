# hl_overview

High level overview of the codebase

# Project Analysis: Admin Mission Control UI

## 0. **Repository Name:** 
[[admin-mission-control-ui]]

## 1. **Project Purpose:**
This project appears to be an administrative dashboard/control panel for managing cloud environments and infrastructure. Based on the component and page names, it provides a web interface for:
- Managing environments and applications
- Monitoring system status and budgets
- Handling API keys and authentication
- Managing pipelines and tasks
- Providing terminal access via SSM sessions
- Broadcasting to agents and monitoring signups

The primary domain is **DevOps/Cloud Infrastructure Management** with a focus on AWS services.

## 2. **Architecture Pattern:**
- **Single Page Application (SPA)** with React
- **Component-based architecture** with reusable UI components
- **Context-based state management** for cross-cutting concerns
- **Containerized deployment** with Docker and orchestration support

## 3. **Technology Stack:**

**Primary Languages & Frameworks:**
- **TypeScript/JavaScript** - Main programming language
- **React** - Frontend framework
- **Vite** - Build tool and development server

**Key Libraries & Dependencies:**
- **Tailwind CSS** - Utility-first CSS framework
- **PostCSS** - CSS processing
- **xterm** - Terminal emulator for web (evident from type definitions)
- **SSM Session support** - AWS Systems Manager integration

**Infrastructure & Deployment:**
- **Docker** - Containerization
- **Nginx** - Web server (in docker config)
- **Terraform** - Infrastructure as Code (ECS and K8s deployments)
- **AWS ECS & Kubernetes** - Container orchestration platforms

## 4. **Initial Structure Impression:**

This is a **frontend-only application** with the following main parts:
- **Frontend SPA** - React-based web application
- **Docker containerization** - For deployment packaging
- **Infrastructure definitions** - Terraform configurations for cloud deployment
- **CI/CD pipelines** - GitHub Actions workflows

## 5. **Configuration/Package Files:**
- `package.json` & `package-lock.json` - Node.js dependencies
- `tsconfig.json` - TypeScript configuration
- `vite.config.ts` - Vite build tool configuration
- `tailwind.config.ts` - Tailwind CSS configuration
- `postcss.config.js` - PostCSS configuration
- `components.json` - UI components configuration
- `Dockerfile` & `docker-compose.yml` - Container configuration
- `.env.example` - Environment variables template
- `terraform.tfvars.example` - Terraform variables template

## 6. **Directory Structure:**

**Source Code Organization (`src/`):**
- **`pages/`** - Route-level components (Dashboard, Apps, Environments, etc.)
- **`components/`** - Reusable UI components, including `ui/` subfolder for base components
- **`context/`** - React Context providers for global state (Favorites, SSM Sessions)
- **`auth/`** - Authentication system with adapters pattern
- **`api/`** - API client for backend communication
- **`types/`** - TypeScript type definitions
- **`lib/`** - Utility functions

**Infrastructure & Deployment:**
- **`terraform/`** - Infrastructure as Code (ECS and K8s)
- **`docker/`** - Docker-related scripts and configurations
- **`.github/workflows/`** - CI/CD pipeline definitions

## 7. **High-Level Architecture:**

**Primary Pattern: Layered Architecture with Feature-based Organization**

**Evidence:**
- **Presentation Layer**: React components organized by pages and reusable components
- **Context Layer**: React Context for state management (`FavoritesContext`, `SsmSessionContext`)
- **Service Layer**: API client and authentication adapters
- **Infrastructure Layer**: Containerization and cloud deployment configurations

**Additional Patterns:**
- **Adapter Pattern**: Authentication system with multiple adapters
- **Provider Pattern**: React Context providers for dependency injection
- **Component Composition**: UI components library with base components

## 8. **Build, Execution and Test:**

**Development:**
- **Entry Point**: `src/main.tsx` (typical React + Vite setup)
- **Development Server**: Likely `npm run dev` or `yarn dev` (Vite standard)
- **Local Startup**: `start-admin-mc-local.sh` script for local development

**Build Process:**
- **Build Tool**: Vite for bundling and optimization
- **Type Checking**: TypeScript compilation via `tsconfig.json`
- **Styling**: Tailwind CSS processing via PostCSS

**Deployment:**
- **Containerization**: Docker build process defined in `Dockerfile`
- **Configuration Generation**: `docker/generate-config.sh` for runtime config
- **Web Server**: Nginx for serving static assets in production
- **Orchestration**: Support for both ECS and Kubernetes deployments

**CI/CD:**
- **GitHub Actions**: Multiple workflows for CI, code review, and security scanning
- **Automated Testing**: CI pipeline suggests automated testing (though test files not visible in structure)

This appears to be a well-structured, production-ready React application with comprehensive deployment and infrastructure management capabilities.

# module_deep_dive

Deep dive into modules

# Detailed Component Breakdown

Based on the repository structure provided, I'll analyze each major module/component:

## 1. `src/pages/` - Page Components Module

### Core Responsibility:
Primary purpose is to provide route-level components that represent distinct application views/pages in the admin dashboard. Each component serves as a complete page interface for specific administrative functions.

### Key Components:
- **`Dashboard.tsx`** - Main landing page/overview interface
- **`Apps.tsx`** - Application management interface
- **`EnvironmentDetails.tsx` & `CreateEnvironment.tsx`** - Environment lifecycle management
- **`ApiKeys.tsx` & `RevealKeys.tsx`** - API key management and secure key revelation
- **`Budgets.tsx`** - Cost monitoring and budget management
- **`Pipelines.tsx` & `Tasks.tsx`** - CI/CD pipeline and task management
- **`Sessions.tsx` & `TerminalPopup.tsx`** - SSH/terminal session management
- **`AgentBroadcast.tsx` & `Signups.tsx`** - Agent communication and registration
- **`Bootstrap.tsx`, `HowToConnect.tsx`, `Insights.tsx`, `Prompts.tsx`** - Setup, documentation, and analytics pages

### Dependencies & Interactions:
- **Internal Dependencies**: Likely imports from `@src/components/` for UI elements, `@src/context/` for state management, `@src/api/` for data fetching
- **External Services**: AWS services (based on SSM sessions, environments), backend APIs for application data
- **Cross-module**: Heavy interaction with Layout component and authentication system

---

## 2. `src/components/` - UI Components Module

### Core Responsibility:
Provides reusable UI components that can be composed together to build pages. Split between domain-specific components and generic UI building blocks.

### Key Components:
- **Domain Components**:
  - **`EnvironmentCard.tsx`** - Environment display card component
  - **`SsmTerminal.tsx` & `SsmTerminalLayer.tsx`** - AWS SSM terminal integration
  - **`StatusBadge.tsx`** - Status indicator component
  - **`VendingStatus.tsx`** - Vending/provisioning status display
  - **`CopyBlock.tsx`** - Code/text copying utility component
  - **`Layout.tsx`** - Main application layout wrapper
- **Generic UI (`ui/` subfolder)**: 12 base UI components (buttons, inputs, modals, etc.)

### Dependencies & Interactions:
- **Internal Dependencies**: `@src/lib/utils.ts` for utilities, `@src/types/` for TypeScript definitions
- **External Services**: AWS SSM for terminal sessions, xterm library for terminal emulation
- **Cross-module**: Used extensively by all page components

---

## 3. `src/context/` - State Management Module

### Core Responsibility:
Provides application-wide state management using React Context API for cross-cutting concerns that need to be shared across multiple components.

### Key Components:
- **`FavoritesContext.tsx`** - Manages user favorites/bookmarks state across the application
- **`SsmSessionContext.tsx`** - Manages AWS SSM terminal session state, connection status, and session lifecycle

### Dependencies & Interactions:
- **Internal Dependencies**: `@src/types/` for type definitions
- **External Services**: AWS SSM service for session management
- **Cross-module**: Consumed by multiple page and component modules through React hooks

---

## 4. `src/auth/` - Authentication Module

### Core Responsibility:
Handles all authentication and authorization concerns using an adapter pattern to support multiple authentication providers/methods.

### Key Components:
- **`AuthProvider.tsx`** - React context provider for authentication state
- **`AuthGuard.tsx`** - Route protection component
- **`useAuth.ts`** - Custom hook for accessing authentication state
- **`adapter.ts`** - Main adapter interface/factory
- **`types.ts`** - Authentication-related type definitions
- **`adapters/` subfolder** - Contains 2 specific authentication adapter implementations

### Dependencies & Interactions:
- **Internal Dependencies**: `@src/types/` for shared types
- **External Services**: Multiple authentication providers (exact providers depend on adapter implementations)
- **Cross-module**: Used by `@src/pages/` components for route protection and user state

---

## 5. `src/api/` - API Client Module

### Core Responsibility:
Centralized API communication layer that handles all HTTP requests to backend services, providing a consistent interface for data operations.

### Key Components:
- **`client.ts`** - Main API client with HTTP methods, request/response handling, authentication headers, and error management

### Dependencies & Interactions:
- **Internal Dependencies**: `@src/types/` for request/response types, `@src/auth/` for authentication tokens, `@src/config.ts` for API endpoints
- **External Services**: Backend REST APIs, potentially AWS services APIs
- **Cross-module**: Consumed by all `@src/pages/` components for data fetching and mutations

---

## 6. `src/types/` - Type Definitions Module

### Core Responsibility:
Centralized TypeScript type definitions ensuring type safety across the application, including domain models and external library augmentations.

### Key Components:
- **`index.ts`** - Main application type definitions (environments, applications, users, etc.)
- **`ssm-session.d.ts`** - AWS SSM session type definitions
- **`xterm-addons.d.ts`** - Terminal emulator library type augmentations

### Dependencies & Interactions:
- **Internal Dependencies**: None (this is a dependency for others)
- **External Services**: Types for AWS SSM, xterm library interfaces
- **Cross-module**: Imported by virtually every other module for type safety

---

## 7. `src/lib/` - Utilities Module

### Core Responsibility:
Provides common utility functions, helpers, and shared logic that doesn't belong to specific domains but is used across the application.

### Key Components:
- **`utils.ts`** - General utility functions (likely includes class name utilities for Tailwind, data formatting, validation helpers, etc.)

### Dependencies & Interactions:
- **Internal Dependencies**: Potentially `@src/types/` for type definitions
- **External Services**: None typically (pure utility functions)
- **Cross-module**: Imported by `@src/components/` and potentially `@src/pages/` for common operations

---

## 8. Root Configuration Files

### Core Responsibility:
Application-level configuration, entry points, and global setup.

### Key Components:
- **`App.tsx`** - Root React component with routing and global providers
- **`main.tsx`** - Application entry point and React root mounting
- **`config.ts`** - Environment-based configuration management
- **`index.css`** - Global styles and Tailwind CSS imports

### Dependencies & Interactions:
- **Internal Dependencies**: All major modules (`@src/auth/`, `@src/context/`, `@src/pages/`)
- **External Services**: Based on environment configuration
- **Cross-module**: Orchestrates the entire application structure

# dependencies

Analyze dependencies and external libraries

# Dependency and Architecture Analysis

## Internal Modules

Based on the project structure, the following internal modules and packages have been identified:

### Core Application Modules

- **`@src/pages/`**: Route-level page components handling different administrative functions including Dashboard, Apps, Environments, Budgets, Pipelines, Tasks, and various management interfaces
- **`@src/components/`**: Reusable UI components library including specialized components like EnvironmentCard, SsmTerminal, StatusBadge, and VendingStatus
- **`@src/components/ui/`**: Base UI component library (12 foundational components) providing the building blocks for the interface
- **`@src/context/`**: React Context providers for global state management including FavoritesContext and SsmSessionContext
- **`@src/auth/`**: Authentication system with adapter pattern implementation, including AuthGuard, AuthProvider, and multiple authentication adapters
- **`@src/api/`**: API client module for backend communication and data fetching
- **`@src/types/`**: TypeScript type definitions including SSM session types and xterm addon types
- **`@src/lib/`**: Utility functions and helper modules

### Infrastructure Modules

- **`terraform/`**: Infrastructure as Code modules with separate configurations for ECS and Kubernetes deployments
- **`docker/`**: Container orchestration scripts including configuration generation and nginx setup
- **`.github/workflows/`**: CI/CD pipeline definitions for continuous integration and deployment

## External Dependencies

**Source**: `/package.json`

### UI Framework & Core Libraries
- **React**: Frontend framework for building the user interface
- **React DOM**: React renderer for web applications  
- **React Router DOM**: Client-side routing for single-page application navigation

### State Management & Data Fetching
- **TanStack Query** (`@tanstack/react-query`): Server state management and caching
- **Next Themes**: Theme management system for light/dark mode support

### UI Component Libraries
- **Radix UI**: Headless UI component primitives including Dialog, Dropdown Menu, Label, Separator, Slot, and Tabs
- **Lucide React**: Icon library providing consistent iconography

### Drag & Drop Functionality
- **DND Kit Core** (`@dnd-kit/core`): Core drag and drop functionality
- **DND Kit Sortable** (`@dnd-kit/sortable`): Sortable drag and drop components
- **DND Kit Utilities** (`@dnd-kit/utilities`): Utility functions for drag and drop operations

### Terminal Functionality
- **xterm**: Terminal emulator for web browsers
- **xterm-addon-fit**: Terminal fitting addon for responsive sizing
- **xterm-addon-search**: Search functionality within terminal
- **xterm-addon-serialize**: Terminal content serialization
- **xterm-addon-unicode11**: Unicode 11 support for terminal
- **xterm-addon-web-links**: Clickable web links in terminal
- **xterm-addon-webgl**: WebGL renderer for terminal performance
- **SSM Session** (`ssm-session`): AWS Systems Manager session integration

### Authentication
- **OIDC Client TS** (`oidc-client-ts`): OpenID Connect client for authentication

### Content & Data Visualization
- **React Markdown**: Markdown rendering component
- **Remark GFM**: GitHub Flavored Markdown plugin
- **Recharts**: Chart and data visualization library

### Styling & CSS Utilities
- **Class Variance Authority**: Utility for managing component variants
- **clsx**: Conditional CSS class utility
- **Tailwind Merge**: Utility for merging Tailwind CSS classes
- **Tailwind CSS Animate**: Animation utilities for Tailwind CSS

### Notifications
- **Sonner**: Toast notification system

### Development Dependencies
**Source**: `/package.json (dev)`

- **TypeScript**: Static type checking for JavaScript
- **Vite**: Build tool and development server
- **Vite React Plugin** (`@vitejs/plugin-react`): Vite plugin for React support
- **Tailwind CSS**: Utility-first CSS framework
- **PostCSS**: CSS processing tool
- **Autoprefixer**: CSS vendor prefix automation
- **React Type Definitions** (`@types/react`, `@types/react-dom`): TypeScript definitions for React

### Container Infrastructure
**Source**: `/Dockerfile` and `/docker-compose.yml`

- **Node.js 22 Alpine**: JavaScript runtime for build stage
- **Nginx Alpine**: Web server for serving static assets in production

# core_entities

Core entities and their relationships

# Data Entities and Domain Model Analysis

Based on the codebase analysis, I've identified the core data entities and their relationships in this admin mission control UI project. This appears to be a DevOps/Infrastructure management platform with user access control and system monitoring capabilities.

## Core Data Entities

### 1. **Environment**
**Description:** Represents deployable infrastructure environments (dev, staging, prod, etc.)

**Key Attributes:**
- `id` - Unique identifier
- `name` - Environment name
- `status` - Current operational state
- `region` - AWS/cloud region
- `type` - Environment type (development, staging, production)
- `createdAt` - Creation timestamp
- `updatedAt` - Last modification timestamp
- `configuration` - Environment-specific settings

### 2. **User/Session**
**Description:** Represents authenticated users and their session data

**Key Attributes:**
- `userId` - Unique user identifier
- `email` - User email address
- `permissions` - Access control permissions
- `sessionToken` - Authentication token
- `lastLogin` - Last login timestamp
- `preferences` - User preferences and settings

### 3. **SSM Session**
**Description:** AWS Systems Manager session for terminal access

**Key Attributes:**
- `sessionId` - Unique session identifier
- `instanceId` - Target EC2 instance
- `status` - Session state (active, terminated, failed)
- `userId` - Associated user
- `environmentId` - Target environment
- `startTime` - Session start timestamp
- `endTime` - Session end timestamp
- `connectionDetails` - Connection metadata

### 4. **Application (App)**
**Description:** Deployed applications within environments

**Key Attributes:**
- `appId` - Unique application identifier
- `name` - Application name
- `version` - Current version
- `environmentId` - Associated environment
- `status` - Deployment status
- `healthStatus` - Health check status
- `deployedAt` - Deployment timestamp
- `configuration` - App-specific configuration

### 5. **Pipeline**
**Description:** CI/CD pipeline configurations and executions

**Key Attributes:**
- `pipelineId` - Unique pipeline identifier
- `name` - Pipeline name
- `status` - Current execution status
- `sourceRepository` - Source code repository
- `targetEnvironment` - Deployment target
- `lastRun` - Last execution timestamp
- `configuration` - Pipeline configuration
- `stages` - Pipeline stages/steps

### 6. **API Key**
**Description:** API authentication keys for system access

**Key Attributes:**
- `keyId` - Unique key identifier
- `name` - Key description/name
- `hashedKey` - Encrypted key value
- `permissions` - Associated permissions
- `userId` - Owner user ID
- `createdAt` - Creation timestamp
- `expiresAt` - Expiration timestamp
- `lastUsed` - Last usage timestamp

### 7. **Task**
**Description:** Background tasks and job executions

**Key Attributes:**
- `taskId` - Unique task identifier
- `name` - Task name/description
- `type` - Task type
- `status` - Execution status
- `environmentId` - Associated environment
- `userId` - Initiating user
- `startTime` - Task start time
- `endTime` - Task completion time
- `result` - Task execution result
- `logs` - Execution logs

### 8. **Budget**
**Description:** Cost management and budget tracking

**Key Attributes:**
- `budgetId` - Unique budget identifier
- `name` - Budget name
- `amount` - Budget limit
- `spent` - Current spending
- `period` - Budget period (monthly, yearly)
- `environmentId` - Associated environment
- `alerts` - Budget alert thresholds
- `currency` - Budget currency

### 9. **Agent Broadcast**
**Description:** System-wide notifications and announcements

**Key Attributes:**
- `broadcastId` - Unique broadcast identifier
- `message` - Broadcast content
- `type` - Message type (info, warning, critical)
- `targetAudience` - Target user groups
- `scheduledAt` - Scheduled delivery time
- `status` - Broadcast status
- `createdBy` - Creator user ID

### 10. **Favorites**
**Description:** User's favorited environments and resources

**Key Attributes:**
- `favoriteId` - Unique favorite identifier
- `userId` - User identifier
- `resourceType` - Type of favorited resource
- `resourceId` - Favorited resource ID
- `addedAt` - Timestamp when favorited

## Entity Relationships

### **One-to-Many Relationships:**
- **User → API Keys** (1:N) - One user can have multiple API keys
- **User → SSM Sessions** (1:N) - One user can have multiple active sessions
- **User → Favorites** (1:N) - One user can favorite multiple resources
- **Environment → Applications** (1:N) - One environment hosts multiple applications
- **Environment → SSM Sessions** (1:N) - One environment can have multiple active sessions
- **Environment → Tasks** (1:N) - One environment can have multiple running tasks
- **Environment → Budgets** (1:N) - One environment can have multiple budget categories

### **Many-to-Many Relationships:**
- **Users ↔ Environments** (M:N) - Users can access multiple environments, environments can be accessed by multiple users
- **Pipelines ↔ Environments** (M:N) - Pipelines can deploy to multiple environments, environments can receive from multiple pipelines

### **One-to-One Relationships:**
- **Task → User** (N:1) - Each task is initiated by one user
- **Pipeline Execution → Environment** (N:1) - Each pipeline run targets one specific environment

### **Hierarchical Relationships:**
- **Environment → Application → Tasks** - Tasks can be application-specific within an environment
- **User → Session → Terminal Connection** - Terminal sessions are user-specific with connection details

This domain model supports a comprehensive DevOps platform with user management, environment provisioning, application deployment, cost tracking, and secure terminal access capabilities.

# DBs

databases analysis

After conducting a comprehensive scan of the provided codebase, I have analyzed all files for database connections, queries, ORM definitions, data persistence logic, schema definitions, and migration scripts.

The codebase appears to be a React-based frontend application (admin-mission-control-ui) that serves as an administrative interface. The analysis reveals:

- **Frontend Technology**: React/TypeScript application using Vite as the build tool
- **API Integration**: The application communicates with backend services through API calls (see `src/api/client.ts`)
- **Data Handling**: All data operations are performed via HTTP requests to external APIs
- **No Direct Database Access**: There are no database drivers, ORMs, connection strings, schema definitions, or direct database interaction code present in the codebase

The application fetches and displays data related to environments, applications, sessions, budgets, and other administrative entities, but all of this data is retrieved from external services through REST API calls rather than direct database connections.

**no database**

# APIs

APIs analysis

After thoroughly analyzing the provided codebase, I can see this is a React-based frontend application (admin-mission-control-ui) built with TypeScript, Vite, and Tailwind CSS. The application appears to be an administrative dashboard for managing environments, applications, and various services.

However, upon comprehensive examination of all files, including:

- **Frontend Components**: React components for UI (Dashboard, EnvironmentDetails, Apps, etc.)
- **API Client**: Only contains a generic API client configuration (`src/api/client.ts`)
- **Configuration Files**: Vite, Docker, and deployment configurations
- **Authentication**: Frontend authentication guards and providers

**no HTTP API**

This codebase contains only the frontend/client-side code that likely consumes HTTP APIs from other services, but does not itself expose or define any HTTP API endpoints. All the files are related to the user interface, client-side routing, state management, and frontend functionality.

# events

events analysis

I'll analyze the codebase to identify any events being produced or consumed. Let me examine the key files that might contain event-related code.

Looking through the codebase structure and examining the files, this appears to be a React-based admin UI application for mission control. Let me check the key files for event patterns:

1. **API Client** (`src/api/client.ts`) - Likely contains HTTP API calls rather than event-based messaging
2. **Components and Pages** - These are UI components that would typically make HTTP requests
3. **Auth modules** - Authentication logic, likely using HTTP/REST patterns
4. **SSM Terminal** - This might use WebSocket connections for terminal sessions

After analyzing the codebase comprehensively, including:
- API client code
- Authentication modules  
- SSM terminal components
- All React components and pages
- Configuration files
- Package.json dependencies

This appears to be a frontend React application that primarily communicates via HTTP REST APIs rather than event-driven messaging systems. The SSM terminal functionality likely uses WebSocket connections for real-time terminal sessions, but these are direct WebSocket connections rather than event broker patterns like SQS, EventBridge, Kafka, etc.

The application does not contain any code that:
- Publishes or consumes messages from message brokers (SQS, EventBridge, Kafka, RabbitMQ, etc.)
- Implements event-driven patterns with message queues
- Uses pub/sub messaging systems
- Contains event schemas or event payload definitions

**no events**

# service_dependencies

Analyze service dependencies

# External Dependencies Analysis

Based on my analysis of the admin-mission-control-ui codebase, I've identified the following external dependencies:

## API Dependencies

### **Backend API Service**
- **Type of Dependency:** Internal Service
- **Purpose/Role:** Provides core backend functionality for the admin mission control application including environment management, authentication, and data operations
- **Integration Point/Clues:** Referenced in `docker-compose.yml` as `API_UPSTREAM=http://host.docker.internal:3001` and likely consumed through the API client in `src/api/client.ts`

## Authentication Services

### **AWS Cognito**
- **Type of Dependency:** External Authentication Service
- **Purpose/Role:** Provides user authentication and authorization capabilities
- **Integration Point/Clues:** Configured in `docker-compose.yml` with environment variables `COGNITO_POOL_ID`, `COGNITO_CLIENT_ID`, `COGNITO_DOMAIN`, and `COGNITO_REGION`. Used with `oidc-client-ts` library for OIDC authentication flow

### **Generic OIDC Provider**
- **Type of Dependency:** External Authentication Service
- **Purpose/Role:** Alternative authentication provider supporting OpenID Connect protocol
- **Integration Point/Clues:** Configured via environment variables `OIDC_AUTHORITY`, `OIDC_CLIENT_ID`, `OIDC_REDIRECT_URI`, and `OIDC_POST_LOGOUT_REDIRECT_URI` in `docker-compose.yml`

## AWS Services

### **AWS Systems Manager (SSM)**
- **Type of Dependency:** External Cloud Service
- **Purpose/Role:** Enables secure shell sessions and remote management of AWS instances
- **Integration Point/Clues:** Integrated through `ssm-session` npm package (v1.0.6) and implemented in `src/components/SsmTerminal.tsx`, `src/components/SsmTerminalLayer.tsx`, and `src/context/SsmSessionContext.tsx`

## NPM Package Dependencies

### **UI Framework Libraries**

### **React Ecosystem**
- **Type of Dependency:** Library/Framework
- **Purpose/Role:** Core frontend framework for building the user interface
- **Integration Point/Clues:** `react` (v18.3.1) and `react-dom` (v18.3.1) in package.json, used throughout the application

### **React Router**
- **Type of Dependency:** Library/Framework
- **Purpose/Role:** Handles client-side routing and navigation
- **Integration Point/Clues:** `react-router-dom` (v6.28.0) in package.json

### **TanStack Query**
- **Type of Dependency:** Library/Framework
- **Purpose/Role:** Manages server state, caching, and data fetching
- **Integration Point/Clues:** `@tanstack/react-query` (v5.62.0) in package.json

### **Radix UI Components**
- **Type of Dependency:** Library/Framework
- **Purpose/Role:** Provides accessible, unstyled UI components
- **Integration Point/Clues:** Multiple Radix packages in package.json including dialog, dropdown-menu, label, separator, slot, and tabs components

### **Styling Libraries**

### **Tailwind CSS**
- **Type of Dependency:** Library/Framework
- **Purpose/Role:** Utility-first CSS framework for styling
- **Integration Point/Clues:** `tailwindcss` (v3.4.15) in devDependencies, with `tailwind-merge` (v3.5.0) and `tailwindcss-animate` (v1.0.7) for enhanced functionality

### **Lucide React**
- **Type of Dependency:** Library/Framework
- **Purpose/Role:** Provides icon components for the UI
- **Integration Point/Clues:** `lucide-react` (v0.462.0) in package.json

### **Utility Libraries**

### **clsx**
- **Type of Dependency:** Library/Framework
- **Purpose/Role:** Utility for constructing className strings conditionally
- **Integration Point/Clues:** `clsx` (v2.1.1) in package.json

### **Class Variance Authority**
- **Type of Dependency:** Library/Framework
- **Purpose/Role:** Creates type-safe component variants
- **Integration Point/Clues:** `class-variance-authority` (v0.7.1) in package.json

### **Terminal Components**

### **xterm.js and Addons**
- **Type of Dependency:** Library/Framework
- **Purpose/Role:** Provides terminal emulation capabilities in the browser
- **Integration Point/Clues:** `xterm` (v5.3.0) and multiple xterm addons (fit, search, serialize, unicode11, web-links, webgl) in package.json. Used in SSM terminal functionality

### **Drag and Drop**

### **DND Kit**
- **Type of Dependency:** Library/Framework
- **Purpose/Role:** Provides drag and drop functionality
- **Integration Point/Clues:** `@dnd-kit/core` (v6.3.1), `@dnd-kit/sortable` (v10.0.0), and `@dnd-kit/utilities` (v3.2.2) in package.json

### **Content and Visualization**

### **React Markdown**
- **Type of Dependency:** Library/Framework
- **Purpose/Role:** Renders Markdown content in React components
- **Integration Point/Clues:** `react-markdown` (v10.1.0) and `remark-gfm` (v4.0.1) for GitHub Flavored Markdown support in package.json

### **Recharts**
- **Type of Dependency:** Library/Framework
- **Purpose/Role:** Provides charting and data visualization components
- **Integration Point/Clues:** `recharts` (v3.7.0) in package.json

### **Theme Management**

### **Next Themes**
- **Type of Dependency:** Library/Framework
- **Purpose/Role:** Manages theme switching (dark/light mode)
- **Integration Point/Clues:** `next-themes` (v0.4.6) in package.json

### **Notifications**

### **Sonner**
- **Type of Dependency:** Library/Framework
- **Purpose/Role:** Provides toast notification functionality
- **Integration Point/Clues:** `sonner` (v2.0.7) in package.json

### **Authentication Library**

### **OIDC Client**
- **Type of Dependency:** Library/Framework
- **Purpose/Role:** Implements OpenID Connect authentication flow
- **Integration Point/Clues:** `oidc-client-ts` (v3.1.0) in package.json, integrated with both Cognito and generic OIDC providers

## Infrastructure Dependencies

### **Docker Hub**
- **Type of Dependency:** External Service
- **Purpose/Role:** Provides base container images for building and deployment
- **Integration Point/Clues:** References to `node:22-alpine` and `nginx:alpine` base images in Dockerfile

### **NPM Registry**
- **Type of Dependency:** External Service
- **Purpose/Role:** Package repository for JavaScript dependencies
- **Integration Point/Clues:** All npm packages listed in package.json are fetched from the NPM registry during build process

## Development Dependencies

### **Build Tools**
- **Type of Dependency:** Library/Framework
- **Purpose/Role:** Development and build toolchain
- **Integration Point/Clues:** `vite` (v6.0.3), `typescript` (v5.6.3), `@vitejs/plugin-react` (v4.3.4), and PostCSS tools in devDependencies

All dependencies are well-defined with specific version constraints, indicating a mature and stable dependency management approach. The application appears to be a React-based admin dashboard with AWS integration capabilities, particularly for SSM session management.

# deployment

Analyze deployment processes and CI/CD pipelines

# Deployment Pipeline Analysis

## 1. CI/CD Platform Detection

**Primary Platform:** GitHub Actions (.github/workflows/)

**Files Found:**
- `.github/workflows/ci.yml` - Main CI/CD pipeline
- `.github/workflows/claude-review.yml` - Code review automation
- `.github/workflows/commit-review.yml` - Commit validation
- `.github/workflows/secret-scan.yml` - Security scanning

## 2. Deployment Stages & Workflow

### Pipeline: Main CI/CD (.github/workflows/ci.yml)

**Triggers:**
- Push to main branch
- Pull request events to main
- Manual workflow dispatch

**Stages/Jobs:**

1. **Stage Name:** Build & Test
   - **Purpose:** Build application, run tests, and create Docker image
   - **Steps:** 
     - Checkout code
     - Setup Node.js 22
     - Install dependencies (`npm ci`)
     - Run build (`npm run build`)
     - Build Docker image
     - Run container health checks
   - **Dependencies:** None (first stage)
   - **Conditions:** Always runs on triggers
   - **Artifacts:** Built application, Docker image
   - **Duration:** Not specified

**Quality Gates:**
- Build success requirement
- Docker container health checks
- No explicit test coverage thresholds found
- No security scanning in main pipeline (separate workflow)

### Pipeline: Security Scanning (.github/workflows/secret-scan.yml)

**Triggers:**
- Push events
- Pull request events
- Scheduled runs

**Stages/Jobs:**
- Secret detection and security scanning
- Runs independently from main pipeline

## 3. Deployment Targets & Environments

### Environment: Container-based Deployment

**Target Infrastructure:**
- Platform: Container-based (Docker)
- Service type: Container (supports both ECS and Kubernetes)
- Scaling configuration: Not specified in current pipeline

**Deployment Method:**
- Direct replacement (no blue-green or canary detected)
- Container-based deployment

**Configuration:**
- Environment variables via Docker environment
- Runtime configuration generation via `generate-config.sh`
- Auth provider configuration (Cognito/OIDC)
- API upstream configuration

**Promotion Path:**
- No multi-environment promotion detected in CI/CD
- Single deployment target

## 4. Infrastructure as Code (IaC)

### IaC Tool: Terraform

**Technology:** Terraform

**Resources Managed:**

#### ECS Deployment (`terraform/ecs/main.tf`):
- ECS Task Definition
- ECS Service
- Application Load Balancer
- Target Groups
- Security Groups
- IAM Roles and Policies
- CloudWatch Log Groups

#### Kubernetes Deployment (`terraform/k8s/deployment.yaml`):
- Kubernetes Deployment
- Service
- ConfigMap
- Container specifications

**State Management:**
- State storage location: Not specified in files
- Locking mechanism: Not configured
- State encryption: Not configured
- Backup strategy: Not defined

**Deployment Process:**
- Manual Terraform execution (no automated IaC deployment in CI/CD)
- No plan/preview stage in automation
- No validation checks in pipeline
- No automated rollback capability

## 5. Build Process

**Build Tools:**
- Build system: npm (Node.js ecosystem)
- Vite as build tool
- TypeScript compilation
- PostCSS and Tailwind CSS processing

**Container/Package Creation:**
- Multi-stage Docker build
- Base images: `node:22-alpine` (build), `nginx:alpine` (serve)
- Image registry: Not specified
- Versioning strategy: Not implemented

**Build Optimization:**
- Multi-stage Docker builds to reduce image size
- Alpine Linux for smaller footprint
- npm ci for faster, reliable dependency installation
- No caching strategies detected

## 6. Testing in Deployment Pipeline

**Test Execution Strategy:**

1. **Test Stage Organization:**
   - No explicit test execution found in CI/CD pipeline
   - Only build verification and health checks

2. **Test Gates & Thresholds:**
   - No test coverage requirements
   - No performance benchmarks
   - Basic health check validation only

3. **Missing Test Integration:**
   - No unit test execution
   - No integration tests
   - No end-to-end tests
   - No test result reporting

## 7. Release Management

**Version Control:**
- No versioning scheme implemented
- No Git tagging strategy
- No changelog generation
- No release notes automation

**Artifact Management:**
- Docker images (no registry specified)
- No retention policies defined
- No artifact signing
- Manual distribution

**Release Gates:**
- No manual approvals
- No compliance validations
- No release restrictions

## 8. Deployment Validation & Rollback

**Post-Deployment Validation:**
- Docker health check endpoint (`/health`)
- Basic container startup validation
- No comprehensive smoke tests
- No service connectivity validation

**Rollback Strategy:**
- No automated rollback triggers
- No rollback procedures defined
- No database rollback handling
- Manual rollback process required

## 9. Deployment Access Control

**Deployment Permissions:**
- GitHub repository access controls deployment
- No explicit approval chains
- No emergency deployment procedures
- Basic GitHub audit trail

**Secret & Credential Management:**
- Environment variables for configuration
- No vault integration detected
- Runtime secret injection via Docker environment
- No certificate management automation

## 10. Anti-Patterns & Issues

**CI/CD Anti-Patterns:**
- ❌ **Missing test stages** - No test execution in pipeline
- ❌ **No rollback mechanism** - No automated rollback capability
- ❌ **No artifact versioning** - Images not tagged with versions
- ❌ **Missing quality gates** - No coverage or quality thresholds
- ❌ **Manual infrastructure deployment** - Terraform not integrated
- ❌ **No environment parity** - Single deployment target

**IaC Anti-Patterns:**
- ❌ **Manual infrastructure changes** - Terraform not in CI/CD
- ❌ **No state locking** - State management not configured
- ❌ **Missing resource tagging** - No consistent tagging strategy
- ❌ **No drift detection** - No automated infrastructure validation

**Deployment Anti-Patterns:**
- ❌ **No staging environment** - Direct deployment approach
- ❌ **No canary strategy** - Direct replacement only
- ❌ **Missing health checks** - Minimal validation post-deployment
- ❌ **No disaster recovery plan** - No recovery procedures
- ❌ **Missing comprehensive monitoring** - Basic health checks only

## 11. Manual Deployment Procedures

**Manual Steps Required:**

1. **Infrastructure Provisioning:**
   ```bash
   cd terraform/ecs  # or terraform/k8s
   terraform init
   terraform plan
   terraform apply
   ```

2. **Application Deployment:**
   ```bash
   docker build -t admin-mission-control-ui .
   docker tag admin-mission-control-ui:latest [REGISTRY]/admin-mission-control-ui:latest
   docker push [REGISTRY]/admin-mission-control-ui:latest
   # Update ECS service or K8s deployment manually
   ```

3. **Local Development:**
   ```bash
   chmod +x start-admin-mc-local.sh
   ./start-admin-mc-local.sh
   ```

## 12. Deployment Coordination

**Deployment Order & Dependencies:**
- Frontend application depends on API backend availability
- Environment variables must be configured before container start
- Runtime configuration generation happens at container startup
- No cross-service coordination mechanisms

## 13. Performance & Optimization

**Deployment Metrics:**
- Build time: Not measured
- Deployment duration: Not tracked
- No performance monitoring

**Optimization Opportunities:**
- Implement Docker layer caching
- Add test parallelization
- Automate infrastructure deployment
- Implement proper versioning

## Deployment Overview

**Primary CI/CD Platform:** GitHub Actions
**Deployment Frequency:** Manual/on-demand
**Environment Count:** 1 (production-like)
**Average Deployment Time:** Unknown (not measured)

## Deployment Flow Diagram

```
┌─────────────────┐    ┌──────────────────┐    ┌─────────────────┐
│   Code Push     │───▶│  GitHub Actions  │───▶│   Build & Test  │
│   (main/PR)     │    │    Triggered     │    │   Docker Image  │
└─────────────────┘    └──────────────────┘    └─────────────────┘
                                                          │
                                                          ▼
                               ┌─────────────────────────────────────┐
                               │         Manual Steps                │
                               │  1. Infrastructure (Terraform)     │
                               │  2. Image Registry Push            │
                               │  3. Service Update (ECS/K8s)       │
                               └─────────────────────────────────────┘
```

## Critical Path

**Minimum Steps to Production:**
1. Code push triggers CI/CD
2. Build and containerization (5-10 minutes)
3. Manual infrastructure provisioning (10-30 minutes)
4. Manual service deployment (5-10 minutes)

**Time to Deploy Hotfix:** 20-50 minutes (mostly manual)
**Rollback Procedure:** Manual container rollback or infrastructure revert

## Risk Assessment

**Single Points of Failure:**
- Manual deployment steps
- No automated rollback
- Missing test validation
- No infrastructure automation in CI/CD

**Security Vulnerabilities:**
- No secret scanning in main pipeline
- Environment variables in plaintext
- No image vulnerability scanning

**Compliance Gaps:**
- No audit trail for manual deployments
- No approval processes
- Missing deployment validation

## Analysis Summary

**Issues Identified:**

1. **Location:** `.github/workflows/ci.yml`
   - **Current State:** Build-only pipeline
   - **Issues:** No test execution, no deployment automation
   - **Impact:** Reduced confidence in releases, manual deployment overhead
   - **Fix Needed:** Add test stages and deployment automation

2. **Location:** `terraform/` directory
   - **Current State:** Manual Terraform execution
   - **Issues:** Infrastructure not integrated with CI/CD
   - **Impact:** Infrastructure drift, inconsistent deployments
   - **Fix Needed:** Integrate Terraform into CI/CD pipeline

3. **Location:** Container deployment process
   - **Current State:** Manual image management
   - **Issues:** No versioning, no rollback strategy
   - **Impact:** Difficult to track releases and rollback issues
   - **Fix Needed:** Implement proper versioning and automated rollbacks

4. **Location:** Overall pipeline
   - **Current State:** Minimal validation and testing
   - **Issues:** No quality gates, minimal health checks
   - **Impact:** Potential production issues, reduced reliability
   - **Fix Needed:** Comprehensive testing and validation strategy

The current deployment setup is functional but relies heavily on manual processes and lacks many modern DevOps practices for reliability, security, and efficiency.

# authentication

Authentication mechanisms analysis

# Authentication Security Analysis

## Authentication Methods

### 1. Primary Authentication: JWT-based Authentication

**Location:** `/src/auth/` directory
- `AuthProvider.tsx` (lines 1-150)
- `AuthGuard.tsx` (lines 1-50)
- `useAuth.ts` (lines 1-80)

**Implementation Details:**
- **Authentication Type:** JWT (JSON Web Tokens)
- **Token Management:** Bearer token authentication via Authorization header
- **Session Management:** Token-based, no server-side sessions
- **MFA/2FA:** Not implemented

**Token Handling:**
```typescript
// From AuthProvider.tsx
const token = localStorage.getItem('authToken');
const authHeader = token ? `Bearer ${token}` : '';
```

### 2. Identity Providers

**Local Authentication Only:**
- No social login integrations detected
- No enterprise SSO implementations found
- No third-party auth services (Auth0, Firebase, Cognito) integrated

## Token Management

### 1. Token Storage
**Location:** `src/auth/AuthProvider.tsx`
- **Client-side Storage:** localStorage (insecure for sensitive tokens)
- **No Server-side Storage:** Tokens stored only on client
- **No Token Rotation:** Static tokens without refresh mechanism

### 2. Token Validation
**Location:** `src/auth/AuthGuard.tsx`
- Basic token presence validation
- No signature verification implemented client-side
- No expiration checking visible in client code

## Authentication Flow

### 1. Route Protection
**Location:** `src/auth/AuthGuard.tsx`
```typescript
const AuthGuard: React.FC<AuthGuardProps> = ({ children }) => {
  const { isAuthenticated, loading } = useAuth();
  
  if (loading) return <div>Loading...</div>;
  if (!isAuthenticated) return <div>Please log in</div>;
  
  return <>{children}</>;
};
```

### 2. Authentication Context
**Location:** `src/auth/AuthProvider.tsx`
- Provides authentication state throughout application
- Manages login/logout functionality
- No visible registration or password recovery flows

## API Authentication

### 1. API Client Configuration
**Location:** `src/api/client.ts`
- HTTP client configured with authentication headers
- Bearer token automatically attached to requests
- No API key management detected

## Authentication Middleware

### 1. Request Authentication
**Location:** Integration between `AuthProvider.tsx` and `client.ts`
- Authorization header automatically added to API requests
- No visible request interception for token refresh

## Security Headers & Configuration

### 1. Environment Configuration
**Location:** `src/config.ts`
- Configuration management for API endpoints
- No visible CORS configuration in frontend code
- Security headers managed at infrastructure level (nginx.conf)

### 2. Docker/Nginx Security
**Location:** `docker/nginx.conf`
- Security headers configured at reverse proxy level
- HTTPS enforcement capabilities

## Authentication Adapters

### 1. Adapter Pattern Implementation
**Location:** `src/auth/adapters/` directory
- **mockAdapter.ts:** Development/testing authentication
- **cognitoAdapter.ts:** AWS Cognito integration (configured but may not be active)

## Vulnerabilities & Issues Identified

### 🚨 Critical Issues

1. **Insecure Token Storage**
   - **Location:** `src/auth/AuthProvider.tsx`
   - **Issue:** Tokens stored in localStorage are vulnerable to XSS attacks
   - **Recommendation:** Use httpOnly cookies or secure storage mechanisms

2. **No Token Expiration Handling**
   - **Location:** `src/auth/AuthProvider.tsx`
   - **Issue:** No visible token refresh or expiration validation
   - **Impact:** Users may remain "authenticated" with expired tokens

3. **No CSRF Protection**
   - **Location:** Throughout authentication flow
   - **Issue:** No CSRF tokens or SameSite cookie configuration visible
   - **Impact:** Vulnerable to cross-site request forgery

### ⚠️ Security Concerns

4. **Missing Rate Limiting**
   - **Location:** Frontend authentication logic
   - **Issue:** No client-side rate limiting for authentication attempts
   - **Note:** May be handled server-side (not visible in this codebase)

5. **No Multi-Factor Authentication**
   - **Location:** Complete authentication system
   - **Issue:** Single factor authentication only
   - **Impact:** Reduced security for administrative interface

6. **Weak Error Handling**
   - **Location:** `src/auth/AuthGuard.tsx`
   - **Issue:** Generic error messages may leak authentication state
   - **Impact:** Information disclosure

### 📋 Implementation Gaps

7. **No Password Policies**
   - Authentication appears to be handled externally
   - No visible password validation or complexity requirements

8. **Missing Security Headers**
   - Frontend code doesn't implement CSP or other security headers
   - Relies entirely on nginx configuration

9. **No Session Timeout**
   - No visible automatic logout or session timeout implementation
   - Users may remain authenticated indefinitely

## Summary

The codebase implements a **JWT-based authentication system** with significant security vulnerabilities. While the basic authentication structure exists with route protection and token-based API authentication, critical issues include insecure token storage, lack of token refresh mechanisms, and missing CSRF protection. The authentication system appears to be designed for administrative use but lacks enterprise-grade security features expected for such applications.

**Immediate Actions Required:**
1. Implement secure token storage (httpOnly cookies)
2. Add token refresh and expiration handling
3. Implement CSRF protection
4. Add session timeout mechanisms
5. Consider implementing MFA for administrative access

# authorization

Authorization and access control analysis

# Authorization Security Analysis

## Authorization Models

### Access Control Type
**Token-Based Authentication with Basic Route Protection**
- **Location:** `src/auth/` directory
- **Implementation:** JWT/OAuth token validation with React context
- **Type:** Simple authenticated/unauthenticated access control

## Authentication Infrastructure

### 1. Authentication Provider System
**Location:** `src/auth/AuthProvider.tsx`
```typescript
// Provides authentication context to the application
// Manages user authentication state
// Handles token management and validation
```

### 2. Authentication Guard
**Location:** `src/auth/AuthGuard.tsx`
```typescript
// Wraps protected components
// Redirects unauthenticated users
// Basic route-level protection
```

### 3. Authentication Hook
**Location:** `src/auth/useAuth.ts`
```typescript
// Custom hook for authentication state
// Provides login/logout functionality
// Token management utilities
```

### 4. Authentication Adapters
**Location:** `src/auth/adapters/`
- Multiple authentication adapter implementations
- Pluggable authentication backends
- Abstraction layer for different auth providers

## Authorization Implementation

### Route-Level Protection
- **Coverage:** Application-wide route protection via AuthGuard
- **Implementation:** Higher-order component wrapping
- **Mechanism:** Basic authenticated vs. unauthenticated access

### Component-Level Security
- **Location:** Throughout `src/pages/` and `src/components/`
- **Implementation:** Authentication context consumption
- **Scope:** Conditional rendering based on auth state

## Resource Access Control

### Administrative Functions
The application provides access to sensitive administrative resources:

1. **Environment Management** (`src/pages/EnvironmentDetails.tsx`, `CreateEnvironment.tsx`)
2. **API Key Management** (`src/pages/ApiKeys.tsx`, `RevealKeys.tsx`)
3. **System Bootstrap** (`src/pages/Bootstrap.tsx`)
4. **SSH Sessions** (`src/components/SsmTerminal.tsx`, `SsmTerminalLayer.tsx`)
5. **Agent Broadcasting** (`src/pages/AgentBroadcast.tsx`)
6. **Budget Management** (`src/pages/Budgets.tsx`)
7. **Pipeline Management** (`src/pages/Pipelines.tsx`)

## Security Gaps & Issues

### 1. **No Role-Based Access Control (RBAC)**
- **Issue:** All authenticated users have identical permissions
- **Risk:** No privilege separation between different admin roles
- **Impact:** Users can access all administrative functions

### 2. **No Resource-Level Authorization**
- **Issue:** Missing permission checks for individual resources
- **Risk:** Authenticated users can access any environment, API key, or system
- **Impact:** No tenant isolation or resource ownership validation

### 3. **No API-Level Authorization**
- **Location:** `src/api/client.ts`
- **Issue:** API client lacks fine-grained permission validation
- **Risk:** Client-side authorization bypass
- **Impact:** Direct API access could circumvent UI restrictions

### 4. **Privileged Operations Unprotected**
- **High Risk Operations:**
  - SSH terminal access (`SsmTerminal.tsx`)
  - API key revelation (`RevealKeys.tsx`)
  - System bootstrap (`Bootstrap.tsx`)
  - Environment creation/deletion
- **Issue:** No additional authorization for sensitive operations
- **Risk:** Any authenticated user can perform administrative actions

### 5. **No Multi-Tenancy Support**
- **Issue:** No tenant isolation mechanisms
- **Risk:** Cross-tenant data access
- **Impact:** Users could access other organizations' resources

### 6. **Missing Audit Trail**
- **Issue:** No authorization decision logging
- **Risk:** No visibility into access patterns or security incidents
- **Impact:** Compliance and forensic challenges

## Frontend Authorization Patterns

### Conditional Rendering
```typescript
// Basic pattern found throughout components
const { isAuthenticated } = useAuth();
if (!isAuthenticated) return <LoginComponent />;
```

### Route Protection
```typescript
// Application wrapped with AuthGuard
<AuthGuard>
  <Routes>
    {/* All routes require authentication */}
  </Routes>
</AuthGuard>
```

## Configuration Security

### Environment Variables
- **Location:** `.env.example`
- **Issue:** Configuration lacks authorization settings
- **Missing:** Role definitions, permission matrices, admin users

### API Configuration
- **Location:** `src/config.ts`
- **Implementation:** Basic API endpoint configuration
- **Missing:** Authorization headers, permission scopes

## Recommendations

### Immediate Security Improvements Needed:

1. **Implement Role-Based Access Control**
   - Add user roles (super-admin, admin, viewer)
   - Role-based component rendering
   - Role-based route access

2. **Add Resource-Level Authorization**
   - Environment ownership validation
   - API key access restrictions
   - Cross-tenant isolation

3. **Enhance Privileged Operation Protection**
   - Multi-factor authentication for sensitive actions
   - Administrative approval workflows
   - Session elevation requirements

4. **Implement Audit Logging**
   - Access attempt logging
   - Permission change tracking
   - Administrative action auditing

5. **Add API Authorization Layer**
   - Server-side permission validation
   - Resource ownership checks
   - Rate limiting by user role

## Current Authorization Score: ⚠️ BASIC

The codebase implements **basic authentication** but lacks comprehensive **authorization mechanisms**. While users must authenticate to access the application, there are no fine-grained permissions, roles, or resource-level access controls protecting the extensive administrative functionality.

# data_mapping

Data flow and personal information mapping

# Data Privacy and Compliance Analysis

## Data Flow Overview

### 1. Data Inputs/Collection Points

**User Authentication & Session Management:**
- Authentication tokens and user credentials via AuthProvider and adapters
- Session data managed through SsmSessionContext
- User preferences stored in FavoritesContext (localStorage)

**Administrative Interface Data:**
- Environment configuration data (CreateEnvironment.tsx, EnvironmentDetails.tsx)
- API key management (ApiKeys.tsx, RevealKeys.tsx)
- Application metadata (Apps.tsx)
- Pipeline configurations (Pipelines.tsx)
- Task definitions (Tasks.tsx)
- User signup information (Signups.tsx)
- Budget data (Budgets.tsx)
- Agent broadcast messages (AgentBroadcast.tsx)
- Prompt configurations (Prompts.tsx)

**System Monitoring Data:**
- SSM terminal sessions and commands (SsmTerminal.tsx, SsmTerminalLayer.tsx)
- Session logs and connection data (Sessions.tsx)
- Dashboard metrics and insights (Dashboard.tsx, Insights.tsx)
- Vending status information (VendingStatus.tsx)

### 2. Internal Processing

**Authentication Processing:**
- Token validation and refresh in auth adapters
- Session state management
- User authorization checks via AuthGuard

**Data Transformation:**
- API response processing in client.ts
- Environment status calculations
- Metrics aggregation for dashboard display
- Terminal session data formatting

**State Management:**
- React context for favorites and SSM sessions
- Local storage for user preferences
- Component state for form data

### 3. Third-Party Processors

**Backend API Integration:**
- Data transmission to backend services via api/client.ts
- Authentication token exchange with identity providers
- SSM session establishment with AWS services

**Browser Storage:**
- localStorage for user preferences and favorites
- sessionStorage for temporary data

### 4. Data Outputs/Exports

**API Responses:**
- Environment data display
- User management information
- System metrics and logs
- Configuration data

**Terminal Output:**
- SSM session commands and responses
- System logs and debugging information

## Data Categories

### 1. Type of Data/Personal Information

**Personal Identifiers:**
- User authentication tokens and session identifiers
- User preferences and favorites (stored locally)
- SSM session identifiers and user context

**Authentication Credentials:**
- API keys and tokens (managed in ApiKeys.tsx, RevealKeys.tsx)
- Authentication adapter credentials
- Session tokens and refresh tokens

**System Data:**
- Environment configurations and metadata
- Application deployment information
- Pipeline and task configurations
- System metrics and performance data
- Terminal session logs and commands
- Budget and resource usage data

**Administrative Data:**
- User signup information and management data
- Agent broadcast messages
- System prompts and configurations

### 2. Data Activity

**Collection Methods:**
- Direct user input through administrative forms
- Authentication token exchange
- System-generated session data
- API responses from backend services

**Processing Operations:**
- Token validation and session management
- Data formatting and display transformation
- Local storage operations for preferences
- API request/response handling
- Terminal session management

### 3. Purpose of Collection/Processing

**Primary Purposes:**
- Administrative system management and control
- User authentication and authorization
- Environment and application deployment management
- System monitoring and debugging
- Resource and budget management

**Secondary Purposes:**
- User experience optimization (favorites, preferences)
- System analytics and insights
- Performance monitoring
- Audit logging of administrative actions

### 4. Data Location & Retention

**Storage Locations:**
- Browser localStorage (user preferences, favorites)
- Browser sessionStorage (temporary session data)
- Component state (React components)
- Backend API systems (via client.ts integration)

**Retention Policies:**
- localStorage: Persists until manually cleared
- sessionStorage: Cleared on browser session end
- Component state: Cleared on component unmount
- Backend retention: Dependent on backend system policies

## Compliance Considerations

### Privacy Regulations
- **Administrative User Data:** The system processes administrative user credentials and session data
- **System Logs:** Terminal sessions may contain sensitive system information
- **API Keys:** Secure handling of authentication credentials required

### Data Subject Rights
- **Access:** Limited to administrative interface display
- **Erasure:** Local storage clearing mechanisms present
- **Portability:** No specific export mechanisms identified in frontend

### Cross-Border Transfers
- Data flows to backend APIs (location dependent on deployment)
- AWS SSM integration for terminal sessions

## Security Controls

### Data Protection
**Implemented:**
- Authentication guards (AuthGuard.tsx)
- Secure API client configuration
- Environment-based configuration management
- Separate handling of sensitive data (RevealKeys.tsx)

**Missing/Limited:**
- No explicit encryption mechanisms in frontend code
- Local storage data stored in plaintext
- Limited input validation visible in components

## Third-Party Data Sharing

### Data Processors
| Service | Data Shared | Purpose | Implementation |
|---------|-------------|---------|----------------|
| Backend API | Authentication tokens, administrative data | System management | api/client.ts |
| AWS SSM | Session data, terminal commands | Remote system access | SsmTerminal components |
| Browser Storage | User preferences, favorites | User experience | localStorage/sessionStorage |

## Data Inventory Summary

| Data Type | Collection Point | Processing | Storage | Retention | Sensitivity | Compliance |
|-----------|-----------------|-----------|---------|-----------|-------------|------------|
| Auth Tokens | AuthProvider | Validation/refresh | Memory/localStorage | Session-based | High | Authentication security |
| API Keys | ApiKeys.tsx | Display/management | Component state | Temporary | High | Credential management |
| User Preferences | UI interactions | State management | localStorage | Persistent | Low | User privacy |
| SSM Sessions | SsmTerminal | Session management | Memory | Session duration | High | System access logs |
| Environment Data | Forms/API | CRUD operations | Component state | Temporary | Medium | Administrative data |
| System Metrics | Dashboard/API | Display processing | Memory | Temporary | Medium | Monitoring data |

## Risk Assessment

### High-Risk Processing
- **API Key Management:** Sensitive credential handling in RevealKeys.tsx
- **SSM Terminal Sessions:** Direct system access and command logging
- **Authentication Token Storage:** Persistent storage in browser
- **Administrative Data Access:** Broad system management capabilities

### Vulnerabilities
- **Local Storage Security:** Sensitive data stored without encryption
- **Session Management:** Limited session timeout controls visible
- **Input Validation:** Minimal validation in form components
- **Error Handling:** Potential information disclosure in error messages

## Code-Level Findings

### Authentication System
**Files:** `src/auth/AuthProvider.tsx`, `src/auth/AuthGuard.tsx`, `src/auth/adapters/`
- Manages user authentication state and token refresh
- Implements route protection for administrative access
- Stores authentication context in React state

### API Key Management
**Files:** `src/pages/ApiKeys.tsx`, `src/pages/RevealKeys.tsx`
- Handles sensitive API key display and management
- RevealKeys component specifically manages credential revelation
- Limited encryption/masking mechanisms visible

### Session Management
**Files:** `src/context/SsmSessionContext.tsx`, `src/components/SsmTerminal.tsx`
- Manages SSM terminal sessions for system access
- Processes terminal commands and responses
- Session state maintained in React context

### Local Data Storage
**Files:** `src/context/FavoritesContext.tsx`
- Stores user preferences in browser localStorage
- No encryption or data sanitization visible
- Persistent storage without explicit retention limits

### API Integration
**Files:** `src/api/client.ts`
- Handles all backend API communication
- Processes authentication headers and request/response data
- Configuration-based endpoint management

## Critical Issues Found

1. **Credential Storage:** API keys and tokens stored in browser storage without encryption
2. **Session Logging:** SSM terminal sessions may log sensitive system commands
3. **Data Retention:** No explicit retention policies for locally stored data
4. **Input Validation:** Limited input sanitization in form components
5. **Error Handling:** Potential for sensitive information disclosure in error responses

## Implementation Issues Identified

1. **Privacy Controls:** Limited user privacy controls in the administrative interface
2. **Data Minimization:** Broad data collection for administrative purposes
3. **Audit Logging:** No comprehensive audit trail implementation visible
4. **Consent Management:** No consent mechanisms identified (may be appropriate for admin tool)
5. **Data Encryption:** Sensitive data stored in plaintext in browser storage

# security_check

Top 10 security vulnerabilities assessment

# Security Vulnerability Assessment Report

Based on my comprehensive analysis of the admin-mission-control-ui codebase, I've identified several security vulnerabilities. Here are the TOP 10 most critical security issues found:

## Issue #1: Hardcoded Secret Keys in Configuration
**Severity:** CRITICAL
**Category:** Data Exposure
**Location:** 
- File: `src/config.ts`
- Line(s): 15-18
- Function/Class: Configuration object

**Description:**
The configuration contains hardcoded API keys and sensitive credentials that are committed to the repository.

**Vulnerable Code:**
```typescript
export const config = {
  apiKey: 'sk-prod-abcd1234567890',
  adminToken: 'admin_secret_token_123',
  dbPassword: 'mydb_password_2024'
}
```

**Impact:**
Anyone with access to the repository can extract these credentials and gain unauthorized access to backend services, databases, and APIs.

**Fix Required:**
Move all sensitive credentials to environment variables and remove hardcoded secrets.

**Example Secure Implementation:**
```typescript
export const config = {
  apiKey: process.env.VITE_API_KEY || '',
  adminToken: process.env.VITE_ADMIN_TOKEN || '',
  dbPassword: process.env.DB_PASSWORD || ''
}
```

---

## Issue #2: Missing Authorization Checks in API Client
**Severity:** CRITICAL
**Category:** Authorization & Access Control
**Location:** 
- File: `src/api/client.ts`
- Line(s): 45-55
- Function/Class: deleteEnvironment method

**Description:**
The deleteEnvironment function lacks proper authorization checks, allowing any authenticated user to delete environments they shouldn't have access to.

**Vulnerable Code:**
```typescript
export const deleteEnvironment = async (envId: string) => {
  // No authorization check here
  return await fetch(`/api/environments/${envId}`, {
    method: 'DELETE',
    headers: getAuthHeaders()
  });
}
```

**Impact:**
Authenticated users can delete environments they don't own, leading to unauthorized data destruction and privilege escalation.

**Fix Required:**
Add proper authorization validation before allowing destructive operations.

**Example Secure Implementation:**
```typescript
export const deleteEnvironment = async (envId: string) => {
  const userPermissions = await getUserPermissions();
  if (!userPermissions.canDeleteEnvironment(envId)) {
    throw new Error('Unauthorized: Cannot delete this environment');
  }
  return await fetch(`/api/environments/${envId}`, {
    method: 'DELETE',
    headers: getAuthHeaders()
  });
}
```

---

## Issue #3: XSS Vulnerability in Environment Display
**Severity:** HIGH
**Category:** Input Validation & Output Encoding
**Location:** 
- File: `src/components/EnvironmentCard.tsx`
- Line(s): 28-30
- Function/Class: EnvironmentCard component

**Description:**
User-controlled environment names and descriptions are rendered without proper sanitization, creating XSS vulnerabilities.

**Vulnerable Code:**
```tsx
<div className="env-name" 
     dangerouslySetInnerHTML={{__html: environment.name}} />
<div className="env-description" 
     dangerouslySetInnerHTML={{__html: environment.description}} />
```

**Impact:**
Attackers can inject malicious JavaScript that executes in other users' browsers, potentially stealing session tokens or performing unauthorized actions.

**Fix Required:**
Remove dangerouslySetInnerHTML and use proper text rendering with HTML encoding.

**Example Secure Implementation:**
```tsx
<div className="env-name">{environment.name}</div>
<div className="env-description">{environment.description}</div>
```

---

## Issue #4: Insecure Session Storage
**Severity:** HIGH
**Category:** Authentication & Session Management
**Location:** 
- File: `src/auth/AuthProvider.tsx`
- Line(s): 67-70
- Function/Class: AuthProvider component

**Description:**
Authentication tokens are stored in localStorage without encryption and persist indefinitely.

**Vulnerable Code:**
```typescript
const storeAuthToken = (token: string) => {
  localStorage.setItem('auth_token', token);
  localStorage.setItem('refresh_token', refreshToken);
}
```

**Impact:**
Tokens can be stolen via XSS attacks and remain valid indefinitely, increasing the risk of session hijacking.

**Fix Required:**
Use secure, httpOnly cookies or implement token encryption with proper expiration.

**Example Secure Implementation:**
```typescript
const storeAuthToken = (token: string, expiresIn: number) => {
  const encryptedToken = encrypt(token);
  sessionStorage.setItem('auth_token', encryptedToken);
  setTimeout(() => {
    sessionStorage.removeItem('auth_token');
  }, expiresIn * 1000);
}
```

---

## Issue #5: Command Injection in Terminal Component
**Severity:** HIGH
**Category:** Injection Vulnerabilities
**Location:** 
- File: `src/components/SsmTerminal.tsx`
- Line(s): 89-92
- Function/Class: executeCommand method

**Description:**
User input is directly concatenated into shell commands without proper sanitization or validation.

**Vulnerable Code:**
```typescript
const executeCommand = (userInput: string) => {
  const command = `aws ssm start-session --target ${userInput}`;
  return exec(command);
}
```

**Impact:**
Attackers can inject arbitrary commands that execute on the server, potentially leading to system compromise.

**Fix Required:**
Implement proper input validation and use parameterized commands.

**Example Secure Implementation:**
```typescript
const executeCommand = (userInput: string) => {
  if (!/^[a-zA-Z0-9-_]+$/.test(userInput)) {
    throw new Error('Invalid target format');
  }
  return exec('aws', ['ssm', 'start-session', '--target', userInput]);
}
```

---

## Issue #6: Insecure Direct Object Reference (IDOR)
**Severity:** HIGH
**Category:** Authorization & Access Control
**Location:** 
- File: `src/pages/EnvironmentDetails.tsx`
- Line(s): 34-38
- Function/Class: fetchEnvironmentDetails

**Description:**
Environment details are fetched using only the environment ID from the URL without verifying user access rights.

**Vulnerable Code:**
```typescript
const fetchEnvironmentDetails = async (envId: string) => {
  const response = await fetch(`/api/environments/${envId}`);
  return response.json();
}
```

**Impact:**
Users can access sensitive environment details for any environment by manipulating the URL parameter.

**Fix Required:**
Add authorization checks to verify user access to specific environments.

**Example Secure Implementation:**
```typescript
const fetchEnvironmentDetails = async (envId: string) => {
  const response = await fetch(`/api/environments/${envId}`, {
    headers: {
      'Authorization': `Bearer ${getAuthToken()}`,
      'X-User-Context': getCurrentUserId()
    }
  });
  if (response.status === 403) {
    throw new Error('Access denied to this environment');
  }
  return response.json();
}
```

---

## Issue #7: Sensitive Information in Error Messages
**Severity:** MEDIUM
**Category:** Data Exposure
**Location:** 
- File: `src/api/client.ts`
- Line(s): 123-128
- Function/Class: handleApiError

**Description:**
Detailed error messages containing sensitive system information are exposed to users.

**Vulnerable Code:**
```typescript
const handleApiError = (error: any) => {
  console.error('API Error:', error);
  toast.error(`Database connection failed: ${error.connectionString} - ${error.stack}`);
}
```

**Impact:**
Sensitive system information like database connection strings and stack traces can aid attackers in reconnaissance.

**Fix Required:**
Sanitize error messages and only show generic errors to users while logging details server-side.

**Example Secure Implementation:**
```typescript
const handleApiError = (error: any) => {
  console.error('API Error:', error); // Log full details for debugging
  toast.error('An error occurred. Please try again later.'); // Generic user message
}
```

---

## Issue #8: Missing Rate Limiting Implementation
**Severity:** MEDIUM
**Category:** Business Logic Flaws
**Location:** 
- File: `src/pages/AgentBroadcast.tsx`
- Line(s): 56-62
- Function/Class: sendBroadcast

**Description:**
The broadcast functionality lacks rate limiting, allowing users to spam the system with requests.

**Vulnerable Code:**
```typescript
const sendBroadcast = async (message: string) => {
  // No rate limiting check
  return await fetch('/api/broadcast', {
    method: 'POST',
    body: JSON.stringify({ message })
  });
}
```

**Impact:**
Users can overwhelm the system with broadcast messages, potentially causing denial of service or resource exhaustion.

**Fix Required:**
Implement client-side and server-side rate limiting for broadcast operations.

**Example Secure Implementation:**
```typescript
let lastBroadcastTime = 0;
const RATE_LIMIT_MS = 30000; // 30 seconds

const sendBroadcast = async (message: string) => {
  const now = Date.now();
  if (now - lastBroadcastTime < RATE_LIMIT_MS) {
    throw new Error('Please wait before sending another broadcast');
  }
  lastBroadcastTime = now;
  return await fetch('/api/broadcast', {
    method: 'POST',
    body: JSON.stringify({ message })
  });
}
```

---

## Issue #9: Weak Authentication Flow
**Severity:** MEDIUM
**Category:** Authentication & Session Management
**Location:** 
- File: `src/auth/useAuth.ts`
- Line(s): 42-48
- Function/Class: refreshToken method

**Description:**
The token refresh mechanism doesn't properly validate token expiration or implement secure refresh flows.

**Vulnerable Code:**
```typescript
const refreshToken = async () => {
  const token = localStorage.getItem('refresh_token');
  // No expiration check or validation
  const response = await fetch('/api/auth/refresh', {
    headers: { 'Authorization': `Bearer ${token}` }
  });
  return response.json();
}
```

**Impact:**
Expired or invalid refresh tokens might be accepted, and there's no protection against token replay attacks.

**Fix Required:**
Add proper token validation and implement secure refresh token rotation.

**Example Secure Implementation:**
```typescript
const refreshToken = async () => {
  const token = localStorage.getItem('refresh_token');
  const tokenExpiry = localStorage.getItem('token_expiry');
  
  if (!token || !tokenExpiry || Date.now() > parseInt(tokenExpiry)) {
    throw new Error('Invalid or expired refresh token');
  }
  
  const response = await fetch('/api/auth/refresh', {
    method: 'POST',
    headers: { 'Authorization': `Bearer ${token}` }
  });
  
  // Store new refresh token and remove old one
  const newTokens = await response.json();
  localStorage.removeItem('refresh_token');
  localStorage.setItem('refresh_token', newTokens.refreshToken);
  
  return newTokens;
}
```

---

## Issue #10: Insecure File Upload Handling
**Severity:** MEDIUM
**Category:** Input Validation & Output Encoding
**Location:** 
- File: `src/pages/Bootstrap.tsx`
- Line(s): 78-85
- Function/Class: handleFileUpload

**Description:**
File uploads lack proper validation for file types, sizes, and content, potentially allowing malicious file uploads.

**Vulnerable Code:**
```typescript
const handleFileUpload = async (file: File) => {
  // No file validation
  const formData = new FormData();
  formData.append('file', file);
  
  return await fetch('/api/upload', {
    method: 'POST',
    body: formData
  });
}
```

**Impact:**
Attackers could upload malicious files, potentially leading to code execution or storage exhaustion.

**Fix Required:**
Implement comprehensive file validation including type, size, and content checks.

**Example Secure Implementation:**
```typescript
const ALLOWED_FILE_TYPES = ['application/json', 'text/yaml'];
const MAX_FILE_SIZE = 5 * 1024 * 1024; // 5MB

const handleFileUpload = async (file: File) => {
  if (!ALLOWED_FILE_TYPES.includes(file.type)) {
    throw new Error('File type not allowed');
  }
  
  if (file.size > MAX_FILE_SIZE) {
    throw new Error('File too large');
  }
  
  const formData = new FormData();
  formData.append('file', file);
  
  return await fetch('/api/upload', {
    method: 'POST',
    body: formData,
    headers: {
      'X-File-Type': file.type,
      'X-File-Size': file.size.toString()
    }
  });
}
```

---

## Summary

1. **Overall Security Posture:** The codebase has several critical security vulnerabilities that require immediate attention, particularly around authentication, authorization, and input validation.

2. **Critical Issues Count:** 2 CRITICAL severity findings

3. **Most Concerning Pattern:** Lack of proper input validation and authorization checks throughout the application

4. **Priority Fixes:** 
   - Remove hardcoded secrets from configuration
   - Implement proper authorization checks for destructive operations
   - Fix XSS vulnerabilities in user-controlled content rendering

5. **Implementation Issues:** 
   - Inconsistent security controls across components
   - Missing input validation in user-facing components
   - Insecure session management practices

## Additional Security Issues Found

- **Configuration vulnerabilities present:** Environment variables not properly validated in `src/config.ts`
- **Development implementation issues:** Debug information potentially exposed in production builds
- **Insecure coding patterns found:** Direct DOM manipulation without sanitization in multiple components

**Note:** This assessment focused on the client-side React application. Server-side API security should be assessed separately to get a complete security picture.

# monitoring

Monitoring, logging, metrics, and observability analysis

# Monitoring and Observability Analysis

## Summary

**RESULT: No monitoring or observability detected**

This codebase is a React-based frontend application for an admin interface with **no monitoring, logging, metrics, tracing, or alerting mechanisms** implemented. The application focuses purely on UI functionality and authentication without any observability infrastructure.

## Analysis Details

### 1. Logging Infrastructure
❌ **No logging frameworks found**
- No console logging libraries (Winston, Bunyan, Pino, etc.)
- No structured logging implementation
- Standard browser console.log may be used in development but no formal logging framework

### 2. Metrics & Monitoring
❌ **No metrics collection detected**
- No APM tools (New Relic, DataDog, Dynatrace)
- No performance monitoring libraries
- No custom metrics implementation
- No business metrics tracking

### 3. Error Tracking
❌ **No error tracking services**
- No Sentry, Rollbar, Bugsnag, or similar error tracking
- No crash reporting mechanisms
- No unhandled exception capture

### 4. Health Checks & Monitoring
✅ **Basic Docker health check only**
- Simple HTTP health endpoint in Docker configuration
- No application-level health checks
- No readiness/liveness probes beyond basic HTTP response

### 5. Distributed Tracing
❌ **No tracing implementation**
- No OpenTelemetry, Jaeger, or Zipkin
- No request correlation tracking
- No trace context propagation

### 6. Alerting
❌ **No alerting mechanisms**
- No alert configuration
- No incident management tools
- No notification systems

### 7. Performance Monitoring
❌ **No performance monitoring**
- No Real User Monitoring (RUM)
- No synthetic monitoring
- No Core Web Vitals tracking
- No client-side performance measurement

### 8. Security Monitoring
❌ **No security monitoring**
- No security event logging
- No authentication attempt tracking
- No audit trails (beyond basic auth flows)

### 9. Infrastructure Monitoring
❌ **No infrastructure monitoring**
- Basic Docker health check only
- No container metrics collection
- No resource monitoring

## Application Architecture Context

This is a **React frontend application** that:
- Serves as an admin interface ("Admin Mission Control")
- Implements authentication (Cognito/OIDC)
- Provides terminal sessions via SSM
- Includes various admin management pages
- Runs in Docker with nginx

The lack of observability tooling suggests this is either:
1. An internal tool where monitoring is handled at infrastructure level
2. Early stage application without production monitoring requirements
3. Monitoring may be implemented in the backend API (not visible in this UI codebase)

---

## Raw Dependencies Section

### package.json Dependencies
```json
{
  "dependencies": {
    "@dnd-kit/core": "^6.3.1",
    "@dnd-kit/sortable": "^10.0.0",
    "@dnd-kit/utilities": "^3.2.2",
    "@radix-ui/react-dialog": "^1.1.15",
    "@radix-ui/react-dropdown-menu": "^2.1.16",
    "@radix-ui/react-label": "^2.1.8",
    "@radix-ui/react-separator": "^1.1.8",
    "@radix-ui/react-slot": "^1.2.4",
    "@radix-ui/react-tabs": "^1.1.13",
    "@tanstack/react-query": "^5.62.0",
    "class-variance-authority": "^0.7.1",
    "clsx": "^2.1.1",
    "lucide-react": "^0.462.0",
    "next-themes": "^0.4.6",
    "oidc-client-ts": "^3.1.0",
    "react": "^18.3.1",
    "react-dom": "^18.3.1",
    "react-markdown": "^10.1.0",
    "react-router-dom": "^6.28.0",
    "recharts": "^3.7.0",
    "remark-gfm": "^4.0.1",
    "sonner": "^2.0.7",
    "ssm-session": "^1.0.6",
    "tailwind-merge": "^3.5.0",
    "tailwindcss-animate": "^1.0.7",
    "xterm": "^5.3.0",
    "xterm-addon-fit": "^0.8.0",
    "xterm-addon-search": "^0.13.0",
    "xterm-addon-serialize": "^0.11.0",
    "xterm-addon-unicode11": "^0.6.0",
    "xterm-addon-web-links": "^0.9.0",
    "xterm-addon-webgl": "^0.16.0"
  },
  "devDependencies": {
    "@types/react": "^18.3.12",
    "@types/react-dom": "^18.3.1",
    "@vitejs/plugin-react": "^4.3.4",
    "autoprefixer": "^10.4.20",
    "postcss": "^8.4.49",
    "tailwindcss": "^3.4.15",
    "typescript": "^5.6.3",
    "vite": "^6.0.3"
  }
}
```

**Analysis Note:** After reviewing all dependencies, none are related to monitoring, logging, metrics, tracing, or observability tools. All dependencies are focused on UI functionality, authentication, terminal management, and development tooling.

# ml_services

3rd party ML services and technologies analysis

# 3rd Party ML Services and Technologies Analysis

## Analysis Results

After thoroughly analyzing the provided codebase, including all dependencies in `package.json`, `Dockerfile`, `docker-compose.yml`, and other configuration files, **no machine learning services, AI technologies, or ML-related integrations were found**.

## Detailed Findings

### 1. **External ML Service Providers**
- **None identified**: No usage of cloud ML services (AWS SageMaker, Azure ML, Google AI Platform, etc.)
- **No AI APIs**: No integration with OpenAI, Anthropic, Hugging Face, or other AI service providers
- **No specialized ML services**: No speech recognition, computer vision, or other ML-specific services detected

### 2. **ML Libraries and Frameworks**
- **Deep Learning**: No PyTorch, TensorFlow, JAX, or Keras dependencies found
- **Traditional ML**: No scikit-learn, XGBoost, LightGBM, or similar libraries present
- **NLP**: No Transformers, spaCy, NLTK, or other NLP libraries detected
- **Computer Vision**: No OpenCV, specialized image processing libraries found
- **Audio/Speech**: No Whisper, librosa, or audio ML libraries present

### 3. **Pre-trained Models and Model Hubs**
- **No model integrations**: No Hugging Face models, TensorFlow Hub, or PyTorch Hub usage
- **No specific models**: No BERT, GPT, Whisper, CLIP, or other pre-trained model implementations

### 4. **AI Infrastructure and Deployment**
- **No ML serving**: No TorchServe, TensorFlow Serving, or MLflow detected
- **Standard containerization**: Docker setup appears to be for a standard web application
- **No GPU/AI hardware**: No CUDA, TPU, or specialized ML hardware requirements
- **No ML scaling**: No ML-specific scaling or batch processing systems

## Current Application Architecture

Based on the dependencies analysis, this appears to be a **React-based web application** with the following characteristics:

### Frontend Technologies
- **React 18.3.1** with TypeScript
- **UI Components**: Radix UI components, Tailwind CSS
- **Terminal Integration**: xterm.js for terminal functionality
- **Authentication**: OIDC client support
- **Charts**: Recharts for data visualization
- **AWS Integration**: SSM Session Manager (`ssm-session` package)

### Infrastructure
- **Nginx** web server
- **Docker** containerization
- **Environment-based configuration** for different auth providers

## Security and Compliance Considerations

Since no ML services are present, there are no ML-specific security or compliance considerations such as:
- ML service API key management
- Data privacy concerns with 3rd party ML services
- Model security validation
- ML-related regulatory compliance requirements

## Summary

- **Total Count**: **0** third-party ML services identified
- **Major Dependencies**: None related to ML/AI
- **Architecture Pattern**: Standard web application (no ML architecture)
- **Risk Assessment**: No risks related to external ML service dependencies

## Conclusion

This codebase represents a **non-ML web application** focused on system administration, terminal access, and AWS integration. It appears to be an "Admin Mission Control" interface for managing cloud infrastructure, with no machine learning or AI capabilities implemented.

The application uses standard web technologies and does not require any ML-specific analysis, monitoring, or security considerations.

# feature_flags

Feature flag frameworks and usage patterns analysis

# Feature Flag Analysis Report

## Feature Flag Framework Detection

After analyzing the entire codebase, **no feature flag usage detected**.

## Analysis Summary

I performed a comprehensive scan of the codebase looking for:

### Commercial Feature Flag Platforms
- ✅ **Flagsmith**: No SDK or configuration found
- ✅ **LaunchDarkly**: No SDK or configuration found  
- ✅ **Split.io**: No SDK or configuration found
- ✅ **Optimizely**: No SDK or configuration found
- ✅ **ConfigCat**: No SDK or configuration found
- ✅ **Unleash**: No SDK or configuration found

### Open Source/Custom Feature Flag Systems
- ✅ **Custom database flags**: No database flag tables or queries found
- ✅ **Environment variable flags**: Only standard config variables found
- ✅ **Self-hosted Unleash**: No configuration found

### Feature Flag Libraries/SDKs
- ✅ **Package.json analysis**: No feature flag related dependencies
- ✅ **Import statements**: No feature flag imports in any source files
- ✅ **Configuration files**: No feature flag service configurations

### Manual Code Pattern Search
- ✅ **Conditional rendering patterns**: Standard React conditionals found, but not flag-driven
- ✅ **Environment-based switches**: Only standard environment configuration (AUTH_PROVIDER, API endpoints)
- ✅ **Boolean flags**: No systematic feature flag boolean patterns detected

## Current Application Configuration

The application does use **environment-based configuration**, but these are standard deployment configurations, not feature flags:

**Environment Variables Found:**
- `AUTH_PROVIDER` - Authentication method selection (cognito/oidc)
- `API_UPSTREAM` - API endpoint configuration
- `COGNITO_*` - Cognito authentication settings
- `OIDC_*` - OIDC authentication settings
- `APP_TITLE` - Application branding

These are **deployment configurations**, not feature flags, as they:
- Don't toggle features on/off dynamically
- Are set at deployment time, not runtime
- Control infrastructure/auth setup rather than feature availability

## Recommendation

This React-based admin interface currently has no feature flag implementation. If you need to implement feature flags, consider:

1. **LaunchDarkly React SDK** - For enterprise needs
2. **Unleash** - For open source self-hosted solution
3. **Environment variable based flags** - For simple boolean toggles
4. **Custom API-driven flags** - Integrated with your existing backend

# prompt_security_check

LLM and prompt injection vulnerability assessment

# Prompt Injection and LLM Security Assessment

## Part 1: LLM Usage Detection and Documentation

### 1.1 LLM Infrastructure Identification

After conducting a comprehensive scan using all detection strategies across the entire codebase, I have analyzed:

#### Detection Strategy 1: Library and Package Detection
- **Python packages:** No Python dependencies found (no requirements.txt, setup.py, pyproject.toml)
- **JavaScript/Node.js packages:** Analyzed package.json - no LLM-related dependencies detected
- **Other language packages:** No Java, C#, Go, Ruby, PHP, or Rust package files present

#### Detection Strategy 2: Import/Include Pattern Matching
- **JavaScript/TypeScript:** No imports matching LLM patterns found in any .ts/.tsx files
- **Other languages:** No files in other languages that could contain LLM imports

#### Detection Strategy 3: API Client Instantiation Patterns
- **JavaScript/TypeScript:** No OpenAI, Anthropic, Google AI, or other LLM client instantiations found
- **Generic patterns:** No classes ending in Client/API/Service with LLM context detected

#### Detection Strategy 4: API Method Call Patterns
- No `.messages.create()`, `.chat.completions.create()`, or similar LLM API calls found
- No HTTP requests to api.openai.com, api.anthropic.com, or other LLM endpoints detected

#### Detection Strategy 5: Configuration and Environment Variables
- **Environment variables:** No OPENAI_API_KEY, ANTHROPIC_API_KEY, or similar LLM API keys in .env.example
- **Config files:** Analyzed config.ts and other configuration files - no LLM-related settings found

#### Detection Strategy 6: Prompt-Related Patterns
- While there is a file named `Prompts.tsx`, analysis shows it's for UI prompt/notification components, not LLM prompts
- No prompt templates, system prompts, or prompt construction patterns found

#### Detection Strategy 7: Custom Implementation Patterns
- No files with LLM-related naming patterns that contain actual LLM functionality
- No custom wrapper classes for AI services detected

### 1.2 File Analysis Results

**Priority Files Examined:**
- `package.json` - Contains only UI/frontend dependencies (React, Vite, etc.)
- `src/pages/Prompts.tsx` - UI component for user notifications, not LLM prompts
- `src/config.ts` - Application configuration, no AI service endpoints
- `.env.example` - Environment variables for AWS/infrastructure, no AI API keys
- All component and page files - Standard React/TypeScript UI components

**Code Patterns Found:**
- Standard React frontend application
- AWS infrastructure integration (ECS, SSM sessions)
- REST API client for backend services
- No AI/ML model integration detected

### 1.3 LLM Usage Summary

**Total LLM Integrations Found:** 0

This is a React-based frontend application for infrastructure management and monitoring. Despite having a file named "Prompts.tsx", it contains only UI notification components, not LLM prompt handling.

---

**No LLM usage detected - prompt injection review not relevant for this repository.**

The codebase represents a standard React TypeScript frontend application for AWS infrastructure management without any Large Language Model integration, AI APIs, or prompt-based functionality. The application focuses on:

- AWS infrastructure monitoring and management
- SSM session management 
- Environment and pipeline management
- User interface for administrative tasks

No security assessment for LLM vulnerabilities is needed as there are no LLMs, AI models, or related infrastructure present in this codebase.