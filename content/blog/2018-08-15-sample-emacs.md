---
utid: 20180815222545
date: 2018-08-15 22:25:45
title: ایمکس، خیلی ساده
_index: emacs 
description: یک راهنمای ساده برای استفاده از ایمکس در ترمینال، با تمام امکانات IDEهای گرافیکی مرسوم
contribute_offer: yes
categories:
  - video
  - emacs
tags:
  - emacs
  - manual
  - ایمکس
  - راهنما
---
برای خیلی‌ها زمان‌هایی پیش اومده که به محیط گرافیکی دسترسی نداشتن، مثلا از راه دور به سرور روصل شده باشید، و بخواید یک کد یا فایل‌های تنظیمات و... رو تغییر بدید، برای خیلی‌هامون که این‌روزها به ابزار گرافیکی و IDEهای به اصطلاح مدرن امروزی عادت کردیم، این ویرایش فایل یکی از سخت‌ترین کارهای دنیاست، محیط‌های متفاوت با کلیدها و میانبرهای متفاوت از چیزی که بهش عادت کردیم.

توی این ویدیو سعی داریم emacs رو برای استفاده توی ترمینال آماده کنیم با این هدف که ظاهر، میانبرها، کلیدها و ابزاری که ازشون استفاده میکنیم، خیلی شبیه بشه به عادت‌های روزانه‌ای که ابزارهای گرافیکی برامون درست کردن. برای مثال کلید کپی همون کنترل+سی معروف باشه و...

<div id="15343631753963214"><script type="text/JavaScript" src="https://www.aparat.com/embed/jdLt7?data[rnddiv]=15343631753963214&data[responsive]=yes"></script></div>

[آدرس ویدیو در یوتیوب](https://youtu.be/kweT_lKHFYA)

[آدرس ویدیو در آپارات](https://www.aparat.com/v/jdLt7)

برای شروع یک فایل با نام `.emacs` رو توی دایرکتوری home خودمون درست میکنیم . و این چند خط رو بهش اضافه می‌کنیم

```
(setq cua-auto-tabify-rectangles nil) ;; Don't tabify after rectangle commands
(transient-mark-mode 1) ;; No region when it is not highlighted
(setq cua-keep-region-after-copy t) ;; Standard Windows behaviour

(cua-mode t)
```

با کلیدهای `ctrl+x ctrl+c` ایمکس رو میبندیم و دوباره اجرا میکنیم، از این به بعد کلیدهای کپی و پیست و کات و آندو و... همه به شکلی که میشناسیم کار میکنن، یعنی با ctrl+c - ctrl+v , ctrl+z و...

برای نمایش شماره هر خط، در ابتدا، این خطوط رو به `.emacs` اضافه میکنیم:

```
(global-linum-mode t)
(setq linum-format "%4d \u2502 ")
```

و بعد چندتا پکیج رو نیاز داریم که نصب کنیم که برای این هم این چند خط رو به به فایل `.emacs` اضافه مکینم

```
(require 'package)
(let* ((no-ssl (and (memq system-type '(windows-nt ms-dos))
                    (not (gnutls-available-p))))
       (proto (if no-ssl "http" "https")))
  ;; Comment/uncomment these two lines to enable/disable MELPA and MELPA Stable as desired
  (add-to-list 'package-archives (cons "melpa" (concat proto "://melpa.org/packages/")) t)
  ;;(add-to-list 'package-archives (cons "melpa-stable" (concat proto "://stable.melpa.org/packages/")) t)
  (when (< emacs-major-version 24)
    ;; For important compatibility libraries like cl-lib
    (add-to-list 'package-archives '("gnu" . (concat proto "://elpa.gnu.org/packages/")))))
(package-initialize)
```

فایل رو ذخیره میکنیم و ایمکس رو میبندیم و دوباره راه اندازی میکنیم و با این دستورها این ۳ پکیج رو نصب میکنیم

```
package-install  (enter)
tabbar (enter)

package-install  (enter)
neotree (enter)

package-install  (enter)
sublime-theme (enter)
```

و این چند خط رو به فایل `.emacs` اضافه میکنیم

```
(defun my-tabbar-buffer-groups () ;; customize to show all normal files in one group
  (list (cond ((string-equal "*" (substring (buffer-name) 0 1)) "emacs")
              ((eq major-mode 'dired-mode) "emacs")
              (t "user"))))
(setq tabbar-buffer-groups-function 'my-tabbar-buffer-groups)

(tabbar-mode 1)

;;;;

(require 'neotree)
(global-set-key [f5] 'neotree-toggle)



;;need tabbar for active forward / backward
(global-set-key [f6] 'tabbar-backward-tab)
(global-set-key [f7] 'tabbar-forward-tab)

(global-set-key [f8] 'kill-buffer)
(global-set-key [f9] 'kill-buffer-and-window)

;;;

(load-theme 'hickey t) ;; from sublime-theme package
```

ایمکس رو میبندیم و مجددا باز میکنیم. تقریبا تمام شد، برای اینکه تنظیمات بیشتری رو هم داشته باشیم این فایل رو میتونید یه نگاه بندازید

[.emacs](https://kiavash.one/assets/emacs.txt)
