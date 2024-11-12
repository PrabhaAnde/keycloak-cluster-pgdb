# Keycloak Deployment Patterns

This repository contains different deployment patterns for Keycloak using Docker Compose.

## Deployment Options

### 1. Standalone PostgreSQL + Keycloak (docker-compose_standalone_pgdb.yaml)
Basic setup with single Keycloak instance and PostgreSQL database.

Features:
- Single Keycloak node
- PostgreSQL database
- Debug port enabled
- Metrics and health monitoring
- Direct access without load balancer

Usage:
```bash
docker-compose -f docker-compose_standalone_pgdb.yaml up -d
```
### 2. Two Node Cluster with PostgreSQL (docker-compose_cluster_pgdb.yaml)

Features:
 - Two Keycloak nodes in cluster mode
 - Shared PostgreSQL database
 - Nginx load balancer
 - JDBC_PING discovery
 - Session replication
 - Health checks

```bash
docker-compose -f docker-compose_2node_cluster.yaml up -d
```
### 3. Dynamic Scaling Cluster

Features:

 - Dynamic scaling capability
 - Nginx load balancer with auto-discovery
 - Shared PostgreSQL database
 - JDBC_PING discovery
 - Health monitoring

#### Deploy and Scale
```bash
# Start with 3 nodes
docker-compose -f docker-compose_dynamic.yaml up -d --scale kc=3

# Scale to 5 nodes
docker-compose -f docker-compose_dynamic.yaml up -d --scale kc=5
```
#### Environment Configuration

```.env
# Timezone
TZ=America/New_York

# Database Configuration  
POSTGRES_DB=keycloak_db
PGDATA=/var/lib/postgresql/data/pgdata
POSTGRES_USER=kc_admin
POSTGRES_PASSWORD=StrongPass#123
DB_HOST=kcaclpgdb
DB_PORT=6432
DB_NAME=keycloak_db
DB_USER=kc_admin
DB_PASS=StrongPass#123

# Keycloak Configuration
KC_ADMIN=admin_user
KC_ADMIN_PASSWORD=AdminPass#456
KC_HOST_URL=http://kclocalws:9180/auth
KC_PORT=9180
KC_VERSION=26.0.0
KC_DEBUG_PORT=8787
```
#### Access Points

```table
Service	        URL
Admin Console	http://localhost:9180/admin
Health Check	http://localhost:9180/health
Metrics	        http://localhost:9180/metrics
```


