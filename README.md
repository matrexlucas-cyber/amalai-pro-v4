# إدارة عملائي Pro — Capacitor Android starter

هذه حزمة هيكلية قابلة للرفع إلى GitHub وتشغيل workflow لبناء APK تجريبي.

## الملفات
- `www/index.html`: واجهة التطبيق الحالية.
- `package.json`: تبعيات Capacitor Core وAndroid وCLI وLocal Notifications.
- `capacitor.config.json`: تعريف التطبيق ومجلد الويب.
- `.github/workflows/android.yml`: workflow يقوم بتثبيت Node/Java، وإضافة Android، ثم بناء APK debug ورفعه كـ Artifact.

## بناء APK من GitHub
1. ارفع محتويات ZIP إلى مستودع GitHub (على الفرع `main` أو `master`).
2. افتح تبويب **Actions**.
3. اختر **Build Android APK** ثم **Run workflow**، أو ادفع commit جديدًا.
4. بعد نجاح البناء افتح تشغيل workflow ثم قسم **Artifacts** ونزّل `amalai-pro-debug-apk`.

## ملاحظات
- هذا المشروع يستخدم إنشاء منصة Android داخل GitHub Actions عبر `npx cap add android`؛ لذلك لا يتضمن مجلد `android/` مولّدًا مسبقًا.
- الناتج APK من نوع **debug** للاختبار، وليس APK موقّعًا للنشر في Google Play.
- تمت إضافة حزمة Capacitor Local Notifications إلى dependencies، لكن الواجهة الحالية لا تزال تستخدم آلية التذكيرات الموجودة في HTML. وجود الحزمة وحده لا يفعّل جدولة Android الأصلية ولا يضمن التنبيهات عند إغلاق التطبيق. يلزم دمج API الخاص بها في التطبيق واختباره على جهاز فعلي.
- التخزين الحالي ما زال يعتمد على `localStorage` داخل WebView. لا تتضمن هذه الحزمة مزامنة سحابية أو قاعدة بيانات خارجية.
- يجب مراجعة توافق نسخ Capacitor والحزم عند تحديث المشروع، كما أن البناء يعتمد على تنزيل Gradle وAndroid SDK داخل GitHub Actions.

### حل مشكلة Dependencies lock file is not found

تم تعديل `.github/workflows/android.yml` لإزالة `cache: npm` من `actions/setup-node`، لأن هذا الخيار يحتاج ملف `package-lock.json` أو ملف lock متوافق. خطوة تثبيت الحزم تستخدم `npm install` مباشرة، لذلك لا يلزم رفع ملف lock لحل هذه المشكلة.
