# User Verification API

Feature: User Verification  

Purpose: Confirms user identity before enabling deposits and betting.  

Endpoints:  
- `POST /api/v1/verification/request`  
  - Input: `{ "user_id": string }`
  - Action: Generates a verification token and sends an email/SMS with the verification link.
  - Response: `{ "status": "pending", "expires_in": 3600 }`

- `POST /api/v1/verification/confirm`
  - Input: `{ "token": string }`
  - Action: Validates token; updates user record to `verified = true`.
  - Response: `{ "status": "verified" }`

Token Logic:  
- Tokens are JWT-based, signed with the internal secret key.
- Valid for 1 hour; single-use only.
- Stored in the verification table with timestamps and IP metadata.
- Expired or reused tokens return HTTP 401 – Invalid Token.

Notes:  
Verification status is cached and refreshed on login.
Downstream services must check the verified flag before processing bets or deposits.
