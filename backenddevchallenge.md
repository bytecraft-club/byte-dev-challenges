# 🚀 Backend Dev Challenge: Prayer Times API Gateway & Analytics Platform

## Challenge Overview
Build a sophisticated backend system that acts as a gateway and analytics platform for the Aladhan Prayer Times API. This **advanced challenge** involves creating a high-performance API with caching, authentication, rate limiting, analytics, and additional Islamic services. Designed for completion in **one intensive day**.

## 🎯 Objectives
- Create a production-ready API gateway with advanced features
- Implement sophisticated caching and optimization strategies
- Build comprehensive analytics and monitoring systems
- Add value through additional Islamic services and calculations
- Design scalable architecture with proper security measures

## 🛠 Technical Requirements

### Core Features (Must Have)
1. **API Gateway**: Proxy and enhance Aladhan API calls
2. **Authentication & Authorization**: JWT-based user system
3. **Rate Limiting**: Multi-tier rate limiting with quotas
4. **Caching Layer**: Redis-based caching with intelligent invalidation
5. **Request Analytics**: Comprehensive API usage tracking
6. **Error Handling**: Robust error management and logging

### Advanced Features (High Difficulty)
1. **Prayer Time Calculations**: Custom calculation methods and adjustments
2. **Geolocation Services**: Advanced location detection and timezone handling
3. **Notification System**: Prayer reminder scheduling with multiple channels
4. **Islamic Calendar API**: Comprehensive Hijri calendar with events
5. **Qibla Calculation**: Precise direction calculations from any location
6. **Multi-language Support**: Internationalization for Islamic terms
7. **Webhook System**: Event-driven notifications for prayer times
8. **Real-time Features**: WebSocket support for live updates

## 🏗 Architecture Design

### System Architecture
```
┌─────────────────┐    ┌─────────────────┐    ┌─────────────────┐
│   Load Balancer │────│   API Gateway   │────│   Aladhan API   │
│    (Nginx)      │    │   (Express/     │    │   (External)    │
└─────────────────┘    │   FastAPI)      │    └─────────────────┘
                       └─────────┬───────┘
                                 │
          ┌──────────────────────┼──────────────────────┐
          │                      │                      │
    ┌─────────────┐    ┌─────────────┐        ┌─────────────┐
    │   Redis     │    │ PostgreSQL  │        │  MongoDB    │
    │  (Cache)    │    │ (Analytics) │        │ (Sessions)  │
    └─────────────┘    └─────────────┘        └─────────────┘
          │                      │                      │
          └──────────────────────┼──────────────────────┘
                                 │
                    ┌─────────────────┐
                    │   Monitoring    │
                    │ (Prometheus/    │
                    │   Grafana)      │
                    └─────────────────┘
```

### Technology Stack Options

#### Node.js + Express (Recommended)
```bash
# Initialize project
npm init -y
npm install express cors helmet compression
npm install redis ioredis
npm install jsonwebtoken bcryptjs
npm install express-rate-limit express-slow-down
npm install winston morgan
npm install prisma @prisma/client
npm install axios node-cron
npm install socket.io
npm install joi celebrate
npm install swagger-jsdoc swagger-ui-express
```

#### Python + FastAPI
```bash
# Create virtual environment
python -m venv venv
source venv/bin/activate  # or `venv\Scripts\activate` on Windows

# Install dependencies
pip install fastapi uvicorn
pip install redis sqlalchemy alembic
pip install python-jose[cryptography] passlib[bcrypt]
pip install slowapi
pip install httpx python-multipart
pip install celery
pip install prometheus-client
pip install websockets
```

#### Go + Gin
```bash
go mod init prayer-times-gateway
go get github.com/gin-gonic/gin
go get github.com/go-redis/redis/v8
go get github.com/golang-jwt/jwt/v4
go get github.com/ulule/limiter/v3
go get gorm.io/gorm gorm.io/driver/postgres
go get github.com/prometheus/client_golang
```

## 📊 Database Schema Design

### PostgreSQL Tables
```sql
-- Users and Authentication
CREATE TABLE users (
    id SERIAL PRIMARY KEY,
    email VARCHAR(255) UNIQUE NOT NULL,
    password_hash VARCHAR(255) NOT NULL,
    api_key VARCHAR(255) UNIQUE,
    plan_type VARCHAR(50) DEFAULT 'free',
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

-- API Analytics
CREATE TABLE api_requests (
    id SERIAL PRIMARY KEY,
    user_id INTEGER REFERENCES users(id),
    endpoint VARCHAR(255) NOT NULL,
    method VARCHAR(10) NOT NULL,
    status_code INTEGER NOT NULL,
    response_time INTEGER NOT NULL, -- in milliseconds
    request_size INTEGER,
    response_size INTEGER,
    location JSONB,
    user_agent TEXT,
    ip_address INET,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

-- Prayer Time Cache Metadata
CREATE TABLE cache_entries (
    id SERIAL PRIMARY KEY,
    cache_key VARCHAR(255) UNIQUE NOT NULL,
    location JSONB NOT NULL,
    calculation_method INTEGER,
    expires_at TIMESTAMP NOT NULL,
    hit_count INTEGER DEFAULT 0,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

-- User Preferences
CREATE TABLE user_preferences (
    id SERIAL PRIMARY KEY,
    user_id INTEGER REFERENCES users(id) UNIQUE,
    default_location JSONB,
    calculation_method INTEGER DEFAULT 2,
    timezone VARCHAR(50),
    language VARCHAR(10) DEFAULT 'en',
    notification_settings JSONB,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

-- Notification Queue
CREATE TABLE notification_queue (
    id SERIAL PRIMARY KEY,
    user_id INTEGER REFERENCES users(id),
    prayer_name VARCHAR(50) NOT NULL,
    prayer_time TIMESTAMP NOT NULL,
    location JSONB NOT NULL,
    status VARCHAR(20) DEFAULT 'pending',
    delivery_method VARCHAR(20) DEFAULT 'push',
    scheduled_at TIMESTAMP NOT NULL,
    delivered_at TIMESTAMP,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);
```

### Redis Schema Design
```javascript
// Cache Structure
const CACHE_KEYS = {
  PRAYER_TIMES: 'prayer:times:{lat}:{lng}:{date}:{method}',
  LOCATION_CACHE: 'location:geocode:{query}',
  RATE_LIMIT: 'rate:limit:{user_id}:{endpoint}',
  ANALYTICS: 'analytics:daily:{date}',
  QIBLA_DIRECTION: 'qibla:{lat}:{lng}',
  CALENDAR_EVENTS: 'islamic:calendar:{year}:{month}'
};

// Example cached prayer times
{
  "prayer:times:40.7128:-74.0060:2024-09-19:2": {
    "timings": {
      "Fajr": "05:30",
      "Sunrise": "07:00",
      "Dhuhr": "12:30",
      "Asr": "15:45",
      "Maghrib": "18:15",
      "Isha": "19:45"
    },
    "location": {
      "latitude": 40.7128,
      "longitude": -74.0060,
      "city": "New York",
      "country": "United States",
      "timezone": "America/New_York"
    },
    "method": 2,
    "cached_at": "2024-09-19T10:00:00Z",
    "ttl": 86400
  }
}
```

## 🔧 Core Implementation

### 1. Advanced API Gateway (Node.js Example)
```javascript
const express = require('express');
const rateLimit = require('express-rate-limit');
const Redis = require('ioredis');
const jwt = require('jsonwebtoken');
const axios = require('axios');

class PrayerTimesGateway {
  constructor() {
    this.app = express();
    this.redis = new Redis(process.env.REDIS_URL);
    this.aladhanAPI = 'https://api.aladhan.com/v1';
    this.setupMiddleware();
    this.setupRoutes();
  }

  setupMiddleware() {
    // Rate limiting with different tiers
    const createRateLimit = (windowMs, max, message) => 
      rateLimit({
        windowMs,
        max,
        message: { error: message },
        standardHeaders: true,
        legacyHeaders: false,
        keyGenerator: (req) => {
          return req.user?.id || req.ip;
        }
      });

    // Different rate limits for different plans
    this.freeTierLimit = createRateLimit(
      15 * 60 * 1000, // 15 minutes
      100, // requests
      'Free tier rate limit exceeded'
    );

    this.premiumLimit = createRateLimit(
      15 * 60 * 1000,
      1000,
      'Premium tier rate limit exceeded'
    );

    // Authentication middleware
    this.authenticate = async (req, res, next) => {
      try {
        const token
