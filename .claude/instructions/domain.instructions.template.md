---
applyTo:
  - "**/*.cs"
  - "**/*.tsx"
  - "**/*.ts"
  - "**/models/**/*"
  - "**/types/**/*"
  - "**/services/**/*"
---

# Domain Instructions - [DOMAIN_CONTEXT]

## Domain Terminology

### [DOMAIN_LANGUAGE]-English Glossary
Always use [DOMAIN_LANGUAGE] terms in code for consistency:

| [DOMAIN_LANGUAGE] | English | Code Usage |
|----------|---------|-------------------|
| [CORE_ENTITY_1] | [ENGLISH_1] | `[Entity1]`, `[Entity1]Service` |
| [CORE_ENTITY_2] | [ENGLISH_2] | `[Entity2]`, `[Entity2]Form` |
| [CORE_ENTITY_3] | [ENGLISH_3] | `[Entity3]`, `[Entity3]List` |
| [CORE_ENTITY_4] | [ENGLISH_4] | `[Entity4]`, `[Entity4]Profile` |
| [CORE_ENTITY_5] | [ENGLISH_5] | `[Entity5]`, `[Entity5]Result` |
| [CORE_ENTITY_6] | [ENGLISH_6] | `[Entity6]`, `[Entity6]Number` |
| [CORE_ENTITY_7] | [ENGLISH_7] | `[Entity7]`, `[Entity7]Service` |
| [CORE_ENTITY_8] | [ENGLISH_8] | `[Entity8]`, `[Entity8]Final` |

### Main Entities

#### [Core Entity 1]
```typescript
interface [Entity1] {
  id: number;
  [property1]: string;                    // "[Example Value]"
  [property2]: string;
  [typeProperty]: [EntityType];           // '[type1]' | '[type2]' | '[type3]' | etc.
  [dateProperty1]: Date;
  [dateProperty2]: Date;
  [dateProperty3]: Date;
  
  // Logistics
  [locationProperty1]: string;          // "[Example Location]"
  [locationProperty2]?: string;         // If different from start
  [measureProperty1]: number;          // [Measurement description]
  [measureProperty2]?: number;         // For [specific use case]
  
  // [Business Context]
  [maxProperty]: number;
  [countProperty]: number;
  [priceProperty]: number;              // In [currency]
  
  // Organization
  [organizerProperty]: [OrganizerEntity];
  [statusProperty]: [StatusType];       // '[status1]' | '[status2]' | '[status3]' | '[status4]'
  [rulesProperty]?: string;             // URL or text of [rules/regulations]
}
```

#### [Core Entity 2]
```typescript
interface [Entity2] {
  id: number;
  
  // [Personal/Demographic data]
  [nameProperty]: string;
  [surnameProperty]: string;
  [birthProperty]: Date;
  [genderProperty]: 'M' | 'F';
  [nationalityProperty]: string;            // ISO code ([country codes])
  
  // [Contact info]
  [emailProperty]: string;
  [phoneProperty]?: string;
  
  // [Business context]
  [categoryProperty]: [CategoryEntity];
  [identifierProperty]?: number;
  [timestampProperty]: Date;
  
  // [Safety/Legal]
  [certificateProperty]?: boolean;
  [emergencyContactProperty]: {
    [contactNameProperty]: string;
    [contactPhoneProperty]: string;
  };
}
```

#### [Core Entity 3]
```typescript
interface [Entity3] {
  id: number;
  [participantProperty]: [Entity2];
  [eventProperty]: [Entity1];
  
  // [Time tracking]
  [startTimeProperty]: Date;
  [endTimeProperty]?: Date;
  [officialTimeProperty]?: string;        // "[time format]" (HH:MM:SS)
  
  // [Rankings]
  [overallPositionProperty]?: number;     // 1, 2, 3, etc.
  [categoryPositionProperty]?: number;
  
  // Status
  [statusProperty]: [ParticipationStatus];     // '[status1]' | '[status2]' | '[status3]' | '[status4]'
  [notesProperty]?: string;
}
```

### Types and Categories

#### [Entity Type]
```typescript
type [EntityType] = 
  | '[type-1]'        // [Description]
  | '[type-2]'        // [Description]
  | '[type-3]'        // [Description]
  | '[type-4]'        // [Description]
  | '[type-5]'        // [Description]
  | '[type-6]'        // [Description]
  | '[type-7]'        // [Description]
  | '[type-8]'        // [Description]
  | '[type-9]'        // [Description]
  | '[type-10]';      // [Description]
```

#### [Category Entity]
```typescript
interface [CategoryEntity] {
  [codeProperty]: string;             // "[code1]", "[code2]", "[code3]", "[code4]"
  [descriptionProperty]: string;      // "[Description format]"
  [minProperty]: number;
  [maxProperty]: number;
  [genderProperty]: 'M' | 'F' | 'MIXED';
}

// Standard [domain context] categories
const [STANDARD_CATEGORIES] = [
  { [codeProperty]: '[code1]', [descriptionProperty]: '[Description 1]', [minProperty]: 18, [maxProperty]: 19, [genderProperty]: 'M' },
  { [codeProperty]: '[code2]', [descriptionProperty]: '[Description 2]', [minProperty]: 18, [maxProperty]: 19, [genderProperty]: 'F' },
  { [codeProperty]: '[code3]', [descriptionProperty]: '[Description 3]', [minProperty]: 20, [maxProperty]: 22, [genderProperty]: 'M' },
  { [codeProperty]: '[code4]', [descriptionProperty]: '[Description 4]', [minProperty]: 20, [maxProperty]: 22, [genderProperty]: 'F' },
  { [codeProperty]: '[code5]', [descriptionProperty]: '[Description 5]', [minProperty]: 23, [maxProperty]: 34, [genderProperty]: 'M' },
  { [codeProperty]: '[code6]', [descriptionProperty]: '[Description 6]', [minProperty]: 23, [maxProperty]: 34, [genderProperty]: 'F' },
  { [codeProperty]: '[code7]', [descriptionProperty]: '[Description 7]', [minProperty]: 35, [maxProperty]: 39, [genderProperty]: 'M' },
  { [codeProperty]: '[code8]', [descriptionProperty]: '[Description 8]', [minProperty]: 35, [maxProperty]: 39, [genderProperty]: 'F' },
  // ... continues for all [category groups]
];
```

## Business Rules

### [Core Business Process]
```typescript
class [BusinessRules] {
  static [canPerformAction]([entity1]: [Entity1], [entity2]: [Entity2]): ValidationResult {
    const errors: string[] = [];
    
    // Check dates
    const [currentTime] = new Date();
    if ([currentTime] < [entity1].[startDateProperty]) {
      errors.push('[Action] not yet available');
    }
    if ([currentTime] > [entity1].[endDateProperty]) {
      errors.push('[Action] deadline passed');
    }
    
    // Check availability
    if ([entity1].[countProperty] >= [entity1].[maxProperty]) {
      errors.push('[Resource] is full');
    }
    
    // Check requirements
    const [requirement] = [calculateRequirement]([entity2].[requirementProperty], [entity1].[dateProperty]);
    if ([requirement] < [MINIMUM_VALUE]) {
      errors.push('[Requirement] not met: [minimum] required');
    }
    
    return {
      isValid: errors.length === 0,
      errors
    };
  }
  
  static [calculateCategory]([entity]: [Entity2], [dateProperty]: Date): [CategoryEntity] {
    const [calculatedValue] = [calculateValue]([entity].[valueProperty], [dateProperty]);
    
    // Logic to determine category based on calculated value and criteria
    for (const [category] of [STANDARD_CATEGORIES]) {
      if ([category].[criteriaProperty] === [entity].[criteriaProperty] &&
          [calculatedValue] >= [category].[minProperty] &&
          [calculatedValue] <= [category].[maxProperty]) {
        return [category];
      }
    }
    
    throw new Error('[Category] not found for [entity]');
  }
}
```

### [Identifier Management]
```typescript
class [IdentifierService] {
  static [assignIdentifier]([entity1]: [Entity1], [entity2]: [Entity2]): number {
    // [Identifier] assignment logic
    // - [Assignment rule 1]
    // - [Assignment rule 2]
    // - [Assignment rule 3]
    
    const [baseNumber] = [entity1].[countProperty] + 1;
    
    // Special logic for [special cases]
    if ([entity1].[maxProperty] > [THRESHOLD]) {
      return [assignSpecialIdentifier]([entity1], [entity2], [baseNumber]);
    }
    
    return [baseNumber];
  }
}
```

### [Time Tracking]
```typescript
interface [IntermediateTime] {
  [checkpointProperty]: number;
  [timeProperty]: Date;
  [elapsedProperty]: string;    // From [start point]
  [segmentProperty]: string;    // From previous checkpoint
}

class [TimeService] {
  static [calculateOfficialTime](
    [startTime]: Date, 
    [endTime]: Date
  ): string {
    const diffMs = [endTime].getTime() - [startTime].getTime();
    const [timeUnit1] = Math.floor(diffMs / (1000 * 60 * 60));
    const [timeUnit2] = Math.floor((diffMs % (1000 * 60 * 60)) / (1000 * 60));
    const [timeUnit3] = Math.floor((diffMs % (1000 * 60)) / 1000);
    
    return `${[timeUnit1].toString().padStart(2, '0')}:${[timeUnit2].toString().padStart(2, '0')}:${[timeUnit3].toString().padStart(2, '0')}`;
  }
  
  static [calculateAverageRate]([totalTime]: string, [distance]: number): string {
    // Calculate average rate per unit ([time format])
    const [[timeUnit1], [timeUnit2], [timeUnit3]] = [totalTime].split(':').map(Number);
    const [totalTimeInUnits] = [timeUnit1] * 3600 + [timeUnit2] * 60 + [timeUnit3];
    const [rateInUnits] = [totalTimeInUnits] / [distance];
    
    const [rateUnit1] = Math.floor([rateInUnits] / 60);
    const [rateUnit2] = Math.floor([rateInUnits] % 60);
    
    return `${[rateUnit1]}:${[rateUnit2].toString().padStart(2, '0')}`;
  }
}
```

## Common Patterns

### [Regional] Data Validation
```typescript
class [RegionalValidation] {
  static [validateRegionalId]([regionalId]: string): boolean {
    // Implement [regional identifier] validation
    const regex = /^[PATTERN_FOR_REGIONAL_ID]$/;
    return regex.test([regionalId].toUpperCase());
  }
  
  static [validatePhone]([phone]: string): boolean {
    // Validate [regional] numbers: [format examples]
    const regex = /^[PHONE_PATTERN]$/;
    return regex.test([phone]);
  }
  
  static [validatePostalCode]([postalCode]: string): boolean {
    // [Regional] postal codes: [format description]
    const regex = /^[POSTAL_CODE_PATTERN]$/;
    return regex.test([postalCode]);
  }
}
```

### [Regional] Formatting
```typescript
class [RegionalFormat] {
  static [formatDate]([date]: Date): string {
    return [date].toLocaleDateString('[LOCALE]');
  }
  
  static [formatTime]([date]: Date): string {
    return [date].toLocaleTimeString('[LOCALE]', { 
      hour: '2-digit', 
      minute: '2-digit' 
    });
  }
  
  static [formatCurrency]([amount]: number): string {
    return new Intl.NumberFormat('[LOCALE]', {
      style: 'currency',
      currency: '[CURRENCY]'
    }).format([amount]);
  }
  
  static [formatDistance]([distance]: number): string {
    if ([distance] < 1) {
      return `${Math.round([distance] * 1000)} [small_unit]`;
    }
    return `${[distance]} [large_unit]`;
  }
}
```

## User Messages

### Communication Templates
```typescript
const [MESSAGE_TEMPLATES] = {
  [confirmationMessage]: ([entity2]: [Entity2], [entity1]: [Entity1]) => `
    [Greeting] ${[entity2].[nameProperty]},
    
    [Confirmation message for] "${[entity1].[nameProperty]}" [action completed]!
    
    [Details section]:
    - [Detail 1]: ${[entity2].[property1]}
    - [Detail 2]: ${[entity2].[categoryProperty].[descriptionProperty]}
    - [Detail 3]: ${[RegionalFormat].[formatDate]([entity1].[dateProperty])}
    - [Detail 4]: ${[entity1].[locationProperty]}
    
    [Call to action message]!
    
    [Signature] ${[entity1].[organizerProperty].[nameProperty]}
  `,
  
  [reminderMessage]: ([entity2]: [Entity2], [entity1]: [Entity1]) => `
    [Greeting] ${[entity2].[nameProperty]},
    
    [Reminder message for] "${[entity1].[nameProperty]}"!
    
    [Reminder items]:
    - [Reminder item 1]
    - [Reminder item 2]
    - [Reminder item 3]
    
    [Closing message]!
  `
};
```

## Best Practices for [Domain Context]

### Security and Privacy
- Respect [PRIVACY_REGULATION] for [entity] data
- Anonymize data for public statistics
- Encrypt sensitive data ([sensitive data types])
- Implement explicit consent for communications

### Performance for [Large Scale Operations]
- Use caching for [real-time data]
- Implement pagination for [large lists]
- Optimize queries for [search operations]
- Use WebSocket for live updates

### Usability for [Target Users]
- Simple interface for non-technical [user type]
- Guided wizard for [complex operations]
- Predefined templates for common [use cases]
- Export data in [common formats]

## Domain-Specific Code Examples

### Complete [Business Process] Validator
```typescript
class [ProcessValidator] {
  static async [validateProcess](
    [entity1]: [Entity1], 
    [entityData]: Create[Entity2]Request
  ): Promise<ValidationResult> {
    const [errors]: string[] = [];
    
    // [Validation type 1]
    if (!this.[checkCondition1]([entity1])) {
      [errors].push('[Error message 1]');
    }
    
    // [Validation type 2]
    if ([entity1].[countProperty] >= [entity1].[maxProperty]) {
      [errors].push('[Error message 2]');
    }
    
    // [Validation type 3]
    if (![RegionalValidation].[validateEmail]([entityData].[emailProperty])) {
      [errors].push('[Error message 3]');
    }
    
    if (!this.[validateRequirement]([entityData].[requirementProperty], [entity1].[dateProperty])) {
      [errors].push('[Error message 4]');
    }
    
    // Check for duplicates
    const [alreadyExists] = await this.[checkDuplicate](
      [entity1].id, 
      [entityData].[uniqueProperty]
    );
    if ([alreadyExists]) {
      [errors].push('[Error message 5]');
    }
    
    return {
      isValid: [errors].length === 0,
      [errors]
    };
  }
  
  private static [checkCondition1]([entity1]: [Entity1]): boolean {
    const [currentTime] = new Date();
    return [currentTime] >= [entity1].[startDateProperty] && [currentTime] <= [entity1].[endDateProperty];
  }
}
```

### Smart [Identifier] Generator
```typescript
class [SmartIdentifierGenerator] {
  static [assignIdentifier]([entity1]: [Entity1], [entity2]: [Entity2]): number {
    // Strategy based on [context type]
    switch ([entity1].[typeProperty]) {
      case '[type1]':
        return this.[identifierStrategy1]([entity1], [entity2]);
      case '[type2]':
        return this.[identifierStrategy2]([entity1], [entity2]);
      default:
        return this.[defaultStrategy]([entity1], [entity2]);
    }
  }
  
  private static [identifierStrategy1]([entity1]: [Entity1], [entity2]: [Entity2]): number {
    // [Strategy 1]: [description]
    if ([entity2].[categoryProperty].[codeProperty].startsWith('[SPECIAL_PREFIX]')) {
      return this.[getNextSpecialNumber]([entity1]);
    }
    
    return this.[getNextRegularNumber]([entity1], [STARTING_NUMBER]); // Start from [number]
  }
  
  private static [identifierStrategy2]([entity1]: [Entity1], [entity2]: [Entity2]): number {
    // [Strategy 2]: [description]
    const [available] = this.[getAvailableNumbers]([entity1]);
    const randomIndex = Math.floor(Math.random() * [available].length);
    return [available][randomIndex];
  }
}
```