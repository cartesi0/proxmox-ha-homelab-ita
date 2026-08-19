# Validazione del cluster

Questa pagina contiene **evidenze reali raccolte dal laboratorio e sanificate prima della pubblicazione**.

<p align="center">
  <img src="../assets/stato-cluster.svg" alt="Snapshot sanificato del cluster" width="950">
</p>

## Snapshot

- Data acquisizione: **19 agosto 2026**
- Piattaforma: Proxmox VE / Linux
- Corosync transport: `knet`
- Secure authentication: attiva

## 1. Cluster e quorum

Comando:

```bash
pvecm status
```

Estratto sanificato:

```text
Cluster information
-------------------
Name:             <CLUSTER_LAB>
Config Version:   3
Transport:        knet
Secure auth:      on

Quorum information
------------------
Nodes:            3
Quorate:          Yes

Votequorum information
----------------------
Expected votes:   3
Highest expected: 3
Total votes:      3
Quorum:           2
Flags:            Quorate
```

### Risultato

Il cluster risultava composto da **3 nodi**, con **3 voti totali** e quorum fissato a **2 voti**.

## 2. Membership

Comando:

```bash
pvecm nodes
```

Estratto sanificato:

```text
Nodo A    1 voto
Nodo B    1 voto
Nodo C    1 voto
```

### Risultato

Tutti i tre nodi attesi erano presenti nella membership del cluster.

## 3. Storage

Comando:

```bash
pvesm status
```

Estratto sanificato:

```text
Name            Type      Status
local           dir       active
local-lvm       lvmthin   active
truenas         nfs       active
```

### Risultato

Lo storage NFS TrueNAS risultava **attivo** e visibile dal cluster.

## 4. High Availability

Comando:

```bash
ha-manager status
```

Estratto sanificato:

```text
quorum OK
master <NODO_A> (active) - dynamic load CRS (load imbalance: 2.9%)
fencing armed (CRM watchdog active)
lrm <NODO_A> (...)
lrm <NODO_B> (...)
lrm <NODO_C> (...)
service <VM_HA> (<NODO_B>, stopped)
```

### Risultato

Nel momento dello snapshot:

- quorum OK;
- HA CRM attivo;
- fencing armato;
- watchdog attivo/visibile;
- risorsa HA registrata;
- `dynamic load CRS` riportato dal sistema;
- load imbalance del **2.9%**;
- la risorsa HA era **stopped** al momento della cattura.

L'ultimo punto è importante: lo snapshot dimostra l'esistenza della risorsa HA, ma non afferma che la VM fosse in esecuzione in quel preciso momento.

## Cosa valida questa evidenza

- [x] 3 membri cluster
- [x] quorum presente
- [x] 3 voti disponibili
- [x] storage TrueNAS NFS attivo
- [x] HA CRM attivo
- [x] fencing armato
- [x] watchdog visibile
- [x] risorsa HA registrata
- [x] stato CRS visibile

## Regola di pubblicazione

Gli output pubblici non contengono IP reali, hostname reali, username, password, chiavi SSH o WireGuard, API key, token, cookie, endpoint pubblici o altri identificativi operativi.