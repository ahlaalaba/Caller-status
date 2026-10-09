<!DOCTYPE html>
<html lang="ar" dir="rtl">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>نظام إدارة المستخدمين وحالات التواجد</title>
    <!-- Tailwind CSS -->
    <script src="https://cdn.tailwindcss.com"></script>
    <!-- FontAwesome Icons -->
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <!-- Google Fonts: Cairo -->
    <link href="https://fonts.googleapis.com/css2?family=Cairo:wght@300;400;500;600;700;800&display=swap" rel="stylesheet">
    <script>
        tailwind.config = {
            theme: {
                extend: {
                    fontFamily: {
                        cairo: ['Cairo', 'sans-serif'],
                    },
                    colors: {
                        brand: {
                            50: '#f0f7ff',
                            100: '#e0effe',
                            500: '#0284c7',
                            600: '#0369a1',
                            700: '#075985',
                            800: '#0c4a6e',
                            900: '#0f172a',
                        }
                    }
                }
            }
        }
    </script>
    <style>
        body {
            font-family: 'Cairo', sans-serif;
            background-color: #0f172a;
            color: #f8fafc;
        }
        .glass-card {
            background: rgba(30, 41, 59, 0.7);
            backdrop-filter: blur(12px);
            border: 1px solid rgba(255, 255, 255, 0.08);
        }
        .pulse-online {
            box-shadow: 0 0 0 0 rgba(34, 197, 94, 0.7);
            animation: pulse-green 2s infinite;
        }
        .pulse-away {
            box-shadow: 0 0 0 0 rgba(245, 158, 11, 0.7);
            animation: pulse-amber 2s infinite;
        }
        @keyframes pulse-green {
            0% { transform: scale(0.95); box-shadow: 0 0 0 0 rgba(34, 197, 94, 0.7); }
            70% { transform: scale(1); box-shadow: 0 0 0 8px rgba(34, 197, 94, 0); }
            100% { transform: scale(0.95); box-shadow: 0 0 0 0 rgba(34, 197, 94, 0); }
        }
        @keyframes pulse-amber {
            0% { transform: scale(0.95); box-shadow: 0 0 0 0 rgba(245, 158, 11, 0.7); }
            70% { transform: scale(1); box-shadow: 0 0 0 8px rgba(245, 158, 11, 0); }
            100% { transform: scale(0.95); box-shadow: 0 0 0 0 rgba(245, 158, 11, 0); }
        }
        /* Custom scrollbar */
        ::-webkit-scrollbar {
            width: 6px;
            height: 6px;
        }
        ::-webkit-scrollbar-track {
            background: #0f172a;
        }
        ::-webkit-scrollbar-thumb {
            background: #334155;
            border-radius: 3px;
        }
        ::-webkit-scrollbar-thumb:hover {
            background: #475569;
        }
    </style>
</head>
<body class="min-h-screen flex flex-col justify-between selection:bg-sky-500 selection:text-white">

    <!-- App Header / Navbar -->
    <header class="glass-card sticky top-0 z-40 border-b border-slate-800 shadow-lg">
        <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8">
            <div class="flex items-center justify-between h-16">
                <!-- Logo & System Title -->
                <div class="flex items-center space-x-3 space-x-reverse">
                    <div class="w-10 h-10 rounded-xl bg-gradient-to-tr from-sky-500 to-indigo-600 flex items-center justify-center text-white shadow-lg shadow-sky-500/30">
                        <i class="fa-solid fa-user-shield text-xl"></i>
                    </div>
                    <div>
                        <h1 class="font-bold text-lg text-white leading-tight">منصة إدارة التواجد</h1>
                        <p class="text-xs text-slate-400">نظام المراقبة والتحكم الحقيقي</p>
                    </div>
                </div>

                <!-- Logged In User Controls (Visible when logged in) -->
                <div id="nav-user-controls" class="hidden flex items-center space-x-4 space-x-reverse">
                    <!-- Status Selector Pill -->
                    <div class="relative">
                        <button id="status-dropdown-btn" onclick="toggleStatusDropdown()" class="flex items-center space-x-2 space-x-reverse px-3 py-1.5 rounded-lg bg-slate-800 border border-slate-700 hover:border-slate-600 transition text-sm">
                            <span id="current-status-dot" class="w-3 h-3 rounded-full bg-emerald-500 inline-block"></span>
                            <span id="current-status-text" class="font-medium text-slate-200">متصل</span>
                            <i class="fa-solid fa-chevron-down text-xs text-slate-400 mr-1"></i>
                        </button>
                        
                        <!-- Status Dropdown Menu -->
                        <div id="status-dropdown" class="hidden absolute left-0 mt-2 w-48 rounded-xl glass-card bg-slate-900 border border-slate-700 shadow-2xl py-2 z-50">
                            <button onclick="changeUserStatus('online')" class="w-full text-right px-4 py-2 text-sm text-slate-300 hover:bg-slate-800 flex items-center space-x-2 space-x-reverse">
                                <span class="w-2.5 h-2.5 rounded-full bg-emerald-500"></span>
                                <span>متصل (Online)</span>
                            </button>
                            <button onclick="changeUserStatus('away')" class="w-full text-right px-4 py-2 text-sm text-slate-300 hover:bg-slate-800 flex items-center space-x-2 space-x-reverse">
                                <span class="w-2.5 h-2.5 rounded-full bg-amber-500"></span>
                                <span>بعيد / غائب (Away)</span>
                            </button>
                            <button onclick="changeUserStatus('offline')" class="w-full text-right px-4 py-2 text-sm text-slate-300 hover:bg-slate-800 flex items-center space-x-2 space-x-reverse">
                                <span class="w-2.5 h-2.5 rounded-full bg-slate-500"></span>
                                <span>غير متصل (Offline)</span>
                            </button>
                        </div>
                    </div>

                    <!-- User Info & Avatar -->
                    <div class="flex items-center space-x-3 space-x-reverse border-r border-slate-700 pr-4">
                        <div class="text-left hidden sm:block">
                            <div id="logged-user-name" class="text-sm font-semibold text-slate-200">--</div>
                            <div id="logged-user-role" class="text-xs text-sky-400 font-medium">--</div>
                        </div>
                        <button onclick="logout()" title="تسجيل الخروج" class="w-9 h-9 rounded-lg bg-rose-500/10 border border-rose-500/20 text-rose-400 hover:bg-rose-500/20 transition flex items-center justify-center">
                            <i class="fa-solid fa-right-from-bracket"></i>
                        </button>
                    </div>
                </div>
            </div>
        </div>
    </header>

    <!-- Main Content Area -->
    <main class="flex-grow flex items-center justify-center p-4 sm:p-6 lg:p-8">

        <!-- SECTION 1: LOGIN & FORGOT PASSWORD PANEL -->
        <div id="auth-section" class="w-full max-w-md space-y-6">
            
            <!-- Quick Login Shortcut Cards (For easy demo testing) -->
            <div class="glass-card rounded-2xl p-4 shadow-xl border border-slate-800">
                <div class="text-xs font-semibold text-slate-400 mb-2 flex items-center justify-between">
                    <span><i class="fa-solid fa-bolt text-amber-400 ml-1"></i> اختيار حساب بنقرة واحدة:</span>
                    <span class="text-[10px] text-slate-500">دخول مباشر</span>
                </div>
                <div class="grid grid-cols-2 gap-2 text-xs">
                    <button onclick="quickLogin('36386184')" class="p-2.5 rounded-xl bg-indigo-950/60 hover:bg-indigo-900/60 border border-indigo-800/60 text-indigo-200 text-right transition flex items-center justify-between">
                        <div>
                            <p class="font-bold">الأدمن</p>
                            <p class="text-[10px] text-indigo-400 font-mono">36386184</p>
                        </div>
                        <i class="fa-solid fa-user-shield text-indigo-400 text-base"></i>
                    </button>
                    <button onclick="quickLogin('mohd@39541014')" class="p-2.5 rounded-xl bg-slate-800/80 hover:bg-slate-800 border border-slate-700 text-slate-200 text-right transition flex items-center justify-between">
                        <div>
                            <p class="font-bold">محمد الشيخ</p>
                            <p class="text-[10px] text-slate-400 font-mono">mohd@39541014</p>
                        </div>
                        <i class="fa-solid fa-user text-slate-400"></i>
                    </button>
                    <button onclick="quickLogin('majeed@33184684')" class="p-2.5 rounded-xl bg-slate-800/80 hover:bg-slate-800 border border-slate-700 text-slate-200 text-right transition flex items-center justify-between">
                        <div>
                            <p class="font-bold">مجيد الشيخ</p>
                            <p class="text-[10px] text-slate-400 font-mono">majeed@33184684</p>
                        </div>
                        <i class="fa-solid fa-user text-slate-400"></i>
                    </button>
                    <button onclick="quickLogin('zuhair@33793910')" class="p-2.5 rounded-xl bg-slate-800/80 hover:bg-slate-800 border border-slate-700 text-slate-200 text-right transition flex items-center justify-between">
                        <div>
                            <p class="font-bold">زهير الشيخ</p>
                            <p class="text-[10px] text-slate-400 font-mono">zuhair@33793910</p>
                        </div>
                        <i class="fa-solid fa-user text-slate-400"></i>
                    </button>
                    <button onclick="quickLogin('abbas@38121180')" class="col-span-2 p-2.5 rounded-xl bg-slate-800/80 hover:bg-slate-800 border border-slate-700 text-slate-200 text-right transition flex items-center justify-between">
                        <div>
                            <p class="font-bold">عباس</p>
                            <p class="text-[10px] text-slate-400 font-mono">abbas@38121180</p>
                        </div>
                        <i class="fa-solid fa-user text-slate-400"></i>
                    </button>
                </div>
            </div>

            <!-- Login Form Card (Without Password Field) -->
            <div id="login-card" class="glass-card rounded-2xl p-6 sm:p-8 shadow-2xl border border-slate-800 relative overflow-hidden">
                <div class="absolute top-0 right-0 w-32 h-32 bg-sky-500/10 rounded-full blur-2xl pointer-events-none"></div>
                
                <div class="text-center mb-6">
                    <h2 class="text-2xl font-bold text-white mb-1">تسجيل الدخول</h2>
                    <p class="text-slate-400 text-sm">أدخل اسم المستخدم أو المعرف فقط للوصول</p>
                </div>

                <form id="login-form" onsubmit="handleLogin(event)" class="space-y-4">
                    <div>
                        <div class="flex justify-between items-center mb-1.5">
                            <label class="block text-sm font-medium text-slate-300">اسم المستخدم / المعرف</label>
                            <button type="button" onclick="switchForm('forgot')" class="text-xs text-sky-400 hover:text-sky-300 transition">نسيت البيانات / استرجاع؟</button>
                        </div>
                        <div class="relative">
                            <div class="absolute inset-y-0 right-0 pr-3 flex items-center pointer-events-none text-slate-400">
                                <i class="fa-solid fa-id-card"></i>
                            </div>
                            <input type="text" id="login-username" required class="w-full pr-10 pl-4 py-3 rounded-xl bg-slate-900/90 border border-slate-700 text-white placeholder-slate-500 focus:outline-none focus:border-sky-500 focus:ring-1 focus:ring-sky-500 transition text-sm" placeholder="أدخل اسم المستخدم (مثال: 36386184 أو mohd@39541014)">
                        </div>
                    </div>

                    <button type="submit" class="w-full py-3 px-4 rounded-xl bg-gradient-to-r from-sky-500 to-blue-600 hover:from-sky-400 hover:to-blue-500 text-white font-bold text-sm shadow-lg shadow-sky-500/25 transition duration-200 mt-2 flex items-center justify-center space-x-2 space-x-reverse">
                        <span>تسجيل الدخول الآن</span>
                        <i class="fa-solid fa-arrow-left"></i>
                    </button>
                </form>
            </div>

            <!-- Forgot Password Card -->
            <div id="forgot-card" class="hidden glass-card rounded-2xl p-6 sm:p-8 shadow-2xl border border-slate-800 relative overflow-hidden">
                <div class="text-center mb-6">
                    <div class="w-12 h-12 rounded-full bg-amber-500/10 border border-amber-500/20 text-amber-400 flex items-center justify-center mx-auto mb-3 text-xl">
                        <i class="fa-solid fa-key"></i>
                    </div>
                    <h2 class="text-2xl font-bold text-white mb-1">استعادة معلومات الحساب</h2>
                    <p class="text-slate-400 text-sm">أدخل اسم المستخدم لإرسال تفاصيل الحساب ورمز التحقق</p>
                </div>

                <form id="forgot-form" onsubmit="handleForgotPassword(event)" class="space-y-4">
                    <div>
                        <label class="block text-sm font-medium text-slate-300 mb-1.5">اسم المستخدم / المعرف</label>
                        <div class="relative">
                            <div class="absolute inset-y-0 right-0 pr-3 flex items-center pointer-events-none text-slate-400">
                                <i class="fa-solid fa-user-tag"></i>
                            </div>
                            <input type="text" id="forgot-username" required class="w-full pr-10 pl-4 py-2.5 rounded-xl bg-slate-900/90 border border-slate-700 text-white placeholder-slate-500 focus:outline-none focus:border-amber-500 focus:ring-1 focus:ring-amber-500 transition text-sm" placeholder="أدخل اسم المستخدم هنا">
                        </div>
                    </div>

                    <button type="submit" class="w-full py-3 px-4 rounded-xl bg-gradient-to-r from-amber-500 to-orange-600 hover:from-amber-400 hover:to-orange-500 text-white font-bold text-sm shadow-lg shadow-amber-500/25 transition duration-200 flex items-center justify-center space-x-2 space-x-reverse">
                        <i class="fa-solid fa-paper-plane"></i>
                        <span>إرسال رسالة الاستعادة</span>
                    </button>

                    <button type="button" onclick="switchForm('login')" class="w-full text-center text-sm text-slate-400 hover:text-slate-200 transition mt-2">
                        <i class="fa-solid fa-chevron-right text-xs ml-1"></i> العودة لتسجيل الدخول
                    </button>
                </form>

                <!-- Recovered Info Notification Card (SMS Simulation Result) -->
                <div id="recovered-alert" class="hidden mt-6 p-4 rounded-xl bg-slate-900/90 border border-emerald-500/30 text-right space-y-3 relative overflow-hidden animate-fade-in">
                    <div class="flex items-center space-x-2 space-x-reverse text-emerald-400 font-bold text-sm">
                        <i class="fa-solid fa-circle-check text-lg"></i>
                        <span>تم العثور على الحساب وإرسال الرسالة!</span>
                    </div>
                    <div class="bg-slate-950 p-3 rounded-lg border border-slate-800 text-xs space-y-1.5 font-mono">
                        <div class="text-slate-400"><span class="text-amber-400 font-bold">اسم المستخدم:</span> <span id="rec-user" class="text-white"></span></div>
                        <div class="text-slate-400"><span class="text-emerald-400 font-bold">كلمة المرور المسجلة:</span> <span id="rec-pass" class="text-white font-bold bg-slate-800 px-2 py-0.5 rounded"></span></div>
                        <div class="text-slate-400"><span class="text-sky-400 font-bold">رمز الأمان التلقائي:</span> <span id="rec-code" class="text-sky-300 font-bold tracking-widest"></span></div>
                        <div class="text-[10px] text-slate-500 pt-1 border-t border-slate-800 mt-1">الرسالة: هذا هو الرمز للتحقق وتم استرجاع معلومات الحساب بنجاح.</div>
                    </div>
                    <button onclick="autofillAndLogin()" class="w-full py-2 bg-emerald-600/20 hover:bg-emerald-600/30 border border-emerald-500/40 text-emerald-300 text-xs font-bold rounded-lg transition">
                        تسجيل الدخول المباشر بهذا الحساب
                    </button>
                </div>
            </div>

        </div>

        <!-- SECTION 2: USER DASHBOARD (Normal User View) -->
        <div id="user-dashboard" class="hidden w-full max-w-4xl space-y-6">
            
            <!-- User Welcome Banner & Active Timer Card -->
            <div class="glass-card rounded-2xl p-6 sm:p-8 shadow-2xl border border-slate-800 relative overflow-hidden">
                <div class="flex flex-col md:flex-row items-center justify-between gap-6">
                    <div class="flex items-center space-x-4 space-x-reverse">
                        <div class="relative">
                            <div class="w-16 h-16 rounded-2xl bg-slate-800 border border-slate-700 flex items-center justify-center text-3xl text-sky-400">
                                <i class="fa-solid fa-user-astronaut"></i>
                            </div>
                            <span id="user-badge-status" class="absolute -bottom-1 -left-1 w-5 h-5 rounded-full bg-emerald-500 border-2 border-slate-900 pulse-online"></span>
                        </div>
                        <div>
                            <h2 class="text-2xl font-bold text-white flex items-center gap-2">
                                أهلاً بك، <span id="dash-user-name" class="text-sky-400">--</span>
                            </h2>
                            <p id="dash-user-id" class="text-slate-400 text-xs font-mono mt-0.5">ID: --</p>
                        </div>
                    </div>

                    <!-- Away Live Counter Box -->
                    <div id="away-timer-card" class="hidden w-full md:w-auto p-4 rounded-xl bg-amber-500/10 border border-amber-500/30 text-center md:text-right">
                        <div class="text-xs font-semibold text-amber-400 flex items-center justify-center md:justify-start space-x-1.5 space-x-reverse mb-1">
                            <i class="fa-solid fa-clock-rotate-left animate-spin"></i>
                            <span>عدّاد مدة الغياب (Away Timer):</span>
                        </div>
                        <div class="text-2xl font-extrabold text-amber-200 font-mono tracking-wider" id="away-timer-clock">
                            00:00:00
                        </div>
                        <p class="text-[11px] text-amber-300/70 mt-1">أنت الآن في حالة "غائب/بعيد". يتم تسجيل المدة تلقائياً.</p>
                    </div>
                </div>
            </div>

            <!-- Presence Quick Action Buttons -->
            <div class="grid grid-cols-1 sm:grid-cols-3 gap-4">
                <button onclick="changeUserStatus('online')" class="glass-card p-5 rounded-xl border border-slate-800 hover:border-emerald-500/50 hover:bg-emerald-950/20 transition text-right group">
                    <div class="flex items-center justify-between mb-2">
                        <span class="w-3 h-3 rounded-full bg-emerald-500 group-hover:scale-125 transition"></span>
                        <i class="fa-solid fa-circle-check text-emerald-400 text-lg"></i>
                    </div>
                    <div class="font-bold text-white">متصل (Online)</div>
                    <div class="text-xs text-slate-400 mt-1">تأكيد تواجدك ونشاطك الآن</div>
                </button>

                <button onclick="changeUserStatus('away')" class="glass-card p-5 rounded-xl border border-slate-800 hover:border-amber-500/50 hover:bg-amber-950/20 transition text-right group">
                    <div class="flex items-center justify-between mb-2">
                        <span class="w-3 h-3 rounded-full bg-amber-500 group-hover:scale-125 transition"></span>
                        <i class="fa-solid fa-user-clock text-amber-400 text-lg"></i>
                    </div>
                    <div class="font-bold text-white">بعيد (Away)</div>
                    <div class="text-xs text-slate-400 mt-1">بدء العداد الزمني لحالة الغياب</div>
                </button>

                <button onclick="changeUserStatus('offline')" class="glass-card p-5 rounded-xl border border-slate-800 hover:border-slate-500/50 hover:bg-slate-800/40 transition text-right group">
                    <div class="flex items-center justify-between mb-2">
                        <span class="w-3 h-3 rounded-full bg-slate-500 group-hover:scale-125 transition"></span>
                        <i class="fa-solid fa-power-off text-slate-400 text-lg"></i>
                    </div>
                    <div class="font-bold text-white">غير متصل (Offline)</div>
                    <div class="text-xs text-slate-400 mt-1">إظهار الحساب كغير متصل</div>
                </button>
            </div>

            <!-- Profile Info Card -->
            <div class="glass-card rounded-2xl p-6 border border-slate-800">
                <h3 class="text-lg font-bold text-white mb-4 border-b border-slate-800 pb-3 flex items-center gap-2">
                    <i class="fa-solid fa-id-card-clip text-sky-400"></i> بيانات تفاصيل الحساب الشخصي
                </h3>
                <div class="grid grid-cols-1 md:grid-cols-2 gap-4 text-sm">
                    <div class="p-3 rounded-xl bg-slate-900/60 border border-slate-800">
                        <span class="text-xs text-slate-400 block mb-1">الاسم الكامل:</span>
                        <span id="profile-fullname" class="font-semibold text-slate-200">--</span>
                    </div>
                    <div class="p-3 rounded-xl bg-slate-900/60 border border-slate-800">
                        <span class="text-xs text-slate-400 block mb-1">اسم المستخدم / المعرف:</span>
                        <span id="profile-username" class="font-semibold text-slate-200 font-mono">--</span>
                    </div>
                    <div class="p-3 rounded-xl bg-slate-900/60 border border-slate-800">
                        <span class="text-xs text-slate-400 block mb-1">كلمة المرور المحفوظة بالنظام:</span>
                        <span id="profile-password" class="font-semibold text-slate-200 font-mono">--</span>
                    </div>
                    <div class="p-3 rounded-xl bg-slate-900/60 border border-slate-800">
                        <span class="text-xs text-slate-400 block mb-1">صلاحية الحساب:</span>
                        <span id="profile-role" class="font-semibold text-sky-400">مستخدم عادي</span>
                    </div>
                </div>
            </div>

        </div>

        <!-- SECTION 3: ADMIN DASHBOARD PANEL (Super Admin View) -->
        <div id="admin-dashboard" class="hidden w-full max-w-6xl space-y-6">
            
            <!-- Admin Top Stats Section -->
            <div class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-4 gap-4">
                <div class="glass-card p-5 rounded-2xl border border-slate-800 flex items-center justify-between">
                    <div>
                        <p class="text-xs text-slate-400 font-medium">إجمالي المستخدمين</p>
                        <h4 id="stat-total-users" class="text-2xl font-black text-white mt-1">0</h4>
                    </div>
                    <div class="w-12 h-12 rounded-xl bg-indigo-500/10 border border-indigo-500/20 text-indigo-400 flex items-center justify-center text-xl">
                        <i class="fa-solid fa-users"></i>
                    </div>
                </div>

                <div class="glass-card p-5 rounded-2xl border border-slate-800 flex items-center justify-between">
                    <div>
                        <p class="text-xs text-slate-400 font-medium">المتصلون الآن (Online)</p>
                        <h4 id="stat-online-users" class="text-2xl font-black text-emerald-400 mt-1">0</h4>
                    </div>
                    <div class="w-12 h-12 rounded-xl bg-emerald-500/10 border border-emerald-500/20 text-emerald-400 flex items-center justify-center text-xl">
                        <i class="fa-solid fa-wifi"></i>
                    </div>
                </div>

                <div class="glass-card p-5 rounded-2xl border border-slate-800 flex items-center justify-between">
                    <div>
                        <p class="text-xs text-slate-400 font-medium">في حالة غياب (Away)</p>
                        <h4 id="stat-away-users" class="text-2xl font-black text-amber-400 mt-1">0</h4>
                    </div>
                    <div class="w-12 h-12 rounded-xl bg-amber-500/10 border border-amber-500/20 text-amber-400 flex items-center justify-center text-xl">
                        <i class="fa-solid fa-user-clock"></i>
                    </div>
                </div>

                <div class="glass-card p-5 rounded-2xl border border-slate-800 flex items-center justify-between">
                    <div>
                        <p class="text-xs text-slate-400 font-medium">غير متصلين (Offline)</p>
                        <h4 id="stat-offline-users" class="text-2xl font-black text-slate-400 mt-1">0</h4>
                    </div>
                    <div class="w-12 h-12 rounded-xl bg-slate-500/10 border border-slate-500/20 text-slate-400 flex items-center justify-center text-xl">
                        <i class="fa-solid fa-user-slash"></i>
                    </div>
                </div>
            </div>

            <!-- Admin Live Presence Table Card -->
            <div class="glass-card rounded-2xl border border-slate-800 overflow-hidden shadow-2xl">
                <div class="p-5 border-b border-slate-800 flex flex-col sm:flex-row sm:items-center justify-between gap-4">
                    <div>
                        <h3 class="text-lg font-bold text-white flex items-center gap-2">
                            <i class="fa-solid fa-desktop text-sky-400"></i> لوحة استكشاف ومراقبة المستخدمين
                        </h3>
                        <p class="text-xs text-slate-400 mt-0.5">مراقبة التواجد والعدادات الزمنية للحسابات لحظة بلحظة</p>
                    </div>

                    <!-- Search Filter Bar -->
                    <div class="relative w-full sm:w-64">
                        <input type="text" id="admin-search-input" oninput="renderAdminTable()" placeholder="بحث باسم أو معرف..." class="w-full pr-9 pl-3 py-1.5 rounded-lg bg-slate-900 border border-slate-700 text-xs text-white placeholder-slate-500 focus:outline-none focus:border-sky-500">
                        <i class="fa-solid fa-magnifying-glass absolute right-3 top-2.5 text-xs text-slate-500"></i>
                    </div>
                </div>

                <!-- Table View -->
                <div class="overflow-x-auto">
                    <table class="w-full text-right border-collapse">
                        <thead>
                            <tr class="bg-slate-900/80 border-b border-slate-800 text-xs text-slate-400 uppercase font-semibold">
                                <th class="py-3 px-4">المستخدم</th>
                                <th class="py-3 px-4">اسم المستخدم / ID</th>
                                <th class="py-3 px-4">الحالة الحالية</th>
                                <th class="py-3 px-4">مدة Away (عداد حي)</th>
                                <th class="py-3 px-4">كلمة المرور</th>
                                <th class="py-3 px-4 text-center">التحكم</th>
                            </tr>
                        </thead>
                        <tbody id="admin-users-table-body" class="divide-y divide-slate-800/60 text-sm">
                            <!-- Rendered dynamically -->
                        </tbody>
                    </table>
                </div>
            </div>

        </div>

    </main>

    <!-- Admin Edit User Password Modal -->
    <div id="edit-user-modal" class="hidden fixed inset-0 z-50 flex items-center justify-center bg-black/70 backdrop-blur-sm p-4">
        <div class="glass-card rounded-2xl max-w-md w-full p-6 border border-slate-700 shadow-2xl relative">
            <h3 class="text-lg font-bold text-white mb-2 flex items-center gap-2">
                <i class="fa-solid fa-user-pen text-sky-400"></i> تعديل كلمة المرور للمستخدم
            </h3>
            <p id="modal-target-username" class="text-xs text-slate-400 mb-4">المستخدم: --</p>

            <form onsubmit="handleAdminPasswordUpdate(event)" class="space-y-4">
                <div>
                    <label class="block text-xs font-medium text-slate-300 mb-1">كلمة المرور الجديدة</label>
                    <input type="text" id="modal-new-password" required class="w-full px-3 py-2 rounded-xl bg-slate-900 border border-slate-700 text-white font-mono text-sm focus:outline-none focus:border-sky-500">
                </div>

                <div class="flex items-center justify-end space-x-2 space-x-reverse pt-2">
                    <button type="button" onclick="closeEditModal()" class="px-4 py-2 rounded-xl bg-slate-800 hover:bg-slate-700 text-slate-300 text-xs font-semibold">إلغاء</button>
                    <button type="submit" class="px-4 py-2 rounded-xl bg-sky-600 hover:bg-sky-500 text-white text-xs font-bold shadow-lg shadow-sky-600/30">حفظ التغييرات</button>
                </div>
            </form>
        </div>
    </div>

    <!-- Footer -->
    <footer class="glass-card border-t border-slate-800 py-4 text-center text-xs text-slate-500">
        <p>نظام إدارة التواجد والمستخدمين الشامل © 2026 - جميع الحقوق محفوظة</p>
    </footer>

    <script>
        // Initial Pre-configured Users Data
        let usersData = [
            {
                id: '36386184',
                username: '36386184',
                password: 'ahlaalaba',
                fullName: 'الأدمن',
                role: 'superadmin',
                status: 'online', // 'online', 'away', 'offline'
                awayStartTime: null, // Timestamp when changed to away
            },
            {
                id: 'mohd@39541014',
                username: 'mohd@39541014',
                password: 'mohd@1986',
                fullName: 'محمد الشيخ',
                role: 'user',
                status: 'offline',
                awayStartTime: null,
            },
            {
                id: 'majeed@33184684',
                username: 'majeed@33184684',
                password: 'majeed@1983',
                fullName: 'مجيد الشيخ',
                role: 'user',
                status: 'offline',
                awayStartTime: null,
            },
            {
                id: 'zuhair@33793910',
                username: 'zuhair@33793910',
                password: 'zuhair@1988',
                fullName: 'زهير الشيخ',
                role: 'user',
                status: 'offline',
                awayStartTime: null,
            },
            {
                id: 'abbas@38121180',
                username: 'abbas@38121180',
                password: 'abbas@2013',
                fullName: 'عباس',
                role: 'user',
                status: 'offline',
                awayStartTime: null,
            }
        ];

        // Global Application State
        let currentUser = null;
        let awayTimerInterval = null;
        let adminTableInterval = null;
        let editingTargetUser = null;

        // Auto Idle Detection Variables
        let idleTimer = null;
        const IDLE_TIMEOUT_MS = 60000; // 1 Minute of inactivity triggers Away state

        // Initialize App on DOM Load
        window.addEventListener('DOMContentLoaded', () => {
            // Load saved state from LocalStorage if available
            const savedData = localStorage.getItem('presence_users_app');
            if (savedData) {
                try {
                    usersData = JSON.parse(savedData);
                } catch (e) {
                    console.error("Failed to parse stored users", e);
                }
            }
            saveState();
            setupIdleListeners();
        });

        function saveState() {
            localStorage.setItem('presence_users_app', JSON.stringify(usersData));
        }

        // Auto Idle Detection Logic
        function setupIdleListeners() {
            const resetIdle = () => {
                if (currentUser && currentUser.status === 'online') {
                    clearTimeout(idleTimer);
                    idleTimer = setTimeout(() => {
                        // User is idle for 1 min, set to Away automatically
                        changeUserStatus('away');
                        showToast('تم تحويل حالتك تلقائياً إلى (غائب / Away) بسبب عدم النشاط');
                    }, IDLE_TIMEOUT_MS);
                }
            };

            window.onload = resetIdle;
            window.onmousemove = resetIdle;
            window.onmousedown = resetIdle;
            window.onclick = resetIdle;
            window.onkeydown = resetIdle;
        }

        // Quick Login Shortcut
        function quickLogin(username) {
            document.getElementById('login-username').value = username;
            loginWithUsername(username);
        }

        // Switch Forms (Login / Forgot Password)
        function switchForm(formName) {
            const loginCard = document.getElementById('login-card');
            const forgotCard = document.getElementById('forgot-card');
            
            if (formName === 'forgot') {
                loginCard.classList.add('hidden');
                forgotCard.classList.remove('hidden');
            } else {
                forgotCard.classList.add('hidden');
                loginCard.classList.remove('hidden');
                document.getElementById('recovered-alert').classList.add('hidden');
            }
        }

        // Handle Login Submission (Only Username)
        function handleLogin(e) {
            e.preventDefault();
            const usernameInput = document.getElementById('login-username').value.trim();
            loginWithUsername(usernameInput);
        }

        function loginWithUsername(usernameInput) {
            const foundUser = usersData.find(u => u.username === usernameInput || u.id === usernameInput);

            if (foundUser) {
                currentUser = foundUser;
                // Automatically set to Online on login
                setUserStatus(currentUser, 'online');
                
                // Show Header User Controls
                document.getElementById('nav-user-controls').classList.remove('hidden');
                document.getElementById('auth-section').classList.add('hidden');

                if (currentUser.role === 'superadmin') {
                    openAdminDashboard();
                } else {
                    openUserDashboard();
                }
            } else {
                alert('خطأ: لم يتم العثور على اسم المستخدم هذا بالنظام!');
            }
        }

        // Handle Forgot Password / Account Recovery
        function handleForgotPassword(e) {
            e.preventDefault();
            const usernameInput = document.getElementById('forgot-username').value.trim();
            const foundUser = usersData.find(u => u.username === usernameInput || u.fullName.includes(usernameInput));

            const alertBox = document.getElementById('recovered-alert');

            if (foundUser) {
                // Generate Random 6-Digit Security Code
                const securityCode = Math.floor(100000 + Math.random() * 900000);

                document.getElementById('rec-user').textContent = foundUser.username + ` (${foundUser.fullName})`;
                document.getElementById('rec-pass').textContent = foundUser.password;
                document.getElementById('rec-code').textContent = securityCode;

                alertBox.classList.remove('hidden');
            } else {
                alert('عذراً، لم يتم العثور على أي حساب بهذا الاسم أو المعرف!');
                alertBox.classList.add('hidden');
            }
        }

        function autofillAndLogin() {
            const recText = document.getElementById('rec-user').textContent;
            const username = recText.split(' ')[0];
            const foundUser = usersData.find(u => u.username === username || u.fullName === username);
            if (foundUser) {
                switchForm('login');
                quickLogin(foundUser.username);
            }
        }

        // Change User Status Logic
        function changeUserStatus(newStatus) {
            if (!currentUser) return;
            setUserStatus(currentUser, newStatus);
            updateHeaderUI();

            if (currentUser.role === 'superadmin') {
                renderAdminTable();
            } else {
                updateUserDashboardUI();
            }
        }

        function setUserStatus(userObj, status) {
            userObj.status = status;
            if (status === 'away') {
                if (!userObj.awayStartTime) {
                    userObj.awayStartTime = Date.now();
                }
            } else {
                userObj.awayStartTime = null;
            }
            saveState();
        }

        // Open User Dashboard
        function openUserDashboard() {
            document.getElementById('admin-dashboard').classList.add('hidden');
            document.getElementById('user-dashboard').classList.remove('hidden');

            updateHeaderUI();
            updateUserDashboardUI();
            startAwayTimer();
        }

        // Open Admin Dashboard
        function openAdminDashboard() {
            document.getElementById('user-dashboard').classList.add('hidden');
            document.getElementById('admin-dashboard').classList.remove('hidden');

            updateHeaderUI();
            renderAdminTable();
            startAdminMonitoringTimer();
        }

        // Update Header User UI
        function updateHeaderUI() {
            if (!currentUser) return;

            document.getElementById('logged-user-name').textContent = currentUser.fullName;
            document.getElementById('logged-user-role').textContent = currentUser.role === 'superadmin' ? 'مدير النظام (Admin)' : 'مستخدم عادي';

            const statusDot = document.getElementById('current-status-dot');
            const statusText = document.getElementById('current-status-text');

            if (currentUser.status === 'online') {
                statusDot.className = 'w-3 h-3 rounded-full bg-emerald-500';
                statusText.textContent = 'متصل (Online)';
            } else if (currentUser.status === 'away') {
                statusDot.className = 'w-3 h-3 rounded-full bg-amber-500';
                statusText.textContent = 'بعيد (Away)';
            } else {
                statusDot.className = 'w-3 h-3 rounded-full bg-slate-500';
                statusText.textContent = 'غير متصل (Offline)';
            }
        }

        // Toggle Status Dropdown
        function toggleStatusDropdown() {
            const dropdown = document.getElementById('status-dropdown');
            dropdown.classList.toggle('hidden');
        }

        // Close dropdown when clicking outside
        window.onclick = function(event) {
            if (!event.target.closest('#status-dropdown-btn')) {
                const dropdown = document.getElementById('status-dropdown');
                if (dropdown && !dropdown.classList.contains('hidden')) {
                    dropdown.classList.add('hidden');
                }
            }
        }

        // Update User Dashboard Card
        function updateUserDashboardUI() {
            if (!currentUser) return;

            document.getElementById('dash-user-name').textContent = currentUser.fullName;
            document.getElementById('dash-user-id').textContent = 'ID: ' + currentUser.username;

            document.getElementById('profile-fullname').textContent = currentUser.fullName;
            document.getElementById('profile-username').textContent = currentUser.username;
            document.getElementById('profile-password').textContent = currentUser.password;
            document.getElementById('profile-role').textContent = currentUser.role === 'superadmin' ? 'مدير النظام الرئيسي' : 'مستخدم (User)';

            const badge = document.getElementById('user-badge-status');
            const awayTimerCard = document.getElementById('away-timer-card');

            if (currentUser.status === 'online') {
                badge.className = 'absolute -bottom-1 -left-1 w-5 h-5 rounded-full bg-emerald-500 border-2 border-slate-900 pulse-online';
                awayTimerCard.classList.add('hidden');
            } else if (currentUser.status === 'away') {
                badge.className = 'absolute -bottom-1 -left-1 w-5 h-5 rounded-full bg-amber-500 border-2 border-slate-900 pulse-amber';
                awayTimerCard.classList.remove('hidden');
            } else {
                badge.className = 'absolute -bottom-1 -left-1 w-5 h-5 rounded-full bg-slate-500 border-2 border-slate-900';
                awayTimerCard.classList.add('hidden');
            }
        }

        // Away Live Counter Function for Logged User
        function startAwayTimer() {
            if (awayTimerInterval) clearInterval(awayTimerInterval);

            awayTimerInterval = setInterval(() => {
                if (currentUser && currentUser.status === 'away' && currentUser.awayStartTime) {
                    const elapsedSeconds = Math.floor((Date.now() - currentUser.awayStartTime) / 1000);
                    document.getElementById('away-timer-clock').textContent = formatSeconds(elapsedSeconds);
                }
            }, 1000);
        }

        // Helper: Format seconds to HH:MM:SS
        function formatSeconds(sec) {
            const hrs = Math.floor(sec / 3600);
            const mins = Math.floor((sec % 3600) / 60);
            const secs = sec % 60;

            const pad = (num) => String(num).padStart(2, '0');
            if (hrs > 0) {
                return `${pad(hrs)}:${pad(mins)}:${pad(secs)}`;
            }
            return `${pad(mins)}:${pad(secs)}`;
        }

        // Render Admin Table & Statistics
        function renderAdminTable() {
            const tableBody = document.getElementById('admin-users-table-body');
            const searchVal = (document.getElementById('admin-search-input').value || '').toLowerCase();

            // Calculate Stats
            let total = usersData.length;
            let onlineCount = usersData.filter(u => u.status === 'online').length;
            let awayCount = usersData.filter(u => u.status === 'away').length;
            let offlineCount = usersData.filter(u => u.status === 'offline').length;

            document.getElementById('stat-total-users').textContent = total;
            document.getElementById('stat-online-users').textContent = onlineCount;
            document.getElementById('stat-away-users').textContent = awayCount;
            document.getElementById('stat-offline-users').textContent = offlineCount;

            const filteredUsers = usersData.filter(u => 
                u.fullName.toLowerCase().includes(searchVal) || 
                u.username.toLowerCase().includes(searchVal)
            );

            tableBody.innerHTML = '';

            filteredUsers.forEach(user => {
                const tr = document.createElement('tr');
                tr.className = 'hover:bg-slate-800/40 transition border-b border-slate-800/40';

                // Status Badge & Text
                let statusBadge = '';
                let awayDurationText = user.status === 'away' && user.awayStartTime 
                    ? formatSeconds(Math.floor((Date.now() - user.awayStartTime) / 1000))
                    : '--';

                if (user.status === 'online') {
                    statusBadge = `<span class="inline-flex items-center px-2.5 py-1 rounded-full text-xs font-semibold bg-emerald-500/10 text-emerald-400 border border-emerald-500/20">
                        <span class="w-1.5 h-1.5 rounded-full bg-emerald-500 ml-1.5"></span> متصل (Online)
                    </span>`;
                } else if (user.status === 'away') {
                    statusBadge = `<span class="inline-flex items-center px-2.5 py-1 rounded-full text-xs font-semibold bg-amber-500/10 text-amber-400 border border-amber-500/20">
                        <span class="w-1.5 h-1.5 rounded-full bg-amber-500 ml-1.5"></span> بعيد (Away)
                    </span>`;
                } else {
                    statusBadge = `<span class="inline-flex items-center px-2.5 py-1 rounded-full text-xs font-semibold bg-slate-500/10 text-slate-400 border border-slate-500/20">
                        <span class="w-1.5 h-1.5 rounded-full bg-slate-500 ml-1.5"></span> غير متصل (Offline)
                    </span>`;
                }

                tr.innerHTML = `
                    <td class="py-3 px-4 font-bold text-white flex items-center space-x-2 space-x-reverse">
                        <div class="w-8 h-8 rounded-lg bg-slate-800 border border-slate-700 flex items-center justify-center text-sky-400 text-xs">
                            <i class="fa-solid ${user.role === 'superadmin' ? 'fa-user-shield text-indigo-400' : 'fa-user'}"></i>
                        </div>
                        <div>
                            <span>${user.fullName}</span>
                            ${user.role === 'superadmin' ? '<span class="text-[10px] text-indigo-400 bg-indigo-950 px-1.5 py-0.5 rounded mr-1">الأدمن</span>' : ''}
                        </div>
                    </td>
                    <td class="py-3 px-4 text-slate-300 font-mono text-xs">${user.username}</td>
                    <td class="py-3 px-4">${statusBadge}</td>
                    <td class="py-3 px-4 text-amber-300 font-mono text-xs font-semibold">
                        ${user.status === 'away' ? `<i class="fa-solid fa-stopwatch text-amber-400 ml-1"></i>${awayDurationText}` : '<span class="text-slate-600">--</span>'}
                    </td>
                    <td class="py-3 px-4 text-slate-300 font-mono text-xs">${user.password}</td>
                    <td class="py-3 px-4 text-center">
                        <div class="flex items-center justify-center space-x-1 space-x-reverse">
                            <!-- Change status dropdown -->
                            <select onchange="adminChangeUserStatus('${user.id}', this.value)" class="bg-slate-900 border border-slate-700 text-slate-200 text-xs rounded-lg px-2 py-1 focus:outline-none">
                                <option value="online" ${user.status === 'online' ? 'selected' : ''}>Online</option>
                                <option value="away" ${user.status === 'away' ? 'selected' : ''}>Away</option>
                                <option value="offline" ${user.status === 'offline' ? 'selected' : ''}>Offline</option>
                            </select>

                            <!-- Edit Password Button -->
                            <button onclick="openEditModal('${user.id}')" title="تغيير كلمة المرور" class="p-1.5 rounded-lg bg-sky-500/10 hover:bg-sky-500/20 text-sky-400 border border-sky-500/20 transition">
                                <i class="fa-solid fa-key text-xs"></i>
                            </button>
                        </div>
                    </td>
                `;

                tableBody.appendChild(tr);
            });
        }

        // Admin Continuous Live Monitoring Loop
        function startAdminMonitoringTimer() {
            if (adminTableInterval) clearInterval(adminTableInterval);
            adminTableInterval = setInterval(() => {
                if (currentUser && currentUser.role === 'superadmin') {
                    renderAdminTable();
                }
            }, 1000);
        }

        // Admin Change User Status directly
        function adminChangeUserStatus(userId, newStatus) {
            const user = usersData.find(u => u.id === userId);
            if (user) {
                setUserStatus(user, newStatus);
                renderAdminTable();
            }
        }

        // Edit Password Modal Logic
        function openEditModal(userId) {
            editingTargetUser = usersData.find(u => u.id === userId);
            if (editingTargetUser) {
                document.getElementById('modal-target-username').textContent = `المستخدم: ${editingTargetUser.fullName} (${editingTargetUser.username})`;
                document.getElementById('modal-new-password').value = editingTargetUser.password;
                document.getElementById('edit-user-modal').classList.remove('hidden');
            }
        }

        function closeEditModal() {
            document.getElementById('edit-user-modal').classList.add('hidden');
            editingTargetUser = null;
        }

        function handleAdminPasswordUpdate(e) {
            e.preventDefault();
            if (editingTargetUser) {
                const newPass = document.getElementById('modal-new-password').value.trim();
                if (newPass) {
                    editingTargetUser.password = newPass;
                    saveState();
                    renderAdminTable();
                    closeEditModal();
                    alert('تم تغيير كلمة المرور بنجاح!');
                }
            }
        }

        // Logout User
        function logout() {
            if (currentUser) {
                setUserStatus(currentUser, 'offline');
            }
            currentUser = null;

            if (awayTimerInterval) clearInterval(awayTimerInterval);
            if (adminTableInterval) clearInterval(adminTableInterval);

            document.getElementById('nav-user-controls').classList.add('hidden');
            document.getElementById('user-dashboard').classList.add('hidden');
            document.getElementById('admin-dashboard').classList.add('hidden');
            document.getElementById('auth-section').classList.remove('hidden');

            // Reset login fields
            document.getElementById('login-username').value = '';
            switchForm('login');
        }

        // Toast Helper
        function showToast(msg) {
            const toast = document.createElement('div');
            toast.className = 'fixed bottom-5 right-5 z-50 glass-card bg-slate-900 border border-sky-500/50 text-sky-200 px-4 py-3 rounded-xl shadow-2xl text-xs flex items-center space-x-2 space-x-reverse animate-bounce';
            toast.innerHTML = `<i class="fa-solid fa-bell text-amber-400 text-sm"></i> <span>${msg}</span>`;
            document.body.appendChild(toast);
            setTimeout(() => toast.remove(), 4000);
        }
    </script>
</body>
</html>
