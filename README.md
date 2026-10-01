# atiaatashin 1.1 🔥

پنل فارسی با تم آتشی، روی هستهٔ رسمی 3x-ui 3.8.5 / Xray. برای اجرای مستقل از میزبان با Docker ساخته شده است؛ source launcher نیز روی Linux amd64 دارای فضای اجرایی قابل استفاده است.

## بدون نوشتن دامنه

فقط نام کاربری و رمز مدیر را در Environment میزبان وارد کن. ATIA_PUBLIC_URL لازم نیست. پس از ورود موفق، آدرس همین مرورگر تشخیص داده می‌شود؛ Address، Host، SNI، TLS، پورت عمومی و اشتراک خودکار از آن ساخته می‌شوند. HTTPS به‌طور پیش‌فرض 443 و HTTP به‌طور پیش‌فرض 80 است؛ پورت عمومی متفاوت هم از آدرس مرورگر خوانده می‌شود.

UUID، Path و پورت داخلی در ساخت کاربر خودکارند. پروفایل‌ها: VLESS + WS، VLESS + XHTTP، VMess + WS، Trojan + WS. سهمیه و انقضا مشترک، ساخت گروهی تا 50 کاربر، QR محلی، اشتراک، بکاپ و بازیابی در رابط موجودند. دامنهٔ دیگر را در صورت نیاز از تنظیمات به‌صورت دستی انتخاب کن؛ حالت پیش‌فرض خودکار است.

## اجرای Deplexo

فایل‌های بسته، از جمله همهٔ فایل‌های Go، پوشهٔ web، Dockerfile و deplexo.yaml را به ریشهٔ مخزن/شاخهٔ انتخاب‌شده بفرست. در Deplexo همان شاخه را با روش Dockerfile وصل کن. PORT را 8080 بگذار، یک Instance و Volume در /data نگه دار. ATIA_ADMIN_USER=admin و ATIA_ADMIN_PASSWORD با حداقل 12 کاراکتر را به Environment اضافه کن. ATIA_PUBLIC_URL را خالی/حذف کن. راهنمای قدم‌به‌قدم: [INSTALL-FA.md](INSTALL-FA.md).

هستهٔ اجرایی هنگام Build داخل Image نصب و SHA-256 آن بررسی می‌شود. اجرای Deplexo ریشهٔ فقط‌خواندنی دارد؛ فایل‌های اجرایی داخل Image می‌مانند. دیتابیس و فایل Xray config روی Volume نوشته می‌شوند و فایل‌های اجرایی از همان Image اجرا می‌شوند. نصب زمان شروع در /tmp برای Deplexo مناسب نیست.

## داده بعد از Restart

یک /data قابل‌نوشتن به‌طور خودکار انتخاب می‌شود؛ پوشهٔ دادهٔ پیش‌فرض در این حالت /data/atiaatashin-data است. کاربران، کلیدها، Path، مصرف و تنظیمات با نگه‌داری همان Volume باقی می‌مانند. نشست ورود در حافظه است: بعد از Restart دوباره وارد شو؛ ساخت دوبارهٔ کاربران لازم نیست.

بدون /data قابل‌نوشتن، fallback موقت /tmp/atiaatashin-data است و UI هشدار بکاپ می‌دهد. متغیر ATIA_DATA_DIR برای Volume با مسیر متفاوت قابل استفاده است. تشخیص Mount به‌تنهایی انتقال داده میان Workerها/میزبان‌ها را تضمین نمی‌کند. حذف اپ/Volume داده را پاک می‌کند. برای انتقال مستقل بکاپ بگیر؛ GitHub فقط کد را نگه می‌دارد.

## میزبان‌های دیگر

Dockerfile به Deplexo وابسته نیست. میزبان باید Linux amd64، اجرای کانتینر، یک Instance، ورودی HTTP و پشتیبانی واقعی WebSocket/HTTP streaming داشته باشد. PORT میزبان خوانده می‌شود. برای حفظ داده، Volume واقعی لازم است؛ نبود این قابلیت در میزبان با کد حل نمی‌شود. روی هاست Static نمی‌توان این پنل را اجرا کرد. XHTTP به پشتیبانی میزبان و کلاینت نیاز دارد. روش‌های TCP/UDP مستقیم، REALITY، WireGuard و Hysteria روی این ورودی HTTP ارائه نمی‌شوند.

## توسعه

Go 1.22 یا جدیدتر، بدون وابستگی خارجی Go:

```sh
go test -race ./...
go vet ./...
go build .
```

برای سرور Docker، از compose.yaml و فایل خصوصی .env ساخته‌شده از .env.example استفاده کن. compose با Volume نام‌دار داده را حفظ می‌کند؛ فقط localhost:8080 را منتشر می‌کند. HTTPS reverse proxy را جدا تنظیم کن.

گزارش آزمون: [TEST-REPORT.md](TEST-REPORT.md). مجوز و انتساب اجزا: [THIRD-PARTY.md](THIRD-PARTY.md). راهنمای launcher قبلی در LEGACY-LAUNCHER-README.md صرفاً سابقه است، نه راهنمای این نسخه.

منابع بررسی‌شده در 2026-10-01: [Deplexo storage](https://docs.deplexo.com/operations/storage/)، [Docker builds](https://docs.deplexo.com/guides/docker/)، [deplexo.yaml](https://docs.deplexo.com/reference/configuration/).
