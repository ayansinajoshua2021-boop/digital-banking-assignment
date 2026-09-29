# Postman test checklist

Base URL: `http://localhost:3000`

## Public

### Register
POST `/api/auth/register`

```json
{
  "fullName": "Alice Demo",
  "email": "alice@example.com",
  "password": "Password123"
}
```

### Login
POST `/api/auth/login`

Save `token` from the response.

## Protected header

```text
Authorization: Bearer <token>
Content-Type: application/json
```

### Verify BVN
POST `/api/onboarding/bvn`

```json
{ "bvn": "12345678901" }
```

### Create account
POST `/api/accounts/create`

### Balance
GET `/api/accounts/balance`

### Name enquiry
GET `/api/transactions/name-enquiry/9000000002?bankCode=000`

### Transfer
POST `/api/transactions/transfer`

```json
{
  "recipientAccount": "9000000002",
  "recipientBankCode": "000",
  "amount": 500,
  "narration": "Assignment test"
}
```

### History
GET `/api/transactions/history`

### Status
GET `/api/transactions/status/<reference>`
