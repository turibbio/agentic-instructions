---
applyTo:
  - "frontend/**/*.tsx"
  - "frontend/**/*.ts"
  - "frontend/**/*.css"
  - "frontend/**/*.json"
  - "**/vite.config.ts"
  - "**/tsconfig*.json"
---

# Frontend Instructions - [FRONTEND_FRAMEWORK]

## Architecture

### Component Structure
Organize components based on responsibility:

```
src/
├── components/
│   ├── ui/                    # Reusable base components
│   │   ├── Button.tsx
│   │   ├── Input.tsx
│   │   └── Modal.tsx
│   ├── layout/               # Layout and navigation
│   │   ├── Header.tsx
│   │   ├── Sidebar.tsx
│   │   └── Footer.tsx
│   └── domain/               # Domain-specific components
│       ├── [Entity]Card.tsx
│       ├── [Entity]Form.tsx
│       └── [Entity]List.tsx
├── pages/                    # Main pages/routes
│   ├── HomePage.tsx
│   ├── [Entities]Page.tsx
│   └── [Entity]DetailPage.tsx
├── hooks/                    # Custom hooks
├── services/                 # API services
├── types/                    # Type definitions
└── utils/                    # Utility functions
```

## Coding Conventions

### React Components
Use functional components with TypeScript:

```tsx
interface [Entity]CardProps {
  [entity]: [Entity];
  on[Action]: ([entity]Id: number) => void;
  isLoading?: boolean;
}

export const [Entity]Card: React.FC<[Entity]CardProps> = ({ 
  [entity], 
  on[Action], 
  isLoading = false 
}) => {
  const handleClick = () => {
    on[Action]([entity].id);
  };

  return (
    <div className="[entity]-card">
      <h3>{[entity].[property]}</h3>
      <p>[Field Label]: {[entity].[dateProperty].toLocaleDateString('[LOCALE]')}</p>
      <button 
        onClick={handleClick} 
        disabled={isLoading}
        className="btn btn-primary"
      >
        {isLoading ? 'Loading...' : '[Action Label]'}
      </button>
    </div>
  );
};
```

### Custom Hooks
Create reusable hooks for shared logic:

```tsx
export const use[Entity]Data = ([entity]Id: number) => {
  const [[entity], set[Entity]] = useState<[Entity] | null>(null);
  const [isLoading, setIsLoading] = useState(true);
  const [error, setError] = useState<string | null>(null);

  useEffect(() => {
    const load[Entity] = async () => {
      try {
        setIsLoading(true);
        const data = await [entity]Service.get[Entity]([entity]Id);
        set[Entity](data);
      } catch (err) {
        setError('Error loading [entity]');
      } finally {
        setIsLoading(false);
      }
    };

    load[Entity]();
  }, [[entity]Id]);

  return { [entity], isLoading, error };
};
```

### Type Definitions
Define TypeScript types for the domain:

```typescript
// types/[entity].ts
export interface [Entity] {
  id: number;
  [property1]: string;
  [property2]: string;
  [dateProperty]: Date;
  [startDateProperty]: Date;
  [endDateProperty]: Date;
  [maxProperty]: number;
  [countProperty]: number;
  [priceProperty]: number;
  [categoryProperty]: [CategoryType];
  [relatedProperty]: [RelatedEntity];
}

export interface [RelatedEntity] {
  id: number;
  [property1]: string;
  [property2]: string;
  [property3]: string;
  [birthDateProperty]: Date;
  [genderProperty]: 'M' | 'F';
  [identifierProperty]?: number;
  [resultProperty]?: string;
}

export type [CategoryType] = 
  | '[category-1]' 
  | '[category-2]' 
  | '[category-3]' 
  | '[category-4]' 
  | '[category-5]';
```

### API Services
Centralize API calls in dedicated services:

```typescript
// services/[entity]Service.ts
class [Entity]Service {
  private readonly baseUrl = import.meta.env.VITE_API_BASE_URL || '[DEFAULT_API_URL]';

  async get[Entities](): Promise<[Entity][]> {
    const response = await fetch(`${this.baseUrl}/[entities]`);
    if (!response.ok) {
      throw new Error('[Error message for loading entities]');
    }
    return response.json();
  }

  async get[Entity](id: number): Promise<[Entity]> {
    const response = await fetch(`${this.baseUrl}/[entities]/${id}`);
    if (!response.ok) {
      throw new Error('[Error message for entity not found]');
    }
    return response.json();
  }

  async create[Entity]([entity]: Create[Entity]Request): Promise<[Entity]> {
    const response = await fetch(`${this.baseUrl}/[entities]`, {
      method: 'POST',
      headers: { 'Content-Type': 'application/json' },
      body: JSON.stringify([entity])
    });
    
    if (!response.ok) {
      const error = await response.json();
      throw new Error(error.message || '[Error message for entity creation]');
    }
    
    return response.json();
  }
}

export const [entity]Service = new [Entity]Service();
```

## State Management

### React State
For local state use useState and useReducer:

```tsx
// Simple state
const [isLoading, setIsLoading] = useState(false);

// Complex state
interface [Entity]State {
  step: number;
  formData: Partial<[Entity]FormData>;
  errors: Record<string, string>;
}

const [entity]Reducer = (
  state: [Entity]State, 
  action: [Entity]Action
): [Entity]State => {
  switch (action.type) {
    case 'SET_STEP':
      return { ...state, step: action.payload };
    case 'UPDATE_FORM_DATA':
      return { 
        ...state, 
        formData: { ...state.formData, ...action.payload } 
      };
    case 'SET_ERRORS':
      return { ...state, errors: action.payload };
    default:
      return state;
  }
};
```

### Context API
For global state use React Context:

```tsx
interface AuthContextType {
  user: User | null;
  login: (email: string, password: string) => Promise<void>;
  logout: () => void;
  isAuthenticated: boolean;
}

const AuthContext = createContext<AuthContextType | null>(null);

export const AuthProvider: React.FC<{ children: React.ReactNode }> = ({ children }) => {
  const [user, setUser] = useState<User | null>(null);

  const login = async (email: string, password: string) => {
    // Implement login logic
  };

  const logout = () => {
    setUser(null);
    localStorage.removeItem('authToken');
  };

  return (
    <AuthContext.Provider value={{ 
      user, 
      login, 
      logout, 
      isAuthenticated: !!user 
    }}>
      {children}
    </AuthContext.Provider>
  );
};

export const useAuth = () => {
  const context = useContext(AuthContext);
  if (!context) {
    throw new Error('useAuth must be used within AuthProvider');
  }
  return context;
};
```

## Form Handling

### Controlled Components
Use controlled components for complex forms:

```tsx
interface [Entity]FormProps {
  onSubmit: (data: [Entity]FormData) => void;
  isLoading?: boolean;
}

export const [Entity]Form: React.FC<[Entity]FormProps> = ({ 
  onSubmit, 
  isLoading = false 
}) => {
  const [formData, setFormData] = useState<[Entity]FormData>({
    [field1]: '',
    [field2]: '',
    [field3]: '',
    [field4]: '',
    [field5]: 'M'
  });

  const [errors, setErrors] = useState<Record<string, string>>({});

  const handleChange = (field: keyof [Entity]FormData) => (
    e: React.ChangeEvent<HTMLInputElement | HTMLSelectElement>
  ) => {
    setFormData(prev => ({
      ...prev,
      [field]: e.target.value
    }));
    
    // Remove error if present
    if (errors[field]) {
      setErrors(prev => {
        const newErrors = { ...prev };
        delete newErrors[field];
        return newErrors;
      });
    }
  };

  const validateForm = (): boolean => {
    const newErrors: Record<string, string> = {};

    if (!formData.[field1].trim()) {
      newErrors.[field1] = '[Field1] is required';
    }

    if (!formData.[field3].includes('@')) {
      newErrors.[field3] = 'Invalid email';
    }

    setErrors(newErrors);
    return Object.keys(newErrors).length === 0;
  };

  const handleSubmit = (e: React.FormEvent) => {
    e.preventDefault();
    if (validateForm()) {
      onSubmit(formData);
    }
  };

  return (
    <form onSubmit={handleSubmit} className="[entity]-form">
      <input
        type="text"
        placeholder="[Field1 Label]"
        value={formData.[field1]}
        onChange={handleChange('[field1]')}
        className={errors.[field1] ? 'error' : ''}
      />
      {errors.[field1] && <span className="error-message">{errors.[field1]}</span>}
      
      <button type="submit" disabled={isLoading}>
        {isLoading ? '[Loading Text]...' : '[Submit Text]'}
      </button>
    </form>
  );
};
```

## Styling

### CSS Modules
Use CSS modules for isolated styling:

```css
/* [Entity]Card.module.css */
.[entity]Card {
  border: 1px solid #ddd;
  border-radius: 8px;
  padding: 16px;
  margin-bottom: 16px;
  transition: box-shadow 0.2s ease;
}

.[entity]Card:hover {
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
}

.[entity]Title {
  font-size: 1.25rem;
  font-weight: 600;
  margin-bottom: 8px;
  color: #2d3748;
}

.[entity]Date {
  color: #718096;
  font-size: 0.875rem;
}
```

```tsx
import styles from './[Entity]Card.module.css';

export const [Entity]Card: React.FC<[Entity]CardProps> = ({ [entity] }) => {
  return (
    <div className={styles.[entity]Card}>
      <h3 className={styles.[entity]Title}>{[entity].[property]}</h3>
      <p className={styles.[entity]Date}>
        {[entity].[dateProperty].toLocaleDateString('[LOCALE]')}
      </p>
    </div>
  );
};
```

## Testing

### Component Testing
Use Vitest with React Testing Library:

```tsx
import { render, screen, fireEvent } from '@testing-library/react';
import { describe, it, expect, vi } from 'vitest';
import { [Entity]Card } from './[Entity]Card';

describe('[Entity]Card', () => {
  const mock[Entity] = {
    id: 1,
    [property]: '[Test Value]',
    [dateProperty]: new Date('2024-06-15'),
    // ... other fields
  };

  it('should show the [entity] [property]', () => {
    render(<[Entity]Card [entity]={mock[Entity]} on[Action]={() => {}} />);
    
    expect(screen.getByText('[Test Value]')).toBeInTheDocument();
  });

  it('should call on[Action] when button is clicked', () => {
    const mockOn[Action] = vi.fn();
    
    render(<[Entity]Card [entity]={mock[Entity]} on[Action]={mockOn[Action]} />);
    
    fireEvent.click(screen.getByText('[Action Label]'));
    
    expect(mockOn[Action]).toHaveBeenCalledWith(1);
  });
});
```

## Best Practices

### Performance
- Use `React.memo` for expensive components
- Implement lazy loading for routes
- Optimize images and assets
- Use `useMemo` and `useCallback` when appropriate

### Accessibility
- Use semantic HTML
- Implement ARIA attributes
- Handle focus and keyboard navigation
- Test with screen readers

### Error Handling
- Implement Error Boundaries
- Handle loading and error states
- Show user-friendly messages

### Anti-Patterns
- Don't abuse useEffect
- Avoid excessive prop drilling
- Don't directly modify state
- Don't mix business logic in UI components
- Don't use `any` without valid reasons
- Avoid inline functions in JSX for performance
- Don't forget dependency array in useEffect
- Don't use index as key in dynamic lists

## Advanced State Management

### Zustand for Global State (alternative to Context)
```tsx
import { create } from 'zustand';

interface AuthStore {
  user: User | null;
  isAuthenticated: boolean;
  login: (credentials: LoginCredentials) => Promise<void>;
  logout: () => void;
}

export const useAuthStore = create<AuthStore>((set, get) => ({
  user: null,
  isAuthenticated: false,
  
  login: async (credentials) => {
    const user = await authService.login(credentials);
    set({ user, isAuthenticated: true });
  },
  
  logout: () => {
    authService.logout();
    set({ user: null, isAuthenticated: false });
  }
}));
```

## [BUILD_TOOL] Configuration

### Environment Variables
```typescript
// vite-env.d.ts
interface ImportMetaEnv {
  readonly VITE_API_BASE_URL: string;
  readonly VITE_APP_TITLE: string;
}

interface ImportMeta {
  readonly env: ImportMetaEnv;
}
```

### Proxy for API
```typescript
// [build-tool].config.ts
export default defineConfig({
  plugins: [[framework]()],
  server: {
    proxy: {
      '/api': {
        target: '[BACKEND_URL]',
        changeOrigin: true
      }
    }
  }
});
```