# سرویس تپسی‌پک: API گرفتن اعتبار کاربر

این
API
موجودی حساب کاربر را باز می‌گرداند.
این مقدار به واحد «تومان» است.

توجه داشته باشید که در صورتی که از روش 
OAuth
استفاده می‌کنید، موجودی حساب صاحب
PAT
را خواهید گرفت و در صورتی که از روش 
Secret Key
استفاده می کنید، موجودی حساب خودتان را خواهید گرفت.


URL:

```
/api/v1/delivery/external/embedded/user/credit
```

Method:

```
GET
```

Response:

```json5
{
  "balance": 125000
}
```

---

[![en](https://img.shields.io/badge/lang-en-red.svg)](./README.md)