# Contributing to Latent

Welcome to Latent! We're excited that you want to contribute to making consciousness meet creation. This guide will help you get started with contributing to our codebase.

## 🌟 Our Philosophy

Before diving into code, please read our design philosophy in `latent-ui/CLAUDE.md`. Key principles:

- **Less interface, more story** - Every feature should serve the narrative
- **Minimalism as poetry** - Remove until it breaks, then add back one thing
- **Consciousness-like interactions** - Blur transitions, thoughts replacing thoughts
- **Time should disappear** - The ultimate test of any feature

## 🚀 Getting Started

### Prerequisites

- Node.js 18+ and npm
- Go 1.21+
- Docker and Docker Compose
- Git
- A code editor (VS Code recommended)

### Initial Setup

1. **Clone the repositories**
   ```bash
   # Create a workspace
   mkdir latent-workspace && cd latent-workspace
   
   # Clone all repositories
   git clone [latent-ui-repo]
   git clone [latent-platform-api-v2-repo]
   git clone [latent-video-ai-v2-repo]
   git clone [oauth-service-repo]
   ```

2. **Set up environment files**
   ```bash
   # Frontend
   cd latent-ui
   cp .env.example .env.local
   
   # Update with your local settings
   # VITE_OAUTH_API_URL=http://localhost:3000
   # VITE_PLATFORM_API_URL=http://localhost:8085
   ```

3. **Install dependencies**
   ```bash
   # Frontend (latent-ui)
   cd latent-ui && npm install
   
   # OAuth Service
   cd ../oauth-service && npm install
   
   # Go services will auto-install on first run
   ```

4. **Start all services**
   ```bash
   # Option 1: Start each service individually
   # Terminal 1: OAuth Service
   cd oauth-service && npm run dev
   
   # Terminal 2: Platform API
   cd latent-platform-api-v2 && go run .
   
   # Terminal 3: Video AI Service  
   cd latent-video-ai-v2 && go run .
   
   # Terminal 4: Frontend
   cd latent-ui && npm run dev
   
   # Option 2: Use docker-compose (if available)
   docker-compose up
   ```

## 📝 Development Workflow

### 1. Create a Feature Branch

```bash
# Branch naming convention
git checkout -b feature/add-personality-insights
git checkout -b fix/video-generation-timeout
git checkout -b docs/update-api-examples
```

### 2. Make Your Changes

Follow these patterns based on what you're working on:

#### Frontend (React/UI)
```jsx
// ✅ Good: Props-only screen component
const PersonalityScreen = ({ data, onUpdate }) => {
  return <div>...</div>;
};

// ❌ Bad: Data fetching in screen component
const PersonalityScreen = () => {
  const { data } = useAPI(); // Don't do this in screens!
  return <div>...</div>;
};
```

#### Backend (Go APIs)
```go
// ✅ Good: Clear error handling
func (s *Service) ProcessURL(url string) (*Result, error) {
    if url == "" {
        return nil, errors.New("url cannot be empty")
    }
    // Process...
}

// ❌ Bad: Swallowing errors
func (s *Service) ProcessURL(url string) *Result {
    // Don't ignore errors!
}
```

### 3. Test Your Changes

```bash
# Frontend
npm run lint        # Check code style
npm run build       # Ensure it builds

# Backend (Go)
go test ./...       # Run tests
go fmt ./...        # Format code

# Manual testing
# 1. Test the happy path
# 2. Test error cases
# 3. Test edge cases
```

### 4. Commit Your Changes

We follow conventional commits:

```bash
# Format: <type>(<scope>): <subject>

# Examples:
git commit -m "feat(personality): add URL context extraction"
git commit -m "fix(auth): handle token expiration gracefully"
git commit -m "docs(api): update shot-builder examples"
git commit -m "style(ui): improve button hover states"
git commit -m "refactor(platform): simplify job queue logic"
git commit -m "test(video-ai): add scene enhancer tests"
git commit -m "chore(deps): update framer-motion to v11"
```

**Types:**
- `feat`: New feature
- `fix`: Bug fix
- `docs`: Documentation changes
- `style`: Code style changes (formatting, etc)
- `refactor`: Code refactoring
- `test`: Adding tests
- `chore`: Maintenance tasks

### 5. Push and Create PR

```bash
git push origin feature/your-feature-name
```

## 🏗️ Code Standards

### JavaScript/TypeScript (Frontend)

```javascript
// File naming
PersonalityProfile.jsx    // React components (PascalCase)
usePersonality.js         // Hooks (camelCase with 'use' prefix)
api-client.js            // Utilities (kebab-case)

// Component structure
import React from 'react';
import { motion } from 'framer-motion';
// ... other imports

const ComponentName = ({ prop1, prop2 }) => {
  // Hooks first
  const [state, setState] = useState();
  
  // Effects next
  useEffect(() => {
    // Effect logic
  }, []);
  
  // Handlers
  const handleClick = () => {
    // Handler logic
  };
  
  // Render
  return (
    <div>
      {/* JSX */}
    </div>
  );
};

export default ComponentName;
```

### Go (Backend Services)

```go
// File structure
package main

import (
    "fmt"
    "log"
    // Standard library first
    
    "github.com/gin-gonic/gin"
    // External packages next
    
    "github.com/latent/internal/service"
    // Internal packages last
)

// Clear function names and error handling
func ProcessRequest(ctx context.Context, req *Request) (*Response, error) {
    // Validate inputs
    if err := req.Validate(); err != nil {
        return nil, fmt.Errorf("validation failed: %w", err)
    }
    
    // Process
    result, err := doProcess(req)
    if err != nil {
        return nil, fmt.Errorf("processing failed: %w", err)
    }
    
    return result, nil
}
```

### CSS/Styling

```css
/* Use Tailwind utilities first */
<div className="flex items-center justify-center p-4">

/* Custom CSS only when necessary */
.consciousness-blur {
  filter: blur(10px);
  transition: filter 0.6s ease;
}

/* Follow the glass morphism pattern */
.glass-primary {
  background: rgba(255, 255, 255, 0.05);
  backdrop-filter: blur(24px);
  border: 1px solid rgba(255, 255, 255, 0.1);
}
```

## 🧪 Testing Guidelines

### Frontend Testing
```javascript
// Test file naming: ComponentName.test.jsx
describe('PersonalityProfile', () => {
  it('should display URL input fields', () => {
    // Test implementation
  });
  
  it('should handle multiple URLs', () => {
    // Test implementation
  });
});
```

### Backend Testing
```go
// Test file naming: service_test.go
func TestProcessURL(t *testing.T) {
    tests := []struct {
        name    string
        input   string
        want    *Result
        wantErr bool
    }{
        {
            name:  "valid URL",
            input: "https://example.com",
            want:  &Result{...},
        },
        {
            name:    "empty URL",
            input:   "",
            wantErr: true,
        },
    }
    
    for _, tt := range tests {
        t.Run(tt.name, func(t *testing.T) {
            got, err := ProcessURL(tt.input)
            // Assertions
        })
    }
}
```

## 📚 Documentation

### When to Document

1. **New Features**: Add to relevant README
2. **API Changes**: Update API.md files
3. **Breaking Changes**: Add to CHANGELOG
4. **Complex Logic**: Add inline comments

### Documentation Style

```markdown
# Clear, Actionable Headers

## Overview
Brief description of what this does.

## Usage
```javascript
// Code example showing how to use it
const result = await generatePersonality(urls);
```

## Parameters
- `urls` (string[]): Array of URLs to analyze
- `options` (object): Optional configuration

## Returns
Returns a Promise that resolves to...
```

## 🐛 Debugging Tips

### Frontend Debugging
```javascript
// Use console.log strategically
console.log('[PersonalityProfile] Processing URLs:', urls);

// Use React DevTools
// Install browser extension for component inspection

// Check Network tab for API calls
// Verify request/response payloads
```

### Backend Debugging
```go
// Use structured logging
log.Printf("[ProcessURL] Starting with URL: %s", url)

// Use debugger
// VS Code Go extension supports breakpoints

// Check service logs
// tail -f logs/platform-api.log
```

### Common Issues

1. **"Awakening your creative space..." stuck**
   - Check if OAuth service is running
   - Verify auth tokens in browser DevTools

2. **API Connection Errors**
   - Ensure all services are running
   - Check service ports match .env config
   - Verify CORS settings

3. **Style Not Applying**
   - Clear Vite cache: `rm -rf node_modules/.vite`
   - Check Tailwind classes are valid
   - Verify CSS import order

## 🚀 Submitting Pull Requests

### PR Checklist

- [ ] Code follows project style guide
- [ ] Tests pass (when applicable)
- [ ] Documentation updated
- [ ] Commit messages follow convention
- [ ] PR description explains the change
- [ ] Screenshots included (for UI changes)

### PR Template

```markdown
## Description
Brief description of what this PR does.

## Type of Change
- [ ] Bug fix
- [ ] New feature
- [ ] Documentation update
- [ ] Performance improvement
- [ ] Refactoring

## Testing
- [ ] Tested locally
- [ ] Added/updated tests
- [ ] Tested error cases

## Screenshots (if UI change)
[Add screenshots here]

## Related Issues
Fixes #123
```

## 🎨 Design Contributions

When contributing to the UI:

1. **Follow the Consciousness Theme**
   - Ethereal, flowing interactions
   - Generous white space
   - Typography-first approach

2. **Animation Guidelines**
   - Use blur transitions (10px)
   - Duration: 0.6s with ease [0.16, 1, 0.3, 1]
   - Replace content, don't accumulate

3. **Component Patterns**
   - Atoms: Pure, reusable UI elements
   - Molecules: Composed components
   - Organisms: Feature-complete sections
   - Pages: Route-level components

## 🤝 Code Review Process

### What Reviewers Look For

1. **Code Quality**
   - Follows established patterns
   - Clear naming and structure
   - Proper error handling

2. **User Experience**
   - Serves the core vision
   - Maintains minimalist approach
   - Smooth, consciousness-like interactions

3. **Performance**
   - No unnecessary re-renders
   - Efficient API calls
   - Optimized bundle size

### Review Etiquette

- Be constructive and specific
- Suggest improvements, don't just criticize
- Acknowledge good patterns
- Ask questions if unclear

## 📞 Getting Help

### Resources

- Read existing code for patterns
- Check API documentation in each service
- Review CLAUDE.md for design philosophy
- Look at merged PRs for examples

### Communication

- Create GitHub issues for bugs/features
- Use discussions for questions
- Tag maintainers for urgent issues
- Be patient and respectful

## 🎉 Recognition

We appreciate all contributions! Contributors will be:
- Added to CONTRIBUTORS.md
- Mentioned in release notes
- Part of building the future of creative AI

## 📜 License

By contributing, you agree that your contributions will be licensed under the same license as the project.

---

Thank you for contributing to Latent! Together, we're making consciousness meet creation. 🚀