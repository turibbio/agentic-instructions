---
applyTo:
  - "**/*.[BACKEND_EXT]"
  - "**/*.[FRONTEND_EXT]"
  - "**/models/**/*"
  - "**/types/**/*"
  - "**/services/**/*"
---

# Domain Instructions - [DOMAIN_CONTEXT]

## Domain Terminology

### [UI_LANGUAGE]-[TECH_LANGUAGE] Glossary
Always use [UI_LANGUAGE] terms in code for consistency:

| [UI_LANGUAGE] | [TECH_LANGUAGE] | Code Usage |
|----------|---------|-------------------|
| [ENTITY_1] | [Entity1] | `[Entity_1]`, `[Entity_1]Service` |
| [ENTITY_2] | [Entity2] | `[Entity_2]`, `[Entity_2]Form` |
| [ENTITY_3] | [Entity3] | `[Entity_3]`, `[Entity_3]List` |
| [ENTITY_4] | [Entity4] | `[Entity_4]`, `[Entity_4]Profile` |
| [ENTITY_5] | [Entity5] | `[Entity_5]`, `[Entity_5]Result` |
| [ENTITY_7] | [Entity7] | `[Entity_7]`, `[Entity_7]Number` |
| [ENTITY_6] | [Entity6] | `[Entity_6]`, `[Entity_6]Service` |
| [ENTITY_8] | [Entity8] | `[Entity_8]`, `[Entity_8]Final` |

### Main Entities

#### [Entity_1]
```typescript
interface [Entity_1] {
  id: number;
  [property1]: string;                    // "[Example name]"
  [property2]: string;
  [property3]: [Type1];                   // '[value1]' | '[value2]' | '[value3]' | etc.
  [dateProperty1]: Date;
  [dateProperty2]: Date;
  [dateProperty3]: Date;
  
  // Logistics
  [locationProperty1]: string;            // "[Example location]"
  [locationProperty2]?: string;           // If different from start
  [measureProperty1]: number;             // [Unit description]
  [measureProperty2]?: number;            // For [specific contexts]
  
  // Participation
  [maxProperty]: number;
  [currentProperty]: number;
  [priceProperty]: number;                // In [currency]
  
  // Organization
  [organizerProperty]: [Entity_4];
  [statusProperty]: [StatusType];         // '[status1]' | '[status2]' | '[status3]' | '[status4]'
  [rulesProperty]?: string;               // URL or text of regulations
}
```

#### [Entity_3]
```typescript
interface [Entity_3] {
  id: number;
  
  // Personal data
  [personalProperty1]: string;
  [personalProperty2]: string;
  [personalProperty3]: Date;
  [personalProperty4]: '[gender1]' | '[gender2]';
  [personalProperty5]: string;            // [Format description] (IT, FR, DE, etc.)
  
  // Contact
  [contactProperty1]: string;
  [contactProperty2]?: string;
  
  // [Context]
  [categoryProperty]: [CategoryType];
  [identifierProperty]?: number;
  [timestampProperty]: Date;
  
  // Medical/Safety
  [medicalProperty]?: boolean;
  [emergencyProperty]: {
    [emergencyName]: string;
    [emergencyContact]: string;
  };
}
```

#### [Entity_5]
```typescript
interface [Entity_5] {
  id: number;
  [participantProperty]: [Entity_3];
  [eventProperty]: [Entity_1];
  
  // Times
  [timeProperty1]: Date;
  [timeProperty2]?: Date;
  [timeProperty3]?: string;        // "[Time format]" (HH:MM:SS)
  
  // Rankings
  [rankProperty1]?: number;        // 1, 2, 3, etc.
  [rankProperty2]?: number;
  
  // Status
  [statusProperty]: [ParticipationStatus];     // '[status1]' | '[status2]' | '[status3]' | '[status4]'
  [notesProperty]?: string;
}
```

### Types and Categories

#### [Context] Types
```typescript
type [Type1] = 
  | '[type1]'           // [Description 1]
  | '[type2]'           // [Description 2]
  | '[type3]'           // [Description 3]
  | '[type4]'           // [Description 4]
  | '[type5]'           // [Description 5]
  | '[type6]'           // [Description 6]
  | '[type7]'           // [Description 7]
  | '[type8]'           // [Description 8]
  | '[type9]'           // [Description 9]
  | '[type10]';         // [Description 10]
```

#### [Entity_3] Categories
```typescript
interface [CategoryType] {
  [codeProperty]: string;             // "[Code1]", "[Code2]", "[Code3]", "[Code4]"
  [descriptionProperty]: string;      // "[Description example]"
  [minProperty]: number;
  [maxProperty]: number;
  [genderProperty]: '[gender1]' | '[gender2]' | '[mixed]';
}

// Standard [context] categories
const [STANDARD_CATEGORIES] = [
  { [codeProperty]: '[Code1]', [descriptionProperty]: '[Description 1]', [minProperty]: 18, [maxProperty]: 19, [genderProperty]: '[gender1]' },
  { [codeProperty]: '[Code2]', [descriptionProperty]: '[Description 2]', [minProperty]: 18, [maxProperty]: 19, [genderProperty]: '[gender2]' },
  { [codeProperty]: '[Code3]', [descriptionProperty]: '[Description 3]', [minProperty]: 20, [maxProperty]: 22, [genderProperty]: '[gender1]' },
  { [codeProperty]: '[Code4]', [descriptionProperty]: '[Description 4]', [minProperty]: 20, [maxProperty]: 22, [genderProperty]: '[gender2]' },
  { [codeProperty]: '[Code5]', [descriptionProperty]: '[Description 5]', [minProperty]: 23, [maxProperty]: 34, [genderProperty]: '[gender1]' },
  { [codeProperty]: '[Code6]', [descriptionProperty]: '[Description 6]', [minProperty]: 23, [maxProperty]: 34, [genderProperty]: '[gender2]' },
  { [codeProperty]: '[Code7]', [descriptionProperty]: '[Description 7]', [minProperty]: 35, [maxProperty]: 39, [genderProperty]: '[gender1]' },
  { [codeProperty]: '[Code8]', [descriptionProperty]: '[Description 8]', [minProperty]: 35, [maxProperty]: 39, [genderProperty]: '[gender2]' },
  // ... continues for all age groups
];
```

## Business Rules

### [Entity_2] Rules
```typescript
class [Entity_2]BusinessRules {
  static [canPerformAction]([eventParam]: [Entity_1], [participantParam]: [Entity_3]): ValidationResult {
    const errors: string[] = [];
    
    // Check dates
    const [currentTime] = new Date();
    if ([currentTime] < [eventParam].[startDateProperty]) {
      errors.push('[Start date validation message]');
    }
    if ([currentTime] > [eventParam].[endDateProperty]) {
      errors.push('[End date validation message]');
    }
    
    // Check available spots
    if ([eventParam].[participantsProperty] >= [eventParam].[maxProperty]) {
      errors.push('[Capacity validation message]');
    }
    
    // Check minimum requirement
    const [calculatedValue] = [calculateFunction]([participantParam].[dateProperty], [eventParam].[dateProperty]);
    if ([calculatedValue] < [minimumValue]) {
      errors.push('[Minimum requirement validation message]');
    }
    
    return {
      isValid: errors.length === 0,
      errors
    };
  }
  
  static [calculateCategory]([participantParam]: [Entity_3], [eventDate]: Date): [CategoryType] {
    const [calculatedValue] = [calculateFunction]([participantParam].[dateProperty], [eventDate]);
    
    // Logic to determine category based on calculated value and other properties
    for (const [category] of [STANDARD_CATEGORIES]) {
      if ([category].[genderProperty] === [participantParam].[genderProperty] &&
          [calculatedValue] >= [category].[minProperty] &&
          [calculatedValue] <= [category].[maxProperty]) {
        return [category];
      }
    }
    
    throw new Error('[Category not found error message]');
  }
}
```

### [Entity_7] Management
```typescript
class [Entity_7]Service {
  static [assignMethod]([eventParam]: [Entity_1], [participantParam]: [Entity_3]): number {
    // [Assignment logic description]
    // - [Rule 1]
    // - [Rule 2]
    // - [Rule 3]
    
    const [baseValue] = [eventParam].[participantsProperty] + 1;
    
    // Special logic for large events
    if ([eventParam].[maxProperty] > [threshold]) {
      return [specialAssignmentMethod]([eventParam], [participantParam], [baseValue]);
    }
    
    return [baseValue];
  }
}
```

### [Entity_6] System
```typescript
interface [IntermediateType] {
  [measureProperty]: number;
  [timeProperty]: Date;
  [elapsedProperty]: string;    // From start
  [segmentProperty]: string;    // From previous checkpoint
}

class [Entity_6]Service {
  static [calculateOfficialTime](
    [startTime]: Date, 
    [endTime]: Date
  ): string {
    const [timeDiff] = [endTime].getTime() - [startTime].getTime();
    const [hours] = Math.floor([timeDiff] / (1000 * 60 * 60));
    const [minutes] = Math.floor(([timeDiff] % (1000 * 60 * 60)) / (1000 * 60));
    const [seconds] = Math.floor(([timeDiff] % (1000 * 60)) / 1000);
    
    return `${[hours].toString().padStart(2, '0')}:${[minutes].toString().padStart(2, '0')}:${[seconds].toString().padStart(2, '0')}`;
  }
  
  static [calculateAveragePace]([totalTime]: string, [distance]: number): string {
    // Calculate average pace per unit ([pace format])
    const [[hours], [minutes], [seconds]] = [totalTime].split(':').map(Number);
    const [totalSeconds] = [hours] * 3600 + [minutes] * 60 + [seconds];
    const [paceSeconds] = [totalSeconds] / [distance];
    
    const [paceMinutes] = Math.floor([paceSeconds] / 60);
    const [paceSecondsRest] = Math.floor([paceSeconds] % 60);
    
    return `${[paceMinutes]}:${[paceSecondsRest].toString().padStart(2, '0')}`;
  }
}
```

## Common Patterns

### [LOCALE] Data Validation
```typescript
class [ValidationClass] {
  static [validateMethod1]([param]: string): boolean {
    // Implement [locale-specific] validation
    const regex = /^[REGEX_PATTERN]$/;
    return regex.test([param].toUpperCase());
  }
  
  static [validateMethod2]([param]: string): boolean {
    // Validate [locale-specific] format
    const regex = /^[REGEX_PATTERN]$/;
    return regex.test([param]);
  }
  
  static [validateMethod3]([param]: string): boolean {
    // [Locale] specific format validation
    const regex = /^[REGEX_PATTERN]$/;
    return regex.test([param]);
  }
}
```

### [LOCALE] Formatting
```typescript
class [FormatClass] {
  static [formatDate]([date]: Date): string {
    return [date].toLocaleDateString('[LOCALE_CODE]');
  }
  
  static [formatTime]([date]: Date): string {
    return [date].toLocaleTimeString('[LOCALE_CODE]', { 
      hour: '2-digit', 
      minute: '2-digit' 
    });
  }
  
  static [formatCurrency]([amount]: number): string {
    return new Intl.NumberFormat('[LOCALE_CODE]', {
      style: 'currency',
      currency: '[CURRENCY_CODE]'
    }).format([amount]);
  }
  
  static [formatMeasurement]([value]: number): string {
    if ([value] < [threshold]) {
      return `${Math.round([value] * [conversion])} [small_unit]`;
    }
    return `${[value]} [large_unit]`;
  }
}
```

## User Messages

### Communication Templates
```typescript
const [TEMPLATE_MESSAGES] = {
  [confirmationTemplate]: ([participantParam]: [Entity_3], [eventParam]: [Entity_1]) => `
    [Greeting] ${[participantParam].[nameProperty]},
    
    [Confirmation message] "${[eventParam].[nameProperty]}" [confirmation text]!
    
    [Details header]:
    - [Detail 1]: ${[participantParam].[identifierProperty]}
    - [Detail 2]: ${[participantParam].[categoryProperty].[descriptionProperty]}
    - [Detail 3]: ${[FormatClass].[formatDate]([eventParam].[dateProperty])}
    - [Detail 4]: ${[eventParam].[locationProperty]}
    
    [Closing message]!
    
    [Signature] ${[eventParam].[organizerProperty].[nameProperty]}
  `,
  
  [reminderTemplate]: ([participantParam]: [Entity_3], [eventParam]: [Entity_1]) => `
    [Greeting] ${[participantParam].[nameProperty]},
    
    [Reminder message] "${[eventParam].[nameProperty]}"!
    
    [Reminder list header]:
    - [Reminder 1]
    - [Reminder 2]
    - [Reminder 3]
    
    [Closing message]!
  `
};
```

## Best Practices for Domain

### Security and Privacy
- Respect [DATA_PROTECTION_REGULATION] for participant data
- Anonymize data for public statistics
- Encrypt sensitive data (medical certificates)
- Implement explicit consent for communications

### Performance for Large Events
- Use caching for real-time rankings
- Implement pagination for participant lists
- Optimize queries for name/identifier searches
- Use WebSocket for live updates

### Usability for [TARGET_AUDIENCE]
- Simple interface for non-technical organizers
- Guided wizard for event creation
- Predefined templates for common events
- Export data in Excel/PDF formats

## Domain-Specific Code Examples

### Complete [Entity_2] Validator
```typescript
class [Entity_2]Validator {
  static async [validateMethod](
    [eventParam]: [Entity_1], 
    [participantData]: Create[Entity_3]Request
  ): Promise<ValidationResult> {
    const [errors]: string[] = [];
    
    // Temporal validations
    if (!this.[isOpenMethod]([eventParam])) {
      [errors].push('[Closed validation message]');
    }
    
    // Capacity validations
    if ([eventParam].[participantsProperty] >= [eventParam].[maxProperty]) {
      [errors].push('[Full capacity validation message]');
    }
    
    // Data validations
    if (![ValidationClass].[validateMethod1]([participantData].[emailProperty])) {
      [errors].push('[Invalid email validation message]');
    }
    
    if (!this.[validateMinimumRequirement]([participantData].[dateProperty], [eventParam].[dateProperty])) {
      [errors].push('[Minimum requirement validation message]');
    }
    
    // Check for duplicates
    const [alreadyExists] = await this.[checkDuplicateMethod](
      [eventParam].id, 
      [participantData].[emailProperty]
    );
    if ([alreadyExists]) {
      [errors].push('[Duplicate participant validation message]');
    }
    
    return {
      isValid: [errors].length === 0,
      [errors]
    };
  }
  
  private static [isOpenMethod]([eventParam]: [Entity_1]): boolean {
    const [currentTime] = new Date();
    return [currentTime] >= [eventParam].[startDateProperty] && [currentTime] <= [eventParam].[endDateProperty];
  }
}
```

### Smart [Entity_7] Generator
```typescript
class [Entity_7]Generator {
  static [assignMethod]([eventParam]: [Entity_1], [participantParam]: [Entity_3]): number {
    // Strategy based on event type
    switch ([eventParam].[typeProperty]) {
      case '[type1]':
        return this.[specialMethod1]([eventParam], [participantParam]);
      case '[type2]':
        return this.[specialMethod2]([eventParam], [participantParam]);
      default:
        return this.[standardMethod]([eventParam], [participantParam]);
    }
  }
  
  private static [specialMethod1]([eventParam]: [Entity_1], [participantParam]: [Entity_3]): number {
    // [Special logic description]
    if ([participantParam].[categoryProperty].[codeProperty].startsWith('[SPECIAL_PREFIX]')) {
      return this.[getSpecialNumber]([eventParam]);
    }
    
    return this.[getRegularNumber]([eventParam], [startingNumber]); // Starts from [startingNumber]
  }
  
  private static [specialMethod2]([eventParam]: [Entity_1], [participantParam]: [Entity_3]): number {
    // [Alternative logic description]
    const [availableNumbers] = this.[getAvailableNumbers]([eventParam]);
    const [randomIndex] = Math.floor(Math.random() * [availableNumbers].length);
    return [availableNumbers][[randomIndex]];
  }
}
```