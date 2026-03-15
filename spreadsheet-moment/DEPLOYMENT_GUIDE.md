# Spreadsheet Moment - Deployment Guide

**Version:** 1.0.0
**Last Updated:** March 15, 2026

---

## Table of Contents

- [Deployment Overview](#deployment-overview)
- [Prerequisites](#prerequisites)
- [Environment Configuration](#environment-configuration)
- [Deployment Options](#deployment-options)
- [Cloudflare Workers Deployment](#cloudflare-workers-deployment)
- [Docker Deployment](#docker-deployment)
- [Kubernetes Deployment](#kubernetes-deployment)
- [Desktop Application](#desktop-application)
- [Monitoring Setup](#monitoring-setup)
- [Rollback Procedures](#rollback-procedures)
- [Troubleshooting](#troubleshooting)

---

## Deployment Overview

Spreadsheet Moment supports multiple deployment strategies to suit different use cases:

| Deployment Type | Best For | Cost | Complexity |
|----------------|----------|------|------------|
| **Cloudflare Workers** | Production, global edge | Pay-per-use | Low |
| **Docker** | Self-hosted, on-premise | Server cost | Medium |
| **Kubernetes** | Enterprise, scalable | Cluster cost | High |
| **Desktop** | Offline, local work | Free | Low |

---

## Prerequisites

### Common Requirements

- **Node.js:** 18.0 or higher
- **npm:** 9.0 or higher
- **Git:** For cloning repository
- **Domain:** Custom domain (optional)

### Cloudflare Workers

- Cloudflare account (free tier available)
- Wrangler CLI installed
- Custom domain (optional)

### Docker

- Docker Engine 20.10+
- Docker Compose 2.0+
- 2GB RAM minimum
- 10GB disk space

### Kubernetes

- Kubernetes cluster 1.25+
- kubectl configured
- Helm 3.0+ (optional)
- Ingress controller (optional)

### Desktop

- Rust toolchain (1.70+)
- Platform-specific dependencies
- Code signing certificate (for distribution)

---

## Environment Configuration

### Environment Variables

Create a `.env` file in the project root:

```bash
# Application
NODE_ENV=production
APP_URL=https://spreadsheet-moment.example.com
API_URL=https://api.spreadsheet-moment.example.com

# Database
DATABASE_URL=postgresql://user:password@host:5432/spreadsheet_moment
DATABASE_POOL_SIZE=20

# Redis
REDIS_URL=redis://host:6379
REDIS_PASSWORD=your-redis-password

# Authentication
JWT_SECRET=your-jwt-secret-key-min-32-chars
JWT_EXPIRES_IN=15m
REFRESH_TOKEN_EXPIRES_IN=30d

# OAuth (Optional)
GOOGLE_CLIENT_ID=your-google-client-id
GOOGLE_CLIENT_SECRET=your-google-client-secret
GITHUB_CLIENT_ID=your-github-client-id
GITHUB_CLIENT_SECRET=your-github-client-secret

# Cloudflare (Optional)
CLOUDFLARE_ACCOUNT_ID=your-account-id
CLOUDFLARE_API_TOKEN=your-api-token

# Monitoring
SENTRY_DSN=https://your-sentry-dsn
ANALYTICS_ID=your-analytics-id

# Features
ENABLE_HARDWARE_I/O=true
ENABLE_WEBSOCKET=true
ENABLE_CORS=true
```

### Configuration Files

**Production Config (`config/production.json`):**

```json
{
  "app": {
    "name": "Spreadsheet Moment",
    "version": "1.0.0",
    "environment": "production"
  },
  "server": {
    "port": 8080,
    "host": "0.0.0.0",
    "cors": {
      "origin": ["https://spreadsheet-moment.example.com"],
      "credentials": true
    },
    "rateLimit": {
      "windowMs": 900000,
      "max": 100
    }
  },
  "database": {
    "ssl": true,
    "pool": {
      "min": 2,
      "max": 20
    }
  },
  "redis": {
    "ttl": 3600,
    "maxRetriesPerRequest": 3
  },
  "logging": {
    "level": "info",
    "format": "json"
  }
}
```

---

## Deployment Options

### Option 1: Cloudflare Workers (Recommended)

**Pros:**
- Zero cold starts
- Global edge deployment
- Auto-scaling
- Pay-per-use pricing
- DDoS protection

**Cons:**
- Vendor lock-in
- Limited compute time
- Specific runtime constraints

**Quick Start:**

```bash
# Install Wrangler CLI
npm install -g wrangler

# Login to Cloudflare
wrangler login

# Deploy Workers
cd workers
wrangler deploy

# Deploy website to Pages
cd ../website
npm run build
wrangler pages deploy dist --project-name=spreadsheet-moment
```

**Full Setup:** See [Cloudflare Workers Deployment](#cloudflare-workers-deployment)

---

### Option 2: Docker

**Pros:**
- Self-hosted
- Full control
- Offline capable
- Portable

**Cons:**
- Server management
- Scaling complexity
- Backup responsibility

**Quick Start:**

```bash
# Build image
docker build -t spreadsheet-moment:1.0.0 .

# Run container
docker run -d \
  --name spreadsheet-moment \
  -p 8080:8080 \
  --env-file .env \
  spreadsheet-moment:1.0.0
```

**Full Setup:** See [Docker Deployment](#docker-deployment)

---

### Option 3: Kubernetes

**Pros:**
- Enterprise-grade
- Auto-scaling
- Self-healing
- Rolling updates

**Cons:**
- High complexity
- Resource intensive
- Steep learning curve

**Quick Start:**

```bash
# Deploy with Helm
helm install spreadsheet-moment ./helm-chart

# Or with kubectl
kubectl apply -f kubernetes/
```

**Full Setup:** See [Kubernetes Deployment](#kubernetes-deployment)

---

### Option 4: Desktop Application

**Pros:**
- Offline capable
- Native performance
- Local data
- Hardware integration

**Cons:**
- Platform-specific
- Update management
- Distribution complexity

**Quick Start:**

```bash
# Build desktop app
cd desktop
npm install
npm run tauri build
```

**Full Setup:** See [Desktop Application](#desktop-application)

---

## Cloudflare Workers Deployment

### Step 1: Install Dependencies

```bash
npm install -g wrangler
```

### Step 2: Configure Wrangler

Create `workers/wrangler.toml`:

```toml
name = "spreadsheet-moment-api"
main = "src/index.ts"
compatibility_date = "2024-01-01"

[vars]
ENVIRONMENT = "production"

[[kv_namespaces]]
binding = "KV"
id = "your-kv-namespace-id"

[[durable_objects.bindings]]
name = "CELLS"
class_name = "CellObject"
```

### Step 3: Deploy Workers

```bash
cd workers
wrangler deploy
```

### Step 4: Configure KV Namespace

```bash
# Create KV namespace
wrangler kv:namespace create "SPREADSHEET_MOMENT"

# Create Durable Object
wrangler d1 create DATABASE
```

### Step 5: Deploy Website to Pages

```bash
cd website
npm run build
wrangler pages deploy dist --project-name=spreadsheet-moment
```

### Step 6: Configure Custom Domain

```bash
# Add custom domain
wrangler pages custom-domains create spreadsheet-moment.example.com
```

### Step 7: Verify Deployment

```bash
# Test Workers
curl https://spreadsheet-moment-api.example.com/health

# Test Website
curl https://spreadsheet-moment.example.com
```

---

## Docker Deployment

### Step 1: Build Image

```bash
docker build -t spreadsheet-moment:1.0.0 .
```

### Step 2: Configure Environment

```bash
# Copy environment template
cp .env.example .env

# Edit configuration
nano .env
```

### Step 3: Run Container

```bash
docker run -d \
  --name spreadsheet-moment \
  -p 8080:8080 \
  --env-file .env \
  -v spreadsheet-data:/data \
  spreadsheet-moment:1.0.0
```

### Step 4: Configure Reverse Proxy (Optional)

**Nginx Configuration:**

```nginx
server {
    listen 80;
    server_name spreadsheet-moment.example.com;

    location / {
        proxy_pass http://localhost:8080;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
    }
}
```

### Step 5: Deploy with Docker Compose

```bash
docker-compose up -d
```

**Docker Compose File:**

```yaml
version: '3.8'

services:
  app:
    image: spreadsheet-moment:1.0.0
    ports:
      - "8080:8080"
    environment:
      - DATABASE_URL=postgresql://postgres:password@db:5432/spreadsheet_moment
      - REDIS_URL=redis://redis:6379
    depends_on:
      - db
      - redis
    restart: unless-stopped

  db:
    image: postgres:15
    environment:
      - POSTGRES_DB=spreadsheet_moment
      - POSTGRES_USER=postgres
      - POSTGRES_PASSWORD=password
    volumes:
      - postgres-data:/var/lib/postgresql/data
    restart: unless-stopped

  redis:
    image: redis:7-alpine
    restart: unless-stopped

volumes:
  postgres-data:
```

---

## Kubernetes Deployment

### Step 1: Create Namespace

```bash
kubectl create namespace spreadsheet-moment
```

### Step 2: Create Secrets

```bash
kubectl create secret generic spreadsheet-moment-secrets \
  --from-literal=jwt-secret=your-jwt-secret \
  --from-literal=database-url=postgresql://... \
  --from-literal=redis-url=redis://... \
  -n spreadsheet-moment
```

### Step 3: Deploy Application

```bash
kubectl apply -f kubernetes/ -n spreadsheet-moment
```

**Deployment YAML:**

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: spreadsheet-moment
  namespace: spreadsheet-moment
spec:
  replicas: 3
  selector:
    matchLabels:
      app: spreadsheet-moment
  template:
    metadata:
      labels:
        app: spreadsheet-moment
    spec:
      containers:
      - name: app
        image: spreadsheet-moment:1.0.0
        ports:
        - containerPort: 8080
        env:
        - name: DATABASE_URL
          valueFrom:
            secretKeyRef:
              name: spreadsheet-moment-secrets
              key: database-url
        - name: REDIS_URL
          valueFrom:
            secretKeyRef:
              name: spreadsheet-moment-secrets
              key: redis-url
        resources:
          requests:
            memory: "512Mi"
            cpu: "500m"
          limits:
            memory: "1Gi"
            cpu: "1000m"
        livenessProbe:
          httpGet:
            path: /health
            port: 8080
          initialDelaySeconds: 30
          periodSeconds: 10
        readinessProbe:
          httpGet:
            path: /ready
            port: 8080
          initialDelaySeconds: 5
          periodSeconds: 5
---
apiVersion: v1
kind: Service
metadata:
  name: spreadsheet-moment
  namespace: spreadsheet-moment
spec:
  selector:
    app: spreadsheet-moment
  ports:
  - port: 80
    targetPort: 8080
  type: LoadBalancer
```

### Step 4: Configure Ingress (Optional)

```bash
kubectl apply -f kubernetes/ingress.yaml
```

**Ingress YAML:**

```yaml
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: spreadsheet-moment
  namespace: spreadsheet-moment
  annotations:
    cert-manager.io/cluster-issuer: letsencrypt-prod
spec:
  tls:
  - hosts:
    - spreadsheet-moment.example.com
    secretName: spreadsheet-moment-tls
  rules:
  - host: spreadsheet-moment.example.com
    http:
      paths:
      - path: /
        pathType: Prefix
        backend:
          service:
            name: spreadsheet-moment
            port:
              number: 80
```

### Step 5: Deploy with Helm (Optional)

```bash
helm install spreadsheet-moment ./helm-chart \
  --namespace spreadsheet-moment \
  --set image.tag=1.0.0 \
  --set env.database.url=postgresql://... \
  --set env.redis.url=redis://...
```

---

## Desktop Application

### Step 1: Install Rust

```bash
# macOS
curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh

# Windows
# Download from https://rustup.rs/

# Linux
curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
```

### Step 2: Install Dependencies

```bash
cd desktop
npm install
```

### Step 3: Configure Application

Edit `desktop/src-tauri/tauri.conf.json`:

```json
{
  "build": {
    "beforeDevCommand": "npm run dev",
    "beforeBuildCommand": "npm run build",
    "devPath": "http://localhost:3000",
    "distDir": "../dist"
  },
  "package": {
    "productName": "Spreadsheet Moment",
    "version": "1.0.0"
  },
  "tauri": {
    "allowlist": {
      "all": false,
      "shell": {
        "all": false,
        "open": true
      },
      "fs": {
        "all": false,
        "readFile": true,
        "writeFile": true,
        "scope": ["$HOME/.spreadsheet-moment/*"]
      }
    },
    "bundle": {
      "active": true,
      "targets": "all",
      "identifier": "com.superinstance.spreadsheet-moment",
      "icon": [
        "icons/32x32.png",
        "icons/128x128.png",
        "icons/128x128@2x.png",
        "icons/icon.icns",
        "icons/icon.ico"
      ]
    }
  }
}
```

### Step 4: Build Application

```bash
# Development build
npm run tauri dev

# Production build
npm run tauri build
```

### Step 5: Distribute

Installers are located in:
- **Windows:** `desktop/src-tauri/target/release/bundle/msi/`
- **macOS:** `desktop/src-tauri/target/release/bundle/dmg/`
- **Linux:** `desktop/src-tauri/target/release/bundle/deb/` or `appimage/`

---

## Monitoring Setup

### Application Monitoring

**Sentry Integration:**

```typescript
import * as Sentry from "@sentry/react";

Sentry.init({
  dsn: process.env.SENTRY_DSN,
  environment: process.env.NODE_ENV,
  tracesSampleRate: 1.0,
  integrations: [
    new Sentry.BrowserTracing(),
    new Sentry.Replay(),
  ],
});
```

**Metrics Collection:**

```typescript
// Custom metrics
import { register, Counter, Histogram } from 'prom-client';

const httpRequestDuration = new Histogram({
  name: 'http_request_duration_seconds',
  help: 'Duration of HTTP requests in seconds',
  labelNames: ['method', 'route', 'status_code'],
});

const cellOperations = new Counter({
  name: 'cell_operations_total',
  help: 'Total number of cell operations',
  labelNames: ['operation', 'status'],
});
```

### Log Aggregation

**Winston Logger:**

```typescript
import winston from 'winston';

const logger = winston.createLogger({
  level: 'info',
  format: winston.format.json(),
  transports: [
    new winston.transports.File({ filename: 'error.log', level: 'error' }),
    new winston.transports.File({ filename: 'combined.log' }),
  ],
});

if (process.env.NODE_ENV !== 'production') {
  logger.add(new winston.transports.Console({
    format: winston.format.simple(),
  }));
}
```

### Health Checks

**Health Endpoint:**

```typescript
app.get('/health', async (req, res) => {
  const health = {
    status: 'healthy',
    timestamp: new Date().toISOString(),
    uptime: process.uptime(),
    checks: {
      database: await checkDatabase(),
      redis: await checkRedis(),
      memory: process.memoryUsage(),
    },
  };

  res.status(200).json(health);
});
```

---

## Rollback Procedures

### Cloudflare Workers

```bash
# View deployment history
wrangler deployments list

# Rollback to previous version
wrangler rollback

# Or deploy specific version
wrangler deploy --version <version-id>
```

### Docker

```bash
# Stop current container
docker stop spreadsheet-moment

# Remove container
docker rm spreadsheet-moment

# Run previous version
docker run -d \
  --name spreadsheet-moment \
  -p 8080:8080 \
  --env-file .env \
  spreadsheet-moment:0.9.0
```

### Kubernetes

```bash
# View rollout history
kubectl rollout history deployment/spreadsheet-moment -n spreadsheet-moment

# Rollback to previous version
kubectl rollout undo deployment/spreadsheet-moment -n spreadsheet-moment

# Rollback to specific revision
kubectl rollout undo deployment/spreadsheet-moment --to-revision=2 -n spreadsheet-moment
```

### Desktop Application

For desktop apps, distribute a new version with the previous code:
1. Revert changes in git
2. Build new installer
3. Distribute update through auto-updater

---

## Troubleshooting

### Common Issues

#### 1. Workers Deployment Fails

**Problem:** `wrangler deploy` fails with authentication error

**Solution:**
```bash
# Re-authenticate
wrangler logout
wrangler login
```

#### 2. Docker Container Crashes

**Problem:** Container exits immediately after start

**Solution:**
```bash
# Check logs
docker logs spreadsheet-moment

# Common issues:
# - Missing environment variables
# - Database connection failed
# - Port already in use
```

#### 3. Kubernetes Pod Not Ready

**Problem:** Pod stuck in pending or crash loop

**Solution:**
```bash
# Check pod status
kubectl get pods -n spreadsheet-moment

# Describe pod
kubectl describe pod <pod-name> -n spreadsheet-moment

# Check logs
kubectl logs <pod-name> -n spreadsheet-moment

# Common issues:
# - Image pull error
# - Resource limits
# - Missing secrets
# - ConfigMap problems
```

#### 4. Database Connection Failed

**Problem:** Application can't connect to database

**Solution:**
```bash
# Check database is running
kubectl get pods -l app=postgres -n spreadsheet-moment

# Check database logs
kubectl logs -l app=postgres -n spreadsheet-moment

# Verify connection string
echo $DATABASE_URL

# Test connection
psql $DATABASE_URL
```

#### 5. High Memory Usage

**Problem:** Application consuming too much memory

**Solution:**
```bash
# Check memory usage
kubectl top pods -n spreadsheet-moment

# Adjust resource limits
kubectl edit deployment spreadsheet-moment -n spreadsheet-moment

# Add memory limits:
# resources:
#   limits:
#     memory: "2Gi"
```

### Debug Mode

**Enable Debug Logging:**

```bash
# Set environment variable
export DEBUG=spreadsheet-moment:*

# Or in .env
DEBUG=spreadsheet-moment:*
```

**Verbose Docker Logs:**

```bash
docker logs spreadsheet-moment --tail 100 -f
```

**Kubernetes Events:**

```bash
kubectl get events -n spreadsheet-moment --sort-by='.lastTimestamp'
```

---

## Performance Tuning

### Database Optimization

**Connection Pooling:**

```javascript
const pool = new Pool({
  max: 20, // Maximum pool size
  min: 2,  // Minimum pool size
  idle: 10000, // Idle timeout
  connectionTimeoutMillis: 1000,
});
```

**Query Optimization:**

```sql
-- Add indexes
CREATE INDEX idx_cells_created_at ON cells(created_at);
CREATE INDEX idx_cells_type ON cells(type);

-- Analyze query performance
EXPLAIN ANALYZE SELECT * FROM cells WHERE type = 'predictor';
```

### Redis Optimization

**Memory Management:**

```bash
# Set max memory
redis-cli CONFIG SET maxmemory 1gb

# Set eviction policy
redis-cli CONFIG SET maxmemory-policy allkeys-lru
```

**Connection Pooling:**

```javascript
const redis = require('redis');
const client = redis.createClient({
  host: process.env.REDIS_HOST,
  port: process.env.REDIS_PORT,
  retry_strategy: (options) => {
    if (options.error && options.error.code === 'ECONNREFUSED') {
      return new Error('Redis server refused connection');
    }
    if (options.total_retry_time > 1000 * 60 * 60) {
      return new Error('Retry time exhausted');
    }
    return Math.min(options.attempt * 100, 3000);
  },
});
```

---

## Backup and Recovery

### Database Backup

**Automated Backup:**

```bash
# Daily backup
0 2 * * * pg_dump -U postgres spreadsheet_moment | gzip > /backups/db_$(date +\%Y\%m\%d).sql.gz

# Keep last 7 days
0 3 * * * find /backups -name "db_*.sql.gz" -mtime +7 -delete
```

**Manual Backup:**

```bash
# Backup database
pg_dump -U postgres spreadsheet_moment > backup.sql

# Restore database
psql -U postgres spreadsheet_moment < backup.sql
```

### Redis Backup

**Automated Backup:**

```bash
# Enable AOF
redis-cli CONFIG SET appendonly yes

# Create backup
redis-cli BGSAVE

# Copy RDB file
cp /var/lib/redis/dump.rdb /backups/dump_$(date +%Y%m%d).rdb
```

---

## Conclusion

This deployment guide covers all major deployment options for Spreadsheet Moment. Choose the deployment method that best fits your use case:

- **Cloudflare Workers** for production, global deployments
- **Docker** for self-hosted, on-premise deployments
- **Kubernetes** for enterprise, scalable deployments
- **Desktop** for offline, local work

For additional help, see:
- [Troubleshooting](#troubleshooting)
- [Monitoring Setup](#monitoring-setup)
- [Rollback Procedures](#rollback-procedures)

---

**Document Version:** 1.0.0
**Last Updated:** March 15, 2026
**Maintained By:** Spreadsheet Moment DevOps Team
