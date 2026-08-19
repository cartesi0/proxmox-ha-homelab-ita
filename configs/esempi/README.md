# Esempi di configurazione

Questa directory è destinata esclusivamente a **esempi sanificati** utili alla documentazione.

## È possibile pubblicare

- snippet Corosync con nomi generici;
- esempi di storage senza credenziali;
- esempi firewall senza endpoint reali;
- definizioni HA con identificativi fittizi;
- configurazioni didattiche con placeholder.

## Non pubblicare mai

- password;
- username operativi;
- chiavi SSH private o pubbliche;
- chiavi WireGuard private o pubbliche;
- API key;
- token;
- cookie e session ID;
- file `.env` reali;
- IP pubblici;
- endpoint pubblici reali;
- account ID cloud;
- backup contenenti credenziali.

## Placeholder consigliati

```text
<IP_ESEMPIO>
<NODO_A>
<USERNAME_RIMOSSO>
<CHIAVE_RIMOSSA>
<TOKEN_RIMOSSO>
<ENDPOINT_RIMOSSO>
```

L'obiettivo del repository è mostrare la comprensione della struttura e del funzionamento, non pubblicare dati operativi dell'ambiente.