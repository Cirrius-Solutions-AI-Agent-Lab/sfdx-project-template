# [PROJECT_NAME]

A Salesforce project built with the [Claude Dev Framework](https://github.com/Cirrius-Solutions-AI-Agent-Lab/claude-dev-framework) for AI-assisted development.

## Getting Started

### Use This Template

1. Click **"Use this template"** on GitHub to create a new repository
2. Clone your new repository:
   ```bash
   git clone --recurse-submodules <your-repo-url>
   cd <your-project-name>
   ```
3. If you forgot `--recurse-submodules`:
   ```bash
   git submodule update --init --recursive
   ```

### Customize

1. Update `sfdx-project.json` — Replace `[PROJECT_NAME]` with your project name
2. Update `config/project-scratch-def.json` — Replace `[PROJECT_NAME]` and adjust features
3. Update `CLAUDE.md` — Fill in your project overview and any project-specific context

### Prerequisites

- [Salesforce CLI](https://developer.salesforce.com/tools/salesforcecli) (`sf`)
- A Salesforce org or DevHub for scratch orgs
- [Node.js](https://nodejs.org/) (for LWC Jest testing)
- [Claude Code](https://claude.ai/claude-code) (for AI-assisted development)

## Development Workflow

### Authenticate
```bash
# Log in to your DevHub
sf org login web --set-default-dev-hub --alias devhub

# Create a scratch org
sf org create scratch --definition-file config/project-scratch-def.json --alias scratch --duration-days 30 --set-default

# Open the scratch org
sf org open
```

### Develop
```bash
# Push source to scratch org
sf project deploy start

# Pull changes from scratch org
sf project retrieve start
```

### Test
```bash
# Run Apex tests
sf apex run test --test-level RunLocalTests --result-format human --code-coverage

# Run LWC Jest tests (after npm install)
npm test
```

### Deploy
```bash
# Deploy to a sandbox or production
sf project deploy start --target-org my-org --test-level RunLocalTests
```

## Project Structure

```
├── CLAUDE.md                  # AI context — project overview and loading rules
├── sfdx-project.json          # SFDX project configuration
├── config/
│   └── project-scratch-def.json   # Scratch org definition
├── force-app/main/default/
│   ├── classes/               # Apex classes
│   ├── triggers/              # Apex triggers
│   ├── lwc/                   # Lightning Web Components
│   ├── aura/                  # Aura Components
│   ├── objects/               # Custom objects and fields
│   ├── flows/                 # Flows
│   ├── permissionsets/        # Permission Sets
│   ├── permissionsetgroups/   # Permission Set Groups
│   ├── layouts/               # Page layouts
│   ├── tabs/                  # Custom tabs
│   └── staticresources/       # Static resources
├── scripts/apex/              # Anonymous Apex scripts
├── docs/                      # Project documentation
└── claude-dev-framework/      # Shared knowledge base (submodule)
```

## Claude Integration

This project includes the [Claude Dev Framework](https://github.com/Cirrius-Solutions-AI-Agent-Lab/claude-dev-framework) as a git submodule, providing:

- **Salesforce-specific context** — Apex, LWC, automation, and integration guides
- **Custom commands** — `/prime`, `/plan`, `/build`, `/test`
- **Security best practices** — CRUD/FLS, sharing rules, input validation
- **Testing strategies** — Approaches for Apex and LWC testing

### Using Claude Commands

When working with Claude Code in this project:

| Command | Purpose |
|---------|---------|
| `/prime` | Load project context and assess health |
| `/plan` | Interactive planning for your current task |
| `/build` | Implement features with TDD approach |
| `/test` | Run and analyze test results |

### Updating the Framework

```bash
# Pull latest framework updates
git submodule update --remote claude-dev-framework
git add claude-dev-framework
git commit -m "Update claude-dev-framework"
```

## Resources

- [Salesforce Developer Documentation](https://developer.salesforce.com/docs)
- [Lightning Web Components Guide](https://developer.salesforce.com/docs/platform/lwc/guide)
- [Apex Developer Guide](https://developer.salesforce.com/docs/atlas.en-us.apexcode.meta/apexcode)
- [Salesforce CLI Reference](https://developer.salesforce.com/docs/atlas.en-us.sfdx_cli_reference.meta/sfdx_cli_reference)
