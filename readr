<html lang="ar" dir="rtl">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>مضيف أهل العباء - نظام إدارة التواجد</title>
    <!-- Tailwind CSS -->
    <script src="https://cdn.tailwindcss.com"></script>
    <!-- FontAwesome Icons -->
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <!-- Google Fonts: Cairo -->
    <link href="https://fonts.googleapis.com/css2?family=Cairo:wght@300;400;500;600;700;800;900&display=swap" rel="stylesheet">
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
            background-color: #0b0f19;
            color: #f8fafc;
        }
        .glass-card {
            background: rgba(18, 24, 38, 0.75);
            backdrop-filter: blur(16px);
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
        ::-webkit-scrollbar {
            width: 6px;
            height: 6px;
        }
        ::-webkit-scrollbar-track {
            background: #0b0f19;
        }
        ::-webkit-scrollbar-thumb {
            background: #1e293b;
            border-radius: 4px;
        }
        ::-webkit-scrollbar-thumb:hover {
            background: #334155;
        }
    </style>
</head>
<body class="min-h-screen flex flex-col justify-between selection:bg-sky-500 selection:text-white">

    <!-- App Header / Navbar -->
    <header class="glass-card sticky top-0 z-40 border-b border-slate-800 shadow-xl">
        <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8">
            <div class="flex items-center justify-between h-16">
                <!-- Logo & System Title -->
                <div class="flex items-center space-x-3 space-x-reverse">
                    <div class="w-10 h-10 rounded-xl bg-gradient-to-tr from-sky-500 to-indigo-600 flex items-center justify-center text-white shadow-lg shadow-sky-500/25">
                        <i class="fa-solid fa-mosque text-xl"></i>
                    </div>
                    <div>
                        <h1 class="font-bold text-lg text-white leading-tight">مضيف أهل العباء</h1>
                        <p class="text-xs text-slate-400">نظام إدارة التواجد والحسابات</p>
                    </div>
                </div>

                <!-- Logged In User Controls -->
                <div id="nav-user-controls" class="hidden flex items-center space-x-4 space-x-reverse">
                    <!-- Status Selector Pill -->
                    <div class="relative">
                        <button id="status-dropdown-btn" onclick="toggleStatusDropdown()" class="flex items-center space-x-2 space-x-reverse px-3 py-1.5 rounded-xl bg-slate-900 border border-slate-700 hover:border-slate-600 transition text-sm">
                            <span id="current-status-dot" class="w-3 h-3 rounded-full bg-emerald-500 inline-block"></span>
                            <span id="current-status-text" class="font-semibold text-slate-200">متصل</span>
                            <i class="fa-solid fa-chevron-down text-xs text-slate-400 mr-1"></i>
                        </button>
                        
                        <!-- Status Dropdown Menu -->
                        <div id="status-dropdown" class="hidden absolute left-0 mt-2 w-48 rounded-xl glass-card bg-slate-900/95 border border-slate-700 shadow-2xl py-2 z-50">
                            <button onclick="changeUserStatus('online')" class="w-full text-right px-4 py-2 text-sm text-slate-300 hover:bg-slate-800 flex items-center space-x-2 space-x-reverse">
                                <span class="w-2.5 h-2.5 rounded-full bg-emerald-500"></span>
                                <span>متصل (Online)</span>
                            </button>
                            <button onclick="changeUserStatus('away')" class="w-full text-right px-4 py-2 text-sm text-slate-300 hover:bg-slate-800 flex items-center space-x-2 space-x-reverse">
                                <span class="w-2.5 h-2.5 rounded-full bg-amber-500"></span>
                                <span>غائب (Away)</span>
                            </button>
                            <button onclick="changeUserStatus('offline')" class="w-full text-right px-4 py-2 text-sm text-slate-300 hover:bg-slate-800 flex items-center space-x-2 space-x-reverse">
                                <span class="w-2.5 h-2.5 rounded-full bg-slate-500"></span>
                                <span>غير متصل (Offline)</span>
                            </button>
                        </div>
                    </div>

                    <!-- User Info & Avatar -->
                    <div class="flex items-center space-x-3 space-x-reverse border-r border-slate-800 pr-4">
                        <div class="text-left hidden sm:block">
                            <div id="logged-user-name" class="text-sm font-bold text-slate-200">--</div>
                            <div id="logged-user-role" class="text-[11px] text-sky-400 font-medium">--</div>
                        </div>
                        <button onclick="logout()" title="تسجيل الخروج" class="w-9 h-9 rounded-xl bg-rose-500/10 border border-rose-500/20 text-rose-400 hover:bg-rose-500/20 transition flex items-center justify-center">
                            <i class="fa-solid fa-right-from-bracket"></i>
                        </button>
                    </div>
                </div>
            </div>
        </div>
    </header>

    <!-- Main Content Area -->
    <main class="flex-grow flex items-center justify-center p-4 sm:p-6 lg:p-8">

        <!-- SECTION 1: SECURE LOGIN FORM (HIDDEN ACCOUNTS LIST) -->
        <div id="auth-section" class="w-full max-w-md space-y-6">
            
            <!-- Login Form Card -->
            <div id="login-card" class="glass-card rounded-2xl p-6 sm:p-8 shadow-2xl border border-slate-800 relative overflow-hidden">
                <div class="absolute top-0 right-0 w-32 h-32 bg-sky-500/10 rounded-full blur-2xl pointer-events-none"></div>
                
                <div class="text-center mb-6">
                    <div class="w-12 h-12 rounded-2xl bg-sky-500/10 border border-sky-500/20 text-sky-400 flex items-center justify-center mx-auto mb-3 text-2xl shadow-lg shadow-sky-500/10">
                        <i class="fa-solid fa-user-lock"></i>
                    </div>
                    <h2 class="text-2xl font-black text-white mb-1">مضيف أهل العباء</h2>
                    <p class="text-slate-400 text-xs">يرجى إدخال اسم المستخدم وكلمة المرور للدخول</p>
                </div>

                <form id="login-form" onsubmit="handleLogin(event)" class="space-y-4">
                    <div>
                        <label class="block text-xs font-semibold text-slate-300 mb-1.5">اسم المستخدم (Username)</label>
                        <div class="relative">
                            <div class="absolute inset-y-0 right-0 pr-3.5 flex items-center pointer-events-none text-slate-500">
                                <i class="fa-solid fa-user"></i>
                            </div>
                            <input type="text" id="login-username" required class="w-full pr-10 pl-4 py-3 rounded-xl bg-slate-900/90 border border-slate-700/80 text-white placeholder-slate-500 focus:outline-none focus:border-sky-500 focus:ring-1 focus:ring-sky-500 transition text-sm" placeholder="أدخل اسم المستخدم...">
                        </div>
                    </div>

                    <div>
                        <div class="flex justify-between items-center mb-1.5">
                            <label class="block text-xs font-semibold text-slate-300">كلمة المرور (Password)</label>
                            <button type="button" onclick="switchForm('forgot')" class="text-xs text-sky-400 hover:text-sky-300 transition">نسيت كلمة المرور؟</button>
                        </div>
                        <div class="relative">
                            <div class="absolute inset-y-0 right-0 pr-3.5 flex items-center pointer-events-none text-slate-500">
                                <i class="fa-solid fa-key"></i>
                            </div>
                            <input type="password" id="login-password" required class="w-full pr-10 pl-10 py-3 rounded-xl bg-slate-900/90 border border-slate-700/80 text-white placeholder-slate-500 focus:outline-none focus:border-sky-500 focus:ring-1 focus:ring-sky-500 transition text-sm" placeholder="أدخل كلمة المرور...">
                            <button type="button" onclick="togglePasswordVisibility('login-password', 'toggle-icon')" class="absolute inset-y-0 left-0 pl-3 flex items-center text-slate-400 hover:text-slate-200">
                                <i id="toggle-icon" class="fa-solid fa-eye"></i>
                            </button>
                        </div>
                    </div>

                    <button type="submit" class="w-full py-3.5 px-4 rounded-xl bg-gradient-to-r from-sky-500 to-blue-600 hover:from-sky-400 hover:to-blue-500 text-white font-bold text-sm shadow-lg shadow-sky-500/25 transition duration-200 mt-3 flex items-center justify-center space-x-2 space-x-reverse">
                        <span>تسجيل الدخول</span>
                        <i class="fa-solid fa-arrow-left"></i>
                    </button>
                </form>

                <div class="mt-5 text-center text-[11px] text-slate-500 border-t border-slate-800/80 pt-4 flex items-center justify-center gap-1.5">
                    <i class="fa-solid fa-shield-halved text-sky-500"></i>
                    <span>تسجيل دخول آمن ومحمي لنظام مضيف أهل العباء</span>
                </div>
            </div>

            <!-- Forgot Password Card -->
            <div id="forgot-card" class="hidden glass-card rounded-2xl p-6 sm:p-8 shadow-2xl border border-slate-800 relative overflow-hidden">
                <div class="text-center mb-6">
                    <div class="w-12 h-12 rounded-2xl bg-amber-500/10 border border-amber-500/20 text-amber-400 flex items-center justify-center mx-auto mb-3 text-2xl shadow-lg shadow-amber-500/10">
                        <i class="fa-solid fa-rotate-left"></i>
                    </div>
                    <h2 class="text-2xl font-black text-white mb-1">استعادة معلومات الحساب</h2>
                    <p class="text-slate-400 text-xs">أدخل المعرف لإرسال تفاصيل البيانات ورمز OTP التفاعلي</p>
                </div>

                <form id="forgot-form" onsubmit="handleForgotPassword(event)" class="space-y-4">
                    <div>
                        <label class="block text-xs font-semibold text-slate-300 mb-1.5">اسم المستخدم / المعرف</label>
                        <div class="relative">
                            <div class="absolute inset-y-0 right-0 pr-3.5 flex items-center pointer-events-none text-slate-500">
                                <i class="fa-solid fa-user-tag"></i>
                            </div>
                            <input type="text" id="forgot-username" required class="w-full pr-10 pl-4 py-3 rounded-xl bg-slate-900/90 border border-slate-700/80 text-white placeholder-slate-500 focus:outline-none focus:border-amber-500 focus:ring-1 focus:ring-amber-500 transition text-sm" placeholder="أدخل اسم المستخدم المعين...">
                        </div>
                    </div>

                    <button type="submit" class="w-full py-3.5 px-4 rounded-xl bg-gradient-to-r from-amber-500 to-orange-600 hover:from-amber-400 hover:to-orange-500 text-white font-bold text-sm shadow-lg shadow-amber-500/25 transition duration-200 flex items-center justify-center space-x-2 space-x-reverse">
                        <i class="fa-solid fa-paper-plane"></i>
                        <span>طلب كلمة المرور ورمز التحقق SMS</span>
                    </button>

                    <button type="button" onclick="switchForm('login')" class="w-full text-center text-xs text-slate-400 hover:text-slate-200 transition mt-2 flex items-center justify-center gap-1">
                        <i class="fa-solid fa-chevron-right text-[10px]"></i> العودة لتسجيل الدخول
                    </button>
                </form>

                <!-- Recovered SMS Notification -->
                <div id="recovered-alert" class="hidden mt-6 p-4 rounded-xl bg-slate-900/90 border border-emerald-500/30 text-right space-y-3 relative overflow-hidden animate-fade-in">
                    <div class="flex items-center space-x-2 space-x-reverse text-emerald-400 font-bold text-xs">
                        <i class="fa-solid fa-circle-check text-base"></i>
                        <span>تم العثور على الحساب والمحاكاة لرسالة الـ SMS!</span>
                    </div>
                    <div class="bg-slate-950 p-3.5 rounded-xl border border-slate-800 text-xs space-y-2 font-mono">
                        <div class="text-slate-400 flex justify-between"><span class="text-amber-400 font-bold">اسم المستخدم:</span> <span id="rec-user" class="text-white"></span></div>
                        <div class="text-slate-400 flex justify-between"><span class="text-emerald-400 font-bold">كلمة المرور:</span> <span id="rec-pass" class="text-white font-bold bg-slate-800 px-2 py-0.5 rounded border border-slate-700"></span></div>
                        <div class="text-slate-400 flex justify-between"><span class="text-sky-400 font-bold">رمز التفعيل (OTP):</span> <span id="rec-code" class="text-sky-300 font-bold tracking-widest bg-sky-950/60 px-2 py-0.5 rounded border border-sky-800/60"></span></div>
                    </div>
                    <button onclick="autofillAndLogin()" class="w-full py-2.5 bg-emerald-600/20 hover:bg-emerald-600/30 border border-emerald-500/40 text-emerald-300 text-xs font-bold rounded-xl transition">
                        تعبئة البيانات والدخول فوراً
                    </button>
                </div>
            </div>

        </div>

        <!-- SECTION 2: USER DASHBOARD -->
        <div id="user-dashboard" class="hidden w-full max-w-4xl space-y-6">
            
            <div class="glass-card rounded-2xl p-6 sm:p-8 shadow-2xl border border-slate-800 relative overflow-hidden">
                <div class="flex flex-col md:flex-row items-center justify-between gap-6">
                    <div class="flex items-center space-x-4 space-x-reverse">
                        <div class="relative">
                            <div class="w-16 h-16 rounded-2xl bg-slate-900 border border-slate-700 flex items-center justify-center text-3xl text-sky-400 shadow-inner">
                                <i class="fa-solid fa-user-astronaut"></i>
                            </div>
                            <span id="user-badge-status" class="absolute -bottom-1 -left-1 w-5 h-5 rounded-full bg-emerald-500 border-2 border-slate-900 pulse-online"></span>
                        </div>
                        <div>
                            <h2 class="text-2xl font-extrabold text-white flex items-center gap-2">
                                مرحباً بك في مضيف أهل العباء، <span id="dash-user-name" class="text-sky-400">--</span>
                            </h2>
                            <p id="dash-user-id" class="text-slate-400 text-xs font-mono mt-0.5">ID: --</p>
                        </div>
                    </div>

                    <!-- Away Live Counter Box -->
                    <div id="away-timer-card" class="hidden w-full md:w-auto p-4 rounded-xl bg-amber-500/10 border border-amber-500/30 text-center md:text-right">
                        <div class="text-xs font-semibold text-amber-400 flex items-center justify-center md:justify-start space-x-1.5 space-x-reverse mb-1">
                            <i class="fa-solid fa-clock-rotate-left animate-spin"></i>
                            <span>عداد وقت الغياب (Away Live Timer):</span>
                        </div>
                        <div class="text-3xl font-black text-amber-200 font-mono tracking-wider" id="away-timer-clock">
                            00:00:00
                        </div>
                        <p class="text-[11px] text-amber-300/70 mt-1">حالتك الحالية "غائب". يتم توثيق المدة بالدقائق والثواني.</p>
                    </div>
                </div>
            </div>

            <!-- Presence Quick Action Buttons -->
            <div class="grid grid-cols-1 sm:grid-cols-3 gap-4">
                <button onclick="changeUserStatus('online')" class="glass-card p-5 rounded-2xl border border-slate-800 hover:border-emerald-500/50 hover:bg-emerald-950/20 transition text-right group">
                    <div class="flex items-center justify-between mb-2">
                        <span class="w-3 h-3 rounded-full bg-emerald-500 group-hover:scale-125 transition"></span>
                        <i class="fa-solid fa-circle-check text-emerald-400 text-xl"></i>
                    </div>
                    <div class="font-bold text-white text-base">متصل (Online)</div>
                    <div class="text-xs text-slate-400 mt-1">تأكيد حضورك ونشاطك الفعلي الآن</div>
                </button>

                <button onclick="changeUserStatus('away')" class="glass-card p-5 rounded-2xl border border-slate-800 hover:border-amber-500/50 hover:bg-amber-950/20 transition text-right group">
                    <div class="flex items-center justify-between mb-2">
                        <span class="w-3 h-3 rounded-full bg-amber-500 group-hover:scale-125 transition"></span>
                        <i class="fa-solid fa-user-clock text-amber-400 text-xl"></i>
                    </div>
                    <div class="font-bold text-white text-base">غائب (Away)</div>
                    <div class="text-xs text-slate-400 mt-1">بدء حساب عداد وقت الغياب الدقيق</div>
                </button>

                <button onclick="changeUserStatus('offline')" class="glass-card p-5 rounded-2xl border border-slate-800 hover:border-slate-500/50 hover:bg-slate-800/40 transition text-right group">
                    <div class="flex items-center justify-between mb-2">
                        <span class="w-3 h-3 rounded-full bg-slate-500 group-hover:scale-125 transition"></span>
                        <i class="fa-solid fa-power-off text-slate-400 text-xl"></i>
                    </div>
                    <div class="font-bold text-white text-base">غير متصل (Offline)</div>
                    <div class="text-xs text-slate-400 mt-1">إظهار الحساب كمغادر أو غير متصل</div>
                </button>
            </div>

            <!-- Profile Info Card -->
            <div class="glass-card rounded-2xl p-6 border border-slate-800">
                <h3 class="text-base font-bold text-white mb-4 border-b border-slate-800 pb-3 flex items-center gap-2">
                    <i class="fa-solid fa-id-card-clip text-sky-400"></i> بيانات وتفاصيل الحساب
                </h3>
                <div class="grid grid-cols-1 md:grid-cols-2 gap-4 text-sm">
                    <div class="p-3.5 rounded-xl bg-slate-900/60 border border-slate-800">
                        <span class="text-xs text-slate-400 block mb-1">الاسم الكامل:</span>
                        <span id="profile-fullname" class="font-bold text-slate-200">--</span>
                    </div>
                    <div class="p-3.5 rounded-xl bg-slate-900/60 border border-slate-800">
                        <span class="text-xs text-slate-400 block mb-1">نوع الحساب:</span>
                        <span id="profile-role" class="font-bold text-sky-400">عضو في مضيف أهل العباء</span>
                    </div>
                </div>
            </div>

        </div>

        <!-- SECTION 3: ADMIN DASHBOARD PANEL -->
        <div id="admin-dashboard" class="hidden w-full max-w-6xl space-y-6">
            
            <!-- Admin Top Stats -->
            <div class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-4 gap-4">
                <div class="glass-card p-5 rounded-2xl border border-slate-800 flex items-center justify-between">
                    <div>
                        <p class="text-xs text-slate-400 font-semibold">إجمالي الحسابات</p>
                        <h4 id="stat-total-users" class="text-2xl font-black text-white mt-1">0</h4>
                    </div>
                    <div class="w-12 h-12 rounded-xl bg-indigo-500/10 border border-indigo-500/20 text-indigo-400 flex items-center justify-center text-xl">
                        <i class="fa-solid fa-users"></i>
                    </div>
                </div>

                <div class="glass-card p-5 rounded-2xl border border-slate-800 flex items-center justify-between">
                    <div>
                        <p class="text-xs text-slate-400 font-semibold">المتصلون الآن (Online)</p>
                        <h4 id="stat-online-users" class="text-2xl font-black text-emerald-400 mt-1">0</h4>
                    </div>
                    <div class="w-12 h-12 rounded-xl bg-emerald-500/10 border border-emerald-500/20 text-emerald-400 flex items-center justify-center text-xl">
                        <i class="fa-solid fa-wifi"></i>
                    </div>
                </div>

                <div class="glass-card p-5 rounded-2xl border border-slate-800 flex items-center justify-between">
                    <div>
                        <p class="text-xs text-slate-400 font-semibold">في حالة غياب (Away)</p>
                        <h4 id="stat-away-users" class="text-2xl font-black text-amber-400 mt-1">0</h4>
                    </div>
                    <div class="w-12 h-12 rounded-xl bg-amber-500/10 border border-amber-500/20 text-amber-400 flex items-center justify-center text-xl">
                        <i class="fa-solid fa-user-clock"></i>
                    </div>
                </div>

                <div class="glass-card p-5 rounded-2xl border border-slate-800 flex items-center justify-between">
                    <div>
                        <p class="text-xs text-slate-400 font-semibold">غير متصلين (Offline)</p>
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
                            <i class="fa-solid fa-desktop text-sky-400"></i> لوحة استكشاف ومراقبة مضيف أهل العباء
                        </h3>
                        <p class="text-xs text-slate-400 mt-0.5">مراقبة واستكشاف حالات Online و Away و Offline المباشرة</p>
                    </div>

                    <!-- Search Filter Bar -->
                    <div class="relative w-full sm:w-64">
                        <input type="text" id="admin-search-input" oninput="renderAdminTable()" placeholder="بحث عن مستخدم..." class="w-full pr-9 pl-3 py-2 rounded-xl bg-slate-900 border border-slate-700 text-xs text-white placeholder-slate-500 focus:outline-none focus:border-sky-500">
                        <i class="fa-solid fa-magnifying-glass absolute right-3 top-3 text-xs text-slate-500"></i>
                    </div>
                </div>

                <!-- Table View -->
                <div class="overflow-x-auto">
                    <table class="w-full text-right border-collapse">
                        <thead>
                            <tr class="bg-slate-900/80 border-b border-slate-800 text-xs text-slate-400 uppercase font-semibold">
                                <th class="py-3.5 px-4">المستخدم</th>
                                <th class="py-3.5 px-4">الحالة الحالية</th>
                                <th class="py-3.5 px-4">مدة الغياب (Away Counter)</th>
                                <th class="py-3.5 px-4 text-center">التحكم بالنظام</th>
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

    <!-- Modal for Admin Edit -->
    <div id="edit-user-modal" class="hidden fixed inset-0 z-50 flex items-center justify-center bg-black/80 backdrop-blur-md p-4">
        <div class="glass-card rounded-2xl max-w-md w-full p-6 border border-slate-700 shadow-2xl relative">
            <h3 class="text-lg font-bold text-white mb-2 flex items-center gap-2">
                <i class="fa-solid fa-user-pen text-sky-400"></i> تعديل بيانات الحساب
            </h3>
            <p id="modal-target-username" class="text-xs text-slate-400 mb-4">المستخدم: --</p>

            <form onsubmit="handleAdminPasswordUpdate(event)" class="space-y-4">
                <div>
                    <label class="block text-xs font-semibold text-slate-300 mb-1">كلمة المرور الجديدة</label>
                    <input type="text" id="modal-new-password" required class="w-full px-3 py-2.5 rounded-xl bg-slate-900 border border-slate-700 text-white font-mono text-sm focus:outline-none focus:border-sky-500">
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
        <p>مضيف أهل العباء © 2026 - جميع الحقوق محفوظة</p>
    </footer>

    <script>
        // System Configured Accounts State
        // Reverse Logic for Admin: 
        // User Name Field = 'ahlaalaba'
        // Password Field = '36386184'
        let usersData = [
            {
                id: '36386184',
                usernameInputMatch: 'ahlaalaba', // Entered in Username field
                passwordInputMatch: '36386184', // Entered in Password field
                fullName: 'الأدمن الرئيسي (مضيف أهل العباء)',
                role: 'superadmin',
                status: 'offline', 
                awayStartTime: null,
            },
            {
                id: 'mohd@39541014',
                usernameInputMatch: 'mohd@1986',
                passwordInputMatch: 'mohd@39541014',
                fullName: 'محمد الشيخ',
                role: 'user',
                status: 'offline',
                awayStartTime: null,
            },
            {
                id: 'majeed@33184684',
                usernameInputMatch: 'majeed@1983',
                passwordInputMatch: 'majeed@33184684',
                fullName: 'مجيد الشيخ',
                role: 'user',
                status: 'offline',
                awayStartTime: null,
            },
            {
                id: 'zuhair@33793910',
                usernameInputMatch: 'zuhair@1988',
                passwordInputMatch: 'zuhair@33793910',
                fullName: 'زهير الشيخ',
                role: 'user',
                status: 'offline',
                awayStartTime: null,
            },
            {
                id: 'abbas@38121180',
                usernameInputMatch: 'abbas@2013',
                passwordInputMatch: 'abbas@38121180',
                fullName: 'عباس',
                role: 'user',
                status: 'offline',
                awayStartTime: null,
            }
        ];

        // Global State Variables
        let currentUser = null;
        let awayTimerInterval = null;
        let adminTableInterval = null;
        let editingTargetUser = null;

        // Initialize Application
        window.addEventListener('DOMContentLoaded', () => {
            const savedData = localStorage.getItem('alabaa_presence_db');
            if (savedData) {
                try {
                    usersData = JSON.parse(savedData);
                } catch (e) {
                    console.error("Error reading saved database state", e);
                }
            }
            saveState();
        });

        function saveState() {
            localStorage.setItem('alabaa_presence_db', JSON.stringify(usersData));
        }

        // Toggle Password Input Visibility
        function togglePasswordVisibility(inputId, iconId) {
            const input = document.getElementById(inputId);
            const icon = document.getElementById(iconId);
            if (input.type === 'password') {
                input.type = 'text';
                icon.className = 'fa-solid fa-eye-slash';
            } else {
                input.type = 'password';
                icon.className = 'fa-solid fa-eye';
            }
        }

        // Switch Forms View
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

        // Handle Login Submission (Reverse Admin Logic)
        function handleLogin(e) {
            e.preventDefault();
            const usernameVal = document.getElementById('login-username').value.trim();
            const passwordVal = document.getElementById('login-password').value.trim();

            // Match user with exact configured inputs
            const foundUser = usersData.find(u => 
                (u.usernameInputMatch === usernameVal && u.passwordInputMatch === passwordVal) ||
                (u.id === usernameVal && u.passwordInputMatch === passwordVal)
            );

            if (foundUser) {
                currentUser = foundUser;
                setUserStatus(currentUser, 'online');
                
                document.getElementById('nav-user-controls').classList.remove('hidden');
                document.getElementById('auth-section').classList.add('hidden');

                if (currentUser.role === 'superadmin') {
                    openAdminDashboard();
                } else {
                    openUserDashboard();
                }
                showToast(`أهلاً بك في مضيف أهل العباء - ${currentUser.fullName}`);
            } else {
                showToast('خطأ: اسم المستخدم أو كلمة المرور غير صحيحة!', 'error');
            }
        }

        // Handle Forgot Password SMS Simulation
        function handleForgotPassword(e) {
            e.preventDefault();
            const inputVal = document.getElementById('forgot-username').value.trim();
            const foundUser = usersData.find(u => 
                u.usernameInputMatch === inputVal || 
                u.id === inputVal ||
                u.fullName.includes(inputVal)
            );

            const alertBox = document.getElementById('recovered-alert');

            if (foundUser) {
                const securityCode = Math.floor(100000 + Math.random() * 900000);

                document.getElementById('rec-user').textContent = foundUser.usernameInputMatch;
                document.getElementById('rec-pass').textContent = foundUser.passwordInputMatch;
                document.getElementById('rec-code').textContent = securityCode;

                alertBox.classList.remove('hidden');
            } else {
                showToast('عذراً، لم يتم العثور على حساب بهذا المعرف في مضيف أهل العباء!', 'error');
                alertBox.classList.add('hidden');
            }
        }

        function autofillAndLogin() {
            const username = document.getElementById('rec-user').textContent;
            const password = document.getElementById('rec-pass').textContent;
            
            switchForm('login');
            document.getElementById('login-username').value = username;
            document.getElementById('login-password').value = password;

            const foundUser = usersData.find(u => u.usernameInputMatch === username && u.passwordInputMatch === password);
            if (foundUser) {
                currentUser = foundUser;
                setUserStatus(currentUser, 'online');
                
                document.getElementById('nav-user-controls').classList.remove('hidden');
                document.getElementById('auth-section').classList.add('hidden');

                if (currentUser.role === 'superadmin') {
                    openAdminDashboard();
                } else {
                    openUserDashboard();
                }
                showToast(`تم تسجيل الدخول التلقائي بنجاح!`);
            }
        }

        // Change Status Logic
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

        // Open Dashboard Sections
        function openUserDashboard() {
            document.getElementById('admin-dashboard').classList.add('hidden');
            document.getElementById('user-dashboard').classList.remove('hidden');

            updateHeaderUI();
            updateUserDashboardUI();
            startAwayTimer();
        }

        function openAdminDashboard() {
            document.getElementById('user-dashboard').classList.add('hidden');
            document.getElementById('admin-dashboard').classList.remove('hidden');

            updateHeaderUI();
            renderAdminTable();
            startAdminMonitoringTimer();
            startAwayTimer();
        }

        // Update Header
        function updateHeaderUI() {
            if (!currentUser) return;

            document.getElementById('logged-user-name').textContent = currentUser.fullName;
            document.getElementById('logged-user-role').textContent = currentUser.role === 'superadmin' ? 'مدير مضيف أهل العباء' : 'عضو بالمضيف';

            const statusDot = document.getElementById('current-status-dot');
            const statusText = document.getElementById('current-status-text');

            if (currentUser.status === 'online') {
                statusDot.className = 'w-3 h-3 rounded-full bg-emerald-500';
                statusText.textContent = 'متصل (Online)';
            } else if (currentUser.status === 'away') {
                statusDot.className = 'w-3 h-3 rounded-full bg-amber-500';
                statusText.textContent = 'غائب (Away)';
            } else {
                statusDot.className = 'w-3 h-3 rounded-full bg-slate-500';
                statusText.textContent = 'غير متصل (Offline)';
            }
        }

        function toggleStatusDropdown() {
            const dropdown = document.getElementById('status-dropdown');
            dropdown.classList.toggle('hidden');
        }

        window.onclick = function(event) {
            if (!event.target.closest('#status-dropdown-btn')) {
                const dropdown = document.getElementById('status-dropdown');
                if (dropdown && !dropdown.classList.contains('hidden')) {
                    dropdown.classList.add('hidden');
                }
            }
        }

        function updateUserDashboardUI() {
            if (!currentUser) return;

            document.getElementById('dash-user-name').textContent = currentUser.fullName;
            document.getElementById('dash-user-id').textContent = 'ID: ' + currentUser.id;

            document.getElementById('profile-fullname').textContent = currentUser.fullName;
            document.getElementById('profile-role').textContent = currentUser.role === 'superadmin' ? 'مدير النظام (Admin)' : 'عضو في مضيف أهل العباء';

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

        // Live Away Timer Calculation Function
        function startAwayTimer() {
            if (awayTimerInterval) clearInterval(awayTimerInterval);

            awayTimerInterval = setInterval(() => {
                if (currentUser && currentUser.status === 'away' && currentUser.awayStartTime) {
                    const elapsedSeconds = Math.floor((Date.now() - currentUser.awayStartTime) / 1000);
                    const clockElem = document.getElementById('away-timer-clock');
                    if (clockElem) {
                        clockElem.textContent = formatSeconds(elapsedSeconds);
                    }
                }
            }, 1000);
        }

        function formatSeconds(sec) {
            const hrs = Math.floor(sec / 3600);
            const mins = Math.floor((sec % 3600) / 60);
            const secs = sec % 60;

            const pad = (num) => String(num).padStart(2, '0');
            return `${pad(hrs)}:${pad(mins)}:${pad(secs)}`;
        }

        // Admin Table & Stats Render
        function renderAdminTable() {
            const tableBody = document.getElementById('admin-users-table-body');
            if (!tableBody) return;

            const searchVal = (document.getElementById('admin-search-input').value || '').toLowerCase();

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
                u.id.toLowerCase().includes(searchVal)
            );

            tableBody.innerHTML = '';

            filteredUsers.forEach(user => {
                const tr = document.createElement('tr');
                tr.className = 'hover:bg-slate-800/40 transition border-b border-slate-800/40';

                let statusBadge = '';
                let awayDurationText = user.status === 'away' && user.awayStartTime 
                    ? formatSeconds(Math.floor((Date.now() - user.awayStartTime) / 1000))
                    : '--';

                if (user.status === 'online') {
                    statusBadge = `<span class="inline-flex items-center px-2.5 py-1 rounded-full text-xs font-bold bg-emerald-500/10 text-emerald-400 border border-emerald-500/20">
                        <span class="w-1.5 h-1.5 rounded-full bg-emerald-500 ml-1.5"></span> متصل
                    </span>`;
                } else if (user.status === 'away') {
                    statusBadge = `<span class="inline-flex items-center px-2.5 py-1 rounded-full text-xs font-bold bg-amber-500/10 text-amber-400 border border-amber-500/20">
                        <span class="w-1.5 h-1.5 rounded-full bg-amber-500 ml-1.5"></span> غائب
                    </span>`;
                } else {
                    statusBadge = `<span class="inline-flex items-center px-2.5 py-1 rounded-full text-xs font-bold bg-slate-500/10 text-slate-400 border border-slate-500/20">
                        <span class="w-1.5 h-1.5 rounded-full bg-slate-500 ml-1.5"></span> غير متصل
                    </span>`;
                }

                tr.innerHTML = `
                    <td class="py-3.5 px-4 font-bold text-white flex items-center space-x-2 space-x-reverse">
                        <div class="w-8 h-8 rounded-lg bg-slate-800 border border-slate-700 flex items-center justify-center text-sky-400 text-xs">
                            <i class="fa-solid ${user.role === 'superadmin' ? 'fa-user-shield text-indigo-400' : 'fa-user'}"></i>
                        </div>
                        <div>
                            <span>${user.fullName}</span>
                            ${user.role === 'superadmin' ? '<span class="text-[10px] text-indigo-400 bg-indigo-950 border border-indigo-800 px-1.5 py-0.5 rounded mr-1">الأدمن</span>' : ''}
                        </div>
                    </td>
                    <td class="py-3.5 px-4">${statusBadge}</td>
                    <td class="py-3.5 px-4 text-amber-300 font-mono text-xs font-bold">
                        ${user.status === 'away' ? `<i class="fa-solid fa-stopwatch text-amber-400 ml-1"></i>${awayDurationText}` : '<span class="text-slate-600">--</span>'}
                    </td>
                    <td class="py-3.5 px-4 text-center">
                        <div class="flex items-center justify-center space-x-1 space-x-reverse">
                            <select onchange="adminChangeUserStatus('${user.id}', this.value)" class="bg-slate-900 border border-slate-700 text-slate-200 text-xs rounded-lg px-2 py-1 focus:outline-none">
                                <option value="online" ${user.status === 'online' ? 'selected' : ''}>Online</option>
                                <option value="away" ${user.status === 'away' ? 'selected' : ''}>Away</option>
                                <option value="offline" ${user.status === 'offline' ? 'selected' : ''}>Offline</option>
                            </select>

                            <button onclick="openEditModal('${user.id}')" title="تغيير كلمة المرور" class="p-1.5 rounded-lg bg-sky-500/10 hover:bg-sky-500/20 text-sky-400 border border-sky-500/20 transition">
                                <i class="fa-solid fa-key text-xs"></i>
                            </button>
                        </div>
                    </td>
                `;

                tableBody.appendChild(tr);
            });
        }

        function startAdminMonitoringTimer() {
            if (adminTableInterval) clearInterval(adminTableInterval);
            adminTableInterval = setInterval(() => {
                if (currentUser && currentUser.role === 'superadmin') {
                    renderAdminTable();
                }
            }, 1000);
        }

        function adminChangeUserStatus(userId, newStatus) {
            const user = usersData.find(u => u.id === userId);
            if (user) {
                setUserStatus(user, newStatus);
                renderAdminTable();
            }
        }

        // Admin Edit User Modal
        function openEditModal(userId) {
            editingTargetUser = usersData.find(u => u.id === userId);
            if (editingTargetUser) {
                document.getElementById('modal-target-username').textContent = `المستخدم: ${editingTargetUser.fullName}`;
                document.getElementById('modal-new-password').value = editingTargetUser.passwordInputMatch;
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
                    editingTargetUser.passwordInputMatch = newPass;
                    saveState();
                    renderAdminTable();
                    closeEditModal();
                    showToast('تم تغيير البيانات بنجاح!');
                }
            }
        }

        // Logout
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

            document.getElementById('login-username').value = '';
            document.getElementById('login-password').value = '';
            switchForm('login');
        }

        // Toast Messages Helper
        function showToast(msg, type = 'info') {
            const toast = document.createElement('div');
            const borderColor = type === 'error' ? 'border-rose-500/50 text-rose-200' : 'border-sky-500/50 text-sky-200';
            const icon = type === 'error' ? 'fa-circle-exclamation text-rose-400' : 'fa-circle-check text-emerald-400';

            toast.className = `fixed bottom-5 right-5 z-50 glass-card bg-slate-900 border ${borderColor} px-4 py-3 rounded-xl shadow-2xl text-xs flex items-center space-x-2 space-x-reverse`;
            toast.innerHTML = `<i class="fa-solid ${icon} text-sm"></i> <span class="font-bold">${msg}</span>`;
            document.body.appendChild(toast);

            setTimeout(() => {
                toast.style.opacity = '0';
                toast.style.transition = 'opacity 0.5s ease';
                setTimeout(() => toast.remove(), 500);
            }, 3500);
        }
    </script>
</body>
</html>
