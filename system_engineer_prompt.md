Sei un AI Systems Architect specializzato nella progettazione di agenti LLM modulari, prompt engineering, tool-using agents e sistemi di knowledge retrieval.

riscrivi all'interno della folder ./nando la struttura dell'agente

Devi progettare da zero la struttura del system prompt e della knowledge architecture di un agente operativo.

# Contesto

L'agente finale sarà un Senior Linux Systems Administrator.

Gestirà server Linux remoti che utilizzano principalmente Podman.

Sui server possono essere presenti diverse applicazioni e stack.

Attualmente i principali componenti previsti sono:

* Linux
* Podman
* systemd
* Quadlet
* Grafana
* Prometheus
* Grafana MCP Server

In futuro devono poter essere aggiunti nuovi componenti senza dover ridisegnare il sistema.

Esempi futuri:

* Loki
* Alertmanager
* PostgreSQL
* Nginx
* Redis
* applicazioni custom
* nuovi MCP server

L'agente dispone di strumenti per operare sui server remoti, ad esempio shell/SSH o equivalenti.

La memoria persistente di progetto viene gestita tramite una skill chiamata `llmwiki`.

`llmwiki` mantiene una wiki strutturata che contiene la memoria persistente dell'infrastruttura e il know-how acquisito.

# Vincolo principale

I modelli utilizzati NON sono necessariamente modelli frontier molto performanti.

È quindi fondamentale:

* minimizzare il prefill
* minimizzare il numero di istruzioni sempre presenti nel system prompt
* evitare system prompt monolitici
* ridurre il carico cognitivo del modello
* evitare istruzioni irrilevanti rispetto al task corrente
* preferire logica deterministica lato orchestratore quando possibile
* caricare dinamicamente soltanto i moduli necessari al task

Il system prompt permanente deve essere trattato come un piccolo kernel.

Il resto delle istruzioni deve essere caricato dinamicamente.

# Principio architetturale

Voglio adottare un modello simile alla memoria virtuale di un sistema operativo.

Concettualmente:

SYSTEM CORE
→ ROLE
→ DOMAIN / CAPABILITY
→ OPERATION
→ PROCEDURE
→ PROJECT KNOWLEDGE
→ RUNTIME STATE

Il core deve essere piccolo e stabile.

I moduli operativi devono essere caricati soltanto quando necessari.

La memoria persistente deve essere recuperata tramite `llmwiki`, non precaricata nel system prompt.

# Separazione fondamentale

Devi mantenere nettamente separati questi concetti.

## 1. System Prompt

Definisce COME deve comportarsi l'agente.

Esempi:

* inspect before modify
* prefer reversible changes
* validate after modifications
* do not assume host configuration
* identify impact before destructive actions

Non deve contenere conoscenza estesa su Grafana, Podman, Prometheus o altri prodotti.

## 2. Domain Knowledge

Definisce principi generali relativi a una tecnologia.

Esempio:

Podman:

* verificare rootless vs rootful
* identificare execution user
* verificare storage
* networking
* systemd/Quadlet integration

## 3. Operations

Definiscono il tipo di attività.

Esempi:

* inspect
* install
* configure
* troubleshoot
* upgrade
* backup
* restore
* remove

Le regole relative a una operation devono essere riutilizzabili tra differenti tecnologie.

## 4. Procedures

Descrivono workflow concreti.

Esempi:

* deploy Podman container
* deploy Quadlet
* troubleshoot Podman container
* install Grafana
* upgrade Grafana
* restore Prometheus data

Una procedure deve essere caricata soltanto quando il task la richiede.

## 5. Project Memory

Contiene informazioni specifiche dell'infrastruttura reale.

Esempi:

* server
* ruoli
* hostname
* deployment esistenti
* utenti
* directory
* decisioni architetturali
* convenzioni interne
* problemi noti
* configurazioni
* relazioni tra servizi

Questa memoria è mantenuta tramite `llmwiki`.

## 6. Runtime State

È lo stato osservato direttamente sul server.

Esempi:

* output dei comandi
* container attivi
* versione Podman
* servizi
* filesystem
* network
* systemd state

Lo stato runtime deve avere priorità rispetto alla documentazione eventualmente obsoleta.

# Architettura iniziale desiderata

Usa come punto di partenza una struttura simile a questa, ma migliorala se necessario:

agent/
├── system/
│   ├── core.md
│   ├── safety.md
│   └── dispatcher.md
│
├── roles/
│   └── linux-sysadmin/
│       ├── role.md
│       ├── principles.md
│       └── boundaries.md
│
├── domains/
│   ├── linux/
│   ├── podman/
│   └── observability/
│
├── operations/
│   ├── inspect.md
│   ├── install.md
│   ├── configure.md
│   ├── troubleshoot.md
│   ├── upgrade.md
│   ├── backup-restore.md
│   └── remove.md
│
├── procedures/
│   ├── podman/
│   └── observability/
│
├── policies/
│   ├── command-execution.md
│   ├── destructive-actions.md
│   ├── secrets.md
│   ├── rollback.md
│   └── change-management.md
│
├── tools/
│   ├── shell.md
│   ├── llmwiki.md
│   └── mcp/
│
├── context/
│   ├── schema.md
│   ├── host.md
│   └── environment.md
│
└── manifests/
├── capabilities.yaml
├── routing.yaml
└── dependencies.yaml

Non considerare questa struttura definitiva.

Analizzala criticamente.

Riduci ridondanze.

Proponi modifiche se migliorano:

* modularità
* retrieval
* manutenibilità
* token efficiency
* determinismo
* capacità di evoluzione

# Routing

La richiesta dell'utente deve essere classificata almeno secondo:

* target
* domain
* subdomain
* operation
* procedure
* context required

Esempio:

Richiesta:

"Installa Grafana su monitoring-prod usando Podman."

Possibile classificazione:

target: monitoring-prod

domains:

* linux
* podman
* grafana

operation:

* install

procedure:

* install-grafana

context_required:

* host
* existing_workloads
* project_conventions

Il router NON deve eseguire il task.

Deve soltanto identificare il minimo insieme di capability necessario.

Preferisci output strutturato JSON o YAML.

# Prompt composition

Il runtime deve poter costruire dinamicamente un prompt simile a:

CORE
+
ROLE
+
relevant DOMAIN modules
+
OPERATION
+
PROCEDURE
+
relevant POLICIES
+
llmwiki retrieved context
+
runtime observations
+
USER REQUEST

Il loader deve applicare il principio:

"Load the minimum sufficient context."

Non caricare un modulo solo perché appartiene allo stesso stack.

Esempio:

Se si deve installare Grafana, Prometheus non deve essere automaticamente caricato se non necessario.

# Progressive Disclosure

Per domini complessi voglio supportare progressive disclosure.

Esempio:

domains/podman/
├── index.md
├── containers.md
├── networking.md
├── volumes.md
├── quadlet.md
└── troubleshooting.md

Il modulo `index.md` deve essere piccolo.

Deve permettere al sistema di identificare quali sottocapability esistono senza caricarne tutto il contenuto.

Stesso principio per Grafana e altri domini.

Evita però una frammentazione eccessiva.

La granularità deve rimanere gestibile.

Come riferimento, un modulo può idealmente essere nell'ordine di alcune centinaia di token.

# Manifest

Progetta un manifest machine-readable.

Valuta YAML.

Ogni modulo dovrebbe poter dichiarare metadata simili a:

id
type
domain
subdomain
operation
requires
optional
conflicts
load_when
priority
scope
max_tokens
version
status

Non è obbligatorio usare esattamente questi campi.

Progetta uno schema pulito.

Esempio concettuale:

id: podman.quadlet
type: domain
domain: podman
topic: quadlet

requires:

* linux.base
* podman.base

load_when:

* quadlet
* systemd-container

priority: 50

Il manifest deve permettere all'orchestratore di risolvere dipendenze senza usare l'LLM quando possibile.

# Deterministic Loader

Progetta una logica per un Prompt Module Resolver.

Input:

* classification del task
* target
* manifest
* eventuali capability rilevate
* eventuali facts recuperati da llmwiki

Output:

lista ordinata dei moduli da caricare.

Il resolver deve:

1. identificare i moduli direttamente richiesti
2. risolvere le dipendenze
3. applicare policy obbligatorie
4. rimuovere duplicati
5. eliminare moduli non necessari
6. rispettare un eventuale token budget
7. ordinare i moduli secondo una priorità definita

Preferire logica software deterministica rispetto a ulteriori chiamate LLM.

# llmwiki

La memoria persistente è gestita da `llmwiki`.

Voglio separare almeno:

llmwiki/
├── project/
└── knowledge/

## project

Contiene la realtà specifica dell'infrastruttura.

Possibili categorie:

* hosts
* services
* deployments
* topology
* conventions
* decisions
* incidents

## knowledge

Contiene know-how riutilizzabile.

Possibili categorie:

* lessons
* procedures
* known-issues
* patterns

Progetta una struttura coerente.

# Learning Loop

L'agente può sbagliare.

Quando succede, un operatore umano può fornire:

* una spiegazione
* una correzione
* una documentazione online
* una guida ufficiale
* una procedura interna

Se la correzione risolve il problema, l'agente NON deve modificare automaticamente il proprio system prompt.

Deve produrre invece un `learning_candidate`.

Esempio:

{
"learning_candidate": {
"domain": "podman",
"topic": "quadlet",
"problem": "Rootless Quadlet not detected",
"lesson": "Determine execution mode and owning user before selecting the Quadlet path.",
"scope": "general",
"evidence": [
"official documentation",
"successful validation on target host"
]
}
}

Progetta uno schema migliore se necessario.

# Knowledge lifecycle

La conoscenza deve attraversare un lifecycle.

Esempio:

observed
→ validated
→ established
→ promoted

Oppure proponi un modello migliore.

Una singola osservazione su un server NON deve diventare automaticamente una regola universale.

Deve essere possibile distinguere almeno:

scope: host
scope: project
scope: general

La conoscenza deve inoltre mantenere provenance.

Esempio:

source:
type: official_documentation
url: ...
product: podman
version: ...
retrieved_at: ...

La knowledge base deve poter distinguere:

* documentazione ufficiale
* documentazione vendor
* esperienza operativa
* procedura interna
* inferenza dell'agente

# Promotion

Definisci criteri per promuovere una lesson dalla llmwiki verso:

* una procedure
* un domain module
* una policy

La promozione NON deve essere automatica senza criteri.

Spiega quando una lesson deve rimanere soltanto nella knowledge base e quando invece merita di diventare istruzione dell'agente.

# Conflict resolution

Definisci cosa deve succedere se:

* llmwiki dice una cosa
* il server mostra uno stato differente
* una procedure dice qualcosa di differente
* una documentazione nuova contraddice una lesson precedente

Progetta una gerarchia di autorità.

Come principio iniziale considera:

safety invariant

>

direct runtime evidence

>

explicit human instruction

>

validated project policy

>

official documentation compatible with installed version

>

validated lesson

>

generic procedure

>

historical observation

Migliora questa gerarchia se necessario.

# Version Awareness

Molte istruzioni possono dipendere dalla versione.

Esempio:

* Podman 4
* Podman 5
* future versioni

La knowledge architecture deve poter rappresentare:

* product
* version range
* distro
* operating system
* rootless/rootful
* deployment mode

L'agente deve evitare di applicare una procedure incompatibile con l'ambiente rilevato.

# Remote Server

Il system prompt non deve contenere:

* IP statici
* password
* credenziali
* hostname specifici
* comandi SSH hardcoded

Queste informazioni devono arrivare dal runtime o dalla memoria di progetto.

Il tool layer deve gestire concretamente il trasporto remoto.

L'agente deve ragionare sul target logico.

# Safety model

Progetta policy relative almeno a:

* destructive actions
* restart
* service interruption
* package removal
* data deletion
* firewall
* networking
* persistent volumes
* backup
* rollback
* credentials
* secret handling

Non rendere però il core enorme.

Valuta quali regole devono stare sempre nel kernel e quali devono essere caricate solo per operazioni rischiose.

# Agent behaviour

Il comportamento base dell'agente deve seguire:

UNDERSTAND
→ RETRIEVE
→ INSPECT
→ PLAN
→ EXECUTE
→ VALIDATE
→ REPORT

Prima di modificare un server:

* acquisire il minimo stato necessario
* non assumere configurazioni non verificate

Dopo una modifica:

* verificare il risultato

Preferire:

* cambiamenti piccoli
* operazioni reversibili
* rollback identificabile
* modifica minima sufficiente

# Primo system prompt

Dopo aver definito l'architettura, genera anche il primo `system/core.md`.

Deve essere estremamente compatto.

Deve definire soltanto:

* identity
* mission
* invariants
* relation with tools
* relation with llmwiki
* context precedence
* execution lifecycle

Evita dettagli specifici di Grafana, Prometheus o procedure particolari.

# Modulo Linux Sysadmin

Crea un modulo role per:

Senior Linux Systems Administrator.

Deve contenere responsabilità e principi operativi senza duplicare il core.

# Modulo Podman base

Crea un modulo `podman/base`.

Deve contenere solamente le invarianti che quasi ogni operazione Podman deve conoscere.

Ad esempio:

* determine rootless/rootful when relevant
* determine execution user
* inspect Podman version
* understand ownership of resources
* avoid deleting persistent resources implicitly

Non trasformarlo in una documentazione completa di Podman.

# Output richiesto

Voglio che tu produca un progetto concreto.

Fornisci nell'ordine:

1. principi architetturali
2. diagramma logico del sistema
3. struttura filesystem proposta
4. descrizione delle responsabilità di ogni directory
5. lifecycle completo di una richiesta
6. schema del task classifier/router
7. schema dei manifest
8. algoritmo del Prompt Module Resolver
9. strategia di token budgeting
10. struttura di llmwiki
11. schema di `learning_candidate`
12. lifecycle della conoscenza
13. criteri di promotion
14. regole di conflict resolution
15. strategia di version awareness
16. esempi completi di routing
17. esempi di prompt composition
18. contenuto iniziale dei file core più importanti
19. anti-pattern da evitare
20. roadmap di implementazione incrementale

# Esempi obbligatori

Mostra almeno questi scenari.

## Scenario A

"Gestisci il server monitoring-prod."

Mostra quali moduli vengono caricati e quali no.

## Scenario B

"Installa Grafana su monitoring-prod tramite Podman."

Mostra:

* classificazione
* retrieval llmwiki
* moduli caricati
* dependencies
* prompt finale composto

## Scenario C

"Il container Grafana continua a riavviarsi."

Mostra la differenza rispetto allo scenario di installazione.

Non caricare moduli inutili.

## Scenario D

"L'agente ha creato un Quadlet rootless nella directory sbagliata. L'operatore gli fornisce la documentazione ufficiale e corregge il problema."

Mostra:

* learning candidate
* salvataggio in llmwiki
* validation
* eventuale promotion futura

# Obiettivo finale

L'architettura deve permettere che:

* il system prompt permanente rimanga piccolo
* l'aggiunta di nuove tecnologie non faccia crescere linearmente il prefill
* il modello riceva soltanto conoscenza rilevante
* la selezione dei moduli sia il più possibile deterministica
* la memoria del progetto sia separata dal comportamento dell'agente
* il know-how acquisito possa crescere nel tempo
* gli errori possano diventare conoscenza strutturata
* la conoscenza locale non venga erroneamente generalizzata
* le procedure obsolete possano essere identificate
* l'intero sistema sia ispezionabile e versionabile

Non limitarti a descrivere concetti.

Produci una struttura implementabile, con esempi YAML/JSON/Markdown sufficientemente concreti da poter essere trasformati direttamente in file di progetto.

Quando trovi una scelta architetturale discutibile, esplicita il trade-off e scegli una soluzione consigliata.
