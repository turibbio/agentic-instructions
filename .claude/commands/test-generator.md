# Test Generator

Analizza il codice esistente e genera test completi con diversi scenari e edge cases.

## Obiettivi

- Generare unit test completi per funzioni/classi
- Creare integration test appropriati
- Identificare edge cases e scenari limite
- Suggerire test data e mock appropriati
- Migliorare code coverage

## Istruzioni

1. **Analisi del Codice**:
   - Identifica funzioni/metodi pubblici da testare
   - Analizza parametri di input e tipi di ritorno
   - Individua dipendenze esterne da mockare
   - Rileva condizioni logiche e branch da coprire

2. **Tipi di Test da Generare**:
   - **Unit Tests**: Test isolati per singole funzioni
   - **Integration Tests**: Test per interazioni tra componenti
   - **Edge Cases**: Valori limite, null, undefined, empty
   - **Error Cases**: Gestione eccezioni e errori
   - **Performance Tests**: Se applicabile

3. **Test Scenarios**:
   - **Happy Path**: Scenario normale di successo
   - **Error Path**: Gestione errori e eccezioni
   - **Boundary Values**: Valori limite e edge cases
   - **Invalid Input**: Input malformati o invalidi
   - **State Changes**: Verificare cambi di stato

4. **Test Structure**:
   - Setup/Arrange: Preparazione dati e mocks
   - Act: Esecuzione della funzione sotto test
   - Assert: Verifiche sui risultati
   - Cleanup: Pulizia risorse se necessario

## Test Template

```markdown
## 🧪 Test Suite per [Componente]

### Unit Tests

#### ✅ Happy Path Tests
```[linguaggio]
describe('[ComponentName]', () => {
  it('should [expected behavior] when [condition]', () => {
    // Arrange
    const input = [test data];
    const expected = [expected result];
    
    // Act
    const result = componentMethod(input);
    
    // Assert
    expect(result).toBe(expected);
  });
});
```

#### ❌ Error Cases
```[linguaggio]
it('should throw error when [invalid condition]', () => {
  expect(() => componentMethod(invalidInput)).toThrow('[expected error]');
});
```

#### 🔄 Edge Cases
```[linguaggio]
it('should handle empty input', () => {
  expect(componentMethod('')).toBe(expectedEmptyResult);
});

it('should handle null/undefined input', () => {
  expect(componentMethod(null)).toBe(expectedNullResult);
});
```

### Integration Tests
[Test per interazioni tra componenti]

### Mock Setup
```[linguaggio]
const mockDependency = jest.fn();
jest.mock('[dependency-path]', () => mockDependency);
```

### Test Data
```[linguaggio]
const testData = {
  valid: [valid test cases],
  invalid: [invalid test cases],
  edge: [edge cases]
};
```
```

Adatta il template al framework di testing utilizzato (Jest, Mocha, PyTest, etc.) e fornisci setup specifico per mocking delle dipendenze.