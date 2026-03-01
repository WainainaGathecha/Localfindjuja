# Developer Onboarding Guide for Jisaidie

Welcome! This guide will help you understand the Jisaidie codebase from scratch. We'll walk through it strategically so everything makes sense.

---

## The Big Picture (Read This First)

### What is Jisaidie?
A **mobile-first web app for finding local businesses** in Juja. Think of it as:
- **Google Maps** but only for your area
- **Curated** (not all random listings)
- **Simple** (no ads, no clutter)

### How Users Experience It
1. User opens app → sees **Home page** with categories
2. User clicks category or searches → sees **Listings page** with filtered results
3. User clicks a business → sees **Detail page** with phone/hours/address
4. User calls or WhatsApps the business directly

That's it. One simple flow.

---

## Quick Setup (5 Minutes)

Before reading code, get the app running:

```bash
# Navigate to project
cd c:\Users\krypton\jisaidie.app

# Install dependencies (one time)
npm install

# Start development server
npm run dev

# Open browser to http://localhost:3000
```

The app should load. Click around. **This helps you understand what the code is doing.**

---

## How the Code Works (Architecture Overview)

### The Data Flow

```
User Types in Search
        ↓
    SearchBar Component
        ↓
    useSearch Hook Updates State
        ↓
    searchHelpers.js Filters Data
        ↓
    Results Passed to BusinessGrid
        ↓
    BusinessCard Components Render
```

### The Navigation Flow

```
index.html (entry point)
        ↓
main.jsx (ReactDOM renders)
        ↓
App.jsx (React Router)
        ↓
3 Pages: HomePage, ListingsPage, BusinessDetailPage
        ↓
Each page imports components & data
```

This is fundamental. Keep this flow in your head.

---

## Read Files in This Order

### Level 1: Understand the Entry Point (10 mins)

**Start here. Read these three files in order:**

1. **[index.html](./index.html)** (2 minutes)
   - The HTML template
   - One `<div id="root"></div>` - React renders inside this
   - One script tag pointing to `src/main.jsx`
   - That's it. HTML is minimal in modern React apps.

2. **[src/main.jsx](./src/main.jsx)** (2 minutes)
   - The first JavaScript file that runs
   - Creates React root element
   - Renders the `<App />` component
   - No logic here. Just entry point.

3. **[src/App.jsx](./src/App.jsx)** (3 minutes)
   - Sets up React Router
   - Defines 3 routes: `/`, `/listings`, `/business/:id`
   - Wraps all pages with `<Navbar />`
   - This is the "shell" of the app

**After reading these 3 files, you understand:**
- How the app starts
- How routing works
- Where pages are defined

---

### Level 2: Understand a User Journey (20 mins)

**Pick one user action and trace it. This teaches you component composition.**

#### Action: User Clicks "Beauty" Category on Home Page

**Files to read in order:**

1. **[src/pages/HomePage.jsx](./src/pages/HomePage.jsx)** (5 minutes)
   - `import categories from '../data/categories.json'`
   - Loops through categories and renders buttons
   - When button clicked: `navigate(\`/listings?category=${categoryId}\`)`
   - You see how data flows into components

2. **[src/data/categories.json](./src/data/categories.json)** (1 minute)
   - Just JSON data structure
   - Each category has: id, name, icon, color
   - This is the "source of truth" for categories

3. **[src/pages/ListingsPage.jsx](./src/pages/ListingsPage.jsx)** (8 minutes)
   - Uses `useSearch` hook (we'll learn that next)
   - Renders `<CategoryFilter>`, `<NeighborhoodFilter>`, `<BusinessGrid>`
   - This page is the "conductor" - coordinates all the filtering
   - Read the imports first to understand what it needs
   - Then read the JSX structure

4. **[src/components/CategoryFilter.jsx](./src/components/CategoryFilter.jsx)** (3 minutes)
   - Takes `selectedCategory` and `onSelectCategory` as props
   - Renders horizontal scrollable buttons
   - One button per category
   - Notice: component is a "dumb" UI component (no logic)

5. **[src/components/BusinessGrid.jsx](./src/components/BusinessGrid.jsx)** (1 minute)
   - Takes `businesses` array as prop
   - Loops through and renders `<BusinessCard />`
   - Handles "no results" state
   - Very simple component

6. **[src/components/BusinessCard.jsx](./src/components/BusinessCard.jsx)** (3 minutes)
   - Shows one business as a card
   - Displays image, name, rating, hours, phone
   - Links to detail page
   - Uses `getOpeningStatus()` helper

**What you learned:**
- Data flows DOWN through props
- Components are small and focused
- Pages are "conductors" that orchestrate components

---

### Level 3: Understand the Search Logic (15 mins)

**This is where the "magic" happens. How does filtering work?**

1. **[src/hooks/useSearch.js](./src/hooks/useSearch.js)** (8 minutes)
   - This is a custom React hook
   - Manages all filter state: search, category, neighborhood
   - Uses `useMemo` to recalculate filtered results only when inputs change (efficient!)
   - Returns: `filtered`, `searchQuery`, `setSearchQuery`, etc.
   - Think of this as the "brain" of filtering

2. **[src/utils/searchHelpers.js](./src/utils/searchHelpers.js)** (7 minutes)
   - Pure functions (no side effects)
   - `fuzzySearch()` - finds partial matches
   - `filterByCategory()` - narrows by type
   - `filterByNeighborhood()` - narrows by location
   - `getOpeningStatus()` - checks if open today
   - These are utilities. Use them anywhere.

**What you learned:**
- Custom hooks encapsulate logic
- Utility functions are reusable helpers
- Separation of concerns: hooks have state, utils have functions

---

### Level 4: Understand Business Data (10 mins)

1. **[src/data/businesses.json](./src/data/businesses.json)** (5 minutes)
   - Array of business objects
   - Each has: id, name, category, address, phone, hours, services, image, rating
   - This is the entire database for MVP
   - No API. No backend yet. Just JSON.

2. **How is it used?**
   - Imported in `ListingsPage.jsx`: `import businessesData from '../data/businesses.json'`
   - Passed to `useSearch()` hook
   - Hook filters it and returns results
   - Results rendered by `BusinessGrid`

**What you learned:**
- Data is a simple JSON file (easy to test, no DB setup)
- It flows from file → hook → component

---

### Level 5: Understand the Detail Page (10 mins)

**User clicked a business card. What happens?**

1. **[src/pages/BusinessDetailPage.jsx](./src/pages/BusinessDetailPage.jsx)** (10 minutes)
   - Uses `useParams()` to get business ID from URL
   - Finds business in `businessesData` array
   - Renders all business info
   - Action buttons: Call, WhatsApp, Share
   - Notice the structure: image → info → hours → actions
   - Read each section to understand what data it displays

**What you learned:**
- URL params contain data (no network request needed)
- Detail page is read-only (no forms yet)
- UI mirrors the data structure

---

### Level 6: Understand Imports & Dependencies (5 mins)

**Why does each file import what it imports?**

Open any file and look at imports. Example from `ListingsPage.jsx`:

```javascript
import { useSearchParams, useNavigate } from 'react-router-dom';  // routing
import { X } from 'lucide-react';  // icon
import { SearchBar } from '../components/SearchBar';  // UI component
import { useSearch } from '../hooks/useSearch';  // custom hook
import businessesData from '../data/businesses.json';  // data
```

Each import has a reason:
- **react-router-dom** - Navigation
- **lucide-react** - Icons (visual)
- Relative imports - Local components
- JSON - Data

**Pattern:** 
- External libraries at top
- Local imports below
- Keep them organized

---

## Now You Understand the Flow

Congratulations. You can now trace any user action through the code:

```
HomePage → User clicks category
           ↓
           Link to /listings?category=beauty
           ↓
ListingsPage loads
           ↓
useSearch hook filters businesses (uses searchHelpers.js)
           ↓
BusinessGrid loops through results
           ↓
BusinessCard rendered for each (click → detail page)
           ↓
BusinessDetailPage shows full info
           ↓
User clicks Call → Opens phone dialer
```

---

## Common Tasks & Where to Make Changes

### Task: Add a New Business

**File:** `src/data/businesses.json`

**What to do:**
1. Open the file
2. Add new object to array with same structure:
   ```json
   {
     "id": 13,
     "name": "Your Business",
     "category": "Beauty",
     "neighborhood": "Downtown Juja",
     ...
   }
   ```
3. Save file
4. Refresh browser (app auto-reloads)

**Why it works:** Components import this JSON. When you edit it, components automatically re-render with new data.

---

### Task: Add a New Category

**File:** `src/data/categories.json`

**What to do:**
1. Add new object:
   ```json
   {
     "id": "sports",
     "name": "Sports",
     "icon": "Trophy",
     "color": "blue"
   }
   ```
2. Save
3. The category automatically appears in CategoryFilter

**Why it works:** CategoryFilter imports and loops through this JSON.

---

### Task: Change the Home Page Layout

**File:** `src/pages/HomePage.jsx`

**What to change:**
- Colors? Search for `bg-blue-600` and change to new color
- Text? Search for the text and edit
- Categories arrangement? Change the grid: `grid-cols-2 sm:grid-cols-3`
- Add new section? Add new `<section>` element with JSX

**Hint:** Use browser DevTools (F12) to inspect elements and find class names.

---

### Task: Change Styling (Colors, Spacing, Fonts)

**File:** `src/index.css` or components (Tailwind classes)

**How Tailwind works:**
- Classes like `bg-blue-600`, `p-4`, `text-lg` = styles
- Used inline in JSX: `className="bg-blue-600 p-4 text-lg"`
- No CSS files needed

**To change colors globally:** `tailwind.config.js`

Example: Change brand color
```javascript
// tailwind.config.js
theme: {
  extend: {
    colors: {
      brand: '#FF6B6B',  // Change this
    },
  },
},
```

Then use: `className="bg-brand-600"`

---

### Task: Add a New Route/Page

**Files:**
1. Create new page: `src/pages/NewPage.jsx`
2. Edit `src/App.jsx`:
   ```javascript
   import { NewPage } from './pages/NewPage';

   // Inside <Routes>:
   <Route path="/newpage" element={<NewPage />} />
   ```
3. Navigate to it: `useNavigate('/newpage')`

---

### Task: Debug Why Something Isn't Working

**Strategy:**

1. **Open browser DevTools** (F12)
2. **Check Console tab** - any red error messages?
   - Error message tells you which file + line number
   - Click the link → goes to exact line
3. **Check Network tab** - do you see 404 errors?
   - Missing imports? Check file paths
4. **Use React DevTools extension**
   - Inspect component props/state
   - See if data is reaching the component

**Common issues:**
- Import path typo → change `./data` to `../data`
- Missing JSON field → check businesses.json structure
- Styling not showing → remember Tailwind needs the class name exactly right

---

## File Summary Reference

| File | Purpose | Edit When |
|------|---------|-----------|
| `index.html` | HTML template | Never (just one root div) |
| `src/main.jsx` | Entry point | Never (React setup) |
| `src/App.jsx` | Router setup | Adding new pages/routes |
| `src/pages/*` | Full page views | Changing page layout/flow |
| `src/components/*` | Reusable UI pieces | Changing buttons/cards look |
| `src/hooks/useSearch.js` | Filter logic | Changing how search works |
| `src/utils/searchHelpers.js` | Utility functions | Adding new helpers |
| `src/data/*.json` | Business/category data | Adding new data |
| `src/index.css` | Global styles | Changing fonts/global colors |
| `tailwind.config.js` | Tailwind config | Extending colors/spacing |
| `vite.config.js` | Build config | Never (unless error) |
| `package.json` | Dependencies | Adding new npm packages |

---

## How to Test Your Changes

### Testing Locally

```bash
# Dev server (hot reload)
npm run dev

# Browser opens to http://localhost:3000
# Make change → save file → browser auto-updates
```

### What to Test

1. **After adding business to JSON:**
   - Can you search for it?
   - Does it appear in correct category?
   - Can you view its detail page?

2. **After changing component:**
   - Does it render without errors?
   - Do buttons work?
   - Is mobile view responsive? (F12 → toggle device toolbar)

3. **After changing styling:**
   - Check on mobile (F12 → device toolbar)
   - Test at small screen sizes
   - Verify buttons are clickable (44px minimum)

### Mobile Testing

Press F12 in browser → click device toolbar icon (top left) → select iPhone/Android

Test on actual screen sizes:
- iPhone SE (375px)
- iPhone 12 (390px)
- Galaxy S20 (360px)
- iPad (800px+)

---

## Installing New Dependencies

If you need a new library:

```bash
npm install package-name

# Then import it:
import { something } from 'package-name';
```

**Common packages already installed:**
- `react` - UI library
- `react-router-dom` - Routing
- `tailwindcss` - Styling
- `lucide-react` - Icons

**Don't install if already available!**

---

## Before You Commit Code

**Checklist:**

- [ ] Code runs without errors (`npm run dev`)
- [ ] No console errors (F12 → Console)
- [ ] Mobile responsive (F12 → toggle device)
- [ ] All buttons clickable (test on mobile)
- [ ] Import paths correct (no red squiggles)
- [ ] Files saved

**To build for production:**
```bash
npm run build

# Creates optimized version in /dist folder
```

---

## Learning Path Recommended

**If you're new to React:**

1. Read this guide (you're doing it!)
2. Run the app locally
3. Make a small change (edit homepage text)
4. Add a new business to JSON
5. Change a color in a component
6. Try adding a new route
7. Break something intentionally, debug it, fix it

**This teaches you:**
- How the codebase is organized
- How to make changes safely
- How to debug when needed
- Confidence to explore

---

## Quick Answers

**Q: Where does data come from?**
A: `src/data/businesses.json` (local JSON file, no API yet)

**Q: How does search work?**
A: `useSearch` hook uses `fuzzySearch()` to match text against business names/descriptions

**Q: How do routes work?**
A: `react-router-dom` - URL path determines which page renders

**Q: What is Tailwind CSS?**
A: Utility classes for styling - `className="bg-blue-600 p-4"` instead of writing CSS

**Q: Where are the styles?**
A: Inline in components using Tailwind classes (e.g., `className="bg-white rounded-lg"`)

**Q: How do I change colors?**
A: Either edit `tailwind.config.js` or change the class name in the component

**Q: Can I use regular CSS?**
A: Yes, but Tailwind is recommended for consistency. Write in `src/index.css`

**Q: Is there a backend?**
A: Not yet. Phase 2 will add Firebase. For now, just JSON file.

**Q: How do hooks work?**
A: Functions that let components use state (`useState`), side effects (`useEffect`), etc. Magic of React.

---

## Next Steps

Once you're comfortable with this guide:

1. **Read [README_APPROACH.md](./README_APPROACH.md)** - Future features & how to add them

2. **Make your first feature change:**
   - Edit a business
   - Add a category
   - Change a color

3. **Read component code deep:**
   - Understand props: data flowing down
   - Understand state: component memory
   - Understand events: user interactions

4. **Study the React docs:** [react.dev](https://react.dev)

5. **Deploy to Vercel** when ready

---

## You're Ready!

You now have a mental model of the entire codebase. You understand:

- Where it starts (`index.html` → `main.jsx` → `App.jsx`)
- How data flows (JSON → hook → components)
- How routing works (URL → page render)
- Where to make changes (data, components, or config)
- How to test locally

**Go build something.** The code is here for you to learn from. Make mistakes, break things, fix them, and become confident.

Welcome to the team!

---

**Questions? Confused by something? Errors?**

1. Check browser console (F12) for error messages
2. Read the error carefully - it tells you what's wrong
3. Google the error message
4. Ask a teammate or open an issue

You got this!
