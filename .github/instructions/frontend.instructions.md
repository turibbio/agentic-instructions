---
applyTo:
  - "frontend/**/*.[FRONTEND_EXT]"
  - "frontend/**/*.[FRONTEND_SCRIPT_EXT]"
  - "frontend/**/*.[STYLE_EXT]"
  - "frontend/**/*.[CONFIG_EXT]"
  - "**/*.[BUILD_CONFIG_EXT]"
  - "**/*.[TS_CONFIG_EXT]"
---

# Frontend Instructions - [FRONTEND_TECH]

## Architecture

### Component Structure
Organize components based on responsibility:

```
src/
├── components/
│   ├── ui/                    # Reusable base components
│   │   ├── Button.[FRONTEND_EXT]
│   │   ├── Input.[FRONTEND_EXT]
│   │   └── Modal.[FRONTEND_EXT]
│   ├── layout/               # Layout and navigation
│   │   ├── Header.[FRONTEND_EXT]
│   │   ├── Sidebar.[FRONTEND_EXT]
│   │   └── Footer.[FRONTEND_EXT]
│   └── domain/               # Domain-specific components
│       ├── [Entity]Card.[FRONTEND_EXT]
│       ├── [Action]Form.[FRONTEND_EXT]
│       └── [Entity]List.[FRONTEND_EXT]
├── pages/                    # Main pages/routes
│   ├── HomePage.[FRONTEND_EXT]
│   ├── [Entities]Page.[FRONTEND_EXT]
│   └── [Entity]DetailPage.[FRONTEND_EXT]
├── hooks/                    # Custom hooks
├── services/                 # API services
├── types/                    # Type definitions
└── utils/                    # Utility functions
```

## Coding Conventions

### [FRONTEND_FRAMEWORK] Components
Use functional components with [FRONTEND_LANGUAGE]:

```[FRONTEND_LANGUAGE]
interface [Entity]CardProps {
  [entity]: [Entity];
  [onAction]: ([entity]Id: number) => void;
  isLoading?: boolean;
}

export const [Entity]Card: [COMPONENT_TYPE]<[Entity]CardProps> = ({ 
  [entity], 
  [onAction], 
  isLoading = false 
}) => {
  const handleClick = () => {
    [onAction]([entity].id);
  };

  return (
    <div className="[entity]-card">
      <h3>{[entity].[nameProperty]}</h3>
      <p>Date: {[entity].[dateProperty].toLocaleDateString('[LOCALE]')}</p>
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

```[FRONTEND_LANGUAGE]
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
Define [FRONTEND_LANGUAGE] types for the domain:

```[FRONTEND_LANGUAGE]
// types/[entity].[FRONTEND_SCRIPT_EXT]
export interface [Entity] {
  id: number;
  [property1]: string;
  [property2]: string;
  [dateProperty]: Date;
  [dateProperty2]: Date;
  [dateProperty3]: Date;
  [maxProperty]: number;
  [currentProperty]: number;
  [priceProperty]: number;
  [categoryProperty]: [CategoryType];
  [organizerProperty]: [OrganizerType];
}

export interface [RelatedEntity] {
  id: number;
  [property1]: string;
  [property2]: string;
  [property3]: string;
  [dateProperty]: Date;
  [genderProperty]: '[GENDER1]' | '[GENDER2]';
  [identifierProperty]?: number;
  [timeProperty]?: string;
}

export type [CategoryType] = 
  | '[category1]' 
  | '[category2]' 
  | '[category3]' 
  | '[category4]' 
  | '[category5]';
```

### API Services
Centralize API calls in dedicated services:

```[FRONTEND_LANGUAGE]
// services/[entity]Service.[FRONTEND_SCRIPT_EXT]
class [Entity]Service {
  private readonly baseUrl = import.meta.env.[API_BASE_URL_VAR] || '[DEFAULT_API_URL]';

  async get[Entities](): Promise<[Entity][]> {
    const response = await fetch(`${this.baseUrl}/[entities]`);
    if (!response.ok) {
      throw new Error('[Error loading entities message]');
    }
    return response.json();
  }

  async get[Entity](id: number): Promise<[Entity]> {
    const response = await fetch(`${this.baseUrl}/[entities]/${id}`);
    if (!response.ok) {
      throw new Error('[Entity not found message]');
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
      throw new Error(error.message || '[Error creating entity message]');
    }
    
    return response.json();
  }
}

export const [entity]Service = new [Entity]Service();
```

## State Management

### [FRONTEND_FRAMEWORK] State
For local state use useState and useReducer:

```[FRONTEND_LANGUAGE]
// Simple state
const [isLoading, setIsLoading] = useState(false);

// Complex state
interface [Action]State {
  step: number;
  formData: Partial<[Action]FormData>;
  errors: Record<string, string>;
}

const [action]Reducer = (
  state: [Action]State, 
  action: [Action]Action
): [Action]State => {
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
For global state use [FRONTEND_FRAMEWORK] Context:

```[FRONTEND_LANGUAGE]
interface AuthContextType {
  user: User | null;
  login: (email: string, password: string) => Promise<void>;
  logout: () => void;
  isAuthenticated: boolean;
}

const AuthContext = createContext<AuthContextType | null>(null);

export const AuthProvider: [COMPONENT_TYPE]<{ children: [CHILDREN_TYPE] }> = ({ children }) => {
  const [user, setUser] = useState<User | null>(null);

  const login = async (email: string, password: string) => {
    // Implement login
  };

  const logout = () => {
    setUser(null);
    localStorage.removeItem('[AUTH_TOKEN_KEY]');
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

```[FRONTEND_LANGUAGE]
interface [Action]FormProps {
  onSubmit: (data: [Action]FormData) => void;
  isLoading?: boolean;
}

export const [Action]Form: [COMPONENT_TYPE]<[Action]FormProps> = ({ 
  onSubmit, 
  isLoading = false 
}) => {
  const [formData, setFormData] = useState<[Action]FormData>({
    [property1]: '',
    [property2]: '',
    [property3]: '',
    [dateProperty]: '',
    [selectProperty]: '[DEFAULT_VALUE]'
  });

  const [errors, setErrors] = useState<Record<string, string>>({});

  const handleChange = (field: keyof [Action]FormData) => (
    e: [EVENT_TYPE]
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

    if (!formData.[property1].trim()) {
      newErrors.[property1] = '[Property1] is required';
    }

    if (!formData.[property3].includes('@')) {
      newErrors.[property3] = 'Invalid email';
    }

    setErrors(newErrors);
    return Object.keys(newErrors).length === 0;
  };

  const handleSubmit = (e: [FORM_EVENT_TYPE]) => {
    e.preventDefault();
    if (validateForm()) {
      onSubmit(formData);
    }
  };

  return (
    <form onSubmit={handleSubmit} className="[action]-form">
      <input
        type="text"
        placeholder="[Property1 Label]"
        value={formData.[property1]}
        onChange={handleChange('[property1]')}
        className={errors.[property1] ? 'error' : ''}
      />
      {errors.[property1] && <span className="error-message">{errors.[property1]}</span>}
      
      <button type="submit" disabled={isLoading}>
        {isLoading ? '[Loading Action Label]...' : '[Action Label]'}
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

```[FRONTEND_LANGUAGE]
import styles from './[Entity]Card.module.css';

export const [Entity]Card: [COMPONENT_TYPE]<[Entity]CardProps> = ({ [entity] }) => {
  return (
    <div className={styles.[entity]Card}>
      <h3 className={styles.[entity]Title}>{[entity].[nameProperty]}</h3>
      <p className={styles.[entity]Date}>
        {[entity].[dateProperty].toLocaleDateString('[LOCALE]')}
      </p>
    </div>
  );
};
```

## Testing

### Component Testing
Use [TEST_FRAMEWORK] with [TESTING_LIBRARY]:

```[FRONTEND_LANGUAGE]
import { render, screen, fireEvent } from '@testing-library/[FRONTEND_FRAMEWORK]';
import { describe, it, expect, vi } from '[TEST_FRAMEWORK]';
import { [Entity]Card } from './[Entity]Card';

describe('[Entity]Card', () => {
  const mock[Entity] = {
    id: 1,
    [nameProperty]: '[Test Entity Name]',
    [dateProperty]: new Date('[TEST_DATE]'),
    // ... other fields
  };

  it('should show the [entity] name', () => {
    render(<[Entity]Card [entity]={mock[Entity]} [onAction]={() => {}} />);
    
    expect(screen.getByText('[Test Entity Name]')).toBeInTheDocument();
  });

  it('should call [onAction] when button is clicked', () => {
    const mock[OnAction] = vi.fn();
    
    render(<[Entity]Card [entity]={mock[Entity]} [onAction]={mock[OnAction]} />);
    
    fireEvent.click(screen.getByText('[Action Label]'));
    
    expect(mock[OnAction]).toHaveBeenCalledWith(1);
  });
});
```

## Best Practices

### Performance
- Use `[COMPONENT_TYPE].memo` for expensive components
- Implement lazy loading for routes
- Optimize images and assets
- Use `useMemo` and `useCallback` when appropriate

### Accessibility
- Use semantic HTML
- Implement ARIA attributes
- Handle focus and keyboard navigation
- Test with screen reader

### Error Handling
- Implement Error Boundaries
- Handle loading and error states
- Show user-friendly messages

### Anti-Patterns
- Don't abuse useEffect
- Avoid excessive prop drilling
- Don't mutate state directly
- Don't mix business logic in UI components
- Don't use `any` without valid reasons
- Avoid inline functions in JSX for performance
- Don't forget dependency array in useEffect
- Don't use index as key in dynamic lists

## Advanced State Management

### [STATE_LIBRARY] for Global State (alternative to Context)
```[FRONTEND_LANGUAGE]
import { create } from '[STATE_LIBRARY]';

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
```[FRONTEND_LANGUAGE]
// [ENV_TYPES_FILE]
interface ImportMetaEnv {
  readonly [API_BASE_URL_VAR]: string;
  readonly [APP_TITLE_VAR]: string;
}

interface ImportMeta {
  readonly env: ImportMetaEnv;
}
```

### Proxy for API
```[FRONTEND_LANGUAGE]
// [BUILD_CONFIG_FILE]
export default defineConfig({
  plugins: [[FRONTEND_PLUGIN]()],
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