# 🤖 Number Bot — main.py + main.pyc (বাংলা গাইড)

এই ফোল্ডারে **৪টা ফাইল** — এতটুকুই লাগবে। আসল কোড `main.pyc`-তে বাইটকোড হিসেবে আছে, কেউ খুলে পড়তে বা এডিট করতে পারবে না।

```
আপনার-ফোল্ডার/
├── main.py             ← এটাই চালাবেন:  python main.py
├── main.pyc            ← আসল কোড (কম্পাইল করা, পড়া যায় না)
├── .env                ← আপনি বানাবেন (২ লাইন) — .env.example থেকে কপি
├── requirements.txt
├── .gitignore          (ঐচ্ছিক, GitHub-এ কাজে লাগে)
└── data/               ← বট নিজেই বানাবে (balance, users, withdrawals)
```

> ℹ️ `main.py`-এর ভেতরে একটা **এনক্রিপ্টেড ব্যাকআপ**ও আছে — কোনো কারণে `main.pyc` কাজ না করলে (ভিন্ন Python ভার্সন, ফাইল নষ্ট) বট তখনো চলবে।

---

## ১. ৩টা ধাপে চালু

### ধাপ ১ — `.env` বানান
`.env.example` কপি করে নাম দিন `.env`, তারপর **শুধু ২ লাইন**:

```env
TELEGRAM_BOT_TOKEN=আপনার_বট_টোকেন
OWNER_ID=আপনার_টেলিগ্রাম_আইডি
```

> `=` এর দুই পাশে স্পেস নয়, কোটেশন নয়। `.env` ফাইল `main.py`-এর **পাশে** থাকবে।

### ধাপ ২ — লাইব্রেরি ইনস্টল
```bash
pip install -r requirements.txt
```

### ধাপ ৩ — চালান
```bash
python main.py
```

সফল হলে:
```
✅ PERMISSION OK — BOT IS STARTING
```
আর পাশে **`data`** ফোল্ডার নিজে থেকেই তৈরি হবে।

---

## ২. প্ল্যাটফর্ম অনুযায়ী

### 📱 Termux (Android)
```bash
pkg update && pkg upgrade -y
pkg install python -y
termux-setup-storage
# ফোল্ডারটা Termux-এ কপি করে
cd ~/Number-Bot
pip install -r requirements.txt
python main.py
```
চালু রাখতে: `termux-wake-lock` + `nohup python main.py &`

### 🚂 Railway
1. ফোল্ডার GitHub-এ পুশ করুন (`.env` পুশ হবে না — `.gitignore` আটকে দেবে)।
2. New Project → Deploy from GitHub repo।
3. **Variables**: `TELEGRAM_BOT_TOKEN`, `OWNER_ID`
4. **Start Command**: `python main.py`
5. (ঐচ্ছিক) `PYTHON_VERSION=3.13`

### 🎨 Render
1. New → Web Service → রিপো কানেক্ট।
2. **Build Command**: `pip install -r requirements.txt`
3. **Start Command**: `python main.py`
4. **Environment**: `TELEGRAM_BOT_TOKEN`, `OWNER_ID`
5. ফ্রি সার্ভিস ঘুমিয়ে পড়ে — UptimeRobot দিয়ে `https://আপনার-অ্যাপ.onrender.com` পিং করুন (বট পোর্ট **8000**-এ keep-alive সার্ভার চালায়)।

### 🐍 PythonAnywhere
1. **Files** → ফোল্ডার/zip আপলোড → Bash-এ `unzip`
2. ```bash
   cd ~/Number-Bot
   pip install --user -r requirements.txt
   python main.py
   ```
3. ২৪/৭ চালাতে: **Tasks** → Always-on task → `cd ~/Number-Bot && python main.py`

---

## ৩. সমস্যা হলে

| কনসোলে যা দেখে | কারণ | সমাধান |
|---|---|---|
| `লাইব্রেরি ইনস্টল করা নেই` | pip হয়নি | `pip install -r requirements.txt` |
| `বট শুরু করা যায়নি` + RuntimeError | `.env` ভুল | টোকেন/আইডি আবার দেখুন |
| `কোড লোড করা যায়নি` | `main.py` আর `main.pyc` আলাদা জায়গায় | দুটো একসাথে রাখুন |
| `PERMISSION DENIED` | `permission.txt`-এ `no` | অ্যাডমিনের সাথে যোগাযোগ |
| `PERMISSION CHECK FAILED` | ইন্টারনেট/লিংক সমস্যা | বট **চলবে**, শুধু সতর্কবার্তা |

---

## ৪. পারবেন / পারবেন না

| ✅ পারবেন | ❌ পারবেন না |
|---|---|
| `.env`-এ টোকেন/আইডি বদলানো | কোডের ভেতরে এডিট |
| admin panel থেকে সব সেটিংস | `main.py`-তে নতুন ফিচার লেখা |
| `data` ফোল্ডার ব্যাকআপ করা | `main.pyc` পড়া বা বদলানো |
| নতুন রিলিজ দিয়ে রিপ্লেস | — |

### আপডেট করার সময় ⚠️
নতুন `main.py` + `main.pyc` **রিপ্লেস** করুন — `.env` আর `data/` **রেখে দিন**। নাহলে balance/users হারাবে।
(নিরাপত্তার জন্য admin panel → 📥 Backup ডাউনলোডও রাখুন।)

---

## ৫. নিরাপত্তা

- 🔒 কোড বাইটকোড (`main.pyc`) — সাধারণভাবে পড়া/এডিট অসম্ভব।
- ⚠️ ১০০% "অদৃশ্য" নয় — খুব দক্ষ কেউ চেষ্টা করলে বের করতে পারে; সাধারণ কপি/চুরি ঠেকাতে যথেষ্ট।
- 🔑 `.env` কখনো GitHub-এ পুশ করবেন না (`.gitignore` আটকে রাখবে)।
