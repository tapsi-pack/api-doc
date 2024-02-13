# راهنمای احراز هویت با API Secret Key

در این راهنما احراز هویت در تپسی‌پک به کمک
API Secret Key
آموزش داده می‌شود.

## معرفی

کلید 
API Secret Key
یک
string
رندوم است که به یک کاربر مشخص اختصاص می‌یابد.
این کلید به عنوان رمز منحصر به فرد حساب کاربری شما عمل می‌کند. با قرار دادن این کلید در 
header
درخواست‌هایتان، می‌توانید خودتان را به تپسی‌پک بشناسانید.


هر کاربر می‌تواند
**حداکثر یک
API Secret Key
فعال**
داشته باشد.

برای دسترسی به
API
های تپسی‌پک باید دو مقدار را در
header
درخواست‌هایتان ثبت کنید.
تپسی‌پک به کمک این هدرها شما را شناسایی می‌کند و به این وسیله از دسترسی ناامن به داده‌های جلوگیری می‌شود.

- کلید
`x-api-secret-key` :
مقدار
API Secret Key
اختصاص داده شده به شما

- کلید
`x-encoded-user-id` :
مقدار
Base64 encode
شده‌ی شناسه‌ی حساب شما

برای گرفتن
API Secret Key
و شناسه‌ی حساب خود، می‌توانید درخواست خود را در
[پشتیبانی تپسی‌پک](https://pack.tapsi.ir/landing)
ثبت کنید.

![Authorization flow](../../images/pack-api-secret-key-flow.png)

## ویژگی های API Secret Key

همه‌ی
API Secret Key
ها این ویژگی‌ها را دارند:

### 1. دسترسی مشخص

هر
API Secret Key
دسترسی مشخص و محدود دارد.
به کمک این قابلیت می‌توانید تنها اجازه‌ی انجام برخی عملکردهای خاص را به کلید خود بدهید.

در حال حاضر می‌توانید اطلاعات
scope 
های موجود و توضیحات هر یک را در
[این‌جا](/apis/README.fa.md#مقدمه)
بخوانید.


### 2. انقضا

هر 
API Secret Key
می‌تواند تاریخ انقضای دلخواه شما را داشته باشد، اما به طور پیش‌فرض هر کلید پس از گذشت یک سال منقضی می‌شود.

هنگامی که قصد داشته باشید کلید شما تنها تا زمان مشخصی معتبر باشد، می‌توانید از این قابلیت استفاده کنید.


### 3. یادداشت

هر 
API Secret Key
می‌تواند یک یادداشت منحصر به فرد برای خود داشته باشد.

به کمک افزودن یادداشت به کلیدهای خود می‌توانید آن‌ها را راحت‌تر مدیریت کنید.

پیشنهاد می‌کنیم کاربرد کلیدهای خود را به عنوان یادداشت آن‌ها ذخیره کنید.

## روش استفاده از API Secret Key

توجه کنید که فیلد
`x-encoded-user-id`
مقدار 
Base64 encoded
شناسه‌ی حساب شماست.

برای مثال، در صورتی که شناسه‌ی حساب شما
**123**
باشد،
مقدار
encoded
آن برابر خواهد بود با:
`MTIz`.

این یک نمونه درخواست 
[API تاریخ‌های سرویس‌دهی](/apis/time/README.md)
است که در آن برای احراز هویت از
API Secret Key
استفاده شده است.


```bash
curl --location --request GET 'https://api.tapsi.cab/api/v1/delivery/external/embedded/available-dates' \
--header 'x-api-secret-key: 6gJqJu-1eIe9ypYzFh3pwtBkDaltr35Y09Z1zQacuzBcWfMAFFZqQgNdb2q_jWc-CU8wQXaUkEvFBpMIJ7_u24xuWoPABRY-_nyEHXreAATlAxrdTh5-64craO8zm8r2' \
--header 'x-encoded-user-id: MTIz' \
--data-raw ''
```

## روش باطل کردن API Secret Key

شما می‌توانید 
API Secret Key
خود را حذف کنید و به جای آن یک کلید جدید بسازید.

در صورتی که حدس می‌زنید ممکن است کلید شما به دست افراد نامطمئنی افتاده است، حتما کلید خود را حذف کنید و به جای آن یک کلید جدید بسازید، برای حذف کلید قبلی و ساخت کلید جدید با ما تماس بگیرید.

---

[![en](https://img.shields.io/badge/lang-en-red.svg)](./README.md)