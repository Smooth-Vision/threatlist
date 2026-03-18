# 🛡️ SV-SIEM Threat IP Blocklist

> Automatisch gegenereerde threat intelligence blocklist, samengesteld uit meerdere open-source feeds.
> Geoptimaliseerd voor directe import in **FortiGate** firewalls via External Connectors.

---

## 📊 Overzicht

| | |
|:---|:---|
| 🔢 **Totaal unieke IPs** | **445,281** |
| 📦 **Chunk bestanden** | 13 bestanden (max 35,000 per bestand) |
| 🔄 **Laatste update** | 2026-03-18 10:11:59 UTC |
| 📡 **Actieve feeds** | `abuseipdb`, `abuseipdb_community`, `blocklist_de`, `cinsscore`, `datashield`, `et_compromised`, `feodo`, `tor_exit` |
---

## 📁 Bestandsstructuur

```
📂 Repository
├── 📄 README.md
├── 📄 SV-SIEM-All-ThreatIPs-IPv4.txt          ← Volledige lijst (445,281 IPs)
└── 📂 Fortigate/
    ├── 📄 sv-siem-block-01.txt                       ← max 35,000 IPs per bestand
    ├── 📄 sv-siem-block-02.txt
    ├── ...
    └── 📄 sv-siem-block-13.txt
```

---

## 🔧 FortiGate Configuratie

FortiGate External Connectors ondersteunen maximaal **~35.000 entries** per connector.
De blocklist is daarom automatisch opgesplitst in **13 bestanden**.

---

### ⚡ Stap 1 — External Connectors aanmaken

Ga naar **Security Fabric → External Connectors → Create New → IP Address** en maak
voor elk bestand een connector aan, of gebruik de CLI:

```fortigate
config system external-resource
    edit "sv-siem-block-01"
        set type address
        set resource "https://raw.githubusercontent.com/Smooth-Vision/threatlist/main/Fortigate/sv-siem-block-01.txt"
        set refresh-rate 60
        set status enable
    next
    edit "sv-siem-block-02"
        set type address
        set resource "https://raw.githubusercontent.com/Smooth-Vision/threatlist/main/Fortigate/sv-siem-block-02.txt"
        set refresh-rate 60
        set status enable
    next
    edit "sv-siem-block-03"
        set type address
        set resource "https://raw.githubusercontent.com/Smooth-Vision/threatlist/main/Fortigate/sv-siem-block-03.txt"
        set refresh-rate 60
        set status enable
    next
    edit "sv-siem-block-04"
        set type address
        set resource "https://raw.githubusercontent.com/Smooth-Vision/threatlist/main/Fortigate/sv-siem-block-04.txt"
        set refresh-rate 60
        set status enable
    next
    edit "sv-siem-block-05"
        set type address
        set resource "https://raw.githubusercontent.com/Smooth-Vision/threatlist/main/Fortigate/sv-siem-block-05.txt"
        set refresh-rate 60
        set status enable
    next
    edit "sv-siem-block-06"
        set type address
        set resource "https://raw.githubusercontent.com/Smooth-Vision/threatlist/main/Fortigate/sv-siem-block-06.txt"
        set refresh-rate 60
        set status enable
    next
    edit "sv-siem-block-07"
        set type address
        set resource "https://raw.githubusercontent.com/Smooth-Vision/threatlist/main/Fortigate/sv-siem-block-07.txt"
        set refresh-rate 60
        set status enable
    next
    edit "sv-siem-block-08"
        set type address
        set resource "https://raw.githubusercontent.com/Smooth-Vision/threatlist/main/Fortigate/sv-siem-block-08.txt"
        set refresh-rate 60
        set status enable
    next
    edit "sv-siem-block-09"
        set type address
        set resource "https://raw.githubusercontent.com/Smooth-Vision/threatlist/main/Fortigate/sv-siem-block-09.txt"
        set refresh-rate 60
        set status enable
    next
    edit "sv-siem-block-10"
        set type address
        set resource "https://raw.githubusercontent.com/Smooth-Vision/threatlist/main/Fortigate/sv-siem-block-10.txt"
        set refresh-rate 60
        set status enable
    next
    edit "sv-siem-block-11"
        set type address
        set resource "https://raw.githubusercontent.com/Smooth-Vision/threatlist/main/Fortigate/sv-siem-block-11.txt"
        set refresh-rate 60
        set status enable
    next
    edit "sv-siem-block-12"
        set type address
        set resource "https://raw.githubusercontent.com/Smooth-Vision/threatlist/main/Fortigate/sv-siem-block-12.txt"
        set refresh-rate 60
        set status enable
    next
    edit "sv-siem-block-13"
        set type address
        set resource "https://raw.githubusercontent.com/Smooth-Vision/threatlist/main/Fortigate/sv-siem-block-13.txt"
        set refresh-rate 60
        set status enable
    next
end
```

> ⏳ **Wacht 1-2 minuten** tot alle connectors hun eerste refresh hebben voltooid.
> FortiGate maakt pas address objects aan na een succesvolle download.
> Controleer de status via **Security Fabric → External Connectors** — alle entries moeten groen zijn.

---

### ⚡ Stap 2 — Address Group aanmaken

Bundel alle connectors in een enkele Address Group:

```fortigate
config firewall addrgrp
    edit "SV-SIEM-Blocklist"
        set member "sv-siem-block-01" "sv-siem-block-02" "sv-siem-block-03" "sv-siem-block-04" "sv-siem-block-05" "sv-siem-block-06" "sv-siem-block-07" "sv-siem-block-08" "sv-siem-block-09" "sv-siem-block-10" "sv-siem-block-11" "sv-siem-block-12" "sv-siem-block-13"
        set comment "SV-SIEM Threat Intelligence - 445,281 IPs"
    next
end
```

---

### ⚡ Stap 3 — Firewall Policies

Gebruik de Address Group `SV-SIEM-Blocklist` in je firewall policies.

**Inkomend verkeer blokkeren** (threat IPs als bron):

```fortigate
config firewall policy
    edit 0
        set name "Block SV-SIEM Threats Inbound"
        set srcintf "any"
        set dstintf "any"
        set srcaddr "SV-SIEM-Blocklist"
        set dstaddr "all"
        set action deny
        set schedule "always"
        set logtraffic all
        set comments "SV-SIEM threat intelligence - inkomend verkeer"
    next
end
```

**Uitgaand verkeer blokkeren** (verkeer naar threat IPs):

```fortigate
config firewall policy
    edit 0
        set name "Block SV-SIEM Threats Outbound"
        set srcintf "any"
        set dstintf "any"
        set srcaddr "all"
        set dstaddr "SV-SIEM-Blocklist"
        set action deny
        set schedule "always"
        set logtraffic all
        set comments "SV-SIEM threat intelligence - uitgaand verkeer"
    next
end
```

> ⚠️ **Let op:** Plaats deze policies **boven** je reguliere allow-policies zodat ze voorrang krijgen.

---

---

## 🔗 Direct downloaden

| Bestand | Link |
|:---|:---|
| 📄 Volledige lijst | [`SV-SIEM-All-ThreatIPs-IPv4.txt`](https://raw.githubusercontent.com/Smooth-Vision/threatlist/main/SV-SIEM-All-ThreatIPs-IPv4.txt) |
| 📦 Chunk 1/13 | [`sv-siem-block-01.txt`](https://raw.githubusercontent.com/Smooth-Vision/threatlist/main/Fortigate/sv-siem-block-01.txt) |
| 📦 Chunk 2/13 | [`sv-siem-block-02.txt`](https://raw.githubusercontent.com/Smooth-Vision/threatlist/main/Fortigate/sv-siem-block-02.txt) |
| 📦 Chunk 3/13 | [`sv-siem-block-03.txt`](https://raw.githubusercontent.com/Smooth-Vision/threatlist/main/Fortigate/sv-siem-block-03.txt) |
| 📦 Chunk 4/13 | [`sv-siem-block-04.txt`](https://raw.githubusercontent.com/Smooth-Vision/threatlist/main/Fortigate/sv-siem-block-04.txt) |
| 📦 Chunk 5/13 | [`sv-siem-block-05.txt`](https://raw.githubusercontent.com/Smooth-Vision/threatlist/main/Fortigate/sv-siem-block-05.txt) |
| 📦 Chunk 6/13 | [`sv-siem-block-06.txt`](https://raw.githubusercontent.com/Smooth-Vision/threatlist/main/Fortigate/sv-siem-block-06.txt) |
| 📦 Chunk 7/13 | [`sv-siem-block-07.txt`](https://raw.githubusercontent.com/Smooth-Vision/threatlist/main/Fortigate/sv-siem-block-07.txt) |
| 📦 Chunk 8/13 | [`sv-siem-block-08.txt`](https://raw.githubusercontent.com/Smooth-Vision/threatlist/main/Fortigate/sv-siem-block-08.txt) |
| 📦 Chunk 9/13 | [`sv-siem-block-09.txt`](https://raw.githubusercontent.com/Smooth-Vision/threatlist/main/Fortigate/sv-siem-block-09.txt) |
| 📦 Chunk 10/13 | [`sv-siem-block-10.txt`](https://raw.githubusercontent.com/Smooth-Vision/threatlist/main/Fortigate/sv-siem-block-10.txt) |
| 📦 Chunk 11/13 | [`sv-siem-block-11.txt`](https://raw.githubusercontent.com/Smooth-Vision/threatlist/main/Fortigate/sv-siem-block-11.txt) |
| 📦 Chunk 12/13 | [`sv-siem-block-12.txt`](https://raw.githubusercontent.com/Smooth-Vision/threatlist/main/Fortigate/sv-siem-block-12.txt) |
| 📦 Chunk 13/13 | [`sv-siem-block-13.txt`](https://raw.githubusercontent.com/Smooth-Vision/threatlist/main/Fortigate/sv-siem-block-13.txt) |

---

## 🔄 Automatische updates

- Deze lijsten worden **elk uur** automatisch bijgewerkt door SV-SIEM
- FortiGate haalt de lijsten op volgens het ingestelde `refresh-rate` interval (standaard 60 minuten)
- Bij wijzigingen in het aantal IPs wordt het aantal chunk-bestanden automatisch aangepast
- De FortiGate CLI-commando's in deze README worden automatisch bijgewerkt

---

<sub>🤖 Automatisch gegenereerd door **SV-SIEM** op 2026-03-18 10:11:59 UTC</sub>
