# Homelab Proxmox VE ad Alta Affidabilità

![Proxmox VE](https://img.shields.io/badge/Proxmox_VE-Cluster_3_nodi-E57000?logo=proxmox&logoColor=white)
![HA](https://img.shields.io/badge/Alta_Affidabilità-Testata-success)
![Storage](https://img.shields.io/badge/Storage_Condiviso-TrueNAS-0095D5?logo=truenas&logoColor=white)
![AWS](https://img.shields.io/badge/AWS-EC2_%26_Security_Groups-232F3E?logo=amazonwebservices&logoColor=white)
[![LinkedIn](https://img.shields.io/badge/LinkedIn-Consolato_Malara-0A66C2?logo=linkedin&logoColor=white)](https://www.linkedin.com/in/consolatomalara/)

**Autore:** Consolato Malara  
**Versione inglese:** [proxmox-ha-homelab](https://github.com/cartesi0/proxmox-ha-homelab)

Questo repository documenta il mio **homelab personale** usato per studiare e fare pratica con virtualizzazione, Linux, networking, storage condiviso, migrazione live, alta affidabilità, monitoraggio, accesso remoto e infrastrutture Windows.

> È un portfolio di apprendimento: descrive ciò che ho configurato, testato e osservato realmente, senza presentarlo come architettura enterprise di produzione.

<p align="center">
  <img src="assets/architettura.svg" alt="Architettura homelab Proxmox" width="900">
</p>

## Obiettivi del laboratorio

- Comprendere cluster, quorum e Corosync
- Configurare storage condiviso con TrueNAS/NFS
- Testare migrazione VM e live migration
- Verificare il comportamento HA durante il guasto di un nodo
- Amministrare Linux e servizi infrastrutturali
- Accedere al laboratorio da remoto tramite WireGuard
- Fare pratica con DNS, Pi-hole, pfSense e Zabbix
- Usare AWS EC2 e Security Groups in scenari cloud
- Costruire un laboratorio Windows Server + Active Directory
- Documentare troubleshooting e risultati reali

## Architettura attuale

| Componente | Ruolo |
|---|---|
| Proxmox VE Nodo A | Nodo cluster |
| Proxmox VE Nodo B | Nodo cluster |
| Proxmox VE Nodo C | Nodo cluster |
| TrueNAS | Storage NFS condiviso |

**Per sicurezza non pubblico IP reali, hostname reali, username, password, chiavi, token, cookie, endpoint pubblici o altri identificativi operativi.**

## VM, container e servizi

<p align="center">
  <img src="assets/workload.svg" alt="Workload e servizi del laboratorio" width="1000">
</p>

Nel laboratorio sono presenti o in fase di utilizzo:

| Workload | Scopo |
|---|---|
| `n8n` | Automazione workflow |
| `pihole-ct` | DNS e filtraggio Pi-hole |
| `telegram-corner` | Automazioni Python / Telegram |
| `VPNWireguard` | Accesso remoto sicuro al laboratorio |
| `pfSense` | Firewall e segmentazione di rete |
| `Zabbix` | Monitoraggio infrastrutturale |
| `Ubuntu` | Test e amministrazione Linux |
| `Windows10client` | Client Windows per test e futuro dominio |
| `WinServer` | Windows Server e prossimo progetto Active Directory |
| `ha-test` | VM dedicata ai test di failover HA |

Dettagli: **[VM, container e servizi](docs/workload.md)**

## Esperienza cloud: AWS EC2

Ho esperienza pratica con **Amazon EC2** per l'esecuzione e la gestione di workload Linux e con gli **AWS Security Groups** per controllare il traffico in ingresso e in uscita dalle istanze.

Questa esperienza è parte del mio percorso Infrastructure/Cloud e completa il lavoro che svolgo nel laboratorio Proxmox.

Approfondimento: **[AWS EC2 e Security Groups](docs/aws-ec2.md)**

## Alta Affidabilità: cosa ho testato

- [x] Cluster Proxmox VE a 3 nodi
- [x] Quorum verificato
- [x] Storage NFS condiviso attivo
- [x] Migrazione manuale VM
- [x] Live migration
- [x] Risorsa HA configurata
- [x] Spegnimento fisico di un nodo per simulare un guasto
- [x] Riavvio automatico della VM su un altro nodo
- [x] Rientro del nodo nel cluster
- [x] Stato HA/fencing/watchdog verificato
- [x] Pi-hole installato
- [x] WireGuard per accesso remoto
- [x] Zabbix come laboratorio di monitoraggio
- [x] Pratica AWS EC2 e Security Groups
- [ ] Active Directory Domain Services
- [ ] Segmentazione di rete più avanzata
- [ ] Test backup e ripristino
- [ ] Automazione infrastrutturale

## Live migration e HA non sono la stessa cosa

**Live migration:** il nodo sorgente è sano e lo stato della VM viene trasferito verso un altro nodo.

**HA failover:** il nodo sorgente è improvvisamente indisponibile; lo stato RAM non è recuperabile e la VM viene riavviata su un altro nodo disponibile.

<p align="center">
  <img src="assets/ha-failover.svg" alt="Differenza tra live migration e HA failover" width="900">
</p>

## Prossimo progetto: Windows Server + Active Directory

Roadmap:

- [ ] Installazione AD DS
- [ ] Promozione del server a Domain Controller
- [ ] DNS integrato con Active Directory
- [ ] Utenti, gruppi e Organizational Unit
- [ ] Group Policy
- [ ] Join del client Windows al dominio
- [ ] Permessi NTFS e cartelle condivise
- [ ] Secondo Domain Controller e replica
- [ ] Monitoraggio Windows con Zabbix
- [ ] Backup e restore

## Documentazione

| Documento | Contenuto |
|---|---|
| [Architettura](docs/architettura.md) | Topologia e componenti del laboratorio |
| [VM e servizi](docs/workload.md) | Inventario logico e ruolo dei servizi |
| [Cluster](docs/cluster.md) | Cluster, Corosync e quorum |
| [Storage condiviso](docs/storage-condiviso.md) | TrueNAS/NFS e migrazione |
| [Alta Affidabilità](docs/alta-affidabilita.md) | Test di failover e comportamento HA |
| [Troubleshooting](docs/troubleshooting.md) | Problemi osservati e lezioni apprese |
| [Validazione](docs/validazione.md) | Evidenze tecniche sanificate |
| [AWS EC2](docs/aws-ec2.md) | Esperienza EC2 e Security Groups |
| [Esempi configurazione](configs/esempi/README.md) | Regole per pubblicare esempi senza segreti |

## Argomenti che sto praticando

- Amministrazione Proxmox VE
- Linux Debian/Ubuntu
- systemd, journalctl e SSH
- Cluster, Corosync e quorum
- Storage NFS condiviso
- Migrazione VM e live migration
- High Availability e failover
- DNS e Pi-hole
- WireGuard VPN
- pfSense e firewall
- Zabbix
- AWS EC2 e Security Groups
- Windows Server e Active Directory
- Troubleshooting infrastrutturale
- Documentazione tecnica

## Uso dell'IA

Uso strumenti di IA come **supporto allo studio e alla documentazione**: mi aiutano a organizzare gli appunti, rivedere spiegazioni e migliorare la chiarezza dei file Markdown. Le configurazioni, i test e gli output tecnici descritti nel progetto provengono dal mio ambiente di laboratorio. Mantengo nel repository solo materiale che riesco a comprendere e spiegare.

## Sicurezza e privacy

Nel repository pubblico non devono comparire:

- IP pubblici o indirizzi operativi reali
- username o account interni
- password
- chiavi SSH private o pubbliche
- chiavi WireGuard private o pubbliche
- API key e token
- cookie o session ID
- file `.env` con segreti
- endpoint pubblici sensibili

Quando serve mostrare una struttura uso nomi generici come `Nodo A`, `Nodo B`, `<IP_ESEMPIO>` o `<VALORE_RIMOSSO>`.

---

> Homelab personale in continua evoluzione, utilizzato per apprendere tramite configurazione, test, errori e troubleshooting reali.