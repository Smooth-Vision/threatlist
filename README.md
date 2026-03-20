# 🛡️ SV-SIEM Threat IP Blocklist

> Automatisch gegenereerde threat intelligence blocklist, samengesteld uit meerdere open-source feeds.
> Geoptimaliseerd voor directe import in **FortiGate** firewalls via External Connectors.

---

## 📊 Overzicht

| | |
|:---|:---|
| 🔢 **Totaal unieke IPs** | **201,010** |
| 📦 **Chunk bestanden** | 8 bestanden (max 35,000 per bestand) |
| 🔄 **Laatste update** | 2026-03-20 05:15:50 UTC |
| 📡 **Actieve feeds** | `abuseipdb`, `abuseipdb_community`, `blocklist_de`, `cinsscore`, `datashield`, `et_compromised`, `feodo`, `tor_exit` |
---

## 📁 Bestandsstructuur

```
📂 Repository
├── 📄 README.md
├── 📄 SV-SIEM-All-ThreatIPs-IPv4.txt          ← Volledige lijst (201,010 IPs)
└── 📂 Fortigate/
    ├── 📄 sv-siem-block-01.txt                       ← max 35,000 IPs per bestand
    ├── 📄 sv-siem-block-02.txt
    ├── ...
    └── 📄 sv-siem-block-08.txt
```

---

## 🔧 FortiGate Configuratie

FortiGate External Connectors ondersteunen maximaal **~35.000 entries** per connector.
De blocklist is daarom automatisch opgesplitst in **8 bestanden**.

---

### ⚡ Stap 1 — External Connectors aanmaken

Ga naar **Security Fabric → External Connectors → Create New → IP Address** en maak
voor elk bestand een connector aan, of gebruik de CLI:

```
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
end
```

> ⏳ **Wacht 1-2 minuten** tot alle connectors hun eerste refresh hebben voltooid.
> Controleer de status via **Security Fabric → External Connectors** of CLI:
> `diagnose sys external-address-resource list`

---

### ⚡ Stap 2 — Firewall Policies aanmaken

Alle connectors kunnen direct als address objects in firewall policies gebruikt worden.

**Inkomend verkeer blokkeren** (threat IPs als bron):

```
config firewall policy
    edit 0
        set name "Block SV-SIEM Threats Inbound"
        set srcintf "any"
        set dstintf "any"
        set srcaddr "sv-siem-block-01" "sv-siem-block-02" "sv-siem-block-03" "sv-siem-block-04" "sv-siem-block-05" "sv-siem-block-06" "sv-siem-block-07" "sv-siem-block-08"
        set dstaddr "all"
        set action deny
        set service "ALL"
        set schedule "always"
        set match-vip enable
        set logtraffic all
    next
end
```

> `match-vip enable` zorgt ervoor dat deze policy ook verkeer naar Virtual IPs (port forwards) blokkeert.
> Zonder deze optie worden gepubliceerde diensten via VIPs **niet** beschermd.

**Uitgaand verkeer blokkeren** (verkeer naar threat IPs):

```
config firewall policy
    edit 0
        set name "Block SV-SIEM Threats Outbound"
        set srcintf "any"
        set dstintf "any"
        set srcaddr "all"
        set dstaddr "sv-siem-block-01" "sv-siem-block-02" "sv-siem-block-03" "sv-siem-block-04" "sv-siem-block-05" "sv-siem-block-06" "sv-siem-block-07" "sv-siem-block-08"
        set action deny
        set service "ALL"
        set schedule "always"
        set logtraffic all
    next
end
```

> ⚠️ **Belangrijk:** Plaats deze policies **bovenaan** je policy list (boven de allow-regels) zodat ze voorrang krijgen.
> Dit kan via de GUI (drag & drop) of via CLI met `move`.

---

---

## 🔗 Direct downloaden

| Bestand | Link |
|:---|:---|
| 📄 Volledige lijst | [`SV-SIEM-All-ThreatIPs-IPv4.txt`](https://raw.githubusercontent.com/Smooth-Vision/threatlist/main/SV-SIEM-All-ThreatIPs-IPv4.txt) |
| 📦 Chunk 1/8 | [`sv-siem-block-01.txt`](https://raw.githubusercontent.com/Smooth-Vision/threatlist/main/Fortigate/sv-siem-block-01.txt) |
| 📦 Chunk 2/8 | [`sv-siem-block-02.txt`](https://raw.githubusercontent.com/Smooth-Vision/threatlist/main/Fortigate/sv-siem-block-02.txt) |
| 📦 Chunk 3/8 | [`sv-siem-block-03.txt`](https://raw.githubusercontent.com/Smooth-Vision/threatlist/main/Fortigate/sv-siem-block-03.txt) |
| 📦 Chunk 4/8 | [`sv-siem-block-04.txt`](https://raw.githubusercontent.com/Smooth-Vision/threatlist/main/Fortigate/sv-siem-block-04.txt) |
| 📦 Chunk 5/8 | [`sv-siem-block-05.txt`](https://raw.githubusercontent.com/Smooth-Vision/threatlist/main/Fortigate/sv-siem-block-05.txt) |
| 📦 Chunk 6/8 | [`sv-siem-block-06.txt`](https://raw.githubusercontent.com/Smooth-Vision/threatlist/main/Fortigate/sv-siem-block-06.txt) |
| 📦 Chunk 7/8 | [`sv-siem-block-07.txt`](https://raw.githubusercontent.com/Smooth-Vision/threatlist/main/Fortigate/sv-siem-block-07.txt) |
| 📦 Chunk 8/8 | [`sv-siem-block-08.txt`](https://raw.githubusercontent.com/Smooth-Vision/threatlist/main/Fortigate/sv-siem-block-08.txt) |

---

## 🔄 Automatische updates

- Deze lijsten worden **elk uur** automatisch bijgewerkt door SV-SIEM
- FortiGate haalt de lijsten op volgens het ingestelde `refresh-rate` interval (standaard 60 minuten)
- Bij wijzigingen in het aantal IPs wordt het aantal chunk-bestanden automatisch aangepast
- De FortiGate CLI-commando's in deze README worden automatisch bijgewerkt

---

<sub>🤖 Automatisch gegenereerd door **SV-SIEM** op 2026-03-20 05:15:50 UTC</sub>
