---
utid: 20200516121701
date: 2020-05-16 12:17:01
title: حذف نسخه‌های قدیمی برنامه‌های snap
_index: remove old snap versions to free up disk space
description: آزاد کردن فضای ذخیره سازی، یا همان خالی کردن هارد دیسک، با حذف نسخه‌های قدیمی برنامه‌های نصب شده توسطsnap
contribute_offer: yes
categories:
  - گنو/لینوکس
tags:
  - snap
  - disk
  - گنو/لینوکس
  - اوبونتو
  - اسنپ
---
موقع کار با کامپیوتر یک دسته از پیغام‌هایی که بارها و بارها با اونها مواجه شدیم، انواع اخطارهای فضای ذخیره سازی هستن که یادآوری میکنن فضای دیسک در حال پر شدنه و دیگه جایی برای ذخیره کرن اطلاعات یا نصب برنامه‌های جدید وجود نداره.  
خوب یک روز که اصلا انتظار و توقعش رو هم نداشتم، من با یکی از همین اخطارها روبرو شدم که بهم میگفت روی درایوی که دایرکتوری `root` یا همون `/` رو پیکربندی کرده بودم جای خالی دیگه وجود نداره، و این در حالی بود که مدت‌ها بود برنامه یا کتابخونه‌ی جدیدی رو نصب نکرده بودم و `log` ها هم مرتب تمیزکاری شده بودند و آخرین باری هم که چک کرده بودم نزدیک به ۶۰درصد فضای درایو خالی بود، پس منطقا نباید همچین پیغامی از ناکجا ظاهر میشد.

خوب اولین کاری که کردم چک کردن فضای هارد با کمک نرم‌افزار [Baobab](https://wiki.gnome.org/action/show/Apps/DiskUsageAnalyzer) یا همون 'disk usage analyzer' معروف که به شکل پیش‌فرض همراه میز کار گنوم نصب میشه، بود و با یک موضوع خیلی جالب روبرو شدم، حجم بالای دایرکتوری اسنپ(snap)  
`/var/lib/snapd/snaps/`

با یک جستجوی سریع متوجه شدم که اسنپ ۳ نسخه‌ی آخر هر برنامه‌ای که نصب کرده رو نگه میداره و حذفشون نمیکنه. یعنی تا ۲ بار بعد از آپدیت، ورژن قبلی برنامه رو همچنان به شکل کامل نگهداری میکنه و میتونید به جز آخرین ورژن به ۲نسخه‌ی قبلش هم دسترسی داشته باشید.

دونکته در مورد نگهداری ورژن‌های قبلی:

- نگهداری نسخه‌های قدیمی شامل نصب برنامه‌های جدید نمیشه، یعنی موقع نصب کردن فقط همون آخرین نسخه رو نصب میکنه، اما وقتی به آپدیت کردن میرسه، به شکل پیش‌فرض از این قانون پیروی میکنه و تا سقف ۳نسخه رو نگهداری میکنه
- دوم هم اینکه از نسخه‌ی `2.34` به بعد خیلی راحت میشه این پیش‌فرض رو تغییر داد.

پس اولین کاری که کردم اصلاح این موضوع بود، از نسخه ۲/۳۴ به بعد، خیلی راحت با تغییر مقدار `refresh.retain` میشه تعداد نسخه‌هایی که قراره نگهداری بشه رو کم یا زیاد کرد.

	sudo snap set system refresh.retain=2

من این گزینه رو از ۳ به ۲ تغییر دادم، یعنی آخرین نسخه و یک نسخه قبل از اون رو داشته باشم، به هرحال پیش اومده بعد از آپدیت کردن بعضی از برنامه‌ها مشکلاتی پیش بیاد، مثلا برنامه درست کار نکنه، یا از تغییرات ورژن جدید خوشتون نیومده باشه یا به هر دلیل دیگه‌ای بخواید به نسخه‌ی قبلی سوییچ کنید.

***

خوب تا الان مشکلات پیش نیومده و احتمال پر شدن هارد دیسکم در آینده رو حل کردم، اما خوب هنوز ورژن‌های قبلی هم روی هارد هستند و من همچنان با اخطار کم بودن فضای دیسک درگیرم، با گرفتن لیست کامل از اسنپ میتونیم تمام نسخه‌های نصب شده‌ی فعال و غیر فعال رو ببینیم:

	snap list --all

خروجی چیزی شبیه به این باید باشه:

	Name  Version  Rev  Tracking  Publisher     Notes
	atom  1.46.0   250  stable    snapcrafters  classic
	vlc   3.0.9    768  stable    videolan✓     disabled
	vlc   3.0.9-1  768  stable    videolan✓     disabled
	vlc   3.0.9-2  770  stable    videolan✓     -
	...
	...

همونطوری که مشخصه، نسخه‌های قدیمی‌تر که در اصل نسخه‌ی غیر فعال شده هستن، با متن 'disabled' در ستون آخر مشخص شدن، برای دیدن فقط این نسخه‌ها میتونیم یه فیلتر کوچیک روی خروجی اعمال کنیم که اگر توی متن ستون آخر کلمه disabled استفاده شده بود، اون خط رو نمایش بده:

	snap list --all | perl -lanE 'say if $F[5] =~ /disabled/'

الان باید خروجی شبیه به این شده باشه:

	vlc   3.0.9    768  stable    videolan✓    disabled
	vlc   3.0.9-1  768  stable    videolan✓    disabled

حالا فقط کافیه که این نسخه‌ها رو حذف کنم، برای حذف کردن یک نسخه خاص از بین تمام نسخه‌های نصب شده در اسنپ میتونیم از دستوری با الگوی زیر استفاده کنیم:

	snap remove "Name" --revision="Rev"

که جای `Name` و `Rev` باید اسم برنامه و ریویژنی که میخوایم حذف بشه رو بنویسیم، از اونجایی که توی لیست نمایش داده شده، ستون اول اسم برنامه و ستون سوم ریویژن اون برنامه هست، پس فیلتری که نوشته بودم رو با این دستور ترکیب کردم و به این دستور رسیدم:

``` bash
snap list --all | perl -lanE 'say qx{snap remove "$F[0]" --revision="$f[2]"} if $F[5] =~ /disabled/'

# --- توضیحات ---
# شمارش ستون‌ها رو از صفر شروع میکنیم، پس
# F[0]: ستون اول - اسم برنامه
# F[2]: ستون سوم - ورژن مورد نظرمون
# F[5]: ستون ششم، فعال یا غیرفعال بودن این ورژن
```

**توصیه:** بهتره قبل از حذف ورژن‌های قدیمی، مطمین باشیم تمام اسنپ‌های در حال اجرا رو متوقف کردیم

تمام. نزدیک به ۴۰ گیگ فضای دیسک آزاد شد :)

	atom (revision 223) removed
	atom (revision 222) removed
	bitwarden (revision 15) removed
	bitwarden (revision 16) removed
	canonical-livepatch (revision 50) removed
	canonical-livepatch (revision 54) removed
	chromium (revision 607) removed
	chromium (revision 660) removed
	core (revision 6531) removed
	core (revision 6405) removed
	core18 (revision 719) removed
	core18 (revision 731) removed
	gallery-dl (revision 36) removed
	gallery-dl (revision 167) removed
	gimp (revision 110) removed
	gimp (revision 113) removed
	...
	...

***

پی‌نوشت:

قبل از انتشار برای اطمینان از درست بودن تمام چیزهایی که نوشتم یه جست و جوی نهایی کردم و به این مطلب رسیدم، [How To Remove Old Snap Versions To Free Up Disk Space](https://www.linuxuprising.com/2019/04/how-to-remove-old-snap-versions-to-free.html) که با توجه به چیزی که توی اون مقاله نوشته شده، برای استفاده بدون دردسر روی سیستم‌هایی که زبان محلی سیستم‌عامل چیزی به جز انگلیسی هست میتونیم یه تغییر خیلی کوچیک دیگه هم اضافه کنیم و اسنپ رو به حالت انگلیسی اجرا کنیم:

	LANG=en_US.UTF-8 snap list --all | perl -lanE 'say qx{snap remove "$F[0]" --revision="$f[2]} if $F[5] =~ /disabled/'

توی اون مقاله برای حذف کردن ورژن‌های قدیمی اسنپ، از یک اسکریپت `bash` به عنوان راه حل استفاده کرده، که به شکل خلاصه توضیحش میدم:

**‍۱-** یه فایل به اسم `remove-old-snaps` بسازید

	touch remove-old-snaps

**۲-** فایل رو باز کنید و متن زیر رو کپی-پیست کنید توی فایل جدید

	#!/bin/bash
	# Removes old revisions of snaps
	# CLOSE ALL SNAPS BEFORE RUNNING THIS
	set -eu

	LANG=en_US.UTF-8 snap list --all | awk '/disabled/{print $1, $3}' |
		while read snapname revision; do
			snap remove "$snapname" --revision="$revision"
	done

**۳-** این فایل رو با کمک دستور زیر قابل اجرا میکنیم:

	chmod +x remove-old-snaps

**۴-** و  با دسترسی root اجراش میکنیم

	sudo ./remove-old-snaps
