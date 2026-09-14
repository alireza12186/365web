---
layout: post
title: "حل مشکل لود بالای سرور با آنتی‌ویروس ClamAV"
slug: "high-load-in-clamav"
date: "2022-09-28T12:05:33"
categories: ["لینوکس"]
tags: ["آنتی‌ویروس ClamAV", "سرور", "لود بالای سرور"]
image: /assets/images/blog/clamav-1-1024x559.png
---
بسیاری از مدیران سرور روی سرورهای لینوکسی از آنتی‌ویروس [ClamAV](https://www.clamav.net/) استفاده می‌کنند. دلیل آن هم ساده است. این آنتی‌ویروس هم بسیار ساده و کاربردی است. و هم اینکه رایگان است. ضمن اینکه اگر از آنتی‌شل‌هایی مثل CXS و Maldet هم استفاده کنید نصب آنتی‌ویروس ClamAV ضروری است.  
اما گاهی آنتی‌ویروس ClamAV باعت افزایس مصرف سی‌پی‌یو و رم سرور می‌شود و در نتیجه لود [سرور](https://365web.ir/vps-hosting/) بالا می‌رود. بالا رفتن لود سرور هم باعث درست اجرا نشدن برنامه‌ها، تاخیر در اجرای برنامه‌ها و حتی کرش کردن سرور می‌شود.  
برای حل این مشکل می‌توان از یک روش بسیار ساده استفاده کنیم.  
کافیست که اسکریپت استارتاپ آنتی‌ویروس ClamAV را باز کنیم.

```
nano /etc/systemd/system/clamd.service
```

و مقادیر زیر را به بخش Service اضافه کنیم.

```
IOSchedulingPriority = 7
CPUSchedulingPolicy = 5
MemoryLimit=256M
CPUQuota=30%
Nice = 19
```

البته عددها را با توجه به نیاز خودتان می‌توانید تغییر بدهید.

اسکریپت شما در نهایت باید به شکل زیر باشد.

```
[Unit]
Description=clamd antivirus daemon
ConditionPathExists=!/etc/clamddisable
After=network-online.target

[Service]
Type=simple
TimeoutSec=300
EnvironmentFile=/etc/sysconfig/exim
ExecStart=/usr/local/cpanel/3rdparty/bin/clamd -F
IOSchedulingPriority = 7
CPUSchedulingPolicy = 5
MemoryLimit=256M
CPUQuota=30%
Nice = 19


# monitor and restart service similar to tailwatchd
Restart=always
RestartSec=30

[Install]
WantedBy=multi-user.target
```

در نهایت آنتی‌ویروس را با دستور زیر ریستارت کنید.

```
systemctl restart clamd
```
