# 1 - Introduzione

"_Main-Proc_" nasce per avere dei promemoria delle attività di manutenzione.

Utilizza il progetto open-source "_Noco DB_" per avere un database semplice da usare e integrare con servizi di terze parti come Microsoft Teams. Il progetto è gratis e può essere self-hosted.

Si è immaginato questo workflow:
1. L'utente esegue un import manuale una tantum delle attività di manutenzione, tramite file CSV
2. Un'automazione interagisce con Noco DB, per recuperare le attività di manutenzione e generare i promemoria. Presumibilmente, l'automazione è uno script schedulato (i. crontab, bash, Java, ...)
3. Noco DB notifica gli utenti su Microsoft Teams con un meccanismo di webhook

In aggiunta, gli utenti hanno a disposizione una lavagna Kanban tramite l'interfaccia web di Noco DB.

Si suggerisce di approfondire la [documentazione](https://docs.nocodb.com/) ufficiale.

Per realizzare il workflow descritto sopra, è necessario completare alcuni punti aperti:
- Automatizzare la creazione dei promemoria mensili, con un algoritmo tale da distribuirli nel corso del tempo
- Configurare gli eventi e i messaggi da inviare agli utenti di Microsoft Teams

# 2 - Procedure

### 2.1 - Installazione Noco DB

Requisiti:
- Linux
- Docker
- Git

Procedura:
1. Accedere alla macchina Linux
2. Creare la directory "nocodb"
3. Posizionarsi nella directory "nocodb"
4. Eseguire il comando:
```
git pull https://github.com/JustNello/timeless-nocodb.git
```
5. Eseguire il comando:
```
docker compose up --detach
```

Risorse:
- [Guida ufficiale](https://docs.nocodb.com/0.109.7/getting-started/installation)

### 2.2 - Avvio Noco DB

Procedura:
1. Posizionarsi nella directory di installazione
2. Eseguire il comando:
```
docker compose up --detach
```

### 2.3 - Stop Noco DB

Procedura:
1. Posizionarsi nella directory di installazione
2. Eseguire il comando:
```
docker stop nocodb
```

# 3 - Configurazione

Dopo aver installato Noco DB, la directory di installazione contiene diversi file utili:
- /nocodb/noco.db
    - Questo file è usato da Noco DB è NON va mai modificato a mano. Si suggerisce di farne un backup regolarmente
- compose.yaml
    - Questo file descrive come Noco DB è installato tramite Docker. Qui si può cambiare la porta da cui accedere all'interfaccia web (il valore di default è 8080). Inoltre, è possibile configurare il nome utente e la password dell'account amministratore, tramite le variabili di ambiente NC_ADMIN_EMAIL e NC_ADMIN_PASSWORD, da definire all'interno del file compose.yaml. Maggiori dettagli nella [documentazione](https://github.com/nocodb/nocodb/issues/4532)

# 4 - Funzionalità Noco DB

Segue una descrizione delle funzionalità di Noco DB usate per le finalità del progetto.

### 4.1 - Notifica Microsoft Teams

I promemoria sono notificati con l'integrazione con Microsoft Teams, che può essere configurata come descritto [qui](https://docs.nocodb.com/0.109.7/developer-resources/webhooks/#microsoft-teams); è necessario creare un team su Teams e configurare il plugin nativo per la ricezione di webhooks.

Il messaggio inviato su Microsoft Teams può essere modificato come descritto [qui](https://docs.nocodb.com/automation/webhook/create-webhook#webhook-with-custom-payload-).

Esistono due eventi per cui viene inviato un messaggio:
1. Creazione nuovo promemoria
2. Modifica stato promemoria esistente

### 4.2 - Import manuale

Le attività di manutenzione si possono importare tramite file CSV; trovi un esempio di file CSV da importare nella cartella "_/examples_".

Il file CSV deve rispettare alcune specifiche. In particolare, alcuni colonne sono enumerate ed accetano dei valori predefiniti.

La colonna "_Frequence_" accetta questi valori:
- DAILY
- WEEKLY
- MONTHLY
- BIMONTHLY
- QUARTERLY
- SEMESTER
- YEARLY

La colonna "_Priority_" accetta questi valori:
- MEDIUM
- HIGH

La colonna "_ExpectedDataExecution_" accetta questi valori:
- JANUARY
- FEBRUARY
- MARCH
- APRIL
- MAY
- JUNE
- AUGUST
- SEPTEMBER
- OCTOBER
- NOVEMBER
- DECEMBER

### 4.3 - Lavagna Kanban

La lavagna Kanban mostra i promemoria e sintettiza alcune informazioni, come chi si sta occupando di cosa e lo stato di un'attività (i. TODO, DONE, ONGOING).

Per accedere alla lavagna:
1. Accedere all'interfaccia web di Noco DB
2. Aprire il menu laterale
3. Espandere la voce "_Main-Proc_"
4. Espandere la voce "_Reminders_"
5. Cliccare sulla voce "_Board_"