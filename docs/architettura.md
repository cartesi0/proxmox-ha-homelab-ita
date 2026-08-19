# Architettura del laboratorio

## Panoramica

Il laboratorio è costruito intorno a un cluster Proxmox VE a tre nodi con storage condiviso TrueNAS. L'obiettivo è studiare concetti reali di virtualizzazione e infrastruttura mantenendo una struttura abbastanza semplice da poter essere compresa, modificata e testata.

<p align="center">
  <img src="../assets/architettura.svg" alt="Architettura homelab Proxmox" width="900">
</p>

## Componenti principali

| Componente | Ruolo |
|---|---|
| Proxmox Nodo A | Cluster e workload |
| Proxmox Nodo B | Cluster e workload |
| Proxmox Nodo C | Cluster e workload |
| TrueNAS | Storage NFS condiviso |

Gli identificativi reali di rete e degli host non sono pubblicati.

## Livello cluster

I tre host partecipano allo stesso cluster Proxmox VE. Corosync gestisce membership e comunicazione tra nodi; il quorum consente al cluster di prendere decisioni in modo sicuro quando un nodo diventa indisponibile.

## Livello storage

TrueNAS fornisce storage raggiungibile da più nodi Proxmox.

La configurazione attuale comprende:

- **3 dischi in un vdev ZFS RAIDZ1**;
- **1 disco aggiuntivo configurato come spare**;
- un **dataset dedicato a Proxmox**;
- condivisione del dataset tramite **NFS**;
- storage NFS aggiunto a Proxmox come **storage condiviso** del cluster.

Quando il disco di una VM è sullo storage condiviso, la VM può essere spostata tra host senza dover ricopiare ogni volta l'intero disco virtuale.

RAIDZ1 utilizza parità singola sul vdev a tre dischi. Il disco spare è capacità di sostituzione disponibile in caso di guasto: non rappresenta una seconda parità e non rende la configurazione equivalente a RAIDZ2.

Approfondimento: [Storage condiviso con TrueNAS](storage-condiviso.md).

## Livello servizi

Il cluster ospita servizi e workload usati per fare pratica:

- Pi-hole per DNS e filtraggio
- WireGuard per accesso remoto
- Zabbix per monitoraggio
- pfSense per firewall e segmentazione
- n8n e Telegram/Python per automazione
- Ubuntu per amministrazione Linux
- Windows Server e Windows Client per il futuro laboratorio Active Directory
- VM dedicata ai test HA

## Accesso remoto

WireGuard viene usato per amministrare il laboratorio da remoto attraverso una VPN cifrata, evitando di esporre direttamente l'interfaccia di gestione Proxmox su Internet.

## Monitoraggio

Zabbix è il laboratorio di monitoraggio per host, risorse e servizi. La roadmap comprende disponibilità, CPU, memoria, storage, rete e alerting.

## Esperienza cloud collegata

In parallelo al laboratorio on-premises, utilizzo AWS EC2 per fare pratica con workload Linux e Security Groups.

## Progetto Windows previsto

Il prossimo progetto importante è un piccolo dominio Microsoft:

```mermaid
flowchart LR
    DC[Windows Server]
    AD[Active Directory]
    DNS[DNS AD]
    GPO[Group Policy]
    CLIENT[Windows Client]
    DC --> AD
    DC --> DNS
    AD --> GPO
    CLIENT -->|Domain Join| AD
```

## Scenari già documentati

- [x] Cluster Proxmox a tre nodi
- [x] Storage NFS condiviso
- [x] Dataset TrueNAS dedicato a Proxmox
- [x] Layout ZFS RAIDZ1 con disco spare documentato
- [x] Migrazione e live migration
- [x] Failover HA
- [x] Pi-hole, WireGuard e Zabbix
- [x] Pratica AWS EC2 e Security Groups
- [ ] Test controllato di sostituzione disco ZFS
- [ ] Dominio Active Directory
- [ ] Segmentazione di rete più avanzata

## Sicurezza della documentazione

La versione pubblica usa nomi generici. Non vengono inseriti IP pubblici, IP operativi reali, hostname reali, username, password, chiavi SSH/WireGuard, token o endpoint sensibili.