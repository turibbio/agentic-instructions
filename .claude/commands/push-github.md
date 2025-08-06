# Push to GitHub

Prepara e esegue il push sicuro delle modifiche a GitHub con validazioni appropriate.

## Obiettivi

- Validare le modifiche prima del push
- Eseguire controlli di qualità automatici
- Gestire commit message secondo convention
- Sincronizzare branch con remote
- Prevenire push di contenuto sensibile

## Istruzioni

1. **Pre-push Validation**:
   - Verifica stato del repository (`git status`)
   - Controlla se ci sono file non tracciati importanti
   - Valida che non ci siano conflitti pendenti
   - Verifica sync con remote (`git fetch`)

2. **Security Check**:
   - Scansiona per credenziali, API keys, passwords
   - Verifica file `.env` o sensibili in `.gitignore`
   - Controlla dimensioni file (per evitare push di file troppo grandi)

3. **Quality Gates**:
   - Suggerisci di eseguire test prima del push
   - Verifica lint/formatting se applicabile
   - Controlla che il build sia funzionante

4. **Commit Message**:
   - Analizza i cambiamenti per suggerire un commit message appropriato
   - Segui convenzioni (Conventional Commits se applicabile)
   - Includi context rilevante

5. **Push Strategy**:
   - Determina il branch target appropriato
   - Suggerisci se fare push diretto o creare PR
   - Considera branch protection rules

## Pre-push Checklist

```markdown
- [ ] Modifiche revisionate
- [ ] Test eseguiti e passing
- [ ] Nessun contenuto sensibile nel commit
- [ ] Commit message descrittivo
- [ ] Branch aggiornato con remote
- [ ] Build funzionante
```

## Comandi Suggeriti

Fornisci sempre i comandi git specifici per:
- Stage dei file appropriati
- Commit con message ottimizzato  
- Push al branch corretto
- Creazione di PR se necessaria

Include anche alternative per diversi scenari (force push, amend, etc.) con relative cautele.