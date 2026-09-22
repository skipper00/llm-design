# GESTIONE DEL REPOSITORY E DELLE REVISIONI (GIT MANAGEMENT)

Devi applicare le seguenti direttive ogni volta che analizzi, modifichi o gestisci un repository Git.

## 1. Standard dei messaggi di commit

Ogni volta che devi creare un commit o suggerire un messaggio di commit, devi utilizzare il seguente formato:

`<TIPO>[ambito opzionale]: <gitmoji> <descrizione breve>`

Esempi:

- `FEAT(auth): ✨ aggiungi autenticazione tramite OAuth`
- `FIX(api): 🐛 correggi la gestione delle risposte vuote`
- `DOCS: 📝 aggiorna le istruzioni di installazione`
- `REFACTOR(core): ♻️ semplifica la validazione degli input`

Il tipo deve essere scritto esclusivamente in lettere maiuscole.

Sono ammessi soltanto i seguenti tipi:

- `FEAT`: aggiunta di una nuova funzionalità o componente
- `FIX`: correzione di un errore o malfunzionamento
- `DOCS`: modifiche esclusive alla documentazione
- `STYLE`: modifiche alla formattazione senza impatto sulla logica
- `REFACTOR`: ristrutturazione del codice senza correzioni o nuove funzionalità
- `TEST`: aggiunta o modifica di test
- `CHORE`: configurazioni, build, CI/CD, manutenzione o dipendenze

Non inventare tipi diversi da quelli elencati.

## 2. Utilizzo delle Gitmoji

Ogni messaggio di commit deve contenere una Gitmoji coerente con la modifica.

Usa come riferimento la seguente associazione:

- `FEAT`: ✨
- `FIX`: 🐛
- `DOCS`: 📝
- `STYLE`: 🎨
- `REFACTOR`: ♻️
- `TEST`: ✅
- `CHORE`: 🔧

La Gitmoji deve essere collocata dopo i due punti e prima della descrizione:

`TIPO(ambito): GITMOJI descrizione`

Se il messaggio fornito contiene già una Gitmoji appropriata, mantienila e non aggiungerne una seconda. Se manca, aggiungila automaticamente in base al tipo di commit.

Se la Gitmoji presente non è coerente con la modifica, sostituiscila con quella appropriata.

## 3. Regole di scrittura e sintassi

### Forma imperativa

Scrivi la descrizione usando esclusivamente il modo imperativo.

Forme corrette:

- `aggiungi`
- `correggi`
- `rimuovi`
- `aggiorna`
- `semplifica`

Forme non ammesse:

- `aggiunto`
- `corretto`
- `aggiungo`
- `aggiornato`

### Lunghezza e capitalizzazione

La prima riga del messaggio deve:

- contenere al massimo 72 caratteri;
- iniziare con un tipo in maiuscolo;
- avere la descrizione dopo la Gitmoji con iniziale minuscola;
- non terminare con un punto fermo.

### Ambito

Usa un ambito solo quando identifica chiaramente il componente interessato:

`FIX(parser): 🐛 correggi la gestione dei valori nulli`

L’ambito deve essere:

- breve;
- scritto in minuscolo;
- privo di spazi;
- racchiuso tra parentesi tonde.

Non aggiungere un ambito generico o poco informativo.

## 4. Breaking changes

Se una modifica interrompe la retrocompatibilità, aggiungi `!` subito prima dei due punti:

- `FEAT!: ✨ rimuovi la vecchia API`
- `FEAT(api)!: ✨ modifica il formato delle risposte`

Aggiungi inoltre nel corpo del messaggio una spiegazione introdotta esattamente da:

`BREAKING CHANGE:`

Esempio completo:

```text
FEAT(api)!: ✨ modifica il formato delle risposte

BREAKING CHANGE: sostituisce il campo "data" con il campo "result"
nelle risposte pubbliche dell'API
```

## 5. Analisi preventiva delle modifiche

Prima di creare o suggerire un commit devi:

1. controllare lo stato del repository;
2. esaminare le modifiche effettive tramite `git diff`;
3. esaminare anche le modifiche in staging tramite `git diff --staged`;
4. verificare i file nuovi, modificati, rinominati o eliminati;
5. determinare il tipo, l’ambito e la Gitmoji in base alle modifiche reali;
6. verificare se la modifica introduce un breaking change.

Non dedurre il contenuto del commit soltanto dal nome del branch, dal ticket o dalla descrizione fornita dall’utente.

## 6. Correzione automatica dei messaggi

Se l’utente fornisce un messaggio vago, incompleto o non conforme, riformulalo automaticamente.

Esempio:

Input:

`risolto problema login`

Output:

`FIX(auth): 🐛 correggi l'errore durante il login`

Correggi automaticamente:

- tipo assente o errato;
- tipo scritto in minuscolo;
- Gitmoji assente o inappropriata;
- forma verbale non imperativa;
- iniziale maiuscola nella descrizione;
- punto fermo finale;
- lunghezza superiore a 72 caratteri;
- ambito poco chiaro;
- indicazione mancante di un breaking change.

Non alterare il significato tecnico delle modifiche e non inventare dettagli che non risultano dal repository.

## 7. Coerenza del commit

Ogni commit deve rappresentare una modifica logica, coerente e circoscritta.

Se le modifiche includono interventi indipendenti appartenenti a tipi diversi, proponi di dividerle in commit separati. Non combinare arbitrariamente funzionalità, correzioni, refactoring e aggiornamenti documentali nello stesso commit.

Prima di eseguire il commit:

- verifica quali file sono in staging;
- evita di includere file estranei alla modifica;
- non aggiungere automaticamente file sensibili;
- segnala eventuali segreti, credenziali, chiavi o configurazioni riservate;
- non modificare la cronologia Git senza un’esplicita richiesta dell’utente.

## 8. Output richiesto

Quando proponi un messaggio di commit, restituisci prima il messaggio completo pronto all’uso.

Se necessario, aggiungi successivamente una breve spiegazione della scelta del tipo, dell’ambito o della Gitmoji. Non presentare messaggi non conformi come alternative valide.
