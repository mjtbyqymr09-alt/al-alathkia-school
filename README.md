<!DOCTYPE html>
<html lang="ar" dir="rtl">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>ثانوية الأذكياء للمتفوقين</title>
    <!-- FontAwesome & TailwindCSS -->
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <script src="https://cdn.tailwindcss.com"></script>
    <script>
        tailwind.config = {
            theme: {
                extend: {
                    colors: {
                        brand: {
                            navy: '#0f172a',
                            royal: '#1e3a8a',
                            gold: '#d97706',
                            goldLight: '#fef3c7',
                            accent: '#2563eb'
                        }
                    }
                }
            }
        }
    </script>
    <style>
        @import url('https://fonts.googleapis.com/css2?family=Cairo:wght@400;600;700;800;900&display=swap');
        body { font-family: 'Cairo', sans-serif; background-color: #f8fafc; color: #1e293b; }
        .glass-panel { background: rgba(255, 255, 255, 0.95); backdrop-filter: blur(10px); border: 1px solid rgba(226, 232, 240, 0.8); }
        .print-only { display: none; }
        @media print {
            body * { visibility: hidden; }
            #print-section, #print-section * { visibility: visible; }
            #print-section { position: absolute; left: 0; top: 0; width: 100%; }
            .no-print { display: none !important; }
            .print-only { display: block; }
        }
    </style>
</head>
<body class="min-h-screen flex flex-col justify-between antialiased selection:bg-brand-gold selection:text-white">

    <!-- Header -->
    <header class="bg-brand-navy text-white shadow-xl border-b-4 border-brand-gold sticky top-0 z-50">
        <div class="max-w-7xl mx-auto px-4 py-3 flex flex-wrap justify-between items-center gap-4">
            <div class="flex items-center gap-3 cursor-pointer" onclick="goHome()">
                <div class="w-12 h-12 rounded-full bg-brand-gold flex items-center justify-center text-brand-navy font-black text-2xl shadow-lg border-2 border-white">
                    <i class="fa-solid fa-graduation-cap"></i>
                </div>
                <div>
                    <h1 class="font-extrabold text-xl md:text-2xl text-amber-400 tracking-wide">ثانوية الأذكياء للمتفوقين</h1>
                    <p class="text-xs text-slate-300">إدارة الأستاذ المدير: <span class="text-white font-bold">أركان حرجان</span></p>
                </div>
            </div>

            <!-- User Auth Bar -->
            <div id="user-bar" class="flex items-center gap-3">
                <!-- Dynamic Content via JS -->
            </div>
        </div>
    </header>

    <!-- Main Content Container -->
    <main class="max-w-7xl mx-auto px-4 py-8 flex-1 w-full">

        <!-- Login / Register View -->
        <section id="auth-section" class="max-w-md mx-auto my-6 glass-panel p-8 rounded-2xl shadow-2xl border-t-8 border-brand-royal">
            <div class="text-center mb-6">
                <div class="inline-block p-4 rounded-full bg-blue-50 text-brand-royal mb-3">
                    <i class="fa-solid fa-user-shield text-3xl"></i>
                </div>
                <h2 id="auth-title" class="text-2xl font-bold text-slate-800">تسجيل الدخول للنظام</h2>
                <p class="text-xs text-slate-500 mt-1">يرجى تأكيد الهوية للوصول للخدمات الإلكترونية</p>
            </div>

            <!-- Auth Tabs -->
            <div class="flex rounded-lg bg-slate-100 p-1 mb-6 text-sm font-bold">
                <button id="tab-login" onclick="switchAuthTab('login')" class="flex-1 py-2 rounded-md bg-white text-brand-royal shadow text-center">تسجيل دخول</button>
                <button id="tab-register" onclick="switchAuthTab('register')" class="flex-1 py-2 rounded-md text-slate-600 hover:text-brand-royal text-center">حساب جديد</button>
            </div>

            <form id="auth-form" onsubmit="handleAuth(event)" class="space-y-4">
                <!-- Role Selector -->
                <div>
                    <label class="block text-xs font-bold text-slate-700 mb-1">الصفة الدراسية / الوظيفية</label>
                    <div class="grid grid-cols-2 gap-2">
                        <label class="flex items-center justify-center p-2.5 border rounded-lg cursor-pointer hover:bg-slate-50 border-slate-300 font-bold text-sm">
                            <input type="radio" name="role" value="student" checked onchange="toggleTeacherFields()" class="ml-2 accent-brand-royal">
                            <i class="fa-solid fa-user-graduate ml-2 text-slate-500"></i> طالب
                        </label>
                        <label class="flex items-center justify-center p-2.5 border rounded-lg cursor-pointer hover:bg-slate-50 border-slate-300 font-bold text-sm">
                            <input type="radio" name="role" value="teacher" onchange="toggleTeacherFields()" class="ml-2 accent-brand-royal">
                            <i class="fa-solid fa-chalkboard-user ml-2 text-slate-500"></i> مدرس
                        </label>
                    </div>
                </div>

                <!-- Phone Number -->
                <div>
                    <label class="block text-xs font-bold text-slate-700 mb-1">رقم الهاتف (حصراً)</label>
                    <div class="relative">
                        <i class="fa-solid fa-phone absolute right-3 top-3 text-slate-400"></i>
                        <input type="tel" id="auth-phone" required placeholder="07700000000" dir="ltr" class="w-full pr-10 pl-3 py-2 border border-slate-300 rounded-lg text-left focus:ring-2 focus:ring-brand-royal focus:outline-none">
                    </div>
                </div>

                <!-- Password -->
                <div>
                    <label class="block text-xs font-bold text-slate-700 mb-1">كلمة المرور</label>
                    <div class="relative">
                        <i class="fa-solid fa-lock absolute right-3 top-3 text-slate-400"></i>
                        <input type="password" id="auth-password" required placeholder="••••••••" class="w-full pr-10 pl-3 py-2 border border-slate-300 rounded-lg focus:ring-2 focus:ring-brand-royal focus:outline-none">
                    </div>
                </div>

                <!-- Teacher Passcode (Hidden for Students) -->
                <div id="teacher-code-field" class="hidden bg-amber-50 p-3 rounded-lg border border-amber-200">
                    <label class="block text-xs font-bold text-amber-800 mb-1"><i class="fa-solid fa-key ml-1"></i> كود المعلمين الخاص</label>
                    <input type="password" id="auth-teachercode" placeholder="ادخل الكود الخاص بالمدرسين" class="w-full px-3 py-2 border border-amber-300 rounded-lg text-sm focus:ring-2 focus:ring-brand-gold focus:outline-none">
                </div>

                <!-- Captcha (Anti-Robot) -->
                <div class="bg-slate-50 p-3 rounded-lg border border-slate-200 space-y-2">
                    <label class="block text-xs font-bold text-slate-700">رمز التحقق الأمني (تأكيد الحماية)</label>
                    <div class="flex items-center gap-3">
                        <div id="captcha-code" class="bg-brand-navy text-brand-gold font-mono font-black text-xl px-4 py-1.5 rounded tracking-widest select-none"></div>
                        <button type="button" onclick="generateCaptcha()" class="text-xs text-brand-royal hover:underline"><i class="fa-solid fa-rotate ml-1"></i>تغيير</button>
                    </div>
                    <input type="text" id="auth-captcha" required placeholder="انقل الأرقام الظاهرة أعلاه" dir="ltr" class="w-full px-3 py-1.5 border border-slate-300 rounded text-center font-bold text-lg tracking-widest focus:ring-2 focus:ring-brand-royal focus:outline-none">
                </div>

                <button type="submit" id="auth-submit-btn" class="w-full py-3 bg-brand-royal hover:bg-blue-800 text-white font-bold rounded-lg shadow-lg transition duration-200">
                    تسجيل الدخول
                </button>
            </form>
        </section>

        <!-- School Dashboard (Restricted until login) -->
        <section id="dashboard-section" class="hidden space-y-6">

            <!-- Breadcrumb Navigation -->
            <div class="flex items-center justify-between bg-white p-4 rounded-xl shadow-sm border border-slate-200 no-print">
                <div id="breadcrumb" class="flex items-center gap-2 text-sm font-bold text-slate-700">
                    <span class="text-brand-royal cursor-pointer hover:underline" onclick="showClassesView()"><i class="fa-solid fa-school ml-1"></i> الصفوف الدراسية</span>
                </div>
                <div id="teacher-actions" class="hidden flex gap-2">
                    <button onclick="openAddStudentModal()" class="bg-emerald-600 hover:bg-emerald-700 text-white text-xs font-bold px-3 py-2 rounded-lg shadow flex items-center gap-1">
                        <i class="fa-solid fa-user-plus"></i> إضافة طالب
                    </button>
                </div>
            </div>

            <!-- VIEW 1: Classes Grid -->
            <div id="view-classes" class="space-y-6">
                <div class="flex justify-between items-center">
                    <h2 class="text-xl font-bold text-slate-800 flex items-center gap-2">
                        <i class="fa-solid fa-layer-group text-brand-gold"></i> المناهج والصفوف الدراسية
                    </h2>
                </div>

                <div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-6" id="classes-grid">
                    <!-- Dynamic rendering -->
                </div>
            </div>

            <!-- VIEW 2: Sections (الشعب) -->
            <div id="view-sections" class="hidden space-y-6">
                <h2 id="current-class-title" class="text-2xl font-bold text-brand-navy border-b pb-2"></h2>
                <div class="grid grid-cols-1 sm:grid-cols-2 md:grid-cols-4 gap-4" id="sections-grid">
                    <!-- Dynamic rendering -->
                </div>
            </div>

            <!-- VIEW 3: Students List in Section -->
            <div id="view-students" class="hidden space-y-6">
                <div class="flex justify-between items-center border-b pb-3">
                    <div>
                        <h2 id="current-section-title" class="text-2xl font-bold text-brand-navy"></h2>
                        <p class="text-xs text-slate-500">اختر اسم الطالب لمشاهدة الملف الشخصي والدرجات والغيابات</p>
                    </div>
                </div>

                <div class="grid grid-cols-1 sm:grid-cols-2 md:grid-cols-3 lg:grid-cols-4 gap-4" id="students-grid">
                    <!-- Dynamic rendering -->
                </div>
            </div>

            <!-- VIEW 4: Student Details & Marks Profile -->
            <div id="view-student-profile" class="hidden space-y-6">
                
                <!-- Student Header Card -->
                <div id="print-section" class="bg-white p-6 rounded-2xl shadow-md border border-slate-200 space-y-6">
                    <div class="flex flex-col md:flex-row justify-between items-start md:items-center border-b pb-4 gap-4">
                        <div class="flex items-center gap-4">
                            <div class="w-16 h-16 rounded-full bg-blue-100 text-brand-royal flex items-center justify-center font-black text-2xl border-2 border-brand-gold">
                                <i class="fa-solid fa-user-graduate"></i>
                            </div>
                            <div>
                                <h2 id="profile-student-name" class="text-2xl font-black text-slate-800"></h2>
                                <p id="profile-student-meta" class="text-sm font-bold text-slate-500"></p>
                            </div>
                        </div>
                        <div class="flex items-center gap-3 no-print">
                            <button onclick="window.print()" class="bg-slate-800 hover:bg-black text-white text-xs font-bold px-4 py-2.5 rounded-lg shadow flex items-center gap-2">
                                <i class="fa-solid fa-print"></i> طباعة الشهادة / الكشف
                            </button>
                        </div>
                    </div>

                    <!-- Attendance & Warnings Row -->
                    <div class="grid grid-cols-1 md:grid-cols-2 gap-6">
                        <!-- Absences Card -->
                        <div class="bg-slate-50 p-4 rounded-xl border border-slate-200">
                            <div class="flex justify-between items-center mb-2">
                                <h3 class="font-bold text-slate-800 text-sm flex items-center gap-2">
                                    <i class="fa-solid fa-calendar-xmark text-red-500"></i> عدد أيام الغياب المسجلة
                                </h3>
                                <div id="absence-controls" class="hidden flex items-center gap-2 no-print">
                                    <button onclick="changeAbsence(-1)" class="w-7 h-7 rounded bg-red-100 text-red-700 font-bold hover:bg-red-200">-</button>
                                    <button onclick="changeAbsence(1)" class="w-7 h-7 rounded bg-emerald-100 text-emerald-700 font-bold hover:bg-emerald-200">+</button>
                                </div>
                            </div>
                            <div class="flex items-baseline gap-2">
                                <span id="profile-student-absences" class="text-3xl font-black text-red-600">0</span>
                                <span class="text-xs font-bold text-slate-500">أيام</span>
                            </div>
                        </div>

                        <!-- Warnings / Commitments Card -->
                        <div class="bg-slate-50 p-4 rounded-xl border border-slate-200 space-y-2">
                            <div class="flex justify-between items-center">
                                <h3 class="font-bold text-slate-800 text-sm flex items-center gap-2">
                                    <i class="fa-solid fa-file-contract text-amber-600"></i> التعهدات والإنذارات السلوكية
                                </h3>
                                <button id="add-warning-btn" onclick="addWarning()" class="hidden text-xs bg-amber-500 hover:bg-amber-600 text-white font-bold px-2 py-1 rounded no-print">+ تعهد جديد</button>
                            </div>
                            <ul id="profile-student-warnings" class="text-xs space-y-1 text-slate-700 list-disc list-inside">
                                <!-- Dynamic -->
                            </ul>
                        </div>
                    </div>

                    <!-- Marks Table -->
                    <div class="space-y-3">
                        <h3 class="font-bold text-slate-800 text-lg flex items-center gap-2">
                            <i class="fa-solid fa-table-list text-brand-royal"></i> السجل الدراسي والدرجات الامتحانية
                        </h3>
                        <div class="overflow-x-auto rounded-xl border border-slate-300 shadow-sm">
                            <table class="w-full text-xs text-center border-collapse">
                                <thead>
                                    <tr class="bg-brand-navy text-white font-bold">
                                        <th class="p-2 border">المادة الدراسية</th>
                                        <th class="p-2 border bg-blue-950">شهر 1 (كورس 1)</th>
                                        <th class="p-2 border bg-blue-950">شهر 2 (كورس 1)</th>
                                        <th class="p-2 border bg-amber-600 text-white">درجة النصف الأول</th>
                                        <th class="p-2 border bg-indigo-900">نصف السنة</th>
                                        <th class="p-2 border bg-blue-950">شهر 1 (كورس 2)</th>
                                        <th class="p-2 border bg-blue-950">شهر 2 (كورس 2)</th>
                                        <th class="p-2 border bg-amber-600 text-white">درجة النصف الثاني</th>
                                        <th class="p-2 border bg-emerald-700">السعي السنوي</th>
                                        <th class="p-2 border bg-slate-800">النهائي</th>
                                        <th class="p-2 border bg-brand-gold text-brand-navy font-black">الدرجة النهائية</th>
                                        <th class="p-2 border bg-red-800">الدور الثاني</th>
                                    </tr>
                                </thead>
                                <tbody id="marks-table-body" class="bg-white divide-y">
                                    <!-- Dynamic Rows -->
                                </tbody>
                            </table>
                        </div>
                    </div>

                </div>
            </div>

        </section>

    </main>

    <!-- Footer -->
    <footer class="bg-brand-navy text-slate-400 text-xs py-4 border-t border-slate-800 no-print">
        <div class="max-w-7xl mx-auto px-4 text-center space-y-1">
            <p>© جميع الحقوق محفوظة لـ <span class="text-amber-400 font-bold">ثانوية الأذكياء للمتفوقين</span></p>
            <p>إدارة المدرسة: الأستاذ أركان حرجان</p>
        </div>
    </footer>

    <!-- Modal: Add Student -->
    <div id="modal-add-student" class="fixed inset-0 bg-black/60 backdrop-blur-sm hidden items-center justify-center z-50 p-4">
        <div class="bg-white rounded-2xl max-w-md w-full p-6 space-y-4 shadow-2xl">
            <div class="flex justify-between items-center border-b pb-2">
                <h3 class="font-bold text-lg text-slate-800">إضافة طالب جديد للشعبة</h3>
                <button onclick="closeAddStudentModal()" class="text-slate-400 hover:text-slate-600 font-bold text-xl">&times;</button>
            </div>
            <div class="space-y-3">
                <div>
                    <label class="block text-xs font-bold text-slate-700 mb-1">الاسم الثلاثي للطالب</label>
                    <input type="text" id="new-student-name" placeholder="مثال: علي حسين محمود" class="w-full px-3 py-2 border rounded-lg text-sm focus:ring-2 focus:ring-brand-royal focus:outline-none">
                </div>
                <div>
                    <label class="block text-xs font-bold text-slate-700 mb-1">رقم هاتف الطالب / ولي الأمر</label>
                    <input type="tel" id="new-student-phone" placeholder="07700000000" dir="ltr" class="w-full px-3 py-2 border rounded-lg text-sm focus:ring-2 focus:ring-brand-royal focus:outline-none">
                </div>
            </div>
            <div class="flex gap-2 justify-end pt-2 border-t">
                <button onclick="closeAddStudentModal()" class="px-4 py-2 bg-slate-200 text-slate-700 font-bold rounded-lg text-xs">إلغاء</button>
                <button onclick="saveNewStudent()" class="px-4 py-2 bg-emerald-600 text-white font-bold rounded-lg text-xs hover:bg-emerald-700">حفظ الطالب</button>
            </div>
        </div>
    </div>

    <!-- JS Logic -->
    <script>
        // System Curriculum Structure Definition
        const CURRICULUM = {
            "first_avg": {
                name: "الأول متوسط",
                subjects: ["رياضيات", "كيمياء", "فيزياء", "حاسوب", "انكليزي", "فنية", "رياضة", "الغة العربية", "اسلامية", "احياء", "الفرنسي", "اجتماعيات"]
            },
            "second_avg": {
                name: "الثاني متوسط",
                subjects: ["رياضيات", "كيمياء", "فيزياء", "حاسوب", "انكليزي", "فنية", "رياضة", "الغة العربية", "اسلامية", "احياء", "الفرنسي", "اجتماعيات"]
            },
            "third_avg": {
                name: "الثالث متوسط",
                subjects: ["رياضيات", "الغة العربية", "رياضة", "فنية", "كيمياء", "فيزياء", "احياء", "اجتماعيات", "اسلامية", "انكليزي"]
            },
            "fourth_sci": {
                name: "الرابع العلمي",
                subjects: ["رياضيات", "اسلامية", "عربي", "فيزياء", "كيمياء", "احياء", "كردي", "حاسوب", "انكليزي", "فنية", "رياضة", "جرائم حزب البعث"]
            },
            "fifth_sci": {
                name: "الخامس العلمي",
                subjects: ["رياضيات", "اسلامية", "عربي", "فيزياء", "كيمياء", "احياء", "كردي", "حاسوب", "انكليزي", "فنية", "رياضة"]
            },
            "sixth_sci": {
                name: "السادس العلمي",
                subjects: ["رياضيات", "فيزياء", "كيمياء", "احياء", "اسلامية", "الغة العربية", "انكليزي", "رياضة", "فنية"]
            }
        };

        // Local Data State
        let DB = {
            users: [
                { phone: '07700000000', password: '123', role: 'teacher', name: 'الأستاذ المدرس' },
                { phone: '07711111111', password: '123', role: 'student', name: 'أحمد جاسم محمد', classKey: 'first_avg', section: 'أ' }
            ],
            students: [
                {
                    id: 'std_1',
                    name: 'أحمد جاسم محمد',
                    phone: '07711111111',
                    classKey: 'first_avg',
                    section: 'أ',
                    absences: 2,
                    warnings: ['تعهد عدم التأخر عن الطابور الصباحي'],
                    marks: {
                        "رياضيات": { m1: 85, m2: 90, half: 88, mid: 85, m3: 92, m4: 88, half2: 90, finalExam: 89, secondRound: '' }
                    }
                }
            ]
        };

        let currentCaptcha = '';
        let currentUser = null;
        let selectedClassKey = null;
        let selectedSection = null;
        let selectedStudentId = null;

        // Initialize App
        window.addEventListener('DOMContentLoaded', () => {
            loadDB();
            generateCaptcha();
            checkSession();
        });

        function loadDB() {
            const saved = localStorage.getItem('al_athkia_db_v3');
            if (saved) {
                try { DB = JSON.parse(saved); } catch (e) {}
            } else {
                saveDB();
            }
        }

        function saveDB() {
            localStorage.setItem('al_athkia_db_v3', JSON.stringify(DB));
        }

        function generateCaptcha() {
            currentCaptcha = Math.floor(1000 + Math.random() * 9000).toString();
            document.getElementById('captcha-code').innerText = currentCaptcha;
        }

        function switchAuthTab(type) {
            const isLogin = type === 'login';
            document.getElementById('tab-login').className = isLogin ? "flex-1 py-2 rounded-md bg-white text-brand-royal shadow text-center font-bold" : "flex-1 py-2 rounded-md text-slate-600 hover:text-brand-royal text-center font-bold";
            document.getElementById('tab-register').className = !isLogin ? "flex-1 py-2 rounded-md bg-white text-brand-royal shadow text-center font-bold" : "flex-1 py-2 rounded-md text-slate-600 hover:text-brand-royal text-center font-bold";
            document.getElementById('auth-title').innerText = isLogin ? "تسجيل الدخول للنظام" : "إنشاء حساب جديد";
            document.getElementById('auth-submit-btn').innerText = isLogin ? "تسجيل الدخول" : "تأكيد وإنشاء الحساب";
            document.getElementById('auth-form').dataset.mode = type;
        }

        function toggleTeacherFields() {
            const role = document.querySelector('input[name="role"]:checked').value;
            const teacherField = document.getElementById('teacher-code-field');
            if (role === 'teacher') {
                teacherField.classList.remove('hidden');
            } else {
                teacherField.classList.add('hidden');
            }
        }

        function handleAuth(e) {
            e.preventDefault();
            const mode = document.getElementById('auth-form').dataset.mode || 'login';
            const role = document.querySelector('input[name="role"]:checked').value;
            const phone = document.getElementById('auth-phone').value.trim();
            const password = document.getElementById('auth-password').value.trim();
            const teacherCode = document.getElementById('auth-teachercode').value.trim();
            const captcha = document.getElementById('auth-captcha').value.trim();

            if (captcha !== currentCaptcha) {
                alert('رمز التحقق الأمني غير صحيح! حاول مجدداً.');
                generateCaptcha();
                return;
            }

            if (role === 'teacher' && teacherCode !== 'mogtba192') {
                alert('كود المعلمين غير صحيح! لا يمكن الدخول بحساب مدرس بدون الكود الخاص.');
                return;
            }

            if (mode === 'register') {
                const exist = DB.users.find(u => u.phone === phone);
                if (exist) {
                    alert('رقم الهاتف مسجل مسبقاً! قم بتسجيل الدخول.');
                    return;
                }
                const newUser = { phone, password, role, name: role === 'teacher' ? 'مدرس' : 'طالب' };
                DB.users.push(newUser);
                saveDB();
                currentUser = newUser;
            } else {
                const user = DB.users.find(u => u.phone === phone && u.password === password && u.role === role);
                if (!user) {
                    alert('بيانات الدخول غير صحيحة!');
                    return;
                }
                currentUser = user;
            }

            localStorage.setItem('al_athkia_session_v3', JSON.stringify(currentUser));
            setupLoggedInState();
        }

        function checkSession() {
            const savedSession = localStorage.getItem('al_athkia_session_v3');
            if (savedSession) {
                try {
                    currentUser = JSON.parse(savedSession);
                    setupLoggedInState();
                } catch(e) {}
            }
        }

        function setupLoggedInState() {
            document.getElementById('auth-section').classList.add('hidden');
            document.getElementById('dashboard-section').classList.remove('hidden');

            const userBar = document.getElementById('user-bar');
            const isTeacher = currentUser.role === 'teacher';
            userBar.innerHTML = `
                <div class="text-left text-xs text-slate-200">
                    <p class="font-bold text-white">${currentUser.name || (isTeacher ? 'كادر التدريس' : 'طالب')}</p>
                    <span class="bg-amber-400/20 text-amber-300 px-2 py-0.5 rounded text-[10px] font-bold">${isTeacher ? 'مدرس / صلاحيات كاملة' : 'طالب / عرض فقط'}</span>
                </div>
                <button onclick="logout()" class="bg-red-600/80 hover:bg-red-700 text-white text-xs px-3 py-1.5 rounded-lg font-bold">خروج</button>
            `;

            if (isTeacher) {
                document.getElementById('teacher-actions').classList.remove('hidden');
                document.getElementById('absence-controls').classList.remove('hidden');
                document.getElementById('add-warning-btn').classList.remove('hidden');
            } else {
                document.getElementById('teacher-actions').classList.add('hidden');
                document.getElementById('absence-controls').classList.add('hidden');
                document.getElementById('add-warning-btn').classList.add('hidden');
            }

            showClassesView();
        }

        function logout() {
            localStorage.removeItem('al_athkia_session_v3');
            currentUser = null;
            location.reload();
        }

        function goHome() {
            if (currentUser) showClassesView();
        }

        // View Navigations
        function showClassesView() {
            document.getElementById('view-classes').classList.remove('hidden');
            document.getElementById('view-sections').classList.add('hidden');
            document.getElementById('view-students').classList.add('hidden');
            document.getElementById('view-student-profile').classList.add('hidden');

            document.getElementById('breadcrumb').innerHTML = `
                <span class="text-brand-royal"><i class="fa-solid fa-school ml-1"></i> الصفحة الرئيسية / الصفوف</span>
            `;

            renderClasses();
        }

        function renderClasses() {
            const grid = document.getElementById('classes-grid');
            grid.innerHTML = '';

            Object.keys(CURRICULUM).forEach(key => {
                const c = CURRICULUM[key];
                const card = document.createElement('div');
                card.className = "glass-panel p-6 rounded-2xl shadow-md border hover:border-brand-gold cursor-pointer transition duration-200 hover:-translate-y-1 flex flex-col justify-between";
                card.onclick = () => showSectionsView(key);
                card.innerHTML = `
                    <div class="space-y-2">
                        <div class="w-12 h-12 rounded-xl bg-blue-50 text-brand-royal flex items-center justify-center text-xl font-bold mb-3">
                            <i class="fa-solid fa-book-open"></i>
                        </div>
                        <h3 class="text-xl font-extrabold text-slate-800">${c.name}</h3>
                        <p class="text-xs text-slate-500">عدد المواد: <span class="font-bold text-slate-700">${c.subjects.length} مادة</span></p>
                    </div>
                    <div class="mt-4 pt-3 border-t flex justify-between items-center text-xs font-bold text-brand-royal">
                        <span>عرض الشعب والطلاب</span>
                        <i class="fa-solid fa-chevron-left"></i>
                    </div>
                `;
                grid.appendChild(card);
            });
        }

        function showSectionsView(classKey) {
            selectedClassKey = classKey;
            document.getElementById('view-classes').classList.add('hidden');
            document.getElementById('view-sections').classList.remove('hidden');
            document.getElementById('view-students').classList.add('hidden');
            document.getElementById('view-student-profile').classList.add('hidden');

            const className = CURRICULUM[classKey].name;
            document.getElementById('current-class-title').innerText = `صفوف ${className} - الشعب`;

            document.getElementById('breadcrumb').innerHTML = `
                <span class="text-slate-400 cursor-pointer hover:underline" onclick="showClassesView()">الصفوف</span>
                <i class="fa-solid fa-chevron-left text-xs text-slate-400"></i>
                <span class="text-brand-royal">${className}</span>
            `;

            renderSections();
        }

        function renderSections() {
            const grid = document.getElementById('sections-grid');
            grid.innerHTML = '';
            const sections = ['أ', 'ب', 'ج', 'د'];

            sections.forEach(sec => {
                const count = DB.students.filter(s => s.classKey === selectedClassKey && s.section === sec).length;
                const card = document.createElement('div');
                card.className = "bg-white p-5 rounded-xl border border-slate-200 shadow-sm hover:shadow-md cursor-pointer text-center space-y-2 hover:border-brand-royal transition";
                card.onclick = () => showStudentsView(sec);
                card.innerHTML = `
                    <div class="text-3xl font-black text-brand-royal">الشعبة (${sec})</div>
                    <div class="text-xs font-bold text-slate-500">${count} طالب مسجل</div>
                `;
                grid.appendChild(card);
            });
        }

        function showStudentsView(section) {
            selectedSection = section;
            document.getElementById('view-classes').classList.add('hidden');
            document.getElementById('view-sections').classList.add('hidden');
            document.getElementById('view-students').classList.remove('hidden');
            document.getElementById('view-student-profile').classList.add('hidden');

            const className = CURRICULUM[selectedClassKey].name;
            document.getElementById('current-section-title').innerText = `${className} - الشعبة (${section})`;

            document.getElementById('breadcrumb').innerHTML = `
                <span class="text-slate-400 cursor-pointer hover:underline" onclick="showClassesView()">الصفوف</span>
                <i class="fa-solid fa-chevron-left text-xs text-slate-400"></i>
                <span class="text-slate-400 cursor-pointer hover:underline" onclick="showSectionsView('${selectedClassKey}')">${className}</span>
                <i class="fa-solid fa-chevron-left text-xs text-slate-400"></i>
                <span class="text-brand-royal">شعبة ${section}</span>
            `;

            renderStudentsList();
        }

        function renderStudentsList() {
            const grid = document.getElementById('students-grid');
            grid.innerHTML = '';

            const students = DB.students.filter(s => s.classKey === selectedClassKey && s.section === selectedSection);

            if (students.length === 0) {
                grid.innerHTML = `<div class="col-span-full p-8 text-center text-slate-400 font-bold bg-white rounded-xl border">لا يوجد طلاب مسجلون في هذه الشعبة حتى الآن.</div>`;
                return;
            }

            students.forEach(s => {
                const card = document.createElement('div');
                card.className = "bg-white p-4 rounded-xl border border-slate-200 shadow-sm hover:border-brand-gold cursor-pointer transition flex items-center justify-between";
                card.onclick = () => showStudentProfile(s.id);
                card.innerHTML = `
                    <div class="flex items-center gap-3">
                        <div class="w-10 h-10 rounded-full bg-slate-100 text-brand-navy font-bold flex items-center justify-center">
                            <i class="fa-solid fa-user"></i>
                        </div>
                        <div>
                            <div class="font-bold text-slate-800 text-sm">${s.name}</div>
                            <div class="text-[10px] text-slate-500">${s.phone}</div>
                        </div>
                    </div>
                    <i class="fa-solid fa-chevron-left text-slate-300 text-xs"></i>
                `;
                grid.appendChild(card);
            });
        }

        function showStudentProfile(studentId) {
            selectedStudentId = studentId;
            const student = DB.students.find(s => s.id === studentId);
            if (!student) return;

            document.getElementById('view-classes').classList.add('hidden');
            document.getElementById('view-sections').classList.add('hidden');
            document.getElementById('view-students').classList.add('hidden');
            document.getElementById('view-student-profile').classList.remove('hidden');

            const className = CURRICULUM[student.classKey].name;

            document.getElementById('breadcrumb').innerHTML = `
                <span class="text-slate-400 cursor-pointer hover:underline" onclick="showClassesView()">الصفوف</span>
                <i class="fa-solid fa-chevron-left text-xs text-slate-400"></i>
                <span class="text-slate-400 cursor-pointer hover:underline" onclick="showSectionsView('${student.classKey}')">${className}</span>
                <i class="fa-solid fa-chevron-left text-xs text-slate-400"></i>
                <span class="text-slate-400 cursor-pointer hover:underline" onclick="showStudentsView('${student.section}')">شعبة ${student.section}</span>
                <i class="fa-solid fa-chevron-left text-xs text-slate-400"></i>
                <span class="text-brand-royal">${student.name}</span>
            `;

            // Fill Data
            document.getElementById('profile-student-name').innerText = student.name;
            document.getElementById('profile-student-meta').innerText = `${className} | الشعبة (${student.section}) | رقم الهاتف: ${student.phone}`;
            document.getElementById('profile-student-absences').innerText = student.absences || 0;

            renderWarnings(student);
            renderMarksTable(student);
        }

        function renderWarnings(student) {
            const list = document.getElementById('profile-student-warnings');
            list.innerHTML = '';
            if (!student.warnings || student.warnings.length === 0) {
                list.innerHTML = `<li class="text-slate-400">لا يوجد تعهدات أو إنذارات مسجلة.</li>`;
                return;
            }
            student.warnings.forEach(w => {
                const li = document.createElement('li');
                li.innerText = w;
                list.appendChild(li);
            });
        }

        function changeAbsence(delta) {
            const student = DB.students.find(s => s.id === selectedStudentId);
            if (!student) return;
            student.absences = Math.max(0, (student.absences || 0) + delta);
            document.getElementById('profile-student-absences').innerText = student.absences;
            saveDB();
        }

        function addWarning() {
            const student = DB.students.find(s => s.id === selectedStudentId);
            if (!student) return;
            const text = prompt('ادخل نص التعهد أو الإنذار الجديد:');
            if (text && text.trim()) {
                if (!student.warnings) student.warnings = [];
                student.warnings.push(text.trim());
                saveDB();
                renderWarnings(student);
            }
        }

        function renderMarksTable(student) {
            const tbody = document.getElementById('marks-table-body');
            tbody.innerHTML = '';

            const subjects = CURRICULUM[student.classKey].subjects;
            const isTeacher = currentUser && currentUser.role === 'teacher';

            subjects.forEach(sub => {
                if (!student.marks[sub]) {
                    student.marks[sub] = { m1: '', m2: '', half: '', mid: '', m3: '', m4: '', half2: '', finalExam: '', secondRound: '' };
                }
                const m = student.marks[sub];

                // Calculations
                const half1 = calculateAvg(m.m1, m.m2);
                const half2 = calculateAvg(m.m3, m.m4);
                const saie = calculateSaie(half1, m.mid, half2);
                const finalGrade = calculateFinal(saie, m.finalExam, m.secondRound);

                const tr = document.createElement('tr');
                tr.className = "hover:bg-slate-50 font-bold";

                tr.innerHTML = `
                    <td class="p-2 border text-right font-black bg-slate-100 text-slate-800">${sub}</td>
                    <td class="p-1 border">${renderCellInput(student.id, sub, 'm1', m.m1, isTeacher)}</td>
                    <td class="p-1 border">${renderCellInput(student.id, sub, 'm2', m.m2, isTeacher)}</td>
                    <td class="p-2 border bg-amber-50 text-amber-900 font-extrabold">${half1}</td>
                    <td class="p-1 border">${renderCellInput(student.id, sub, 'mid', m.mid, isTeacher)}</td>
                    <td class="p-1 border">${renderCellInput(student.id, sub, 'm3', m.m3, isTeacher)}</td>
                    <td class="p-1 border">${renderCellInput(student.id, sub, 'm4', m.m4, isTeacher)}</td>
                    <td class="p-2 border bg-amber-50 text-amber-900 font-extrabold">${half2}</td>
                    <td class="p-2 border bg-emerald-50 text-emerald-900 font-extrabold">${saie}</td>
                    <td class="p-1 border">${renderCellInput(student.id, sub, 'finalExam', m.finalExam, isTeacher)}</td>
                    <td class="p-2 border bg-amber-100 text-brand-navy font-black text-sm">${finalGrade}</td>
                    <td class="p-1 border">${renderCellInput(student.id, sub, 'secondRound', m.secondRound, isTeacher)}</td>
                `;
                tbody.appendChild(tr);
            });
        }

        function renderCellInput(stdId, subject, field, val, isTeacher) {
            if (!isTeacher) {
                return `<span class="text-slate-700">${val !== '' ? val : '-'}</span>`;
            }
            return `<input type="number" min="0" max="100" value="${val}" onchange="updateMark('${stdId}', '${subject}', '${field}', this.value)" class="w-12 text-center py-1 border rounded focus:ring-2 focus:ring-brand-royal focus:outline-none">`;
        }

        function updateMark(stdId, subject, field, val) {
            const student = DB.students.find(s => s.id === stdId);
            if (!student) return;
            student.marks[subject][field] = val !== '' ? Number(val) : '';
            saveDB();
            renderMarksTable(student);
        }

        function calculateAvg(a, b) {
            if (a === '' && b === '') return '-';
            const numA = Number(a) || 0;
            const numB = Number(b) || 0;
            return Math.round((numA + numB) / (a !== '' && b !== '' ? 2 : 1));
        }

        function calculateSaie(h1, mid, h2) {
            const v1 = Number(h1) || 0;
            const vMid = Number(mid) || 0;
            const v2 = Number(h2) || 0;
            if (h1 === '-' && mid === '' && h2 === '-') return '-';
            return Math.round((v1 + vMid + v2) / 3);
        }

        function calculateFinal(saie, finalExam, secondRound) {
            if (secondRound !== '') return `${secondRound} (د2)`;
            if (saie === '-' || finalExam === '') return '-';
            const total = Math.round((Number(saie) + Number(finalExam)) / 2);
            return total;
        }

        // Modal Handlers
        function openAddStudentModal() {
            document.getElementById('modal-add-student').classList.remove('hidden');
            document.getElementById('modal-add-student').classList.add('flex');
        }

        function closeAddStudentModal() {
            document.getElementById('modal-add-student').classList.add('hidden');
            document.getElementById('modal-add-student').classList.remove('flex');
        }

        function saveNewStudent() {
            const name = document.getElementById('new-student-name').value.trim();
            const phone = document.getElementById('new-student-phone').value.trim();

            if (!name || !phone) {
                alert('يرجى كتابة اسم الطالب ورقم الهاتف!');
                return;
            }

            const newStd = {
                id: 'std_' + Date.now(),
                name,
                phone,
                classKey: selectedClassKey || 'first_avg',
                section: selectedSection || 'أ',
                absences: 0,
                warnings: [],
                marks: {}
            };

            DB.students.push(newStd);
            saveDB();
            closeAddStudentModal();

            if (selectedClassKey && selectedSection) {
                renderStudentsList();
            }
            alert('تمت إضافة الطالب بنجاح!');
        }
    </script>
</body>
</html>
