# 📡 AREDNstack

**Complete toolkit for deploying SIP phone and network infrastructure on [AREDN](https://www.arednmesh.org/) amateur radio mesh networks.**

Everything you need to set up voice communications, phone provisioning, and network services on your AREDN mesh — from router configuration to SIP phone firmware.

> 💡 **Recommendation:** Use the **MikroTik HAP AC3** instead of the HAP AC Lite when starting with AREDN.

---

## 🏗️ What's Inside

```
AREDNstack/
├── 📖 Documentation/          Setup guides, phone configs, virtualization
│   ├── AREDN SetupV2.3        Complete deployment guide (PDF & DOCX)
│   ├── Phones/                 Snom SIP phone setup
│   └── Routers/                Proxmox virtualization guides
├── 📱 Firmware Repo/           Yealink SIP phone firmware & templates
│   └── Yealink phones Firmware/
│       ├── T41P, T41S, T42
│       ├── T46G, T46S
│       ├── T48G, T48S
│       └── T58A
├── 🕐 NTP Server/             GPS-disciplined NTP server build guide
├── 🔧 Tools/                  Network utilities
│   ├── MobaXterm/              SSH & terminal client
│   └── pxesrv/                 PXE boot server for provisioning
└── 💡 Hardwaredeployment ideas
```

---

## 📖 Documentation

| Guide | Format | Description |
|-------|--------|-------------|
| **AREDN Setup v2.3** | 📄 PDF, DOCX | Complete English setup guide — router config, phonebook replication, network deployment |
| **Snom Setup** | 📄 PDF | Configuring Snom SIP phones on AREDN mesh |
| **AREDN on Proxmox** | 📄 PDF, DOCX, PPTM | Running AREDN nodes in Proxmox virtual environments |
| **5.8 GHz Channels** | 📊 XLSX | Channel planning reference for 5.8 GHz links |
| **NTP Server** | 📄 PDF | Building a GPS-disciplined NTP server with DS3231 RTC + Ublox-7 (German) |

---

## 📱 Supported SIP Phones

### Yealink T-Series

Pre-packaged firmware and a template configuration file for quick provisioning:

| Model | Series | Display |
|-------|--------|---------|
| **T41P** / **T41S** | Entry | 2.7" LCD |
| **T42** | Mid-range | 2.7" LCD |
| **T46G** / **T46S** | Executive | 4.3" Color LCD |
| **T48G** / **T48S** | Premium | 7" Color Touchscreen |
| **T58A** | Smart | 7" Color Touchscreen, Android |

### Snom

Setup documentation included — see `Documentation/Phones/`.

---

## 🚀 Getting Started

1. **Choose your router** — MikroTik HAP AC3 recommended
2. **Flash AREDN firmware** — Follow the [AREDN documentation](https://docs.arednmesh.org/)
3. **Read the setup guide** — Open `Documentation/AREDN SetupV2.3-English.pdf`
4. **Provision SIP phones** — Use the firmware and templates from `Firmware Repo/`
5. **Install the Phonebook** — See [AREDN-Phonebook](https://github.com/swissdigitalnet/AREDN-Phonebook) for the SIP directory server

---

## 🔧 Tools

| Tool | Purpose |
|------|---------|
| **MobaXterm** | SSH terminal client for managing AREDN nodes remotely |
| **pxesrv** | PXE boot server for network-based device provisioning |

---

## 🔗 Related Projects

| Project | Description |
|---------|-------------|
| [**AREDN-Phonebook**](https://github.com/swissdigitalnet/AREDN-Phonebook) | SIP directory server and network monitoring dashboard for AREDN mesh (OpenWrt IPK) |
| [**AREDN**](https://github.com/aredn) | Amateur Radio Emergency Data Network firmware |

---

## 📻 License

This project is intended for amateur radio operators. Usage must comply with your local amateur radio regulations.

---

## 🤝 Contributing

Contributions are welcome! Whether it's documentation updates, new phone firmware templates, or deployment guides — feel free to open an issue or submit a pull request.
