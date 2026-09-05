# Fix&Go — Production Web Foundation

A responsive automotive marketplace/service web application built from the Fix&Go prototype. It includes a real HTTP backend, SQLite database, authentication, vehicle garage, product catalog, fitment, cart, checkout demo adapter, bookings, maintenance plans/subscriptions, AI development adapter, support tickets and admin metrics.

## Run

Requires Node.js 22+.

```bash
cp .env.example .env
node server.js
```

Open `http://localhost:3000`.

The app creates `fixgo.sqlite` automatically and seeds products, vehicle fitment, services and the three maintenance plans on first run.

## Demo authentication

Register a customer from `/register`. Passwords are hashed with Node's built-in `scrypt`; session tokens are HMAC signed. No real payment information is stored.

To create an admin for local testing, after registering, update the role in SQLite:

```sql
UPDATE users SET role='admin' WHERE email='your-email@example.com';
```

Then log in again and visit `/admin`.

## Architecture

- Frontend: semantic responsive HTML/CSS/JavaScript SPA served by Node.
- Backend: Node.js built-in HTTP server with `/api/*` endpoints.
- Database: SQLite through Node 22's built-in `node:sqlite` API.
- Authentication: scrypt password hashing + signed HMAC session tokens.
- Integrations: development adapters are clearly indicated in UI and can be replaced with production providers.

## Core flows implemented

- Vehicle selection and garage
- Vehicle-aware catalog and compatibility indicators
- Product detail pages
- Search, filters and sorting
- Wishlist API
- Cart persistence
- Demo checkout → order
- Maintenance service booking
- Maintenance Basic / Care / Complete plans
- Subscription state and renewal data
- AI automotive development adapter
- Mechanic discovery and booking entry
- Installation booking entry
- Support ticket API
- Admin dashboard metrics

## Production integrations still required

Configure real providers for payment/subscription billing, maps/location, VIN lookup, authoritative fitment, AI, email/SMS, inventory/ERP and delivery. The code keeps these concerns behind API/adaptor boundaries rather than pretending that demo behavior is production-connected.

## Security notes

- Never commit `.env` or production secrets.
- Replace the development `AUTH_SECRET`.
- Put the app behind HTTPS and a production reverse proxy.
- Add production rate limiting/WAF, CSRF strategy as appropriate, secure cookies/session storage, observability and backup/restore procedures.
- Replace demo payment checkout with a PCI-compliant provider; raw card details are never stored by this app.

## Roadside exclusion

Fix&Go intentionally has no roadside assistance, stranded, emergency dispatch, roadside technician, roadside tracking or related route/database/API functionality.
