# ⚡ Product AI Studio

> Upload a product image with white background → Get 9 professional photography styles using AI

---

## 🏗️ Architecture

```
product-ai-studio/
├── backend/          # FastAPI + Python
│   ├── app/
│   │   ├── main.py
│   │   ├── db.py
│   │   ├── models/       # SQLAlchemy models
│   │   ├── routers/      # auth, images, admin, users
│   │   ├── services/     # auth_service, image_service
│   │   └── middleware/   # rate limiting
│   └── tests/            # pytest unit tests (90%+ coverage)
├── frontend/         # React + Vite
│   ├── src/App.jsx       # Full SPA
│   └── tests/            # Playwright E2E tests
└── .github/workflows/    # CI/CD automation
```

---

## 🚀 Quick Start

### Backend
```bash
cd backend
pip install -r requirements.txt
cp .env.example .env   # fill in your keys
python -c "from app.db import create_tables; create_tables()"
uvicorn app.main:app --reload
# → http://localhost:8000
# → http://localhost:8000/docs  (Swagger UI)
```

### Frontend
```bash
cd frontend
npm install
cp .env.example .env
npm run dev
# → http://localhost:5173
```

---

## 🔐 Authentication Features

| Feature | Status |
|---------|--------|
| Email + Password (JWT) | ✅ |
| TOTP 2FA (Google Authenticator) | ✅ |
| Google OAuth | ✅ |
| GitHub OAuth | ✅ |
| Refresh Tokens | ✅ |
| Rate Limiting | ✅ |

### Social Login Setup
1. **Google**: Create OAuth app at [console.cloud.google.com](https://console.cloud.google.com) → add `GOOGLE_CLIENT_ID` + `GOOGLE_CLIENT_SECRET`
2. **GitHub**: Create OAuth app at github.com/settings/applications/new → add `GITHUB_CLIENT_ID` + `GITHUB_CLIENT_SECRET`

---

## 👥 User Roles & Permissions

| Role | Monthly Limit | Admin Dashboard | Notes |
|------|--------------|-----------------|-------|
| `user` | 50 generations | ❌ | Default |
| `pro` | 500 generations | ❌ | Upgraded by admin |
| `admin` | Unlimited | ✅ | Full control |

Admin dashboard allows **dynamic role changes** without redeployment.

---

## 🧪 Testing

### Unit Tests (Backend) — 90%+ Coverage
```bash
cd backend
pytest tests/ -v --cov=app --cov-report=html --cov-fail-under=90
# Open htmlcov/index.html for coverage report
```

### E2E Tests (Playwright)
```bash
cd frontend
npm install
npx playwright install chromium
npx playwright test
npx playwright show-report
```

---

## ⚙️ CI/CD Pipeline (GitHub Actions)

Every push to `main` or PR triggers:
1. **Backend unit tests** with 90%+ coverage gate
2. **Frontend build** validation
3. **Playwright E2E tests** on real browser (Chromium + Mobile Chrome)
4. **Auto-deploy** to Railway (backend) + Vercel (frontend) on main merge

See `.github/workflows/ci.yml`

---

## 🎨 9 Generated Photography Styles

1. Studio White — clean commercial
2. Dark Luxury — premium moody
3. Marble Surface — lifestyle minimalist
4. Pastel Gradient — beauty editorial
5. Nature Flat Lay — organic brand
6. Neon Pop — bold consumer
7. Industrial Concrete — texture contrast
8. Gradient Sky — fresh e-commerce
9. Minimal Shadow — high fashion

---

## 📡 API Endpoints

```
POST /api/auth/register        # Create account
POST /api/auth/login           # Login (+ 2FA support)
POST /api/auth/google/callback # Google OAuth
POST /api/auth/github/callback # GitHub OAuth
POST /api/auth/2fa/setup       # Get QR code
POST /api/auth/2fa/verify      # Enable 2FA
GET  /api/auth/me              # Current user

POST /api/images/generate      # Upload & generate 9 images
GET  /api/images/generation/:id # Poll generation status
GET  /api/images/history       # User's past generations

GET  /api/admin/stats          # Dashboard stats (admin only)
GET  /api/admin/users          # List users (admin only)
PATCH /api/admin/users/:id     # Update role/status (admin only)
```

---

## 🔑 Environment Variables

See `backend/.env.example` and `frontend/.env.example`

Required for full functionality:
- `STABILITY_API_KEY` — image generation (Stability AI)
- `REMOVE_BG_API_KEY` — background removal
- `GOOGLE_CLIENT_ID/SECRET` — Google login
- `GITHUB_CLIENT_ID/SECRET` — GitHub login

> **Without API keys**: app runs in demo mode with color-variant mock images.
