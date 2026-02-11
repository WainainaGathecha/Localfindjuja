# LocalFindJuja - Discover Local Businesses in Juja

A web application for discovering local businesses and services in Juja. Built to solve the problem of finding reliable local services like salons, electronic shops, hardware stores, furniture shops, and more - all in one searchable directory.

## The Problem

Living near campus, I constantly struggled to find basic services:
- Where can I get my ears pierced?
- Which hardware stores are nearby?
- Where's a good furniture shop?
- Which salons are open on weekends?

Google Maps is cluttered and generic. Word-of-mouth is unreliable. I needed a curated, local-focused directory for Juja specifically that just works.

## The Solution

LocalFindJuja is a simple, fast directory of local businesses organized by category with search and filter functionality. No ads. No clutter. Just local businesses you can trust.

## Features

### Current (MVP)
-  Browse businesses by category (Beauty, Hardware, Furniture, Food, etc.)
-  Search by business name or service type
-  Filter by location/neighborhood
-  View business details (address, hours, contact)
-  Mobile-responsive design

### Planned (Future)
-  User-submitted business listings (crowdsourced)
-  Google Maps integration for location view
-  User reviews/ratings
-  Save favorites/bookmarks
-  Backend with database (Firebase/Supabase)

## Tech Stack

**Frontend:**
- React (Vite)
- React Router (for navigation)
- Tailwind CSS (styling)

**Data (MVP):**
- JSON file with business data (will migrate to database if project grows)

**Deployment:**
- Vercel

**Future:**
- Firebase (database, authentication)
- Google Maps API (map view)

## Project Structure
```
localfind/
├── public/
├── src/
│   ├── components/
│   │   ├── Navbar.jsx
│   │   ├── SearchBar.jsx
│   │   ├── CategoryGrid.jsx
│   │   ├── BusinessCard.jsx
│   │   ├── FilterSidebar.jsx
│   │   └── BusinessDetail.jsx
│   ├── pages/
│   │   ├── Home.jsx
│   │   ├── Listings.jsx
│   │   ├── BusinessPage.jsx
│   │   └── About.jsx
│   ├── data/
│   │   └── businesses.json
│   ├── utils/
│   │   └── filterHelpers.js
│   ├── App.jsx
│   ├── main.jsx
│   └── index.css
├── package.json
└── README.md
```

## Data Structure (businesses.json)
```json
[
  {
    "id": 1,
    "name": "Studio Glam Salon",
    "category": "Beauty",
    "subcategory": "Salon",
    "address": "123 Main St, [Your City]",
    "neighborhood": "Downtown",
    "phone": "+254 700 000 000",
    "hours": "Mon-Sat: 9AM-7PM, Sun: Closed",
    "services": ["Hair", "Nails", "Makeup"],
    "description": "Full-service salon specializing in modern cuts and color.",
    "image": "/images/studio-glam.jpg"
  },
  {
    "id": 2,
    "name": "Ink & Steel Piercing",
    "category": "Beauty",
    "subcategory": "Piercing",
    "address": "456 Campus Rd, [Your City]",
    "neighborhood": "Near Campus",
    "phone": "+254 700 111 111",
    "hours": "Tue-Sun: 11AM-8PM, Mon: Closed",
    "services": ["Ear Piercing", "Body Piercing", "Jewelry"],
    "description": "Professional piercing studio with certified piercers.",
    "image": "/images/ink-steel.jpg"
  }
]
```

## Installation & Setup

1. **Clone the repository**
```bash
   git clone https://github.com/wainainagathecha/localfindjuja.git
   cd localfindjuja
```

2. **Install dependencies**
```bash
   npm install
```

3. **Run development server**
```bash
   npm run dev
```

4. **Build for production**
```bash
   npm run build
```

## Learning Goals

This project is my first real React application. I'm using it to learn:

-  React components and props
-  State management with `useState` and `useContext`
-  React Router for navigation
-  Conditional rendering and list mapping
-  Search and filter logic in React
-  CSS in React (Tailwind)
-  API integration
-  Firebase for backend

## Development Phases

### Phase 1: Static Structure (Day 1-2)
- Set up React project with Vite
- Create component structure (Navbar, SearchBar, CategoryGrid, etc.)
- Basic styling and layout
- Responsive design

### Phase 2: Data & Display (Day 2-3)
- Create `businesses.json` with 20-30 real local businesses
- Build BusinessCard component to display business data
- Map through JSON to render business cards
- Individual business detail pages

### Phase 3: Search & Filter (Day 3-4)
- Search bar functionality (filter by name/service)
- Category filter (Beauty, Hardware, Furniture, etc.)
- Neighborhood filter
- Clear filters button

### Phase 4: Polish & Deploy (Day 4-5)
- Improve UI/UX, add loading states
- Mobile optimization
- Deploy to Netlify/Vercel
- Share with classmates for feedback

### Phase 5: Iterate Based on Feedback (Ongoing)
- Add user-submitted business feature (if needed)
- Integrate Google Maps (if users want location view)
- Add favorites/bookmarks (if users request)

## Why I'm Building This

I'm not building this to change the world or scale to a million users.

I'm building this because:
1. **I need it.** I genuinely want to find local businesses easily.
2. **Others need it too.** My classmates and neighbors face the same problem.
3. **I want to learn React** by building something real, not just following tutorials.
4. **It's achievable.** This is scoped small enough that I can actually finish it.

If 10 people use this, great. If only I use it, that's fine too - it still solves my problem.

## Contributing

This is a personal learning project, but if you're local to Juja and want to suggest a business, feel free to open an issue or reach out.

## License

MIT

---

**Built by Wainaina Gathecha **
