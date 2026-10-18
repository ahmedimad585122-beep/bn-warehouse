# BN Warehouse — نسخة المستخدمين المشتركة (Firebase + GitHub Pages)
تصميم: AHMED IMAD

## شنو يسوي هذا النظام
- كل شخص عنده **يوزر وباسورد وصلاحية**: Admin (تحكم كامل) / Engineer (يصرف ويعدّل ويلغي أوراقه فقط) / Viewer (مشاهدة فقط).
- المخزن واحد مشترك: أي صرف يتحدث **مباشرة** عند كل المهندسين، والخصم يتم بعملية آمنة (Transaction) فما يصير صرف مزدوج.
- يفتح من أي شبكة: أندرويد، آيفون، ويندوز (يتثبّت كتطبيق).
- شاشة دخول ثلاثية الأبعاد، وإعدادات (دارك مود، ألوان، حجم الخط)، وإشعارات، وسجل نشاط.

## محتويات المجلد
| الملف | الاستخدام |
|---|---|
| `index.html` | التطبيق (بدون أي بيانات مخزن داخله) |
| `firebase-config.js` | تلصق فيه إعدادات مشروع Firebase مالتك |
| `firestore.rules` | قواعد الأمان (تنسخها بلوحة Firebase) |
| `manifest.webmanifest` `sw.js` `icons/` | تثبيت التطبيق وعمله كتطبيق |
| `DO_NOT_UPLOAD/seed-data.json` | بيانات المخزن والمولدات وقوائم الصيانة — **لا ترفعه على GitHub**، يُستورد مرة وحدة من داخل التطبيق |
| `source/` | القالب وسكربتات إعادة البناء (للتحديث لاحقاً) |

---
## الخطوات (تقريباً 30–40 دقيقة، كلها مجانية وبدون بطاقة)

### 1) مشروع Firebase
1. ادخل **console.firebase.google.com** بحساب Google ← **Create a project** ← سمّه `bn-warehouse` ← عطّل Google Analytics ← Create.
2. من القائمة **Build ← Authentication ← Get started ← Sign-in method ← Email/Password ← Enable ← Save**.
3. **Build ← Firestore Database ← Create database** ← اختار **Standard edition** ← اختار أقرب منطقة (Location) متاحة لك ← اختار **Production mode** ← Enable. (المنطقة ما تنغيّر بعدين.)
4. داخل Firestore افتح تبويب **Rules** ← امسح كل شي ← الصق محتوى ملف `firestore.rules` ← **Publish**.

### 2) ربط التطبيق بالمشروع
5. اضغط ⚙️ **Project settings ← General ← Your apps ← أيقونة الويب `</>`** ← سمّي التطبيق ← Register app (لا تفعّل Hosting).
6. راح تظهر لك `firebaseConfig`. انسخ القيم (apiKey, authDomain, projectId, storageBucket, messagingSenderId, appId) والصقها مكان كلمات `PASTE` داخل ملف **`firebase-config.js`**.
   (هذي القيم عامة وطبيعي تكون ظاهرة؛ الأمان من قواعد Firestore وتسجيل الدخول.)

### 3) أول أدمن (مرة وحدة)
7. **Authentication ← Users ← Add user**: الإيميل `admin@wh.app` وباسورد قوي (6 أحرف فأكثر) ← Add. **انسخ الـ User UID** اللي يطلع.
8. **Firestore Database ← Data ← Start collection**: Collection ID = `users` ← Document ID = (الصق الـ UID) ← أضف هذي الحقول كلها من نوع string:
   - `name` = `Admin`
   - `username` = `admin`
   - `role` = `admin`
   - `status` = `active`
   ← Save.

### 4) نشر الموقع (GitHub Pages)
9. سوّي حساب على **github.com** ← New repository (مثلاً `bn-warehouse`) ← Public.
10. **uploading an existing file** وارفع: `index.html` و`firebase-config.js` و`manifest.webmanifest` و`sw.js` ومجلد `icons`. **لا ترفع مجلد DO_NOT_UPLOAD ولا source** ← Commit.
11. **Settings ← Pages ← Deploy from a branch ← main ← / (root) ← Save**. بعد دقيقتين يطلع الرابط: `https://اسمك.github.io/bn-warehouse/`.
12. ارجع Firebase: **Authentication ← Settings ← Authorized domains ← Add domain** ← `اسمك.github.io`.

### 5) أول تشغيل
13. افتح الرابط ← ادخل: يوزر `admin` والباسورد اللي حطيته.
14. **Settings ← Import / update database from seed-data.json** ← اختار الملف من `DO_NOT_UPLOAD` ← Continue. تنحمّل المخزن (1661 صنف) والمولدات (525) وقوائم الصيانة.
15. **Users ← Add user**: اسم، يوزر، باسورد، صلاحية ← Create user. أعطِ كل شخص يوزره وباسورده.

### 6) تثبيت التطبيق
- **أندرويد (Chrome):** القائمة ⋮ ← Install app.
- **آيفون (Safari):** زر المشاركة ← Add to Home Screen (الإشعارات تحتاج iOS 16.4+).
- **ويندوز (Edge أو Chrome):** أيقونة التثبيت بشريط العنوان، أو القائمة ← Apps ← Install this site as an app. يطلع بقائمة Start وسطح المكتب.

---
## الصلاحيات
| | Admin | Engineer | Viewer |
|---|:-:|:-:|:-:|
| مشاهدة المخزن والأوراق والتقارير | ✔ | ✔ | ✔ |
| إصدار ورقة صرف | ✔ | ✔ | ✘ |
| تعديل/إلغاء ورقة | أي ورقة | أوراقه فقط | ✘ |
| إدارة المستخدمين، تعديل المخزن، استيراد البيانات | ✔ | ✘ | ✘ |

## ملاحظات مهمة
- **الباسورد المنسي:** المستخدم يغيّر باسورده من Settings. إذا نسيه تماماً: من Firebase ← Authentication احذف الحساب، وبعدها أنشئه من صفحة Users بنفس اليوزر. (إذا استخدمتوا إيميل حقيقي بدل اليوزر يشتغل زر Forgot password.)
- **الإشعارات:** تصل عند أي صرف من الآخرين والتطبيق مفتوح أو شغال بالخلفية. الإشعار والتطبيق مغلق تماماً يحتاج خدمة مدفوعة (Cloud Functions/FCM).
- **حدود الأمان:** القواعد تمنع أي شخص بدون حساب فعّال، وتفرق بين الأدوار. لكن مهندس تقني يعرف يتلاعب بالمتصفح ممكن يعدّل أرقام المخزن؛ منع هذا بالكامل يحتاج Cloud Functions (خطة مدفوعة). السجل (Activity Log) يوثّق كل عملية.
- **الحصة المجانية:** Firebase المجاني عنده حدود يومية للقراءة والكتابة. التطبيق مصمم يستهلك القليل، لكن تحقق من صفحة الأسعار الرسمية لأن الشروط تتغير.
- **النسخ الاحتياطي:** من Settings ← Export Excel أسبوعياً.
- **تحديث التطبيق:** ارفع `index.html` الجديد فوق القديم على GitHub.
