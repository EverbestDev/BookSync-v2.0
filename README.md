BookSync
BookSync is a modern, user-friendly book discovery platform built with Vue.js and Tailwind CSS. It leverages the Google Books API to help users search, explore, and save their favorite books in a visually appealing Pinterest-style grid. With a responsive design, intuitive navigation, and a contact form powered by Formspree, BookSync makes finding your next great read seamless across all devices.
Features

Search Books: Search millions of books by title, author, or genre using the Google Books API.
Favorites: Save and organize favorite books, stored in localStorage for persistence.
Responsive Design: Optimized for mobile, tablet, and desktop with a clean, emerald-themed UI.
Pinterest-Style Grid: Beautiful masonry layout for browsing books with hover effects and fallbacks for missing covers.
Random Book Toast: Get periodic book recommendations via a stylish toast notification.
Contact Form: Submit feedback via a Formspree-integrated form with client-side validation.
About Page: Learn about BookSync’s mission and features with a polished, animated layout.
SVG Favicon: Custom book icon for brand consistency across tabs and bookmarks.

Tech Stack

Frontend: Vue.js 3 (Composition API)
Styling: Tailwind CSS
API: Google Books API
Form Handling: Formspree (contact form submissions)
Build Tool: Vite (or Vue CLI, depending on your setup)
Deployment: Suitable for static hosting (e.g., Netlify, Vercel)

Prerequisites

Node.js (v16 or higher)
npm or yarn
A Formspree account for contact form submissions (free tier available)

Installation

Clone the Repository
git clone https://github.com/your-username/your-repo-name.git
cd your-repo-name


Install Dependencies
npm install

Or, if using Yarn:
yarn install


Set Up Environment

(Optional) Add a Google Books API key for higher request limits:

Create a .env file in the root directory.
Add: VUE_APP_GOOGLE_BOOKS_API_KEY=your-api-key
Get your API key from Google Cloud Console.


Configure Formspree for the contact form:

Sign up at formspree.io and create a new form.
Copy your Formspree endpoint ID (e.g., xabcde123).
In src/views/ContactView.vue, replace YOUR_ENDPOINT_ID with your endpoint ID in the formspreeEndpoint constant.




Run the Development Server
npm run dev

Or, with Yarn:
yarn dev

Open http://localhost:5173 (or the port shown) in your browser.

Build for Production
npm run build

The output will be in the dist/ folder, ready for deployment to Netlify, Vercel, or similar.


Usage

Search: Use the search bar to find books by title, author, or genre.
Favorites: Click the heart icon on book cards to save/remove favorites. View them in the Favorites section.
Navigation: Use the sidebar (desktop) or bottom nav (mobile) to switch between Home, Books, Favorites, About, and Contact.
Contact: Submit feedback via the contact form, which sends data to your Formspree endpoint.
Random Recommendations: A toast appears every 10 seconds suggesting a random book from the current search.

Formspree Setup

Create a Formspree account and form at formspree.io.
Enable AJAX in the form settings for seamless submissions.
Update src/views/ContactView.vue:
Set formspreeEndpoint to https://formspree.io/f/YOUR_ENDPOINT_ID.


Test the form: Submissions appear in your Formspree dashboard, and users see a success message.

Project Structure
├── public/
│   ├── favicon.svg          # Book-themed SVG favicon
│   └── index.html          # Entry HTML with favicon links
├── src/
│   ├── components/
│   │   ├── BottomNav.vue   # Mobile navigation
│   │   ├── MasonryGrid.vue # Book card grid
│   │   ├── SearchBar.vue   # Search input
│   │   └── Sidebar.vue     # Desktop navigation
│   ├── views/
│   │   ├── AboutView.vue   # About page with mission and features
│   │   ├── BooksView.vue   # Books search results
│   │   ├── ContactView.vue # Contact form with Formspree
│   │   ├── FavoritesView.vue # Favorite books display
│   │   └── HomeView.vue    # Home page with book grid
│   └── App.vue             # Main app component
├── .env                    # Optional: Google Books API key
├── package.json            # Dependencies and scripts
└── README.md               # This file

Contributing
Contributions are welcome! To contribute:

Fork the repository.
Create a feature branch (git checkout -b feature/your-feature).
Commit your changes (git commit -m "Add your feature").
Push to the branch (git push origin feature/your-feature).
Open a pull request.

Please ensure your code follows the existing style (ESLint, Prettier) and includes tests if applicable.
License
This project is licensed under the MIT License. See the LICENSE file for details.
Acknowledgments

Google Books API for book data
Formspree for contact form handling
Tailwind CSS for styling
Vue.js for the reactive frontend
