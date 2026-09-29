# Digital Banking System — TS Academy Assignment

A Node.js/Express + PostgreSQL backend implementing customer onboarding, account creation, transfers, balances, transaction history, transaction status and NibssByPhoenix integration.

## Requirements covered

- Customer registration and JWT login
- BVN or NIN onboarding before account creation
- No real BVN/NIN is required in local/mock mode
- One account maximum per customer
- Account is pre-funded with NGN 15,000
- Name enquiry before transfers
- Intra-bank and inter-bank transfer support
- Account balance check
- Transaction status check
- Customer-only transaction history
- SQL-level data isolation using `customer_id`
- Password hashing with bcrypt
- Helmet, CORS and rate limiting
- NibssByPhoenix service layer with token caching
- Mock Nibss mode for assignment testing without real banking credentials

## 1. Install

```bash
npm install
```

## 2. Start PostgreSQL

If Docker is installed:

```bash
docker compose up -d
```

Or create a PostgreSQL database called `digital_banking` and run `schema.sql`.

## 3. Environment variables

Copy `.env.example` to `.env`.

For local testing, leave:

```env
MOCK_NIBSS=true
```

This creates test accounts and simulates identity verification, name enquiry and transfers.

## 4. Run

```bash
npm run dev
```

Health check:

```text
GET http://localhost:3000/health
```

## 5. API flow

### Register

`POST /api/auth/register`

```json
{
  "fullName": "Demo Customer",
  "email": "demo@example.com",
  "password": "Password123"
}
```

### Login

`POST /api/auth/login`

```json
{
  "email": "demo@example.com",
  "password": "Password123"
}
```

Copy the returned JWT and use:

```text
Authorization: Bearer YOUR_TOKEN
```

for protected endpoints.

### Verify BVN

`POST /api/onboarding/bvn`

```json
{
  "bvn": "12345678901"
}
```

Do not use a real BVN for assignment testing.

### Or verify NIN

`POST /api/onboarding/nin`

```json
{
  "nin": "10987654321"
}
```

### Create account

`POST /api/accounts/create`

The customer must be verified first.

The new test account starts with NGN 15,000.

### Balance

`GET /api/accounts/balance`

### Name enquiry

`GET /api/transactions/name-enquiry/9000000001?bankCode=000`

### Transfer

`POST /api/transactions/transfer`

```json
{
  "recipientAccount": "9000000002",
  "recipientBankCode": "000",
  "amount": 1000,
  "narration": "Test transfer"
}
```

### Transaction history

`GET /api/transactions/history`

The SQL query always filters by the authenticated customer's ID.

### Transaction status

`GET /api/transactions/status/TXN-...`

## NibssByPhoenix production/testing integration

The assignment's Swagger is the source of truth for the exact provider paths and request/response shapes. This project keeps provider calls in `src/config/nibss.js`.

When you receive your NibssByPhoenix API credentials, set:

```env
MOCK_NIBSS=false
NIBSS_API_KEY=...
NIBSS_API_SECRET=...
NIBSS_API_EMAIL=...
NIBSS_BANK_CODE=...
NIBSS_BANK_NAME=...
```

Then set the `NIBSS_*_PATH` variables to the exact endpoint paths shown in the supplied Swagger.

This avoids hard-coding uncertain provider paths and keeps the banking integration in one service layer.

## Suggested Postman testing order

1. Register customer A.
2. Login customer A.
3. Verify A with test BVN or NIN.
4. Create A's account.
5. Register customer B.
6. Login B.
7. Verify B.
8. Create B's account.
9. Run name enquiry for B's account.
10. Transfer from A to B.
11. Check A's balance.
12. Check A's transaction history.
13. Check the transaction status.
14. Attempt to create a second account for A — it should fail.
15. Attempt to access another customer's transaction reference — it should return 404.

## Security notes

Never commit `.env`, API keys, API secrets or real BVN/NIN data. This project is an educational banking simulation and should not be used with real customer data without proper security, compliance, audit and provider approval.
