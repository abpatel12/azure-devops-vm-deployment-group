# Azure DevOps Agent-Based VM Deployment Pipeline

> Provisioned a Windows Server VM on Azure, registered it as an Azure DevOps Deployment Group agent, and built a release pipeline that deploys a web application to both Azure App Service and IIS — achieving 100% pipeline pass rate on first release.

---

## Overview

This project demonstrates end-to-end agent-based deployment using Azure DevOps Deployment Groups — a more advanced pattern than standard YAML pipelines, targeting an actual VM rather than a hosted agent.

A Windows Server VM was provisioned on Azure, configured with IIS, and registered as a live deployment target. A multi-stage release pipeline was then built to deploy a React web application to both Azure App Service and the on-VM IIS server simultaneously.

---

## Architecture

```
Azure DevOps (Project: B786)
        │
        ▼
  Release Pipeline
        │
   ┌────┴────┐
   │  Stage 1│  Deployment Group Job
   └────┬────┘
        │
   ┌────┴──────────────────┐
   │                       │
   ▼                       ▼
Azure App Service      Windows VM (IIS)
(react1234567223)      Korea South Region
                       Standard_D2L_v3
```

---

## What Was Built

### 1. Azure VM Provisioning
- Deployed a **Windows Server VM** (`Standard_D2L_v3`) in the Korea South region
- Configured networking: VNet, subnet, public IP, NSG rules
- Enabled secure remote access via Azure portal

### 2. IIS Web Server Configuration
- Installed **Web Server (IIS)** role via Windows Server Manager
- Added required features including IIS Management Console
- Configured default website for application hosting

### 3. Azure DevOps Deployment Group Agent
- Registered the VM as a **Deployment Group agent** in Azure DevOps project `B786`
- Executed PowerShell registration script to download and install Azure Pipelines agent `v4.274.1`
- Agent connected and passed capability scan — status: **1 Online, 1 Passing**

### 4. Release Pipeline
Configured a multi-task release pipeline with two deployment targets:

| Task | Target | Config |
|------|--------|--------|
| Azure App Service Deploy | `react1234567223` (Web App on Windows) | Package from `$(System.DefaultWorkingDirectory)/**/*.zip` |
| Deploy IIS Website/App | VM IIS Default Website | Package from `$(System.DefaultWorkingDirectory)/B786-CI (R)/drop` |

### 5. Successful Deployment
- **Release-4** completed: `1 Succeeded / 0 Failed — 100% Passed`
- Runtime: ~1 minute 9 seconds
- Live web application confirmed running at Azure App Service URL

---

## Tech Stack

| Category | Tools |
|----------|-------|
| Cloud | Microsoft Azure |
| VM | Windows Server, Standard_D2L_v3, Korea South |
| Web Server | IIS (Internet Information Services) |
| CI/CD | Azure DevOps, Release Pipelines, Deployment Groups |
| Agent | Azure Pipelines Agent v4.274.1 |
| App | React.js (Mediplus web application) |
| Scripting | PowerShell |

---

## Key Concepts Demonstrated

- **Deployment Groups** — agent-based deployment targeting specific VMs, not hosted agents
- **Hybrid deployment** — simultaneously targeting Azure PaaS (App Service) and IaaS (IIS on VM)
- **Agent registration via PowerShell** — automated agent bootstrap with TLS 1.2 enforcement
- **Release pipeline orchestration** — multi-task stage with deployment group job
- **IIS server role configuration** — Windows Server roles and features management

---

## Project Structure

```
B786/
├── Pipelines/
│   ├── CI Pipeline          # Build pipeline
│   └── Release Pipeline     # Multi-stage release (this project)
├── Deployment Groups/
│   └── html                 # Target: Windows VM agent
└── Artifacts/
    └── B786-CI (R)/drop     # Build output deployed to IIS
```

---

## Result

| Metric | Value |
|--------|-------|
| Deployment status | ✅ Succeeded |
| Pass rate | 100% |
| Targets registered | 1 Online |
| Deployment time | ~1m 9s |
| Application | Live on Azure App Service + IIS |

---

## Author

**Abdullah Mohammed Patel**  
Junior Cloud / DevSecOps Engineer — Melbourne, VIC  
[LinkedIn](https://www.linkedin.com/in/patelmohammedabdullah) · [GitHub](https://github.com/abpatel12)
