# AGENTS.md

## Project Context

The **Vegan Campsite Cookbook** is a standalone Alpine.js application for outdoor cooking enthusiasts. This single-file HTML app works offline, loads recipes from markdown files, and provides a mobile-first experience for camping scenarios.

**Key Architecture**: Single HTML file with Alpine.js + Tailwind CSS + Bootstrap Icons via CDN. No build process required.

## Role-Based Development

This project uses a **5-role methodology**. Adopt the appropriate role perspective based on the issue type:

### 🏢 **Product-Owner Role** 
**When to adopt**: Requirements, features, user stories, business decisions
- Define user stories with camping/outdoor context
- Focus on offline-first value proposition  
- Prioritize mobile usability for outdoor conditions
- Consider recipe discovery and meal planning scenarios

### 🏗️ **Architect Role**
**When to adopt**: System design, technology choices, performance architecture
- Maintain standalone single-file architecture
- Optimize for mobile/3G performance (< 2sec load, < 100ms search)
- Ensure Alpine.js reactive patterns and CDN fallbacks
- Design for offline-first functionality

### 🎨 **Designer Role** 
**When to adopt**: UI/UX, styling, accessibility, responsive design
- Mobile-first responsive design (camping use case)
- WCAG 2.1 AA accessibility compliance
- Dark mode support with smooth transitions
- Touch targets ≥ 44px for outdoor/gloved use
- High contrast for sunlight readability

### 💻 **Developer Role**
**When to adopt**: Code implementation, Alpine.js patterns, technical features
- Use Alpine.js reactive patterns (x-data, computed properties, x-show/x-if)
- Follow project naming conventions (camelCase JS, kebab-case CSS)
- Implement proper error handling for network failures
- Use inline styles for modal dimensions (Tailwind classes fail here)

### 🧪 **QA-Engineer Role**
**When to adopt**: Testing, validation, quality assurance
- Test offline functionality (file:// protocol)
- Validate cross-browser compatibility (Chrome, Firefox, Safari, Edge)
- Ensure mobile touch interactions work properly
- Verify accessibility with keyboard navigation and screen readers

## Development Environment

### File Structure
```
/
├── index.html              # Main application (single file)
├── recipes.json           # Recipe manifest
├── recipes/               # Recipe markdown files
├── .github/
│   ├── copilot-instructions.md
│   └── roles/             # Role definition files
└── docs/                  # Project documentation
```

### Core Patterns

#### Alpine.js App Structure
```javascript
function cookbookApp() {
    return {
        // State
        recipes: [],
        searchTerm: '',
        selectedFilter: 'all',
        
        // Computed Properties (reactive)
        get filteredRecipes() {
            return this.recipes.filter(/* filtering logic */);
        },
        
        // Methods
        async loadRecipes() { /* async loading */ }
    }
}
```

#### Modal Pattern (Critical)
```html
<!-- ALWAYS use inline styles for modal dimensions -->
<div class="fixed inset-0" style="height: 80vh">
    <div class="overflow-y-scroll" style="height: calc(80vh - 120px)">
        <!-- Scrollable content -->
    </div>
</div>
```

### Recipe Content Format
```markdown
# Recipe Name
## Description
## Cuisine  
## Category
## Prep Time
## Cook Time
## Ingredients
## Instructions
## Camping Notes
```

## Testing Instructions

### Manual Testing Checklist
- [ ] **Offline Test**: Open via `file://` protocol (true offline)
- [ ] **Mobile Test**: Test on actual mobile devices with touch
- [ ] **Accessibility Test**: Navigate entirely with keyboard
- [ ] **Performance Test**: Load with 100+ recipes, measure search speed
- [ ] **Cross-Browser**: Test Chrome, Firefox, Safari, Edge (last 2 versions)

### Key Test Scenarios
- Recipe loading and parsing from markdown files
- Real-time search across recipe names, ingredients, descriptions
- Filter functionality (All, Main Dishes, Appetizers, Quick Recipes)
- Modal recipe detail view with proper scrolling
- Dark mode toggle and persistence
- Responsive design breakpoints (mobile, tablet, desktop)

### Performance Validation
```bash
# Test load times (should be < 2 seconds on 3G)
# Test search responsiveness (should be < 100ms)
# Test memory usage with large recipe datasets
# Test offline functionality after initial CDN loads
```

## Issue Assignment Guidelines

### For Feature Requests
1. **Adopt Product-Owner**: Define user story and acceptance criteria
2. **Adopt Architect**: Design technical approach within constraints  
3. **Adopt Designer**: Create mobile-first UI specifications
4. **Adopt Developer**: Implement using Alpine.js patterns
5. **Adopt QA-Engineer**: Define testing strategy and validation

### For Bug Reports
1. **Adopt QA-Engineer**: Reproduce and analyze the issue
2. **Adopt Developer**: Implement fix following established patterns
3. **Adopt Designer**: Ensure UI consistency (if UI-related)
4. **Adopt QA-Engineer**: Validate fix across browsers and devices

### For Performance Issues
1. **Adopt Architect**: Analyze bottlenecks and optimization strategy
2. **Adopt Developer**: Implement Alpine.js performance patterns
3. **Adopt QA-Engineer**: Benchmark and validate improvements

### For Accessibility Issues  
1. **Adopt Designer**: Review WCAG 2.1 AA compliance requirements
2. **Adopt Developer**: Implement accessibility fixes (ARIA, semantic HTML)
3. **Adopt QA-Engineer**: Test with assistive technologies

## Pull Request Instructions

### Title Format
`[Role]: Brief description of change`

Examples:
- `[Developer]: Add recipe rating system with localStorage persistence`
- `[Designer]: Improve mobile touch targets for outdoor use`
- `[Architect]: Optimize search performance for large recipe datasets`

### Required Validation
- [ ] **Functionality**: Feature works as specified in acceptance criteria
- [ ] **Performance**: Maintains < 2sec load, < 100ms search response
- [ ] **Accessibility**: Passes keyboard navigation and screen reader tests
- [ ] **Mobile**: Touch interactions work properly on mobile devices
- [ ] **Offline**: App functions correctly via file:// protocol
- [ ] **Cross-Browser**: Consistent behavior across supported browsers

### Code Quality Gates
- Use `x-cloak` to prevent flash of unstyled content
- Prefer computed properties (getters) for derived state
- Use inline styles for modal dimensions (Tailwind classes don't work)
- Implement proper error handling for async operations
- Follow project naming conventions consistently

## Recipe Management

### Adding New Recipes
1. Create markdown file in `/recipes/` folder
2. Add filename to `recipes.json` manifest
3. Follow established markdown template format
4. Focus on camping-friendly ingredients and instructions
5. Include practical camping notes and equipment requirements

### Recipe Quality Standards
- Instructions suitable for outdoor cooking (campfire, portable stove)
- Ingredients available at typical camping supply stores
- Preparation methods that work with limited equipment
- Nutritional focus on energy-rich plant-based options
- Clear timing estimates accounting for outdoor conditions

---

**This project demonstrates modern Alpine.js capabilities while serving the practical needs of outdoor cooking enthusiasts. Use the role-based approach to ensure comprehensive, high-quality solutions that work reliably anywhere, anytime.**