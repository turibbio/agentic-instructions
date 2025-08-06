# Branch Management

Analizza lo stato corrente del repository e aiuta con la gestione dei branch.

## Obiettivi

- Analizzare la struttura dei branch esistenti
- Suggerire strategie di branching appropriate
- Aiutare con merge, rebase e cleanup dei branch
- Identificare branch obsoleti o non utilizzati
- Gestire conflitti di merge

## Istruzioni

1. Prima esegui `git status` e `git branch -a` per capire lo stato attuale
2. Analizza la cronologia dei commit recenti con `git log --oneline --graph --all -10`
3. Identifica:
   - Branch attivi vs inattivi
   - Branch che possono essere eliminati
   - Potenziali conflitti
   - Strategie di merge/rebase appropriate
4. Fornisci comandi git specifici e sicuri
5. Spiega le conseguenze di ogni operazione proposta
6. Suggerisci best practices per il workflow del team

## Esempi di operazioni

- Cleanup di branch merged
- Risoluzione conflitti
- Rebase interattivo
- Cherry-picking di commit specifici
- Creazione di branch con naming convention appropriata

Ricorda sempre di suggerire backup o strategie di rollback prima di operazioni distruttive.
