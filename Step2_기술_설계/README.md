# Step 2: 기술 설계

## 📍 이 폴더의 목적

TarotMind의 **시스템 아키텍처, 기술 스택, 데이터베이스 설계, API 설계**를 다룹니다.
개발을 시작하기 전에 반드시 이해해야 할 기술적 청사진입니다.

---

## 📄 문서 목록

### 01_타로앱_개발기획서.md ⭐⭐⭐⭐⭐
**읽기 시간**: 3시간
**대상**: 개발팀, CTO, 기술 리드

**주요 내용**:
1. **시스템 아키텍처**
   - 마이크로서비스 아키텍처
   - Frontend + Backend + AI/ML 분리

2. **기술 스택**
   - Frontend: React Native, Next.js
   - Backend: Node.js + NestJS, GraphQL + REST
   - Database: PostgreSQL 15, Redis 7, Elasticsearch
   - Cloud: AWS (EKS, RDS, S3, CloudFront)
   - AI/ML: OpenAI GPT-4 API, TensorFlow

3. **데이터베이스 설계**
   - 사용자, 타로카드, 리딩, 구독 테이블 스키마
   - 인덱싱 전략

4. **API 설계**
   - RESTful API 엔드포인트
   - GraphQL 스키마
   - 인증/인가 (JWT)

5. **AI/ML 시스템**
   - GPT-4 API 통합
   - 타로 해석 엔진
   - 추천 시스템

6. **보안 및 인프라**
   - SSL/TLS 암호화
   - AWS WAF
   - 백업 전략

7. **개발 프로세스**
   - Git Flow
   - CI/CD (GitHub Actions)
   - 코드 리뷰

8. **테스트 전략**
   - 단위 테스트 (Jest)
   - 통합 테스트
   - E2E 테스트 (Cypress)

9. **배포 및 운영**
   - Kubernetes 기반
   - 모니터링 (Prometheus, Grafana)
   - 로깅 (ELK Stack)

---

## 🛠️ 기술 스택 요약

| 계층 | 기술 |
|------|------|
| **모바일** | React Native |
| **웹** | Next.js 14, TypeScript |
| **백엔드** | Node.js 20, NestJS, GraphQL |
| **데이터베이스** | PostgreSQL 15, Redis 7 |
| **AI** | OpenAI GPT-4, TensorFlow |
| **클라우드** | AWS (EKS, RDS, S3) |
| **DevOps** | Docker, Kubernetes, GitHub Actions |

---

## 📐 아키텍처 다이어그램

```
[Mobile App (React Native)]
          ↓↑
[Web App (Next.js)]
          ↓↑
[API Gateway (GraphQL + REST)]
          ↓↑
┌─────────┴─────────┬──────────┬──────────┐
│                   │          │          │
[Auth Service]  [Tarot Service]  [Payment]  [AI/ML]
│                   │          │          │
└─────────┬─────────┴──────────┴──────────┘
          ↓↑
[PostgreSQL + Redis + Elasticsearch]
```

---

## 🚀 다음 단계

기술 설계를 이해한 후:
1. **UX/UI**: `Step3_UX_UI_디자인`으로 이동
2. **개발 시작**: `Step4_개발_실행`으로 이동
