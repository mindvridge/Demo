# 🚀 TarotMind Quick Start Guide
## 개발 환경 설정 및 시작 가이드

---

## 📦 Prerequisites

### Required Software
```bash
# Node.js 20 LTS
node --version  # v20.x.x

# Docker Desktop
docker --version  # 24.x.x

# PostgreSQL Client
psql --version  # 15.x

# Redis CLI
redis-cli --version  # 7.x

# AWS CLI
aws --version  # 2.x.x

# kubectl
kubectl version  # 1.28.x
```

---

## 🛠️ Local Development Setup

### 1. Repository Clone & Install

```bash
# Clone repositories
git clone https://github.com/tarotmind/backend.git
git clone https://github.com/tarotmind/frontend-mobile.git
git clone https://github.com/tarotmind/frontend-web.git

# Backend setup
cd backend
npm install
cp .env.example .env
# Edit .env with your local settings

# Mobile app setup
cd ../frontend-mobile
npm install
cd ios && pod install  # iOS only
cd ..

# Web app setup
cd ../frontend-web
npm install
```

### 2. Database Setup

```bash
# Start PostgreSQL with Docker
docker run --name tarotmind-postgres \
  -e POSTGRES_USER=tarot \
  -e POSTGRES_PASSWORD=mystic123 \
  -e POSTGRES_DB=tarotmind \
  -p 5432:5432 \
  -d postgres:15

# Run migrations
cd backend
npm run migration:run

# Seed initial data
npm run seed:tarot-cards
npm run seed:spreads
npm run seed:test-users
```

### 3. Redis Setup

```bash
# Start Redis
docker run --name tarotmind-redis \
  -p 6379:6379 \
  -d redis:7-alpine

# Verify connection
redis-cli ping
# Should return: PONG
```

### 4. Environment Variables

#### Backend (.env)
```env
# App
NODE_ENV=development
PORT=3000
APP_URL=http://localhost:3000

# Database
DATABASE_URL=postgresql://tarot:mystic123@localhost:5432/tarotmind

# Redis
REDIS_HOST=localhost
REDIS_PORT=6379

# JWT
JWT_SECRET=your-super-secret-jwt-key-change-in-production
JWT_EXPIRES_IN=1h
JWT_REFRESH_EXPIRES_IN=7d

# OpenAI
OPENAI_API_KEY=sk-your-openai-api-key

# AWS (LocalStack for development)
AWS_REGION=ap-northeast-2
AWS_ACCESS_KEY_ID=test
AWS_SECRET_ACCESS_KEY=test
S3_BUCKET=tarotmind-assets-dev

# Payment (Test Keys)
STRIPE_SECRET_KEY=sk_test_your-stripe-test-key
STRIPE_WEBHOOK_SECRET=whsec_your-webhook-secret

# Push Notifications
FCM_SERVER_KEY=your-fcm-server-key
APNS_KEY_ID=your-apns-key-id
APNS_TEAM_ID=your-team-id

# Monitoring
SENTRY_DSN=https://your-sentry-dsn
DATADOG_API_KEY=your-datadog-key
```

#### Mobile App (.env)
```env
API_URL=http://localhost:3000
ENABLE_DEV_MENU=true
```

#### Web App (.env.local)
```env
NEXT_PUBLIC_API_URL=http://localhost:3000
NEXT_PUBLIC_WS_URL=ws://localhost:3000
```

---

## 🏃 Running the Application

### Start All Services

```bash
# Terminal 1: Backend
cd backend
npm run dev
# API running at http://localhost:3000
# GraphQL Playground at http://localhost:3000/graphql

# Terminal 2: Mobile App (iOS)
cd frontend-mobile
npm run ios
# Or for Android:
npm run android

# Terminal 3: Web App
cd frontend-web
npm run dev
# Web app running at http://localhost:3001

# Terminal 4: LocalStack (AWS Services Mock)
docker run --rm -it \
  -p 4566:4566 \
  -p 4571:4571 \
  localstack/localstack
```

### Useful Commands

```bash
# Backend
npm run dev          # Start development server
npm run build        # Build for production
npm run test         # Run tests
npm run test:watch   # Run tests in watch mode
npm run test:cov     # Run tests with coverage
npm run lint         # Run ESLint
npm run format       # Format code with Prettier

# Database
npm run migration:create -- CreateUserTable
npm run migration:run
npm run migration:revert
npm run schema:sync  # Sync schema (dev only!)

# Mobile
npm run ios          # Run on iOS simulator
npm run android      # Run on Android emulator
npm run test         # Run tests
npm run lint         # Run ESLint

# Web
npm run dev          # Start development server
npm run build        # Build for production
npm run start        # Start production server
npm run lint         # Run ESLint
npm run test         # Run tests
```

---

## 🧪 Testing

### Run All Tests
```bash
# Backend
cd backend
npm run test:all

# This runs:
# - Unit tests
# - Integration tests
# - E2E tests
```

### Test Specific Features
```bash
# Test tarot engine
npm test -- src/tarot/tarot.service.spec.ts

# Test auth flow
npm test -- src/auth/auth.e2e.spec.ts

# Test with debugging
node --inspect-brk ./node_modules/.bin/jest --runInBand
```

### Frontend Testing
```bash
# Mobile app testing
cd frontend-mobile
npm test
npm run test:e2e  # Detox tests

# Web app testing
cd frontend-web
npm test
npm run test:e2e  # Cypress tests
```

---

## 📱 Mobile Development Tips

### iOS Specific

```bash
# Clear build cache
cd ios
rm -rf build/
pod cache clean --all
pod install

# Fix common issues
cd ..
npx react-native-clean-project
```

### Android Specific

```bash
# Clear build cache
cd android
./gradlew clean
cd ..

# Start emulator from command line
emulator -avd Pixel_4_API_30

# Reverse port for localhost API access
adb reverse tcp:3000 tcp:3000
```

### React Native Debugging

```javascript
// Enable Flipper
import {Flipper} from 'react-native-flipper';

if (__DEV__) {
  Flipper.init();
}

// Redux DevTools
import {composeWithDevTools} from 'redux-devtools-extension';

const store = createStore(
  rootReducer,
  composeWithDevTools(applyMiddleware(...middleware))
);
```

---

## 🔄 Git Workflow

### Feature Development

```bash
# 1. Create feature branch
git checkout develop
git pull origin develop
git checkout -b feature/TAR-123-card-animation

# 2. Make changes and commit
git add .
git commit -m "feat: add 3D flip animation to tarot cards"

# 3. Push and create PR
git push origin feature/TAR-123-card-animation
# Create PR on GitHub

# 4. After approval, merge
git checkout develop
git merge --no-ff feature/TAR-123-card-animation
git push origin develop
```

### Commit Message Format

```
<type>(<scope>): <subject>

<body>

<footer>
```

Examples:
```bash
feat(tarot): add three-card spread layout
fix(auth): resolve token refresh race condition  
docs(api): update reading endpoint documentation
style(mobile): improve card selection animation
refactor(ai): optimize interpretation generation
test(payment): add subscription renewal tests
chore(deps): update React Native to 0.72.5
```

---

## 🐛 Debugging

### Backend Debugging

```javascript
// VS Code launch.json
{
  "version": "0.2.0",
  "configurations": [
    {
      "type": "node",
      "request": "launch",
      "name": "Debug NestJS",
      "runtimeArgs": [
        "-r",
        "ts-node/register",
        "-r",
        "tsconfig-paths/register"
      ],
      "args": ["src/main.ts"],
      "env": {
        "NODE_ENV": "development"
      },
      "sourceMaps": true,
      "cwd": "${workspaceRoot}",
      "protocol": "inspector"
    }
  ]
}
```

### Database Queries

```bash
# Connect to local database
psql -U tarot -d tarotmind

# Useful queries
\dt                           # List all tables
\d+ users                     # Describe users table
SELECT COUNT(*) FROM users;   # Count users
SELECT * FROM readings 
  WHERE user_id = 'uuid' 
  ORDER BY created_at DESC 
  LIMIT 10;                   # Recent readings

# Monitor active queries
SELECT pid, now() - pg_stat_activity.query_start AS duration, query 
FROM pg_stat_activity 
WHERE (now() - pg_stat_activity.query_start) > interval '5 minutes';
```

### Redis Debugging

```bash
# Connect to Redis
redis-cli

# Useful commands
KEYS *                    # List all keys (dev only!)
GET session:user-id       # Get session data
TTL daily_reading:user-id # Check TTL
FLUSHDB                   # Clear database (dev only!)
MONITOR                   # Watch commands in real-time
```

---

## 🚀 Deployment

### Build for Production

```bash
# Backend
cd backend
npm run build
# Output in dist/

# Mobile (iOS)
cd frontend-mobile/ios
fastlane beta  # TestFlight
fastlane release  # App Store

# Mobile (Android)
cd frontend-mobile/android
./gradlew bundleRelease
# Output in android/app/build/outputs/bundle/release/

# Web
cd frontend-web
npm run build
npm run export  # Static export
# Output in out/
```

### Docker Build

```bash
# Build images
docker build -t tarotmind-api:latest ./backend
docker build -t tarotmind-web:latest ./frontend-web

# Run with docker-compose
docker-compose up -d

# Check logs
docker-compose logs -f api
```

### Deploy to AWS

```bash
# Configure AWS CLI
aws configure

# Deploy to EKS
kubectl apply -f k8s/

# Update deployment
kubectl set image deployment/api api=tarotmind-api:v1.0.1

# Check status
kubectl rollout status deployment/api
```

---

## 📚 Useful Resources

### Documentation
- [API Documentation](http://localhost:3000/api-docs)
- [GraphQL Playground](http://localhost:3000/graphql)
- [Storybook](http://localhost:6006)
- [Database Schema](./docs/database-schema.md)
- [Architecture Decision Records](./docs/adr/)

### External Documentation
- [NestJS Docs](https://docs.nestjs.com)
- [React Native Docs](https://reactnative.dev)
- [Next.js Docs](https://nextjs.org/docs)
- [PostgreSQL Docs](https://www.postgresql.org/docs/15/)
- [Redis Docs](https://redis.io/documentation)

### Monitoring Links
- [Local Metrics](http://localhost:3000/metrics)
- [Development Sentry](https://sentry.io/organizations/tarotmind/projects/)
- [DataDog Dashboard](https://app.datadoghq.com/dashboard)

---

## 🆘 Troubleshooting

### Common Issues

#### Port Already in Use
```bash
# Find process using port
lsof -i :3000
# Kill process
kill -9 <PID>
```

#### Node Modules Issues
```bash
rm -rf node_modules package-lock.json
npm cache clean --force
npm install
```

#### Database Connection Failed
```bash
# Check PostgreSQL is running
docker ps | grep postgres
# Restart if needed
docker restart tarotmind-postgres
```

#### iOS Build Failed
```bash
cd ios
pod deintegrate
pod cache clean --all
rm -rf ~/Library/Developer/Xcode/DerivedData
pod install
```

#### Android Build Failed
```bash
cd android
./gradlew clean
rm -rf ~/.gradle/caches/
./gradlew build
```

---

## 👥 Team Contacts

| Role | Name | Slack | Email |
|------|------|-------|-------|
| Tech Lead | 김민수 | @minsu | minsu@tarotmind.com |
| Backend Dev | 이지은 | @jieun | jieun@tarotmind.com |
| Frontend Dev | 박준영 | @junyoung | junyoung@tarotmind.com |
| DevOps | 최서연 | @seoyeon | seoyeon@tarotmind.com |
| QA Engineer | 정태호 | @taeho | taeho@tarotmind.com |

### Support Channels
- **Slack**: #dev-general, #dev-help
- **Wiki**: https://wiki.tarotmind.com
- **JIRA**: https://tarotmind.atlassian.net

---

## 🎯 Development Checklist

### Before Committing
- [ ] Code compiles without warnings
- [ ] All tests pass
- [ ] ESLint/Prettier rules pass
- [ ] New code has tests
- [ ] Documentation updated if needed
- [ ] Commit message follows convention

### Before Creating PR
- [ ] Branch is up to date with develop
- [ ] PR description filled out
- [ ] Screenshots added (if UI changes)
- [ ] Linked to JIRA ticket
- [ ] Requested reviewers assigned
- [ ] CI/CD checks pass

### Before Deploying
- [ ] All tests pass in staging
- [ ] Database migrations reviewed
- [ ] Environment variables updated
- [ ] Monitoring alerts configured
- [ ] Rollback plan prepared
- [ ] Team notified in Slack

---

**Happy Coding! 🎴✨**

*Last Updated: January 2025*
*Version: 1.0.0*
