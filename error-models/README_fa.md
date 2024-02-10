# مدل ارورها

در این آموزش با ارورهای تپسی‌پک و مدل آن‌ها آشنا می‌شوید.

## مقدمه

تپسی‌پک در
API
های خود از  زیرساخت
[مدل ارورهای گوگل](https://cloud.google.com/apis/design/errors)
استفاده می‌کند.

تمام ارورهایی که ممکن است در استفاده از تپسی‌پک به آن‌ها بر بخورید، مطابق این فرمت هستند:

```json5
{
  "code": "Int",
  "message": "String",
  "details": [
    {
      "@type": "type.googleapis.com/google.rpc.LocalizedMessage",
      "locale": "fa-IR",
      "message": "مختصات داده شده اشتباه است یا تپسی پک در این مختصات سرویس ارائه نمی‌دهد."
    }
  ]
}
```

## فیلد message

فیلد
`message`
شامل توضیحاتی در مورد ارور است. این توضیحات به زبان انگلیسی است و برای توسعه‌دهندگان طراحی شده است تا به کمک آن بتوانند متوجه دلیل خطاها شوند و آن‌ها را رفع کنند.

## فیلد details

فیلد
`details`
شامل آرایه‌ای از
payload
هاست که همگی آن‌ها همواره به فرمت استاندارد و ثابتی هستند.
توسعه دهندگان باید از تمام این
payload
ها استفاده کنند.
در حال حاضر همه‌ی این
payload
ها از این یک نوع هستند

| Field             | Type   | Description                                                                                                             |
|-------------------|--------|-------------------------------------------------------------------------------------------------------------------------|
| ErrorLocalization | String | پیام خطایی که باید به کاربر نمایش داده شود. |

### بومی‌سازی ارور (Error Localization)

در صورتی که نیاز باشد پیام خطایی به کاربر نمایش داده شود، در آبجکت
payload
مقدار
`@type`
برابر 
`type.googleapis.com/google.rpc.LocalizedMessage`
خواهد بود و مقدار
`message`
نیز پیام خطایی‌ست که باید به کاربر نمایش داده شود، مشابه نمونه‌ی زیر:

```json5
{
  "@type": "type.googleapis.com/google.rpc.LocalizedMessage",
  "locale": "fa-IR",
  "message": "مختصات داده شده اشتباه است یا تپسی پک در این مختصات سرویس ارائه نمی‌دهد."
}
```

### ویژگی‌های Error Details

تپسی‌پک تضمین می‌کند که همه‌ی آبجکت‌های
error details
همواره به فرمت یکی از
[payload های استاندارد](#فیلد-details)
باشند.

توجه داشته باشید که ممکن است
`details`
یک آرایه‌ی خالی باشد. برای مثال، هنگامی که به خاطر یک مشکل در شبکه درخواست شما با ارور مواجه شده است
`details`
می‌تواند یک آرایه‌ی خالی باشد. باید در پیاده سازی به این نکته توجه داشت و در صورتی که هیچ
payload
ای در
`details`
نباشد، برای هندل کردن خطا باید از فیلدهای
`code`
و
`message`
استفاده کرد که همواره وجود دارند.

همچنین، تپسی‌پک تضمین می‌کند که در لیست
`details`
از هر
type
یک یا صفر
payload
داریم.

همچنین برای هندل کردن ارورها، حتما به این نکات توجه کنید:

- در صورتی که
payload
ای وجود داشت که
type
آن
ErrorLocalization
بود، باید مقدار فیلد
`message`
آن به کاربر نهایی محصول شما نمایش داده شود.
