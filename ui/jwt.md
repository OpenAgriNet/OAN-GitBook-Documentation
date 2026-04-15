# JWT

JWT-based authentication

A React application with JWT-based authentication built with Vite, TypeScript, shadcn-ui, and Tailwind CSS.

## Table of Contents

* [Quick Start](jwt.md#quick-start)
* [JWT Authentication](jwt.md#jwt-authentication)
* [Demo Mode](jwt.md#demo-mode)
* [Production Setup](jwt.md#production-setup)
* [Development](jwt.md#development)
* [Technologies Used](jwt.md#technologies-used)

## Quick Start

### Prerequisites

* [Node.js](https://nodejs.org/) (v16 or higher recommended)
* [npm](https://www.npmjs.com/) (comes with Node.js)

### Installation

1. Clone the repository:

```bash
git clone https://github.com/OpenAgriNet/oan-ui-service
cd oan-ui-service
```

2. Install dependencies:

```bash
npm install
```

3. Start the development server:

```bash
npm run dev
```

4. Access the application:
   * The app will be available at http://localhost:5173 (default Vite development server port)
   * Port 5173 is the default for Vite and can be customized in `vite.config.ts` or via `--port` flag
   * **Important:** You need a valid JWT token to access the application

## JWT Authentication

This application uses JWT (JSON Web Token) authentication with RS256 algorithm. Users must provide a valid JWT token to access the application.

### How Authentication Works

1. **Token-based Access:** Users access the app by visiting: `http://localhost:5173?token=YOUR_JWT_TOKEN`
2. **Token Validation:** The app validates the JWT using RSA public key cryptography
3. **Session Storage:** Valid tokens are stored locally for subsequent visits
4. **Automatic Cleanup:** The token parameter is removed from the URL after successful authentication

### JWT Token Requirements

Your JWT token must:

* Be signed with **RS256** algorithm
* Include the following claims:
  * `sub` (subject): User identifier
  * `name`: User's display name
  * `email`: User's email address
  * `iat` (issued at): Token creation timestamp
  * `exp` (expiry): Token expiration timestamp

**Example JWT payload:**

```json
{
  "sub": "user123",
  "name": "John Doe",
  "email": "john@example.com",
  "iat": 1640995200,
  "exp": 1641081600,
  "aud": "oan-ui-service",
  "iss": "your-auth-service"
}
```

### API Services

#### Text-to-Speech (TTS)

* **How it works:** Text messages are sent to `/api/tts/` endpoint with session ID and target language
* **What's sent:** Response text, language preference (e.g., `'mr'`, `'hi'`, `'en'`), and session identifier
* **Response:** Base64-encoded audio data that gets played back to the user

#### Automatic Speech Recognition (ASR)

* **How it works:** Audio is captured from user's microphone, converted to base64, and sent to `/api/transcribe/`
* **What's sent:** Base64-encoded audio data along with the service identifier, and the session identifier
* **Response:** Transcribed text and detected language code for further processing

> All API calls require JWT authentication via Bearer token in headers.

## Demo Mode

### Quick Demo Access with jwt.io

🚀 Want to try the app immediately? Use [jwt.io](https://jwt.io) to generate a demo token:

1. Go to [jwt.io](https://jwt.io)
2. Set the Algorithm to **RS256**
3. Use the demo public/private key pair:

**Private Key (for signing):**

```
-----BEGIN PRIVATE KEY-----
MIIEvQIBADANBgkqhkiG9w0BAQEFAASCBKcwggSjAgEAAoIBAQC7VJTUt9Us8cKj
MzEfYyjiWA4R4/M2bS1GB4t7NXp98C3SC6dVMvDuictGeurT8jNbvJZHtCSuYEvu
NMoSfm76oqFvAp8Gy0iz5sxjZmSnXyCdPEovGhLa0VzMaQ8s+CLOyS56YyCFGeJZ
qgtzJ6GR3eqoYSW9b9UMvkBpZODSctWSNGj3P7jRFDO5VoTwCQAWbFnOjDfH5Ulg
p2PKSQnSJP3AJLQNFNe7br1XbrhV//eO+t51mIpGSDCUv3E0DDFcWDTH9cXDTTlR
ZVEiR2BwpZOOkE/Z0/BVnhZYL71oZV34bKfWjQIt6V/isSMahdoAAQ8GR6YpFCyI
lXcbmwIDAQAB
-----END PRIVATE KEY-----
```

**Public Key (already configured in the app):**

```
-----BEGIN PUBLIC KEY-----
MIIBIjANBgkqhkiG9w0BAQEFAAOCAQ8AMIIBCgKCAQEAu1SU1LfVLPHCozMxH2Mo
4lgOEePzNm0tRgeLezV6ffAt0gunVTLw7onLRnrq0/IzW7yWR7QkrmBL7jTKEn5u
+qKhbwKfBstIs+bMY2Zkp18gnTxKLxoS2tFczGkPLPgizskuemMghRniWaoLcyeh
kd3qqGElvW/VDL5AaWTg0nLVkjRo9z+40RQzuVaE8AkAFmxZzow3x+VJYKdjykkJ
0iT9wCS0DRTXu269V264Vf/3jvredZiKRkgwlL9xNAwxXFg0x/XFw005UWVRIkdg
cKWTjpBP2dPwVZ4WWC+9aGVd+Gyn1o0CLelf4rEjGoXbAAEgAqeGUxrcIlbjXfbc
mwIDAQAB
-----END PUBLIC KEY-----
```

4. Set the payload with your user data:

```json
{
  "sub": "demo-user-123",
  "name": "Demo User",
  "email": "demo@example.com",
  "iat": 1640995200,
  "exp": 9999999999,
  "aud": "oan-ui-service",
  "iss": "demo-auth-service"
}
```

5. Copy the generated JWT token from the left panel
6. Visit your app with the token:

```
http://localhost:5173?token=YOUR_GENERATED_TOKEN
```

**Basic JWT Signing Instructions for jwt.io:**

1. Paste your private key in the "Private Key" section (right panel)
2. Verify the public key appears automatically in the "Public Key" section
3. Edit the payload (middle section) with your desired claims
4. The encoded JWT will appear in the left panel — copy this token
5. **Important:** The signature will only be valid if the public/private key pair matches

### Alternative Online JWT Tools

* [jwt.io](https://jwt.io) — Most popular JWT debugger and generator
* [jwtbuilder.jamiekurtz.com](https://jwtbuilder.jamiekurtz.com) — Simple JWT builder
* [token.dev](https://token.dev) — JWT generator with various algorithms

> ⚠️ **IMPORTANT:** The demo keys are for development only. **DO NOT** use them in production!

## Production Setup

### 1. Generate Your Own RSA Key Pair

For production, you **MUST** generate your own RSA key pair using online tools:

**Option 1: Using jwt.io**

1. Go to [jwt.io](https://jwt.io)
2. Select RS256 algorithm
3. Click "Generate New Keys" at the bottom
4. Copy both the public and private keys

**Option 2: Using Online Key Generators**

* [cryptotools.net](https://cryptotools.net/rsagen) — RSA key pair generator
* [travistidwell.com](https://travistidwell.com/jsencrypt/demo/) — JSEncrypt key generator
* [8gwifi.org](https://8gwifi.org/RSAFunctionality) — RSA key generation tool

**Option 3: Using OpenSSL (if available)**

```bash
# Generate private key
openssl genpkey -algorithm RSA -out private-key.pem -pkcs8 -pass pass:mypassword

# Generate public key
openssl rsa -pubout -in private-key.pem -out public-key.pem
```

### 2. Update the Public Key

Replace the `publicKeyPEM` in `src/contexts/AuthContext.tsx` with your production public key:

```typescript
// In src/contexts/AuthContext.tsx
const publicKeyPEM = `-----BEGIN PUBLIC KEY-----
YOUR_PRODUCTION_PUBLIC_KEY_HERE
-----END PUBLIC KEY-----`;
```

### 3. Secure Your Private Key

* Store your private key securely (environment variables, key vault, etc.)
* **Never** commit private keys to version control
* Use the private key in your authentication service to sign JWT tokens

### 4. Configure Your Authentication Service

Your authentication service should:

1. Validate user credentials (login, OAuth, etc.)
2. Generate JWT tokens using your private key and any JWT library
3. Redirect users to your app with the token: `https://your-app.com?token=${token}`

You can test token generation using [jwt.io](https://jwt.io) with your production keys before implementing in your auth service.

## Development

### File Structure

```
src/
├── contexts/
│   └── AuthContext.tsx    # JWT authentication logic
├── pages/
│   ├── ErrorPage.tsx      # Authentication error page
│   └── ...
└── ...
```

### Key Files

* `src/contexts/AuthContext.tsx`: Main authentication logic with demo public key
* `src/pages/ErrorPage.tsx`: Handles authentication errors

### Environment Variables

You can customize JWT settings and development server configuration via environment variables:

```bash
# .env.local
VITE_JWT_AUDIENCE=your-app-name
VITE_JWT_ISSUER=your-auth-service
```

### Testing Authentication

1. Generate a test token using [jwt.io](https://jwt.io):
   * Use RS256 algorithm
   * Use the demo private key provided above
   * Set your desired payload

2. Test with the token:

```
# Visit this URL in your browser (port 5173 is Vite's default)
http://localhost:5173?token=YOUR_TEST_TOKEN
```

3. Verify authentication:
   * Check browser console for JWT validation logs
   * Confirm user data appears in the application
   * Test token persistence across page refreshes

## Technologies Used

* [Vite](https://vitejs.dev/) — Build tool and development server
* [React](https://react.dev/) — UI framework
* [TypeScript](https://www.typescriptlang.org/) — Type-safe JavaScript
* [shadcn-ui](https://ui.shadcn.com/) — UI component library
* [Tailwind CSS](https://tailwindcss.com/) — Utility-first CSS framework
* [jose](https://github.com/panva/jose) — JWT verification library

## Security Considerations

### Production Checklist

* [ ] Generate your own RSA key pair
* [ ] Replace demo public key in `AuthContext.tsx`
* [ ] Secure your private key (never commit to version control)
* [ ] Set appropriate JWT expiration times
* [ ] Implement proper token refresh mechanisms
* [ ] Use HTTPS in production
* [ ] Validate JWT issuer and audience claims
* [ ] Monitor for token misuse

### JWT Best Practices

* **Short expiration times:** Use short-lived tokens (1–24 hours)
* **Refresh tokens:** Implement refresh token rotation
* **Secure storage:** Store tokens securely (httpOnly cookies preferred over localStorage)
* **Token validation:** Always validate tokens server-side
* **Audience validation:** Verify the `aud` claim matches your application

## Troubleshooting

### Common Issues

**1. "Authentication Required" Error**

* Ensure you have a valid JWT token
* Check token expiration
* Verify the public key matches your private key

**2. JWT Verification Failed**

* Confirm token is signed with RS256 algorithm
* Check that public key in `AuthContext.tsx` is correct
* Verify token hasn't expired

**3. Token Not Persisting**

* Check browser's localStorage for token data
* Ensure token includes required claims (`sub`, `name`, `email`)

### Getting Help

1. Check browser console for detailed error messages
2. Verify JWT token at [jwt.io](https://jwt.io)
3. Review authentication logs in browser developer tools
4. Test with the demo keys provided in this README
