### Shopping App Backend

Backend for a scalable full‑stack shopping application built with NestJS, Prisma and PostgreSQL. Implements JWT authentication, Stripe payments and webhooks, AWS S3 uploads, WebSocket real‑time events, and CI/CD-ready configuration.

Author: Stanislav Zakharchenko

---

### Technologies

- **Framework:** NestJS (TypeScript)  
- **ORM:** Prisma (PostgreSQL)  
- **Database:** PostgreSQL  
- **Authentication:** JWT (bcrypt for password hashing)  
- **Payments:** Stripe (webhooks)  
- **Storage:** AWS S3 (optional local filesystem)  
- **Realtime:** WebSockets (NestJS Gateway)  
- **CI/CD:** GitHub Actions (build/test), deploy to AWS / Vercel

---

### Project structure

```
src/
├── main.ts
├── app.module.ts
├── auth/           # JWT strategy, guards, auth service, DTOs
├── users/          # user service, controller, DTOs
├── products/       # product service, controller, DTOs
├── orders/         # order service, Stripe integration
├── uploads/        # file upload service (S3/local)
├── events/         # websocket gateways and event handlers
├── prisma/         # Prisma client usage, schema.prisma
└── common/         # pipes, interceptors, filters, utils
```

---

### Security

- **Route protection:** JwtAuthGuard on protected routes.  
- **Password handling:** bcrypt hashing and secure comparison.  
- **Input validation:** DTOs with class-validator and class-transformer.  
- **Role-based access:** Guards/Decorators for admin vs user actions.  
- **Webhook validation:** Verify Stripe signature on incoming webhooks.  
- **Secrets:** Keep JWT secret, DB URL, Stripe keys, and AWS credentials in environment variables.

---

### Local setup

1. Clone and install
```bash
git clone https://github.com/<your-username>/shopping-app-backend.git
cd shopping-app-backend
npm install
```

2. Environment
```bash
cp .env.example .env
# Set at minimum:
# DATABASE_URL=postgresql://user:pass@localhost:5432/dbname
# JWT_SECRET=your_jwt_secret
# STRIPE_SECRET=sk_xxx
# STRIPE_WEBHOOK_SECRET=whsec_xxx
# AWS_ACCESS_KEY_ID=...
# AWS_SECRET_ACCESS_KEY=...
# S3_BUCKET=...
```

3. Prisma and DB
```bash
npx prisma generate
npx prisma migrate dev --name init
# Optional seed
npx prisma db seed
```

4. Run
```bash
npm run start:dev
```

---

### Integrations & deployment

- **Stripe:** Create checkout sessions on the server, handle webhook events to confirm payments and update orders, validate webhook signature using STRIPE_WEBHOOK_SECRET. Emit order/product events to connected WebSocket clients on successful payment.  
- **File uploads:** Support local filesystem for development and AWS S3 for production. Validate MIME type and size server-side before saving. Use presigned URLs for direct-to-S3 uploads where appropriate.  
- **WebSockets:** Use NestJS Gateway to broadcast product additions and order status updates; authenticate socket connections using JWT.  
- **CI/CD:** GitHub Actions for test/build pipelines; use separate deployment workflows for AWS (backend) and Vercel (frontend). Ensure environment secrets are configured in CI and deployment targets. Use HTTPS and enforce secure headers in production.

---

### Useful scripts

- **Start dev:** npm run start:dev  
- **Build:** npm run build  
- **Test:** npm run test  
- **Prisma migrate:** npx prisma migrate dev  
- **Prisma generate:** npx prisma generate

---

### Contact

Open issues or pull requests on this repository. For direct questions, contact Stanislav Zakharchenko via the repository profile email.
