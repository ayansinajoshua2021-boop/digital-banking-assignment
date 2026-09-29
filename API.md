# Endpoint summary

| Method | Endpoint | Auth |
|---|---|---|
| POST | `/api/auth/register` | No |
| POST | `/api/auth/login` | No |
| POST | `/api/onboarding/bvn` | Yes |
| POST | `/api/onboarding/nin` | Yes |
| POST | `/api/accounts/create` | Yes |
| GET | `/api/accounts/balance` | Yes |
| GET | `/api/transactions/name-enquiry/:accountNumber` | Yes |
| POST | `/api/transactions/transfer` | Yes |
| GET | `/api/transactions/history` | Yes |
| GET | `/api/transactions/status/:reference` | Yes |
| GET | `/health` | No |
