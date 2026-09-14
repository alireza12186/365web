---
layout: post
title: "آموزش استفاده از فایل htaccess برای باز شدن سایت با https"
slug: "open-with-https-with-htaccess"
date: "2020-07-12T14:11:59"
categories: ["میزبانی وب"]
tags: ["htaccess", "http", "https", "فایل htaccess", "گواهینامه ssl"]
image: /assets/images/blog/secure-https-logo-av-1.png
---
همه ما از مزایای استفاده از گواهینامه‌های ssl و استفاده از https به جای http مطلع هستیم. در نتیجه مدیران وبسایتها تلاش می‌کنند که برای سایت خود از این گواهینامه‌ها استفاده کنند. ولی بعد از استفاده از گواهینامه ssl مشاهده می‌کنیم که در اکثر مواقع سایت هم با http باز می‌شود و هم با https. در [این مطلب](https://365web.ir/?p=4153) می‌خواهیم یک روش خیلی ساده را به شما یاد بدهیم: استفاده از فایل htaccess برای باز شدن سایت با https.

### ریدایرکت کردن کل ترافیک

در بسیاری از نرم‌افزارهای مدیریت محتوا فایلی وجود دارد به نام .htaccess . ما با استفاده از این فایل می‌توانیم ساایت را مجبور کنیم که فقط به صورت https باز بشود. برای این کار باید کد زیر را در فایل htaccess بگذاریم.

```
RewriteEngine On
RewriteCond %{HTTPS} !on
RewriteCond %{REQUEST_URI} !^/[0-9]+..+.cpaneldcv$
RewriteCond %{REQUEST_URI} !^/.well-known/pki-validation/[A-F0-9]{32}.txt(?:\ Comodo\ DCV)?$
RewriteRule (.*) https://%{HTTP_HOST}%{REQUEST_URI} [L,R=301]
```

### ریدایرکت یک دامنه خاص

اگر می‌خواهید یک دامنه خاص را به صورت https باز کنید می‌توانید کد زیر را در فایل htaccess قرار بدهید.

```
RewriteCond %{REQUEST_URI} !^/[0-9]+..+.cpaneldcv$
RewriteCond %{REQUEST_URI} !^/.well-known/pki-validation/[A-F0-9]{32}.txt(?:\ Comodo\ DCV)?$
RewriteEngine On
RewriteCond %{HTTP_HOST} ^example.com [NC]
RewriteCond %{SERVER_PORT} 80
RewriteRule ^(.*)$ https://www.example.com/$1 [R=301,L]
```

در کد بالا باید در خط آخر آدرس سایت خود را بگذارید.  
اگر احیاناً کد شما کار نکرد می‌توانید دو خط اول را حذف کنید.

### ریدایرکت کردن یک پوشه خاص

اگر فقط یک پوشه خاص را می‌خواهید به صورت https باز کنید می‌توانید کد زیر را در فایل htaccess قرار بدهید.

```
RewriteCond %{REQUEST_URI} !^/[0-9]+..+.cpaneldcv$
RewriteCond %{REQUEST_URI} !^/.well-known/pki-validation/[A-F0-9]{32}.txt(?:\ Comodo\ DCV)?$
RewriteEngine On
RewriteCond %{SERVER_PORT} 80
RewriteCond %{REQUEST_URI} folder
RewriteRule ^(.*)$ https://www.example.com/folder/$1 [R=301,L]
```

اینجا هم فراموش نکنید آدرس سایت و نام پوشه را درست وارد کنید.

[منبع](https://www.inmotionhosting.com/support/website/ssl/how-to-force-https-using-the-htaccess-file)
