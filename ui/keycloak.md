# Keycloak

Keycloak-based authentication

## OAN UI Service

A React application with Keycloak-based authentication built with Vite, TypeScript, shadcn-ui, and Tailwind CSS.

## Table of Contents

* [Quick Start](keycloak.md#quick-start)
* [Keycloak Authentication](keycloak.md#keycloak-authentication)
* [Local Development Setup](keycloak.md#local-development-setup)
* [Production Setup](keycloak.md#production-setup)
* [Configuration](keycloak.md#configuration)
* [Technologies Used](keycloak.md#technologies-used)

## Quick Start

### Prerequisites

* [Node.js](https://nodejs.org/) (v16 or higher recommended)
* [npm](https://www.npmjs.com/) (comes with Node.js)
* [Docker](https://www.docker.com/get-started/) (for running Keycloak locally)

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
   * The app will be available at the local address shown in your terminal (usually http://localhost:5173)
   * **Important:** You need a running Keycloak server and valid user credentials to access the application

## Keycloak Authentication

This application uses Keycloak for authentication and authorization. Keycloak is an open-source identity and access management solution that provides features like single sign-on (SSO), social login, and user federation.

### How Authentication Works

1. **Login Required:** When users visit the application, they are automatically redirected to Keycloak for authentication
2. **Keycloak Login:** Users enter their credentials on the Keycloak login page
3. **Token Management:** Keycloak issues JWT tokens that are automatically managed by the application
4. **API Authorization:** All API calls include the Bearer token for server-side authorization
5. **Automatic Logout:** Users can logout through the application, which clears their Keycloak session

## Local Development Setup

### 1. Run Keycloak Locally with Docker

Start a Keycloak server using Docker:

```bash
docker run -p 8888:8080 \
  -e KEYCLOAK_ADMIN=admin \
  -e KEYCLOAK_ADMIN_PASSWORD=admin \
  quay.io/keycloak/keycloak:24.0.4 start-dev
```

* Keycloak will be available at: http://localhost:8888
* Admin console username/password: `admin` / `admin`

### 2. Configure Keycloak

**Create a Realm**

1. Open http://localhost:8888
2. Login with admin credentials
3. Click "Create realm" or hover over the realm dropdown
4. Set realm name: `Vistaar`
5. Click "Create"

**Create a Client**

1. Go to Clients → "Create client"
2. General Settings:
   * Client type: `OpenID Connect`
   * Client ID: `vistaar-ui`
3. Capability config:
   * Client authentication: `OFF` (public client)
   * Authorization: `OFF`
   * Standard flow: `ON`
   * Direct access grants: `ON`
4. Login settings:
   * Root URL: `http://localhost:5173`
   * Home URL: `http://localhost:5173`
   * Valid redirect URIs: `http://localhost:5173/*`
   * Valid post logout redirect URIs: `http://localhost:5173/*`
   * Web origins: `http://localhost:5173`

**Create a User**

1. Go to Users → "Add user"
2. Fill in user details:
   * Username: `testuser`
   * Email: `test@example.com`
   * First name: `Test`
   * Last name: `User`
3. Click "Create"
4. Go to "Credentials" tab
5. Set password and toggle "Temporary" to `OFF`
6. Click "Set password"

### 3. Update Application Configuration

The application is pre-configured to work with the local Keycloak setup. The configuration is in `src/lib/keycloak.ts`:

```typescript
const keycloak = new Keycloak({
  url: 'http://localhost:8888/',
  realm: 'Vistaar',
  clientId: 'vistaar-ui',
});
```

For local development, this should work out of the box if you followed the setup above.

## Production Setup

### 1. Keycloak Production Setup

For production, you'll need a running Keycloak instance. Options include:

* **Kubernetes:** Use official Keycloak operator
* **Docker/Container platforms:** Run using Docker Compose or container orchestration
* **Cloud services:** Use managed identity services that support OIDC
* **Self-hosted:** Install Keycloak on your servers

### 2. Production Keycloak Configuration

**Realm Configuration**

* Create a production realm (e.g., `production-realm`)
* Configure proper themes and login pages
* Set up user federation if needed (LDAP, Active Directory, etc.)
* Configure social login providers if required

**Client Configuration**

* Client ID: `your-production-client-id`
* Set proper redirect URIs for your production domain
* Configure client scopes and roles as needed
* Enable proper security settings

**Security Settings**

* Enable HTTPS for all communications
* Configure proper CORS settings
* Set up rate limiting and security policies
* Configure session management and token lifetimes

### 3. Update Application Configuration

Update `src/lib/keycloak.ts` for production:

```typescript
const keycloak = new Keycloak({
  url: 'https://your-keycloak-domain.com/auth',
  realm: 'your-production-realm',
  clientId: 'your-production-client-id',
});
```

### 4. Environment Variables (Optional)

You can use environment variables for configuration:

```bash
# .env.local
VITE_KEYCLOAK_URL=https://your-keycloak-domain.com/auth
VITE_KEYCLOAK_REALM=your-production-realm
VITE_KEYCLOAK_CLIENT_ID=your-production-client-id
```

Then update `keycloak.ts`:

```typescript
const keycloak = new Keycloak({
  url: import.meta.env.VITE_KEYCLOAK_URL || 'http://localhost:8888/',
  realm: import.meta.env.VITE_KEYCLOAK_REALM || 'Vistaar',
  clientId: import.meta.env.VITE_KEYCLOAK_CLIENT_ID || 'vistaar-ui',
});
```

## Configuration

### Current Keycloak Settings

The application is currently configured to connect to:

* **URL:** `https://auth-oan.kenpath.ai/auth`
* **Realm:** `Vistaar`
* **Client ID:** `vistaar-ui`

### Keycloak Integration Features

* **Automatic Login:** Users are redirected to Keycloak login when not authenticated
* **Token Management:** JWT tokens are automatically handled and refreshed
* **User Profile:** User information is available from Keycloak token
* **Logout:** Proper logout that clears Keycloak session
* **API Integration:** Bearer tokens are automatically added to API requests

### File Structure

```
src/
├── lib/
│   └── keycloak.ts        # Keycloak configuration
├── main.tsx               # Keycloak provider setup
├── App.tsx                # Authentication checks
├── components/
│   ├── Layout.tsx         # User profile and logout
│   └── PrivateRoute.tsx   # Route protection
└── ...
```

## Technologies Used

* [Vite](https://vitejs.dev/) — Build tool and development server
* [React](https://react.dev/) — UI framework
* [TypeScript](https://www.typescriptlang.org/) — Type-safe JavaScript
* [Keycloak](https://www.keycloak.org/) — Identity and Access Management
* [@react-keycloak/web](https://github.com/react-keycloak/react-keycloak) — React Keycloak integration
* [shadcn-ui](https://ui.shadcn.com/) — UI component library
* [Tailwind CSS](https://tailwindcss.com/) — Utility-first CSS framework

## Troubleshooting

### Common Issues

**1. "Loading..." screen forever**

* Check if Keycloak server is running and accessible
* Verify the Keycloak URL in `src/lib/keycloak.ts`
* Check browser console for connection errors

**2. "Invalid client" error**

* Ensure the client ID matches in Keycloak and `keycloak.ts`
* Verify the client is properly configured in Keycloak
* Check that the client is enabled

**3. CORS errors**

* Add your application URL to "Web origins" in Keycloak client settings
* Ensure "Valid redirect URIs" includes your app URL with `/*`

**4. Redirect loop**

* Check that "Valid redirect URIs" are properly configured
* Ensure the application URL matches the configured URLs
* Verify the realm name is correct

**5. User cannot login**

* Verify user exists in the correct realm
* Check that user is enabled
* Ensure password is set correctly
* Check if user has required roles/permissions

### Getting Help

1. Check browser console for detailed error messages
2. Review Keycloak server logs for authentication issues
3. Verify all URLs and configuration match between app and Keycloak
4. Test with a fresh browser session to clear cached tokens

### Useful Keycloak Commands

```bash
# View Keycloak logs (if running with Docker)
docker logs <keycloak-container-id>

# Stop Keycloak container
docker stop <keycloak-container-id>

# Remove Keycloak container (loses data)
docker rm <keycloak-container-id>
```
