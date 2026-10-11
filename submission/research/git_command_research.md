# Git Command Deep-Dive

**Simulation Academy — .NET Diploma (Cycle 1)**  
**Assignment 01 — Git Mastery & Problem Solving**  
**Student:** Belal Fayez

## 1. Git Squash

### Definition

<p dir="rtl" align="right">دمج عدة <bdi dir="ltr">Commits</bdi> في <bdi dir="ltr">Commit</bdi> واحدة لتبسيط تاريخ التغييرات. لا يوجد أمر مستقل اسمه <code dir="ltr"><bdi dir="ltr">git squash</bdi></code>؛ من الطرق الشائعة <code dir="ltr"><bdi dir="ltr">git rebase</bdi> -<bdi dir="ltr">i</bdi></code> أو <code dir="ltr"><bdi dir="ltr">git merge</bdi> --<bdi dir="ltr">squash</bdi></code>.</p>

### When and Why

<p dir="rtl" align="right">قبل دمج <bdi dir="ltr">Feature Branch</bdi> لتحسين وضوح الـ <bdi dir="ltr">History.</bdi> <strong>تنبيه:</strong> العملية تعيد كتابة الـ <bdi dir="ltr">Commit Hashes</bdi>؛ تجنّبها على تاريخ مشترك دون تنسيق. وإذا كانت الـ <bdi dir="ltr">Commits</bdi> منشورة، فقد يلزم <bdi dir="ltr">Push</bdi> من نوع <code dir="ltr">--<bdi dir="ltr">force-with-lease</bdi></code> بعد الاتفاق مع الفريق.</p>

### Example

<p dir="rtl" align="right">أثناء تنفيذ <bdi dir="ltr">Login API</bdi> عملت أربع <bdi dir="ltr">Commits</bdi> لإضافة الـ <bdi dir="ltr">Endpoint</bdi> وتصحيح <bdi dir="ltr">Validation</bdi> وتعديل <bdi dir="ltr">JWT</bdi> وتحسين الكود. قبل فتح <bdi dir="ltr">Pull Request</bdi> تريد أن تظهر كميزة واحدة واضحة للمراجع.</p>
```bash
git rebase -i HEAD~4
```
<p dir="rtl" align="right">داخل المحرر اترك أول <bdi dir="ltr">Commit</bdi> بـ <code dir="ltr"><bdi dir="ltr">pick</bdi></code> وغيّر الباقي إلى <code dir="ltr"><bdi dir="ltr">squash</bdi></code> (أو <code dir="ltr"><bdi dir="ltr">s</bdi></code>) ثم احفظ وعدّل رسالة الـ <bdi dir="ltr">Commit</bdi> النهائية.</p>
```text
pick   a1b2c3d Add login endpoint
squash b2c3d4e Fix validation
squash c3d4e5f Fix JWT handling
squash d4e5f6a Refactor login service
```

---

## 2. Git Merge vs Git Rebase

### Definition

<p dir="rtl" align="right">كلاهما يدمج تغييرات من <bdi dir="ltr">Branch</bdi> أخرى، لكن <code dir="ltr"><bdi dir="ltr">merge</bdi></code> يحافظ على شكل التفرعات في التاريخ، بينما <code dir="ltr"><bdi dir="ltr">rebase</bdi></code> يعيد تطبيق <bdi dir="ltr">Commits</bdi> الـ <bdi dir="ltr">Feature</bdi> فوق <bdi dir="ltr">Base</bdi> جديدة، وينتج عادةً تاريخًا خطيًا.</p>

### When and Why

<p dir="rtl" align="right">استخدم <code dir="ltr"><bdi dir="ltr">merge</bdi></code> حين يكون الحفاظ على التاريخ المشترك مهمًا، و<code dir="ltr"><bdi dir="ltr">rebase</bdi></code> عندما تريد تاريخًا مرتبًا قبل الدمج. <strong>تنبيه:</strong> قد تحدث <bdi dir="ltr">Conflicts</bdi> في الحالتين، ولا تعمل <bdi dir="ltr">Rebase</bdi> لتاريخ يتعاون عليه الآخرون دون اتفاق.</p>

### Example

<p dir="rtl" align="right">تعمل على <code dir="ltr"><bdi dir="ltr">feature/booking-api</bdi></code> بينما تم تحديث <code dir="ltr"><bdi dir="ltr">main</bdi></code>. تريد جلب آخر التغييرات قبل فتح <bdi dir="ltr">Pull Request.</bdi></p>
**خيار Merge:**
```bash
git switch feature/booking-api
git fetch origin
git merge origin/main
```
**خيار Rebase:**
```bash
git switch feature/booking-api
git fetch origin
git rebase origin/main
```
| المقارنة | Merge | Rebase |
|---|---|---|
| التاريخ | يحتفظ بتاريخ التفرعات | يعيد تطبيق الـ Commits بتسلسل جديد |
| الـ Commit Hashes القديمة | لا يعيد كتابتها | تتغير للـ Commits المعاد تطبيقها |
| الاستخدام الشائع | دمج شغل مشترك بأمان نسبيًا | ترتيب تاريخ Feature Branch شخصية |

---

## 3. Git Help

### Definition

<p dir="rtl" align="right">عرض توثيق <bdi dir="ltr">Git</bdi> الرسمي للأوامر والخيارات المتاحة.</p>

### When and Why

<p dir="rtl" align="right">عند تعلّم أمر جديد أو التحقق من خيارات أمر حساس قبل تشغيله.</p>

### Example

<p dir="rtl" align="right">تحتاج إلى اختيار <bdi dir="ltr">Option</bdi> مناسبة في <code dir="ltr"><bdi dir="ltr">git log</bdi></code> ولا تتذكر الصيغة الصحيحة.</p>
```bash
git help log
git log -h
git help -a
```

---

## 4. Git Cherry-Pick

### Definition

<p dir="rtl" align="right">تطبيق التغيير الذي أدخلته <bdi dir="ltr">Commit</bdi> محددة على الـ <bdi dir="ltr">Branch</bdi> الحالية، دون دمج جميع <bdi dir="ltr">Commits</bdi> الـ <bdi dir="ltr">Branch</bdi> المصدر.</p>

### When and Why

<p dir="rtl" align="right">لنقل <bdi dir="ltr">Bug Fix</bdi> محدد أو تغيير صغير بين <bdi dir="ltr">Branches.</bdi> <strong>تنبيه:</strong> غالبًا ينشئ <bdi dir="ltr">Commit</bdi> جديدة بـ <bdi dir="ltr">Hash</bdi> مختلف، وقد تحدث <bdi dir="ltr">Conflicts</bdi> أو تنقصه تغييرات يعتمد عليها.</p>

### Example

<p dir="rtl" align="right">يوجد إصلاح عاجل لخطأ في <bdi dir="ltr">Authentication</bdi> داخل <bdi dir="ltr">Feature Branch</bdi>، لكن باقي الـ <bdi dir="ltr">Feature</bdi> لم يكتمل. تريد نقل إصلاح الخطأ وحده إلى <bdi dir="ltr">Branch</bdi> مخصصة للإصلاح.</p>
```bash
git switch -c hotfix/auth main
git cherry-pick a1b2c3d
```

---

## 5. Git Clean

### Definition

<p dir="rtl" align="right">حذف ملفات غير متتبعة (<bdi dir="ltr">Untracked</bdi>) من <bdi dir="ltr">Working Directory.</bdi> لا يحذف الملفات المتتبعة، ولا يحذف الملفات المتجاهلة افتراضيًا.</p>

### When and Why

<p dir="rtl" align="right">قبل إعادة تشغيل <bdi dir="ltr">Tests</bdi> في بيئة نظيفة. <strong>تنبيه:</strong> قد يحذف ملفات محلية لا يمكن استرجاعها من <bdi dir="ltr">Git</bdi>، خصوصًا مع <code dir="ltr">-<bdi dir="ltr">x</bdi></code>؛ راجع المعاينة أولًا.</p>

### Example

<p dir="rtl" align="right">أثناء تجربة <bdi dir="ltr">.NET API</bdi> أنشأت ملفات <bdi dir="ltr">Logs</bdi> وتجارب مؤقتة غير متتبعة، وتريد تنظيف مجلد العمل.</p>
```bash
git clean -n       # معاينة الملفات قبل الحذف
git clean -f       # حذف الملفات غير المتتبعة
git clean -fd      # حذف الملفات والمجلدات غير المتتبعة
git clean -ndx     # معاينة تشمل الملفات المتجاهلة
git clean -fdx     # حذف يشمل الملفات المتجاهلة أيضًا
```

---

## 6. Git Grep

### Definition

<p dir="rtl" align="right">البحث السريع عن نص أو <bdi dir="ltr">Pattern</bdi> داخل الملفات التي يتتبعها <bdi dir="ltr">Git.</bdi></p>

### When and Why

<p dir="rtl" align="right">أثناء <bdi dir="ltr">Refactoring</bdi> أو <bdi dir="ltr">Debugging</bdi> أو تتبع استخدام <bdi dir="ltr">Class</bdi> أو <bdi dir="ltr">Method.</bdi></p>

### Example

<p dir="rtl" align="right">تريد تغيير اسم <code dir="ltr"><bdi dir="ltr">PaymentService</bdi></code> في مشروع <bdi dir="ltr">.NET</bdi> وتحتاج معرفة كل المواضع التي تشير إليه.</p>
```bash
git grep "PaymentService"
git grep -n "PaymentService"  # مع أرقام الأسطر
git grep -i "paymentservice"  # دون حساسية لحالة الحروف
git grep "PaymentService" HEAD  # البحث في نسخة آخر Commit
```

---

## 7. Git Blame

### Definition

<p dir="rtl" align="right">معرفة آخر <bdi dir="ltr">Commit</bdi> مسؤولة عن كل سطر في ملف، مع معلومات الكاتب والتاريخ.</p>

### When and Why

<p dir="rtl" align="right">لفهم تاريخ سطر معين وربطه بالـ <bdi dir="ltr">Requirement</bdi> أو الـ <bdi dir="ltr">Bug Fix</bdi> السابق. <strong>تنبيه:</strong> يعرض آخر تعديل منسوب للسطر، وليس بالضرورة أول شخص كتبه، وليس أداة لإلقاء اللوم.</p>

### Example

<p dir="rtl" align="right">وجدت شرطًا غير متوقع في <code dir="ltr"><bdi dir="ltr">BookingService.cs</bdi></code> وتريد معرفة متى أُدخل وما السياق وراءه قبل تعديله.</p>
```bash
git blame BookingService.cs
git blame -L 10,25 BookingService.cs
git show <commit-hash>
```

---

## 8. Git Bisect

### Definition

<p dir="rtl" align="right">استخدام <bdi dir="ltr">Binary Search</bdi> في تاريخ الـ <bdi dir="ltr">Commits</bdi> لتحديد التغيير الذي تسبب في <bdi dir="ltr">Bug</bdi>، بشرط معرفة نسخة سليمة وأخرى بها المشكلة.</p>

### When and Why

<p dir="rtl" align="right">لتحديد <bdi dir="ltr">Commit</bdi> مسببة لمشكلة قابلة لإعادة الإنتاج دون فحص كل <bdi dir="ltr">Commit</bdi> يدويًا. <strong>تنبيه:</strong> احفظ تغييراتك المحلية قبل البدء، وقد تحتاج إلى <code dir="ltr"><bdi dir="ltr">git bisect skip</bdi></code> لو تعذر اختبار نسخة معينة.</p>

### Example

<p dir="rtl" align="right"><bdi dir="ltr">Login API</bdi> أصبحت ترجع <bdi dir="ltr">HTTP</bdi> 500 بعد عشرات الـ <bdi dir="ltr">Commits.</bdi> لا تعرف أي تعديل أدخل المشكلة.</p>
```bash
git bisect start
git bisect bad                 # النسخة الحالية بها المشكلة
git bisect good a1b2c3d      # نسخة قديمة مؤكدة السلامة
# اختبر النسخة التي انتقل إليها Git، ثم:
git bisect good                # أو git bisect bad
# كرر الاختبار حتى تحديد الـ Commit
git bisect reset
```

---

## 9. Git Shortlog

### Definition

<p dir="rtl" align="right">تلخيص الـ <bdi dir="ltr">Commit History</bdi> وتجميع الـ <bdi dir="ltr">Commits</bdi> حسب الكاتب.</p>

### When and Why

<p dir="rtl" align="right">لإعداد <bdi dir="ltr">Release Notes</bdi> أو مراجعة المساهمات. <strong>تنبيه:</strong> عدد الـ <bdi dir="ltr">Commits</bdi> ليس مقياسًا كافيًا لجودة أو إنتاجية المطور.</p>

### Example

<p dir="rtl" align="right">تريد مراجعة قائمة المساهمين في إصدار جديد من المشروع وعدد <bdi dir="ltr">Commits</bdi> كل مساهم.</p>
```bash
git shortlog -sn HEAD
git shortlog -sne HEAD
git shortlog --since="2026-09-01" HEAD
```

---

## 10. Git Prune

### Definition

<p dir="rtl" align="right">حذف <bdi dir="ltr">Git Objects</bdi> غير القابلة للوصول (<bdi dir="ltr">Unreachable Objects</bdi>) والتي انتهت مدة الاحتفاظ بها وفق إعدادات <bdi dir="ltr">Git.</bdi></p>

### When and Why

<p dir="rtl" align="right">لصيانة مستودع محلي عند الحاجة. <strong>تنبيه:</strong> لا تخلط بين <code dir="ltr"><bdi dir="ltr">git prune</bdi></code> و<code dir="ltr"><bdi dir="ltr">git fetch</bdi> --<bdi dir="ltr">prune</bdi></code>؛ الأخير يزيل مراجع <bdi dir="ltr">Remote-Tracking</bdi> القديمة. الحذف قد يصعّب استرجاع <bdi dir="ltr">Commits</bdi> مفقودة.</p>

### Example

<p dir="rtl" align="right">بعد عمليات <bdi dir="ltr">Rebase</bdi> وReset متكررة، تريد إجراء صيانة للمستودع وتقليل البيانات القديمة غير المطلوبة.</p>
```bash
git prune --dry-run   # معاينة فقط
git prune             # تنظيف وفق مدة الاحتفاظ الافتراضية
git gc                # الصيانة المعتادة الموصى بها غالبًا
```

---

## 11. Git Worktree

### Definition

<p dir="rtl" align="right">فتح أكثر من <bdi dir="ltr">Working Directory</bdi> مرتبطين بنفس <bdi dir="ltr">Repository</bdi>، بحيث تستطيع العمل على <bdi dir="ltr">Branches</bdi> مختلفة في وقت واحد.</p>

### When and Why

<p dir="rtl" align="right">للعمل المتوازي على <bdi dir="ltr">Feature</bdi> وHotfix أو مقارنة إصدارات دون <bdi dir="ltr">Clone</bdi> إضافي. <strong>تنبيه:</strong> لا تزل <bdi dir="ltr">Worktree</bdi> فيها تغييرات غير محفوظة.</p>

### Example

<p dir="rtl" align="right">تعمل على <bdi dir="ltr">Feature</bdi> الدفع ولديك تعديلات غير مكتملة، ثم يأتي <bdi dir="ltr">Hotfix</bdi> عاجل للـ <bdi dir="ltr">Login.</bdi> تريد العمل عليه دون تغيير الـ <bdi dir="ltr">Working Directory</bdi> الحالية.</p>
```bash
git worktree add -b hotfix/login ../login-hotfix main
git worktree list
# بعد الانتهاء، ومن خارج مجلد الـ Worktree المراد حذفه:
git worktree remove ../login-hotfix
```

---

## 12. Git Verify-Commit

### Definition

<p dir="rtl" align="right">التحقق من التوقيع الرقمي الموجود على <bdi dir="ltr">Commit</bdi>، باستخدام إعدادات الثقة والمفاتيح المتاحة محليًا.</p>

### When and Why

<p dir="rtl" align="right">للتحقق من سلامة التوقيع وهوية المفتاح وفق سياسة الفريق. <strong>تنبيه:</strong> نجاح التحقق لا يعني أن الكود آمن أو خالٍ من الأخطاء، ولا يمكن التحقق من <bdi dir="ltr">Commit</bdi> غير موقّعة بالطريقة نفسها.</p>

### Example

<p dir="rtl" align="right">فريق العمل يشترط <bdi dir="ltr">Signed Commits</bdi> لتغييرات <bdi dir="ltr">Authentication.</bdi> تريد التحقق من توقيع <bdi dir="ltr">Commit</bdi> قبل مراجعتها.</p>
```bash
git verify-commit HEAD
git verify-commit -v HEAD
git verify-commit a1b2c3d
```
<p dir="rtl" align="right">بعد إعداد توقيع <bdi dir="ltr">GPG</bdi> أو <bdi dir="ltr">SSH</bdi> يمكن إنشاء <bdi dir="ltr">Commit</bdi> موقّعة:</p>
```bash
git commit -S -m "Update authentication"
```

---

## 13. Git Filter-Repo

### Definition

<p dir="rtl" align="right">أداة خارجية (تُثبَّت بشكل مستقل) لإعادة كتابة <bdi dir="ltr">Git History</bdi>، مثل إزالة ملفات من جميع الـ <bdi dir="ltr">Commits</bdi> أو إعادة تنظيم مسارات قديمة.</p>

### When and Why

<p dir="rtl" align="right">لإزالة ملفات حساسة أو كبيرة من تاريخ المستودع. <strong>تنبيه:</strong> العملية تغيّر <bdi dir="ltr">Commit Hashes</bdi> وقد تتطلب إعادة نشر التاريخ على الـ <bdi dir="ltr">Remote</bdi> ومعالجة الـ <bdi dir="ltr">Forks</bdi> والنسخ القديمة؛ إزالة الملف لا تضمن اختفاء الأسرار من كل النسخ.</p>

### Example

<p dir="rtl" align="right">تم رفع ملف <code dir="ltr"><bdi dir="ltr">.env</bdi></code> يحتوي على <bdi dir="ltr">API Key</bdi> بالخطأ إلى <bdi dir="ltr">GitHub.</bdi> حذفه في <bdi dir="ltr">Commit</bdi> جديدة لا يمحوه من التاريخ السابق.</p>
<p dir="rtl" align="right">أولًا: ألغِ المفتاح المكشوف أو غيّره (<bdi dir="ltr">Rotate/Revoke</bdi>)، ثم نسّق مع الفريق بشأن تنظيف التاريخ، ونفّذ العملية على <bdi dir="ltr">Clone</bdi> جديدة مخصصة لذلك:</p>
```bash
git filter-repo --path .env --invert-paths
```
<p dir="rtl" align="right">---</p>
<p dir="rtl" align="right">## <bdi dir="ltr">Quick Revision</bdi></p>
| الأمر | الهدف |
|---|---|
| Squash | دمج عدة Commits في واحدة |
| Merge | دمج التفرعات مع الحفاظ على تاريخها |
| Rebase | إعادة تطبيق Commits فوق Base جديدة |
| Help | قراءة توثيق الأوامر |
| Cherry-pick | نقل تغيير Commit معينة |
| Clean | حذف ملفات غير متتبعة |
| Grep | البحث في ملفات Git |
| Blame | تتبع آخر تعديل على سطر |
| Bisect | إيجاد Commit سببت Bug |
| Shortlog | تلخيص Commits حسب الكاتب |
| Prune | تنظيف Objects غير قابلة للوصول |
| Worktree | العمل على أكثر من Branch في مجلدات منفصلة |
| Verify-commit | التحقق من توقيع Commit |
| Filter-repo | إعادة كتابة التاريخ |
<p dir="rtl" align="right">## مراجع رسمية للتوسع</p>
<p dir="rtl" align="right">- [Git Documentation](https://git-scm.com/docs)
- [Interactive Rebase](https://git-scm.com/docs/git-rebase)
- [git merge](https://git-scm.com/docs/git-merge)
- [git cherry-pick](https://git-scm.com/docs/git-cherry-pick)
- [git filter-repo project](https://github.com/newren/git-filter-repo)</p>

---

## Quick Revision

| Command       | Purpose                                           |
| ------------- | ------------------------------------------------- |
| Squash        | Combine several commits into one                  |
| Merge         | Integrate branches while preserving their history |
| Rebase        | Replay commits on a new base                      |
| Help          | Read Git documentation                            |
| Cherry-pick   | Apply one selected commit                         |
| Clean         | Remove untracked files                            |
| Grep          | Search tracked content                            |
| Blame         | Trace the latest change to each line              |
| Bisect        | Find the commit that introduced a bug             |
| Shortlog      | Summarize commits by author                       |
| Prune         | Remove eligible unreachable objects               |
| Worktree      | Use separate working directories                  |
| Verify-commit | Verify a commit signature                         |
| Filter-repo   | Rewrite repository history                        |

## References

- [Git Documentation](https://git-scm.com/docs)
- [Interactive Rebase](https://git-scm.com/docs/git-rebase)
- [Git Merge](https://git-scm.com/docs/git-merge)
- [Git Cherry-Pick](https://git-scm.com/docs/git-cherry-pick)
- [Git Filter-Repo](https://github.com/newren/git-filter-repo)
