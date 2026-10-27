# Assignment 36 – SRX Simple VLANs

Konfiguration til SRX240 i JSON: `r1-config.json`.

## Routeren

- **R1 / SRX-C1** – bruges som switch med to VLANs, SALES og OPERATIONS. VLANs er ikke forbundet, så der er ingen trafik mellem dem. PC5 er DHCP-server på OPERATIONS. Testet med tre PC'er: PC1 på SALES og PC4 og PC5 på OPERATIONS.

## Adresser (LLD)

| VLAN | ID | Netværk | Port | Enhed | Adresse |
|---|---|---|---|---|---|
| SALES | 10 | 192.168.10.0/24 | ge-0/0/6 | PC1 | 192.168.10.1/24 (statisk) |
| SALES | 10 | 192.168.10.0/24 | ge-0/0/7 | – | ubrugt |
| OPERATIONS | 20 | 192.168.20.0/24 | ge-0/0/3 | PC5 DHCP-server | 192.168.20.5/24 (statisk) |
| OPERATIONS | 20 | 192.168.20.0/24 | ge-0/0/4 | – | ubrugt |
| OPERATIONS | 20 | 192.168.20.0/24 | ge-0/0/5 | PC4 | DHCP .100–.150 |
