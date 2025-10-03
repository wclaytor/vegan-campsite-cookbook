# 🍃 Vegan Campsite Cookbook

A modern, single-file Alpine.js application for browsing plant-based recipes perfect for outdoor adventures.

[GitHub Pages Live Preview](https://wclaytor.github.io/vegan-campsite-cookbook/index.html)

## 🚀 Features

- **Recipe Index**: Automatically loads and displays recipes from `/recipes/*.md` files
- **Smart Search**: Search across recipe names, descriptions, ingredients, and cuisines
- **Recipe Filters**: Filter by meal type (Appetizers, Main Dishes) and dietary preferences
- **Responsive Design**: Mobile-friendly interface perfect for camping
- **Dark Mode**: Toggle between light and dark themes
- **Offline Ready**: Works offline after initial load
- **Recipe Details**: Full-screen modal with ingredients, steps, and cooking notes

## 🏕️ Perfect for Camping

- Single HTML file - easy to bookmark and access
- Mobile-optimized for use on phones and tablets
- Quick recipe lookup for campsite cooking
- Vegan/plant-based focus with nutritious outdoor meal options
- Works without internet after first visit

## 📱 Usage

1. **Browse Recipes**: View all available recipes in the card grid
2. **Search**: Use the search bar to find recipes by name, ingredient, or cuisine
3. **Filter**: Click filter buttons to show specific types of recipes
4. **View Details**: Click any recipe card to see full ingredients and instructions
5. **Dark Mode**: Toggle the theme using the moon/sun icon

## 🍽️ Adding Recipes

To add new recipes to the cookbook:

1. **Create the recipe file**: Add a new Markdown file in the `/recipes/` folder (e.g., `my-new-recipe.md`)
2. **Update the manifest**: Add the filename to the `recipes.json` file in the root directory
3. **Use the standard format**: Follow the template below for consistent parsing

### Recipe Template

```markdown
# Recipe Name

## Description
Brief description of the recipe

## Cuisine
- Cuisine type (e.g., Italian, Mexican, etc.)

## Course
- Course type (e.g., Appetizer, Main, Side)

## Preparation Time
- Time needed (e.g., 30 minutes)

## Servings
- Number of servings (e.g., 4 servings)

## Ingredients
- Ingredient 1
- Ingredient 2
- etc.

## Steps  
1. Step 1
2. Step 2
3. etc.

## Serving Guidelines
How to serve the dish

## Notes  
Additional cooking tips or variations

## Source / Inspiration
Where the recipe came from
```

### Example: Adding "Veggie Tacos"

1. Create `/recipes/veggie-tacos.md` with the recipe content
2. Add `"veggie-tacos.md"` to the `recipes.json` array:
```json
[
    "hummus-moosewood.md",
    "chili-fire-roasted.md", 
    "peanut-sesame-noodles.md",
    "veggie-tacos.md"
]
```
3. The app will automatically load the new recipe on next visit

## 🔧 Technical Details

- **Built with**: Alpine.js 3.x + Tailwind CSS 3.x + Bootstrap Icons
- **Architecture**: Single-file standalone HTML application
- **Recipe Loading**: Dynamic manifest-based system (`recipes.json`)
- **Deployment**: GitHub Pages compatible
- **Storage**: Uses localStorage for preferences
- **Parsing**: Custom Markdown parser for recipe files
- **Fallback**: Graceful degradation if manifest fails

## 🌐 GitHub Pages Deployment

This application is designed to work perfectly with GitHub Pages:

1. Enable GitHub Pages in repository settings
2. Set source to main branch root directory
3. The app will be available at `https://yourusername.github.io/repository-name/`

## 🖥️ Local Development

1. Clone the repository
2. Start a local web server (e.g., `python -m http.server 8000`)
3. Open `http://localhost:8000` in your browser

## 📝 Recipe Format

The application automatically parses Markdown files with structured sections. All sections are optional, but using the standard format ensures the best display in the application.

---

*Built with 💚 for plant-based outdoor cooking*
