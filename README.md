<div align="center">

<img src="Frontend/public/mabo_logo.png" alt="Mabo Games logo" width="320">

<br><br>

**A full-stack digital game store built with ASP.NET Core 8 and Angular 21.**

![.NET](https://img.shields.io/badge/.NET_8-%235C2D91.svg?style=for-the-badge&logo=.net&logoColor=white)
![Angular](https://img.shields.io/badge/Angular_21-%23DD0031.svg?style=for-the-badge&logo=angular&logoColor=white)
![SQL Server](https://img.shields.io/badge/SQL_Server-%23CC2927.svg?style=for-the-badge&logo=microsoftsqlserver&logoColor=white)
![Stripe](https://img.shields.io/badge/Stripe-%235469d4.svg?style=for-the-badge&logo=stripe&logoColor=ffffff)

</div>

<a name="overview"></a>
## Overview

Mabo Games is an e-commerce platform for browsing, purchasing, and managing digital games, originally built as a seminar project for a Software Development 1 course.

The backend is an ASP.NET Core 8 API with a layered architecture (API, Application, Domain, Infrastructure, Shared) using CQRS via MediatR. The frontend is a standalone Angular 21 application built with Angular Material.

The project covers the complete storefront flow: authentication, catalog browsing, cart management, Stripe checkout, a personal game library, and an admin dashboard for managing the store.

<br clear="right">

<br>

# Table of Contents

- [Features](#features)
- [Tech Stack](#tech-stack)
- [Project Structure](#project-structure)
- [Getting Started](#getting-started)
  - [Requirements](#requirements)
  - [Backend Setup](#backend-setup)
  - [Frontend Setup](#frontend-setup)
  - [Demo Accounts](#demo-accounts)
- [API Keys Configuration](#api-keys-configuration)
  - [IGDB](#igdb)
  - [Google reCAPTCHA](#google-recaptcha)
  - [ImgBB](#imgbb)
- [Stripe Setup](#stripe-setup)

---

<br>

<a name="features"></a>
## Features

### Implemented

- [x] User authentication (register, login, logout) with JWT
- [x] Forgot password flow (email verification code or security question)
- [x] Storefront with latest and cheapest game sections
- [x] Catalog browsing with search, sorting, genre filtering, and pagination
- [x] Game details page with screenshots, ratings, and reviews
- [x] Shopping cart (add, remove, clear, save for later)
- [x] Stripe checkout (order creation, redirect, webhook confirmation)
- [x] Personal game library with search and genre filters
- [x] Favourites (add/remove, highlighted in library)
- [x] Admin dashboard (revenue, orders, top-selling games)
- [x] Notifications
- [x] Achievements
- [x] User profile management
- [x] Multi-language support (English, Bosnian)
- [x] Backend unit tests (xUnit)

### Planned

- [ ] Light mode / theme toggle

---

<br>

<a name="tech-stack"></a>
## Tech Stack

| Layer        | Technology                                            |
|--------------|-------------------------------------------------------|
| Backend      | ASP.NET Core 8, Entity Framework Core, MediatR (CQRS) |
| Database     | SQL Server, Supabase                                  |
| Frontend     | Angular 21, Angular Material, RxJS                    |
| Auth         | JWT                                                   |
| Payments     | Stripe                                                |

---

<br>

<a name="project-structure"></a>
## Project Structure

```
Backend/
├── Market.API             HTTP layer: controllers, middleware, composition root
├── Market.Application     Use cases and CQRS handlers (MediatR)
├── Market.Domain          Core entities and domain logic
├── Market.Infrastructure  EF Core, database access, external integrations
├── Market.Shared          DTOs, constants, shared options
└── Market.Tests           xUnit test suite

Frontend/src/app/
├── core                   Singleton services, guards, interceptors, models
├── modules                Feature areas (admin, auth, public, shared)
├── shared                 Shared UI components
└── api-services           Typed API client services

Documents/                 Project documentation
```

---

<br>

<a name="getting-started"></a>
## Getting Started

<a name="requirements"></a>
### Requirements

- [.NET 8 SDK](https://dotnet.microsoft.com/download/dotnet/8.0)
- SQL Server (local instance)
- [Node.js](https://nodejs.org/)
- [Stripe CLI](https://stripe.com/docs/stripe-cli) (for payment testing)

<a name="backend-setup"></a>
### Backend Setup

1. Open the `Backend` solution in Visual Studio.
2. Set `Market.API` as the startup project and run it.
3. The database is created and seeded automatically on first startup.

<a name="frontend-setup"></a>
### Frontend Setup

1. Open the `Frontend` folder in your editor.
2. Install dependencies and start the dev server:

   ```bash
   npm install
   ng serve
   ```

3. Open [http://localhost:4200](http://localhost:4200).

<a name="demo-accounts"></a>
### Demo Accounts

The database seeder creates the following accounts:

| Email             | Password   | Role  |
|-------------------|------------|-------|
| admin@market.com  | Admin123!  | Admin |
| user@market.com   | User123!   | User  |

---

<br>

<a name="api-keys-configuration"></a>
## API Keys Configuration

Some features depend on external services. Backend keys go in `Backend/Market.API/appsettings.json`; public keys used by the browser go in the Angular environment file (`Frontend/src/environments/environment.ts`).

```json
"Igdb": {
  "ClientId": "your-twitch-client-id",
  "ClientSecret": "your-twitch-client-secret"
},
"ReCaptcha": {
  "SecretKey": "your-recaptcha-secret-key"
},
"ImgBB": {
  "ApiKey": "your-imgbb-api-key"
}
```

<a name="igdb"></a>
### IGDB

Used for game data (covers, screenshots, metadata). IGDB authenticates through Twitch.

<details>
<summary>Setup steps</summary>

1. Log in to the [Twitch Developer Console](https://dev.twitch.tv/console) (two-factor authentication must be enabled on your Twitch account).
2. Go to **Applications → Register Your Application**.
3. Set the OAuth redirect URL to `http://localhost` and the category to **Application Integration**.
4. Open the created application, copy the **Client ID**, and generate a **Client Secret**.
5. Add both to the `Igdb` section of `appsettings.json`.

See the [IGDB API docs](https://api-docs.igdb.com/#account-creation) for details.

</details>

<br>

<a name="google-recaptcha"></a>
### Google reCAPTCHA

Used to protect the login and registration forms.

<details>
<summary>Setup steps</summary>

1. Go to the [reCAPTCHA Admin Console](https://www.google.com/recaptcha/admin/create) and register a new site.
2. Add `localhost` to the list of domains.
3. Copy the two keys:
   - **Site key** → `Frontend/src/environments/environment.ts`
   - **Secret key** → `ReCaptcha` section of `appsettings.json`

```ts
export const environment = {
  // ...
  recaptchaSiteKey: 'your-recaptcha-site-key'
};
```

</details>

<br>

<a name="imgbb"></a>
### ImgBB

Used for image uploads (e.g. profile pictures and game images).

<details>
<summary>Setup steps</summary>

1. Create a free account at [imgbb.com](https://imgbb.com).
2. Go to [api.imgbb.com](https://api.imgbb.com) and click **Get API key**.
3. Add the key to the `ImgBB` section of `appsettings.json`.

</details>

---

<br>

<a name="stripe-setup"></a>
## Stripe Setup

Purchases require Stripe configured in **test mode**. No real payments are made.

### 1. Create an account and get your secret key

1. Create a free account at [stripe.com](https://stripe.com) and switch to **Test Mode**.
2. Go to **Developers → API Keys** and copy the **Secret key** (`sk_test_...`).

### 2. Install the Stripe CLI and log in

Install the CLI from the [Stripe CLI docs](https://stripe.com/docs/stripe-cli), then run:

```bash
stripe login
```

### 3. Forward webhooks to the API

In a separate terminal, run:

```bash
stripe listen --forward-to https://localhost:7260/api/webhooks/stripe
```

The CLI will print a webhook signing secret (`whsec_...`).

### 4. Add both keys to `appsettings.json`

```json
"Stripe": {
  "SecretKey": "sk_test_...",
  "WebhookSecret": "whsec_..."
}
```

### 5. Test a purchase

Keep `stripe listen` running while you check out, and use Stripe's test card:

| Field       | Value                   |
|-------------|-------------------------|
| Card number | `4242 4242 4242 4242`   |
| Expiry      | Any future date         |
| CVC         | Any 3 digits            |