---
utid: 20200225210317
date: 2020-02-25 21:03:17
title: zsh: فایل خراب شده‌ تاریخچه
_index:  zsh corrupt history file
description: چگونه فایل آسیب دیده‌ی تاریخچه‌ی zsh را درست کنیم
contribute_offer: no
categories:
  - linux
tags:
  - zsh
  - shell
  - linux
  - لینوکس
---
اگر از zsh استفاده می‌کنید یکی از مشکلاتی که ممکن است هراز گاهی با آن روبرو شوید یک فایل تاریخچه‌ی خراب شده است که مانع دسترسی شما به تاریخچه دستورات تایپ شده و اجرای صحیح عملکردها و دستورهایی مانند `history`، `fc` یا `Crtl+R` می‌شود.

خطایی که با آن مواجه می‌شوید چیزی شبیه به خط زیر است:

	zsh: corrupt history file /home/kiavash/.zsh_history

### چه کار کنیم؟

حل این مشکل یک راه  حل ساده در ۴قدم دارد،

- به دایرکتوری خانه برگردید،
- فایل تاریخچه را تغییر نام دهید.
- تاریخچه را بازسازی کنید.

```
cd ~
mv .zsh_history .zsh_history_bad
strings .zsh_history_bad > .zsh_history
fc -R .zsh_history
```

- بعد از چک کردن درستی مراحل، فایل تغییر نام داده شده را هم پاک می‌کنیم.

```
rm ~/.zsh_history_bad
```

### ساختن یک اسکریپت

اگر برای شما هم مثل من بیشتر از چندبار این اتفاق افتاده و حوصله تکرار این مراحل را هم ندارید، می‌توانید تمام مراحل بالا را در قالب یک اسکریپت ذخیره کنید و یک بار برای همیشه خودتان را راحت کنید.

برای این‌کار در دایرکتوری `~/bin` یا هرجای دیگری که برای اجرای اسکریپت‌های دستوری در `$PATH` خود مشخص کرده‌اید، یک فایل با اسمی شبیه به `zsh_history_fix` بسازید و تمام مراحل بالا را در آن ذخیره کنید.


	cd ~/bin
	touch zsh_history_fix

فایل `zsh_history_fix` را با هر ادیتوری که می‌خواهید باز کنید و چند خط‌ زیر را در آن کپی کنید

```zsh
#!/usr/bin/env zsh

mv ~/.zsh_history ~/.zsh_history_bad
strings ~/.zsh_history_bad > ~/.zsh_history
fc -R ~/.zsh_history
rm ~/.zsh_history_bad
```

و در نهایت فایل ساخته شده را قابل اجرا کنید

	chmod +x zsh_history_fix


* * *

#### منبع انگلیسی

[How to fix a corrupt zsh history file](https://shapeshed.com/zsh-corrupt-history-file/)
{= .left-para }
