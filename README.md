# 🌿 GoGreen Wireless

> **Private Emergency Communication Network powered by FRN (Federal Registration Number)**

[![License: MIT](https://img.shields.io/badge/License-MIT-green.svg)](LICENSE)
[![Status: Active](https://img.shields.io/badge/Status-Active-brightgreen)]()
[![Network: Emergency Comm](https://img.shields.io/badge/Network-Emergency%20Comm-red)]()

---

## 📡 Project Overview

GoGreen Wireless is a private, resilient emergency communication network designed to operate independently of commercial infrastructure. Built around a licensed FRN (Federal Registration Number) identity, GoGreen Wireless ensures reliable voice, data, and mesh communications during grid-down scenarios, natural disasters, and public safety emergencies — particularly across the Manhattan, NY metro corridor.

This repository contains the architecture, configuration, scripts, and documentation necessary to deploy and maintain the GoGreen Wireless network.

---

## 🎯 Mission

To provide a **sustainable, operator-led emergency communication backbone** that supports first responders, community coordinators, and licensed amateur/GMRS operators — keeping communities connected when commercial networks fail.

---

## ✨ Features

- 🔐 **FRN-Integrated Identity** — All transmissions and devices tied to a verified FCC FRN for legal compliance and accountability
- 📶 **Mesh Networking** — Self-healing, peer-to-peer node architecture with no single point of failure
- 🆘 **Emergency Priority Routing** — Automatic traffic prioritization for distress signals and emergency traffic
- 🔌 **Off-Grid Power Compatibility** — Designed for solar, battery, and generator-backed deployments
- 📻 **Multi-Band Support** — VHF, UHF, GMRS, and digital mode integration (Winlink, APRS, JS8Call)
- 🗺️ **APRS Position Reporting** — Real-time node location tracking on the APRS-IS network
- 📁 **Winlink Email Gateway** — Store-and-forward email capability over radio when internet is unavailable
- 🖥️ **Web-Based Dashboard** — Local network monitoring UI accessible without internet
- 🔒 **Encrypted Messaging Layer** — AES-256 encrypted packet messaging between nodes (where legally permitted)
- 🤝 **Interoperability** — Compatible with ARRL ARES, RACES, CERT, and served agency protocols

---

## 🏗️ Network Architecture

```
                        ┌─────────────────────────┐
                        │    GoGreen Wireless HQ   │
                        │   (Primary Gateway Node) │
                        │   FRN: 0038591772        │
                        └────────────┬────────────┘
                                     │
                    ┌────────────────┼────────────────┐
                    │                │                │
             ┌──────▼──────┐  ┌──────▼──────┐  ┌──────▼──────┐
             │  Node Alpha  │  │  Node Beta  │  │  Node Gamma  │
             │  (VHF Mesh) │  │ (UHF Relay) │  │ (APRS/DIGI) │
             └──────┬──────┘  └──────┬──────┘  └──────┬──────┘
                    │                │                │
             ┌──────▼──────┐  ┌──────▼──────┐  ┌──────▼──────┐
             │  Field Units │  │  Winlink GW │  │  End Clients │
             │  (Portable) │  │   (Email)   │  │  (GMRS/HT)  │
             └─────────────┘  └─────────────┘  └─────────────┘
```

### Node Types

| Node Type | Role | Frequency Band | Power Source |
|-----------|------|---------------|--------------|
| Gateway Node | Internet bridge, primary coordination | VHF/UHF | Grid + Battery Backup |
| Relay Node | Signal extension, mesh routing | UHF | Solar + Battery |
| APRS Digipeater | Position beaconing, packet routing | 144.390 MHz | Solar |
| Winlink RMS | Email gateway, store-and-forward | HF/VHF | Grid + Generator |
| Field Unit | Portable endpoint, event deployment | GMRS/VHF | Battery/Vehicle |

---

## 📋 FRN Integration

GoGreen Wireless operates under a valid **FCC Federal Registration Number (FRN)**, ensuring all licensed activities are traceable, compliant, and authorized.

### FRN Usage

- **Equipment Registration** — All transmitters registered under the FRN via FCC ULS
- **License Management** — GMRS license and any Part 90 authorizations filed under the FRN
- **Callsign Association** — Network callsigns linked to FRN holder identity
- **Compliance Reporting** — Renewal dates and regulatory filings tracked in `/docs/compliance/`

### FCC Resources

- FCC ULS License Manager: [https://wireless.fcc.gov/uls](https://wireless.fcc.gov/uls)
- FRN Registration: [https://apps.fcc.gov/coresWeb/](https://apps.fcc.gov/coresWeb/)
- GMRS Rules (Part 95E): [47 CFR Part 95E](https://www.ecfr.gov/current/title-47/chapter-I/subchapter-D/part-95)

> ⚠️ Never share your raw FRN credentials in this repository. Store them in `.env` files listed in `.gitignore`.

---

## 🚨 Emergency Communications Role

GoGreen Wireless is purpose-built to serve as an auxiliary communication resource during declared emergencies and community events.

### Served Agencies & Activations

- **Severe Weather Events** — Spotter coordination and shelter-in-place messaging
- **Power Grid Failures** — Mesh backbone remains operational on battery/solar
- **Mass Casualty / Disaster Response** — ICS-compliant net structure, RACES activation-ready
- **Community Events** — Tactical communication support for public safety personnel

### Net Operations

| Net Type | Frequency | Schedule | Purpose |
|----------|-----------|----------|---------|
| Primary Net | 146.520 MHz (Simplex) | On activation | Emergency coordination |
| Digital Net | Winlink / JS8Call | Continuous | Message traffic |
| APRS Net | 144.390 MHz | Continuous | Positioning & telemetry |
| Training Net | TBD | Weekly | Operator readiness |

### ICS Integration

GoGreen Wireless follows **NIMS/ICS communication protocols**:
- Assigned to Communications Unit (COMU) in ICS structure
- ICS-205 (Incident Radio Communications Plan) templates in `/docs/ics/`
- Interoperable with served agency frequencies and talk-groups

---

## ⚙️ Setup Instructions

### Prerequisites

- Linux-based OS (Ubuntu 22.04 LTS recommended) or Raspberry Pi OS
- Python 3.10+
- `git`, `pip`, `docker` (optional for containerized nodes)
- Valid FCC license and FRN
- Compatible radio hardware (see `/docs/hardware.md`)

### 1. Clone the Repository

```bash
git clone https://github.com/YOUR_USERNAME/gogreen-wireless.git
cd gogreen-wireless
```

### 2. Install Dependencies

```bash
pip install -r requirements.txt
```

### 3. Configure Environment Variables

```bash
cp .env.example .env
nano .env
```

Edit `.env` with your node-specific values:

```env
NODE_ID=alpha
NODE_CALLSIGN=YOUR_CALLSIGN
FRN_REFERENCE=STORED_SECURELY   # Reference only — never paste raw FRN
APRS_PASSCODE=XXXXX
WINLINK_GATEWAY=YES
MESH_FREQUENCY=5800
DASHBOARD_PORT=8080
```

### 4. Initialize the Node

```bash
python gogreen/init_node.py --config config/node_alpha.yaml
```

### 5. Start the Dashboard

```bash
python gogreen/dashboard.py
# Access at http://localhost:8080
```

### 6. Join the Mesh Network

```bash
python gogreen/mesh.py --join --gateway 192.168.x.x
```

### Docker Deployment (Optional)

```bash
docker-compose up -d
```

---

## 📁 Folder Structure

```
gogreen-wireless/
├── README.md
├── LICENSE
├── .env.example
├── .gitignore
├── requirements.txt
├── docker-compose.yml
│
├── gogreen/                    # Core application modules
│   ├── __init__.py
│   ├── init_node.py            # Node initialization script
│   ├── mesh.py                 # Mesh networking engine
│   ├── dashboard.py            # Web dashboard server
│   ├── aprs.py                 # APRS beacon & digi logic
│   ├── winlink.py              # Winlink gateway integration
│   ├── routing.py              # Emergency priority routing
│   └── crypto.py               # Encrypted messaging layer
│
├── config/                     # Per-node YAML configs
│   ├── node_alpha.yaml
│   ├── node_beta.yaml
│   └── node_gamma.yaml
│
├── scripts/                    # Deployment & utility scripts
│   ├── install.sh
│   ├── start_node.sh
│   └── backup_config.sh
│
├── docs/                       # Documentation
│   ├── hardware.md             # Supported radios & hardware
│   ├── compliance/             # FCC/FRN compliance records (template)
│   ├── ics/                    # ICS-205 templates
│   └── training/               # Operator training materials
│
├── tests/                      # Unit & integration tests
│   ├── test_mesh.py
│   ├── test_aprs.py
│   └── test_routing.py
│
└── logs/                       # Runtime logs (gitignored)
    └── .gitkeep
```

---

## 🔌 API Endpoints

The GoGreen Wireless dashboard exposes a local REST API for node management and monitoring.

> Base URL: `http://localhost:8080/api/v1`

| Method | Endpoint | Description |
|--------|----------|-------------|
| `GET` | `/status` | Overall node health and network status |
| `GET` | `/nodes` | List all active mesh nodes |
| `GET` | `/nodes/{id}` | Detail for a specific node |
| `POST` | `/nodes/register` | Register a new node to the mesh |
| `DELETE` | `/nodes/{id}` | Remove a node from the mesh |
| `GET` | `/aprs/beacon` | Current APRS beacon status |
| `POST` | `/aprs/beacon` | Trigger a manual APRS beacon |
| `GET` | `/winlink/queue` | View pending Winlink message queue |
| `POST` | `/winlink/send` | Inject a message into Winlink queue |
| `GET` | `/traffic` | Real-time network traffic stats |
| `GET` | `/alerts` | Active emergency alerts |
| `POST` | `/alerts` | Broadcast an emergency alert to the net |
| `GET` | `/logs` | Retrieve recent system logs |
| `POST` | `/config/reload` | Hot-reload node configuration |

### Example Response — `/api/v1/status`

```json
{
  "node_id": "alpha",
  "callsign": "WXXXXXXX",
  "status": "online",
  "uptime_seconds": 43200,
  "mesh_peers": 3,
  "aprs_last_beacon": "2026-06-22T14:30:00Z",
  "winlink_queue_depth": 2,
  "emergency_mode": false,
  "battery_percent": 87
}
```

---

## 🗺️ Future Roadmap

### Phase 1 — Foundation ✅ *(Current)*
- [x] Repository structure and documentation
- [x] FRN integration and compliance framework
- [x] Node configuration system (YAML-based)
- [x] Basic mesh networking prototype
- [x] APRS beaconing

### Phase 2 — Core Network 🔧 *(In Progress)*
- [ ] Full mesh self-healing routing engine
- [ ] Web dashboard with live node map
- [ ] Winlink RMS gateway deployment
- [ ] Emergency alert broadcast system
- [ ] JS8Call digital mode integration

### Phase 3 — Hardening 📅 *(Planned Q3 2026)*
- [ ] AES-256 encrypted messaging between nodes
- [ ] Offline-first dashboard (no internet required)
- [ ] Battery/solar telemetry monitoring
- [ ] ICS-205 auto-generation from active net data
- [ ] Mobile operator app (Android)

### Phase 4 — Expansion 📅 *(Planned Q4 2026)*
- [ ] Multi-city node deployment (NYC metro coverage)
- [ ] Served agency interoperability testing (CERT/ARES)
- [ ] HF Winlink integration for long-haul messaging
- [ ] Automated propagation & band condition feeds
- [ ] Public-facing network status page

### Phase 5 — Community 🌱 *(2027)*
- [ ] Open-source node firmware for Raspberry Pi
- [ ] Community operator onboarding portal
- [ ] Training curriculum and certification pathway
- [ ] Integration with municipal emergency management systems

---

## 🤝 Contributing

GoGreen Wireless is a private project. Authorized contributors only. Contact the repository owner to request access.

1. Fork the repository (authorized contributors)
2. Create a feature branch: `git checkout -b feature/your-feature`
3. Commit your changes: `git commit -m 'Add feature'`
4. Push to the branch: `git push origin feature/your-feature`
5. Open a Pull Request for review

---

## 📜 License

This project is licensed under the **MIT License** — see the [LICENSE](LICENSE) file for details.

---

## 📞 Contact & Net Info

- **Operator:** Ronald — GoGreen Wireless  
- **Location:** Manhattan, NY  
- **FCC FRN:** [Registered — stored securely]  
- **Primary Simplex:** 146.520 MHz  
- **APRS:** 144.390 MHz  
- **Digital:** Winlink / JS8Call

---

*GoGreen Wireless — Keeping communities connected when it matters most.* 🌿📡

