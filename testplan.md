# Testplan – Assignment 36 (SRX Simple VLANs)

Udført 2026-10-27 på R1 / SRX-C1 (SRX240H-POE) med tre PC'er. Enheder og adresser som i [README](README.md).

| Enhed | Port | VLAN | MAC | Adresse |
|---|---|---|---|---|
| PC1 | ge-0/0/6 | SALES (10) | 34:5a:60:29:94:d8 | 192.168.10.1/24, statisk |
| PC4 | ge-0/0/5 | OPERATIONS (20) | 90:2e:16:68:ae:cd | 192.168.20.109/24, DHCP |
| PC5 | ge-0/0/3 | OPERATIONS (20) | 68:f7:28:be:46:85 | 192.168.20.5/24, statisk, DHCP-server (dnsmasq) |

Ingen af PC'erne har default gateway, da SRX-C1 kun switcher og der ikke er routing mellem VLANs.

## Tests

| # | Test | Fra | Kommando | Forventet | Resultat | Status |
|---|---|---|---|---|---|---|
| 1 | Porte i rigtige VLANs | SRX-C1 | `show ethernet-switching interfaces` | ge-0/0/3–5 i OPERATIONS, ge-0/0/6–7 i SALES, untagged | Som forventet; ge-0/0/3, 5 og 6 up | Bestået |
| 2 | MAC-adresser lært i rigtigt VLAN | SRX-C1 | `show ethernet-switching table` | PC5 og PC4 i OPERATIONS, PC1 i SALES | Som forventet | Bestået |
| 3 | PC4 får adresse via DHCP | PC4 | Forny DHCP | Adresse i 192.168.20.100–150 | Fik 192.168.20.109 | Bestået |
| 4 | Samme VLAN kan tale sammen | PC4 | `ping 192.168.20.5` | Svar | Svar | Bestået |
| 5 | Samme VLAN, modsat retning | PC5 | `ping 192.168.20.109` | Svar | Svar | Bestået |
| 6 | SALES kan ikke nå OPERATIONS | PC1 | `ping 192.168.20.5` | Ingen svar | Ingen svar | Bestået |
| 7 | SALES kan ikke nå OPERATIONS | PC1 | `ping 192.168.20.109` | Ingen svar | Ingen svar | Bestået |
| 8 | VLAN adskiller, ikke kun subnet | PC1 midlertidigt på 192.168.20.50/24 | `ping 192.168.20.5` | Ingen svar | Ingen svar | Bestået |
| 9 | DHCP krydser ikke VLAN | PC1 sat til DHCP | Forny DHCP | Ingen adresse fra PC5 | Ingen adresse | Bestået |

## Konklusion

PC4 og PC5 på OPERATIONS kan nå hinanden, og PC4 får adresse fra DHCP-serveren på PC5. PC1 på SALES kan ikke nå OPERATIONS, heller ikke med en adresse i samme subnet eller via DHCP-broadcast (test 8–9). Adskillelsen sker altså i SRX-C1's VLANs og ikke kun via forskellige subnets.
