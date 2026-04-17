# 🍽 Qazaq Catering Bot

> Production-ready Telegram bot for a catering business in Shymkent, Kazakhstan.  
> Built with **aiogram 3.x**, **PostgreSQL (Neon.tech)**, **SQLAlchemy 2.0**, **Gemini AI**.

![Python](https://img.shields.io/badge/Python-3.11+-blue?logo=python)
![Aiogram](https://img.shields.io/badge/Aiogram-3.7-blue?logo=telegram)
![PostgreSQL](https://img.shields.io/badge/PostgreSQL-Neon.tech-green?logo=postgresql)
![License](https://img.shields.io/badge/License-MIT-green)

---

## 📸 Preview

| Start | Menu | Cart | Order |
|-------|------|------|-------|
| `/start` → welcome + buttons | Categories → Products | Add by qty or guests | Checkout FSM → WhatsApp |

---

## ✨ Features

### 👤 User Side
- **Welcome screen** with inline keyboard
- **Catalog** — categories + paginated products
- **Dual calculator** — add by quantity (штуки) or by guest count (по гостям)
- **Smart cart** — persistent DB-based, multiple items
- **Checkout FSM** — name → date → time → location → confirm
- **WhatsApp link** — auto-generated order summary
- **Order history** — view past orders, repeat with one tap
- **Review system** — auto-sent 1 day after event
- **AI Assistant** — Gemini 1.5 Flash for FAQ & recommendations

### 👨‍💼 Admin Panel (`/admin`)
- **Category management** — add / edit / delete
- **Product management** — photo, name, description, price, serving factor, stop-list toggle
- **Order management** — view, confirm, mark done
- **Broadcast** — send message to all active users, auto-marks blocked
- **Excel reports** — last 30 days: revenue, top products, avg check (pandas + openpyxl)

---

## 🏗 Architecture

```
qazaq_pro/
├── bot.py                    # Entry point
├── states.py                 # FSM states
├── config/
│   ├── settings.py           # pydantic-settings (.env loader)
│   └── logging_setup.py      # Structured logging
├── models/
│   ├── base.py               # SQLAlchemy engine + session
│   └── models.py             # 7 ORM tables
├── repositories/             # Data access layer (no SQL in handlers)
│   ├── user_repo.py
│   ├── product_repo.py
│   ├── cart_repo.py
│   └── order_repo.py
├── services/                 # Business logic
│   ├── cart_service.py       # Dual calc engine
│   ├── order_service.py      # Checkout, validation, repeat
│   ├── ai_service.py         # Gemini integration
│   └── report_service.py     # Excel generation
├── handlers/                 # Thin handlers — no business logic
│   ├── common.py
│   ├── menu.py
│   ├── cart.py
│   ├── checkout.py
│   ├── orders.py
│   ├── review.py
│   ├── admin.py
│   └── broadcast.py
├── keyboards/
│   ├── user_kb.py
│   └── admin_kb.py
├── middlewares/
│   ├── db_middleware.py      # Session injection
│   ├── user_middleware.py    # Auto user registration
│   └── error_middleware.py   # Global error handling
└── scheduler/
    └── review_scheduler.py   # APScheduler — review requests
```

---

## 🗄 Database Schema

```sql
users        — Telegram users, is_active, is_blocked
categories   — Menu categories with emoji
products     — Items with price, serving_factor, stop-list flag
cart_items   — Persistent cart (manual or per-person mode)
orders       — Full order with event_date, location, status
order_items  — Snapshot of products at order time
reviews      — Rating + text, review_sent flag
```

---

## ⚙️ Calculation Engine

```python
# Per-person mode
quantity = ceil(guests / serving_factor)
# e.g. 50 guests, serving_factor=10 → 5 units needed

# Manual mode
quantity = user_input
```

---

## 🚀 Quick Start

### 1. Clone & configure

```bash
git clone https://github.com/yourname/qazaq-catering-bot.git
cd qazaq-catering-bot
cp .env.example .env
nano .env  # Fill in your values
```

### 2. Install dependencies

```bash
python3 -m venv venv
source venv/bin/activate
pip install -r requirements.txt
```

### 3. Run

```bash
python bot.py
```

### 4. Deploy with systemd (VPS)

```bash
cp qazaq_pro.service /etc/systemd/system/
systemctl daemon-reload
systemctl enable qazaq_pro
systemctl start qazaq_pro
```

---

## 🔧 Environment Variables

```env
BOT_TOKEN=          # BotFather token
DATABASE_URL=       # postgresql+asyncpg://user:pass@host/db?ssl=require
GEMINI_KEY=         # Google AI Studio API key
ADMIN_ID=           # Your Telegram user ID
ADMIN_PHONE=        # Phone for contact button
ADMIN_WHATSAPP=     # WhatsApp number for order links
```

---

## 📦 Tech Stack

| Layer | Technology |
|-------|-----------|
| Bot framework | aiogram 3.7 |
| Database | PostgreSQL (Neon.tech) |
| ORM | SQLAlchemy 2.0 async |
| DB driver | asyncpg |
| AI | Google Gemini 1.5 Flash |
| Scheduler | APScheduler 3.x |
| Reports | pandas + openpyxl |
| Config | pydantic-settings |

---

## 📄 License

MIT — free to use and modify.
