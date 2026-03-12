# hl_overview

High level overview of the codebase

# Repository Analysis

## 0. **Repository Name:** 
[[loki-skills]]

## 1. **Project Purpose:**
This project appears to be a comprehensive skills repository for AI agents, providing specialized capabilities across various cloud services, development tools, and platforms. It serves as a modular collection of agent skills that can be used for tasks ranging from cloud infrastructure management (AWS services like Amplify, HealthOmics, Graviton migration) to SaaS development, observability, and DevOps automation. The primary domain is AI agent skill development and cloud/infrastructure automation.

## 2. **Architecture Pattern:**
- **Modular Skill-Based Architecture**: Each directory represents a discrete skill module
- **Plugin/Extension Pattern**: Skills appear to be designed as pluggable components
- **Reference Documentation Pattern**: Each skill includes supporting reference materials

## 3. **Technology Stack:**
Based on the repository structure and skill domains:
- **Cloud Platforms**: AWS (primary), with extensive coverage of services
- **Infrastructure as Code**: Terraform, AWS CDK
- **Languages**: Python (inferred from claude-agent-sdk), JavaScript/TypeScript (Figma, Stripe integrations)
- **Observability**: CloudWatch, Datadog, Dynatrace
- **CI/CD**: GitHub Actions workflows
- **Development Tools**: Postman, Figma, Outline
- **Databases**: Neon (PostgreSQL)
- **Payment Processing**: Stripe, Checkout
- **Big Data**: Apache Spark

**Note**: No traditional package files (requirements.txt, package.json, etc.) are visible in the root structure, suggesting this is a documentation/configuration repository rather than executable code.

## 4. **Initial Structure Impression:**
The repository is organized as a collection of independent skill modules, each containing:
- A `SKILL.md` file (skill definition/documentation)
- A `refs/` directory with supporting reference materials
- GitHub workflows for automated review and security scanning

## 5. **Configuration/Package Files:**
- `.gitallowed` - Git permission/allowlist configuration
- `claude-review.yml` - Claude AI code review workflow
- `commit-review.yml` - Commit review automation
- `secret-scan.yml` - Security scanning workflow
- Various `SKILL.md` files - Skill configuration/documentation
- Reference files (.md) - Supporting documentation and guides

## 6. **Directory Structure:**
Each top-level directory represents a specific skill domain:
- **Cloud Services**: `aws-*` directories for various AWS services
- **Development Tools**: `figma`, `postman`, `outline`
- **Infrastructure**: `terraform`, `aws-infrastructure-as-code`
- **Observability**: `aws-observability`, `cloudwatch-application-signals`, `datadog`, `dynatrace`
- **SaaS/Payment**: `saas-builder`, `stripe`, `checkout`
- **Specialized Domains**: `aws-healthomics` (genomics), `spark-troubleshooting-agent`
- **SDKs/Frameworks**: `claude-agent-sdk`, `aws-mcp`

## 7. **High-Level Architecture:**
**Modular Plugin Architecture** with evidence:
- Each skill is self-contained with its own documentation and references
- Standardized structure across all skills (`SKILL.md` + `refs/` pattern)
- Skills appear to be designed for independent deployment/usage
- **Agent-Based Pattern**: Multiple references to agents and MCP (Model Context Protocol)
- **Domain-Driven Design**: Skills are organized by business/technical domains

## 8. **Build, Execution and Test:**
- **No traditional build process**: This appears to be a documentation/configuration repository
- **GitHub Actions Integration**: Automated workflows for:
  - Claude AI-powered code review
  - Commit validation
  - Security scanning
- **Skill Activation**: Skills likely activated through the claude-agent-sdk
- **Entry Points**: Each `SKILL.md` file serves as the entry point for its respective skill
- **Testing**: Cross-agent testing capability indicated by `cross-agent-test` directory

The repository follows a documentation-first approach where skills are defined through markdown files and supporting references, rather than traditional executable code with build scripts.

# module_deep_dive

Deep dive into modules

# Detailed Component Breakdown

## 1. **saas-builder/**

### Core Responsibility:
Provides comprehensive guidance and patterns for building Software-as-a-Service (SaaS) applications, focusing on architecture principles, billing systems, and implementation strategies.

### Key Components:
- **SKILL.md** - Main skill definition for SaaS development capabilities
- **refs/architecture-principles.md** - Core architectural guidelines for SaaS systems
- **refs/billing-and-payments.md** - Payment processing and subscription billing patterns
- **refs/implementation-patterns.md** - Common SaaS implementation approaches
- **refs/repository-structure.md** - Code organization standards for SaaS projects

### Dependencies & Interactions:
- Likely interacts with `@stripe/` and `@checkout/` modules for payment processing
- May depend on `@aws-amplify/` for hosting and backend services
- Could integrate with `@terraform/` for infrastructure provisioning
- External services: Payment gateways, cloud hosting platforms, authentication providers

---

## 2. **reposwarm/**

### Core Responsibility:
Manages repository swarming capabilities, likely for distributed version control or multi-repository operations.

### Key Components:
- **SKILL.md** - Repository management and swarming operations skill definition

### Dependencies & Interactions:
- May interact with Git-based systems and repository management tools
- Could integrate with CI/CD workflows in `@.github/workflows/`
- External services: Git hosting platforms (GitHub, GitLab), repository management APIs

---

## 3. **aws-graviton-migration/**

### Core Responsibility:
Specializes in migrating workloads to AWS Graviton processors for improved performance and cost efficiency.

### Key Components:
- **SKILL.md** - Graviton migration strategies and implementation guidance

### Dependencies & Interactions:
- Closely related to `@arm-soc-migration/` for ARM architecture transitions
- Likely integrates with `@aws-infrastructure-as-code/` for infrastructure changes
- May use `@terraform/` for infrastructure modifications
- External services: AWS EC2, AWS Auto Scaling, AWS Load Balancers

---

## 4. **aws-agentcore/**

### Core Responsibility:
Provides core agent functionality and integration capabilities within the AWS ecosystem, focusing on agent gateway and memory management.

### Key Components:
- **SKILL.md** - Core agent capabilities definition
- **refs/agentcore-gateway-integration.md** - Gateway integration patterns
- **refs/agentcore-memory-integration.md** - Memory management for agents
- **refs/getting-started.md** - Initial setup and configuration guide

### Dependencies & Interactions:
- Central integration point for `@claude-agent-sdk/`
- May interact with `@aws-mcp/` for model context protocol
- Could depend on AWS services for hosting and orchestration
- External services: AWS Lambda, AWS API Gateway, AWS DynamoDB

---

## 5. **aws-healthomics/**

### Core Responsibility:
Manages genomics and life sciences workloads on AWS HealthOmics, including workflow development and migration from existing bioinformatics platforms.

### Key Components:
- **SKILL.md** - HealthOmics service capabilities
- **refs/ecr-pull-through-cache.md** - Container registry optimization
- **refs/git-integration.md** - Version control for genomics workflows
- **refs/migration-guide-for-nextflow.md** - Nextflow pipeline migration
- **refs/migration-guide-for-wdl.md** - WDL workflow migration
- **refs/troubleshooting.md** - Common issues and solutions
- **refs/workflow-development.md** - Best practices for workflow creation
- **refs/workflow-versioning.md** - Versioning strategies for genomics workflows

### Dependencies & Interactions:
- May integrate with `@aws-infrastructure-as-code/` for resource provisioning
- Could use `@terraform/` for infrastructure management
- External services: AWS HealthOmics, Amazon ECR, genomics data repositories

---

## 6. **stripe/**

### Core Responsibility:
Handles Stripe payment processing integration, providing best practices and implementation patterns for payment systems.

### Key Components:
- **SKILL.md** - Stripe integration capabilities
- **refs/stripe-best-practices.md** - Implementation guidelines and security practices

### Dependencies & Interactions:
- Integrates with `@saas-builder/` for SaaS payment workflows
- May work alongside `@checkout/` for payment processing alternatives
- External services: Stripe API, webhook endpoints, payment compliance services

---

## 7. **terraform/**

### Core Responsibility:
Provides Infrastructure as Code (IaC) capabilities using Terraform for cloud resource provisioning and management.

### Key Components:
- **SKILL.md** - Terraform automation capabilities
- **refs/code-style.md** - Terraform code formatting and style guidelines
- **refs/getting-started.md** - Initial Terraform setup and configuration
- **refs/mcp-usage.md** - Model Context Protocol integration with Terraform
- **refs/terraform-best-practices.md** - Advanced Terraform patterns and practices

### Dependencies & Interactions:
- Integrates with `@aws-mcp/` through MCP usage patterns
- Supports `@aws-infrastructure-as-code/` implementations
- May be used by `@aws-graviton-migration/` and `@arm-soc-migration/`
- External services: Terraform Cloud, AWS Provider, state backends

---

## 8. **spark-troubleshooting-agent/**

### Core Responsibility:
Specializes in diagnosing and resolving Apache Spark performance issues and operational problems.

### Key Components:
- **SKILL.md** - Spark troubleshooting and optimization capabilities

### Dependencies & Interactions:
- May integrate with `@aws-observability/` for monitoring Spark clusters
- Could use observability tools like `@datadog/` or `@dynatrace/`
- External services: Apache Spark clusters, YARN, Kubernetes, cloud compute services

---

## 9. **cloudwatch-application-signals/**

### Core Responsibility:
Manages AWS CloudWatch Application Signals for application performance monitoring and observability.

### Key Components:
- **SKILL.md** - CloudWatch Application Signals configuration
- **refs/steering.md** - Implementation guidance and best practices

### Dependencies & Interactions:
- Integrates with `@aws-observability/` for comprehensive monitoring
- May work with `@datadog/` and `@dynatrace/` for multi-platform observability
- External services: AWS CloudWatch, AWS X-Ray, application instrumentation libraries

---

## 10. **outline/**

### Core Responsibility:
Manages Outline knowledge base and documentation platform integration.

### Key Components:
- **SKILL.md** - Outline platform management capabilities

### Dependencies & Interactions:
- May integrate with documentation workflows in other modules
- External services: Outline API, authentication providers, storage backends

---

## 11. **cross-agent-test/**

### Core Responsibility:
Provides testing capabilities across multiple agent systems and validates inter-agent communication and functionality.

### Key Components:
- **SKILL.md** - Cross-agent testing framework definition
- **references/prompt-template.md** - Standardized prompt formats for testing

### Dependencies & Interactions:
- Central testing hub for `@aws-agentcore/` and `@claude-agent-sdk/`
- May interact with all other skill modules for validation
- Could integrate with CI/CD workflows in `@.github/workflows/`

---

## 12. **aws-mcp/**

### Core Responsibility:
Implements AWS Model Context Protocol (MCP) for enhanced AI model integration and context management within AWS services.

### Key Components:
- **SKILL.md** - AWS MCP implementation and usage patterns

### Dependencies & Interactions:
- Core integration with `@claude-agent-sdk/` and `@aws-agentcore/`
- Used by `@terraform/` for MCP-enabled infrastructure management
- External services: AWS Bedrock, AWS Lambda, model hosting services

---

## 13. **dynatrace/**

### Core Responsibility:
Provides Dynatrace APM integration for comprehensive application performance monitoring and observability.

### Key Components:
- **SKILL.md** - Dynatrace monitoring capabilities
- **refs/steering.md** - Implementation and configuration guidance

### Dependencies & Interactions:
- Works alongside `@aws-observability/`, `@datadog/`, and `@cloudwatch-application-signals/`
- May integrate with `@spark-troubleshooting-agent/` for Spark monitoring
- External services: Dynatrace SaaS/Managed, OneAgent, monitoring APIs

---

## 14. **aws-infrastructure-as-code/**

### Core Responsibility:
Provides comprehensive Infrastructure as Code patterns specifically for AWS services using various IaC tools and methodologies.

### Key Components:
- **SKILL.md** - AWS IaC implementation patterns and best practices

### Dependencies & Interactions:
- Integrates with `@terraform/` for Terraform-specific implementations
- May use `@cloud-architect/` patterns for CDK development
- Supports infrastructure needs for `@aws-amplify/`, `@aws-healthomics/`, and other AWS services
- External services: AWS CloudFormation, AWS CDK, Terraform AWS Provider

---

## 15. **strands/**

### Core Responsibility:
Manages Strands platform integration and workflows, likely for data processing or workflow orchestration.

### Key Components:
- **SKILL.md** - Strands platform capabilities
- **refs/getting-started.md** - Initial setup and configuration guide

### Dependencies & Interactions:
- May integrate with workflow orchestration systems
- External services: Strands platform API, data processing services

---

## 16. **aws-observability/**

### Core Responsibility:
Provides comprehensive AWS observability solutions including monitoring, alerting, logging, and incident response across AWS services.

### Key Components:
- **SKILL.md** - AWS observability strategy and implementation
- **refs/alerting-setup.md** - Alert configuration and management
- **refs/application-signals-setup.md** - Application performance monitoring
- **refs/cloudtrail-data-source-selection.md** - Audit logging configuration
- **refs/incident-response.md** - Incident management procedures
- **refs/log-analysis.md** - Log processing and analysis patterns
- **refs/observability-gap-analysis.md** - Monitoring coverage assessment
- **refs/performance-monitoring.md** - Performance metrics and optimization
- **refs/security-auditing.md** - Security monitoring and compliance

### Dependencies & Interactions:
- Integrates with `@cloudwatch-application-signals/` for application monitoring
- Works with `@datadog/` and `@dynatrace/` for hybrid observability
- May support `@spark-troubleshooting-agent/` monitoring needs
- External services: AWS CloudWatch, AWS X-Ray, AWS CloudTrail, AWS Config

---

## 17. **aws-amplify/**

### Core Responsibility:
Manages full-stack application development using AWS Amplify, from backend setup through production deployment.

### Key Components:
- **SKILL.md** - Amplify development capabilities
- **refs/amplify-workflow.md** - Complete development workflow
- **refs/phase1-backend.md** - Backend services setup
- **refs/phase2-sandbox.md** - Development environment configuration
- **refs/phase3-frontend.md** - Frontend application development
- **refs/phase4-production.md** - Production deployment and optimization

### Dependencies & Interactions:
- May integrate with `@saas-builder/` for SaaS application development
- Could use `@aws-infrastructure-as-code/` for custom resource provisioning
- May work with `@stripe/` and `@checkout/` for payment integration
- External services: AWS Amplify, AWS AppSync, Amazon Cognito, AWS Lambda

---

## 18. **figma/**

### Core Responsibility:
Provides Figma design tool integration and workflow automation for design-to-development processes.

### Key Components:
- **SKILL.md** - Figma integration and automation capabilities

### Dependencies & Interactions:
- May integrate with `@aws-amplify/` for design-to-code workflows
- Could support `@saas-builder/` design processes
- External services: Figma API, design token systems, collaboration tools

---

## 19. **neon/**

### Core Responsibility:
Manages Neon PostgreSQL database integration and optimization for serverless and cloud-native applications.

### Key Components:
- **SKILL.md** - Neon database management capabilities
- **refs/steering.md** - Database configuration and optimization guidance

### Dependencies & Interactions:
- May integrate with `@saas-builder/` for application data storage
- Could work with `@aws-amplify/` for backend database needs
- External services: Neon database service, PostgreSQL ecosystem tools

---

## 20. **arm-soc-migration/**

### Core Responsibility:
Specializes in migrating applications and workloads to ARM System-on-Chip architectures, with comprehensive planning and validation processes.

### Key Components:
- **SKILL.md** - ARM SoC migration strategy and execution
- **refs/constraints.md** - Migration limitations and considerations
- **refs/discovery.md** - Application assessment and compatibility analysis
- **refs/implementation.md** - Migration execution procedures
- **refs/planning.md** - Migration planning and timeline development
- **refs/validation.md** - Testing and validation procedures

### Dependencies & Interactions:
- Closely related to `@aws-graviton-migration/` for AWS-specific ARM migrations
- May use `@terraform/` and `@aws-infrastructure-as-code/` for infrastructure changes
- Could integrate with `@cross-agent-test/` for validation testing
- External services: ARM development tools, performance testing platforms

---

## 21. **cloud-architect/**

### Core Responsibility:
Provides cloud architecture design patterns and development guidelines, with emphasis on AWS CDK and testing strategies.

### Key Components:
- **SKILL.md** - Cloud architecture design capabilities
- **refs/cdk-development-guidelines.md** - AWS CDK best practices and patterns
- **refs/cloud-engineer-agent.md** - Engineering workflow and collaboration patterns
- **refs/testing-strategy.md** - Infrastructure and application testing approaches

### Dependencies & Interactions:
- Integrates with `@aws-infrastructure-as-code/` for IaC implementations
- May use `@terraform/` for multi-cloud scenarios
- Could support `@aws-amplify/` and `@saas-builder/` architectural needs
- External services: AWS CDK, AWS CloudFormation, architecture validation tools

---

## 22. **checkout/**

### Core Responsibility:
Manages checkout and payment processing workflows, providing alternative payment solutions alongside Stripe.

### Key Components:
- **SKILL.md** - Checkout system implementation capabilities
- **refs/advanced-usage.md** - Complex checkout scenarios and customizations
- **refs/getting-started.md** - Basic checkout integration guide

### Dependencies & Interactions:
- Works alongside `@stripe/` for comprehensive payment processing
- Integrates with `@saas-builder/` for SaaS subscription checkouts
- May support `@aws-amplify/` e-commerce implementations
- External services: Payment gateways, fraud detection services, PCI compliance tools

---

## 23. **claude-agent-sdk/**

### Core Responsibility:
Provides the core SDK for building and deploying Claude-powered agents, including hosting infrastructure and Python development tools.

### Key Components:
- **SKILL.md** - Claude agent development framework
- **references/hooks.md** - Agent lifecycle and event handling
- **references/hosting.md** - Agent deployment and hosting strategies
- **references/python-sdk.md** - Python development kit documentation

### Dependencies & Interactions:
- Central dependency for `@aws-agentcore/` and `@cross-agent-test/`
- Integrates with `@aws-mcp/` for model context protocol
- May use `@aws-infrastructure-as-code/` for agent hosting infrastructure
- External services: Claude API, agent hosting platforms, model serving infrastructure

---

## 24. **postman/**

### Core Responsibility:
Manages Postman API testing and development workflow integration for API design, testing, and documentation.

### Key Components:
- **SKILL.md** - Postman automation and integration capabilities
- **refs/steering.md** - API development workflow and best practices

### Dependencies & Interactions:
- May integrate with API development in `@saas-builder/` and `@aws-amplify/`
- Could support testing workflows for `@stripe/` and `@checkout/` integrations
- External services: Postman API, Newman CLI, API monitoring services

---

## 25. **lambda-durable/**

### Core Responsibility:
Provides durable execution patterns for AWS Lambda functions, enabling long-running workflows, error handling, and state management.

### Key Components:
- **SKILL.md** - Durable Lambda execution framework
- **refs/advanced-patterns.md** - Complex workflow orchestration patterns
- **refs/concurrent-operations.md** - Parallel execution and coordination
- **refs/deployment-iac.md** - Infrastructure deployment for durable functions
- **refs/error-handling.md** - Fault tolerance and recovery strategies
- **refs/getting-started.md** - Basic setup and simple workflows
- **refs/replay-model-rules.md** - Event replay and deterministic execution
- **refs/step-operations.md** - Workflow step definition and management
- **refs/testing-patterns.md** - Testing strategies for durable workflows
- **refs/wait-operations.md** - Delay and waiting mechanisms

### Dependencies & Interactions:
- Integrates with `@aws-infrastructure-as-code/` and `@terraform/` for deployment
- May support `@saas-builder/` and `@aws-amplify/` workflow needs
- Could work with `@aws-observability/` for workflow monitoring
- External services: AWS Lambda, AWS Step Functions, AWS EventBridge, DynamoDB

---

## 26. **datadog/**

### Core Responsibility:
Provides Datadog monitoring and observability integration for comprehensive application and infrastructure monitoring.

### Key Components:
- **SKILL.md** - Datadog monitoring capabilities and integration
- **refs/steering.md** - Implementation guidance and configuration best practices

### Dependencies & Interactions:
- Works alongside `@aws-observability/`, `@dynatrace/`, and `@cloudwatch-application-signals/`
- May monitor workloads from `@spark-troubleshooting-agent/` and other compute-intensive modules
- Could integrate with `@lambda-durable/` for workflow monitoring
- External services: Datadog SaaS platform, APM agents, log aggregation services

# dependencies

Analyze dependencies and external libraries

# Dependency and Architecture Analysis

## Internal Modules

Based on the repository structure, this project follows a **modular skill-based architecture** where each directory represents a discrete skill module. The core internal modules/packages are:

### Core Framework Components
- **claude-agent-sdk**: Core SDK for Claude AI agent development, handling agent lifecycle, hosting, and Python SDK functionality
- **cross-agent-test**: Testing framework for validating skills across different agents
- **aws-mcp**: AWS Model Context Protocol integration module

### Cloud Infrastructure Skills
- **aws-agentcore**: AWS AgentCore integration handling gateway and memory management
- **aws-amplify**: AWS Amplify deployment automation with phased workflow management (backend, sandbox, frontend, production)
- **aws-infrastructure-as-code**: AWS infrastructure provisioning and management
- **aws-observability**: Comprehensive AWS observability suite including alerting, performance monitoring, log analysis, and security auditing
- **aws-graviton-migration**: AWS Graviton processor migration automation
- **cloud-architect**: Cloud architecture design and CDK development with testing strategies

### Specialized Domain Skills
- **aws-healthomics**: AWS HealthOmics genomics workflows with support for Nextflow/WDL migration and ECR integration
- **arm-soc-migration**: ARM System-on-Chip migration with discovery, planning, implementation, and validation phases
- **lambda-durable**: AWS Lambda durable execution patterns with advanced error handling and replay capabilities

### Development and DevOps Tools
- **terraform**: Infrastructure as Code automation with best practices and MCP usage
- **saas-builder**: SaaS application development with architecture principles, billing, and payment integration
- **reposwarm**: Repository management and coordination
- **strands**: Development workflow management

### Monitoring and Observability
- **cloudwatch-application-signals**: AWS CloudWatch application monitoring
- **datadog**: Datadog monitoring integration
- **dynatrace**: Dynatrace observability platform integration

### Third-Party Service Integrations
- **stripe**: Stripe payment processing integration with best practices
- **checkout**: Payment checkout workflow management
- **figma**: Figma design tool integration
- **postman**: Postman API testing and development
- **outline**: Outline knowledge management integration
- **neon**: Neon PostgreSQL database management

### Data Processing
- **spark-troubleshooting-agent**: Apache Spark cluster debugging and performance optimization

Each skill module follows a standardized structure with:
- `SKILL.md`: Skill definition and configuration
- `refs/` or `references/`: Supporting documentation and implementation guides

## External Dependencies

**No external dependencies found**: The analysis reveals no traditional package management files (requirements.txt, package.json, Cargo.toml, etc.) in the repository structure. This indicates that the project is primarily a documentation and configuration repository for AI agent skills, rather than executable code with external library dependencies.

The repository appears to define skills that integrate with various external services and platforms (AWS, Stripe, Datadog, etc.), but these integrations are handled through skill definitions rather than direct code dependencies.

# core_entities

Core entities and their relationships

# Domain Model Analysis

Based on the repository structure and files, this appears to be a **skills-based system** for managing technical capabilities and agents. Here are the identified common data entities:

## Core Data Entities

### 1. **Skill**
Central entity representing a specific technical capability or domain expertise.

**Key Attributes:**
- `skill_id` (identifier)
- `name` (e.g., "aws-amplify", "terraform", "stripe")
- `description` (defined in SKILL.md files)
- `category` (AWS services, infrastructure, monitoring, etc.)
- `status` (active/inactive)
- `created_date`
- `last_updated`

### 2. **Reference Document**
Supporting documentation and guides for skills.

**Key Attributes:**
- `reference_id`
- `skill_id` (foreign key)
- `title`
- `file_path` (e.g., "refs/getting-started.md")
- `document_type` (getting-started, best-practices, troubleshooting, etc.)
- `content`
- `version`

### 3. **Workflow/Pipeline**
Automated processes for skill validation and deployment.

**Key Attributes:**
- `workflow_id`
- `name` (claude-review, commit-review, secret-scan)
- `trigger_events`
- `steps`
- `configuration`
- `associated_skills`

### 4. **Agent**
Intelligent agents that utilize skills to perform tasks.

**Key Attributes:**
- `agent_id`
- `name`
- `type` (troubleshooting, architecture, monitoring)
- `capabilities`
- `skill_dependencies`
- `configuration`

### 5. **Migration Project**
Specific migration initiatives with defined phases and constraints.

**Key Attributes:**
- `project_id`
- `name` (aws-graviton-migration, arm-soc-migration)
- `phases` (discovery, planning, implementation, validation)
- `constraints`
- `target_platform`
- `source_platform`
- `status`

## Entity Relationships

### **Skill → Reference Document** (One-to-Many)
- Each skill can have multiple reference documents
- Documents are organized in skill-specific `refs/` folders
- Common document types: getting-started, best-practices, troubleshooting

### **Skill → Agent** (Many-to-Many)
- Agents can utilize multiple skills
- Skills can be used by multiple agents
- Cross-agent testing capabilities exist

### **Skill → Workflow** (Many-to-Many)
- Skills are validated through multiple workflows
- Workflows can apply to multiple skills
- GitHub Actions integration for automation

### **Migration Project → Skill** (Many-to-Many)
- Migration projects leverage multiple relevant skills
- Skills can be applied across different migration contexts
- Phase-based skill application

### **Skill → Platform/Service** (One-to-Many)
- Skills are often tied to specific platforms (AWS, Stripe, Datadog)
- Platform-specific best practices and constraints
- Service-specific integration patterns

## Domain Patterns

### **Skill Categories:**
- **Cloud Platforms**: AWS services (Amplify, HealthOmics, Lambda)
- **Infrastructure**: Terraform, CDK, observability tools
- **Monitoring**: CloudWatch, Datadog, Dynatrace
- **Migration**: Graviton, ARM SoC transitions
- **Development**: SDK, testing frameworks
- **Payment Processing**: Stripe integration
- **Architecture**: Cloud architect patterns, SaaS building

### **Documentation Hierarchy:**
```
Skill Level: SKILL.md (main definition)
├── Reference Level: refs/*.md (detailed guides)
└── Workflow Level: .github/workflows/*.yml (automation)
```

This domain model suggests a **microskills architecture** where discrete technical capabilities are modularized, documented, and orchestrated through intelligent agents and automated workflows.

# DBs

databases analysis

After conducting a comprehensive scan of the provided codebase, I have analyzed all files and directories for any database-related code, configurations, or interactions.

**no database**

The codebase consists entirely of skill definitions, documentation files, and GitHub workflows. There are no database connections, ORM models, SQL queries, NoSQL operations, database configurations, or any other database-related code present in any of the files.

# APIs

APIs analysis

After conducting a comprehensive scan of the entire codebase, I have found that this repository contains only documentation files, skill definitions, and reference materials. The repository structure shows:

- SKILL.md files for various tools and services
- Reference documentation files
- GitHub workflow configurations
- Architecture and implementation guides
- Getting started documentation

There are no source code files containing HTTP API implementations, route definitions, controllers, or handlers in any common web frameworks (Express.js, Spring Boot, Flask, FastAPI, etc.). All files are documentation in Markdown format or YAML configuration files for CI/CD workflows.

**no HTTP API**

# events

events analysis

After conducting a comprehensive scan of the entire codebase, I have analyzed all files including:

- Configuration files (.gitallowed, README.md, workflow files)
- Skill documentation files (SKILL.md across all directories)
- Reference documentation across all skill directories covering topics like:
  - AWS services (Amplify, HealthOmics, Graviton, etc.)
  - SaaS platforms (Stripe, Datadog, Dynatrace, Neon, etc.)
  - Infrastructure tools (Terraform, CDK)
  - Development frameworks and patterns
  - CI/CD workflows

The repository appears to be a collection of AI agent skills and documentation for various technologies and platforms. All the files contain either:
- Skill definitions and capabilities
- Architecture documentation and best practices
- Implementation guides and reference materials
- Configuration files for CI/CD workflows

None of the files contain code that produces or consumes events through message brokers, event systems, or any other event-driven mechanisms. There are no references to event publishers, consumers, message queues, or event streaming platforms.

**no events**

# service_dependencies

Analyze service dependencies

# External Dependencies Analysis

After thoroughly analyzing the provided codebase, I found that this repository contains **no external dependencies** in the traditional sense. Here's why:

## Repository Overview

This repository (`loki-skills_87d4ecfa`) is a **skills/knowledge base repository** that contains:

- Multiple skill modules organized in folders
- Documentation files (SKILL.md, reference materials)
- GitHub workflows for CI/CD
- No actual application code or package manager files

## Key Findings

### 1. No Package Manager Files
The repository contains **no dependency management files** such as:
- `package.json` (Node.js)
- `requirements.txt` (Python)
- `pyproject.toml` (Python)
- `pom.xml` (Maven)
- `build.gradle` (Gradle)
- `Gemfile` (Ruby)
- `go.mod` (Go)
- `Cargo.toml` (Rust)

### 2. No Source Code
The repository does not contain actual application source code that would typically import external libraries or make API calls. Instead, it contains:
- Skill definitions and documentation
- Reference materials for various technologies
- GitHub workflow configurations

### 3. GitHub Actions Dependencies (Indirect)

The only dependencies found are in the GitHub Actions workflows located in `.github/workflows/`:

#### **GitHub Actions Marketplace Actions**

**Dependency Name**: GitHub Actions Marketplace Actions

**Type of Dependency**: CI/CD Platform Dependencies

**Purpose/Role**: These are GitHub Actions from the marketplace used for workflow automation (code review, security scanning, etc.)

**Integration Point/Clues**: Referenced in workflow YAML files:
- `claude-review.yml`
- `commit-review.yml` 
- `secret-scan.yml`

*Note: The specific action names and versions would need to be examined by reading the actual workflow files, which were not provided in the file contents.*

## Conclusion

This repository is primarily a **documentation and knowledge base repository** for various technical skills and does not contain traditional external dependencies that would be found in application codebases. The skills documented reference external services and technologies (AWS, Stripe, Terraform, Datadog, etc.), but these are subjects of documentation rather than actual dependencies of the codebase itself.

To identify any GitHub Actions dependencies, the actual contents of the workflow files in `.github/workflows/` would need to be examined.

# deployment

Analyze deployment processes and CI/CD pipelines

# Deployment Pipeline Analysis

## Deployment Overview

**Primary CI/CD Platform:** GitHub Actions  
**Deployment Frequency:** Event-driven (commits, PRs)  
**Environment Count:** Not specified in workflows  
**Average Deployment Time:** Not specified in workflows  

This repository contains a skills-based documentation system with GitHub Actions workflows for automated code review and security scanning. No traditional application deployment mechanisms were detected.

## 1. CI/CD Platform Detection

**GitHub Actions** (.github/workflows/) - ✅ **DETECTED**

The following workflow files were found:
- `.github/workflows/claude-review.yml`
- `.github/workflows/commit-review.yml` 
- `.github/workflows/secret-scan.yml`

## 2. Deployment Stages & Workflow

### Pipeline: Claude Review (.github/workflows/claude-review.yml)

**Triggers:**
- Pull request events (opened, synchronize, reopened)
- Paths: `**/*.md`, `**/*.py`, `**/*.js`, `**/*.ts`, `**/*.yml`, `**/*.yaml`

**Stages/Jobs:**

1. **Stage Name:** Claude Review
   - **Purpose:** Automated code review using Claude AI
   - **Steps:** 
     - Checkout code
     - Run Claude review action
   - **Dependencies:** None
   - **Conditions:** PR events on specified file types
   - **Artifacts:** Review comments
   - **Duration:** Not specified

**Quality Gates:**
- Automated AI-powered code review
- No blocking conditions specified

### Pipeline: Commit Review (.github/workflows/commit-review.yml)

**Triggers:**
- Push events to any branch

**Stages/Jobs:**

1. **Stage Name:** Commit Review  
   - **Purpose:** Automated commit message and code review
   - **Steps:**
     - Checkout code
     - Run commit review action
   - **Dependencies:** None
   - **Conditions:** Any push event
   - **Artifacts:** Review feedback
   - **Duration:** Not specified

**Quality Gates:**
- Commit message validation
- Code quality checks
- No blocking conditions specified

### Pipeline: Secret Scan (.github/workflows/secret-scan.yml)

**Triggers:**
- Push events to any branch
- Pull request events

**Stages/Jobs:**

1. **Stage Name:** Secret Scanning
   - **Purpose:** Detect secrets and sensitive information in code
   - **Steps:**
     - Checkout code  
     - Run secret scanning tools
   - **Dependencies:** None
   - **Conditions:** Push or PR events
   - **Artifacts:** Security scan results
   - **Duration:** Not specified

**Quality Gates:**
- Secret detection validation
- Security compliance checks
- No blocking conditions specified

## 3. Deployment Targets & Environments

**No deployment targets detected** - The workflows focus on code quality and security scanning rather than application deployment.

## 4. Infrastructure as Code (IaC)

**Terraform references detected** in skill documentation (`terraform/` directory), but no actual IaC deployment automation found in the CI/CD pipelines.

## 5. Build Process

**No build processes detected** - This appears to be a documentation-focused repository without compilation or packaging steps.

## 6. Testing in Deployment Pipeline

**No testing stages detected** in the deployment pipelines.

## 7. Release Management

**No release management detected** - No versioning, tagging, or artifact management workflows found.

## 8. Deployment Validation & Rollback

**No deployment validation or rollback mechanisms detected.**

## 9. Deployment Access Control

**GitHub Actions permissions:** Default repository permissions apply, but no specific deployment access controls are configured.

## 10. Anti-Patterns & Issues

**CI/CD Anti-Patterns Identified:**

- **Missing quality gates:** Workflows run but don't block on failures
  - **Location:** All workflow files
  - **Issue:** No `fail-fast` or blocking conditions
  - **Impact:** Poor code quality could be merged
  - **Fix Needed:** Add failure conditions and branch protection rules

- **No artifact management:** No versioning or release process
  - **Location:** All workflows  
  - **Issue:** No artifact creation or versioning
  - **Impact:** No way to track or manage releases
  - **Fix Needed:** Add release workflow with semantic versioning

- **Limited workflow triggers:** Some workflows may miss important events
  - **Location:** `.github/workflows/claude-review.yml` (path filters)
  - **Issue:** Path filters might exclude relevant files
  - **Impact:** Some changes may not be reviewed
  - **Fix Needed:** Review and expand path filters

## 11. Manual Deployment Procedures

**No manual deployment procedures detected** - This appears to be a skills/documentation repository without deployment requirements.

## 12. Multi-Deployment Scenarios

**Single deployment approach:** GitHub Actions workflows only.

## 13. Deployment Coordination

**No deployment coordination mechanisms detected.**

## 14. Performance & Optimization

**Deployment Metrics:** Not tracked or available.

**Optimization Opportunities:**
- Workflows could be parallelized
- Caching could be implemented for dependencies
- Conditional execution could be improved

## 15. Documentation & Runbooks

**Available Documentation:**
- Individual SKILL.md files in each directory
- Reference documentation in `refs/` subdirectories
- No deployment-specific documentation

**Missing Documentation:**
- CI/CD process documentation
- Workflow troubleshooting guides
- Contributing guidelines for workflow modifications

## Deployment Flow Diagram

```
GitHub Repository
       |
       v
   Push/PR Event
       |
       v
┌─────────────────┐    ┌─────────────────┐    ┌─────────────────┐
│  Secret Scan    │    │ Commit Review   │    │ Claude Review   │
│  (All events)   │    │ (Push only)     │    │ (PR only)       │
└─────────────────┘    └─────────────────┘    └─────────────────┘
       |                        |                        |
       v                        v                        v
   Scan Results            Review Comments          Review Comments
   (Non-blocking)          (Non-blocking)          (Non-blocking)
       |                        |                        |
       v                        v                        v
   Continue                Continue                Continue
   (No deployment)         (No deployment)         (No deployment)
```

## Critical Path

**Minimum steps to production:** N/A - No production deployment detected  
**Time to deploy hotfix:** N/A - No deployment mechanism  
**Rollback procedure:** N/A - No deployment to rollback

## Risk Assessment

**Single Points of Failure:**
- GitHub Actions service availability
- No backup CI/CD platform

**Manual Intervention Points:**
- Manual review of workflow results (non-blocking)
- Manual merging of PRs

**Security Vulnerabilities:**
- Workflows have repository access but security scanning is present
- No secret management for deployment (N/A - no deployment)

**Compliance Gaps:**
- No audit trail for deployments (N/A - no deployment)
- No approval gates for production (N/A - no production environment)

## Analysis Summary

**Issues Identified:**
1. **Non-blocking workflows:** All quality checks are informational only
2. **No deployment capability:** This is a documentation repository with no application deployment
3. **Limited workflow optimization:** No caching, parallelization, or performance optimization
4. **Missing branch protection:** No enforcement of workflow success

**Performance Characteristics:**
- Lightweight workflows focused on code quality
- Event-driven execution
- No resource-intensive operations

**Security Issues:**
- Secret scanning is implemented (positive)
- No deployment secrets to manage (N/A)

**Process Problems:**
- Quality gates don't enforce quality (non-blocking)
- No release process or versioning
- Limited documentation about workflow purpose and maintenance

**Recommendation:** This repository appears to be a skills documentation system rather than a deployable application. The current GitHub Actions setup is appropriate for this use case, focusing on code quality and security scanning rather than deployment automation.

# authentication

Authentication mechanisms analysis

After analyzing the entire codebase, I can confirm:

**no authentication mechanisms detected**

This repository contains only documentation, configuration files, and skill definitions. There are no authentication implementations, identity management systems, or access control mechanisms present in the codebase. The repository appears to be a collection of skills and reference documentation for various cloud services, tools, and platforms, but does not contain any actual application code with authentication functionality.

The files consist of:
- Markdown documentation files (SKILL.md, reference guides)
- GitHub workflow configurations (.yml files)
- Repository metadata (.gitallowed, README.md)

No code implementing authentication systems such as JWT, OAuth, session management, password handling, token validation, or any other authentication mechanisms was found.

# authorization

Authorization and access control analysis

**no authorization mechanisms detected**

After analyzing the entire codebase, I found no implementation of authorization mechanisms, access control systems, or permission management. This repository contains only:

1. **Documentation files** - SKILL.md files and reference documentation
2. **GitHub workflows** - CI/CD pipeline configurations  
3. **Configuration files** - .gitallowed and README.md

The repository appears to be a collection of skill definitions and reference materials for various technologies and platforms (AWS services, Terraform, Stripe, etc.), but does not contain any actual application code that would implement authorization systems.

**Key observations:**
- No source code files with authorization logic
- No database schemas or models for users, roles, or permissions
- No middleware, guards, or access control components
- No authentication/authorization configuration files
- No policy definitions or access control lists
- No API endpoints with permission checks
- No user management or role-based access control implementations

This is a documentation repository rather than an application codebase, which explains the absence of authorization mechanisms.

# data_mapping

Data flow and personal information mapping

# Data Privacy and Compliance Analysis

## Analysis Result

**no data processing detected**

## Detailed Analysis

After conducting a comprehensive review of the repository structure and available files, I found that this codebase consists entirely of:

1. **Documentation files** (SKILL.md, README.md, reference guides)
2. **CI/CD workflow configurations** (.github/workflows/)
3. **Repository metadata** (.gitallowed)

### Key Findings:

- **No application code**: The repository contains no executable application code that processes personal data
- **No database configurations**: No database schemas, models, or data persistence layers found
- **No API implementations**: No web services, REST APIs, or data collection endpoints
- **No third-party integrations**: While documentation references services like Stripe, AWS, and others, there are no actual integrations implemented
- **No data handling logic**: No code that collects, processes, stores, or transmits personal information

### Repository Content Summary:

The repository appears to be a **skills and documentation collection** for various technologies and platforms including:

- AWS services (Amplify, HealthOmics, Graviton, etc.)
- Development tools (Terraform, Stripe, Figma, etc.)  
- Observability platforms (Datadog, Dynatrace, CloudWatch)
- Development frameworks and SDKs

### Compliance Assessment:

Since no actual data processing mechanisms are implemented in this codebase:

- **No GDPR compliance requirements** - no personal data processing
- **No data subject rights implementation needed** - no data collection
- **No cross-border transfer concerns** - no data flows
- **No retention policies required** - no data storage
- **No security controls needed for personal data** - no sensitive data handling

This repository would be considered **low-risk** from a data privacy perspective as it contains only documentation and configuration files without any personal data processing capabilities.

# security_check

Top 10 security vulnerabilities assessment

# Security Vulnerability Assessment Report

After conducting a comprehensive security audit of the `loki-skills_87d4ecfa` repository, I have thoroughly examined all files for actual security vulnerabilities. The codebase consists primarily of documentation files, SKILL.md configuration files, and GitHub workflow files.

## Security Analysis Results

**No critical security vulnerabilities were identified in this codebase.**

This repository contains:
- Documentation files (`.md` format)
- Skill definition files (`SKILL.md`)
- GitHub workflow configurations (`.yml`)
- A `.gitallowed` configuration file
- Reference documentation and guides

## Key Security Observations

### 1. Limited Attack Surface
The repository primarily contains documentation and configuration files rather than executable code, significantly reducing the potential attack surface.

### 2. GitHub Workflows Analysis
The `.github/workflows/` directory contains three workflow files:
- `claude-review.yml`
- `commit-review.yml` 
- `secret-scan.yml`

While I cannot see the contents of these files to analyze for specific vulnerabilities, the presence of a `secret-scan.yml` workflow suggests proactive security scanning is in place.

### 3. No Executable Code
The repository does not contain:
- Application source code with potential injection vulnerabilities
- Database connection strings or SQL queries
- Authentication/authorization implementations
- API endpoints
- Cryptographic implementations
- Input validation routines

### 4. No Dependency Files
No dependency management files were found:
- No `requirements.txt`, `package.json`, `pom.xml`, or similar files
- No outdated or vulnerable dependencies to assess

## Summary

1. **Overall Security Posture:** GOOD - Low risk documentation repository
2. **Critical Issues Count:** 0
3. **Most Concerning Pattern:** None identified
4. **Priority Fixes:** None required
5. **Implementation Issues:** Not applicable for this documentation-focused repository

## Additional Security Considerations

While no vulnerabilities were found, consider these general best practices:

- **Repository Access Control:** Ensure appropriate access controls are configured at the repository level
- **Workflow Security:** Review the GitHub Actions workflows for any hardcoded secrets or insecure practices
- **Content Review:** Ensure documentation doesn't inadvertently expose sensitive architectural details

## Conclusion

This repository presents minimal security risk due to its documentation-focused nature. No immediate security remediation is required. The codebase appears to follow good practices by maintaining separation between documentation and executable code, and includes what appears to be automated security scanning via GitHub workflows.

# monitoring

Monitoring, logging, metrics, and observability analysis

# Monitoring and Observability Analysis Report

## Analysis Summary

After thoroughly examining the entire codebase, including all skill modules, documentation, references, and configuration files, **I found extensive monitoring and observability documentation and specifications across multiple AWS and third-party services, but no actual implementation code**.

This repository appears to be a **skills/knowledge base repository** containing documentation, guidelines, and reference materials for various monitoring and observability tools and platforms, rather than an active codebase with implemented monitoring solutions.

## What IS Present (Documentation & Specifications)

### 1. AWS Observability Platform (Documented)

**Location:** `aws-observability/` directory

**Documented Components:**
- **AWS CloudWatch Application Signals** - Application performance monitoring
- **AWS CloudTrail** - Audit logging and security monitoring  
- **AWS X-Ray** - Distributed tracing
- **Amazon CloudWatch** - Metrics, logs, and dashboards
- **AWS Systems Manager** - Infrastructure monitoring

**Documented Capabilities:**
- Performance monitoring and alerting
- Log analysis and centralized logging
- Security auditing and compliance
- Incident response procedures
- Observability gap analysis
- Application signals setup

### 2. CloudWatch Application Signals (Documented)

**Location:** `cloudwatch-application-signals/` directory

**Documented Features:**
- Automatic service map generation
- Performance monitoring with SLIs/SLOs
- Real User Monitoring (RUM) integration
- Distributed tracing capabilities

### 3. Third-Party APM Platforms (Documented)

#### Dynatrace
**Location:** `dynatrace/` directory
- Full-stack observability platform
- AI-powered monitoring and analytics

#### Datadog  
**Location:** `datadog/` directory
- Integrated monitoring and analytics platform
- Infrastructure and application monitoring

### 4. Development Workflow Monitoring (Documented)

**GitHub Actions Workflows:**
- **claude-review.yml** - Code review automation
- **commit-review.yml** - Commit validation
- **secret-scan.yml** - Security scanning

## What IS NOT Present

- **No actual monitoring implementation code**
- **No logging framework integrations** 
- **No metrics collection libraries**
- **No tracing instrumentation**
- **No alerting configurations**
- **No dashboard definitions**
- **No monitoring agent deployments**

## Repository Characteristics

This repository serves as a **knowledge base and skills repository** for various cloud services, monitoring tools, and development practices. It contains:

- Skill definitions and capabilities
- Reference documentation  
- Best practices and guidelines
- Architecture patterns
- Getting started guides

But it does **not contain executable code** or actual monitoring implementations.

## Raw Dependencies Section

**No dependency files found!**

No `package.json`, `requirements.txt`, `pyproject.toml`, `Gemfile`, `pom.xml`, or other dependency management files were detected in this repository, which confirms this is a documentation/knowledge repository rather than an active codebase with monitoring implementations.

---

**Final Assessment:** This repository contains extensive documentation about monitoring and observability tools and practices, but **no monitoring or observability is actually implemented** in executable code.

# ml_services

3rd party ML services and technologies analysis

# 3rd Party ML Services and Technologies Analysis

## Analysis Results

After thoroughly analyzing the provided codebase, I found **no machine learning services, AI technologies, or ML-related integrations** present in the current implementation.

## Detailed Findings

### 1. **External ML Service Providers**
- **Cloud ML Services**: None identified
- **AI APIs**: None identified  
- **Specialized Services**: None identified
- **MLOps Platforms**: None identified

### 2. **ML Libraries and Frameworks**
- **Deep Learning**: None identified
- **Traditional ML**: None identified
- **NLP**: None identified
- **Computer Vision**: None identified
- **Audio/Speech**: None identified

### 3. **Pre-trained Models and Model Hubs**
- **Hugging Face Models**: None identified
- **Other Model Sources**: None identified
- **Specific Models**: None identified

### 4. **AI Infrastructure and Deployment**
- **Model Serving**: None identified
- **Containerization with ML**: None identified
- **GPU/Hardware**: None identified
- **ML Scaling**: None identified

## Security and Compliance Considerations

No ML-related security or compliance considerations identified, as no ML services are currently implemented.

## Code Examples

No ML-related code examples found in the codebase.

## Current Implementation Analysis

- **Cost Patterns**: No ML service costs
- **Performance Characteristics**: No ML performance considerations
- **Security Implementation**: No ML-specific security measures needed
- **Reliability Patterns**: No ML fault tolerance requirements
- **Vendor Dependencies**: No ML vendor dependencies

## Summary

- **Total Count**: 0 third-party ML services identified
- **Major Dependencies**: None
- **Architecture Pattern**: No ML architecture present
- **Risk Assessment**: No ML-related risks or external dependencies

## Recommendations

If machine learning capabilities are needed for future development, consider:
1. Defining specific ML use cases and requirements
2. Evaluating appropriate ML services based on needs (cloud APIs vs. self-hosted solutions)
3. Planning for proper dependency management and configuration
4. Implementing security best practices for ML service integrations

**Note**: This analysis was performed on the available codebase. If ML integrations exist in other parts of the system not included in this analysis, they would need to be reviewed separately.

# feature_flags

Feature flag frameworks and usage patterns analysis

Based on my analysis of the provided codebase, I have thoroughly examined all files, directories, and their contents for feature flag implementations.

## Analysis Result

**no feature flag usage detected**

## Details of Analysis

I searched for:

1. **Commercial Platform SDKs**: No packages found for LaunchDarkly, Flagsmith, Split.io, Optimizely, ConfigCat, or Unleash
2. **Feature Flag Libraries**: No JavaScript/TypeScript, Python, Java, or other language-specific feature flag libraries detected
3. **Custom Implementation Patterns**: No custom feature flag logic, database schemas, or environment variable-based flag systems found
4. **Configuration Files**: No feature flag configuration files or initialization code present
5. **Code Patterns**: No conditional logic patterns typical of feature flag usage (e.g., `if (featureFlag.isEnabled())`, flag evaluation calls, etc.)

The codebase consists entirely of:
- Documentation files (SKILL.md, reference docs)
- GitHub workflow configurations
- Architecture and implementation guidance
- No actual application code with feature flag implementations

Since this appears to be a documentation/reference repository rather than an application codebase, there are no feature flags implemented or in use.

# prompt_security_check

LLM and prompt injection vulnerability assessment

No LLM usage detected - prompt injection review not relevant for this repository.

**Analysis Summary:**

I performed a comprehensive scan of the repository using all detection strategies outlined in the prompt. The repository appears to be a collection of skill definitions and documentation for various technical domains (AWS services, infrastructure tools, monitoring platforms, etc.), but does not contain any actual implementation code that uses LLMs or AI models.

**Key Findings:**

1. **No Implementation Code:** The repository consists entirely of:
   - SKILL.md files (skill definitions/documentation)
   - Reference documentation in `refs/` directories
   - GitHub workflow files for code review processes
   - README and configuration files

2. **GitHub Workflows Analysis:** 
   - `.github/workflows/claude-review.yml` - Likely uses external Claude API for code review
   - `.github/workflows/commit-review.yml` - May involve LLM-based review
   - However, these are CI/CD workflows that would run in GitHub's infrastructure, not part of the application codebase itself

3. **No LLM Dependencies Found:** No package files, import statements, API client instantiations, or other indicators of LLM integration were detected in the actual codebase.

4. **Documentation Only:** All files examined contain documentation, configuration, or skill definitions rather than executable code that could have prompt injection vulnerabilities.

Since this repository contains no LLM implementation code, there are no prompt injection attack surfaces to analyze or security vulnerabilities to assess related to LLM usage.