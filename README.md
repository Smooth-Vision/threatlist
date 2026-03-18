# SV-SIEM Threat IP Blocklist

Automatisch gegenereerde threat intelligence blocklist door [SV-SIEM](https://github.com/Smooth-Vision/threatlist).

| | |
|---|---|
| **Totaal IPs** | 445,281 |
| **Aantal bestanden** | 13 chunks van max 35,000 |
| **Laatste update** | 2026-03-18 09:54:26 UTC |
| **Feeds** | abuseipdb, abuseipdb_community, blocklist_de, cinsscore, datashield, et_compromised, feodo, tor_exit |

---

## Bestandsstructuur

```
SV-SIEM-All-ThreatIPs-IPv4.txt          # Volledige lijst (445,281 IPs)
Fortigate/
  sv-siem-block-01.txt                   # Chunk 1 (max 35,000 IPs)
  sv-siem-block-02.txt                   # Chunk 2
  ...
  sv-siem-block-13.txt                   # Chunk 13
```

---

## FortiGate Configuratie

FortiGate External Connectors ondersteunen maximaal ~35.000 entries per connector.
De blocklist is daarom opgesplitst in **13 bestanden**.

### Stap 1 — External Connectors aanmaken

Ga naar **Security Fabric > External Connectors > Create New > IP Address** en maak
voor elk bestand een connector aan. Of gebruik de CLI:

```
config system external-resource
    edit "sv-siem-block-01"
        set type ip
        set resource "https://raw.githubusercontent.com/Smooth-Vision/threatlist/main/Fortigate/sv-siem-block-01.txt"
        set refresh-rate 60
        set status enable
    next
    edit "sv-siem-block-02"
        set type ip
        set resource "https://raw.githubusercontent.com/Smooth-Vision/threatlist/main/Fortigate/sv-siem-block-02.txt"
        set refresh-rate 60
        set status enable
    next
    edit "sv-siem-block-03"
        set type ip
        set resource "https://raw.githubusercontent.com/Smooth-Vision/threatlist/main/Fortigate/sv-siem-block-03.txt"
        set refresh-rate 60
        set status enable
    next
    edit "sv-siem-block-04"
        set type ip
        set resource "https://raw.githubusercontent.com/Smooth-Vision/threatlist/main/Fortigate/sv-siem-block-04.txt"
        set refresh-rate 60
        set status enable
    next
    edit "sv-siem-block-05"
        set type ip
        set resource "https://raw.githubusercontent.com/Smooth-Vision/threatlist/main/Fortigate/sv-siem-block-05.txt"
        set refresh-rate 60
        set status enable
    next
    edit "sv-siem-block-06"
        set type ip
        set resource "https://raw.githubusercontent.com/Smooth-Vision/threatlist/main/Fortigate/sv-siem-block-06.txt"
        set refresh-rate 60
        set status enable
    next
    edit "sv-siem-block-07"
        set type ip
        set resource "https://raw.githubusercontent.com/Smooth-Vision/threatlist/main/Fortigate/sv-siem-block-07.txt"
        set refresh-rate 60
        set status enable
    next
    edit "sv-siem-block-08"
        set type ip
        set resource "https://raw.githubusercontent.com/Smooth-Vision/threatlist/main/Fortigate/sv-siem-block-08.txt"
        set refresh-rate 60
        set status enable
    next
    edit "sv-siem-block-09"
        set type ip
        set resource "https://raw.githubusercontent.com/Smooth-Vision/threatlist/main/Fortigate/sv-siem-block-09.txt"
        set refresh-rate 60
        set status enable
    next
    edit "sv-siem-block-10"
        set type ip
        set resource "https://raw.githubusercontent.com/Smooth-Vision/threatlist/main/Fortigate/sv-siem-block-10.txt"
        set refresh-rate 60
        set status enable
    next
    edit "sv-siem-block-11"
        set type ip
        set resource "https://raw.githubusercontent.com/Smooth-Vision/threatlist/main/Fortigate/sv-siem-block-11.txt"
        set refresh-rate 60
        set status enable
    next
    edit "sv-siem-block-12"
        set type ip
        set resource "https://raw.githubusercontent.com/Smooth-Vision/threatlist/main/Fortigate/sv-siem-block-12.txt"
        set refresh-rate 60
        set status enable
    next
    edit "sv-siem-block-13"
        set type ip
        set resource "https://raw.githubusercontent.com/Smooth-Vision/threatlist/main/Fortigate/sv-siem-block-13.txt"
        set refresh-rate 60
        set status enable
    next
end
```

### Stap 2 — Address Group aanmaken

Bundel alle connectors in een enkele Address Group:

```
config firewall addrgrp
    edit "SV-SIEM-Blocklist"
        set member "sv-siem-block-01" "sv-siem-block-02" "sv-siem-block-03" "sv-siem-block-04" "sv-siem-block-05" "sv-siem-block-06" "sv-siem-block-07" "sv-siem-block-08" "sv-siem-block-09" "sv-siem-block-10" "sv-siem-block-11" "sv-siem-block-12" "sv-siem-block-13"
        set comment "SV-SIEM Threat Intelligence - 445,281 IPs"
    next
end
```

### Stap 3 — Firewall Policy

Gebruik de Address Group `SV-SIEM-Blocklist` in je firewall policies:

```
config firewall policy
    edit 0
        set name "Block SV-SIEM Threats"
        set srcintf "any"
        set dstintf "any"
        set srcaddr "SV-SIEM-Blocklist"
        set dstaddr "all"
        set action deny
        set schedule "always"
        set logtraffic all
        set comments "Automatische blokkering op basis van SV-SIEM threat intelligence"
    next
end
```

> **Tip:** Maak ook een policy aan met `dstaddr "SV-SIEM-Blocklist"` om uitgaand
> verkeer naar bekende threat IPs te blokkeren.

### Stap 4 — Volledig script (copy-paste)

Onderstaand script maakt alle connectors en de Address Group in een keer aan:

```
config system external-resource
    edit "sv-siem-block-01"
        set type ip
        set resource "https://raw.githubusercontent.com/Smooth-Vision/threatlist/main/Fortigate/sv-siem-block-01.txt"
        set refresh-rate 60
        set status enable
    next
    edit "sv-siem-block-02"
        set type ip
        set resource "https://raw.githubusercontent.com/Smooth-Vision/threatlist/main/Fortigate/sv-siem-block-02.txt"
        set refresh-rate 60
        set status enable
    next
    edit "sv-siem-block-03"
        set type ip
        set resource "https://raw.githubusercontent.com/Smooth-Vision/threatlist/main/Fortigate/sv-siem-block-03.txt"
        set refresh-rate 60
        set status enable
    next
    edit "sv-siem-block-04"
        set type ip
        set resource "https://raw.githubusercontent.com/Smooth-Vision/threatlist/main/Fortigate/sv-siem-block-04.txt"
        set refresh-rate 60
        set status enable
    next
    edit "sv-siem-block-05"
        set type ip
        set resource "https://raw.githubusercontent.com/Smooth-Vision/threatlist/main/Fortigate/sv-siem-block-05.txt"
        set refresh-rate 60
        set status enable
    next
    edit "sv-siem-block-06"
        set type ip
        set resource "https://raw.githubusercontent.com/Smooth-Vision/threatlist/main/Fortigate/sv-siem-block-06.txt"
        set refresh-rate 60
        set status enable
    next
    edit "sv-siem-block-07"
        set type ip
        set resource "https://raw.githubusercontent.com/Smooth-Vision/threatlist/main/Fortigate/sv-siem-block-07.txt"
        set refresh-rate 60
        set status enable
    next
    edit "sv-siem-block-08"
        set type ip
        set resource "https://raw.githubusercontent.com/Smooth-Vision/threatlist/main/Fortigate/sv-siem-block-08.txt"
        set refresh-rate 60
        set status enable
    next
    edit "sv-siem-block-09"
        set type ip
        set resource "https://raw.githubusercontent.com/Smooth-Vision/threatlist/main/Fortigate/sv-siem-block-09.txt"
        set refresh-rate 60
        set status enable
    next
    edit "sv-siem-block-10"
        set type ip
        set resource "https://raw.githubusercontent.com/Smooth-Vision/threatlist/main/Fortigate/sv-siem-block-10.txt"
        set refresh-rate 60
        set status enable
    next
    edit "sv-siem-block-11"
        set type ip
        set resource "https://raw.githubusercontent.com/Smooth-Vision/threatlist/main/Fortigate/sv-siem-block-11.txt"
        set refresh-rate 60
        set status enable
    next
    edit "sv-siem-block-12"
        set type ip
        set resource "https://raw.githubusercontent.com/Smooth-Vision/threatlist/main/Fortigate/sv-siem-block-12.txt"
        set refresh-rate 60
        set status enable
    next
    edit "sv-siem-block-13"
        set type ip
        set resource "https://raw.githubusercontent.com/Smooth-Vision/threatlist/main/Fortigate/sv-siem-block-13.txt"
        set refresh-rate 60
        set status enable
    next
end

config firewall addrgrp
    edit "SV-SIEM-Blocklist"
        set member "sv-siem-block-01" "sv-siem-block-02" "sv-siem-block-03" "sv-siem-block-04" "sv-siem-block-05" "sv-siem-block-06" "sv-siem-block-07" "sv-siem-block-08" "sv-siem-block-09" "sv-siem-block-10" "sv-siem-block-11" "sv-siem-block-12" "sv-siem-block-13"
        set comment "SV-SIEM Threat Intelligence - 445,281 IPs"
    next
end
```

---

## Direct downloaden

| Bestand | URL |
|---|---|
| Volledige lijst | `https://raw.githubusercontent.com/Smooth-Vision/threatlist/main/SV-SIEM-All-ThreatIPs-IPv4.txt` |
| sv-siem-block-01 | `https://raw.githubusercontent.com/Smooth-Vision/threatlist/main/Fortigate/sv-siem-block-01.txt` |
| sv-siem-block-02 | `https://raw.githubusercontent.com/Smooth-Vision/threatlist/main/Fortigate/sv-siem-block-02.txt` |
| sv-siem-block-03 | `https://raw.githubusercontent.com/Smooth-Vision/threatlist/main/Fortigate/sv-siem-block-03.txt` |
| sv-siem-block-04 | `https://raw.githubusercontent.com/Smooth-Vision/threatlist/main/Fortigate/sv-siem-block-04.txt` |
| sv-siem-block-05 | `https://raw.githubusercontent.com/Smooth-Vision/threatlist/main/Fortigate/sv-siem-block-05.txt` |
| sv-siem-block-06 | `https://raw.githubusercontent.com/Smooth-Vision/threatlist/main/Fortigate/sv-siem-block-06.txt` |
| sv-siem-block-07 | `https://raw.githubusercontent.com/Smooth-Vision/threatlist/main/Fortigate/sv-siem-block-07.txt` |
| sv-siem-block-08 | `https://raw.githubusercontent.com/Smooth-Vision/threatlist/main/Fortigate/sv-siem-block-08.txt` |
| sv-siem-block-09 | `https://raw.githubusercontent.com/Smooth-Vision/threatlist/main/Fortigate/sv-siem-block-09.txt` |
| sv-siem-block-10 | `https://raw.githubusercontent.com/Smooth-Vision/threatlist/main/Fortigate/sv-siem-block-10.txt` |
| sv-siem-block-11 | `https://raw.githubusercontent.com/Smooth-Vision/threatlist/main/Fortigate/sv-siem-block-11.txt` |
| sv-siem-block-12 | `https://raw.githubusercontent.com/Smooth-Vision/threatlist/main/Fortigate/sv-siem-block-12.txt` |
| sv-siem-block-13 | `https://raw.githubusercontent.com/Smooth-Vision/threatlist/main/Fortigate/sv-siem-block-13.txt` |

---

## Automatische updates

Deze lijsten worden **elk uur** automatisch bijgewerkt door SV-SIEM.
FortiGate haalt de lijsten op volgens het ingestelde `refresh-rate` interval (standaard 60 minuten).

Bij wijzigingen in het aantal IPs kan het aantal chunk-bestanden veranderen.
Het FortiGate-script en de Address Group worden automatisch aangepast in deze README.

---

*Gegenereerd door SV-SIEM op 2026-03-18 09:54:26 UTC*
