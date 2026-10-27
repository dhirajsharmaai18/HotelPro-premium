# HotelPro-premium

A hotel management system for handling reservations, room inventory, guest records and payments from one screen. This version is a front-end prototype that runs entirely in the browser; a secure, scalable backend is the planned next step (see [Roadmap](#roadmap)).

## Features

- **Authentication and roles:** sign-in screen with two roles. Managers see everything; front desk staff do not see the Payments tab or revenue figures.
- **Dashboard:** occupancy rate, arrivals waiting, departures due, amount collected and outstanding balances, with one-click check-in and check-out.
- **Room inventory:** 16 rooms across three types (Standard, Deluxe, Suite) with live status for today: vacant, reserved or occupied.
- **Booking management:** create, search, check in, check out and cancel bookings.
- **Availability checking:** a new booking only offers rooms that are free for the chosen dates, and overlapping stays are rejected.
- **Guest management:** guest list built from bookings, showing contact details, number of stays, last stay and total paid.
- **Payments:** record card, UPI or cash payments against a booking. Check-out is blocked until the balance is settled.
- **Responsive and themed:** works on phones and desktops, and follows the system light or dark setting.

## Getting started

No build step or dependencies.

1. Download `hotelpro.html`.
2. Open it in any modern browser.
3. Sign in with a demo account:

| Username | Password   | Role       |
|----------|------------|------------|
| `admin`  | `admin123` | Manager    |
| `desk`   | `desk123`  | Front desk |

Fonts (Newsreader and Figtree) load from Google Fonts. Without internet access the app falls back to system fonts.

## Project structure

```
hotelpro.html   # the whole app: markup, styles and JavaScript in one file
README.md
```

## How it works

- **State:** all bookings are held in one JavaScript array and rendered by a single `render()` function.
- **Storage:** bookings are saved to the browser's `localStorage` under the key `hotelpro-v1`. Clearing site data resets the app to its sample bookings.
- **Rooms and rates:** defined in the `TYPES` and `ROOMS` constants near the top of the script. Edit these to match your property. Prices are in Indian rupees.
- **Availability rule:** a room is free for a stay if no other active booking overlaps it, using `existing.in < new.out && new.in < existing.out`. Cancelled and checked-out bookings do not block a room.
- **Booking record:**

```js
{ id, guest, phone, email, room, in, out, status, paid, total }
// status: 'reserved' | 'in' | 'out' | 'cancelled'
```

## Security notice

This prototype is **not production-ready**. Because it runs only in the browser:

- Demo credentials are stored in the page source, and the login check can be bypassed by anyone who opens the file.
- Guest data lives in the browser's local storage, unencrypted and not shared between devices or staff.
- Payments are recorded as entries only. No real payment gateway is connected.

Do not use it with real guest or payment data until the backend below is in place.

## Roadmap

The intended production architecture:

- **API server** (for example Node.js with Express, or Django) exposing endpoints for bookings, rooms, guests and payments.
- **Database** (for example PostgreSQL) with transactional booking creation, so two staff members cannot double-book a room.
- **Authentication:** server-side password hashing (bcrypt or Argon2), JWT or session tokens, role-based access control and login rate limiting.
- **Data protection:** HTTPS everywhere, encryption of sensitive fields at rest, audit logs and a data-retention policy for guest information.
- **Payments:** integration with a hosted payment gateway such as Razorpay or Stripe, so card details never touch your servers.
- **Real-time availability:** shared updates across devices so every desk sees the same room status.
- **Extras:** housekeeping status, rate plans and seasonal pricing, invoices as PDF, and reporting.


