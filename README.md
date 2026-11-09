# 🛡️ M&F Panel (Katabump / Railway)

راه‌انداز بدون نیاز به root برای **M&F Panel** (مبتنی بر `vpn-ui v1.9.4`) با پرتال کاربری فارسی و داشبورد اشتراک.
نسخه‌ی فعلی: `mf-panel-1.2`

> ⚠️ **هشدار مهم درباره‌ی Railway**
> طبق سیاست استفاده‌ی Railway (Acceptable Use Policy)، اجرای سرویس‌های پروکسی/VPN ممنوع است و پشتیبانی Railway در
> پاسخ به پرسش‌های مشابه این کار را مجاز ندانسته است. استقرار روی Railway ممکن است باعث محدود شدن یا بسته شدن
> کل workspace شما شود. مسئولیت استفاده با خود شماست. اگر می‌خواهید ریسک نکنید، از VPS یا Katabump استفاده کنید.

## فایل‌ها
- `index.js` — هسته: پچ بررسی root، دانلود و بررسی هش باینری، مالتی‌پلکسر روی یک پورت، پرتال کاربران و ساب.
- `package.json` — بدون وابستگی خارجی؛ `npm start` و `npm test`.
- `renew.py` و `.github/workflows/renew.yml` — فقط برای تمدید خودکار **Katabump** (به Railway ربطی ندارد).

## مسیرها
| مسیر | کاربرد |
|---|---|
| `/` ، `/login` ، `/sub/<id>` | پرتال کاربران، ورود و لینک اشتراک |
| `/bp-vless` ، `/bp-vmess` ، `/bp-trojan` ، `/bp-up` | اینباندهای WebSocket / HTTPUpgrade |

مسیر قدیمی `/kb` غیرفعال است. مدیریت از داشبورد ادمین داخل همان پرتال انجام می‌شود (با حساب `admin`).
ثبت‌نام عمومی بسته است؛ کاربران را ادمین از تب Users می‌سازد.

## نصب روی Railway
1. پروژه‌ی جدید → **Deploy from GitHub repo** (Railway خودش `npm start` را اجرا می‌کند).
2. **Volume** اضافه کنید (مثلاً mount روی `/data`). برنامه به‌طور خودکار از `RAILWAY_VOLUME_MOUNT_PATH` استفاده می‌کند.
   بدون Volume، باینری ~۳۳۰ مگابایتی و کاربران با هر دیپلوی پاک می‌شوند.
3. در **Settings → Networking** فقط **Public Domain** بسازید و سپس یک‌بار دوباره Deploy کنید
   (دامنه از `RAILWAY_PUBLIC_DOMAIN` خوانده می‌شود). **TCP Proxy نسازید.**
4. در **Variables** مقدار `ADMIN_PASSWORD` را بگذارید. اگر نگذارید، رمز تصادفی ساخته و در لاگ چاپ می‌شود.
5. لینک‌ها خودکار به شکل `https://دامنه` ، پورت `443` ، `security=tls` ساخته می‌شوند.

### آنچه روی Railway کار می‌کند
VLESS-WS ، VMess-WS ، Trojan-WS ، VLESS-HTTPUpgrade.
اینباندهای TCP خام، gRPC، Shadowsocks و WireGuard (UDP) پشت لبه‌ی HTTP/TLS کار نمی‌کنند و لینکشان در حالت Railway داده نمی‌شود.

## نصب روی Katabump / VPS
فایل‌های `index.js` و `package.json` را آپلود/اجرا کنید. رمز اولیه در `vpn/PANEL.txt` و لاگ است
(پیش‌فرض `admin/admin` مگر اینکه `ADMIN_PASSWORD` بگذارید؛ **حتماً عوضش کنید**).

## متغیرهای محیطی
| متغیر | توضیح |
|---|---|
| `PORT` / `SERVER_PORT` | پورت عمومی (Railway خودش می‌دهد؛ پیش‌فرض ۸۰۸۰) |
| `PANEL_HOST` / `SERVER_IP` | دامنه یا IP عمومی برای لینک‌ها (روی Railway از `RAILWAY_PUBLIC_DOMAIN` خوانده می‌شود) |
| `ADMIN_USER` / `ADMIN_PASSWORD` | حساب ادمین اولیه |
| `KATABUMP_ROOT` | مسیر ذخیره‌سازی (روی Railway پیش‌فرض: مسیر Volume) |
| `USER_TRAFFIC_GB` / `USER_EXPIRY_DAYS` | سهمیه‌ی پیش‌فرض کاربران جدید (`0` = نامحدود) |
| `PUBLIC_TLS=1` | حالت TLS/443 را بیرون از Railway هم فعال می‌کند (پشت Cloudflare/Nginx) |
| `PUBLIC_LINK_PORT` | پورتی که در لینک‌ها نوشته می‌شود |

## تمدید خودکار Katabump (GitHub Actions)
`KATABUMP_EMAIL` ، `KATABUMP_PASSWORD` ، `SERVER_ID` را فقط در **Settings → Secrets and variables → Actions** بگذارید، نه داخل کد.

## تست
`npm test` (هم حالت عادی و هم با `RAILWAY_PROJECT_ID=x RAILWAY_PUBLIC_DOMAIN=demo.up.railway.app npm test`).

تلگرام: [@M_Fpanel](https://t.me/M_Fpanel)
