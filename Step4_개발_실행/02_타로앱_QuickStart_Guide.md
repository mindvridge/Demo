# 🚀 TarotMind Quick Start Guide
## 개발 환경 설정 및 시작 가이드

---

## 📦 Prerequisites

### Required Software
```bash
# Unity Hub & Unity Editor
Unity Hub --version  # 3.x.x
Unity Editor 2022.3 LTS

# Visual Studio 2022 (Windows) or Rider (Cross-platform)
# Xcode 14+ (macOS, for iOS builds)

# Node.js 20 LTS (Backend & Web)
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
git clone https://github.com/tarotmind/unity-client.git
git clone https://github.com/tarotmind/frontend-web.git

# Backend setup
cd backend
npm install
cp .env.example .env
# Edit .env with your local settings

# Unity Client setup
cd ../unity-client
# Open with Unity Hub → Select Unity 2022.3 LTS
# Unity will automatically resolve Package Manager dependencies
# Install required packages:
#   - Unity UI Toolkit
#   - DOTween Pro (Asset Store)
#   - Unity IAP
#   - AR Foundation
#   - Firebase SDK

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

#### Unity Client (Assets/Resources/config.json)
```json
{
  "API_URL": "http://localhost:3000",
  "ENABLE_DEV_MODE": true,
  "PLATFORM": "Development"
}
```

또는 Unity Build Settings → Player Settings → Scripting Define Symbols:
```
DEVELOPMENT_BUILD;ENABLE_DEBUG_LOGS
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

# Terminal 2: Unity Client
# Open Unity Hub → Open Project → unity-client
# Unity Editor에서 Play 버튼 클릭 (Editor Play Mode)
#
# 실기기 테스트:
# - iOS: File → Build Settings → iOS → Build and Run (Xcode 필요)
# - Android: File → Build Settings → Android → Build and Run (Android SDK 필요)

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

# Unity Client
# Unity Editor 내에서:
# - Play Mode: Ctrl/Cmd + P
# - Build: Ctrl/Cmd + B
# - Run Tests: Window → General → Test Runner

# Unity CLI (자동화용)
Unity -quit -batchmode -projectPath ./unity-client -executeMethod BuildScript.BuildIOS
Unity -quit -batchmode -projectPath ./unity-client -executeMethod BuildScript.BuildAndroid
Unity -quit -batchmode -projectPath ./unity-client -runTests -testPlatform playmode

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

## 🎮 Unity Development Tips

### iOS Build (macOS only)

```bash
# Unity에서 iOS 빌드 후 Xcode 프로젝트 생성
# File → Build Settings → iOS → Build

# Xcode에서 프로젝트 열기
cd Builds/iOS
open Unity-iPhone.xcodeproj

# CocoaPods 의존성 설치 (Firebase 등)
pod install
open Unity-iPhone.xcworkspace

# 실기기에 배포
# Xcode → Product → Destination → Your Device
# Xcode → Product → Run
```

### Android Build

```bash
# Unity에서 Android 빌드
# File → Build Settings → Android → Build

# Gradle 빌드 (CLI)
cd Builds/Android
./gradlew assembleDebug

# APK 설치
adb install -r app-debug.apk

# 로그 확인
adb logcat Unity:D *:S
```

### Unity Debugging

```csharp
// Unity Console 로그
Debug.Log("일반 로그");
Debug.LogWarning("경고");
Debug.LogError("에러");

// 조건부 컴파일
#if UNITY_EDITOR
    Debug.Log("에디터에서만 실행");
#endif

// Visual Studio Debugger 연결
// Unity → Edit → Preferences → External Tools → Visual Studio
// Visual Studio → Debug → Attach Unity Debugger

// Unity Profiler
// Window → Analysis → Profiler
// Play Mode에서 성능 분석
```

### Unity Performance Tips

```csharp
// Object Pooling (빈번한 생성/삭제 방지)
public class ObjectPool : MonoBehaviour {
    private Queue<GameObject> pool = new Queue<GameObject>();

    public GameObject GetObject() {
        if (pool.Count > 0) {
            return pool.Dequeue();
        }
        return Instantiate(prefab);
    }

    public void ReturnObject(GameObject obj) {
        obj.SetActive(false);
        pool.Enqueue(obj);
    }
}

// Coroutine으로 프레임 분산
IEnumerator LoadCardsAsync() {
    foreach (var card in cards) {
        LoadCard(card);
        yield return null; // 다음 프레임으로 넘김
    }
}
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

# Unity (iOS)
# Unity Editor:
# 1. File → Build Settings → iOS
# 2. Player Settings:
#    - Bundle Identifier: com.tarotmind.app
#    - Version: 1.0.0
#    - IL2CPP, ARM64
# 3. Build → Generate Xcode Project
# 4. Xcode에서 Archive → Upload to App Store Connect

# Unity (Android)
# Unity Editor:
# 1. File → Build Settings → Android
# 2. Player Settings:
#    - Package Name: com.tarotmind.app
#    - Version: 1.0.0
#    - IL2CPP, ARM64
#    - Keystore 설정
# 3. Build App Bundle (AAB)
# 4. Google Play Console에 업로드

# Unity (WebGL)
# Unity Editor:
# 1. File → Build Settings → WebGL
# 2. Build
# 3. Output을 웹 서버에 배포

# Web (Next.js)
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
- [Unity Docs](https://docs.unity3d.com/Manual/index.html)
- [NestJS Docs](https://docs.nestjs.com)
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

#### Unity iOS Build Failed
```bash
# Unity Library 폴더 재생성
rm -rf Library/
# Unity Editor 재시작 후 프로젝트 다시 열기

# Xcode DerivedData 삭제
rm -rf ~/Library/Developer/Xcode/DerivedData

# CocoaPods 재설치
cd Builds/iOS
pod deintegrate
pod cache clean --all
pod install
```

#### Unity Android Build Failed
```bash
# Unity Library 폴더 재생성
rm -rf Library/

# Gradle 캐시 삭제
rm -rf ~/.gradle/caches/

# Unity Editor:
# Edit → Preferences → External Tools
# - JDK, SDK, NDK 경로 재설정
```

#### Unity General Issues
```bash
# Package Manager 오류
# Window → Package Manager → 우측 상단 톱니바퀴 → Reset Packages

# 컴파일 오류
# Assets → Reimport All

# Scene이 열리지 않음
# Assets → Refresh (Ctrl/Cmd + R)
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
