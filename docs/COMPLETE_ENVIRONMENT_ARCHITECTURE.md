# 🚀 XPIPS Complete Environment Architecture

## Complete environment separation across all applications - Backend, Web Frontend, and Dashboard.

## 📊 **Complete Architecture Overview**

```
┌─────────────────┬──────────────────┬─────────────────────────────┬───────────────────────────────────────────┐
│ Environment     │ Database         │ Purpose                     │ Applications                              │
├─────────────────┼──────────────────┼─────────────────────────────┼───────────────────────────────────────────┤
│ Local Dev       │ Xpips            │ Development & testing       │ • Backend: localhost:8000                 │
│                 │                  │                             │ • worker-server: localhost:8001           │
│                 │                  │                             │ • Web: localhost:3000                     │
│                 │                  │                             │ • Dashboard: localhost:3001               │
├─────────────────┼──────────────────┼─────────────────────────────┼───────────────────────────────────────────┤
│ Test/Staging    │ Xpips            │ Testing before production   │ • Backend: xpips-backend-staging          │
│                 │                  │                             │ • worker-server: xpips-worker-server-staging│
│                 │ (same as dev)    │                             │ • Web: staging.xpips.com                 │
│                 │                  │                             │ • Dashboard: dashboard-staging.xpips.com │
├─────────────────┼──────────────────┼─────────────────────────────┼───────────────────────────────────────────┤
│ Production      │ Xpips-prod       │ Live production data        │ • Backend: xpips-backend-prod            │
│                 │                  │                             │ • worker-server: xpips-worker-server-prod│
│                 │ (isolated)       │                             │ • Web: web-prod.xpips.com                │
│                 │                  │                             │ • Dashboard: dashboard-prod.xpips.com    │
└─────────────────┴──────────────────┴─────────────────────────────┴───────────────────────────────────────────┘
```

## 🌐 **Complete URL Map**

### Backend APIs

- **Local Dev**: `http://localhost:8000/api/v1`
- **Local Dev Worker-Server**: `http://localhost:8001/api/v1`
- **Staging**: `https://xpips-backend-staging.eba-k4bb9nur.eu-north-1.elasticbeanstalk.com/api/v1`
- **Production**: `https://xpips-backend-prod.eba-k4bb9nur.eu-north-1.elasticbeanstalk.com/api/v1`

### Web Frontend

- **Local Dev**: `http://localhost:3000`
- **Staging**: `https://staging.xpips.com`
- **Web Production**: `https://xpips.com`

### Dashboard

- **Local Dev**: `http://localhost:3001`
- **Staging**: `https://dashboard-staging.xpips.com`
- **Production**: `https://dashboard-prod.xpips.com`

## 🗄️ **Database Configuration**

### MongoDB Atlas - Cluster0

- **Development Database**: `Xpips`

  - Used by: Local dev, Test/Staging environments
  - Contains: All existing development data
  - Collections: contracts, coupons, kycs, messagetemplates, payments, processedcallbacks, programs, users, etc.

- **Production Database**: `Xpips-prod`
  - Used by: Production environments only
  - Contains: Clean production data (no test/dev data)
  - Collections: Same structure as dev, but isolated data

## 🚀 **Deployment Commands**

### Web Frontend (`/xpips-web/`)

```bash
# Test your changes
./deploy-environments.sh test

# Deploy to web production
./deploy-environments.sh web-prod

# Deploy to existing staging (current prod)
./deploy-environments.sh staging

# Future: Deploy to xpips.com
./deploy-environments.sh production
```

### Dashboard (`/xpips-dashboard/`)

```bash
# Test your changes
./deploy-environments.sh staging

# Deploy to dashboard production
./deploy-environments.sh production

# Deploy to current dashboard
./deploy-environments.sh current
```

### Backend (`/xpips-backend/`)

```bash
# Switch to staging
eb use xpips-backend-staging
eb deploy

# Switch to production
eb use xpips-backend-prod
eb deploy
```

## 🔄 **Recommended Development Workflow**

### 1. **Development Phase**

```bash
# Work locally with development database
bun run dev  # All apps use localhost + Xpips database
```

### 2. **Testing Phase**

```bash
# Deploy to test environments (use staging database)
cd xpips-web && ./deploy-environments.sh test
cd xpips-dashboard && ./deploy-environments.sh staging
# Backend staging already deployed
```

### 3. **Production Release**

```bash
# Deploy to production environments (use production database)
cd xpips-web && ./deploy-environments.sh web-prod
cd xpips-dashboard && ./deploy-environments.sh production
cd xpips-backend && eb use xpips-backend-prod && eb deploy
```
