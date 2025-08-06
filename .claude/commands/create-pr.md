# Create Pull Request

Analizza le modifiche correnti e crea una pull request completa e ben documentata.

## Obiettivi

- Analizzare i cambiamenti nel branch corrente
- Generare un titolo e una descrizione dettagliata per la PR
- Identificare reviewer appropriati
- Suggerire label e milestone
- Creare checklist per il review

## Istruzioni

1. Esamina i file modificati con `git diff --name-status`
2. Analizza i commit del branch con `git log origin/main..HEAD --oneline`
3. Leggi il contenuto dei file principali modificati
4. Genera:
   - Titolo conciso e descrittivo
   - Descrizione dettagliata con contesto
   - Lista delle modifiche principali
   - Istruzioni per il testing
   - Screenshots o esempi se necessario
5. Identifica potenziali reviewer basandoti sui file modificati
6. Suggerisci label appropriate (feature, bugfix, documentation, etc.)
7. Crea una checklist di review

## Template PR

```markdown
## Descrizione
[Descrizione dettagliata delle modifiche]

## Tipo di modifica
- [ ] Bug fix
- [ ] Nuova feature
- [ ] Breaking change
- [ ] Documentazione

## Come testare
1. [Step 1]
2. [Step 2]
3. [Step 3]

## Checklist
- [ ] Il codice segue le style guidelines
- [ ] Ho eseguito un self-review del codice
- [ ] Ho commentato le parti complesse
- [ ] I test passano tutti
- [ ] La documentazione è aggiornata
```

Fornisci anche il comando git o GitHub CLI per creare effettivamente la PR.