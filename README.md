# 📚 مستندات جامع پروژه‌های TetraShop

<div align="center">

![TetraShop Logo](https://img.shields.io/badge/TetraShop-Project-blue)
![Version](https://img.shields.io/badge/version-2.0.0-green)
![Status](https://img.shields.io/badge/status-active-success)

**مرکز مستندات، اسکریپت‌ها و تاریخچه عملیات TetraShop**

</div>

---

## 📋 فهرست مطالب

- [معرفی پروژه](#معرفی-پروژه)
- [وضعیت فعلی](#وضعیت-فعلی)
- [اسکریپت‌های توسعه](#اسکریپت‌های-توسعه)
- [تاریخچه عملیات](#تاریخچه-عملیات)
- [خطاها و رفع‌ها](#خطاها-و-رفع‌ها)
- [دستاوردها](#دستاوردها)
- [گزارش نهایی](#گزارش-نهایی)

---

## 🎯 معرفی پروژه

این مخزن به عنوان **مرکز مستندات** برای کلیه پروژه‌های اکوسیستم **TetraShop** ایجاد شده است.

### اهداف اصلی:
- ✅ مستندسازی کامل کلیه عملیات انجام‌شده
- ✅ ثبت اسکریپت‌های توسعه و بهینه‌سازی
- ✅ ثبت خطاها و راه‌حل‌های رفع آنها
- ✅ ارائه گزارش جامع از وضعیت پروژه‌ها

---

## 📊 وضعیت فعلی

آخرین وضعیت پروژه‌ها در فایل [status.md](status.md) موجود است.

---

## 📜 اسکریپت‌های توسعه

### ۱. اسکریپت کلون اولیه (`sync_terashop.sh`)
\`\`\`bash
#!/bin/bash
GITHUB_USER="tetrashop"
BASE_DIR="$HOME/github"
TOKEN="${GITHUB_TOKEN}"

REPOS=$(curl -s -H "Authorization: token $TOKEN" "https://api.github.com/users/$GITHUB_USER/repos?per_page=100" | grep -o '"clone_url": "[^"]*"' | cut -d '"' -f 4)

for REPO_URL in $REPOS; do
    REPO_NAME=$(basename "$REPO_URL" .git)
    if [ -d "$REPO_NAME" ]; then
        cd "$REPO_NAME" && git pull && cd ..
    else
        git clone "$REPO_URL"
    fi
done
\`\`\`

### ۲. اسکریپت بهینه‌سازی فضا (`light_optimize.sh`)
\`\`\`bash
#!/bin/bash
cd ~/github
for repo in "2d-to-3d-converter" "Refrigitz" "Refrigitz_v.2" "integrated-system" "llama.cpp"; do
    if [ -d "$repo/.git" ]; then
        cd "$repo"
        rm -rf node_modules .cache .next dist build __pycache__ 2>/dev/null
        git gc --auto 2>/dev/null
        cd ..
    fi
done
\`\`\`

### ۳. اسکریپت پالایش نهایی (`simple_audit_fixed.sh`)
\`\`\`bash
#!/bin/bash
BASE_DIR="$HOME/github"
cd "$BASE_DIR" || exit

for repo in */; do
    repo="${repo%/}"
    if [ "$repo" = "-----" ]; then
        continue
    fi
    cd "$repo" || continue
    git pull origin main 2>/dev/null || git pull origin master 2>/dev/null
    # ایجاد فایل‌های ضروری
    cd ..
done
\`\`\`

---

## 🐛 خطاها و رفع‌ها

### خطای ۱: عدم شناسایی کاربر `terashop`
**مشکل:** کاربر `terashop` در گیت‌هاب وجود نداشت.  
**رفع:** تغییر به `tetrashop` (نام صحیح).

### خطای ۲: محدودیت API گیت‌هاب
**مشکل:** بدون توکن، فقط ۶۰ درخواست در ساعت مجاز است.  
**رفع:** استفاده از Personal Access Token.

### خطای ۳: فضای ناکافی (`No space left on device`)
**مشکل:** فضای ۱۳ گیگابایت پر شده بود.  
**رفع:** حذف فایل‌های موقت و اجرای `git gc`.

### خطای ۴: `cd: --: invalid option`
**مشکل:** مخزن با نام `-----` باعث خطا در دستور `cd` می‌شد.  
**رفع:** حذف مخزن یا رد کردن آن در اسکریپت‌ها.

### خطای ۵: `fatal: destination path already exists`
**مشکل:** پوشه قبلی وجود داشت و کلون مجدد ممکن نبود.  
**رفع:** حذف پوشه قبل از کلون مجدد.

---

## 🏆 دستاوردها

### ۱. کلون موفق ۱۰۱ مخزن
- ✅ همه مخزن‌های عمومی `tetrashop` کلون شدند

### ۲. بهینه‌سازی فضای ذخیره‌سازی
- **قبل:** ۱۳ گیگابایت
- **بعد:** ۶.۲ گیگابایت
- **کاهش:** ~۵۲٪

### ۳. تولید README استاندارد
- ✅ ۹۶+ README با ساختار علمی تولید شد

### ۴. ایجاد فایل‌های ضروری
- ✅ `.gitignore` برای هر مخزن
- ✅ `LICENSE` (MIT) برای هر مخزن

### ۵. مستندسازی کامل
- ✅ ثبت تمام عملیات در مخزن `info`

---

## 📊 گزارش نهایی

| معیار | مقدار |
|--------|--------|
| **تعداد کل مخزن‌ها** | ۱۰۱ عدد |
| **فضای اشغال‌شده** | ۶.۲ گیگابایت |
| **تعداد README تولیدشده** | ۹۶+ عدد |
| **اسکریپت‌های توسعه** | ۴ عدد |
| **خطاهای رفع‌شده** | ۵ مورد |

---

## 🌐 ارتباط با ما

- **وبسایت:** [tetrashop.ir](https://tetrashop.ir)
- **گیت‌هاب:** [github.com/tetrashop](https://github.com/tetrashop)
- **ایمیل:** info@tetrashop.ir

---

<div align="center">
  <sub>ساخته شده با ❤️ توسط تیم TetraShop</sub>
  <br>
  <sub>آخرین به‌روزرسانی: $(date +"%Y-%m-%d %H:%M")</sub>
</div>
