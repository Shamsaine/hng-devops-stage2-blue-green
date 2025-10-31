# 🚀 DevOps Stage 2 – Blue/Green Deployment with Nginx

A **zero-downtime deployment** solution using Docker Compose and Nginx to manage Blue/Green application environments with automatic failover capabilities.

## 📋 Overview

This project demonstrates a **high-availability Blue/Green deployment** pattern where:
- **Blue Environment**: Primary active application instance
- **Green Environment**: Backup standby application instance  
- **Nginx Load Balancer**: Intelligent traffic router with automatic failover

When the active environment fails, Nginx automatically routes traffic to the backup instance without any downtime, ensuring **100% service availability**.

## 🏗️ Architecture

```
┌─────────────────┐
│   Client/User   │
└────────┬────────┘
         │
         ▼
┌────────────────┐
│     Nginx      │ ← Reverse Proxy + Auto-Failover
│   (Port 8080)  │
└───────┬────────┘
        │
   ┌────┴────┐
   ▼         ▼
┌──────┐  ┌──────┐
│ Blue │  │Green │
│(8081)│  │(8082)│
└──────┘  └──────┘
```

**Traffic Flow:**
- **Normal State**: All traffic → Blue (primary)
- **Failure State**: Nginx detects failure → Auto-route to Green (backup)
- **Recovery**: Manual restoration or automatic failback

## 🎯 Features

- ✅ **Zero-Downtime Deployments**: Seamless traffic switching between environments
- ✅ **Automatic Failover**: Nginx detects failures and routes to healthy instances
- ✅ **Health Monitoring**: Built-in health checks for all services
- ✅ **Header Forwarding**: Preserves application headers (`X-App-Pool`, `X-Release-Id`)
- ✅ **Environment Parameterization**: Fully configurable via `.env` file
- ✅ **Chaos Engineering**: Built-in failure simulation for testing

## 📁 Project Structure

```
hng-devops-stage2-blue-green/
├── docker-compose.yml          # Container orchestration
├── nginx.conf                  # Nginx proxy configuration
├── .env                       # Environment variables
├── .env.example               # Environment template
├── .gitignore                # Git ignore rules
├── README.md                 # This file
└── DECISION.md               # Architecture decisions
```

## 🚀 Quick Start

### 1️⃣ Clone Repository
```bash
git clone <your-repo-url>
cd hng-devops-stage2-blue-green
```

### 2️⃣ Configure Environment
```bash
cp .env.example .env
# Edit .env with your values
```

### 3️⃣ Start Services
```bash
docker-compose up -d
```

### 4️⃣ Verify Deployment
```bash
# Check service status
docker-compose ps

# Test main endpoint
curl http://localhost:8080/version
```

## 🌐 API Endpoints

### Application Endpoints
| Endpoint | Method | Description | Response |
|----------|--------|-------------|----------|
| `/version` | GET | Application version info | JSON with headers |
| `/healthz` | GET | Health check status | Health status |
| `/chaos/start` | POST | Simulate service failure | Chaos mode activated |
| `/chaos/stop` | POST | Stop failure simulation | Chaos mode stopped |

### Nginx Endpoints
| Endpoint | Port | Description |
|----------|------|-------------|
| `http://localhost:8080/*` | 8080 | Main application (load balanced) |
| `http://localhost:8081/*` | 8081 | Blue environment (direct access) |
| `http://localhost:8082/*` | 8082 | Green environment (direct access) |

## 🧪 Testing Failover

### Normal Operation
```bash
# Should return Blue environment
curl -i http://localhost:8080/version

# Expected response headers:
# X-App-Pool: blue
# X-Release-Id: v1.0.0
```

### Simulate Failure
```bash
# Trigger chaos on Blue environment
curl -X POST http://localhost:8081/chaos/start

# Test automatic failover (should now return Green)
curl -i http://localhost:8080/version

# Expected response headers:
# X-App-Pool: green  
# X-Release-Id: v1.0.1
```

### Recovery
```bash
# Stop chaos mode
curl -X POST http://localhost:8081/chaos/stop

# Traffic may return to Blue (depending on configuration)
curl -i http://localhost:8080/version
```

## ⚙️ Configuration

### Environment Variables (.env)
```bash
# Docker Images
BLUE_IMAGE=yimikaade/wonderful:devops-stage-two
GREEN_IMAGE=yimikaade/wonderful:devops-stage-two

# Active Pool
ACTIVE_POOL=blue

# Release Identifiers  
RELEASE_ID_BLUE=v1.0.0
RELEASE_ID_GREEN=v1.0.1

# Port Configuration
PORT=8080
```

### Nginx Configuration
- **Primary/Backup Pattern**: Blue is primary, Green is backup
- **Fast Failure Detection**: 3s connection timeout, 5s read timeout
- **Automatic Retry**: Up to 2 retries on failure
- **Header Preservation**: All upstream headers forwarded to clients

## 🔍 Monitoring & Debugging

### Check Service Health
```bash
# Overall status
docker-compose ps

# Individual health checks
curl http://localhost:8081/healthz  # Blue
curl http://localhost:8082/healthz  # Green
```

### View Logs
```bash
# All services
docker-compose logs

# Specific service
docker-compose logs nginx
docker-compose logs blue
docker-compose logs green
```

### Network Connectivity
```bash
# Test container connectivity
docker exec nginx-proxy ping blue-app
docker exec nginx-proxy ping green-app
```

## 🚨 Troubleshooting

### Common Issues

**Containers not starting:**
```bash
# Check logs for errors
docker-compose logs

# Restart services
docker-compose down
docker-compose up -d
```

**Health checks failing:**
```bash
# Test health endpoints directly
docker exec blue-app wget -O- http://localhost:3000/healthz
docker exec green-app wget -O- http://localhost:3000/healthz
```

**Nginx routing issues:**
```bash
# Test nginx configuration
docker exec nginx-proxy nginx -t

# Check upstream connectivity
docker exec nginx-proxy nslookup blue-app
docker exec nginx-proxy nslookup green-app
```

## 📊 Performance Considerations

- **Failure Detection**: ~3-5 seconds
- **Failover Time**: ~1-2 seconds  
- **Recovery Time**: Configurable (manual or automatic)
- **Availability Target**: 99.9%+ uptime

## 🤝 Contributing

1. Fork the repository
2. Create a feature branch
3. Make your changes
4. Test thoroughly
5. Submit a pull request

## 📝 License

This project is part of the HNG DevOps Stage 2 curriculum.

---

**Author**: Shamsudeen Ibrahim 
**Stage**: HNG DevOps Stage 2  
**Date**: October 2025  

For questions or support, please refer to the [DECISION.md](DECISION.md) for architectural decisions and reasoning.
