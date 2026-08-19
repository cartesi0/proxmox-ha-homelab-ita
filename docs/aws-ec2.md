# AWS EC2 e Security Groups

## Contesto

Oltre all'homelab Proxmox, ho esperienza pratica con **Amazon EC2** per l'esecuzione e la gestione di workload Linux nel cloud.

Non presento questa esperienza come competenza senior: la considero parte del mio percorso pratico verso ruoli Infrastructure e Cloud.

## Attività praticate

- creazione e utilizzo di istanze EC2;
- accesso e amministrazione di sistemi Linux;
- gestione di servizi applicativi su istanze cloud;
- utilizzo dei Security Groups;
- controllo del traffico inbound e outbound;
- troubleshooting di connettività e servizi;
- gestione operativa di workload Linux ospitati su EC2.

## Security Groups

I Security Groups funzionano come un firewall virtuale associato alle risorse AWS e permettono di definire il traffico consentito in ingresso e in uscita. Sono **stateful**: il traffico di risposta relativo a una connessione consentita viene gestito automaticamente, senza richiedere una regola separata per il traffico di ritorno.

Nel repository pubblico non vengono riportati IP reali, regole associate a endpoint reali o altri dati dell'account AWS.

Esempio concettuale:

```text
Internet / rete autorizzata
          |
   Security Group
          |
       EC2 Linux
```

## Collegamento con l'homelab

L'esperienza EC2 completa ciò che pratico localmente con:

- Linux;
- networking;
- firewall;
- accesso remoto;
- monitoraggio;
- automazione;
- troubleshooting.

## Prossimi argomenti AWS

- networking VPC;
- subnet e route table;
- IAM di base;
- storage EBS;
- monitoraggio CloudWatch;
- backup e snapshot;
- automazione infrastrutturale.

## Sicurezza

Non vengono pubblicati:

- Account ID AWS;
- Access Key ID;
- Secret Access Key;
- token temporanei;
- IP pubblici delle istanze;
- endpoint reali;
- chiavi SSH private o pubbliche;
- credenziali o file di configurazione dell'account.
