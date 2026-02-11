# [PROJECT_NAME] - Salesforce Project

## Overview
[Describe your Salesforce project here — what it does, who it's for, and key business requirements.]

## Tech Stack
- **Platform**: Salesforce (API 62.0)
- **Backend**: Apex (triggers, batch, queueable, REST APIs)
- **Frontend**: Lightning Web Components (LWC)
- **Automation**: Flows (record-triggered, screen)
- **Testing**: Apex Test Framework, Jest (LWC)
- **CLI**: Salesforce CLI (`sf`)

## Framework Location
The Claude Dev Framework is located at `./claude-dev-framework/` (git submodule).

If the submodule directory is empty, initialize it:
```bash
git submodule update --init --recursive
```

## Context Loading Rules

When working on this project, load context files based on the task at hand:

### Always Load
- `claude-dev-framework/context/security-best-practices.md` — Security fundamentals

### Apex Development
When working on Apex classes, triggers, batch jobs, or test classes:
- `claude-dev-framework/context/salesforce-apex-guide.md` — Governor limits, trigger patterns, testing, CRUD/FLS

### LWC Development
When working on Lightning Web Components:
- `claude-dev-framework/context/salesforce-lwc-guide.md` — Component patterns, wire service, Jest testing

### Automation Work
When working on Flows, Platform Events, or automation:
- `claude-dev-framework/context/salesforce-automation-guide.md` — Flow types, order of execution, platform events

### Integration Work
When working on REST/SOAP callouts, APIs, or external system connections:
- `claude-dev-framework/context/salesforce-integration-guide.md` — Callouts, Named Credentials, REST APIs, OAuth

### General Development
For architecture decisions or cross-cutting concerns:
- `claude-dev-framework/context/agent-architecture.md` — Design patterns
- `claude-dev-framework/context/testing-strategy.md` — Testing approaches

## Project Structure

```
force-app/main/default/
├── classes/          # Apex classes (services, handlers, controllers, tests)
├── triggers/         # Apex triggers (one per object)
├── lwc/              # Lightning Web Components
├── aura/             # Aura Components (legacy, avoid for new work)
├── objects/          # Custom objects and fields
├── flows/            # Flows and automation
├── permissionsets/   # Permission Sets (prefer over Profiles)
├── permissionsetgroups/  # Permission Set Groups
├── layouts/          # Page layouts
├── tabs/             # Custom tabs
└── staticresources/  # Static resources
```

## Project-Specific Context
Additional project documentation lives in the `docs/` directory.

## Common Commands

### Authentication
```bash
# Authenticate to org
sf org login web --alias my-org

# Authenticate to DevHub
sf org login web --set-default-dev-hub --alias devhub
```

### Scratch Org Development
```bash
# Create scratch org
sf org create scratch --definition-file config/project-scratch-def.json --alias scratch-org --duration-days 30

# Push source to scratch org
sf project deploy start

# Pull changes from scratch org
sf project retrieve start

# Open scratch org in browser
sf org open
```

### Deployment
```bash
# Deploy to target org
sf project deploy start --target-org my-org

# Deploy specific metadata
sf project deploy start --source-dir force-app/main/default/classes --target-org my-org

# Run all tests during deployment
sf project deploy start --target-org my-org --test-level RunLocalTests
```

### Testing
```bash
# Run all local tests
sf apex run test --test-level RunLocalTests --result-format human

# Run specific test class
sf apex run test --class-names MyTestClass --result-format human

# Run specific test method
sf apex run test --tests MyTestClass.testMethod --result-format human

# Run tests with code coverage
sf apex run test --test-level RunLocalTests --code-coverage --result-format human
```

### Anonymous Apex
```bash
# Execute anonymous Apex
sf apex run --file scripts/apex/my-script.apex

# Execute inline
echo "System.debug('Hello');" | sf apex run
```

### Data Operations
```bash
# Export data
sf data export tree --query "SELECT Id, Name FROM Account LIMIT 10" --output-dir data/

# Import data
sf data import tree --files data/Account.json

# Execute SOQL
sf data query --query "SELECT Id, Name FROM Account LIMIT 5"
```

## Coding Standards

### Apex
- **One trigger per object** — Use trigger handler pattern (see apex guide)
- **Bulkify everything** — Process collections, never single records
- **CRUD/FLS enforcement** — Use `WITH USER_MODE` or `Security.stripInaccessible()`
- **Test coverage** — Minimum 75%, aim for 90%+
- **No hardcoded IDs** — Use Custom Metadata or Custom Settings

### LWC
- **Use @wire when possible** — Reactive data binding over imperative calls
- **SLDS utility classes** — Use Lightning Design System, avoid custom CSS where possible
- **Component communication** — Parent-child via @api/events, cross-component via LMS
- **Jest tests for all components** — Mock wire adapters and Apex calls

### General
- **No logic in triggers** — Delegate to handler classes
- **Descriptive naming** — `AccountTriggerHandler`, `OrderService`, `ContactTestDataFactory`
- **Permission Sets over Profiles** — Always use Permission Sets for access control
- **Metadata-driven config** — Use Custom Metadata Types for configuration values
