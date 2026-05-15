# Bites App

Bites is a food-ordering demo application built with a React + Vite frontend and a Node/Express + MongoDB backend. It includes authenticated browsing, cart management, checkout, order history, profile customization, notifications, and a seeded restaurant/menu catalog.

## Overview

The app follows a simple ordering flow:

1. Sign up or log in.
2. Browse restaurants and dishes on the home screen.
3. Search, filter by category, and add items to the cart.
4. Checkout with a saved or selected payment method.
5. Track, cancel, or reorder from the orders page.
6. Edit profile details and store user-specific preferences locally.

The frontend stores the active session token, theme, notifications, usage stats, addresses, and payment methods in `localStorage` so each user keeps their own state across refreshes.

## Features

### Frontend

- Authentication with sign up and login forms.
- Protected routes for the main app experience.
- Dark/light theme toggle.
- Global restaurant and dish search in the header.
- Home screen with featured restaurant cards, category filters, live tracker, stats, and quick add actions.
- Cart sidebar and cart page with quantity updates and removal.
- Checkout flow with payment selection and saved delivery address.
- Orders page with order history, cancel and deliver actions, countdown tracking, and reorder support.
- Profile page for editing name, email, and phone.
- Per-user saved addresses, payment methods, notifications, and usage statistics.

### Backend

- JWT-based authentication with hashed passwords.
- Protected cart and order endpoints.
- Food item catalog API with search and category filtering.
- Cart persistence per authenticated user.
- Checkout that creates an order, clears the cart, and recalculates totals.
- Order cancellation and delivery status updates.
- MongoDB seed data on startup.
- Production static file serving from `dist/` when `NODE_ENV=production`.

## Tech Stack

- React 19
- Vite
- React Router
- Express 5
- MongoDB + Mongoose
- JWT authentication
- bcryptjs for password hashing
- Playwright, Supertest, and Node test runner for automated checks

## Requirements

- Node.js 18 or newer is recommended.
- npm
- MongoDB running locally or a MongoDB Atlas connection string.

## Setup

1. Install dependencies.

```bash
npm install
```

2. Create a `.env` file in the repository root.

```env
MONGODB_URI=mongodb://127.0.0.1:27017/bites-app
JWT_SECRET=change-this-in-production
PORT=5000
ALLOWED_ORIGINS=http://localhost:5173
SEED_ON_START=true
```

Environment variables used by the app:

- `MONGODB_URI` - required MongoDB connection string.
- `JWT_SECRET` - signing key for auth tokens. Use a strong value in production.
- `PORT` - optional backend port. Defaults to `5000`.
- `ALLOWED_ORIGINS` - comma-separated CORS allowlist. Defaults to `http://localhost:5173`.
- `SEED_ON_START` - set to `false` to skip automatic seeding. Defaults to `true`.
- `VITE_API_BASE_URL` - optional frontend override if you are not using the Vite proxy.

3. Start the app.

```bash
# backend only
npm run server

# frontend only
npm run dev

# backend and frontend together
npm run dev:all
```

4. Open the app.

The Vite frontend runs at http://localhost:5173 and proxies `/api` requests to http://localhost:5000.

## Scripts

- `npm run dev` - start the Vite development server.
- `npm run server` - start the Express API server.
- `npm run dev:all` - run frontend and backend together.
- `npm run build` - create a production frontend build.
- `npm run preview` - preview the production build locally.
- `npm run lint` - run ESLint.
- `npm run lint:fix` - run ESLint with auto-fix.
- `npm run format` - format the codebase with Prettier.
- `npm test` - run backend API tests.
- `npm run test:e2e` - run Playwright end-to-end tests.
- `npm run a11y:scan` - run the accessibility scan script.
- `npm run migrate:orders:user-email` - backfill missing `userEmail` values on orders.
- `npm run migrate:orders:link-user` - link orders to user records.

## Folder Structure

```text
.
├── .env.example
├── .eslintrc.cjs
├── .eslintrc.json
├── .gitignore
├── .prettierrc
├── .github/
│   └── workflows/
│       └── ci.yml
├── index.html
├── package-lock.json
├── package.json
├── playwright.config.js
├── PRIORITY_FIXES.md
├── README.md
├── eslint.config.cjs
├── vite.config.js
├── server/
│   ├── config/              # Database connection helpers
│   │   └── db.js
│   ├── middleware/          # Authentication middleware
│   │   └── auth.js
│   ├── models/              # Mongoose schemas
│   │   ├── CartItem.js
│   │   ├── FoodItem.js
│   │   ├── Order.js
│   │   └── User.js
│   ├── routes/              # API route handlers
│   │   ├── auth.js
│   │   ├── cart.js
│   │   ├── foodItems.js
│   │   └── orders.js
│   ├── seed.js              # Database seeding entry point
│   ├── seedData.js          # Seed content for food items, cart, and orders
│   ├── index.js
│   └── tests/               # Backend API tests
│       ├── cart.api.test.js
│       └── orders.api.test.js
├── src/
│   ├── App.css
│   ├── App.jsx
│   ├── assets/
│   │   ├── hero.png
│   │   ├── react.svg
│   │   └── vite.svg
│   ├── components/          # Shared UI components
│   │   ├── CartSidebar.jsx
│   │   ├── CartSidebar.module.css
│   │   ├── Header.jsx
│   │   ├── Header.module.css
│   │   ├── LiveTracker.jsx
│   │   ├── LiveTracker.module.css
│   │   ├── RestaurantCard.jsx
│   │   ├── RestaurantCard.module.css
│   │   ├── StatCard.jsx
│   │   └── StatCard.module.css
│   ├── context/             # Cart and notification state
│   │   ├── CartContext.jsx
│   │   └── NotificationContext.jsx
│   ├── data/                # Static UI data such as categories and notifications
│   │   └── index.js
│   ├── hooks/
│   ├── pages/               # Route-level screens
│   │   ├── Cart.jsx
│   │   ├── Cart.module.css
│   │   ├── Home.jsx
│   │   ├── Home.module.css
│   │   ├── Login.jsx
│   │   ├── Login.module.css
│   │   ├── Orders.jsx
│   │   ├── Orders.module.css
│   │   ├── Profile.jsx
│   │   └── Profile.module.css
│   ├── services/            # API client wrappers
│   │   └── api.js
│   └── utils/               # Shared utilities
│       └── currency.js
├── scripts/
│   ├── backfill-order-user-email.js
│   ├── link-orders-to-user.js
│   ├── reset-user-password.js
│   └── run-axe.js
├── test-results/
└── tests/
    └── e2e/
        └── auth-ui.spec.js
```

## Key Application Areas

### Authentication

- Sign up and login are handled through `/api/auth/signup` and `/api/auth/login`.
- Successful login returns a JWT token and public user profile data.
- Protected frontend routes redirect unauthenticated users to `/login`.

### Browse and Search

- The home page loads restaurant and dish data from `/api/food-items`.
- Search matches restaurant name or cuisine.
- Category filtering is driven by the app's category list.

### Cart and Checkout

- Cart data is scoped to the signed-in user.
- The cart API accepts add, quantity update, and delete actions.
- Checkout calculates subtotal, delivery, and taxes before creating an order.
- After checkout, the cart is cleared and the new order is shown in order history.

### Orders

- Orders are stored per user email.
- Users can cancel pending orders or mark them delivered through the UI flow.
- The orders page also supports reordering from previous orders.

### Profile and Personalization

- Profile details can be edited locally.
- Saved addresses and payment methods are stored per user in `localStorage`.
- Usage stats such as order count, total spent, and streaks are tracked locally.
- Notifications are also stored per user and can be marked read individually or all at once.

## Backend API Summary

- `GET /api/health` - health check.
- `GET /api/food-items` - list food items, with optional `q` and `category` filters.
- `POST /api/auth/signup` - create a new account.
- `POST /api/auth/login` - log in and receive a token.
- `GET /api/cart` - get the authenticated user's cart.
- `POST /api/cart/add` - add an item to the cart.
- `PATCH /api/cart/:id` - update cart quantity by delta.
- `DELETE /api/cart/:id` - remove a cart item.
- `GET /api/orders` - fetch the authenticated user's order history.
- `POST /api/orders/checkout` - place an order from the current cart.
- `PATCH /api/orders/:id/cancel` - cancel an order.
- `PATCH /api/orders/:id/deliver` - mark an order delivered.

## Data Seeding

- On server start, the app seeds food items when the collection is empty.
- Missing food items are added on later starts, and legacy string price values are normalized.
- Set `SEED_ON_START=false` if you want to start the server without seeding.

## Testing

- Backend API tests live in `server/tests/` and use Node's test runner with Supertest and mongodb-memory-server.
- End-to-end tests live in `tests/e2e/` and run with Playwright.
- The accessibility scan script uses Axe to check the UI.

## Troubleshooting

- If cart or order requests fail, confirm the backend is running and `MONGODB_URI` is valid.
- If the frontend cannot reach the API, verify the Vite proxy is active or set `VITE_API_BASE_URL`.
- If auth fails in production, make sure `JWT_SECRET` is set.
- If seeded data does not appear, check the server logs for MongoDB connection issues.

## Related Files

- [Priority fixes and implementation notes](PRIORITY_FIXES.md)
