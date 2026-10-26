# WebHW2

<div dir="rtl">

<p dir="rtl">سوال 1)</p>

<p dir="rtl">0.9 : فقط <span dir="ltr">GET</span> / بدون هدر / بدون کد وضعیت / فقط <span dir="ltr">HTML</span> / بسته شدن اتصال بعد هر ریسپانس</p>

<p dir="rtl">1.0 : هدر و کد وضعیت و متد <span dir="ltr">POST</span> و <span dir="ltr">HEAD</span> / <span dir="ltr">Content-Type</span> / یه درخواست برای هر <span dir="ltr">TCP</span></p>

<p dir="rtl">1.1 : <span dir="ltr">keep-alive</span> / هدر <span dir="ltr">Host</span> / <span dir="ltr">chunked transfer</span> / <span dir="ltr">pipelining</span> / کش بهتر / <span dir="ltr">range request</span> / متد بیشتر / <span dir="ltr">head of line blocking</span> در لایه برنامه هنوز هست</p>

<p dir="rtl">2 : قالب بندی باینری / مالتی پلکسینگ چند درخواست رو یه اتصال / فشرده کردن هدر با <span dir="ltr">HPACK</span> / اولویت بندی استریم و سرور پوش / <span dir="ltr">head of line blocking</span> در لایه <span dir="ltr">TCP</span> هنوز هست</p>

<p dir="rtl">3 : روی <span dir="ltr">QUIC</span> و <span dir="ltr">UDP</span> / <span dir="ltr">TLS1.3</span> داخلی / استریم مستقل / <span dir="ltr">handshake</span> سریع تر (<span dir="ltr">1-RTT</span> و <span dir="ltr">0-RTT</span>) / <span dir="ltr">connection migration</span> / فشرده سازی <span dir="ltr">QPACK</span></p>

<p dir="rtl">چرا <span dir="ltr">UDP</span> :</p>

<p dir="rtl">- در <span dir="ltr">TCP</span> اگر یک بسته گم بشه همه استریم ها منتظر میمانند . <span dir="ltr">QUIC</span> برای هر استریم قابلیت اطمینان جداگانه دارد.</p>

<p dir="rtl">- <span dir="ltr">TCP</span> تو هسته <span dir="ltr">OS</span> و میان افزار های شبکه خیلی درگیر عه و تغییرش تقریبا ممکن نیست (<span dir="ltr">ossification</span>) . <span dir="ltr">QUIC</span> تو <span dir="ltr">user space</span> پیاده میشه و راحت تر آپدیت میشه</p>

<p dir="rtl">- <span dir="ltr">UDP</span> از <span dir="ltr">NAT</span> و فایروال ها رد می‌شود. ولی یک پروتکل جدید در لایه چهارم عبور نمی کند و مشکل میخورد</p>

<p dir="rtl">- <span dir="ltr">QUIC</span> خودش قابلیت اطمینان و کنترل ازدحام و رمزنگاری روی <span dir="ltr">UDP</span> را میسازد و <span dir="ltr">handshake</span> انتقال و <span dir="ltr">TLS</span> را ادغام میکند</p>

<p dir="rtl">سوال 2 )</p>

<p dir="rtl">الف) <span dir="ltr">stateless</span> : سرور هیچ حافظه ای از درخواست های قبلی نداره / هر درخواست باید همه اطلاعات لازم رو خودش حمل کنه / ساده و مقیاس پذیر (هر سروری میتونه جواب بده)</p>

<p dir="rtl"><span dir="ltr">stateful</span> : سرور وضعیت نشست رو نگه میداره / درخواست بعدی به همون وضعیت وابسته‌ست / مثل <span dir="ltr">TCP</span> و <span dir="ltr">FTP</span> / برای نشست پیچیده راحت تره ولی مقیاس پذیری و تحمل خطا سخت تره</p>

<p dir="rtl">ب) <span dir="ltr">HTTP</span> هست <span dir="ltr">stateless</span> . برای نگه داشتن وضعیت :</p>

<p dir="rtl">- کوکی : سرور با <span dir="ltr">Set-Cookie</span> یه مقدار به مرورگر میده / مرورگر تو هر درخواست برمیگردونه</p>

<p dir="rtl">- <span dir="ltr">Session</span> : شناسه نشست تو کوکی / داده اصلی سمت سرور (حافظه یا <span dir="ltr">Redis</span>)</p>

<p dir="rtl">- توکن (مثل <span dir="ltr">JWT</span>) : وضعیت یا هویت داخل توکن امضا شده / کلاینت تو هدر <span dir="ltr">Authorization</span> میفرسته</p>

<p dir="rtl">- پارامتر <span dir="ltr">URL</span> / فیلد مخفی فرم</p>

<p dir="rtl">سوال 3 )</p>

<p dir="rtl">الف) تو <span dir="ltr">HTTP</span> فقط کلاینت میتونه ارتباط رو شروع کنه / سرور خودش نمیتونه داده بفرسته / ولی تو چت و اعلان (<span dir="ltr">alerting</span>) و <span dir="ltr">webhook</span> رویداد سمت سرور اتفاق میفته و باید سریع به کلاینت برسه / کلاینت نمیدونه رویداد کی میاد</p>

<p dir="rtl">ب) راهکار ها :</p>

<p dir="rtl">- <span dir="ltr">Polling</span> : کلاینت هر چند ثانیه درخواست میده و میپرسه خبری هست یا نه / ساده / ترافیک الکی زیاد و تاخیر</p>

<p dir="rtl">- <span dir="ltr">Long polling</span> : سرور درخواست رو باز نگه میداره تا رویداد بیاد بعد جواب میده / کلاینت سریع درخواست جدید میده / تاخیر کمتر ولی اتصال باز زیاد</p>

<p dir="rtl">- <span dir="ltr">SSE</span> : یه اتصال <span dir="ltr">HTTP</span> یک طرفه و بلندمدت / سرور رویداد ها رو توش استریم میکنه / ساده و بازاتصال خودکار / فقط سرور به کلاینت</p>

<p dir="rtl">- <span dir="ltr">WebSocket</span> : بعد <span dir="ltr">handshake</span> اولیه <span dir="ltr">HTTP</span> اتصال میشه دوطرفه (<span dir="ltr">full-duplex</span>) / مناسب چت و بازی / نگه داشتن اتصال و مقیاس پذیری هزینه داره</p>

<p dir="rtl">- <span dir="ltr">Webhook</span> : برای سرور به سرور / مصرف کننده یه <span dir="ltr">URL</span> ثبت میکنه / تولید کننده موقع رویداد بهش <span dir="ltr">POST</span> میزنه / مصرف کننده باید خودش سرور در دسترس داشته باشه</p>

<p dir="rtl">- <span dir="ltr">Push notification</span> (مثل <span dir="ltr">FCM</span> و <span dir="ltr">APNs</span>) : سرویس واسط اتصال دائمی با دستگاه داره / سرور برنامه پیام رو از طریق اون میفرسته</p>

<p dir="rtl">- <span dir="ltr">HTTP/2 Server Push</span> : سرور منابع رو قبل درخواست میفرسته / برای رویداد زنده مناسب نیست / عملا منسوخ شده</p>

<p dir="rtl">سوال 4 )</p>

<p dir="ltr"><code>ss://asghar:1234!!@ss.myproxy.com:1234#shadowSocks1</code></p>

<p dir="rtl"><span dir="ltr">Scheme</span> : <span dir="ltr">ss</span></p>

<p dir="rtl"><span dir="ltr">Userinfo</span> : <span dir="ltr">asghar:1234!!</span> (نام کاربری <span dir="ltr">asghar</span> / رمز 1234!!)</p>

<p dir="rtl"><span dir="ltr">Host</span> : <span dir="ltr">ss.myproxy.com</span> (ساب دامین <span dir="ltr">ss</span> / دامین <span dir="ltr">myproxy</span> / <span dir="ltr">TLD com</span>)</p>

<p dir="rtl"><span dir="ltr">Port</span> : 1234</p>

<p dir="rtl"><span dir="ltr">Path</span> و <span dir="ltr">Query</span> : ندارد</p>

<p dir="rtl"><span dir="ltr">Fragment</span> : <span dir="ltr">shadowSocks1</span></p>

<p dir="rtl">سوال 5 )</p>

<p dir="rtl">- 503 (<span dir="ltr">Service Unavailable</span>) : مشکل موقتی سمت سرور / میشه <span dir="ltr">Retry-After</span> گذاشت / 500 هم قبوله ولی 503 دقیق تره</p>

<p dir="rtl">- 401 (<span dir="ltr">Unauthorized</span>) : احراز هویت ناموفق / پیام باید عمومی باشه (نام کاربری یا رمز اشتباه است) تا وجود یا عدم وجود کاربر لو نره (<span dir="ltr">user enumeration</span>) / 404 مناسب نیست</p>

<p dir="rtl">- 301 (<span dir="ltr">Moved Permanently</span>) : انتقال دائمی / اگر متد و بدنه باید حفظ بشه 308 بهتره / برای انتقال موقت 307 یا 302</p>

<p dir="rtl">- 429 (<span dir="ltr">Too Many Requests</span>) : مخصوص <span dir="ltr">rate limiting</span> / میشه <span dir="ltr">Retry-After</span> هم فرستاد</p>

<p dir="rtl">- 200 (<span dir="ltr">OK</span>) : عملیات موفق و توکن تو هدر برگشته / 201 مناسب نیست چون منبع جدیدی ساخته نشده</p>

<p dir="rtl">- 403 (<span dir="ltr">Forbidden</span>) : هویت مشخصه ولی دسترسی ممنوعه / چون دلیل تحریم و قانونیه 451 (<span dir="ltr">Unavailable For Legal Reasons</span>) هم مناسبه و دلیل رو شفاف تر میگه</p>

<p dir="rtl">سوال 6 )</p>

<p dir="rtl">- <span dir="ltr">Caching</span> / <span dir="ltr">Forward proxy</span> (پروکسی کش یا <span dir="ltr">mirror</span> محلی) : درخواست بسته ها از کلاینت های داخلی میره به پروکسی داخلی / پروکسی کش میکنه</p>

<p dir="rtl">- <span dir="ltr">Reverse proxy</span> / <span dir="ltr">Load balancer</span> (همراه <span dir="ltr">GeoDNS</span> یا <span dir="ltr">CDN</span>) : درخواست ها رو بر اساس مبدا جغرافیایی به سرور مناسب میفرسته</p>

<p dir="rtl">- <span dir="ltr">Reverse proxy</span> (دروازه احراز هویت مثل <span dir="ltr">nginx</span> یا <span dir="ltr">API gateway</span>) : جلوی سرویس قدیمی قرار میگیره / احراز هویت رو قبل رسیدن درخواست به سرویس انجام میده / بدون تغییر تو خود سرویس</p>

</div>
