# Note di troubleshooting

L'obiettivo di questa sezione è documentare non solo ciò che funziona, ma anche i problemi osservati e il ragionamento usato per capirli.

## Migrazione VM molto lenta

### Sintomo

Una migrazione ha iniziato a trasferire dati molto più a lungo del previsto.

### Interpretazione

La VM utilizzava storage locale, quindi Proxmox doveva copiare anche il disco virtuale verso il nodo di destinazione.

### Lezione

Con storage condiviso, il disco è già accessibile dai nodi e la migrazione può essere molto più rapida.

---

## La VM HA si è riavviata

### Sintomo

Dopo lo spegnimento del nodo che ospitava la VM HA, la VM è comparsa su un altro nodo ma si è riavviata.

### Interpretazione

È il comportamento atteso dopo un guasto fisico improvviso: lo stato RAM del nodo guasto è perso. L'HA può riavviare la VM dal disco, ma non continuare lo stato volatile perso.

### Lezione

**HA failover e live migration sono due meccanismi diversi.**

---

## La VM non è tornata automaticamente sul nodo originale

### Sintomo

Dopo il rientro del nodo guasto, la VM è rimasta sul nodo dove era stata recuperata.

### Interpretazione

Il rientro di un host nel cluster non implica automaticamente il ritorno del workload sul nodo precedente. Il posizionamento può essere modificato successivamente.

## Comandi utili

```bash
pvecm status
pvecm nodes
pvesm status
ha-manager status
systemctl status corosync
systemctl status pve-cluster
systemctl status pve-ha-lrm
systemctl status pve-ha-crm
```

## Metodo di documentazione

Per ogni problema cerco di registrare:

1. sintomo;
2. comportamento atteso;
3. evidenze raccolte;
4. causa o spiegazione più plausibile;
5. correzione o test successivo;
6. lezione appresa.

## Sicurezza

Prima di pubblicare output o configurazioni rimuovo IP, hostname, username, password, chiavi, token, cookie, endpoint pubblici e altri identificativi operativi.