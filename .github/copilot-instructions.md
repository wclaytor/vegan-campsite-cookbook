# GitHub Copilot Instructions - Vegan Campsite Cookbook

## 🎯 Project Context and Purpose

The **Vegan Campsite Cookbook** is a modern, single-file Alpine.js application designed for outdoor enthusiasts who want quick access to plant-based recipes perfect for camping adventures. This project demonstrates best practices for creating standalone HTML applications that work offline and can be easily shared.

### Core Technology Stack
- **Alpine.js 3.x**: Lightweight reactive framework for interactivity
- **Tailwind CSS**: Utility-first CSS framework via CDN
- **Bootstrap Icons**: Comprehensive icon library
- **Markdown**: Recipe content format for easy editing
- **JSON**: Recipe manifest for dynamic loading

### Project Architecture
This is a **standalone single-file application** that:
- Works completely offline after initial load
- Requires no build process or server
- Can be shared via email, USB, or any file transfer method
- Loads recipes dynamically from markdown files
- Provides responsive design for mobile camping use

## 🚀 Alpine.js Development Best Practices

When working on this project, always follow these Alpine.js patterns:

### 1. Core Template Structure
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Vegan Campsite Cookbook</title>
    
    <!-- Alpine.js for reactivity -->
    <script defer src="https://cdn.jsdelivr.net/npm/alpinejs@3.x.x/dist/cdn.min.js"></script>
    
    <!-- Tailwind CSS for styling -->
    <script src="https://cdn.tailwindcss.com"></script>
    
    <!-- Bootstrap Icons -->
    <link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/bootstrap-icons@1.11.0/font/bootstrap-icons.css">
    
    <style>
        [x-cloak] { display: none !important; }
    </style>
</head>
<body>
    <div x-data="cookbookApp()" x-init="init()" x-cloak class="min-h-screen">
        <!-- App content here -->
    </div>
    
    <script>
        function cookbookApp() {
            return {
                // State and methods
            }
        }
    </script>
</body>
</html>
```

### 2. State Management Pattern
```javascript
function cookbookApp() {
    return {
        // === STATE ===
        recipes: [],
        searchTerm: '',
        selectedFilter: 'all',
        darkMode: false,
        loading: false,
        
        // === COMPUTED PROPERTIES ===
        get filteredRecipes() {
            return this.recipes.filter(recipe => {
                const matchesSearch = recipe.name.toLowerCase()
                    .includes(this.searchTerm.toLowerCase()) ||
                    recipe.description.toLowerCase()
                    .includes(this.searchTerm.toLowerCase());
                const matchesFilter = this.selectedFilter === 'all' || 
                    recipe.category === this.selectedFilter;
                return matchesSearch && matchesFilter;
            });
        },
        
        // === METHODS ===
        async loadRecipes() {
            this.loading = true;
            // Load recipe logic
            this.loading = false;
        }
    }
}
```

### 3. Modal Pattern (CRITICAL - Use Inline Styles)
```html
<!-- Recipe detail modal -->
<div x-show="selectedRecipe" 
     @click="selectedRecipe = null"
     class="fixed inset-0 bg-black bg-opacity-50 flex items-center justify-center p-4 z-50">
    <div @click.stop 
         class="bg-white dark:bg-gray-800 rounded-lg shadow-xl w-full max-w-4xl" 
         style="height: 80vh">
        
        <!-- Fixed Header -->
        <div class="bg-white dark:bg-gray-800 p-6 border-b dark:border-gray-700 rounded-t-lg">
            <h3 class="text-xl font-bold" x-text="selectedRecipe?.name"></h3>
        </div>
        
        <!-- Scrollable Content -->
        <div class="bg-gray-50 dark:bg-gray-900 p-6 overflow-y-scroll" 
             style="height: calc(80vh - 120px)">
            <!-- Recipe content -->
        </div>
    </div>
</div>
```

### 4. Recipe Loading Pattern
```javascript
async loadRecipes() {
    try {
        // Load recipe manifest
        const response = await fetch('recipes.json');
        const recipeFiles = await response.json();
        
        // Load each recipe markdown file
        const recipePromises = recipeFiles.map(async (filename) => {
            const recipeResponse = await fetch(`recipes/${filename}`);
            const markdown = await recipeResponse.text();
            return this.parseRecipeMarkdown(markdown, filename);
        });
        
        this.recipes = await Promise.all(recipePromises);
    } catch (error) {
        console.error('Error loading recipes:', error);
    }
}
```

### 5. Dark Mode Implementation
```javascript
// Dark mode toggle
toggleDarkMode() {
    this.darkMode = !this.darkMode;
    localStorage.setItem('darkMode', this.darkMode);
    document.documentElement.classList.toggle('dark', this.darkMode);
}

// Initialize dark mode from localStorage
init() {
    const savedDarkMode = localStorage.getItem('darkMode');
    this.darkMode = savedDarkMode === 'true';
    document.documentElement.classList.toggle('dark', this.darkMode);
    this.loadRecipes();
}
```

## 🏗️ Role-Based Development Methodology

This project follows a structured **role-based development approach** where different aspects of development are handled by specialized roles. When working on features, adopt the appropriate role perspective to ensure comprehensive, high-quality solutions.

### The Five Core Roles

#### 1. **Product-Owner** - Strategy & Vision
**When to adopt**: Defining requirements, user stories, business value
- Focus on user needs and camping scenarios  
- Define acceptance criteria and success metrics
- Prioritize features based on outdoor cooking use cases
- [Role Definition](./roles/Product-Owner.md)

#### 2. **Architect** - Technical Blueprint  
**When to adopt**: System design, technology choices, performance strategy
- Design Alpine.js application architecture
- Make technology stack decisions (Alpine.js + Tailwind + Bootstrap Icons)
- Define performance requirements for mobile/offline use
- [Role Definition](./roles/Architect.md)

#### 3. **Designer** - User Experience
**When to adopt**: UI/UX design, accessibility, mobile optimization
- Create mobile-first responsive designs
- Ensure WCAG 2.1 AA accessibility compliance
- Design dark mode and theming systems
- [Role Definition](./roles/Designer.md)

#### 4. **Developer** - Implementation
**When to adopt**: Coding features, Alpine.js patterns, technical implementation
- Implement Alpine.js reactive patterns
- Write clean, performant JavaScript code
- Follow established coding standards and patterns
- [Role Definition](./roles/Developer.md)

#### 5. **QA-Engineer** - Quality & Validation
**When to adopt**: Testing, validation, cross-browser compatibility
- Test Alpine.js functionality across devices
- Validate accessibility and mobile responsiveness  
- Ensure offline functionality works correctly
- [Role Definition](./roles/QA-Engineer.md)

## 🔄 Multi-Role Collaboration Pattern

For complex requests, adopt multiple role perspectives:

### Example: "Add recipe rating feature"

**Product-Owner perspective:**
```markdown
**User Story**: As a camper, I want to rate recipes so I can remember which ones worked well for outdoor cooking.

**Acceptance Criteria**:
- Users can rate recipes 1-5 stars
- Ratings persist in localStorage
- Average ratings display on recipe cards
- Ratings are mobile-friendly for campsite use
```

**Architect perspective:**
```javascript
// Rating system architecture
ratingSystem: {
    storage: 'localStorage', // Offline-first approach
    scale: '1-5 stars',
    persistence: 'per-device', // No server required
    performance: 'Computed averages cached'
}
```

**Designer perspective:**
```html
<!-- Mobile-optimized star rating component -->
<div class="flex items-center space-x-1">
    <template x-for="star in 5" :key="star">
        <button @click="setRating(recipe.id, star)"
                :class="star <= (ratings[recipe.id] || 0) ? 
                    'text-yellow-400' : 'text-gray-300'"
                class="text-xl hover:text-yellow-300 transition-colors">
            <i class="bi bi-star-fill"></i>
        </button>
    </template>
</div>
```

**Developer implementation:**
```javascript
// Rating system methods
setRating(recipeId, rating) {
    this.ratings[recipeId] = rating;
    localStorage.setItem('recipeRatings', JSON.stringify(this.ratings));
},

get averageRating() {
    const ratings = Object.values(this.ratings);
    return ratings.length > 0 ? 
        ratings.reduce((a, b) => a + b, 0) / ratings.length : 0;
}
```

**QA-Engineer validation:**
```markdown
**Test Scenarios**:
- [ ] Star ratings work on touch devices
- [ ] Ratings persist after page reload
- [ ] Rating UI is accessible with keyboard navigation
- [ ] Performance remains smooth with 100+ rated recipes
```

## 📚 Role Collaboration Resources

Understanding how roles work together is crucial for effective development:

### **Core Collaboration Guide**
[Roles.md](./docs/guides/Roles.md) - Complete role collaboration methodology including:
- Role hierarchy and dependencies
- Collaboration patterns and workflows
- Communication standards and escalation paths
- Success metrics and best practices

### **GitHub Copilot Role Usage**
[Roles-Copilot.md](./docs/guides/Roles-Copilot.md) - Specific guidance for AI-assisted development:
- When to adopt each role perspective
- Role detection keywords and triggers
- Multi-role response patterns
- Practical usage examples

## 🎯 Project-Specific Guidelines

### Recipe Content Standards
```markdown
# Recipe Name

## Description
Brief description perfect for camping scenarios

## Cuisine
- Cuisine type (Italian, Mexican, etc.)

## Category
- Main Dish / Appetizer / Quick Recipe

## Prep Time
- X minutes

## Cook Time
- X minutes (consider camping equipment limitations)

## Ingredients
- Ingredient with camping-friendly measurements
- Focus on non-perishable and easy-to-pack items

## Instructions
1. Step-by-step instructions
2. Written for outdoor cooking conditions
3. Include campfire/portable stove considerations

## Camping Notes
- Equipment needed
- Storage and prep tips for outdoor cooking
- Nutritional benefits for active outdoor adventures
```

### Performance Requirements
- **Initial Load**: < 2 seconds on 3G connection
- **Recipe Search**: < 100ms response time
- **Offline Functionality**: Must work after initial CDN loads
- **Mobile Optimization**: Touch targets ≥ 44px, thumb-friendly navigation
- **Memory Usage**: Handle 100+ recipes without performance degradation

### Accessibility Standards  
- **WCAG 2.1 AA Compliance**: All interactive elements keyboard accessible
- **Screen Reader Support**: Proper ARIA labels and semantic HTML
- **Color Contrast**: 4.5:1 ratio for normal text, 3:1 for large text
- **Touch Accessibility**: Clear focus states and adequate touch targets

### Browser Compatibility
- **Modern Browsers**: Chrome, Firefox, Safari, Edge (last 2 versions)
- **Mobile Browsers**: iOS Safari, Chrome Mobile, Samsung Internet
- **Progressive Enhancement**: Graceful degradation for older browsers
- **Offline Support**: Service Worker not required (CDN caching sufficient)

## 🚀 Development Workflow

### Feature Development Process
1. **Product-Owner**: Define user story and acceptance criteria
2. **Architect**: Design technical approach using Alpine.js patterns
3. **Designer**: Create mobile-first UI specifications  
4. **Developer**: Implement using established patterns and conventions
5. **QA-Engineer**: Validate functionality, accessibility, and mobile experience

### Code Quality Standards
- **Consistent Naming**: Use camelCase for JavaScript, kebab-case for CSS classes
- **Comment Complex Logic**: Especially Alpine.js reactive patterns and computed properties
- **Error Handling**: Graceful fallbacks for network failures and missing data
- **Performance**: Debounce search inputs, optimize DOM operations
- **Testing**: Manual testing on mobile devices and various screen sizes

### Documentation Requirements
- **Code Comments**: Explain Alpine.js patterns and business logic
- **Recipe Format**: Follow established markdown template
- **Change Documentation**: Update relevant guides when adding new patterns
- **User Instructions**: Keep README.md updated with new features

## 💡 Best Practices Summary

### For Alpine.js Development:
- Always use `x-cloak` to prevent flash of unstyled content
- Prefer computed properties (getters) for derived state
- Use inline styles for modal dimensions (Tailwind classes don't work)
- Implement proper error handling for async operations
- Optimize for mobile-first responsive design

### For Camping Cookbook Features:
- Focus on offline-first functionality
- Design for outdoor lighting conditions (high contrast)
- Consider limited storage and preparation capabilities
- Emphasize nutritious, energy-rich plant-based options
- Include practical camping tips and equipment notes

### For Role-Based Development:
- Match the role perspective to the type of request
- Use multiple roles for comprehensive feature development  
- Document decisions using role-specific methodologies
- Collaborate across roles for complex architectural decisions
- Maintain role consistency and expertise throughout development

---

**This project demonstrates how modern Alpine.js applications can provide rich, interactive experiences while maintaining the simplicity and portability of a single HTML file. Use the role-based approach to ensure every aspect of development meets professional standards while serving the unique needs of outdoor cooking enthusiasts.**