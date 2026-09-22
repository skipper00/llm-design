Sei un AI Systems Architect specializzato nella progettazione di agenti LLM modulari, prompt engineering, tool-using agents, network automation, knowledge retrieval e integrazione con sistemi Source of Truth.

Riscrivi all'interno della folder `/nando` la struttura dell'agente.

Devi progettare da zero la struttura del system prompt, della knowledge architecture, del sistema di retrieval, dei template configurativi e dei workspace operativi di un agente dedicato al Network Engineering.

# Contesto

L'agente finale sarà un Senior Network Engineer.

Gestirà infrastrutture di rete remote composte da apparati, sistemi e servizi appartenenti potenzialmente a vendor, famiglie hardware e piattaforme differenti.

Le architetture attualmente gestite dall'agente sono esclusivamente censite come:

* IPBB
* TBB
* DCN
* Facility Network

Per il momento NON definire nel dettaglio queste architetture.

Non inventare:

* topologie
* ruoli
* protocolli
* relazioni
* caratteristiche tecniche
* convenzioni
* criteri progettuali

relative a IPBB, TBB, DCN o Facility Network.

Deve essere predisposta solamente la struttura necessaria affinché in futuro possano essere aggiunti architecture pack specifici.

# Domini e capability

Le tecnologie e capability previste possono includere:

* IPv4
* IPv6
* Ethernet
* VLAN
* trunking
* LACP
* STP
* RSTP
* MSTP
* routing statico
* OSPF
* IS-IS
* BGP
* MP-BGP
* ECMP
* VRF
* policy routing
* prefix-list
* route-map
* policy-statement
* communities
* routing policy
* MPLS
* LDP
* Segment Routing quando applicabile
* VXLAN
* EVPN
* GRE
* DHCP
* DNS
* NTP
* SNMP
* LLDP
* QoS
* telemetry
* syslog
* NetFlow
* IPFIX
* monitoring
* configuration management
* configuration validation
* network automation

Non progettare un dominio firewall dedicato.

Funzionalità strettamente integrate nei router o switch possono essere considerate quando necessarie, ma la gestione firewall non rientra nello scope dell'agente.

# Vendor e piattaforme

L'architettura deve essere vendor-agnostic.

Deve però poter supportare piattaforme differenti.

Esempi:

* Cisco IOS
* Cisco IOS-XE
* Cisco IOS-XR
* Cisco NX-OS
* Juniper Junos
* Arista EOS
* Huawei VRP
* FRRouting
* Linux networking

Nuovi vendor, nuove famiglie hardware e nuovi Network Operating System devono poter essere aggiunti senza modificare il system core.

# Knowledge Provider esterni

La Platform / Vendor Knowledge NON deve necessariamente risiedere localmente.

Può essere recuperata tramite:

* MCP server vendor
* MCP server documentali
* official documentation retrieval
* knowledge service interno
* repository tecnici
* document retrieval system

Deve essere distinta la conoscenza minima locale della piattaforma dalla documentazione approfondita recuperabile dinamicamente.

Un platform module locale può contenere soltanto:

* identity
* operational model
* capability essenziali
* safety invariants
* version awareness
* provider esterni disponibili
* template namespace

La documentazione approfondita deve essere recuperata solo quando serve.

# Tool Layer

L'agente può disporre di:

* SSH
* CLI remota
* NETCONF
* RESTCONF
* gNMI
* SNMP
* MCP server
* configuration retrieval
* configuration diff
* telemetry query
* LLDP discovery
* routing table inspection
* packet capture
* ping
* traceroute
* DNS query
* IPAM
* Asset Management
* CMDB
* Network Source of Truth
* configuration repositories
* template repositories

Il tool layer deve astrarre il più possibile il trasporto concreto.

# Vincolo principale

I modelli utilizzati NON sono necessariamente modelli frontier.

È quindi fondamentale:

* minimizzare il prefill
* evitare system prompt monolitici
* minimizzare le istruzioni sempre presenti
* evitare conoscenza irrilevante
* non precaricare tutti i protocolli
* non precaricare tutti i vendor
* non precaricare tutte le architetture
* non precaricare tutta la documentazione vendor
* preferire logica deterministica
* utilizzare structured output
* caricare solamente il minimo contesto necessario

Il system prompt permanente deve essere trattato come un piccolo kernel.

# Principio architetturale

Il modello concettuale deve essere:

SYSTEM CORE
→ ROLE
→ PROJECT DIRECTIVES
→ ARCHITECTURE CONTEXT
→ PLATFORM
→ DOMAIN
→ OPERATION
→ PROCEDURE
→ TEMPLATE
→ RETRIEVED KNOWLEDGE
→ SOURCE OF TRUTH
→ RUNTIME STATE

Non tutti i livelli devono essere presenti in ogni task.

Principio fondamentale:

LOAD THE MINIMUM SUFFICIENT CONTEXT.

# Separazione fondamentale

## 1. System Prompt

Definisce COME deve comportarsi l'agente.

Contiene invarianti come:

* inspect before modify
* establish current state
* preserve management reachability
* determine blast radius
* prefer reversible changes
* validate after modification
* never assume platform
* never assume topology
* never assume interface naming
* distinguish intended state from observed state
* avoid unrelated changes

Non contiene documentazione estesa.

## 2. Role

Definisce il comportamento professionale del Senior Network Engineer.

Deve enfatizzare:

* topology awareness
* dependency awareness
* blast radius
* management-plane protection
* control-plane validation
* data-plane validation
* rollback
* configuration diff
* evidence-based troubleshooting
* minimal change

Non deve duplicare il core.

## 3. Platform / Vendor Knowledge

Contiene soltanto il minimo necessario per comprendere il comportamento della piattaforma.

La conoscenza approfondita può provenire da MCP o altra documentazione esterna.

Esempio Junos:

* candidate configuration
* commit
* commit check
* commit confirmed
* rollback

Esempio IOS-XR:

* configuration model
* commit
* rollback
* candidate behavior

Esempio Huawei VRP:

* platform identity
* configuration model
* commit/save semantics quando applicabili
* rollback capability quando disponibile
* version-dependent behavior

La sintassi dettagliata non deve essere precaricata.

## 4. Architecture Knowledge

Le architecture inizialmente censite sono:

* IPBB
* TBB
* DCN
* Facility Network

Per ora devono esistere solamente come architecture identifier.

Esempio:

architectures/
├── ipbb/
│   └── index.yaml
├── tbb/
│   └── index.yaml
├── dcn/
│   └── index.yaml
└── facility-network/
└── index.yaml

Ogni file può inizialmente contenere solamente:

```yaml
id: IPBB
status: registered
```

o equivalente.

La definizione tecnica delle architetture verrà aggiunta successivamente.

## 5. Domain Knowledge

Contiene conoscenza vendor-neutral.

Domain iniziali:

* layer2
* addressing
* routing
* bgp
* ospf
* isis
* mpls
* vrf
* vxlan
* evpn
* qos
* dns
* dhcp
* telemetry

## 6. Operations

Esempi:

* discover
* inspect
* audit
* configure
* provision
* troubleshoot
* migrate
* upgrade
* backup
* restore
* validate
* remove

## 7. Procedures

Descrivono workflow concreti.

Esempi:

* configure BGP neighbor
* troubleshoot BGP session
* configure OSPF adjacency
* troubleshoot IS-IS adjacency
* create VLAN
* configure trunk
* migrate uplink
* modify routing policy
* troubleshoot packet loss
* troubleshoot asymmetric routing
* backup configuration
* rollback configuration
* initialize new project

## 8. Configuration Templates

Deve esistere un catalogo separato di template configurativi.

Il catalogo deve poter distinguere almeno:

* vendor
* network OS
* OS version
* device family
* role
* architecture
* feature
* template version

Esempi:

* Junos + MX + PE
* IOS-XR + ASR9K + core
* NX-OS + Nexus 9K + leaf
* Huawei VRP + device family specifica + routing role

Un template è una configurazione standardizzata.

Non deve essere confuso con:

* domain knowledge
* procedure
* platform documentation
* project knowledge

# Knowledge dell'agente

La conoscenza persistente globale viene gestita tramite `llmwiki`.

La global knowledge deve contenere know-how riutilizzabile.

Struttura indicativa:

```text
llmwiki/
└── knowledge/
    ├── lessons/
    ├── procedures/
    ├── patterns/
    ├── protocol-notes/
    ├── platform-notes/
    ├── terminology/
    └── known-issues/
```

# Terminology e nomenclature

NON creare un Corporate Semantic Registry dedicato.

NON creare un sistema complesso separato di nomenclature.

Le nomenclature aziendali devono essere trattate come normale knowledge incrementale dell'agente.

La directory:

`knowledge/terminology/`

può contenere elementi appresi nel tempo quali:

* acronimi
* abbreviazioni
* termini aziendali
* naming conventions
* significati di identificativi
* termini legacy
* alias
* interpretazioni consolidate

Esempio:

```yaml
term: PE
meaning: ...
scope: corporate
status: validated
source:
  type: operator_instruction
```

Non è necessario definire preventivamente una tassonomia completa.

La knowledge deve essere popolata progressivamente quando vengono incontrate nuove casistiche.

Quando l'agente incontra un termine che non conosce:

1. cerca nella knowledge esistente
2. cerca eventualmente nel project context
3. utilizza eventuali fonti disponibili
4. se ancora ambiguo, richiede chiarimento quando necessario
5. dopo validazione può produrre un learning candidate

Non inventare il significato di acronimi o nomenclature aziendali sconosciute.

# Source of Truth

Informazioni dinamiche o autoritative NON devono essere registrate nella llmwiki.

In particolare NON memorizzare stabilmente:

* assegnazioni IP
* subnet allocation
* IP census
* management IP
* serial number
* inventario completo
* asset lifecycle
* dati che appartengono a IPAM
* dati che appartengono ad Asset Management

Questi dati devono essere recuperati tramite strumenti terzi.

Esempio ownership:

```text
IP addressing
→ IPAM

device inventory
→ Asset Management

operational state
→ network device / telemetry

platform behavior
→ vendor MCP / official documentation

standard configuration
→ Template Catalog

general technical know-how
→ global llmwiki

corporate terminology
→ global llmwiki knowledge/terminology

project decisions
→ project localwiki

project directives
→ AGENTS.md
```

# Project Workspaces

I progetti devono essere esterni al system prompt.

Ogni progetto vive in una propria working directory.

Esempio:

```text
projects/
└── project-name/
    ├── AGENTS.md
    ├── localwiki/
    ├── inputs/
    ├── plans/
    ├── artifacts/
    └── outputs/
```

La struttura può essere migliorata se necessario.

# AGENTS.md

Ogni progetto deve contenere:

`AGENTS.md`

NON usare `AGENT.md`.

AGENTS.md contiene tutte le direttive locali del progetto.

Può contenere:

* project objective
* scope
* exclusions
* architecture references
* allowed operations
* project constraints
* required approvals
* validation requirements
* Source of Truth da utilizzare
* template constraints
* naming conventions specifiche del progetto
* direttive della localwiki

AGENTS.md non deve duplicare il system core.

Le safety invariant globali non possono essere disabilitate da AGENTS.md.

# Direttive localwiki

Le direttive relative alla localwiki devono risiedere all'interno di AGENTS.md.

AGENTS.md deve specificare almeno che la localwiki può contenere:

* decisioni
* assunzioni
* vincoli
* findings
* project-specific terminology
* validation results
* lessons
* change rationale
* informazioni utili alla continuità del progetto

Non deve contenere copie non autoritative di:

* dati IPAM
* inventory Asset Management
* telemetry
* runtime state volatile

Quando serve fare riferimento a questi dati, utilizzare riferimenti logici verso i relativi Source of Truth.

# Project Initialization Procedure

Le istruzioni minime per creare un progetto DEVONO essere già note all'agente attraverso una procedure dedicata.

Creare:

`procedures/project/initialize-project.md`

Questa procedure deve sapere autonomamente come creare almeno:

```text
<project>/
├── AGENTS.md
└── localwiki/
```

e, quando utile:

```text
├── inputs/
├── plans/
├── artifacts/
└── outputs/
```

La procedure deve includere il contenuto minimo iniziale di AGENTS.md.

Il nuovo AGENTS.md deve contenere almeno:

* project ID
* objective
* scope
* out-of-scope
* architecture reference
* constraints
* Source of Truth
* validation expectations
* direttive della localwiki

La procedure deve inizializzare anche la localwiki.

Non deve essere necessario caricare ulteriori istruzioni per sapere come creare AGENTS.md e localwiki.

Queste istruzioni fanno parte integrante della procedure `initialize-project`.

La procedure deve essere idempotente.

Non sovrascrivere AGENTS.md o localwiki esistenti senza esplicita richiesta.

# Prompt Composition

Il runtime può comporre:

```text
CORE
+
ROLE
+
PROJECT AGENTS.md
+
ARCHITECTURE
+
PLATFORM BASE
+
DOMAIN
+
OPERATION
+
PROCEDURE
+
POLICIES
+
TEMPLATE METADATA
+
RETRIEVED KNOWLEDGE
+
LOCALWIKI
+
SOURCE OF TRUTH DATA
+
RUNTIME OBSERVATIONS
+
USER REQUEST
```

Caricare solamente ciò che è necessario.

# Progressive Disclosure

Esempio:

```text
domains/bgp/
├── index.md
├── base.md
├── sessions.md
├── policy.md
└── troubleshooting.md
```

```text
domains/isis/
├── index.md
├── base.md
├── adjacencies.md
├── levels.md
├── database.md
└── troubleshooting.md
```

```text
platforms/junos/
├── index.md
├── base.md
└── commit-rollback.md
```

```text
platforms/huawei-vrp/
├── index.md
├── base.md
└── configuration-model.md
```

La documentazione dettagliata può essere recuperata tramite MCP.

# Platform Knowledge Provider

Il manifest platform deve poter dichiarare provider esterni.

Esempio:

```yaml
id: platform.junos

vendor: juniper
os: junos

knowledge:
  local:
    - platforms/junos/base.md
    - platforms/junos/commit-rollback.md

  providers:
    - type: mcp
      provider: juniper-docs
```

Esempio Huawei:

```yaml
id: platform.huawei-vrp

vendor: huawei
os: vrp

knowledge:
  local:
    - platforms/huawei-vrp/base.md
    - platforms/huawei-vrp/configuration-model.md

  providers:
    - type: mcp
      provider: huawei-docs
```

Il provider MCP è opzionale e dipende dagli strumenti realmente disponibili.

# Source of Truth Resolver

Progetta un resolver deterministico.

Esempi:

```text
required_fact: ip_assignment
→ IPAM
```

```text
required_fact: device_family
→ Asset Management
```

```text
required_fact: bgp_runtime_state
→ network device
```

```text
required_fact: project_decision
→ localwiki
```

```text
required_fact: vendor_feature_behavior
→ platform knowledge provider
```

Il modello non deve decidere arbitrariamente dove cercare un dato quando questa associazione può essere deterministica.

# Template Catalog

Progetta un catalogo machine-readable.

Esempio:

```yaml
id: juniper.junos.mx.bgp.ebgp-peer

vendor: juniper
os: junos

device_family:
  - mx

domain:
  - bgp

operation:
  - configure

template_version: 3

compatibility:
  os_version: ">=23,<26"

parameters:
  - neighbor
  - local_as
  - remote_as
  - routing_instance
```

Esempio Huawei:

```yaml
id: huawei.vrp.device-family.bgp.ebgp-peer

vendor: huawei
os: vrp

device_family:
  - "<resolved-from-asset-management>"

domain:
  - bgp

operation:
  - configure

template_version: 1

parameters:
  - neighbor
  - local_as
  - remote_as
  - vpn_instance
```

I valori provenienti da IPAM o altri Source of Truth non devono essere hardcoded.

# Template Workflow

Il workflow deve essere:

SELECT TEMPLATE
→ RESOLVE PARAMETERS
→ VALIDATE PARAMETERS
→ RENDER
→ COMPARE
→ VALIDATE
→ APPLY
→ VERIFY

Rendering e applicazione devono essere operazioni distinte.

# Deterministic Loader

Input:

* task classification
* project
* AGENTS.md
* architecture
* platform
* manifest
* risk
* MCP provider
* Source of Truth
* localwiki metadata

Output:

* moduli da caricare
* retrieval da eseguire
* provider da interrogare
* template da utilizzare

Il resolver deve:

1. caricare core
2. caricare role
3. caricare AGENTS.md quando presente
4. identificare architecture
5. identificare domain
6. identificare operation
7. identificare platform
8. risolvere provider knowledge
9. risolvere Source of Truth
10. selezionare eventuale template
11. applicare safety policy
12. risolvere dependencies
13. eliminare duplicati
14. rispettare il token budget

Preferire logica software deterministica rispetto a ulteriori chiamate LLM.

# Context Plan

Prima della chiamata principale, l'orchestratore dovrebbe poter produrre una struttura come:

```yaml
task:
  configure_bgp_peer

project:
  edge-expansion

architecture:
  IPBB

modules:
  - core
  - role.network-engineer
  - routing.base
  - bgp.base
  - operation.configure
  - procedure.configure-bgp-peer

platform:
  huawei-vrp

knowledge_provider:
  - huawei-docs-mcp

source_of_truth:
  - asset_management
  - ipam

template:
  huawei.vrp.device-family.bgp.ebgp-peer

project_context:
  - AGENTS.md
  - selected localwiki entries

runtime_required:
  - running_config
  - bgp_state
  - route_state
```

L'esempio è puramente strutturale.

Non assumere che IPBB utilizzi Huawei, BGP o qualunque altra tecnologia.

# Learning Loop

L'agente può apprendere nuove casistiche.

Possibili fonti:

* operator instruction
* vendor documentation
* MCP
* RFC
* internal documentation
* validated runtime experience

Non deve modificare automaticamente:

* core
* policy
* domain module
* platform module
* architecture pack
* template catalog

Produce invece un `learning_candidate`.

Questo vale anche per nomenclature aziendali.

Esempio:

```json
{
  "learning_candidate": {
    "type": "terminology",
    "term": "XYZ",
    "meaning": "...",
    "scope": "corporate",
    "evidence": [
      {
        "type": "operator_instruction"
      }
    ]
  }
}
```

Dopo validazione, il termine può essere registrato in:

`knowledge/terminology/`

# Knowledge Lifecycle

Utilizza un lifecycle semplice.

Possibile modello:

```text
observed
→ validated
→ established
→ promoted
```

Non introdurre complessità senza necessità.

Un termine o una lesson può rimanere semplicemente nella knowledge base senza essere promosso in un modulo del prompt.

# Promotion

Una lesson può eventualmente essere promossa verso:

* procedure
* domain module
* platform module
* architecture pack
* template
* policy

La promozione deve richiedere evidenza sufficiente e corretta applicabilità.

Un comportamento Huawei-specific non deve diventare una regola generale BGP.

Una decisione di progetto non deve diventare automaticamente knowledge globale.

# Conflict Resolution

Per CURRENT STATE:

```text
runtime evidence
>
historical documentation
```

Per ADDRESSING:

```text
IPAM
>
wiki references
```

Per INVENTORY:

```text
Asset Management
>
wiki references
```

Per PLATFORM BEHAVIOR:

```text
version-compatible vendor documentation / MCP
>
validated platform knowledge
>
validated lessons
>
agent inference
```

Per PROJECT DIRECTIVES:

```text
AGENTS.md
>
localwiki historical notes
```

Per STANDARD CONFIGURATION:

```text
approved Template Catalog
>
generated ad-hoc configuration
```

Safety invariant globali rimangono non derogabili.

# Version Awareness

La knowledge architecture deve rappresentare quando necessario:

* vendor
* Network OS
* OS release
* hardware family
* device family
* feature
* license
* supported API
* configuration model

Queste informazioni devono provenire preferibilmente da:

* runtime discovery
* Asset Management
* platform knowledge provider

# Device Identity

Non duplicare nella knowledge globale il record completo dei device.

Utilizzare riferimenti logici o asset identifier.

Esempio:

```yaml
device_ref:
  asset_id: NET-12345
```

Recuperare dal Source of Truth:

* hostname
* management address
* model
* device family
* serial
* lifecycle state

quando necessari.

# Topology Awareness

La topologia può essere derivata combinando:

* Network Source of Truth
* LLDP
* routing adjacencies
* architecture context
* project knowledge

Non trasformare llmwiki in un database topology completo se esiste una Source of Truth dedicata.

# Management Plane Safety

Progetta policy per proteggere:

* management interface
* management VRF
* management routing
* AAA
* SSH
* API
* NETCONF
* gNMI
* jump host path
* automation controller path

Prima di modifiche potenzialmente impattanti:

1. determinare management path
2. identificare dipendenze
3. determinare rollback
4. utilizzare safe commit quando disponibile
5. verificare reachability dopo la modifica

# Blast Radius

Classifica l'impatto.

Possibile tassonomia:

LOW

modifica locale senza traffico significativo coinvolto.

MEDIUM

modifica limitata a un link, segmento o servizio.

HIGH

modifica routing o topology con impatto potenzialmente multi-servizio.

CRITICAL

modifica core, backbone, default routing, route reflector, management plane o inter-site connectivity.

Il livello di rischio deve influenzare quali safety policy vengono caricate.

# Multi-device Transactions

Supporta:

PRECHECK
→ ORDERED CHANGE
→ INTERMEDIATE VALIDATION
→ NEXT DEVICE
→ END-TO-END VALIDATION
→ ROLLBACK IF REQUIRED

L'ordine può in futuro dipendere dall'architecture pack.

# Configuration Diff

Prima di applicare:

CURRENT CONFIG
→ DESIRED STATE
→ TEMPLATE / INTENT
→ EXPECTED DIFF

Preferire il cambiamento minimo sufficiente.

Quando disponibile utilizzare:

* candidate configuration
* configuration session
* compare
* commit check
* dry-run
* native validation
* rollback

# Idempotency

Le procedure e i template devono essere progettati per essere idempotenti quando possibile.

Prima di creare configurazione:

* verificare se esiste
* verificare se è equivalente
* applicare soltanto la differenza necessaria

# Agent Behaviour

Il ciclo operativo deve essere:

UNDERSTAND
→ LOAD PROJECT
→ CLASSIFY
→ RESOLVE CONTEXT
→ RETRIEVE
→ DISCOVER
→ INSPECT
→ MODEL
→ PLAN
→ PRECHECK
→ RENDER
→ DIFF
→ EXECUTE
→ VALIDATE
→ REPORT
→ LEARN

Dove:

UNDERSTAND

comprende l'intento.

LOAD PROJECT

carica AGENTS.md quando il task appartiene a un progetto.

CLASSIFY

determina architecture, domain, operation e rischio.

RESOLVE CONTEXT

determina moduli, Source of Truth, template e provider necessari.

RETRIEVE

recupera solamente la knowledge rilevante.

DISCOVER

determina platform, versione e capability.

INSPECT

osserva configurazione e stato operativo.

MODEL

costruisce il minimo modello necessario delle dipendenze.

PLAN

determina la modifica minima.

PRECHECK

verifica precondizioni, blast radius e rollback.

RENDER

genera configurazione quando necessario.

DIFF

confronta current e desired configuration.

EXECUTE

applica il cambiamento.

VALIDATE

verifica control plane, data plane e risultato finale.

REPORT

riporta azioni ed evidenze.

LEARN

produce eventuali learning candidate.

# Control Plane vs Data Plane

Distinguere sempre:

CONFIGURATION SUCCESS

CONTROL PLANE SUCCESS

DATA PLANE SUCCESS

END-TO-END SUCCESS

Esempio BGP:

il commit riuscito non implica sessione Established.

La sessione Established non implica che le route desiderate siano ricevute.

Le route ricevute non implicano automaticamente installazione in RIB/FIB.

La presenza in FIB non implica automaticamente successo end-to-end.

# Domain Routing Base

Crea:

`domains/routing/base.md`

con sole invarianti generali.

Deve includere quando rilevante:

* RIB vs FIB
* routing instance / VRF
* route source
* next-hop resolution
* address family
* path asymmetry
* dependencies

# Domain BGP Base

Crea:

`domains/bgp/base.md`

vendor-neutral.

Deve includere:

* local ASN
* remote ASN
* address family
* VRF/routing instance
* session state
* learned routes
* accepted routes
* installed routes
* advertised routes
* routing policy
* peer reachability

# Domain OSPF Base

Crea:

`domains/ospf/base.md`

vendor-neutral.

# Domain IS-IS Base

Crea:

`domains/isis/base.md`

vendor-neutral.

Deve includere invarianti relative a:

* system ID
* NET
* L1/L2
* adjacency
* interface participation
* DIS quando rilevante
* metrics
* database
* route installation
* address family
* eventuale Segment Routing quando applicabile

Non includere sintassi vendor-specific.

# Tool Abstraction

Prevedi semantic tools quali:

```text
get_device_facts
get_platform_capabilities
get_running_config
get_interface_state
get_route
get_bgp_neighbors
get_bgp_routes
get_ospf_neighbors
get_isis_adjacencies
get_isis_database
get_arp_table
get_nd_table
get_mac_table
get_lldp_neighbors
compare_configuration
validate_configuration
apply_configuration
rollback_configuration
ping
traceroute
```

Source of Truth tools:

```text
lookup_asset
lookup_device_family
lookup_ip_assignment
lookup_prefix
lookup_vrf
lookup_circuit
lookup_intended_state
```

Il backend può utilizzare:

* SSH
* NETCONF
* RESTCONF
* gNMI
* API
* MCP

Preferire semantic tools quando possibile.

Mantenere comunque una capability raw CLI per i casi non coperti.

# Token Budget

Priorità indicativa:

P0:

* core safety
* user objective
* AGENTS.md constraints
* critical runtime state

P1:

* directly relevant domain
* operation
* platform base
* architecture identifier/context

P2:

* procedure
* immediate topology
* template metadata

P3:

* retrieved vendor knowledge
* validated lessons
* terminology
* localwiki

P4:

* background material

Eliminare prima il contesto meno importante.

# Filesystem iniziale

Usa come base:

```text
/nando/
├── system/
│   ├── core.md
│   ├── safety.md
│   └── dispatcher.md
│
├── roles/
│   └── network-engineer/
│       ├── role.md
│       ├── principles.md
│       └── boundaries.md
│
├── domains/
│   ├── layer2/
│   ├── addressing/
│   ├── routing/
│   ├── bgp/
│   ├── ospf/
│   ├── isis/
│   ├── mpls/
│   ├── vrf/
│   ├── vxlan/
│   ├── evpn/
│   ├── qos/
│   ├── dns/
│   ├── dhcp/
│   └── telemetry/
│
├── platforms/
│   ├── cisco-ios/
│   ├── cisco-iosxe/
│   ├── cisco-iosxr/
│   ├── cisco-nxos/
│   ├── junos/
│   ├── arista-eos/
│   ├── huawei-vrp/
│   ├── frr/
│   └── linux/
│
├── architectures/
│   ├── ipbb/
│   ├── tbb/
│   ├── dcn/
│   └── facility-network/
│
├── operations/
│   ├── discover.md
│   ├── inspect.md
│   ├── audit.md
│   ├── configure.md
│   ├── troubleshoot.md
│   ├── migrate.md
│   ├── upgrade.md
│   ├── backup-restore.md
│   ├── validate.md
│   └── remove.md
│
├── procedures/
│   ├── project/
│   ├── layer2/
│   ├── routing/
│   ├── bgp/
│   ├── ospf/
│   ├── isis/
│   ├── mpls/
│   └── diagnostics/
│
├── policies/
│   ├── change-safety.md
│   ├── management-plane.md
│   ├── blast-radius.md
│   ├── destructive-actions.md
│   ├── rollback.md
│   ├── secrets.md
│   ├── multi-device-change.md
│   └── change-management.md
│
├── templates/
│   ├── catalog.yaml
│   ├── cisco/
│   ├── juniper/
│   ├── arista/
│   ├── huawei/
│   ├── frr/
│   └── linux/
│
├── providers/
│   ├── platform-knowledge/
│   ├── source-of-truth/
│   ├── ipam/
│   ├── asset-management/
│   ├── telemetry/
│   └── mcp/
│
├── tools/
│   ├── ssh.md
│   ├── netconf.md
│   ├── restconf.md
│   ├── gnmi.md
│   ├── snmp.md
│   ├── diagnostics.md
│   ├── llmwiki.md
│   └── mcp.md
│
├── context/
│   ├── schema.md
│   ├── architecture.md
│   ├── device-reference.md
│   ├── topology.md
│   ├── project.md
│   └── change.md
│
└── manifests/
    ├── capabilities.yaml
    ├── platforms.yaml
    ├── architectures.yaml
    ├── providers.yaml
    ├── templates.yaml
    ├── routing.yaml
    ├── dependencies.yaml
    └── policies.yaml
```

Non considerare questa struttura intoccabile.

Analizzala criticamente e semplificala se necessario.

# Output richiesto

Produci nell'ordine:

1. principi architetturali
2. diagramma logico
3. filesystem `/nando`
4. responsabilità delle directory
5. data ownership model
6. separazione core / role / architecture / platform / domain / operation / procedure
7. architecture registry minimale
8. Platform Knowledge Resolver
9. External Knowledge Provider model
10. Source of Truth Resolver
11. Project Workspace model
12. schema AGENTS.md
13. direttive localwiki contenute in AGENTS.md
14. procedure `initialize-project`
15. lifecycle della richiesta
16. task classifier/router
17. topology-aware context resolution
18. manifest schema
19. Prompt Module Resolver
20. Template Catalog
21. template selection algorithm
22. template rendering workflow
23. token budgeting
24. global knowledge structure
25. terminology incremental knowledge
26. learning_candidate schema
27. knowledge lifecycle
28. promotion criteria
29. conflict resolution
30. version/platform awareness
31. management-plane safety
32. blast radius
33. multi-device strategy
34. pre-check/post-check
35. esempi completi di routing
36. esempi di prompt composition
37. contenuto iniziale dei file principali
38. anti-pattern
39. roadmap incrementale

# Scenari obbligatori

## Scenario A — Gestione generica

"Gestisci edge-rtr-01."

Mostra:

* classificazione
* Asset Management quando necessario
* minimo retrieval
* moduli caricati
* moduli NON caricati

Non caricare automaticamente tutti i domain.

## Scenario B — Configurazione BGP

"Configura una sessione BGP verso ISP-A."

Mostra:

* project detection
* AGENTS.md
* architecture identifier se noto
* Asset Management
* IPAM
* platform detection
* MCP vendor quando necessario
* BGP domain
* eventuale template
* expected diff
* validation

## Scenario C — Troubleshooting BGP

"La sessione BGP verso ISP-A è down."

Mostra progressive troubleshooting.

Non assumere la causa.

Valuta progressivamente:

* reachability
* session configuration
* ASN
* address family
* authentication se applicabile
* routing policy
* control plane

## Scenario D — IS-IS

"L'adiacenza IS-IS non sale."

Mostra:

* domain IS-IS
* platform knowledge
* runtime adjacency
* interface state
* level compatibility
* NET/system ID relevance
* database quando necessario
* progressive disclosure

## Scenario E — Management risk

"Modifica la default route del router attraverso cui stai amministrando il device."

Mostra:

* HIGH/CRITICAL risk
* management path detection
* pre-check
* rollback
* safe commit quando disponibile
* post-change reachability

## Scenario F — Template

"Configura un nuovo PE secondo lo standard aziendale."

Mostra:

* Asset Management
* device family
* OS
* architecture identifier quando disponibile
* Template Catalog
* IPAM
* rendering
* diff
* validation
* application

Non inventare indirizzi o altri dati.

## Scenario G — Nuovo progetto

"Crea un nuovo progetto per la migrazione DC1."

La procedure `initialize-project` deve sapere direttamente come creare:

* AGENTS.md
* localwiki

con le direttive minime.

## Scenario H — Terminologia sconosciuta

"L'utente utilizza un acronimo aziendale mai incontrato prima."

Mostra:

knowledge lookup
→ eventuale project lookup
→ chiarimento o fonte se necessario
→ utilizzo
→ learning candidate
→ futura memorizzazione in `knowledge/terminology`

Non creare registry complessi.

## Scenario I — Conflict con IPAM

"localwiki contiene un IP differente da IPAM."

IPAM è autoritativo.

Non aggiornare automaticamente IPAM.

Identificare la localwiki come stale quando appropriato.

## Scenario J — Vendor MCP

"Una funzione della piattaforma non è conosciuta localmente."

Mostra:

platform detection
→ MCP vendor
→ version/applicability check
→ retrieved knowledge
→ task execution

Non precaricare tutta la documentazione vendor.

## Scenario K — Huawei

"Devo configurare una funzione BGP su un router Huawei con VRP."

Mostra:

* platform detection
* hardware/device-family lookup
* VRP version
* local Huawei platform module
* eventuale Huawei MCP/documentation provider
* BGP domain vendor-neutral
* eventuale template Huawei
* version compatibility
* diff
* validation

La conoscenza Huawei-specific deve rimanere nel platform layer o nel template, non nel domain BGP generale.

## Scenario L — Learning

"L'agente utilizza una sintassi errata. La documentazione vendor o MCP fornisce la forma corretta e la configurazione viene validata."

Mostra:

* learning candidate
* provenance
* platform applicability
* validation
* eventuale promotion verso platform knowledge o template

Non promuovere automaticamente verso il domain generale.

# Anti-pattern obbligatori

## Monolithic Prompt

Caricare tutta la conoscenza nel system prompt.

## Wiki as CMDB

Utilizzare llmwiki come inventario autoritativo.

## Wiki as IPAM

Registrare assegnazioni IP come memoria permanente.

## Hardcoded Inventory

Inserire IP, hostname, serial o asset nel system prompt.

## Vendor Documentation Prefill

Caricare documentazione vendor estesa senza necessità.

## Vendor Duplication

Duplicare l'intera conoscenza BGP per Cisco, Juniper, Huawei e altri vendor.

## Architecture Overload

Caricare tutte le architecture contemporaneamente.

## Platform Leakage

Inserire sintassi Junos, IOS-XR o Huawei VRP nel modulo BGP generale.

## Template as Knowledge

Usare template come sostituti della conoscenza di dominio.

## Knowledge as Template

Inserire configurazioni complete nei domain module.

## Project Leakage

Trasferire automaticamente decisioni di un progetto in altri progetti.

## Localwiki as Global Memory

Promuovere automaticamente la localwiki nella knowledge globale.

## Overengineered Terminology Registry

Costruire ontology o registry complessi per nomenclature che possono essere apprese progressivamente.

## Premature Taxonomy

Tentare di censire tutta la terminologia aziendale a priori.

## Guessing Corporate Terms

Inventare il significato di acronimi sconosciuti.

## Blind MCP Trust

Applicare documentazione recuperata senza verificare:

* vendor
* platform
* product
* version
* applicability

## Command Success Equals Task Success

Considerare riuscito il task soltanto perché il comando è stato accettato.

## Unsafe Remote Change

Modificare il management path senza rollback.

## Automatic Generalization

Trasformare un singolo incidente in regola generale.

## Unbounded Learning

Permettere all'agente di modificare autonomamente core, policy, architecture pack o template.

# Roadmap

## Phase 1 — Kernel e funzionamento minimo

* system core
* Network Engineer role
* deterministic routing
* Source of Truth Resolver
* global llmwiki
* terminology knowledge semplice
* AGENTS.md
* localwiki
* initialize-project
* platform discovery
* routing/base
* BGP
* OSPF
* IS-IS
* una o più piattaforme iniziali
* MCP vendor integration
* basic safety policies

## Phase 2 — Template e Source of Truth

* Template Catalog
* device-family awareness
* template selection
* template rendering
* IPAM
* Asset Management
* configuration diff
* idempotency

## Phase 3 — Architecture Packs

Predisporre e successivamente sviluppare architecture pack per:

* IPBB
* TBB
* DCN
* Facility Network

Non definirne oggi il contenuto tecnico.

## Phase 4 — Advanced Network Domains

* MPLS
* LDP
* Segment Routing
* EVPN
* VXLAN
* QoS
* advanced IPv6
* telemetry

## Phase 5 — Knowledge Lifecycle

* richer learning candidate workflow
* provenance
* validation
* promotion review
* obsolete knowledge detection
* template lifecycle
* platform knowledge lifecycle

# Obiettivo finale

L'architettura deve permettere che:

* il core rimanga piccolo
* Platform/Vendor Knowledge possa arrivare via MCP
* Huawei VRP sia supportabile come le altre piattaforme
* la documentazione vendor venga caricata solo quando serve
* nuovi vendor non facciano crescere linearmente il prefill
* nuovi protocolli non facciano crescere linearmente il prefill
* nuove architetture non facciano crescere linearmente il prefill
* IPBB, TBB, DCN e Facility Network siano censite senza inventarne ancora i dettagli
* IS-IS sia un domain di primo livello
* firewall non sia un domain dell'agente
* IPAM sia autoritativo per addressing
* Asset Management sia autoritativo per inventory
* llmwiki non duplichi Source of Truth
* nomenclature e corporate terminology siano knowledge incrementale
* nuovi termini possano essere appresi durante l'uso
* non sia necessaria una tassonomia aziendale completa iniziale
* AGENTS.md governi ogni progetto
* AGENTS.md contenga anche le direttive della localwiki
* initialize-project sappia creare direttamente AGENTS.md e localwiki
* i template siano separati dalla knowledge
* i template siano selezionabili per vendor, OS e device family
* project knowledge non contamini automaticamente global knowledge
* vendor syntax e domain knowledge rimangano separati
* configuration success non sia confuso con operational success
* la selezione dei moduli sia il più possibile deterministica
* ogni dato abbia un Source of Truth identificabile
* il sistema sia ispezionabile
* il sistema sia versionabile
* il kernel rimanga stabile mentre knowledge, platform, architecture, template e capability crescono nel tempo

Non limitarti a descrivere concetti.

Produci una struttura implementabile con esempi YAML, JSON e Markdown concretamente trasformabili nei file del progetto.

Quando trovi una scelta architetturale discutibile:

1. identifica il trade-off
2. confronta brevemente le alternative
3. scegli una soluzione
4. motivala

Privilegia:

* semplicità
* determinismo
* basso prefill
* ownership chiaro dei dati
* knowledge incrementale
* modularità
* sicurezza operativa

rispetto a un'architettura eccessivamente astratta.

Il risultato finale deve essere sufficientemente concreto da diventare la specifica iniziale dell'agente Network Engineering implementato sotto `/nando`.
