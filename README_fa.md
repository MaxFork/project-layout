# طرح استاندارد پروژه Go
Translations:

* [한국어 문서](README_ko.md)
* [简体中文](README_zh.md)
* [正體中文](README_zh-TW.md)
* [简体中文](README_zh-CN.md) - ???
* [Français](README_fr.md)
* [日本語](README_ja.md)
* [Português](README_ptBR.md)
* [Español](README_es.md)
* [Română](README_ro.md)
* [Русский](README_ru.md)
* [Türkçe](README_tr.md)
* [Italiano](README_it.md)
* [Vietnamese](README_vi.md)
* [Українська](README_ua.md)
* [Indonesian](README_id.md)
* [हिन्दी](README_hi.md)


## بررسی اجمالی

این یک طرح پایه برای پروژه‌های اپلیکیشن Go است. توجه داشته باشید که این طرح از نظر محتوا بسیار ساده است، زیرا فقط بر روی ساختار کلی تمرکز دارد و نه محتوای داخلی آن. همچنین به دلیل این که بسیار سطح بالا است و جزئیات دقیق‌تری از چگونگی ساختاردهی پروژه شما را پوشش نمی‌دهد، ساده محسوب می‌شود. به عنوان مثال، ساختار پروژه‌ای که دارای معماری Clean باشد را پوشش نمی‌دهد.

این **`یک استاندارد رسمی که توسط تیم توسعه اصلی Go تعریف شده باشد، نیست`**. این مجموعه‌ای از الگوهای معمول تاریخی و نوظهور در اکوسیستم Go است. برخی از این الگوها نسبت به بقیه محبوب‌تر هستند. همچنین چندین بهبود کوچک به همراه چندین دایرکتوری پشتیبانی که در هر اپلیکیشن واقعی به اندازه کافی بزرگ کاربرد دارند، اضافه شده است. توجه داشته باشید که **تیم اصلی Go مجموعه‌ای عالی از راهنماهای کلی درباره ساختاردهی پروژه‌های Go و معنای آن برای پروژه شما هنگامی که وارد شده و نصب می‌شود، ارائه می‌دهد**. به صفحه [`سازمان‌دهی یک ماژول Go`](https://go.dev/doc/modules/layout) در مستندات رسمی Go برای جزئیات بیشتر مراجعه کنید. این صفحه شامل الگوهای دایرکتوری `internal` و `cmd` (که در زیر توضیح داده شده‌اند) و اطلاعات مفید دیگری است.

**`اگر سعی دارید Go یاد بگیرید یا یک PoC یا پروژه ساده برای خود بسازید، این طرح پروژه اضافی است. با چیزی بسیار ساده‌تر شروع کنید (یک فایل ساده `main.go` و `go.mod` کافی است).`** با رشد پروژه خود به یاد داشته باشید که ساختار کد شما بسیار مهم است، وگرنه به کدی آشفته با وابستگی‌های پنهان و وضعیت جهانی زیادی خواهید رسید. هنگامی که افراد بیشتری روی پروژه کار کنند، شما به ساختار بیشتری نیاز خواهید داشت. در این مواقع اهمیت دارد که روش مشترکی برای مدیریت بسته‌ها/کتابخانه‌ها معرفی کنید. هنگامی که پروژه‌ای متن‌باز دارید یا زمانی که می‌دانید پروژه‌های دیگر کد شما را وارد می‌کنند، در این مواقع اهمیت دارد که بسته‌های خصوصی (معروف به `internal`) داشته باشید. ریپازیتوری را کلون کنید، آنچه نیاز دارید نگه دارید و بقیه را حذف کنید! فقط به این دلیل که وجود دارد، به این معنا نیست که شما باید از همه چیز استفاده کنید. هیچ یک از این الگوها در هر پروژه‌ای استفاده نمی‌شوند. حتی الگوی `vendor` نیز جهانی نیست.

با Go 1.14، [`Go Modules`](https://go.dev/wiki/Modules) در نهایت برای تولید آماده شدند. از [`Go Modules`](https://blog.golang.org/using-go-modules) استفاده کنید، مگر اینکه دلیل خاصی برای عدم استفاده از آنها داشته باشید و در این صورت نیازی نیست نگران $GOPATH و محل قرارگیری پروژه خود باشید. فایل `go.mod` پایه در ریپو فرض می‌کند که پروژه شما در GitHub میزبانی می‌شود، اما این یک الزام نیست. مسیر ماژول می‌تواند هر چیزی باشد، اگرچه اولین مولفه مسیر ماژول باید یک نقطه در نام خود داشته باشد (نسخه فعلی Go دیگر آن را اجرا نمی‌کند، اما اگر از نسخه‌های کمی قدیمی‌تر استفاده می‌کنید، تعجب نکنید اگر ساخت‌هایتان بدون آن شکست بخورند). به مسائل [`37554`](https://github.com/golang/go/issues/37554) و [`32819`](https://github.com/golang/go/issues/32819) مراجعه کنید اگر می‌خواهید بیشتر در مورد آن بدانید.

این طرح پروژه به عمد عمومی است و سعی نمی‌کند ساختار بسته Go خاصی را تحمیل کند.

این یک تلاش جامعه‌محور است. اگر الگویی جدید می‌بینید یا فکر می‌کنید یکی از الگوهای موجود نیاز به به‌روزرسانی دارد، یک مسئله باز کنید.

اگر نیاز به کمک در نام‌گذاری، فرمت و استایل دارید، با اجرای [`gofmt`](https://golang.org/cmd/gofmt/) و [`staticcheck`](https://github.com/dominikh/go-tools/tree/master/cmd/staticcheck) شروع کنید. لینتر استاندارد قبلی، golint، اکنون منسوخ شده و نگهداری نمی‌شود؛ استفاده از یک لینتر نگهداری‌شده مانند staticcheck توصیه می‌شود. همچنین مطمئن شوید که این دستورالعمل‌ها و توصیه‌های استایل کد Go را خوانده‌اید:
* https://talks.golang.org/2014/names.slide
* https://golang.org/doc/effective_go.html#names
* https://blog.golang.org/package-names
* https://go.dev/wiki/CodeReviewComments
* [راهنمای سبک برای بسته‌های Go](https://rakyll.org/style-packages) (rakyll/JBD)

برای اطلاعات بیشتر در مورد نام‌گذاری و سازمان‌دهی بسته‌ها و همچنین توصیه‌های ساختار کد:
* [GopherCon EU 2018: Peter Bourgon - Best Practices for Industrial Programming](https://www.youtube.com/watch?v=PTE4VJIdHPg)
* [GopherCon Russia 2018: Ashley McNamara + Brian Ketelsen - بهترین شیوه‌ها در Go.](https://www.youtube.com/watch?v=MzTcsI6tn-0)
* [GopherCon 2017: Edward Muller - الگوهای ضد Go](https://www.youtube.com/watch?v=ltqV6pDKZD8)
* [GopherCon 2018: Kat Zien - چگونه اپلیکیشن‌های Go خود را ساختاردهی می‌کنید](https://www.youtube.com/watch?v=oL6JBUk6tj0)

یک پست چینی درباره راهنمای طراحی Package-Oriented و لایه‌بندی معماری
* [طراحی و معماری لایه‌بندی شده بر اساس بسته‌ها](https://github.com/danceyoung/paper-code/blob/master/package-oriented-design/packageorienteddesign.md)

## دایرکتوری‌های Go

### /cmd

برنامه‌های اصلی برای این پروژه.

نام دایرکتوری هر برنامه باید با نام اجرایی که می‌خواهید داشته باشید مطابقت داشته باشد (مثلاً /cmd/myapp).

کد زیادی را در دایرکتوری برنامه قرار ندهید. اگر فکر می‌کنید کد می‌تواند وارد شود و در پروژه‌های دیگر استفاده شود، باید در دایرکتوری /pkg قرار بگیرد. اگر کد قابل استفاده مجدد نیست یا نمی‌خواهید دیگران از آن استفاده کنند، آن را در دایرکتوری /internal قرار دهید. از کارهایی که دیگران انجام می‌دهند شگفت‌زده خواهید شد، بنابراین در مورد نیت‌های خود صریح باشید!

رایج است که یک تابع اصلی کوچک وجود داشته باشد که کد را از دایرکتوری‌های /internal و /pkg وارد کند و آن را فراخوانی کند و بس.

برای مثال‌ها به دایرکتوری [/cmd](cmd/README.md) مراجعه کنید.

### /internal

کد خصوصی برنامه و کتابخانه. این همان کدی است که نمی‌خواهید دیگران در برنامه‌ها یا کتابخانه‌های خود وارد کنند. توجه داشته باشید که این الگوی چیدمان توسط خود کامپایلر Go اعمال می‌شود. برای جزئیات بیشتر به [یادداشت‌های انتشار Go 1.4](https://golang.org/doc/go1.4#internalpackages) مراجعه کنید. توجه داشته باشید که شما محدود به دایرکتوری سطح بالای internal نیستید. می‌توانید بیش از یک دایرکتوری internal در هر سطحی از درخت پروژه خود داشته باشید.

می‌توانید ساختار اضافی کمی را به بسته‌های داخلی خود اضافه کنید تا کدهای مشترک و غیر مشترک داخلی خود را جدا کنید. این ضروری نیست (به خصوص برای پروژه‌های کوچکتر)، اما دیدن سرنخ‌های بصری که استفاده مورد نظر از بسته را نشان می‌دهند خوب است. کد واقعی برنامه شما می‌تواند در دایرکتوری /internal/app قرار بگیرد (مثلاً /internal/app/myapp) و کدی که توسط آن برنامه‌ها به اشتراک گذاشته شده است در دایرکتوری /internal/pkg قرار بگیرد (مثلاً /internal/pkg/myprivlib).

دایرکتوری‌های internal برای خصوصی‌سازی بسته‌ها استفاده می‌شوند. اگر یک بسته را در یک دایرکتوری internal قرار دهید، دیگر بسته‌ها نمی‌توانند آن را وارد کنند مگر اینکه جد مشترکی داشته باشند. و این تنها دایرکتوری است که در مستندات Go نام برده شده و توسط کامپایلر به طور خاص پردازش می‌شود.

### /pkg

کد کتابخانه‌ای که استفاده از آن توسط برنامه‌های خارجی مجاز است (مثلاً /pkg/mypubliclib). پروژه‌های دیگر این کتابخانه‌ها را وارد می‌کنند و انتظار دارند که کار کنند، بنابراین قبل از اینکه چیزی را اینجا قرار دهید دو بار فکر کنید :-) توجه داشته باشید که دایرکتوری internal راه بهتری برای اطمینان از غیر قابل وارد کردن بودن بسته‌های خصوصی شما است، زیرا توسط Go اعمال می‌شود. دایرکتوری /pkg هنوز هم یک راه خوب برای صریح اعلام کردن این است که کد در این دایرکتوری برای استفاده دیگران ایمن است. پست وبلاگ [I'll take pkg over internal](https://travisjeffery.com/b/2019/11/i-ll-take-pkg-over-internal/) نوشته تراویس جفری یک مرور خوب از دایرکتوری‌های pkg و internal و زمان استفاده از آنها ارائه می‌دهد.

همچنین یک راه برای گروه‌بندی کد Go در یک مکان است زمانی که دایرکتوری ریشه شما شامل تعداد زیادی از اجزا و دایرکتوری‌های غیر Go است، و این باعث می‌شود که اجرای ابزارهای مختلف Go آسان‌تر شود (همانطور که در این گفتگوها ذکر شده است: [Best Practices for Industrial Programming](https://www.youtube.com/watch?v=PTE4VJIdHPg) از GopherCon EU 2018، [GopherCon 2018: Kat Zien - How Do You Structure Your Go Apps](https://www.youtube.com/watch?v=oL6JBUk6tj0) و [GoLab 2018 - Massimiliano Pippi - Project layout patterns in Go](https://www.youtube.com/watch?v=3gQa1LWwuzk)).

برای دیدن اینکه کدام مخازن Go محبوب از این الگوی چیدمان پروژه استفاده می‌کنند، به دایرکتوری [/pkg](pkg/README.md) مراجعه کنید. این یک الگوی چیدمان رایج است، اما به طور جهانی پذیرفته نشده و برخی از اعضای جامعه Go آن را توصیه نمی‌کنند.

اگر پروژه برنامه شما واقعاً کوچک است و جایی که یک سطح اضافی از لانه‌گذاری ارزش زیادی اضافه نمی‌کند، استفاده نکردن از آن خوب است (مگر اینکه واقعاً بخواهید :-)). به آن فکر کنید وقتی پروژه شما به اندازه‌ای بزرگ شد که دایرکتوری ریشه شما شلوغ شود (به ویژه اگر تعداد زیادی از اجزای برنامه غیر Go داشته باشید).

منشأ دایرکتوری pkg: کد منبع قدیمی Go قبلاً از pkg برای بسته‌های خود استفاده می‌کرد و سپس پروژه‌های مختلف Go در جامعه شروع به کپی کردن این الگو کردند (برای دیدن زمینه بیشتر، به [این](https://twitter.com/bradfitz/status/1039512487538970624) توییت برد فیتزپاتریک مراجعه کنید).

### /vendor

وابستگی‌های برنامه (به صورت دستی یا با استفاده از ابزار مدیریت وابستگی مورد علاقه شما مانند ویژگی جدید [Go Modules](https://go.dev/wiki/Modules) داخلی). دستور go mod vendor دایرکتوری /vendor را برای شما ایجاد خواهد کرد. توجه داشته باشید که ممکن است نیاز باشد که پرچم -mod=vendor را به دستور go build خود اضافه کنید اگر از Go 1.14 استفاده نمی‌کنید که به صورت پیش‌فرض فعال است.

اگر در حال ساخت یک کتابخانه هستید، وابستگی‌های برنامه خود را به مخزن ندهید.

توجه داشته باشید که از [1.13](https://golang.org/doc/go1.13#modules) Go همچنین ویژگی پروکسی ماژول را فعال کرده است (استفاده از [https://proxy.golang.org](https://proxy.golang.org) به عنوان سرور پروکسی ماژول خود به صورت پیش‌فرض). برای اطلاعات بیشتر درباره آن [اینجا](https://blog.golang.org/module-mirror-launch) را بخوانید تا ببینید آیا با تمام نیازها و محدودیت‌های شما مطابقت دارد یا خیر. اگر اینطور باشد، دیگر به دایرکتوری vendor نیازی نخواهید داشت.

## دایرکتوری‌های برنامه سرویس

### /api

مشخصات OpenAPI/Swagger، فایل‌های طرح‌واره JSON، فایل‌های تعریف پروتکل.

برای مثال‌ها به دایرکتوری [/api](api/README.md) مراجعه کنید.

## دایرکتوری‌های برنامه وب

### /web

اجزای خاص برنامه وب: دارایی‌های وب استاتیک، الگوهای سمت سرور و SPAs.

## دایرکتوری‌های رایج برنامه

### /configs

قالب‌های فایل پیکربندی یا پیکربندی‌های پیش‌فرض.

فایل‌های قالب confd یا consul-template خود را اینجا قرار دهید.

### /init

پیکربندی‌های init سیستم (systemd، upstart، sysv) و مدیر فرایند/ناظر (runit، supervisord).

### /scripts

اسکریپت‌ها برای انجام عملیات‌های مختلف ساخت، نصب، تحلیل و غیره.

این اسکریپت‌ها فایل Makefile سطح ریشه را کوچک و ساده نگه می‌دارند (مثلاً [https://github.com/hashicorp/terraform/blob/main/Makefile](https://github.com/hashicorp/terraform/blob/main/Makefile)).

برای مثال‌ها به دایرکتوری [/scripts](scripts/README.md) مراجعه کنید.

### /build

بسته‌بندی و یکپارچه‌سازی مداوم.

پیکربندی‌ها و اسکریپت‌های بسته‌بندی ابر (AMI)، کانتینر (Docker)، سیستم‌عامل (deb، rpm، pkg) را در دایرکتوری /build/package قرار دهید.

پیکربندی‌ها و اسکریپت‌های CI (travis، circle، drone) را در دایرکتوری /build/ci قرار دهید. توجه داشته باشید که برخی از ابزارهای CI (مانند Travis CI) بسیار حساس به مکان فایل‌های پیکربندی خود هستند. سعی کنید فایل‌های پیکربندی را در دایرکتوری /build/ci قرار دهید و آنها را به مکانی که ابزارهای CI انتظار دارند پیوند دهید (در صورت امکان).

### /deployments

پیکربندی‌ها و الگوهای استقرار IaaS، PaaS، سیستم و ارکستراسیون کانتینر (docker-compose، kubernetes/helm، terraform). توجه داشته باشید که در برخی مخازن (به ویژه برنامه‌هایی که با kubernetes مستقر می‌شوند) این دایرکتوری /deploy نامیده می‌شود.

### /test

برنامه‌های آزمایشی خارجی اضافی و داده‌های آزمایشی. به هر نحوی که می‌خواهید دایرکتوری /test را ساختاردهی کنید. برای داده‌های آزمایشی اضافی، توصیه می‌شود از دایرکتوری `/test/testdata` یا `/test/data` مطابق با نیازهای Go استفاده کنید.

این جای خوبی برای تست‌های سطح یکپارچه‌سازی و end-to-end برای ذخیره شدن همراه با ابزارهای اضافی برای تسهیل آزمایش است.



### `/test`

Additional external test apps and test data. Feel free to structure the `/test` directory anyway you want. For bigger projects it makes sense to have a data subdirectory. For example, you can have `/test/data` or `/test/testdata` if you need Go to ignore what's in that directory. Note that Go will also ignore directories or files that begin with "." or "_", so you have more flexibility in terms of how you name your test data directory.

See the [`/test`](test/README.md) directory for examples.


برای مثال‌ها به دایرکتوری [/test](test/README.md) مراجعه کنید.

## دایرکتوری‌های موقتی

### /tmp

فایل‌های موقت را اینجا قرار دهید. این معمولاً به صورت دستی در طول اجرای برنامه توسط شما ایجاد می‌شود و نباید برای مدت طولانی وجود داشته باشد (مثلاً فایل‌های کش یا فایل‌های خروجی موقت که توسط عملیات خاصی تولید می‌شوند).

## سایر دایرکتوری‌های رایج

### /docs

مستندات طراحی و پیاده‌سازی. توجه داشته باشید که گوگل در زمان استفاده از [مدل طراحی Monorepo](https://github.com/google/mono_repo) از این مدل به عنوان یک الگوی ثابت استفاده می‌کند. همچنین توجه داشته باشید که پروژه‌های بزرگ Go به جای /docs، از دایرکتوری‌های دیگر مانند /doc استفاده می‌کنند.

برای مثال‌ها به دایرکتوری [/docs](docs/README.md) مراجعه کنید.

### /tools

ابزارها و اسکریپت‌های مربوط به این پروژه، اما از کد منبع اصلی جدا هستند (به عنوان مثال کد سازنده‌هایی که برای تولید کد استفاده می‌شوند و در کد منبع نهایی پروژه نیازی نیست).

### /examples

نمونه‌های استفاده شده از پروژه شما. بهتر است این دایرکتوری را به عنوان بخشی از پروژه خود نگه دارید.

برای مثال‌ها به دایرکتوری [/examples](examples/README.md) مراجعه کنید.

### /third_party

کتابخانه‌ها و ابزارهای وابسته‌ای که توسط شخص ثالث ارائه شده‌اند و پروژه شما به آنها وابسته است. همچنین ممکن است برای ابزارهای اضافی که توسط دیگران ساخته شده‌اند و مورد نیاز هستند، مورد استفاده قرار گیرد. در پروژه‌های مدرن، به دلیل استفاده از مدیریت وابستگی‌های ماژولی Go، کمتر دیده می‌شود.

### /githooks

اسکریپت‌های Git Hook. می‌توانید اسکریپت‌های Hook Git خود را در اینجا قرار دهید. این معمولاً شامل مواردی مانند pre-commit یا pre-push است که به صورت خودکار اجرا می‌شوند.

### /assets

فایل‌های استاتیک و تصاویر مانند لوگوها و آیکون‌ها.

### /website

اگر پروژه شما شامل مستندات یا وب‌سایت محصول است، می‌توانید فایل‌های وب‌سایت خود را اینجا ذخیره کنید.

## دایرکتوری‌های غیراستاندارد

بعضی از دایرکتوری‌ها در پروژه‌های خاص ایجاد می‌شوند که استاندارد نیستند اما ممکن است در موارد خاص مفید باشند:

### /integrations

فایل‌های مرتبط با یکپارچه‌سازی با سیستم‌ها و سرویس‌های دیگر (مثلاً ادغام با APIهای شخص ثالث).

### /tasks

وظایف خودکار، مانند ساخت و استقرار خودکار برنامه.

### /sandbox

محیط‌های آزمایشی و توسعه برای ایده‌های جدید یا آزمایش ویژگی‌های جدید پروژه.

### /playbooks

مستندات و دستورالعمل‌هایی برای اجرای عملیات خاص مانند استقرار، تعمیر و نگهداری و مدیریت سیستم.

## Other Directories

### `/docs`

Design and user documents (in addition to your godoc generated documentation).

See the [`/docs`](docs/README.md) directory for examples.

### `/tools`

Supporting tools for this project. Note that these tools can import code from the `/pkg` and `/internal` directories.

See the [`/tools`](tools/README.md) directory for examples.

### `/examples`

Examples for your applications and/or public libraries.

See the [`/examples`](examples/README.md) directory for examples.

### `/third_party`

External helper tools, forked code and other 3rd party utilities (e.g., Swagger UI).

### `/githooks`

Git hooks.

### `/assets`

Other assets to go along with your repository (images, logos, etc).

### `/website`

This is the place to put your project's website data if you are not using GitHub pages.

See the [`/website`](website/README.md) directory for examples.

## Directories You Shouldn't Have

### `/src`

Some Go projects do have a `src` folder, but it usually happens when the devs came from the Java world where it's a common pattern. If you can help yourself try not to adopt this Java pattern. You really don't want your Go code or Go projects to look like Java :-)

Don't confuse the project level `/src` directory with the `/src` directory Go uses for its workspaces as described in [`How to Write Go Code`](https://golang.org/doc/code.html). The `$GOPATH` environment variable points to your (current) workspace (by default it points to `$HOME/go` on non-windows systems). This workspace includes the top level `/pkg`, `/bin` and `/src` directories. Your actual project ends up being a sub-directory under `/src`, so if you have the `/src` directory in your project the project path will look like this: `/some/path/to/workspace/src/your_project/src/your_code.go`. Note that with Go 1.11 it's possible to have your project outside of your `GOPATH`, but it still doesn't mean it's a good idea to use this layout pattern.


## Badges

* [Go Report Card](https://goreportcard.com/) - It will scan your code with `gofmt`, `go vet`, `gocyclo`, `golint`, `ineffassign`, `license` and `misspell`. Replace `github.com/golang-standards/project-layout` with your project reference.

    [![Go Report Card](https://goreportcard.com/badge/github.com/golang-standards/project-layout?style=flat-square)](https://goreportcard.com/report/github.com/golang-standards/project-layout)

* ~~[GoDoc](http://godoc.org) - It will provide online version of your GoDoc generated documentation. Change the link to point to your project.~~

    [![Go Doc](https://img.shields.io/badge/godoc-reference-blue.svg?style=flat-square)](http://godoc.org/github.com/golang-standards/project-layout)

* [Pkg.go.dev](https://pkg.go.dev) - Pkg.go.dev is a new destination for Go discovery & docs. You can create a badge using the [badge generation tool](https://pkg.go.dev/badge).

    [![PkgGoDev](https://pkg.go.dev/badge/github.com/golang-standards/project-layout)](https://pkg.go.dev/github.com/golang-standards/project-layout)

* Release - It will show the latest release number for your project. Change the github link to point to your project.

    [![Release](https://img.shields.io/github/release/golang-standards/project-layout.svg?style=flat-square)](https://github.com/golang-standards/project-layout/releases/latest)

## Notes

A more opinionated project template with sample/reusable configs, scripts and code is a WIP.
