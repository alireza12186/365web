---
layout: post
title: "تغییر نسخه php برای سرورهایی که از mod_lsapi‌ استفاده می‌کنند"
slug: "change-php-version-for-mod-lsapi"
date: "2019-02-17T19:56:59"
categories: ["میزبانی وب"]
tags: ["php", "تغییر نسخه php"]
image: /assets/images/blog/The-Best-Software-For-PHP-Coding-1.jpg
---
در [این مطلب](https://365web.ir/%d8%aa%d8%ba%db%8c%db%8c%d8%b1-%d9%86%d8%b3%d8%ae%d9%87-php-%d9%81%d8%a7%db%8c%d9%84-htaccess/) در مورد تغییر نسخه php برای یک پوشه خاص صحبت کردیم. روشی که در آن مطلب ذکر شدیم برای شرایطی است که روی سرور شما از mod\_php استفاده شده باشد. ولی ممکن است سرور شما از هندلر دیگری برای php استفاده کند. این روزها خیلی از شرکتهای هاستینگ از سیستم عامل کلودلینوکس و mod\_lsapi استفاده می‌کنند. این مود هم سرعت بیشتری به فایلهای php می‌بخشد و هم امنیت بسیار بالاتری دارد.

به منظور تغییر نسخه php برای سرورهایی که از این مود استفاده می‌کنند دیگر دستور قبلی کار نخواهد کرد. در نتیجه باید از روش دیگری استفاده کرد.

### مراحل تغییر نسخه php

#### حالت اول: سی‌پنل + easyapache 3 + php selector

اگر شما از کنترل‌پنل قدرتمند سی‌پنل به همراه easyapache 3 استفاده می‌کنید (گر چه easyapaceh 3 دیگر پشتیبانی نمی‌شود ولی ممکن است هنوز بعضی‌ها به easyapache 4 مهاجرت نکرده باشند) و php selector راه هم نصب کرده‌اید باید با دستور زیر فایل هندلر php را بسازید.

```
application/x-lsphp56 /opt/alt/php56/usr/bin/lsphp
application/x-lsphp70 /opt/alt/php70/usr/bin/lsphp
application/x-lsphp71 /opt/alt/php71/usr/bin/lsphp
application/x-lsphp72 /opt/alt/php72/usr/bin/lsphp
```

بعد از ساخت این فایل کدهای زیر داخل آن کپی کنید. دقت کنید ما اینجا از نسخه 5.6 تا نسخه 7.2 را وارد کرده‌ایم. اگر در آینده نسخه 5.6 به پایان دوره حیات خود رسید و نسخه‌های جدیدتر هم از راه رسیدند می‌توانید خودتان کد را تغییر بدهید.

```
application/x-lsphp56 /opt/alt/php56/usr/bin/lsphp
application/x-lsphp70 /opt/alt/php70/usr/bin/lsphp
application/x-lsphp71 /opt/alt/php71/usr/bin/lsphp
application/x-lsphp72 /opt/alt/php72/usr/bin/lsphp
```

سپس دکمه کنترل را به همراه X بگیرید و فایل را ذخیره کنید و در ادامه آپاچی را هم با دستور زیر ریستارت کنید.

```
service httpd restart
```

#### حالت دوم: سی‌پنل + easyapache4

در این حالت شما از قبل فایل هندلر php را روی سرور دارید و داخل آن محتویات زیر وجود دارد.

```
application/x-httpd-ea-php56-lsphp /opt/cpanel/ea-php56/root/usr/bin/lsphp
application/x-httpd-ea-php70-lsphp /opt/cpanel/ea-php70/root/usr/bin/lsphp
application/x-httpd-ea-php71-lsphp /opt/cpanel/ea-php71/root/usr/bin/lsphp 
application/x-httpd-ea-php72-lsphp /opt/cpanel/ea-php72/root/usr/bin/lsphp
```

#### حالت سوم: سی‌پنل + easyapache3

در این حالت کدهای مورد نظر در فایل هندلر به شکل زیر خواهد بود.

```
application/x-lsphp56 /opt/cpanel/ea-php56/root/usr/bin/lsphp
application/x-lsphp70 /opt/cpanel/ea-php70/root/usr/bin/lsphp
application/x-lsphp71 /opt/cpanel/ea-php71/root/usr/bin/lsphp
application/x-lsphp72 /opt/cpanel/ea-php72/root/usr/bin/lsphp
```

دقیقاً مثل حالت اول فایل را ذخیره و آپاچی را ریستارت کنید.

خب تا اینجای کار فایل هندلر را تنظیم کردیم. حالا باید در فایل htaccess خودمان یک کدی را اضافه کنیم و به cms بفهمانیم که از چه نسخه php استفاده کند.

اگر از easyapache 3 استفاده می‌کنیم باید کد زیر را در ابتدای فایل htaccess بگذاریم. نسخه را خودتان می‌توانید تغییر دهید. در حال حاضر روی php 7.1 قرار داده شده است.

```
<FilesMatch "\.(php4|php5|php3|php2|php|phtml)$">
SetHandler application/x-lsphp71
</FilesMatch>
```

اگر هم از easyapache 4 استفاده می‌کنیم باید کد زیر را در ابتدای فایل htaccess بگذاریم.

```
<FilesMatch "\.(php4|php5|php3|php2|php|phtml)$">
SetHandler application/x-httpd-ea-php71-lsphp
</FilesMatch>
```

**نکته اول**: اگر شما مدیر سرور نیستید و یک کاربر عادی هستید از مدیر سرور خود سوال کنید که سرور از چه نسخه‌های php و چه مودی استفاده می‌کند. بر اساس آن فقط کافیست مرحله آخر را انجام دهید.

**نکته دوم**: اگر شما در داخل دامنه خود چند پوشه داشته باشید و فایل htaccess را با این روش تغییر دهید تمام پوشه‌های زیر مجموعه هم دچار تغییر خواهند شد. اگر نمی‌خواهید این اتفاق بیفتد باید برای هر پوشه این کار را به صورت جداگانه تکرار کنید.
