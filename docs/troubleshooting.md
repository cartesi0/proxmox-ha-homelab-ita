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

---

## Risorse HA bloccate in `error` dopo un'interruzione dello storage condiviso

### Scenario osservato

Lo storage condiviso TrueNAS è diventato temporaneamente non raggiungibile mentre alcuni container LXC erano gestiti dal cluster HA.

Nei log HA è comparso un errore equivalente a:

```text
storage 'truenas' is not online
unable to start service ct:<ID>
service ct:<ID> is in an error state and needs manual intervention
```

### Sintomo

1. TrueNAS non era disponibile.
2. Proxmox HA ha tentato di avviare o recuperare i container.
3. I tentativi sono falliti perché i dischi erano sullo storage condiviso non disponibile.
4. Le risorse sono entrate nello stato HA `error`.
5. TrueNAS è tornato online e `pvesm status` mostrava nuovamente lo storage come disponibile.
6. Nonostante ciò, i container continuavano a non avviarsi finché lo stato HA non veniva resettato/rimosso.

### Causa

Il ritorno online dello storage risolve la causa infrastrutturale originale, ma **non cancella automaticamente lo stato `error` di una risorsa HA**.

Dopo ripetuti tentativi falliti, Proxmox HA smette intenzionalmente di tentare il recupero della risorsa e richiede un intervento amministrativo. Questo evita cicli continui di start/fail/relocate quando una dipendenza fondamentale, come lo storage condiviso, non è disponibile.

### Recovery

Prima di intervenire sulla risorsa, verificare che lo storage sia realmente tornato disponibile sui nodi interessati:

```bash
pvesm status
ha-manager status
```

Una modalità di recovery consiste nel portare temporaneamente la risorsa HA nello stato `disabled` e poi richiederne nuovamente l'avvio:

```bash
ha-manager set ct:<ID> --state disabled
ha-manager set ct:<ID> --state started
```

Nel test di laboratorio è stato inoltre verificato che rimuovere temporaneamente le risorse dalla configurazione HA eliminava il blocco e permetteva di avviarle nuovamente; successivamente potevano essere reinserite in HA dopo aver verificato la disponibilità dello storage.

### Verifiche utili

```bash
pvesm status
ha-manager status
pct status <ID>
pct config <ID>
journalctl -u pve-ha-lrm -n 150 --no-pager
```

### Lezione appresa

**Il ripristino di una dipendenza non implica necessariamente il ripristino automatico dello stato HA del workload.**

Inoltre, un cluster Proxmox con più nodi può fornire ridondanza del compute ma dipendere ancora da un singolo storage condiviso. Se quello storage non è ridondato e diventa indisponibile, nessun nodo può avviare i workload che dipendono da esso.

In altre parole:

```text
Nodo Proxmox guasto + storage disponibile  -> HA può recuperare il workload
Storage condiviso guasto                  -> tutti i nodi perdono l'accesso al disco
```

Questo test ha quindi evidenziato sia il comportamento dello stato `error` di Proxmox HA sia il ruolo dello storage condiviso come possibile single point of failure.

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