
# 🎬 YT During Outage

**یک ابزار محلی تحلیل HAR و دروازه HTTP سبک برای ترافیک YouTube، ساخته‌شده به‌صورت یک فایل Python تک‌واحدی با Flask.**

[![مجوز: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![Python 3.6+](https://img.shields.io/badge/python-3.6+-blue.svg)](https://www.python.org/downloads/)
[![Flask](https://img.shields.io/badge/Flask-2.0+-green.svg)](https://flask.palletsprojects.com/)

---

## 📖 نمای کلی

**YT During Outage** یک ابزار جامع و خودکفا برای:

- **وارد کردن** فایل‌های `.har` (HTTP Archive) گرفته‌شده از Chrome DevTools.
- **تحلیل** درخواست‌های شبکه مرتبط با YouTube و تشخیص بخش‌های ویدئو، ویدئوهای کوتاه (Shorts)، ویدئوهای بلند، رزولوشن‌ها و زمان تماشای تخمینی.
- **پاک‌سازی** داده‌های حساس (کوکی‌ها، هدرهای احراز هویت، پارامترهای پرس‌وجو، Set-Cookie) از فایل‌های HAR با پیش‌نمایش و فیلتر دامنه.
- **بازپخش** پاسخ‌های ذخیره‌شده به‌صورت محلی از طریق یک دروازه HTTP داخلی (حالت YouTube-Only) برای ارائه منابع ضبط‌شده قبلی زمانی که اتصال زنده محدود است.

> ⚠️ **مهم:** فایل HAR فقط یک **ثبت** از فعالیت شبکه گذشته است؛ **به‌تنهایی** نمی‌تواند دسترسی جدید به اینترنت ایجاد کند یا قطعی کامل شبکه را دور بزند. دروازه فقط می‌تواند منابعی را بازپخش کند که قبلاً در HAR آپلودشده وجود دارند و از شبکه محلی شما قابل دسترسی هستند.

---

## ✨ ویژگی‌ها

- **برنامه تک‌فایلی** – همه چیز (بک‌اند، فرانت‌اند، CSS، JS، SVG) در `youtube_har_gateway.py` جاسازی شده است. هیچ قالب خارجی، فایل استاتیک یا پیکربندی جداگانه‌ای نیاز نیست.
- **رابط کاربری فارسی** – پشتیبانی کامل از RTL با برچسب‌ها، دکمه‌ها، پیام‌های خطا، راهنمای ابزار و مستندات راهنمای جامع به فارسی.
- **نصب خودکار وابستگی‌ها** – بررسی Flask و نصب خودکار در صورت عدم وجود، با استفاده از PyPI رسمی و سرورهای آینه پشتیبان (Liara، Runflare، Abrha).
- **تحلیل هوشمند HAR** – تشخیص دامنه‌های YouTube، googlevideo، ytimg، ggpht، fonts.googleapis.com و سایر دامنه‌های مرتبط؛ دسته‌بندی درخواست‌ها و شناسایی بخش‌های ویدئو.
- **پیش‌بینی ویدئو** – تخمین:
  - تعداد ویدئوهای کوتاه (Shorts) در مقابل ویدئوهای بلند
  - حجم کل داده ویدئو
  - زمان تماشای تخمینی (دقیقه)
  - رزولوشن غالب (۱۴۴p تا ۴K) و سطح کیفیت
- **پاک‌سازی پیشرفته** – حذف کوکی‌ها، هدرهای احراز هویت، Set-Cookie، هدرهای حساس سفارشی و پارامترهای پرس‌وجو؛ فیلتر بر اساس دامنه (فقط YouTube، لیست سفارشی، یا نگهداری همه).
- **دروازه HTTP محلی** – ارائه پاسخ‌های ذخیره‌شده از HAR هنگام فعال‌سازی START؛ به‌طور پیش‌فرض در حالت YouTube-Only کار می‌کند (دامنه‌های غیرمرتبط را مسدود می‌کند).
- **داشبورد زنده** – نمایش تعداد درخواست‌ها، تفکیک YouTube/googlevideo، پهنای باند مصرفی، میانگین زمان پاسخ، تشخیص داده حساس و وضعیت فونت.
- **حالت تاریک/روشن** – ذخیره‌شده در `localStorage`؛ تغییر بدون بارگذاری مجدد صفحه.
- **طراحی واکنش‌گرا** – کار بر روی دسکتاپ، تبلت و موبایل (ناوبری پایین در صفحه‌های کوچک).
- **گزارش قابل خروجی** – تولید خلاصه متنی از تمام نتایج تحلیل، شامل هاست‌های برتر، متدهای HTTP، کدهای وضعیت و پیش‌بینی‌های ویدئو.
- **پاک‌سازی سریع** – یک‌کلیک پاک‌سازی با تنظیمات پیش‌فرض ایمن، همراه با دانلود فوری فایل پاک‌شده.

---

## 🚀 نصب و اجرا

### پیش‌نیازها
- پایتون ۳٫۶ یا بالاتر
- اتصال به اینترنت (فقط برای نصب اولیه وابستگی‌ها و دانلود اختیاری فونت)

### مراحل

1. **دانلود فایل واحد**  
   فایل `youtube_har_gateway.py` را در پوشه دلخواه خود ذخیره کنید.

2. **اجرای برنامه**  
   ```bash
   python youtube_har_gateway.py
   ```
   - اسکریپت به‌طور خودکار Flask را بررسی کرده و در صورت نیاز نصب می‌کند.
   - در اولین اجرا، سعی می‌کند فونت **وزیر بولـد** (Vazir Bold) را از انتشار رسمی گیت‌هاب دانلود کرده و در `runtime/fonts/` ذخیره کند تا به‌صورت آفلاین در دسترس باشد. اگر دانلود ناموفق باشد، از Google Fonts و فونت‌های سیستمی استفاده می‌کند.

3. **باز کردن رابط وب**  
   سرور به‌طور پیش‌فرض روی `http://0.0.0.0:8080` اجرا می‌شود. مرورگر پیش‌فرض شما به‌طور خودکار باز می‌شود.  
   می‌توانید پورت را در قسمت **تنظیمات** تغییر دهید.

4. **شروع استفاده**  
   - یک فایل `.har` را آپلود کنید (با کشیدن یا کلیک برای انتخاب).
   - منتظر بمانید تا تحلیل کامل شود – آمار و پیش‌بینی‌های ویدئو نمایش داده می‌شوند.
   - از دکمه **START** برای راه‌اندازی دروازه محلی استفاده کنید.
   - مرورگر خود را برای استفاده از `http://127.0.0.1:8080` به‌عنوان پروکسی HTTP تنظیم کنید (اختیاری؛ دروازه به‌عنوان یک سرور محلی مستقل نیز کار می‌کند).

---

## 🧩 معماری

```
┌─────────────────────────────────────────────────────────────┐
│                    برنامه تک‌فایلی Flask                    │
│                   youtube_har_gateway.py                    │
├─────────────────────────────────────────────────────────────┤
│  ┌─────────────┐   ┌─────────────┐   ┌─────────────────┐  │
│  │  تحلیل‌گر    │   │  تحلیل‌گر   │   │  پاک‌ساز HAR    │  │
│  │  HAR         │   │  (دسته‌بندی،│   │  (حذف داده‌های │  │
│  │  (ساختار JSON)│   │  پیش‌بینی)  │   │   حساس)        │  │
│  └─────────────┘   └─────────────┘   └─────────────────┘  │
│                                                             │
│  ┌─────────────────────────────────────────────────────┐   │
│  │           دروازه HTTP محلی (با کش)                   │   │
│  │  ┌──────────┐  ┌──────────┐  ┌──────────────────┐ │   │
│  │  │ مدیریت  │  │ YouTube  │  │ مسدودسازی دامنه‌های│ │   │
│  │  │ کش       │  │ Only     │  │ غیرمرتبط (در صورت │ │   │
│  │  └──────────┘  └──────────┘  │  فعال بودن)      │ │   │
│  │                              └──────────────────┘ │   │
│  └─────────────────────────────────────────────────────┘   │
│                                                             │
│  ┌─────────────────────────────────────────────────────┐   │
│  │              پیکربندی و لاگ‌نویسی                    │   │
│  │  runtime/config.json + runtime/logs/app.log         │   │
│  └─────────────────────────────────────────────────────┘   │
│                                                             │
│  ┌─────────────────────────────────────────────────────┐   │
│  │      رابط کاربری جاسازی‌شده (HTML + CSS + JS + SVG)│   │
│  │  رندر شده با Flask.render_template_string()        │   │
│  └─────────────────────────────────────────────────────┘   │
└─────────────────────────────────────────────────────────────┘
```

---

## 🔌 API‌ها

| متد | مسیر | توضیح |
|-----|------|-------|
| POST | `/api/upload` | آپلود فایل `.har` (multipart/form-data) |
| GET  | `/api/analyze` | دریافت نتایج تحلیل برای HAR آپلودشده |
| POST | `/api/clean` | پاک‌سازی HAR بر اساس گزینه‌های ارائه‌شده (بدنه JSON) |
| POST | `/api/quick_clean` | پاک‌سازی با تنظیمات پیش‌فرض ایمن و دریافت فایل پاک‌شده |
| GET  | `/api/download/<file>` | دانلود یک فایل HAR پاک‌شده |
| POST | `/api/gateway/start` | راه‌اندازی دروازه HTTP محلی |
| POST | `/api/gateway/stop` | توقف دروازه HTTP محلی |
| GET  | `/api/gateway/status` | دریافت وضعیت دروازه (در حال اجرا/متوقف) |
| GET  | `/api/stats` | دریافت آمار جاری (تعداد کل، دسته‌بندی‌ها، پهنای باند و ...) |
| GET  | `/api/requests` | لیست صفحه‌بندی‌شده و قابل فیلتر درخواست‌ها (پشتیبانی از `search`، `domain`، `status`، `type`، `page`، `per_page`) |
| GET  | `/api/export_report` | تولید یک خلاصه متنی از نتایج تحلیل |
| POST | `/api/settings` | به‌روزرسانی پیکربندی (بدنه JSON) |
| POST | `/api/clear_session` | پاک‌سازی HAR آپلودشده و وضعیت تحلیل |
| POST | `/api/cleanup` | حذف فایل‌های موقت (آپلودها، پاک‌شده‌ها، کش، لاگ‌ها) |

همه پاسخ‌های JSON از ساختار زیر پیروی می‌کنند:
```json
{
  "success": true,
  "message": "...",
  "data": { ... }
}
```
خطاها با `"success": false` و یک فیلد `"error"` توضیحی بازگردانده می‌شوند.

---

## 🛠 تنظیمات

تنظیمات در `runtime/config.json` ذخیره می‌شوند و می‌توان آن‌ها را از طریق بخش تنظیمات رابط کاربری یا با ویرایش مستقیم فایل تغییر داد. گزینه‌های موجود:

| کلید | نوع | مقدار پیش‌فرض | توضیح |
|------|-----|---------------|--------|
| `gateway_host` | string | `0.0.0.0` | هاست برای اتصال دروازه |
| `gateway_port` | integer | `8080` | پورت دروازه |
| `upload_max_size_mb` | integer | `100` | حداکثر حجم فایل HAR بر حسب مگابایت |
| `cache_enabled` | boolean | `true` | فعال‌سازی کش پاسخ‌ها از HAR |
| `cache_max_size_mb` | integer | `500` | حداکثر حجم کش بر حسب مگابایت |
| `youtube_only_mode` | boolean | `true` | مسدودسازی دامنه‌های غیرمرتبط با YouTube در حالت دروازه |
| `custom_allowed_domains` | array | `[]` | دامنه‌های اضافی برای اجازه در حالت YouTube-Only |
| `auto_open_browser` | boolean | `true` | باز کردن خودکار مرورگر در هنگام راه‌اندازی |
| `theme` | string | `light` | `light` یا `dark` |
| `log_level` | string | `INFO` | سطح لاگ‌نویسی Python |
| `remove_headers` | array | `['cookie','authorization','proxy-authorization','set-cookie']` | هدرهایی که در زمان پاک‌سازی حذف می‌شوند |
| `remove_query_params` | array | `['utm_source','utm_medium','utm_campaign','sid','token','auth']` | پارامترهای پرس‌وجویی که در زمان پاک‌سازی حذف می‌شوند |

---

## 📦 تکنولوژی‌های استفاده‌شده

- **بک‌اند:** Python 3.6+، Flask 2.0+
- **فرانت‌اند:** JavaScript خالص، HTML5، CSS3 (طراحی شیشه‌ای، شبکه‌های واکنش‌گرا)
- **فونت‌ها:** Google Fonts (Vazirmatn) + فونت آفلاین (Vazir Bold دانلود شده در `runtime/fonts/`)
- **ذخیره‌سازی:** فایل‌های JSON (دایرکتوری runtime برای پیکربندی، کش، آپلودها، HARهای پاک‌شده، لاگ‌ها)
- **بدون وابستگی خارجی غیر از Flask** – بقیه کتابخانه‌ها از کتابخانه استاندارد Python هستند.

---

## 📜 مجوز

این پروژه تحت مجوز **MIT** منتشر شده است – برای جزئیات بیشتر به فایل [LICENSE](https://github.com/CodeNev/YT-During-Outage/blob/main/LICENSE) مراجعه کنید.

```
MIT License

Copyright (c) 2026 CodeNev

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.
```

---

## 🤝 مشارکت

مشارکت شما خوش‌آمد است! لطفاً این مراحل را دنبال کنید:

1. مخزن را فورک کنید.
2. یک شاخه ویژگی ایجاد کنید (`git checkout -b feature/your-feature`).
3. تغییرات خود را commit کنید (`git commit -m 'Add some feature'`).
4. به شاخه push کنید (`git push origin feature/your-feature`).
5. یک درخواست Pull باز کنید.

برای گزارش باگ یا درخواست ویژگی، از [صفحه Issues](https://github.com/CodeNev/YT-During-Outage/issues) استفاده کنید.

---

## 📄 قدردانی

- **فونت وزیر** توسط صابر راستیکردار – استفاده‌شده تحت دامنه عمومی.
- **Google Fonts** – Vazirmatn به‌عنوان فونت جایگزین تحت وب.
- **Flask** – چارچوب وب WSGI سبک.

---

## 🔗 لینک‌ها

- **مخزن گیت‌هاب:** [https://github.com/CodeNev/YT-During-Outage](https://github.com/CodeNev/YT-During-Outage)
- **گزارش مشکلات:** [https://github.com/CodeNev/YT-During-Outage/issues](https://github.com/CodeNev/YT-During-Outage/issues)

---

*ساخته‌شده با ❤️ برای تحلیل آفلاین YouTube و اهداف آموزشی.*
```
