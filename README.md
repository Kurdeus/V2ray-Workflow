# V2ray-Workflow

‏*[فارسی](#فارسی) ↓*

Aggregates V2Ray subscription links from a private list of sources, health-checks
every config, and publishes only the ones that are actually alive and fast as a
ready-to-use subscription file — automatically, on a schedule, via GitHub Actions.

**Subscription link (import this in your client):**

```
https://raw.githubusercontent.com/<YOUR-USERNAME>/V2ray-Workflow/main/configs/healthy.txt
```

Replace `<YOUR-USERNAME>` with your actual GitHub username once you've created the repo.

## How it works

1. **Fetch** — reads a list of subscription URLs from the `BASE_URLS` repository
   secret (not a committed file, so your sources stay private even in a public
   repo) and downloads each one.
2. **Merge** — concatenates everything into one temporary list.
3. **Health-check** — tests every config with
   [`xray-knife`](https://github.com/lilendian0x00/xray-knife): a fast TCP
   pre-scan drops dead servers immediately, then a real connection test measures
   actual latency. Only configs with a real ping **under 250ms** survive.
4. **Publish** — writes the survivors to `configs/healthy.txt` and commits it
   back to the repo.

This runs automatically every hour (`.github/workflows/main.yml`), and can also
be triggered manually from the **Actions** tab (`workflow_dispatch`).

## Setup

1. Create a new **public** GitHub repository named `V2ray-Workflow` and push
   the contents of this folder to it.
2. Go to **Settings → Secrets and variables → Actions → New repository secret**.
3. Name it `BASE_URLS` and paste your subscription source URLs in the value
   box, **one per line**, e.g.:
   ```
   https://raw.githubusercontent.com/example/repo1/main/vless.txt
   https://raw.githubusercontent.com/example/repo2/main/configs.txt
   ```
   Lines starting with `#` and blank lines are ignored, so you can comment
   sources out.
4. Go to the **Actions** tab, enable workflows if prompted, and run
   **"Merge and Health-check V2Ray Subscriptions"** manually once (or wait for
   the hourly schedule).
5. After the first successful run, `configs/healthy.txt` appears in the repo —
   that's your subscription link.

## Why the sources are a secret, not a file

Keeping `BASE_URLS` as a repository secret means anyone who finds this public
repo sees the health-checking logic and the final healthy config list, but not
where the raw, untested configs come from. Only `configs/healthy.txt` — the
output — is ever committed.

## Tuning

Open `.github/workflows/main.yml` to adjust:

- `--mdelay 250` — maximum allowed real ping in milliseconds. Lower = stricter.
- `--prescan-workers 300` — concurrency of the initial TCP pre-scan.
- `cron: '0 */1 * * *'` — how often the workflow runs.

## License

MIT — see [LICENSE](LICENSE).

---

## ‏فارسی

‏*[English](#v2ray-workflow) ↑*

‏این ریپو لینک‌های ساب‌اسکریپشن «⁠⁦v2ray⁩⁠» را از یک فهرست خصوصی از منابع جمع‌آوری می‌کند، سلامت تک‌تک کانفیگ‌ها را تست می‌کند، و فقط کانفیگ‌های زنده و سریع را به‌صورت یک فایل ساب‌اسکریپشن آماده منتشر می‌کند — به‌طور خودکار، طبق زمان‌بندی، با «⁠⁦GitHub Actions⁩⁠».

‏لینک ساب‌اسکریپشن (برای وارد کردن در کلاینت):

```
https://raw.githubusercontent.com/<YOUR-USERNAME>/V2ray-Workflow/main/configs/healthy.txt
```

‏به‌جای `<«⁠⁦YOUR-USERNAME⁩⁠»>` نام کاربری واقعی گیت‌هاب خودت را بگذار.

### ‏روش کار

۱. ‏دریافت — فهرست لینک‌های ساب‌اسکریپشن از سکرت ریپو با نام «⁠⁦BASE_URLS⁩⁠» خوانده می‌شود (نه یک فایل کامیت‌شده، پس منابع تو حتی در یک ریپوی عمومی هم خصوصی می‌مانند) و هرکدام دانلود می‌شوند.

۲. ‏ادغام — همه را در یک فهرست موقت ترکیب می‌کند.

۳. ‏تست سلامت — هر کانفیگ با «⁠⁦xray-knife⁩⁠» تست می‌شود: یک پیش‌بررسی سریع «⁠⁦TCP⁩⁠» سرورهای مرده را فوراً حذف می‌کند، سپس یک تست اتصال واقعی، تأخیر واقعی را اندازه می‌گیرد. فقط کانفیگ‌هایی با پینگ واقعی زیر ۲۵۰ میلی‌ثانیه باقی می‌مانند.

۴. ‏انتشار — بازمانده‌ها در «⁠⁦configs/healthy.txt⁩⁠» نوشته و به ریپو پوش می‌شوند.

‏این روند هر ساعت به‌طور خودکار اجرا می‌شود (.«⁠⁦github/workflows/main.yml⁩⁠»)، و همچنین می‌توان آن را از تب «⁠⁦Actions⁩⁠» به‌صورت دستی هم اجرا کرد («⁠⁦workflow_dispatch⁩⁠»).

### ‏راه‌اندازی

۱. ‏یک ریپوی عمومی جدید با نام «⁠⁦V2ray-Workflow⁩⁠» بساز و محتوای این پوشه را به آن پوش کن.

۲. ‏برو به «⁠⁦Settings⁩⁠» → «⁠⁦Secrets and variables⁩⁠» → «⁠⁦Actions⁩⁠» → «⁠⁦New repository secret⁩⁠».

۳. ‏نامش را «⁠⁦BASE_URLS⁩⁠» بگذار و لینک‌های منبع ساب‌اسکریپشن را در کادر مقدار وارد کن، هر خط یک لینک، مثلاً:
   ```
   https://raw.githubusercontent.com/example/repo1/main/vless.txt
   https://raw.githubusercontent.com/example/repo2/main/configs.txt
   ```
‏   خط‌هایی که با # شروع می‌شوند و خط‌های خالی نادیده گرفته می‌شوند، پس می‌توانی منابع را کامنت کنی.

۴. ‏برو به تب «⁠⁦Actions⁩⁠»، اگر لازم بود ورکفلوها را فعال کن، و یک‌بار ورکفلو با نام «⁠⁦Merge and Health-check V2Ray Subscriptions⁩⁠» را دستی اجرا کن (یا منتظر زمان‌بندی ساعتی بمان).

۵. ‏بعد از اولین اجرای موفق، «⁠⁦configs/healthy.txt⁩⁠» در ریپو ظاهر می‌شود — همان لینک ساب‌اسکریپشن توست.

### ‏چرا منابع سکرت هستند نه فایل

‏نگه‌داشتن «⁠⁦BASE_URLS⁩⁠» به‌صورت یک سکرت یعنی هرکسی این ریپوی عمومی را پیدا کند، منطق تست سلامت و فهرست نهایی کانفیگ‌های سالم را می‌بیند، ولی نمی‌فهمد کانفیگ‌های خام و تست‌نشده از کجا می‌آیند. فقط «⁠⁦configs/healthy.txt⁩⁠» یعنی خروجی نهایی کامیت می‌شود.

### ‏تنظیمات

‏فایل .«⁠⁦github/workflows/main.yml⁩⁠» را باز کن تا این موارد را تغییر بدی:

- ‏--«⁠⁦mdelay 250⁩⁠» — حداکثر پینگ واقعی مجاز به میلی‌ثانیه. کمتر یعنی سخت‌گیرانه‌تر.
- ‏--«⁠⁦prescan-workers 300⁩⁠» — همزمانی پیش‌بررسی اولیه «⁠⁦TCP⁩⁠».
- ‏«⁠⁦cron: '0⁩⁠» */1 * * *' — فاصله‌ی اجرای ورکفلو.

### ‏لایسنس

‏«⁠⁦MIT⁩⁠» — فایل [«⁠⁦LICENSE⁩⁠»](«⁠⁦LICENSE⁩⁠») را ببین.
