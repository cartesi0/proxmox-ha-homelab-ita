# Cluster Proxmox VE

## Scopo

Il laboratorio usa tre host Proxmox VE nello stesso cluster per studiare gestione centralizzata, Corosync, quorum, migrazione e High Availability.

## Nodi

| Nodo | Ruolo |
|---|---|
| Nodo A | Membro cluster |
| Nodo B | Membro cluster |
| Nodo C | Membro cluster |

Gli hostname e gli indirizzi reali sono stati sostituiti con nomi generici.

## Corosync e quorum

La validazione reale del cluster ha mostrato:

- 3 nodi visibili
- 3 voti totali
- quorum a 2 voti
- stato `Quorate: Yes`
- trasporto `knet`
- autenticazione sicura attiva

Comandi principali:

```bash
pvecm status
pvecm nodes
```

## Verifica servizi cluster

```bash
systemctl status corosync
systemctl status pve-cluster
systemctl status pve-ha-lrm
systemctl status pve-ha-crm
```

## Storage condiviso

TrueNAS fornisce storage NFS condiviso al cluster. La verifica viene eseguita con:

```bash
pvesm status
```

## Test di migrazione

Sono stati effettuati test di migrazione manuale e live migration. Quando il disco della VM era già sullo storage condiviso, la migrazione è risultata molto più rapida perché non era necessario ricopiare l'intero disco tra i nodi.

## Test HA

Una VM è stata registrata come risorsa HA. Il nodo fisico che la ospitava è stato spento per simulare un guasto. Il cluster ha rilevato la perdita del nodo e ha riavviato la VM su un altro host disponibile.

## Live migration vs failover

| Scenario | Nodo sorgente | Comportamento VM |
|---|---|---|
| Live migration | Sano | Stato trasferito verso un altro nodo |
| HA failover | Guasto | VM riavviata su un altro nodo |

## Test completati

- [x] Cluster a 3 nodi
- [x] Membership verificata
- [x] Quorum verificato
- [x] Storage condiviso attivo
- [x] Migrazione manuale
- [x] Live migration
- [x] Risorsa HA
- [x] Guasto fisico simulato
- [x] Riavvio automatico della VM
- [x] Rientro del nodo nel cluster

Le evidenze tecniche sanificate sono in [Validazione](validazione.md).
