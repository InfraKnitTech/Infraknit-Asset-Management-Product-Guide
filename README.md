# Infraknit-Asset-Management-Product-Guide
The Full Product Guide for InfraKnit Asset and Patch Management

**Unified IT Asset & Endpoint Management Platform**

---

## What is InfraKnit Asset Management?

InfraKnit Asset Management is an all-in-one IT infrastructure management platform that gives your IT team centralized visibility and control over every endpoint, network device, and software asset in your organization. From a single web console you can discover devices, deploy software, patch systems, monitor health, and automate routine operations -- across both Windows and Linux environments.

Whether you manage 10 machines or 10,000, InfraKnit scales with you while remaining simple enough that any IT administrator can be productive on day one.

---

## Platform at a Glance

| Component | Description |
|-----------|-------------|
| **Web Console** | Modern, responsive dashboard (React + TypeScript) accessible from any browser |
| **Backend Server** | High-performance REST API server (FastAPI + MySQL) |
| **Windows Agent** | Lightweight service for Windows 10/11 and Windows Server 2016+ |
| **Ubuntu Agent** | Native Linux agent for Ubuntu 20.04 (Focal), 22.04 (Jammy), 24.04 (Noble), 24.10 (Oracular), and 25.04 (Plucky) |
| **SNMP Integration** | Agentless monitoring for network switches, routers, printers, and other SNMP-capable devices |

---

## Supported Platforms

### Windows
- Windows 10 (all editions)
- Windows 11 (all editions)
- Windows Server 2016, 2019, 2022

### Linux (Ubuntu)
- Ubuntu 20.04 LTS (Focal Fossa)
- Ubuntu 22.04 LTS (Jammy Jellyfish)
- Ubuntu 24.04 LTS (Noble Numbat)
- Ubuntu 24.10 (Oracular Oriole)
- Ubuntu 25.04 (Plucky Puffin)

### Network Devices (via SNMP)
- Managed switches and routers
- Network printers
- UPS devices
- Any SNMPv1/v2c/v3 capable device

---

## Core Features

### 1. Agent Management & Approval Workflow

Every endpoint runs a lightweight agent that auto-registers with the server. New agents land in a **Pending** state and require explicit administrator approval before they can send data -- ensuring no unauthorized device ever joins your managed fleet.

- **Approve** -- Grant access; agent goes online and begins reporting
- **Reject** -- Deny with a 24-hour cooldown; agent can re-request after the cooldown
- **Permanently Ignore** -- Silently block the agent forever; it appears in the Dead Agents list and can be reactivated later if needed
- **Decommission** -- Soft-delete an agent; if the same machine re-bootstraps, it must go through approval again
- **Reapprove** -- Bring back a decommissioned or orphaned agent without reinstalling

Real-time agent health is tracked automatically -- agents that miss heartbeats for 3 minutes are marked offline, and admins are notified.

### 2. Hardware & Software Inventory

Agents automatically collect and report comprehensive asset data:

**Hardware Inventory**
- Processor (model, cores, clock speed, virtualization support)
- Memory (total RAM, individual module details -- slot, size, type, speed, manufacturer)
- Storage (physical disks -- HDD/SSD/NVMe, capacity, health status, partitions)
- GPU (model, vendor, VRAM, driver version)
- Motherboard (vendor, model, serial)
- BIOS/UEFI (vendor, version, secure boot status)
- TPM (presence, version, enabled state)
- Power (battery info, power plan, laptop vs desktop detection)
- Network adapters (MAC addresses, IP configuration, link speed)

**Software Inventory**
- Complete list of installed applications with name, version, publisher, install date, and size
- Automatic periodic collection (every 24 hours by default)
- On-demand refresh from the console
- Chunked reporting for machines with thousands of packages

**Peripheral Tracking**
- Monitors (resolution, serial number)
- Keyboards, mice, webcams, headsets, speakers, microphones
- Printers
- USB storage, USB hubs, docking stations, external drives

**Hardware Grading**
- Every asset receives an A-F hardware grade based on CPU, memory, storage, and age
- Quickly identify aging hardware that needs replacement

### 3. Software Package Management

Upload, catalog, and deploy software packages across your entire fleet.

**Windows Packages**
- Supports .exe, .msi, .msu, .ps1, and .zip installers
- Configure silent install/uninstall arguments per package
- Set install timeout, reboot requirements, and architecture constraints

**Linux Packages**
- Upload .deb packages and InfraKnit automatically creates a per-app APT repository
- Agents configure themselves as APT clients dynamically -- no manual source list editing
- Supports .sh and .tar.gz script-based installers as well
- Per-codename repositories (Focal, Jammy, Noble, Oracular, Plucky)

**Package Catalog**
- Organize by category (Productivity, Browser, Utility, Security, Development, Communication)
- Track versions, vendors, and architectures
- File integrity verification via hash checking
- Download packages from the console for offline distribution

### 4. Patch Management

Keep your fleet secure with structured patch deployment.

**Windows Patches**
- Upload .msu, .cab, .msp, and .exe patches with KB IDs
- Track severity (Critical, Important, Moderate, Low)
- Deploy to individual agents or groups

**Linux Patches**
- Upload .deb patches with automatic APT repository integration
- Auto-generated KB IDs (LP-{package_name}) for tracking
- Per-codename patch repositories

**Automated Patch Discovery (Linux)**
- InfraKnit can automatically sync Ubuntu package indices and compare them against your agent inventories
- Discovers available security and feature updates across Focal, Jammy, and Noble
- Select which updates to download, and InfraKnit fetches the .deb files and creates patch entries automatically
- One-click deployment of discovered patches to affected agents
- Filter by security-only updates, codename, or status

### 5. Service Packs (Deployment Bundles)

Bundle multiple packages and patches together into a single deployable unit.

- Define installation order (drag to reorder)
- Mix packages and patches in the same bundle
- Platform-aware -- Windows and Linux bundles are kept separate
- Lifecycle management: Draft, Active, Deprecated
- Duplicate existing service packs to create variants
- Deploy to multiple agents in one action with ordered installation
- If a required item fails, deployment stops and admin is notified

**Example Use Cases:**
- "New Employee Setup" -- Office, Chrome, VPN, Slack
- "Developer Workstation" -- VS Code, Git, Node.js, Docker
- "Security Baseline" -- Latest critical patches bundled together

### 6. Job System

Every action on an endpoint is tracked as a Job with full lifecycle visibility.

**Supported Job Types:**
- Install/Uninstall Package
- Install Patch
- Run Script (PowerShell on Windows, Bash on Linux)
- Collect Inventory
- Reboot / Shutdown / Cancel Shutdown
- Deploy Service Pack
- Agent Self-Update

**Job Features:**
- Priority levels (Low, Normal, High, Critical)
- Configurable timeout and retry count
- Full execution timeline (created, dispatched, queued, running, completed)
- Captured stdout, stderr, and exit code
- Cancel pending jobs from the console
- Version pre-check -- skip redundant installs if the same version is already present

### 7. Deployment Tracking

When you deploy to multiple agents at once, InfraKnit groups all jobs into a **Deployment** for unified tracking.

- Progress bar showing completion percentage
- Success and failure counts at a glance
- Drill into failed devices to see exact error messages, exit codes, stdout/stderr logs
- Deployment types: Package, Service Pack, Patch, Script, Reboot, Shutdown

### 8. Deploy Wizard

A guided multi-step deployment workflow that makes complex rollouts simple.

**Three deployment modes:** Packages, Patches, or Service Pack

**Smart target selection:**
- Pick individual agents by name
- Target entire groups
- Target by department or tags
- Mix and match -- InfraKnit resolves all targets to individual agents

**Platform-aware deployment:**
- Auto-filters packages/patches based on selected agents' operating systems
- Warns on mixed-platform selections
- Blocks cross-platform deployments

**Flexible scheduling:**
- Deploy Now -- execute immediately
- Schedule for Later -- one-time, daily, weekly, or monthly recurrence
- Day-of-week and day-of-month selectors for recurring schedules
- IST timezone support with automatic UTC conversion

### 9. Scheduled Tasks

Automate recurring operations without manual intervention.

- Schedule types: Once, Daily, Weekly, Monthly
- Supported tasks: Install Package, Install Patch, Deploy Service Pack, Run Script, Reboot, Shutdown, Collect Inventory
- Pause and resume schedules
- Run Now to execute a schedule immediately outside its normal cadence
- Track run count, last run status, and next run time
- Per-schedule target resolution (agents, groups, departments, tags)

### 10. Network Discovery

Find every device on your network -- even the ones without agents.

**Scan Capabilities:**
- Quick Scan -- fast ICMP + common port sweep
- Full Scan -- comprehensive port scan with service detection
- TCP-only scan for firewall-blocked environments
- Single-IP quick scan for targeted investigation

**Device Intelligence:**
- IP address, MAC address, hostname (DNS + NetBIOS)
- Open ports with service identification
- OS fingerprinting (best-guess detection)
- MAC vendor lookup (manufacturer identification)
- Service banner grabbing
- Device classification with confidence scoring and explainable signals

**SNMP Discovery:**
- Auto-detect SNMP-capable devices during network scans
- Detected SNMP version and working community string
- Register SNMP devices for ongoing monitoring
- Poll SNMP data on demand
- Promote SNMP devices to full assets for tracking

**Device Actions:**
- Register discovered devices with custom details
- Promote to asset (create a trackable asset without an agent)
- Ignore devices to hide from future results
- Link to existing agents or assets
- Custom device types for specialized equipment

### 11. SNMP Monitoring (Agentless)

Monitor network infrastructure without installing agents.

- Register discovered SNMP devices for periodic polling
- Collect metrics, inventory, and heartbeat data via SNMP
- SNMP assets appear alongside agent-managed assets in a unified view
- Automatic offline detection for SNMP devices (5-minute threshold)
- View collected OID data and poll status

### 12. Locations

Organize your infrastructure geographically.

- Hierarchical location tree (Region > Building > Floor > Room > Rack)
- Location types: Region, Building, Floor, Room, Rack, Data Center, Warehouse, Office
- Assign agents and assets to locations
- Filter any page by location
- Map view with coordinate support (manual entry, address geocoding, or pick-on-map)
- Contact information per location (name, email, phone)
- View agents and assets assigned to each location

### 13. Groups & Tags

Organize agents logically, independent of physical location.

**Static Groups** -- Manually curated collections (e.g., "Production Servers", "Finance Department")
**Dynamic Groups** -- Rule-based auto-membership (e.g., "All Windows 11 machines", "All Linux servers")

**Tags** -- Key-value pairs attached to agents for flexible classification (e.g., department=Engineering, environment=production, owner=john.doe)

- Color-coded groups for visual identification
- Priority levels for deployment ordering
- Use groups and tags as deployment targets
- Combine with locations for precise targeting

### 14. User Management & Access Control

Role-based access ensures the right people have the right permissions.

| Role | Access Level |
|------|-------------|
| **Admin** | Full access to all features including user management and system settings |
| **Operator** | Deploy software, manage agents, create and run jobs |
| **Viewer** | Read-only access to all data |
| **Auditor** | Read-only access plus audit logs and compliance data |

- Create, edit, deactivate user accounts
- Password reset from the admin console
- JWT-based session management with configurable timeout

### 15. Notifications & Alerts

Stay informed about what matters in your infrastructure.

**In-App Notifications:**
- Bell icon with unread count in the header
- Agent events: new registration, approval needed, agent offline
- Job events: completed, failed
- Deployment events: progress, partial failure
- Decommissioned agent re-registration alerts

**Notification Categories:**
- Agent Requests -- pending approvals and re-registrations
- Jobs -- execution results
- Deployments -- progress and failures
- Patches -- patch-related alerts
- Dead Agents -- permanently ignored agents

**Quick Actions from Notifications:**
- Approve agents directly from the notification
- View deployment failure details inline
- Navigate to relevant pages with one click
- Mark as read, dismiss, or mark all as read

### 16. Dashboard

Real-time operational overview at a glance.

**Key Metrics:**
- Total Agents, Online Agents, Total Assets
- Pending Jobs, Failed Jobs (24h), Fleet Health percentage

**Charts:**
- Agent Activity (24-hour area chart)
- Agent Status Distribution (pie chart)
- Job History (7-day stacked bar chart -- completed vs failed vs pending)
- OS Distribution (pie chart)

**System Alerts:**
- Offline agents, failed jobs, unmanaged devices, stale agents

**Recent Activity Feed:**
- Agent registrations, job completions, and other system events in chronological order

**Quick Actions:**
- Jump to Manage Agents, Create Job, Software Catalog, or Network Discovery

### 17. Global Search

Find anything across your entire infrastructure from a single search bar.

- Search across Assets, Jobs, Agents, Locations, Groups, Packages, and Patches
- Results grouped by category with counts
- Click any result to navigate directly to its detail page
- Accessible from the header on every page

### 18. Server Migration

Safely migrate your entire agent fleet to a new server.

- Configure new server protocol (HTTP/HTTPS), IP, and port
- Select which agents to migrate
- **Safe Migration** mode: ping the new server from each agent before migrating to verify reachability
- View per-agent ping results before committing
- Partial migration option -- only migrate agents that can reach the new server
- Built-in safety checks to prevent accidental misconfiguration

### 19. Appearance & Customization

- **Light**, **Dark**, and **System** (auto-detect) themes
- Collapsible sidebar for more workspace
- Configurable auto-refresh intervals (10s, 30s, 1m, 5m)
- Responsive design for different screen sizes

### 20. Offline & Air-Gapped Network Support

InfraKnit is designed to work in restricted network environments.

- Self-hosted on-premises -- no cloud dependency
- APT repository served directly from the backend server
- APT operations use configurable timeouts and retry limits to prevent hangs on air-gapped networks
- Agents queue telemetry locally and retry when connectivity is restored
- Disk-backed job persistence on agents -- jobs survive agent restarts

---

## How It Works

```
                          ┌─────────────────────┐
                          │    Web Console       │
                          │  (Browser-based UI)  │
                          └─────────┬───────────┘
                                    │
                          ┌─────────▼───────────┐
                          │   Backend Server     │
                          │  (FastAPI + MySQL)   │
                          │                      │
                          │  REST API + APT Repo │
                          └──┬──────┬──────┬────┘
                             │      │      │
              ┌──────────────┤      │      ├──────────────┐
              │              │      │      │              │
     ┌────────▼──────┐ ┌────▼──────▼─┐ ┌─▼────────────┐ │
     │ Windows Agent │ │ Ubuntu Agent │ │ SNMP Devices │ │
     │ (.exe service)│ │ (systemd)   │ │ (agentless)  │ │
     └───────────────┘ └─────────────┘ └──────────────┘ │
                                                         │
                                              ┌──────────▼──┐
                                              │  Discovered  │
                                              │   Devices    │
                                              └─────────────┘
```

1. **Deploy agents** to your Windows and Ubuntu endpoints
2. Agents **auto-register** and await admin approval
3. Once approved, agents begin **collecting inventory** and **reporting metrics**
4. Use the web console to **deploy software**, **apply patches**, **run scripts**, and **monitor health**
5. **Discover** network devices and optionally monitor them via SNMP
6. **Schedule** recurring operations to keep your fleet up to date automatically

---

## Why InfraKnit?

| Advantage | Details |
|-----------|---------|
| **Cross-Platform** | Native agents for both Windows and Ubuntu Linux -- not just a Windows tool |
| **Self-Hosted** | Deploy on your own infrastructure; your data never leaves your network |
| **Offline-Ready** | Works in air-gapped and restricted network environments |
| **Built-in APT Repository** | Linux package deployment via proper APT infrastructure, not hacky file copies |
| **Automated Patch Discovery** | Automatically find available Linux updates across your fleet |
| **Modern Stack** | React + FastAPI + MySQL -- fast, maintainable, and extensible |
| **Easy Setup** | Get up and running quickly without complex infrastructure requirements |
| **Approval Workflow** | Multi-stage agent approval ensures no unauthorized device joins your fleet |
| **Unified View** | Agent-managed endpoints and SNMP devices in one console |
| **Full Audit Trail** | Every action, deployment, and state change is tracked |

---

## Feature Comparison

| Feature | InfraKnit | SCCM/Intune | PDQ Deploy | ManageEngine | NinjaRMM |
|---------|-----------|-------------|------------|--------------|----------|
| **Windows Support** | Yes | Yes | Yes | Yes | Yes |
| **Linux Support** | Yes (Ubuntu) | Limited | No | Yes | Yes |
| **Agent Management** | Yes | Yes | Yes | Yes | Yes |
| **Hardware Inventory** | Yes | Yes | Partial | Yes | Yes |
| **Software Inventory** | Yes | Yes | Yes | Yes | Yes |
| **Package Deployment** | Yes | Yes | Yes | Yes | Yes |
| **Patch Management** | Yes | Yes | Yes | Yes | Yes |
| **Auto Patch Discovery** | Yes (Linux) | Yes | No | Yes | Yes |
| **Service Packs/Bundles** | Yes | Yes | No | Yes | Limited |
| **Remote Scripts** | Yes | Yes | Yes | Yes | Yes |
| **Network Discovery** | Yes | Yes | No | Yes | Yes |
| **SNMP Monitoring** | Yes | Limited | No | Yes | Yes |
| **Scheduled Tasks** | Yes | Yes | Yes | Yes | Yes |
| **Groups & Tags** | Yes | Yes | Yes | Yes | Yes |
| **Location Management** | Yes (with maps) | Yes | No | Yes | Yes |
| **RBAC** | Yes | Yes | Basic | Yes | Yes |
| **Deployment Tracking** | Yes | Yes | Basic | Yes | Yes |
| **Self-Hosted Option** | Yes | No (Intune) | Yes | Yes | No |
| **Offline/Air-Gap** | Yes | No | Limited | Limited | No |
| **APT Repository** | Built-in | No | No | No | No |
| **REST API** | Yes | Yes | Limited | Yes | Yes |

---

**InfraKnit** -- Unified IT Infrastructure Management

Built for IT teams who need real control over their endpoints.
