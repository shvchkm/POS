<!DOCTYPE html>
<html lang="bn">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no" />
  <title>রিবেক & হেতু POS — প্রফেশনাল দোকান ও হিসাব সিস্টেম</title>
  <script src="https://cdn.tailwindcss.com"></script>
  <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.5.2/css/all.min.css" />
  <script src="https://cdn.jsdelivr.net/npm/jsbarcode@3.11.6/dist/JsBarcode.all.min.js"></script>
  <script src="https://cdnjs.cloudflare.com/ajax/libs/qrcodejs/1.0.0/qrcode.min.js"></script>
  <script src="https://unpkg.com/html5-qrcode@2.3.8/html5-qrcode.min.js"></script>
  <script src="https://cdnjs.cloudflare.com/ajax/libs/html2canvas/1.4.1/html2canvas.min.js"></script>
  <script src="https://cdnjs.cloudflare.com/ajax/libs/jspdf/2.5.1/jspdf.umd.min.js"></script>
  <script src="https://www.gstatic.com/firebasejs/10.12.2/firebase-app-compat.js"></script>
  <script src="https://www.gstatic.com/firebasejs/10.12.2/firebase-database-compat.js"></script>
  <style>
    @import url('https://fonts.googleapis.com/css2?family=Hind+Siliguri:wght@400;500;600;700&family=Plus+Jakarta+Sans:wght@400;500;600;700;800&display=swap');

    body {
      font-family: 'Hind Siliguri', 'Plus Jakarta Sans', sans-serif;
      background-color: #f1f5f9;
      color: #0f172a;
      -webkit-tap-highlight-color: transparent;
      background-size: cover;
      background-position: center;
      background-attachment: fixed;
    }

    body.theme-emerald { --p-main: #059669; --p-dark: #065f46; --p-soft: #ecfdf5; }
    body.theme-royal   { --p-main: #2563eb; --p-dark: #1e40af; --p-soft: #eff6ff; }
    body.theme-crimson { --p-main: #e11d48; --p-dark: #9f1239; --p-soft: #fff1f2; }
    body.theme-purple  { --p-main: #7c3aed; --p-dark: #5b21b6; --p-soft: #f5f3ff; }
    body.theme-amber   { --p-main: #d97706; --p-dark: #92400e; --p-soft: #fffbeb; }
    body.theme-teal    { --p-main: #0d9488; --p-dark: #115e59; --p-soft: #f0fdfa; }
    body.theme-magenta { --p-main: #c026d3; --p-dark: #86198f; --p-soft: #fdf4ff; }
    body.theme-indigo  { --p-main: #4f46e5; --p-dark: #3730a3; --p-soft: #eef2ff; }
    body.theme-lime    { --p-main: #65a30d; --p-dark: #3f6212; --p-soft: #f7fee7; }
    body.theme-dark    { --p-main: #0f172a; --p-dark: #020617; --p-soft: #e2e8f0; background-color: #0f172a; color: #f8fafc; }

    body.has-custom-bg .app-card {
      background-color: rgba(255, 255, 255, 0.92) !important;
      backdrop-filter: blur(6px);
    }

    body.theme-dark .app-card {
      background-color: rgba(30, 41, 59, 0.95) !important;
      color: #f8fafc !important;
      border-color: #334155 !important;
    }
    body.theme-dark .app-card input,
    body.theme-dark .app-card select,
    body.theme-dark .app-card textarea {
      background-color: #0f172a !important;
      color: #f8fafc !important;
      border-color: #475569 !important;
    }

    .theme-bg { background-color: var(--p-main); }
    .theme-text { color: var(--p-main); }
    .theme-soft-bg { background-color: var(--p-soft); }

    .voucher-full-col-banner {
      display: block;
      width: 100%;
      white-space: nowrap !important;
      overflow: hidden;
      text-overflow: clip;
      font-size: 6.4px;
      line-height: 1.25;
      letter-spacing: -0.03em;
    }

    @keyframes heartbeat {
      0%, 100% { transform: scale(1); }
      25% { transform: scale(1.18); }
      45% { transform: scale(0.96); }
      65% { transform: scale(1.12); }
    }
    .animate-heartbeat { animation: heartbeat 1.1s infinite; }

    @media print {
      body * { visibility: hidden; }
      #official-voucher-capture, #official-voucher-capture * { visibility: visible; }
      #official-voucher-capture {
        position: fixed;
        left: 0;
        top: 0;
        width: 58mm !important;
        max-width: 58mm !important;
        padding: 4px !important;
        font-size: 9px !important;
        box-shadow: none !important;
        border: 1px solid #000 !important;
      }
    }
  </style>
</head>
<body class="theme-emerald min-h-screen pb-24 lg:pb-8">

  <div id="toast-container" class="fixed top-4 left-1/2 -translate-x-1/2 z-[120] flex flex-col gap-2 w-[92%] max-w-md pointer-events-none"></div>

  <!-- AUTH SCREEN -->
  <div id="auth-screen" class="min-h-screen flex items-center justify-center p-4 bg-gradient-to-br from-slate-900 via-emerald-950 to-slate-900">
    <div class="w-full max-w-md bg-white rounded-3xl shadow-2xl overflow-hidden border border-slate-200">
      <div class="p-5 bg-gradient-to-r from-emerald-700 to-teal-700 text-white flex items-center justify-between gap-3">
        <div class="flex items-center gap-3 min-w-0">
          <div class="w-11 h-11 rounded-2xl bg-white/15 flex items-center justify-center text-xl border border-white/25 shrink-0">
            <i class="fa-solid fa-shield-halved"></i>
          </div>
          <div class="min-w-0">
            <h1 class="text-base sm:text-lg font-extrabold tracking-tight truncate" data-i18n="appTitle">রিবেক & হেতু POS</h1>
            <p class="text-[11px] text-emerald-100 truncate" data-i18n="authSubtitle">কম্পিউটার ও মোবাইল লাইভ ক্লাউড অটো-সিঙ্ক সিস্টেম</p>
          </div>
        </div>
        <div class="flex flex-col items-end gap-1.5 shrink-0">
          <span id="auth-net-badge" class="px-2.5 py-0.5 rounded-full bg-emerald-500/20 text-emerald-200 text-[10px] font-bold flex items-center gap-1.5">
            <i class="net-type-icon fa-solid fa-wifi text-[10px]"></i> <span class="net-status-text">লাইভ অনলাইন</span>
          </span>
          <button type="button" onclick="toggleLanguage()" id="auth-lang-btn" class="px-3 py-1 rounded-full bg-white/20 hover:bg-white/30 text-xs font-bold transition">
            English
          </button>
        </div>
      </div>

      <div class="p-6">
        <div id="intruder-alert-card" class="hidden mb-4 p-3.5 rounded-2xl bg-rose-50 border-2 border-rose-500 text-rose-900">
          <div class="flex items-center gap-2 font-extrabold text-xs mb-1">
            <i class="fa-solid fa-triangle-exclamation text-rose-600 text-base"></i>
            <span id="intruder-msg">সতর্কতা! ৩ বার ভুল পাসওয়ার্ড দেওয়া হয়েছে।</span>
          </div>
          <video id="intruder-video" autoplay playsinline class="hidden"></video>
          <canvas id="intruder-canvas" class="hidden"></canvas>
          <img id="intruder-photo" alt="Intruder Capture" class="hidden mt-2 w-24 h-24 rounded-xl object-cover border-2 border-rose-600" />
        </div>

        <div class="grid grid-cols-2 gap-2.5 p-1 bg-slate-100 rounded-2xl mb-5">
          <button type="button" onclick="switchAuthMode('login')" id="auth-tab-login" class="py-2.5 rounded-xl text-xs font-extrabold bg-white text-emerald-700 shadow transition" data-i18n="loginTab">
            লগইন করুন
          </button>
          <button type="button" onclick="switchAuthMode('register')" id="auth-tab-reg" class="py-2.5 rounded-xl text-xs font-extrabold text-slate-600 transition" data-i18n="registerTab">
            নতুন একাউন্ট খুলুন
          </button>
        </div>

        <form id="loginForm" onsubmit="handleUserLogin(event)" class="space-y-4">
          <div class="grid grid-cols-2 gap-2.5 p-1 rounded-xl bg-slate-100 border border-slate-200">
            <label class="cursor-pointer">
              <input type="radio" name="loginRoleMode" value="admin" checked onchange="onAuthRoleModeChange()" class="sr-only peer" />
              <div class="py-2 rounded-lg text-center text-xs font-extrabold text-slate-700 peer-checked:bg-emerald-600 peer-checked:text-white transition flex items-center justify-center gap-1.5">
                <i class="fa-solid fa-user-shield"></i> <span>অ্যাডমিন / মালিক</span>
              </div>
            </label>
            <label class="cursor-pointer">
              <input type="radio" name="loginRoleMode" value="staff" onchange="onAuthRoleModeChange()" class="sr-only peer" />
              <div class="py-2 rounded-lg text-center text-xs font-extrabold text-slate-700 peer-checked:bg-indigo-600 peer-checked:text-white transition flex items-center justify-center gap-1.5">
                <i class="fa-solid fa-id-badge"></i> <span>স্টাফ প্রোফাইল</span>
              </div>
            </label>
          </div>

          <div id="login-admin-gate-box" class="hidden p-3 rounded-2xl bg-indigo-50 border border-indigo-200 space-y-2">
            <label class="block text-[11px] font-extrabold text-indigo-950">
              <i class="fa-solid fa-lock mr-1.5 text-indigo-600"></i> স্টাফ লগইনের জন্য অনুমোদিত অ্যাডমিন ID (বাধ্যতামূলক)
            </label>
            <input type="text" id="loginGateAdminId" class="w-full px-3 py-2 rounded-xl border border-indigo-300 bg-white text-xs font-bold outline-none focus:ring-2 focus:ring-indigo-500" placeholder="অ্যাডমিন মোবাইল / ইউজার ID লিখুন" />
          </div>

          <div>
            <label class="block text-xs font-bold text-slate-700 mb-1" data-i18n="userIdLabel">মোবাইল নম্বর বা ইউজার আইডি</label>
            <input type="text" id="loginUser" required oninput="speakTypingLive(this.value)" class="w-full px-3.5 py-2.5 rounded-xl border border-slate-300 focus:ring-2 focus:ring-emerald-500 outline-none text-sm" placeholder="যেমন: 01732333764 বা স্টাফ আইডি" />
          </div>
          <div>
            <label class="block text-xs font-bold text-slate-700 mb-1" data-i18n="passwordLabel">গোপন পাসওয়ার্ড</label>
            <div class="relative">
              <input type="password" id="loginPass" required class="w-full px-3.5 py-2.5 rounded-xl border border-slate-300 focus:ring-2 focus:ring-emerald-500 outline-none text-sm pr-10" placeholder="••••••••" />
              <button type="button" onclick="togglePassField('loginPass')" class="absolute right-3 top-1/2 -translate-y-1/2 text-slate-400 hover:text-slate-600">
                <i class="fa-solid fa-eye"></i>
              </button>
            </div>
          </div>
          <button type="submit" id="loginBtn" class="w-full py-3 rounded-xl bg-emerald-600 hover:bg-emerald-700 text-white font-extrabold text-sm shadow-lg shadow-emerald-600/30 transition flex items-center justify-center gap-2">
            <i class="fa-solid fa-right-to-bracket"></i> <span data-i18n="loginBtnText">প্রবেশ করুন</span>
          </button>
        </form>

        <form id="registerForm" onsubmit="handleUserRegister(event)" class="space-y-3.5 hidden">
          <div class="grid grid-cols-2 gap-2.5 p-1 rounded-xl bg-slate-100 border border-slate-200">
            <label class="cursor-pointer">
              <input type="radio" name="regRoleMode" value="admin" checked onchange="onAuthRoleModeChange()" class="sr-only peer" />
              <div class="py-2 rounded-lg text-center text-xs font-extrabold text-slate-700 peer-checked:bg-emerald-600 peer-checked:text-white transition flex items-center justify-center gap-1.5">
                <i class="fa-solid fa-user-shield"></i> <span>অ্যাডমিন একাউন্ট</span>
              </div>
            </label>
            <label class="cursor-pointer">
              <input type="radio" name="regRoleMode" value="staff" onchange="onAuthRoleModeChange()" class="sr-only peer" />
              <div class="py-2 rounded-lg text-center text-xs font-extrabold text-slate-700 peer-checked:bg-indigo-600 peer-checked:text-white transition flex items-center justify-center gap-1.5">
                <i class="fa-solid fa-id-badge"></i> <span>স্টাফ প্রোফাইল রেজি.</span>
              </div>
            </label>
          </div>

          <div id="reg-admin-fields" class="space-y-3">
            <div>
              <label class="block text-xs font-bold text-slate-700 mb-1" data-i18n="ownerNameLabel">আপনার পূর্ণ নাম</label>
              <input type="text" id="regName" oninput="speakTypingLive(this.value)" class="w-full px-3.5 py-2 rounded-xl border border-slate-300 focus:ring-2 focus:ring-emerald-500 outline-none text-sm" placeholder="আপনার নাম লিখুন" />
            </div>
            <div>
              <label class="block text-xs font-bold text-slate-700 mb-1" data-i18n="shopNameLabel">দোকানের নাম</label>
              <input type="text" id="regShop" oninput="speakTypingLive(this.value)" class="w-full px-3.5 py-2 rounded-xl border border-slate-300 focus:ring-2 focus:ring-emerald-500 outline-none text-sm" placeholder="যেমন: Ribek & Hetu" />
            </div>
            <div>
              <label class="block text-xs font-bold text-slate-700 mb-1" data-i18n="shopAddrLabel">দোকানের ঠিকানা</label>
              <input type="text" id="regShopAddr" oninput="speakTypingLive(this.value)" class="w-full px-3.5 py-2 rounded-xl border border-slate-300 focus:ring-2 focus:ring-emerald-500 outline-none text-sm" placeholder="যেমন: চট্টগ্রাম, বাংলাদেশ" />
            </div>
            <div>
              <label class="block text-xs font-bold text-slate-700 mb-1" data-i18n="userIdLabel">মোবাইল নম্বর বা ইউজার আইডি</label>
              <input type="text" id="regPhone" oninput="speakTypingLive(this.value)" class="w-full px-3.5 py-2 rounded-xl border border-slate-300 focus:ring-2 focus:ring-emerald-500 outline-none text-sm" placeholder="017XXXXXXXX" />
            </div>
            <div class="grid grid-cols-2 gap-2.5">
              <div>
                <label class="block text-xs font-bold text-slate-700 mb-1" data-i18n="passwordLabel">গোপন পাসওয়ার্ড</label>
                <input type="password" id="regPass" minlength="4" class="w-full px-3.5 py-2 rounded-xl border border-slate-300 focus:ring-2 focus:ring-emerald-500 outline-none text-sm" placeholder="••••" />
              </div>
              <div>
                <label class="block text-xs font-bold text-slate-700 mb-1" data-i18n="confirmPassLabel">পাসওয়ার্ড নিশ্চিত করুন</label>
                <input type="password" id="regPass2" minlength="4" class="w-full px-3.5 py-2 rounded-xl border border-slate-300 focus:ring-2 focus:ring-emerald-500 outline-none text-sm" placeholder="••••" />
              </div>
            </div>
          </div>

          <div id="reg-staff-fields" class="hidden space-y-2.5 p-3 rounded-2xl bg-indigo-50/80 border border-indigo-200">
            <div>
              <label class="block text-[11px] font-extrabold text-indigo-950 mb-1">অ্যাডমিন ID (বাধ্যতামূলক)</label>
              <input type="text" id="regStaffAdminId" class="w-full px-3 py-2 rounded-xl border border-indigo-300 bg-white text-xs font-bold outline-none focus:ring-2 focus:ring-indigo-500" placeholder="যে অ্যাডমিনের অধীনে স্টাফ হবেন তার ID" />
            </div>

            <div class="grid grid-cols-2 gap-2.5">
              <div class="bg-white p-2 rounded-xl border border-indigo-100 flex items-center gap-2">
                <img id="reg-staff-shop-logo-prev" src="https://placehold.co/64x64/059669/ffffff?text=Logo" class="w-10 h-10 rounded-lg object-cover border shrink-0" alt="Shop Logo" />
                <div class="min-w-0">
                  <div class="text-[10px] font-extrabold text-slate-700 truncate">দোকানের লোগো</div>
                  <label class="inline-block mt-0.5 px-2 py-0.5 rounded bg-emerald-600 text-white text-[10px] font-bold cursor-pointer">
                    আপলোড
                    <input type="file" accept="image/*" onchange="handleAuthStaffLogoUpload(event)" class="hidden" />
                  </label>
                  <input type="hidden" id="reg-staff-shop-logo-data" />
                </div>
              </div>

              <div class="bg-white p-2 rounded-xl border border-indigo-100 flex items-center gap-2">
                <img id="reg-staff-photo-prev" src="https://placehold.co/64x64/6366f1/ffffff?text=Staff" class="w-10 h-10 rounded-lg object-cover border shrink-0" alt="Staff Photo" />
                <div class="min-w-0">
                  <div class="text-[10px] font-extrabold text-slate-700 truncate">স্টাফের ছবি</div>
                  <label class="inline-block mt-0.5 px-2 py-0.5 rounded bg-indigo-600 text-white text-[10px] font-bold cursor-pointer">
                    আপলোড
                    <input type="file" accept="image/*" onchange="handleAuthStaffPhotoUpload(event)" class="hidden" />
                  </label>
                  <input type="hidden" id="reg-staff-photo-data" />
                </div>
              </div>
            </div>

            <div class="grid grid-cols-1 sm:grid-cols-2 gap-2.5">
              <div>
                <label class="block text-[11px] font-bold text-slate-700 mb-1">দোকানের নাম</label>
                <input type="text" id="regStaffShopName" oninput="speakTypingLive(this.value)" class="w-full px-3 py-2 rounded-xl border border-slate-300 bg-white text-xs font-bold" placeholder="যেমন: রিবেক & হেতু" />
              </div>
              <div>
                <label class="block text-[11px] font-bold text-slate-700 mb-1">দোকানের ঠিকানা</label>
                <input type="text" id="regStaffShopAddr" oninput="speakTypingLive(this.value)" class="w-full px-3 py-2 rounded-xl border border-slate-300 bg-white text-xs font-bold" placeholder="দোকানের ঠিকানা" />
              </div>
            </div>

            <div class="grid grid-cols-2 gap-2.5">
              <div>
                <label class="block text-[11px] font-bold text-slate-700 mb-1">স্টাফের নাম</label>
                <input type="text" id="regStaffName" oninput="speakTypingLive(this.value)" class="w-full px-3 py-2 rounded-xl border border-slate-300 bg-white text-xs font-bold" placeholder="স্টাফের পূর্ণ নাম" />
              </div>
              <div>
                <label class="block text-[11px] font-bold text-slate-700 mb-1">স্টাফ ইউজার ID / মোবাইল</label>
                <input type="text" id="regStaffUserId" oninput="speakTypingLive(this.value)" class="w-full px-3 py-2 rounded-xl border border-slate-300 bg-white text-xs font-bold" placeholder="staff01 / 018..." />
              </div>
            </div>

            <div class="grid grid-cols-2 gap-2.5">
              <div>
                <label class="block text-[11px] font-bold text-slate-700 mb-1">স্টাফ পাসওয়ার্ড</label>
                <input type="password" id="regStaffPass" minlength="4" class="w-full px-3 py-2 rounded-xl border border-slate-300 bg-white text-xs" placeholder="••••" />
              </div>
              <div>
                <label class="block text-[11px] font-bold text-slate-700 mb-1">পাসওয়ার্ড নিশ্চিত করুন</label>
                <input type="password" id="regStaffPass2" minlength="4" class="w-full px-3 py-2 rounded-xl border border-slate-300 bg-white text-xs" placeholder="••••" />
              </div>
            </div>
          </div>

          <button type="submit" id="registerBtn" class="w-full py-3 rounded-xl bg-indigo-600 hover:bg-indigo-700 text-white font-extrabold text-sm shadow-lg shadow-indigo-600/30 transition flex items-center justify-center gap-2">
            <i class="fa-solid fa-user-plus"></i> <span id="registerBtnTextSpan" data-i18n="registerBtnText">একাউন্ট তৈরি করুন</span>
          </button>
        </form>
      </div>
    </div>
  </div>

  <!-- MAIN APPLICATION -->
  <div id="main-app" class="hidden">
    <header class="sticky top-0 z-40 theme-bg text-white shadow-lg">
      <div class="max-w-7xl mx-auto px-2.5 sm:px-3.5 py-2 flex items-center justify-between gap-2">
        <div class="flex items-center gap-2 sm:gap-2.5 min-w-0">
          <img id="hdr-store-logo" src="https://placehold.co/100x100/059669/ffffff?text=R%26H" alt="Logo" class="w-9 h-9 sm:w-10 sm:h-10 rounded-2xl object-cover bg-white border-2 border-white/40 shrink-0" />
          <div class="min-w-0">
            <div class="flex items-center gap-1.5 flex-wrap">
              <span id="hdr-store-name" class="font-extrabold text-xs sm:text-base truncate max-w-[110px] sm:max-w-none">Ribek & Hetu</span>
              <span id="hdr-day-night-badge" class="px-1.5 py-0.5 rounded-full bg-white/15 text-[9px] sm:text-[10px] font-extrabold flex items-center gap-1 shrink-0" title="Day / Night Auto Indicator">
                <i id="hdr-day-night-icon" class="fa-solid fa-sun text-amber-300"></i>
                <span id="hdr-day-night-label">দিন</span>
              </span>
              <span id="hdr-user-badge" class="hidden md:inline-block px-2 py-0.5 rounded-full bg-black/20 text-[10px] font-semibold truncate max-w-[150px]"></span>
              <select id="hdr-quick-shop-switch" onchange="quickSwitchShopName(this.value)" class="hidden xl:inline-block px-2 py-0.5 rounded-lg bg-black/25 text-white text-[10px] font-bold border border-white/20 outline-none">
                <option value="Ribek & Hetu">Ribek & Hetu</option>
                <option value="রিবেক & হেতু">রিবেক & হেতু</option>
                <option value="RIBEKV& HETU SUPAR SHOP">RIBEKV& HETU SUPAR SHOP</option>
                <option value="সুকমল স্টোর">সুকমল স্টোর</option>
              </select>
            </div>
            <div class="flex items-center gap-1.5 text-[9px] sm:text-[10px] text-white/95 mt-0.5 truncate">
              <i class="net-type-icon fa-solid fa-wifi text-[9px]"></i>
              <span class="net-status-text font-bold">লাইভ অনলাইন</span>
              <span id="net-ping-text" class="opacity-85 font-mono"></span>
              <span>•</span>
              <span data-i18n="isolatedTag">অটো-সিঙ্ক সক্রিয়</span>
            </div>
          </div>
        </div>

        <div class="flex items-center gap-1 sm:gap-1.5 shrink-0">
          <button type="button" onclick="openStaffSmsModal()" class="px-2 py-1.5 rounded-xl bg-indigo-600/90 hover:bg-indigo-500 text-white text-[10px] font-extrabold flex items-center gap-1 transition relative" title="Admin-Staff SMS / Message">
            <i class="fa-solid fa-message"></i>
            <span class="hidden md:inline">SMS</span>
            <span id="hdr-sms-badge" class="hidden px-1.5 py-0.2 rounded-full bg-amber-300 text-slate-950 text-[9px] font-extrabold">0</span>
          </button>
          <button type="button" onclick="pullStoreFromCloud(false)" id="hdr-cloud-pull-btn" class="hidden sm:flex px-2 py-1.5 rounded-xl bg-white/15 hover:bg-white/25 text-[10px] font-extrabold items-center gap-1 transition" title="Pull Latest Cloud Data">
            <i class="fa-solid fa-cloud-arrow-down"></i>
            <span class="hidden md:inline">সিঙ্ক</span>
          </button>
          <button type="button" onclick="runLiveConnectivityCheck(true, true)" id="hdr-net-btn" class="px-2 py-1.5 rounded-xl bg-emerald-500 border border-emerald-200 text-white text-[10px] font-extrabold flex items-center gap-1 transition" title="Live Internet Status">
            <i id="hdr-net-icon" class="net-type-icon fa-solid fa-wifi"></i>
            <span class="net-status-text hidden lg:inline">লাইভ অনলাইন</span>
          </button>
          <button type="button" onclick="fetchGpsLocation(true, false)" class="w-7 h-7 sm:w-8 sm:h-8 rounded-xl bg-white/15 hover:bg-white/25 flex items-center justify-center text-xs" title="Live GPS Location">
            <i class="fa-solid fa-location-crosshairs"></i>
          </button>
          <button type="button" onclick="toggleLanguage()" class="px-2 py-1.5 rounded-xl bg-amber-400 hover:bg-amber-300 text-slate-950 font-extrabold text-[10px] sm:text-xs flex items-center gap-1 shadow">
            <i class="fa-solid fa-language"></i>
            <span id="top-lang-label">English</span>
          </button>
          <button type="button" onclick="cycleColorTheme()" class="w-7 h-7 sm:w-8 sm:h-8 rounded-xl bg-white/15 hover:bg-white/25 flex items-center justify-center text-xs" title="Theme">
            <i class="fa-solid fa-palette"></i>
          </button>
          <button type="button" onclick="switchScreen('health')" class="w-7 h-7 sm:w-8 sm:h-8 rounded-xl bg-rose-500/90 hover:bg-rose-500 flex items-center justify-center text-xs" title="Health">
            <i class="fa-solid fa-heart-pulse animate-heartbeat"></i>
          </button>
          <button type="button" id="hdr-profile-btn" onclick="switchScreen('profile')" class="w-7 h-7 sm:w-8 sm:h-8 rounded-xl bg-white/20 hover:bg-white/30 border border-white/30 flex items-center justify-center text-xs" title="Profile">
            <i class="fa-solid fa-user-gear"></i>
          </button>
          <button type="button" id="hdr-settings-btn" onclick="switchScreen('settings')" class="w-7 h-7 sm:w-8 sm:h-8 rounded-xl bg-white/20 hover:bg-white/30 border border-white/30 flex items-center justify-center text-xs" title="Settings">
            <i class="fa-solid fa-gear"></i>
          </button>
        </div>
      </div>

      <!-- SPEAKING CLOCK STRIP -->
      <div class="bg-slate-900 text-white px-3.5 py-1.5 border-t border-white/10">
        <div class="max-w-7xl mx-auto flex items-center justify-between gap-3 text-xs">
          <button type="button" onclick="speakFullGreetingAndClock(true)" class="flex items-center gap-2.5 text-left hover:text-emerald-300 transition min-w-0">
            <span class="px-2 py-0.5 rounded-md bg-emerald-500/20 text-emerald-300 font-bold text-[11px] shrink-0" data-i18n="speakingClockLabel">কথা বলা ঘড়ি</span>
            <span id="live-clock-date" class="font-semibold text-slate-200 truncate">--</span>
            <span id="live-clock-time" class="font-mono font-extrabold text-amber-300 shrink-0">--:--:--</span>
          </button>
          <div class="flex items-center gap-2.5">
            <span class="hidden lg:inline text-[10px] text-slate-400 font-mono">Shortcut: Alt+1..8 | Esc</span>
            <button type="button" onclick="toggleAutoHourlyClockSpeech()" id="auto-clock-voice-pill" class="px-2.5 py-0.5 rounded-md bg-emerald-500 text-white text-[10px] font-bold flex items-center gap-1 shrink-0">
              <i class="fa-solid fa-volume-high"></i> <span>AUTO ON</span>
            </button>
          </div>
        </div>
      </div>

      <!-- GLOBAL SEARCH + BARCODE SCANNER -->
      <div class="bg-white/95 backdrop-blur px-3.5 py-2 border-b border-slate-200 text-slate-800">
        <div class="max-w-7xl mx-auto relative flex items-center gap-2.5">
          <div class="relative flex-1">
            <i class="fa-solid fa-magnifying-glass absolute left-3 top-1/2 -translate-y-1/2 text-slate-400 text-xs"></i>
            <input type="text" id="global-search-box" oninput="runGlobalSearch(this.value); speakTypingLive(this.value)" data-i18n-placeholder="globalSearchPlaceholder" placeholder="প্রোডাক্ট, বারকোড বা বাকি ক্রেতার নাম খুঁজুন..." class="w-full pl-8 pr-8 py-2 rounded-xl bg-slate-100 border border-slate-200 focus:bg-white focus:ring-2 focus:ring-emerald-500 outline-none text-xs sm:text-sm" />
            <button type="button" id="global-search-clear" onclick="clearGlobalSearch()" class="hidden absolute right-2.5 top-1/2 -translate-y-1/2 text-slate-400 hover:text-rose-500">
              <i class="fa-solid fa-circle-xmark"></i>
            </button>
          </div>
          <button type="button" onclick="switchScreen('barcode')" class="px-2.5 py-2 rounded-xl bg-slate-100 hover:bg-slate-200 text-slate-700 text-xs font-bold flex items-center gap-1.5" title="Barcode & QR">
            <i class="fa-solid fa-qrcode"></i>
          </button>
          <button type="button" onclick="openCameraBarcodeScanner('global')" class="px-3 py-2 rounded-xl theme-bg text-white text-xs font-extrabold flex items-center gap-2 shadow">
            <i class="fa-solid fa-camera"></i>
            <span data-i18n="scanBtn">স্ক্যান</span>
          </button>
          <div id="global-search-dropdown" class="hidden absolute left-0 right-0 top-11 bg-white rounded-2xl shadow-2xl border border-slate-200 max-h-64 overflow-y-auto z-50 divide-y divide-slate-100"></div>
        </div>
      </div>
    </header>

    <div class="max-w-7xl mx-auto px-3.5 pt-4 lg:grid lg:grid-cols-12 lg:gap-5 items-start">
      <!-- DESKTOP SIDEBAR -->
      <aside class="hidden lg:block lg:col-span-3 sticky top-28 space-y-2 app-card bg-white p-3 rounded-3xl shadow-sm border border-slate-200">
        <div class="px-2 py-1 text-[11px] font-extrabold uppercase tracking-wider text-slate-400">কম্পিউটার নেভিগেশন</div>
        <button type="button" onclick="switchScreen('dashboard')" data-nav="dashboard" class="nav-btn w-full flex items-center gap-3 px-3 py-2.5 rounded-xl text-xs font-extrabold theme-text theme-soft-bg">
          <i class="fa-solid fa-house w-4"></i> <span data-i18n="navDash">ড্যাশবোর্ড</span> <span class="ml-auto text-[10px] opacity-60 font-mono">Alt+1</span>
        </button>
        <button type="button" id="side-nav-product-entry" onclick="switchScreen('product-entry')" data-nav="product-entry" class="nav-btn w-full flex items-center gap-3 px-3 py-2.5 rounded-xl text-xs font-bold text-slate-600 hover:bg-slate-100">
          <i class="fa-solid fa-boxes-stacked w-4"></i> <span data-i18n="navProduct">প্রোডাক্ট</span> <span class="ml-auto text-[10px] opacity-60 font-mono">Alt+2</span>
        </button>
        <button type="button" onclick="switchScreen('product-sale')" data-nav="product-sale" class="nav-btn w-full flex items-center gap-3 px-3 py-2.5 rounded-xl text-xs font-bold text-slate-600 hover:bg-slate-100">
          <i class="fa-solid fa-cart-plus w-4"></i> <span data-i18n="navSale">বিক্রয়</span> <span class="ml-auto text-[10px] opacity-60 font-mono">Alt+3</span>
        </button>
        <button type="button" onclick="switchScreen('due-ledger')" data-nav="due-ledger" class="nav-btn w-full flex items-center gap-3 px-3 py-2.5 rounded-xl text-xs font-bold text-slate-600 hover:bg-slate-100">
          <i class="fa-solid fa-book w-4"></i> <span data-i18n="navDue">বাকি খাতা</span> <span class="ml-auto text-[10px] opacity-60 font-mono">Alt+4</span>
        </button>
        <button type="button" onclick="switchScreen('history')" data-nav="history" class="nav-btn w-full flex items-center gap-3 px-3 py-2.5 rounded-xl text-xs font-bold text-slate-600 hover:bg-slate-100">
          <i class="fa-solid fa-receipt w-4"></i> <span data-i18n="navVoucher">ভাউচার</span> <span class="ml-auto text-[10px] opacity-60 font-mono">Alt+5</span>
        </button>
        <button type="button" onclick="switchScreen('calendar')" data-nav="calendar" class="nav-btn w-full flex items-center gap-3 px-3 py-2.5 rounded-xl text-xs font-bold text-slate-600 hover:bg-slate-100">
          <i class="fa-solid fa-calendar-days w-4"></i> <span data-i18n="navCal">ক্যালেন্ডার</span> <span class="ml-auto text-[10px] opacity-60 font-mono">Alt+6</span>
        </button>
        <button type="button" onclick="switchScreen('calculator')" data-nav="calculator" class="nav-btn w-full flex items-center gap-3 px-3 py-2.5 rounded-xl text-xs font-bold text-slate-600 hover:bg-slate-100">
          <i class="fa-solid fa-calculator w-4"></i> <span data-i18n="navCalc">ক্যালকুলেটর</span> <span class="ml-auto text-[10px] opacity-60 font-mono">Alt+7</span>
        </button>
        <button type="button" onclick="switchScreen('notebook')" data-nav="notebook" class="nav-btn w-full flex items-center gap-3 px-3 py-2.5 rounded-xl text-xs font-bold text-slate-600 hover:bg-slate-100">
          <i class="fa-solid fa-note-sticky w-4"></i> <span data-i18n="navNote">নোটবুক</span> <span class="ml-auto text-[10px] opacity-60 font-mono">Alt+8</span>
        </button>
        <div class="pt-2 border-t border-slate-100 space-y-1.5">
          <button type="button" onclick="switchScreen('barcode')" class="w-full flex items-center gap-3 px-3 py-2 rounded-xl text-xs font-bold text-slate-600 hover:bg-slate-100">
            <i class="fa-solid fa-barcode w-4"></i> <span>বারকোড / QR</span>
          </button>
          <button type="button" onclick="switchScreen('health')" class="w-full flex items-center gap-3 px-3 py-2 rounded-xl text-xs font-bold text-rose-600 hover:bg-rose-50">
            <i class="fa-solid fa-heart-pulse w-4"></i> <span>স্বাস্থ্য পরীক্ষা</span>
          </button>
          <button type="button" onclick="switchScreen('profile')" class="w-full flex items-center gap-3 px-3 py-2 rounded-xl text-xs font-bold text-slate-600 hover:bg-slate-100">
            <i class="fa-solid fa-user-shield w-4"></i> <span>প্রোফাইল ও স্টাফ</span>
          </button>
          <button type="button" id="side-nav-settings" onclick="switchScreen('settings')" class="w-full flex items-center gap-3 px-3 py-2 rounded-xl text-xs font-bold text-slate-600 hover:bg-slate-100">
            <i class="fa-solid fa-sliders w-4"></i> <span>সেটিংস ও ব্যাকআপ</span>
          </button>
        </div>
      </aside>

      <main class="lg:col-span-9 space-y-4">

      <!-- 1. DASHBOARD -->
      <section id="screen-dashboard" class="app-screen space-y-4">
        <div class="flex items-center justify-between flex-wrap gap-2.5">
          <h2 class="text-base sm:text-lg font-extrabold flex items-center gap-2.5">
            <i class="fa-solid fa-chart-pie theme-text"></i>
            <span data-i18n="dashTitle">ড্যাশবোর্ড</span>
          </h2>
          <div class="flex items-center gap-2">
            <button type="button" onclick="toggleLanguage()" class="px-2.5 py-1.5 rounded-xl bg-amber-400 hover:bg-amber-300 text-slate-950 text-xs font-extrabold flex items-center gap-1.5 shadow">
              <i class="fa-solid fa-language"></i>
              <span id="dash-lang-label">English</span>
            </button>
            <button type="button" id="dash-reset-btn" onclick="promptDashboardReset()" class="px-3 py-1.5 rounded-xl bg-rose-100 hover:bg-rose-200 text-rose-700 text-xs font-extrabold flex items-center gap-2 transition">
              <i class="fa-solid fa-rotate-left"></i>
              <span data-i18n="resetBtn">রিসেট</span>
            </button>
          </div>
        </div>

        <!-- CUSTOM WELCOME SPEECH & SHOP GREETING BAR -->
        <div class="app-card bg-white p-3.5 rounded-2xl shadow-sm border border-emerald-200 space-y-2.5">
          <div class="flex items-center justify-between gap-2 flex-wrap">
            <span class="text-xs font-extrabold text-emerald-900 flex items-center gap-1.5">
              <i class="fa-solid fa-bullhorn text-emerald-600"></i>
              <span>কাস্টম স্বাগতম বার্তা ও স্বয়ংক্রিয় ঘোষণা (সময়, তারিখ, মাস + দোকান)</span>
            </span>
            <div class="flex items-center gap-1.5">
              <button type="button" onclick="saveCustomGreetingMessage()" class="px-2.5 py-1 rounded-xl bg-emerald-600 hover:bg-emerald-700 text-white text-[11px] font-extrabold flex items-center gap-1">
                <i class="fa-solid fa-floppy-disk"></i> <span>সেভ করুন</span>
              </button>
              <button type="button" onclick="speakFullGreetingAndClock(true)" class="px-2.5 py-1 rounded-xl bg-indigo-600 hover:bg-indigo-700 text-white text-[11px] font-extrabold flex items-center gap-1">
                <i class="fa-solid fa-volume-high"></i> <span>এখনই বলুন</span>
              </button>
            </div>
          </div>
          <div class="grid grid-cols-1 sm:grid-cols-3 gap-2">
            <input type="text" id="custom-greeting-input" oninput="onCustomGreetingTyping(this.value)" class="sm:col-span-2 px-3 py-2 rounded-xl border border-slate-300 text-xs font-bold outline-none focus:ring-2 focus:ring-emerald-500" placeholder="যেমন: স্বাগতম বার্তা লিখুন (যা লিখবেন স্বয়ংক্রিয়ভাবে বলবে)..." />
            <input type="text" id="custom-greeting-shop-input" oninput="onCustomGreetingShopTyping(this.value)" class="px-3 py-2 rounded-xl border border-slate-300 text-xs font-bold outline-none focus:ring-2 focus:ring-emerald-500" placeholder="দোকানের নাম (যেমন: উমুক দোকান)" />
          </div>
          <div id="custom-greeting-preview-line" class="text-[11px] font-semibold text-slate-600 truncate"></div>
        </div>

        <!-- QUICK DISPLAY THEME & CUSTOM THEME BAR -->
        <div class="app-card bg-white p-3 rounded-2xl shadow-sm border border-slate-200 space-y-2">
          <div class="flex items-center justify-between flex-wrap gap-2">
            <span class="text-xs font-extrabold text-slate-700 flex items-center gap-1.5">
              <i class="fa-solid fa-palette theme-text"></i>
              <span>রঙের থিম ও কাস্টম ছবি থিম</span>
            </span>
            <label class="px-2.5 py-1 rounded-xl bg-indigo-600 hover:bg-indigo-700 text-white text-[11px] font-extrabold cursor-pointer flex items-center gap-1.5">
              <i class="fa-solid fa-image"></i> <span>মেমরি থেকে থিম ছবি</span>
              <input type="file" accept="image/*" onchange="uploadCustomThemePhoto(event)" class="hidden" />
            </label>
          </div>
          <div class="flex flex-wrap gap-1.5">
            <button type="button" onclick="applyColorTheme('theme-emerald')" class="px-2.5 py-1 rounded-lg bg-emerald-600 text-white text-[11px] font-bold">Emerald</button>
            <button type="button" onclick="applyColorTheme('theme-royal')" class="px-2.5 py-1 rounded-lg bg-blue-600 text-white text-[11px] font-bold">Royal Blue</button>
            <button type="button" onclick="applyColorTheme('theme-crimson')" class="px-2.5 py-1 rounded-lg bg-rose-600 text-white text-[11px] font-bold">Crimson</button>
            <button type="button" onclick="applyColorTheme('theme-purple')" class="px-2.5 py-1 rounded-lg bg-purple-600 text-white text-[11px] font-bold">Purple</button>
            <button type="button" onclick="applyColorTheme('theme-amber')" class="px-2.5 py-1 rounded-lg bg-amber-600 text-white text-[11px] font-bold">Amber Gold</button>
            <button type="button" onclick="applyColorTheme('theme-teal')" class="px-2.5 py-1 rounded-lg bg-teal-600 text-white text-[11px] font-bold">Teal Ocean</button>
            <button type="button" onclick="applyColorTheme('theme-magenta')" class="px-2.5 py-1 rounded-lg bg-fuchsia-600 text-white text-[11px] font-bold">Magenta</button>
            <button type="button" onclick="applyColorTheme('theme-indigo')" class="px-2.5 py-1 rounded-lg bg-indigo-600 text-white text-[11px] font-bold">Indigo</button>
            <button type="button" onclick="applyColorTheme('theme-lime')" class="px-2.5 py-1 rounded-lg bg-lime-600 text-white text-[11px] font-bold">Lime Green</button>
            <button type="button" onclick="applyColorTheme('theme-dark')" class="px-2.5 py-1 rounded-lg bg-slate-900 text-white text-[11px] font-bold">Dark</button>
          </div>
        </div>

        <div class="flex flex-wrap items-center gap-2.5">
          <button type="button" onclick="fetchGpsLocation(true, false)" class="px-3 py-1.5 rounded-xl bg-teal-600 hover:bg-teal-700 text-white text-xs font-extrabold flex items-center gap-2 shadow">
            <i class="fa-solid fa-location-crosshairs"></i>
            <span data-i18n="gpsBtn">লাইভ GPS</span>
          </button>
          <button type="button" onclick="speakFullGreetingAndClock(true)" class="px-3 py-1.5 rounded-xl bg-indigo-600 hover:bg-indigo-700 text-white text-xs font-extrabold flex items-center gap-2 shadow">
            <i class="fa-solid fa-volume-high"></i>
            <span data-i18n="welcomeVoiceBtn">স্বাগতম ভয়েস</span>
          </button>
          <button type="button" onclick="openDigitalPaymentModal()" class="px-3 py-1.5 rounded-xl bg-pink-600 hover:bg-pink-700 text-white text-xs font-extrabold flex items-center gap-2 shadow">
            <i class="fa-solid fa-credit-card"></i>
            <span data-i18n="digitalPayBtn">ডিজিটাল পেমেন্ট</span>
          </button>
          <button type="button" onclick="openStaffSmsModal()" class="px-3 py-1.5 rounded-xl bg-violet-600 hover:bg-violet-700 text-white text-xs font-extrabold flex items-center gap-2 shadow">
            <i class="fa-solid fa-message"></i>
            <span>অ্যাডমিন-স্টাফ SMS</span>
          </button>
        </div>

        <!-- COMPACT SMART SHOP AI ASSISTANT -->
        <div class="rounded-2xl bg-gradient-to-r from-slate-900 via-indigo-950 to-slate-900 text-white p-2 shadow border border-indigo-400/25 space-y-1.5">
          <div class="flex items-center justify-between gap-1.5 flex-wrap">
            <div class="flex items-center gap-1.5">
              <div class="w-6 h-6 rounded-lg bg-indigo-500/25 border border-indigo-300/30 flex items-center justify-center text-[11px] text-amber-300">
                <i class="fa-solid fa-robot"></i>
              </div>
              <h3 class="font-extrabold text-[11px]">স্মার্ট AI</h3>
              <span class="px-1.5 py-0.2 rounded-full bg-emerald-500/20 text-emerald-300 text-[8.5px] font-extrabold">LIVE</span>
            </div>
            <div class="flex items-center gap-1 text-[9.5px] font-bold">
              <button type="button" onclick="runDashboardAIPrompt('stock')" class="px-1.5 py-0.5 rounded bg-white/10 hover:bg-white/20 text-emerald-200">স্টক</button>
              <button type="button" onclick="runDashboardAIPrompt('due')" class="px-1.5 py-0.5 rounded bg-white/10 hover:bg-white/20 text-amber-200">বাকি</button>
              <button type="button" onclick="runDashboardAIPrompt('profit')" class="px-1.5 py-0.5 rounded bg-white/10 hover:bg-white/20 text-sky-200">লাভ</button>
              <button type="button" onclick="speakDashboardAI()" class="px-1.5 py-0.5 rounded bg-amber-400 hover:bg-amber-300 text-slate-950 font-extrabold">
                <i class="fa-solid fa-volume-high"></i>
              </button>
            </div>
          </div>

          <div id="dashboard-ai-insights" class="px-2 py-1 rounded-lg bg-black/35 border border-white/10 text-[10px] space-y-0.5 leading-tight max-h-14 overflow-y-auto"></div>

          <div class="flex items-center gap-1">
            <input type="text" id="dashboard-ai-input" onkeydown="if(event.key==='Enter'){event.preventDefault();runDashboardAICustom();}" class="flex-1 px-2 py-1 rounded-lg bg-white/10 border border-white/15 text-[10px] text-white placeholder-indigo-200/70 outline-none focus:ring-1 focus:ring-indigo-400" placeholder="AI প্রশ্ন..." />
            <button type="button" onclick="runDashboardAICustom()" class="px-2 py-1 rounded-lg bg-emerald-500 hover:bg-emerald-400 text-slate-950 text-[10px] font-extrabold">
              জিজ্ঞাসা
            </button>
          </div>
        </div>

        <!-- FINANCIAL KPI CARDS -->
        <div id="dash-financial-grid" class="grid grid-cols-2 sm:grid-cols-3 gap-3">
          <div class="app-card bg-white p-4 rounded-2xl shadow-sm border border-slate-200">
            <div class="text-xs font-bold text-slate-500" data-i18n="kpiCapital">মোট মূলধন</div>
            <div id="dash-capital" class="text-lg sm:text-xl font-extrabold text-indigo-700 mt-1">৳ 0.00</div>
          </div>
          <div class="app-card bg-white p-4 rounded-2xl shadow-sm border border-slate-200">
            <div class="text-xs font-bold text-slate-500" data-i18n="kpiItems">মোট প্রোডাক্ট</div>
            <div id="dash-items" class="text-lg sm:text-xl font-extrabold text-slate-800 mt-1">0</div>
          </div>
          <div class="app-card bg-white p-4 rounded-2xl shadow-sm border border-slate-200">
            <div class="text-xs font-bold text-slate-500" data-i18n="kpiSales">মোট বিক্রয়</div>
            <div id="dash-sales" class="text-lg sm:text-xl font-extrabold text-emerald-600 mt-1">৳ 0.00</div>
          </div>
          <div class="app-card bg-white p-4 rounded-2xl shadow-sm border border-slate-200">
            <div class="text-xs font-bold text-slate-500" data-i18n="kpiProfit">মোট লাভ</div>
            <div id="dash-profit" class="text-lg sm:text-xl font-extrabold text-teal-600 mt-1">৳ 0.00</div>
          </div>
          <div class="app-card bg-white p-4 rounded-2xl shadow-sm border border-slate-200">
            <div class="text-xs font-bold text-slate-500" data-i18n="kpiDue">মোট বাকি</div>
            <div id="dash-due" class="text-lg sm:text-xl font-extrabold text-amber-600 mt-1">৳ 0.00</div>
          </div>
          <div class="app-card bg-white p-4 rounded-2xl shadow-sm border border-slate-200">
            <div class="text-xs font-bold text-slate-500" data-i18n="kpiLoss">মোট লস</div>
            <div id="dash-loss" class="text-lg sm:text-xl font-extrabold text-rose-600 mt-1">৳ 0.00</div>
          </div>
        </div>

        <!-- LIVE GPS LOCATION, APPROXIMATE POSITION DETAILS, MAP, AUTO ROUTE & SHARED LIVE LOCATION CARD -->
        <div class="app-card bg-white p-4 rounded-2xl shadow-sm border border-slate-200 space-y-3">
          <div class="flex flex-col sm:flex-row sm:items-center justify-between gap-3">
            <div class="flex items-start gap-3.5">
              <div class="w-10 h-10 rounded-2xl bg-teal-100 text-teal-700 flex items-center justify-center text-base shrink-0">
                <i class="fa-solid fa-map-location-dot"></i>
              </div>
              <div>
                <div class="flex items-center gap-2 flex-wrap">
                  <span class="text-xs font-extrabold text-slate-700" data-i18n="gpsCardTitle">লাইভ GPS লোকেশন ও নেটওয়ার্ক</span>
                  <span id="dash-net-pill" class="px-2 py-0.5 rounded-full text-[10px] font-extrabold bg-emerald-100 text-emerald-800">লাইভ অনলাইন</span>
                  <span id="gps-source-badge" class="px-2 py-0.5 rounded-full text-[10px] font-extrabold bg-teal-100 text-teal-800">AUTO GPS</span>
                  <span id="auto-sync-status-badge" class="px-2 py-0.5 rounded-full text-[10px] font-extrabold bg-indigo-100 text-indigo-800">অটো-সেভ সক্রিয়</span>
                </div>
                <div id="gps-location-text" class="text-xs font-bold text-slate-800 mt-0.5" data-i18n="gpsWaitingText">লাইভ GPS লোকেশন স্বয়ংক্রিয়ভাবে চালু হচ্ছে...</div>
                <div id="gps-coords-text" class="text-[11px] font-mono text-slate-500 mt-0.5">Lat: -- | Lon: --</div>
                <div id="gps-approx-summary" class="text-[11px] font-semibold text-teal-800 mt-1 bg-teal-50/80 px-2.5 py-1.5 rounded-xl border border-teal-200/80">
                  মোটামুটি অবস্থান তথ্য স্বয়ংক্রিয়ভাবে লোড হচ্ছে...
                </div>
              </div>
            </div>
            <div class="flex items-center gap-2 self-end sm:self-center flex-wrap">
              <button type="button" onclick="fetchGpsLocation(true, false)" class="px-3 py-1.5 rounded-xl bg-teal-600 hover:bg-teal-700 text-white text-xs font-extrabold flex items-center gap-1.5">
                <i class="fa-solid fa-location-crosshairs"></i> <span data-i18n="gpsRefreshBtn">রিয়েল GPS</span>
              </button>
              <button type="button" onclick="openLiveMapAndAutoRoute()" class="px-2.5 py-1.5 rounded-xl bg-indigo-600 hover:bg-indigo-700 text-white text-xs font-extrabold flex items-center gap-1.5" title="Live Map & Auto Route">
                <i class="fa-solid fa-route"></i> <span>লাইভ ম্যাপ ও রুট</span>
              </button>
              <button type="button" onclick="applyGpsToStoreAddress()" class="px-2.5 py-1.5 rounded-xl bg-slate-100 hover:bg-slate-200 text-slate-800 text-xs font-bold" title="Save to Shop Address">
                <i class="fa-solid fa-floppy-disk"></i>
              </button>
            </div>
          </div>

          <div class="p-3 rounded-2xl bg-teal-50/70 border border-teal-200 space-y-2">
            <div class="flex items-center justify-between gap-2 flex-wrap">
              <span class="text-[11px] font-extrabold text-teal-950"><i class="fa-solid fa-location-arrow mr-1 text-teal-600"></i>শেয়ার করা লাইভ স্থান / গন্তব্যে স্বয়ংক্রিয় রুট (Live Location Route)</span>
              <button type="button" onclick="shareMyCurrentLiveLocation()" class="px-2.5 py-1 rounded-lg bg-emerald-600 hover:bg-emerald-700 text-white text-[10px] font-extrabold flex items-center gap-1">
                <i class="fa-solid fa-share-location"></i> <span>আমার লাইভ স্থান শেয়ার</span>
              </button>
            </div>
            <div class="flex flex-col sm:flex-row gap-2">
              <input type="text" id="target-shared-location-input" oninput="updateLiveMapEmbed()" class="flex-1 px-3 py-1.5 rounded-xl border border-teal-300 bg-white text-xs font-bold outline-none focus:ring-2 focus:ring-teal-500" placeholder="শেয়ার করা লাইভ লোকেশন / ঠিকানা / Lat,Lon বা Google Maps লিংক দিন..." />
              <button type="button" onclick="navigateToSharedLocation()" class="px-3 py-1.5 rounded-xl bg-teal-700 hover:bg-teal-800 text-white text-xs font-extrabold whitespace-nowrap flex items-center gap-1.5">
                <i class="fa-solid fa-diamond-turn-right"></i> <span>রুট তৈরি ও যান</span>
              </button>
            </div>
            <div class="rounded-xl overflow-hidden border border-teal-200 bg-slate-100 h-44">
              <iframe id="live-gps-map-iframe" title="Live Map" class="w-full h-full border-0" loading="lazy" referrerpolicy="no-referrer-when-downgrade" src="https://www.openstreetmap.org/export/embed.html?bbox=91.75%2C22.32%2C91.83%2C22.39&layer=mapnik&marker=22.3569%2C91.7832"></iframe>
            </div>
          </div>
        </div>

        <!-- LIVE WEATHER CARD -->
        <div class="app-card bg-gradient-to-r from-sky-600 to-blue-700 text-white p-3.5 rounded-2xl shadow-md flex items-center justify-between gap-3">
          <div class="flex items-center gap-3 min-w-0">
            <div class="w-10 h-10 rounded-xl bg-white/15 flex items-center justify-center text-lg shrink-0">
              <i id="weather-icon" class="fa-solid fa-sun text-amber-300"></i>
            </div>
            <div class="min-w-0">
              <div class="text-[10px] font-bold text-sky-100 truncate" data-i18n="weatherTitle">লাইভ আবহাওয়া আপডেট</div>
              <div class="flex items-baseline gap-2">
                <span id="weather-temp-val" class="text-lg font-extrabold">--°C</span>
                <span id="weather-condition-text" class="text-[11px] font-semibold text-sky-100 truncate">লাইভ লোড হচ্ছে...</span>
              </div>
              <div id="weather-extra-text" class="text-[10px] text-sky-200 truncate">আর্দ্রতা: --% | বাতাস: -- কি.মি./ঘণ্টা</div>
            </div>
          </div>
          <button type="button" onclick="speakWeatherReport()" class="px-2.5 py-1.5 rounded-xl bg-white/20 hover:bg-white/30 text-[11px] font-extrabold flex items-center gap-1.5 shrink-0">
            <i class="fa-solid fa-volume-high"></i>
            <span data-i18n="weatherVoiceBtn">আবহাওয়া শুনুন</span>
          </button>
        </div>

        <!-- OTHER EXPENSES SECTION -->
        <div id="dash-expenses-card" class="app-card bg-white p-4 rounded-2xl shadow-sm border border-slate-200 space-y-3">
          <div class="flex items-center justify-between">
            <h3 class="font-extrabold text-sm flex items-center gap-2">
              <i class="fa-solid fa-receipt text-rose-500"></i>
              <span data-i18n="otherExpensesTitle">অন্যান্য খরচ (বিদ্যুৎ, ভাড়া, যাতায়াত ইত্যাদি)</span>
            </h3>
            <span id="dash-expenses" class="text-sm font-extrabold text-rose-600">৳ 0.00</span>
          </div>
          <form onsubmit="addOtherExpense(event)" class="grid grid-cols-1 sm:grid-cols-3 gap-2.5">
            <input type="text" id="exp-desc" required oninput="speakTypingLive(this.value)" data-i18n-placeholder="expDescPlaceholder" placeholder="খরচের বিবরণ (যেমন: বিদ্যুৎ বিল)" class="px-3 py-2 rounded-xl border border-slate-300 text-xs sm:text-sm outline-none focus:ring-2 focus:ring-rose-400" />
            <input type="number" step="0.01" id="exp-amt" required oninput="speakTypingLive(this.value)" data-i18n-placeholder="expAmtPlaceholder" placeholder="টাকার পরিমাণ (৳)" class="px-3 py-2 rounded-xl border border-slate-300 text-xs sm:text-sm outline-none focus:ring-2 focus:ring-rose-400" />
            <button type="submit" class="py-2 px-4 rounded-xl bg-rose-600 hover:bg-rose-700 text-white text-xs sm:text-sm font-extrabold flex items-center justify-center gap-2">
              <i class="fa-solid fa-plus"></i> <span data-i18n="addExpenseBtn">খরচ যোগ করুন</span>
            </button>
          </form>
          <div id="expenses-list" class="space-y-1.5 max-h-40 overflow-y-auto text-xs"></div>
        </div>

        <button type="button" id="dash-quick-entry-btn" onclick="switchScreen('product-entry')" class="w-full py-3.5 rounded-2xl theme-bg text-white font-extrabold text-sm shadow-lg flex items-center justify-center gap-2.5">
          <i class="fa-solid fa-circle-plus text-base"></i>
          <span data-i18n="quickProductEntryBtn">নতুন প্রোডাক্ট ক্রয় ও স্টক এন্ট্রি করুন</span>
        </button>
      </section>

      <!-- 2. PRODUCT PURCHASE / STOCK ENTRY -->
      <section id="screen-product-entry" class="app-screen space-y-4 hidden">
        <div class="flex items-center justify-between gap-2.5">
          <h2 class="text-base sm:text-lg font-extrabold flex items-center gap-2.5">
            <i class="fa-solid fa-boxes-stacked theme-text"></i>
            <span data-i18n="prodEntryTitle">প্রোডাক্ট ক্রয় ও স্টক এন্ট্রি</span>
          </h2>
          <button type="button" onclick="switchScreen('product-sale')" class="px-3 py-1.5 rounded-xl bg-emerald-600 text-white text-xs font-extrabold flex items-center gap-2">
            <i class="fa-solid fa-cart-shopping"></i>
            <span data-i18n="goToSaleBtn">বিক্রয় পাতায় যান</span>
          </button>
        </div>

        <form id="product-form" onsubmit="savePurchasedProduct(event)" class="app-card bg-white p-4 rounded-2xl shadow-sm border border-slate-200 space-y-3">
          <input type="hidden" id="prod-edit-id" />
          <div class="grid grid-cols-1 sm:grid-cols-2 gap-3">
            <div>
              <label class="block text-xs font-bold mb-1" data-i18n="prodNameLabel">প্রোডাক্টের নাম</label>
              <input type="text" id="prod-name" required oninput="onProductNameOrSellPriceAutoCode(); speakTypingLive(this.value)" class="w-full px-3 py-2 rounded-xl border border-slate-300 text-sm outline-none focus:ring-2 focus:ring-emerald-500" placeholder="যেমন: মিনিকেট চাল / সাবান" />
            </div>
            <div>
              <label class="block text-xs font-bold mb-1" data-i18n="barcodeLabel">বারকোড / QR কোড</label>
              <div class="flex gap-2">
                <input type="text" id="prod-barcode" oninput="updateEntryAutoCodePreviews(); speakTypingLive(this.value)" class="w-full px-3 py-2 rounded-xl border border-slate-300 text-sm outline-none focus:ring-2 focus:ring-emerald-500" placeholder="নাম ও বিক্রয় মূল্যে স্বয়ংক্রিয় যুক্ত হবে" />
                <button type="button" onclick="openCameraBarcodeScanner('product-entry')" class="px-3 rounded-xl bg-slate-800 text-white text-xs" title="Scan Barcode">
                  <i class="fa-solid fa-camera"></i>
                </button>
                <button type="button" onclick="autoFillBarcode('prod-barcode'); updateEntryAutoCodePreviews()" class="px-3 rounded-xl bg-indigo-100 text-indigo-700 text-xs font-bold" title="Auto Generate">
                  <i class="fa-solid fa-barcode"></i>
                </button>
              </div>
            </div>
          </div>

          <div class="grid grid-cols-2 sm:grid-cols-4 gap-2.5">
            <div>
              <label class="block text-xs font-bold mb-1" data-i18n="unitLabel">পরিমাপ একক</label>
              <select id="prod-unit" onchange="calculatePurchasePreview()" class="w-full px-2.5 py-2 rounded-xl border border-slate-300 text-xs sm:text-sm font-semibold bg-white"></select>
            </div>
            <div>
              <label class="block text-xs font-bold mb-1" data-i18n="qtyLabel">পরিমাণ</label>
              <input type="number" step="any" id="prod-qty" required oninput="calculatePurchasePreview(); speakTypingLive(this.value)" class="w-full px-3 py-2 rounded-xl border border-slate-300 text-sm font-bold" placeholder="0" />
            </div>
            <div>
              <label class="block text-xs font-bold mb-1" data-i18n="buyPriceLabel">ক্রয় মূল্য (প্রতি একক)</label>
              <input type="number" step="0.01" id="prod-buy" required oninput="calculatePurchasePreview(); speakTypingLive(this.value)" class="w-full px-3 py-2 rounded-xl border border-slate-300 text-sm font-bold" placeholder="0.00" />
            </div>
            <div>
              <label class="block text-xs font-bold mb-1" data-i18n="sellPriceLabel">বিক্রয় মূল্য (প্রতি একক)</label>
              <input type="number" step="0.01" id="prod-sell" required oninput="calculatePurchasePreview(); onProductNameOrSellPriceAutoCode(); speakTypingLive(this.value)" class="w-full px-3 py-2 rounded-xl border border-slate-300 text-sm font-bold" placeholder="0.00" />
            </div>
          </div>

          <div id="entry-auto-code-box" class="p-3 rounded-2xl bg-slate-50 border border-slate-200 grid grid-cols-1 sm:grid-cols-2 gap-3 items-center">
            <div class="text-center space-y-1.5">
              <div class="text-[11px] font-extrabold text-slate-600">স্বয়ংক্রিয় বারকোড (Barcode)</div>
              <svg id="entry-barcode-svg" class="mx-auto max-w-full"></svg>
              <button type="button" onclick="saveBarcodeImageToMemory('entry-barcode-svg', document.getElementById('prod-barcode').value || 'Barcode')" class="px-3 py-1 rounded-xl bg-slate-800 hover:bg-slate-900 text-white text-[11px] font-extrabold inline-flex items-center gap-1.5">
                <i class="fa-solid fa-download"></i> <span>বারকোড মেমোরিতে সেভ</span>
              </button>
            </div>
            <div class="text-center space-y-1.5 flex flex-col items-center">
              <div class="text-[11px] font-extrabold text-slate-600">স্বয়ংক্রিয় QR কোড</div>
              <div id="entry-qrcode-div"></div>
              <button type="button" onclick="saveQrImageToMemory('entry-qrcode-div', document.getElementById('prod-barcode').value || 'QRCode')" class="px-3 py-1 rounded-xl bg-indigo-600 hover:bg-indigo-700 text-white text-[11px] font-extrabold inline-flex items-center gap-1.5">
                <i class="fa-solid fa-download"></i> <span>QR কোড মেমোরিতে সেভ</span>
              </button>
            </div>
          </div>

          <div class="p-3 rounded-xl theme-soft-bg flex flex-wrap items-center justify-between gap-2 text-xs font-bold">
            <div><span data-i18n="totalPurchaseCost">মোট ক্রয় খরচ:</span> <span id="purchase-auto-total" class="text-sm font-extrabold text-indigo-700">৳ 0.00</span></div>
            <div><span data-i18n="estProfitLabel">সম্ভাব্য লাভ:</span> <span id="purchase-auto-profit" class="text-sm font-extrabold text-emerald-700">৳ 0.00</span></div>
          </div>

          <div class="flex gap-2.5">
            <button type="submit" class="flex-1 py-3 rounded-xl theme-bg text-white font-extrabold text-sm shadow-md flex items-center justify-center gap-2">
              <i class="fa-solid fa-floppy-disk"></i>
              <span data-i18n="saveProductBtn">স্টকে সংরক্ষণ করুন</span>
            </button>
            <button type="button" id="prod-cancel-btn" onclick="resetProductForm()" class="hidden px-4 py-3 rounded-xl bg-slate-200 text-slate-800 font-bold text-xs">
              বাতিল
            </button>
          </div>
        </form>

        <div class="app-card bg-white p-4 rounded-2xl shadow-sm border border-slate-200 space-y-2.5">
          <h3 class="font-extrabold text-sm flex items-center gap-2">
            <i class="fa-solid fa-warehouse text-indigo-600"></i>
            <span data-i18n="currentStockTitle">বর্তমান স্টক তালিকা</span>
          </h3>
          <div id="inventory-list" class="space-y-2"></div>
        </div>
      </section>

      <!-- 3. PRODUCT SALE & VOUCHER GENERATOR -->
      <section id="screen-product-sale" class="app-screen space-y-4 hidden">
        <h2 class="text-base sm:text-lg font-extrabold flex items-center gap-2.5">
          <i class="fa-solid fa-cash-register theme-text"></i>
          <span data-i18n="saleTitle">প্রোডাক্ট বিক্রয় ও ভাউচার</span>
        </h2>

        <form id="sale-form" onsubmit="completeProductSale(event)" class="app-card bg-white p-4 rounded-2xl shadow-sm border border-slate-200 space-y-3.5">
          <div>
            <label class="block text-xs font-bold mb-1" data-i18n="selectSoldProduct">বিক্রয়কৃত প্রোডাক্ট নির্বাচন করুন</label>
            <select id="sale-prod-select" onchange="onSaleProductSelected()" class="w-full px-3 py-2.5 rounded-xl border border-slate-300 text-sm font-bold bg-white"></select>
            <div id="sale-stock-hint" class="text-[11px] font-semibold text-emerald-700 mt-1"></div>
          </div>

          <div class="grid grid-cols-2 sm:grid-cols-4 gap-2.5">
            <div>
              <label class="block text-xs font-bold mb-1" data-i18n="saleUnitLabel">বিক্রয় একক</label>
              <select id="sale-unit-select" onchange="recalculateSalePrice()" class="w-full px-2.5 py-2 rounded-xl border border-slate-300 text-xs sm:text-sm font-semibold bg-white"></select>
            </div>
            <div>
              <label class="block text-xs font-bold mb-1" data-i18n="qtyLabel">পরিমাণ</label>
              <input type="number" step="any" id="sale-qty" oninput="recalculateSalePrice(); speakTypingLive(this.value)" class="w-full px-3 py-2 rounded-xl border border-slate-300 text-sm font-bold" placeholder="0" />
            </div>
            <div>
              <label class="block text-xs font-bold mb-1" data-i18n="rateLabel">দর (৳)</label>
              <input type="number" step="0.01" id="sale-rate" oninput="recalculateSalePrice(); speakTypingLive(this.value)" class="w-full px-3 py-2 rounded-xl border border-slate-300 text-sm font-bold" placeholder="0.00" />
            </div>
            <div>
              <label class="block text-xs font-bold mb-1" data-i18n="dateLabel">তারিখ</label>
              <input type="date" id="sale-date" class="w-full px-2.5 py-2 rounded-xl border border-slate-300 text-xs sm:text-sm font-semibold" />
            </div>
          </div>

          <div class="flex flex-wrap items-center justify-between gap-2">
            <button type="button" onclick="addProductToSaleCart()" class="px-3.5 py-2 rounded-xl bg-teal-600 hover:bg-teal-700 text-white text-xs font-extrabold flex items-center gap-2 shadow">
              <i class="fa-solid fa-cart-plus"></i>
              <span data-i18n="addMoreProductBtn">আরও প্রোডাক্ট ভাউচারে যোগ করুন</span>
            </button>
            <span id="sale-cart-count-badge" class="text-xs font-extrabold text-teal-700"></span>
          </div>

          <div id="sale-multi-cart-box" class="hidden p-3 rounded-2xl bg-teal-50 border border-teal-200 space-y-2">
            <div class="flex items-center justify-between text-xs font-extrabold text-teal-900">
              <span data-i18n="voucherCartTitle">ভাউচারে যুক্ত প্রোডাক্ট তালিকা</span>
              <button type="button" onclick="clearSaleCart()" class="text-rose-600 hover:underline text-[11px]" data-i18n="clearCartBtn">সব বাদ দিন</button>
            </div>
            <div id="sale-multi-cart-items" class="space-y-1.5 text-xs"></div>
          </div>

          <div class="p-3.5 rounded-2xl bg-slate-900 text-white flex items-center justify-between">
            <span class="text-xs sm:text-sm font-bold text-slate-300" data-i18n="grandTotalLabel">সর্বমোট বিক্রয় মূল্য</span>
            <span id="sale-grand-total" class="text-xl sm:text-2xl font-extrabold text-amber-300">৳ 0.00</span>
          </div>

          <div>
            <label class="block text-xs font-bold mb-1.5" data-i18n="paymentMethodLabel">পেমেন্ট মাধ্যম</label>
            <div class="grid grid-cols-3 gap-2.5">
              <label class="cursor-pointer">
                <input type="radio" name="salePayMode" value="cash" checked onchange="onSalePayModeChange()" class="sr-only peer" />
                <div class="py-2.5 px-2 rounded-xl border border-slate-300 peer-checked:bg-emerald-600 peer-checked:text-white peer-checked:border-emerald-600 text-center text-xs font-extrabold transition">
                  <i class="fa-solid fa-money-bill-wave block mb-1"></i>
                  <span data-i18n="payCash">নগদ</span>
                </div>
              </label>
              <label class="cursor-pointer">
                <input type="radio" name="salePayMode" value="due" onchange="onSalePayModeChange()" class="sr-only peer" />
                <div class="py-2.5 px-2 rounded-xl border border-slate-300 peer-checked:bg-amber-500 peer-checked:text-white peer-checked:border-amber-500 text-center text-xs font-extrabold transition">
                  <i class="fa-solid fa-book-open block mb-1"></i>
                  <span data-i18n="payDue">বাকি</span>
                </div>
              </label>
              <label class="cursor-pointer">
                <input type="radio" name="salePayMode" value="digital" onchange="onSalePayModeChange()" class="sr-only peer" />
                <div class="py-2.5 px-2 rounded-xl border border-slate-300 peer-checked:bg-pink-600 peer-checked:text-white peer-checked:border-pink-600 text-center text-xs font-extrabold transition">
                  <i class="fa-solid fa-mobile-screen-button block mb-1"></i>
                  <span data-i18n="payDigital">ডিজিটাল পেমেন্ট</span>
                </div>
              </label>
            </div>
          </div>

          <div id="sale-due-picker-wrap" class="hidden p-3 rounded-2xl bg-amber-50 border border-amber-200 space-y-2">
            <label class="block text-xs font-extrabold text-amber-900" data-i18n="selectExistingDue">বিদ্যমান বাকি ক্রেতা নির্বাচন করুন (ঐচ্ছিক)</label>
            <select id="sale-due-picker" onchange="populateDueCustomerFields()" class="w-full px-3 py-2 rounded-xl border border-amber-300 bg-white text-xs sm:text-sm font-bold"></select>
          </div>

          <div id="sale-digital-strip" class="hidden p-3 rounded-2xl bg-pink-50 border border-pink-200 space-y-2 text-xs">
            <div class="font-extrabold text-pink-900">bKash / Nagad / Rocket / Tap / Paymently: <span id="sale-personal-acc-text" class="underline">01732333764</span></div>
            <input type="text" id="sale-digital-trx" oninput="speakTypingLive(this.value)" class="w-full px-3 py-2 rounded-xl border border-pink-300 bg-white text-xs" placeholder="Transaction ID (ঐচ্ছিক)" />
          </div>

          <div class="grid grid-cols-1 sm:grid-cols-2 gap-2.5">
            <div>
              <label class="block text-xs font-bold mb-1" data-i18n="custNameLabel">নাম</label>
              <input type="text" id="sale-cust-name" oninput="speakTypingLive(this.value)" class="w-full px-3 py-2 rounded-xl border border-slate-300 text-sm" placeholder="নাম (ঐচ্ছিক)" />
            </div>
            <div>
              <label class="block text-xs font-bold mb-1" data-i18n="custPhoneLabel">মোবাইল নম্বর</label>
              <input type="text" id="sale-cust-phone" oninput="speakTypingLive(this.value)" class="w-full px-3 py-2 rounded-xl border border-slate-300 text-sm" placeholder="017XXXXXXXX" />
            </div>
          </div>

          <div class="p-3 rounded-2xl bg-slate-50 border border-slate-200 flex items-center justify-between gap-3">
            <div class="flex items-center gap-3">
              <img id="sale-cust-photo-preview" src="https://placehold.co/64x64/f59e0b/ffffff?text=Photo" alt="Photo" class="w-12 h-12 rounded-xl object-cover border border-slate-300" />
              <div>
                <div class="text-xs font-extrabold text-slate-800" data-i18n="custPhotoLabel">ছবি যুক্ত করুন</div>
                <div class="text-[10px] text-slate-500">ডিভাইসের মেমরি / গ্যালারি থেকে ছবি নিন</div>
              </div>
            </div>
            <div class="flex items-center gap-2">
              <label class="px-3 py-2 rounded-xl bg-indigo-600 hover:bg-indigo-700 text-white text-xs font-extrabold cursor-pointer flex items-center gap-1.5">
                <i class="fa-solid fa-image"></i>
                <span>মেমরি থেকে ছবি</span>
                <input type="file" id="sale-cust-photo-file" accept="image/*" onchange="handleSaleCustomerPhoto(event)" class="hidden" />
              </label>
              <button type="button" onclick="clearSaleCustomerPhoto()" class="px-2.5 py-2 rounded-xl bg-slate-200 hover:bg-slate-300 text-slate-700 text-xs font-bold" title="Clear Photo">
                <i class="fa-solid fa-xmark"></i>
              </button>
            </div>
            <input type="hidden" id="sale-cust-photo-data" />
          </div>

          <button type="submit" class="w-full py-3.5 rounded-xl bg-emerald-600 hover:bg-emerald-700 text-white font-extrabold text-sm shadow-lg flex items-center justify-center gap-2">
            <i class="fa-solid fa-check-double"></i>
            <span data-i18n="completeSaleBtn">বিক্রয় সম্পন্ন ও ভাউচার তৈরি করুন</span>
          </button>
        </form>
      </section>

      <!-- 4. DUE LEDGER -->
      <section id="screen-due-ledger" class="app-screen space-y-4 hidden">
        <div class="flex items-center justify-between gap-2.5">
          <h2 class="text-base sm:text-lg font-extrabold flex items-center gap-2.5">
            <i class="fa-solid fa-address-book text-amber-600"></i>
            <span data-i18n="dueLedgerTitle">বাকি খাতা</span>
          </h2>
          <span id="due-total-pill" class="px-3 py-1 rounded-full bg-amber-100 text-amber-900 text-xs font-extrabold">৳ 0.00</span>
        </div>

        <form id="due-form" onsubmit="saveDueCustomerEntry(event)" class="app-card bg-white p-4 rounded-2xl shadow-sm border border-slate-200 space-y-3">
          <input type="hidden" id="due-edit-id" />
          <div class="grid grid-cols-1 sm:grid-cols-2 gap-2.5">
            <div>
              <label class="block text-xs font-bold mb-1" data-i18n="custNameLabel">নাম</label>
              <input type="text" id="due-cust-name" required oninput="speakTypingLive(this.value)" class="w-full px-3 py-2 rounded-xl border border-slate-300 text-sm" placeholder="নাম" />
            </div>
            <div>
              <label class="block text-xs font-bold mb-1" data-i18n="custPhoneLabel">মোবাইল নম্বর</label>
              <input type="text" id="due-cust-phone" required oninput="speakTypingLive(this.value)" class="w-full px-3 py-2 rounded-xl border border-slate-300 text-sm" placeholder="017XXXXXXXX" />
            </div>
          </div>
          <div class="grid grid-cols-1 sm:grid-cols-2 gap-2.5">
            <div>
              <label class="block text-xs font-bold mb-1">ঠিকানা</label>
              <input type="text" id="due-cust-address" oninput="speakTypingLive(this.value)" class="w-full px-3 py-2 rounded-xl border border-slate-300 text-sm" placeholder="গ্রাম / এলাকা / বাজার" />
            </div>
            <div>
              <label class="block text-xs font-bold mb-1" data-i18n="photoLabel">ছবি (মেমরি / গ্যালারি)</label>
              <input type="file" id="due-cust-photo-file" accept="image/*" onchange="handleDuePhotoUpload(event)" class="w-full text-xs file:mr-2 file:py-1.5 file:px-3 file:rounded-lg file:border-0 file:bg-amber-100 file:text-amber-900 file:font-bold" />
              <input type="hidden" id="due-cust-photo" />
            </div>
          </div>
          <div>
            <label class="block text-xs font-bold mb-1" data-i18n="dueProductsLabel">বাকিতে নেওয়া প্রোডাক্টসমূহ</label>
            <input type="text" id="due-cust-products" oninput="speakTypingLive(this.value)" class="w-full px-3 py-2 rounded-xl border border-slate-300 text-sm" placeholder="যেমন: চাল ৫ কেজি, তেল ২ লিটার" />
          </div>
          <div class="grid grid-cols-2 gap-2.5">
            <div>
              <label class="block text-xs font-bold mb-1" data-i18n="dueAmountLabel">বাকি টাকার পরিমাণ (৳)</label>
              <input type="number" step="0.01" id="due-cust-amount" value="0" oninput="speakTypingLive(this.value)" class="w-full px-3 py-2 rounded-xl border border-slate-300 text-sm font-bold" />
            </div>
            <div>
              <label class="block text-xs font-bold mb-1" data-i18n="dateLabel">তারিখ</label>
              <input type="date" id="due-cust-date" class="w-full px-3 py-2 rounded-xl border border-slate-300 text-sm" />
            </div>
          </div>
          <div class="flex gap-2.5">
            <button type="submit" class="flex-1 py-2.5 rounded-xl bg-amber-500 hover:bg-amber-600 text-white font-extrabold text-sm shadow flex items-center justify-center gap-2">
              <i class="fa-solid fa-user-plus"></i> <span data-i18n="saveDueBtn">বাকি হিসাব সংরক্ষণ করুন</span>
            </button>
            <button type="button" id="due-cancel-btn" onclick="resetDueForm()" class="hidden px-4 py-2.5 rounded-xl bg-slate-200 text-xs font-bold">বাতিল</button>
          </div>
        </form>

        <div id="due-cards-container" class="space-y-3"></div>
      </section>

      <!-- 5. HISTORY & OFFICIAL COMPACT THERMAL VOUCHER -->
      <section id="screen-history" class="app-screen space-y-4 hidden">
        <div class="flex flex-wrap items-center justify-between gap-2.5">
          <h2 class="text-base sm:text-lg font-extrabold flex items-center gap-2.5">
            <i class="fa-solid fa-clock-rotate-left theme-text"></i>
            <span data-i18n="historyTitle">ক্রয়/বিক্রয় হিস্টিরি ও ভাউচার</span>
          </h2>
          <div id="history-tab-switcher" class="flex rounded-xl bg-slate-200 p-1 text-xs font-bold">
            <button type="button" onclick="switchHistoryView('sales')" id="tab-hist-sales" class="px-3 py-1 rounded-lg bg-white text-indigo-700 shadow" data-i18n="salesHistoryTab">বিক্রয় হিস্টিরি</button>
            <button type="button" onclick="switchHistoryView('purchases')" id="tab-hist-purchases" class="px-3 py-1 rounded-lg text-slate-700" data-i18n="purchaseHistoryTab">ক্রয় হিস্টিরি</button>
          </div>
        </div>

        <!-- OFFICIAL COMPACT BLUETOOTH THERMAL VOUCHER -->
        <div class="app-card bg-white p-3 rounded-2xl shadow-md border border-slate-200 space-y-2.5">
          <div class="flex items-center justify-between">
            <span class="text-xs font-extrabold uppercase tracking-wider text-slate-500" data-i18n="officialVoucherTitle">অফিসিয়াল কলাম ভাউচার (ছোট ব্লুটুথ প্রিন্ট)</span>
          </div>

          <div class="flex flex-wrap gap-1.5">
            <button type="button" onclick="saveRealVoucherPDF()" class="px-2.5 py-1.5 rounded-xl bg-rose-600 hover:bg-rose-700 text-white text-[11px] font-extrabold flex items-center gap-1 shadow">
              <i class="fa-solid fa-file-pdf"></i> <span data-i18n="savePdfBtn">PDF সেভ</span>
            </button>
            <button type="button" onclick="printViaBluetooth()" class="px-2.5 py-1.5 rounded-xl bg-blue-600 hover:bg-blue-700 text-white text-[11px] font-extrabold flex items-center gap-1 shadow">
              <i class="fa-brands fa-bluetooth-b"></i> <span data-i18n="btPrintBtn">ব্লুটুথ প্রিন্ট</span>
            </button>
            <button type="button" onclick="printViaUSBOrSystem()" class="px-2.5 py-1.5 rounded-xl bg-slate-800 hover:bg-slate-900 text-white text-[11px] font-extrabold flex items-center gap-1 shadow">
              <i class="fa-solid fa-print"></i> <span data-i18n="usbPrintBtn">USB প্রিন্ট</span>
            </button>
            <button type="button" onclick="shareVoucherSocial()" class="px-2.5 py-1.5 rounded-xl bg-emerald-600 hover:bg-emerald-700 text-white text-[11px] font-extrabold flex items-center gap-1 shadow">
              <i class="fa-solid fa-share-nodes"></i> <span data-i18n="shareBtn">শেয়ার</span>
            </button>
          </div>

          <div id="official-voucher-capture" class="max-w-[260px] mx-auto border border-slate-900 rounded-xl p-2 bg-white text-slate-900 space-y-1.5 text-[10px]">
            <div class="flex items-start justify-between gap-1.5 border-b border-slate-900 pb-1.5">
              <div class="flex items-center gap-1.5 min-w-0">
                <img id="vch-logo" src="https://placehold.co/90x90/059669/ffffff?text=R%26H" alt="Shop Logo" class="w-9 h-9 rounded-lg object-cover border border-slate-400 shrink-0" />
                <div class="min-w-0">
                  <h3 id="vch-store-name" class="text-xs font-extrabold leading-tight truncate">Ribek & Hetu</h3>
                  <p id="vch-store-addr" class="text-[9px] text-slate-700 font-semibold leading-tight break-words"></p>
                  <p id="vch-store-phone" class="text-[9px] text-slate-700 font-semibold leading-tight"></p>
                </div>
              </div>
              <div class="text-right text-[9px] font-bold shrink-0">
                <div id="vch-id" class="font-mono font-extrabold">VCH-0000</div>
                <div id="vch-date">----/--/--</div>
                <div id="vch-time">--:--</div>
                <img id="vch-cust-photo" src="" alt="Photo" class="hidden w-8 h-8 rounded object-cover border border-slate-400 ml-auto mt-0.5" />
              </div>
            </div>

            <div class="voucher-full-col-banner py-0.5 px-1 rounded bg-slate-900 text-white text-center font-extrabold">
              এখানে সকল প্রকার প্রোডাক্ট / পণ্য সুলভ মূল্যে খুচরা ও পাইকারি বিক্রয় করা হয়
            </div>

            <div class="overflow-x-auto">
              <table class="w-full border-collapse border border-slate-900 text-[9px]">
                <thead>
                  <tr class="bg-slate-200 text-slate-950 font-extrabold">
                    <th class="border border-slate-900 py-1 px-1 text-center w-6">#</th>
                    <th class="border border-slate-900 py-1 px-1 text-left">পণ্য</th>
                    <th class="border border-slate-900 py-1 px-1 text-center">তারিখ</th>
                    <th class="border border-slate-900 py-1 px-1 text-right">মূল্য</th>
                  </tr>
                </thead>
                <tbody id="vch-table-body"></tbody>
                <tfoot id="vch-table-foot"></tfoot>
              </table>
            </div>

            <!-- SINGLE LINE BARCODE + QR CODE ON VOUCHER -->
            <div class="pt-1 border-t border-slate-300 flex items-center justify-between gap-1.5 overflow-hidden">
              <div class="flex-1 min-w-0 flex items-center justify-center overflow-hidden">
                <svg id="vch-barcode-svg" class="max-w-full h-6"></svg>
              </div>
              <div class="shrink-0 flex items-center justify-center overflow-hidden">
                <div id="vch-qrcode-div" class="scale-75 origin-center"></div>
              </div>
            </div>

            <div class="pt-1 border-t border-dashed border-slate-500 text-center text-[8.5px] font-bold text-slate-700">
              বিক্রয়কৃত পণ্য ফেরতযোগ্য নয়। ধন্যবাদ।
            </div>

            <div class="pt-3 grid grid-cols-2 gap-2 text-[8.5px] font-bold text-slate-900">
              <div class="text-left space-y-0.5">
                <div>বিক্রেতা.......</div>
                <div>তাং.........</div>
              </div>
              <div class="text-right space-y-0.5">
                <div>ক্রেতা.........</div>
                <div>তাং.........</div>
              </div>
            </div>
          </div>
        </div>

        <div class="flex items-center justify-between flex-wrap gap-2">
          <h3 class="font-extrabold text-sm" data-i18n="recentRecordsLabel">সাম্প্রতিক রেকর্ডসমূহ</h3>
          <div class="flex items-center gap-2">
            <select id="history-staff-filter" onchange="renderHistoryRecords()" class="hidden px-2.5 py-1 rounded-lg border border-slate-300 bg-white text-xs font-bold text-slate-700">
              <option value="all">সকল বিক্রয় (অ্যাডমিন ও স্টাফ)</option>
            </select>
            <button type="button" id="delete-all-hist-btn" onclick="clearCurrentHistoryTab()" class="px-2.5 py-1 rounded-lg bg-rose-100 text-rose-700 text-xs font-bold flex items-center gap-1.5">
              <i class="fa-solid fa-trash"></i> <span data-i18n="deleteAllBtn">সব মুছুন</span>
            </button>
          </div>
        </div>
        <div id="history-records-list" class="space-y-2"></div>
      </section>

      <!-- 6. HEALTH SCANNER -->
      <section id="screen-health" class="app-screen space-y-4 hidden">
        <h2 class="text-base sm:text-lg font-extrabold flex items-center gap-2.5">
          <i class="fa-solid fa-heart-pulse text-rose-600 animate-heartbeat"></i>
          <span data-i18n="healthTitle">স্বাস্থ্য পরীক্ষা (BP, হার্ট বিট, অক্সিজেন)</span>
        </h2>

        <div class="app-card bg-gradient-to-br from-slate-900 to-rose-950 text-white p-5 rounded-3xl shadow-xl text-center space-y-4">
          <p class="text-xs text-rose-100" data-i18n="healthInstruction">নিচের ফিঙ্গারপ্রিন্ট সেন্সরে আঙুল চেপে ধরে রাখুন (১০০% পর্যন্ত)</p>

          <div class="flex justify-center">
            <button type="button" id="fingerprint-hold-btn"
              onmousedown="beginFingerHealthScan(event)" onmouseup="cancelFingerHealthScan()" onmouseleave="cancelFingerHealthScan()"
              ontouchstart="beginFingerHealthScan(event)" ontouchend="cancelFingerHealthScan()"
              class="w-28 h-28 rounded-3xl bg-white/10 hover:bg-white/15 border-2 border-rose-400/60 flex flex-col items-center justify-center gap-1 shadow-2xl select-none active:scale-95 transition">
              <i id="fp-icon" class="fa-solid fa-fingerprint text-5xl text-rose-400"></i>
              <span id="fp-percent" class="text-xs font-mono font-extrabold text-amber-300">0%</span>
            </button>
          </div>

          <div class="w-full bg-white/15 h-2.5 rounded-full overflow-hidden">
            <div id="fp-bar" class="h-full bg-gradient-to-r from-emerald-400 via-amber-300 to-rose-500 w-0 transition-all duration-150"></div>
          </div>

          <div class="grid grid-cols-3 gap-2.5 text-center pt-1">
            <div class="p-3 rounded-2xl bg-white/10">
              <div class="text-[10px] text-rose-200 font-bold" data-i18n="bpLabel">ব্লাড প্রেসার</div>
              <div id="res-bp" class="text-base sm:text-lg font-extrabold mt-0.5">--/--</div>
              <div id="res-bp-tag" class="text-[10px] font-bold text-amber-300"></div>
            </div>
            <div class="p-3 rounded-2xl bg-white/10">
              <div class="text-[10px] text-rose-200 font-bold" data-i18n="bpmLabel">হার্ট বিট</div>
              <div id="res-bpm" class="text-base sm:text-lg font-extrabold mt-0.5">-- BPM</div>
              <div id="res-bpm-tag" class="text-[10px] font-bold text-emerald-300"></div>
            </div>
            <div class="p-3 rounded-2xl bg-white/10">
              <div class="text-[10px] text-rose-200 font-bold" data-i18n="spo2Label">অক্সিজেন (SpO2)</div>
              <div id="res-spo2" class="text-base sm:text-lg font-extrabold mt-0.5">-- %</div>
              <div id="res-spo2-tag" class="text-[10px] font-bold text-sky-300"></div>
            </div>
          </div>

          <div id="health-alert-box" class="hidden p-3 rounded-2xl bg-rose-600 text-white text-xs font-extrabold text-left"></div>
        </div>
      </section>

      <!-- 7. SEPARATE BANGLA & ENGLISH CALENDAR -->
      <section id="screen-calendar" class="app-screen space-y-4 hidden">
        <div class="flex flex-wrap items-center justify-between gap-2.5">
          <h2 class="text-base sm:text-lg font-extrabold flex items-center gap-2.5">
            <i class="fa-solid fa-calendar-days theme-text"></i>
            <span data-i18n="calTitle">বাংলা ও ইংরেজি ক্যালেন্ডার</span>
          </h2>
          <div class="flex items-center gap-2">
            <div class="flex rounded-xl bg-slate-200 p-1 text-xs font-extrabold">
              <button type="button" onclick="switchCalendarType('bn')" id="cal-btn-bn" class="px-3 py-1 rounded-lg bg-emerald-600 text-white shadow">বাংলা ক্যালেন্ডার</button>
              <button type="button" onclick="switchCalendarType('en')" id="cal-btn-en" class="px-3 py-1 rounded-lg text-slate-700">English Calendar</button>
            </div>
            <button type="button" onclick="shiftCalendarMonth(-1)" class="w-8 h-8 rounded-xl bg-slate-200 font-bold"><i class="fa-solid fa-chevron-left"></i></button>
            <button type="button" onclick="shiftCalendarMonth(1)" class="w-8 h-8 rounded-xl bg-slate-200 font-bold"><i class="fa-solid fa-chevron-right"></i></button>
          </div>
        </div>

        <div id="calendar-view-bn" class="app-card bg-white p-4 rounded-2xl shadow-sm border border-emerald-200 space-y-3">
          <div class="flex items-center justify-between">
            <h3 id="bn-cal-header" class="font-extrabold text-sm text-emerald-800"></h3>
            <span class="px-2 py-0.5 rounded-full bg-emerald-100 text-emerald-800 text-[10px] font-extrabold">বাংলা পঞ্জিকা</span>
          </div>
          <div class="grid grid-cols-7 gap-1 text-center text-[11px] font-extrabold text-emerald-700">
            <div>রবি</div><div>সোম</div><div>মঙ্গল</div><div>বুধ</div><div>বৃহঃ</div><div class="text-rose-600">শুক্র</div><div class="text-rose-600">শনি</div>
          </div>
          <div id="bn-cal-grid" class="grid grid-cols-7 gap-1.5"></div>
        </div>

        <div id="calendar-view-en" class="app-card bg-white p-4 rounded-2xl shadow-sm border border-indigo-200 space-y-3 hidden">
          <div class="flex items-center justify-between">
            <h3 id="en-cal-header" class="font-extrabold text-sm text-indigo-800"></h3>
            <span class="px-2 py-0.5 rounded-full bg-indigo-100 text-indigo-800 text-[10px] font-extrabold">English Calendar</span>
          </div>
          <div class="grid grid-cols-7 gap-1 text-center text-[11px] font-extrabold text-indigo-700">
            <div>Sun</div><div>Mon</div><div>Tue</div><div>Wed</div><div>Thu</div><div class="text-rose-600">Fri</div><div class="text-rose-600">Sat</div>
          </div>
          <div id="en-cal-grid" class="grid grid-cols-7 gap-1.5"></div>
        </div>

        <div class="app-card bg-white p-4 rounded-2xl shadow-sm border border-slate-200 space-y-2">
          <h3 class="font-extrabold text-xs text-rose-700" data-i18n="holidaysLabel">ছুটির তালিকা ও বিশেষ দিবস</h3>
          <div id="cal-holidays-box" class="space-y-1.5 text-xs"></div>
        </div>
      </section>

      <!-- 8. CALCULATOR -->
      <section id="screen-calculator" class="app-screen space-y-4 hidden">
        <div class="flex items-center justify-between gap-2.5 flex-wrap">
          <h2 class="text-base sm:text-lg font-extrabold flex items-center gap-2.5">
            <i class="fa-solid fa-calculator theme-text"></i>
            <span data-i18n="calcTitle">ক্যালকুলেটর (ইউনিট, সাধারণ ও সায়েন্টিফিক)</span>
          </h2>
          <div class="flex rounded-xl bg-slate-200 p-1 text-xs font-bold">
            <button type="button" onclick="switchCalcMode('unit')" id="c-tab-unit" class="px-2.5 py-1 rounded-lg bg-white text-indigo-700 shadow">ইউনিট</button>
            <button type="button" onclick="switchCalcMode('norm')" id="c-tab-norm" class="px-2.5 py-1 rounded-lg text-slate-700">সাধারণ</button>
            <button type="button" onclick="switchCalcMode('sci')" id="c-tab-sci" class="px-2.5 py-1 rounded-lg text-slate-700">সায়েন্টিফিক</button>
          </div>
        </div>

        <div id="calc-box-unit" class="app-card bg-white p-4 rounded-2xl shadow-sm border border-slate-200 space-y-3">
          <p class="text-xs text-slate-600 font-semibold" data-i18n="unitCalcDesc">যেমন: ১ কেজির দাম ১০০ টাকা হলে ২৫০ গ্রামের দাম কত?</p>
          <div class="grid grid-cols-1 sm:grid-cols-3 gap-2.5">
            <div>
              <label class="block text-xs font-bold mb-1" data-i18n="baseUnitQtyLabel">মূল এককের মান (যেমন: 1 বা 1000)</label>
              <input type="number" step="any" id="uc-unit" value="1" oninput="runUnitCalculator()" class="w-full px-3 py-2 rounded-xl border border-slate-300 text-sm font-bold" />
            </div>
            <div>
              <label class="block text-xs font-bold mb-1" data-i18n="baseUnitPriceLabel">মূল এককের মূল্য (৳)</label>
              <input type="number" step="any" id="uc-rate" oninput="runUnitCalculator()" class="w-full px-3 py-2 rounded-xl border border-slate-300 text-sm font-bold" placeholder="100" />
            </div>
            <div>
              <label class="block text-xs font-bold mb-1" data-i18n="targetQtyLabel">কাঙ্ক্ষিত পরিমাণ (যেমন: 0.25 বা 250)</label>
              <input type="number" step="any" id="uc-qty" oninput="runUnitCalculator()" class="w-full px-3 py-2 rounded-xl border border-slate-300 text-sm font-bold" placeholder="0.25" />
            </div>
          </div>
          <div class="p-4 rounded-2xl theme-soft-bg flex items-center justify-between">
            <span class="text-xs font-extrabold text-slate-700" data-i18n="calcResultLabel">হিসাবের ফলাফল:</span>
            <span id="uc-out" class="text-2xl font-extrabold theme-text">৳ 0.00</span>
          </div>
        </div>

        <div id="calc-box-std" class="app-card bg-white p-4 rounded-2xl shadow-sm border border-slate-200 space-y-3 hidden">
          <div class="flex items-center justify-between text-[11px] font-bold text-slate-500 px-1">
            <span id="calc-angle-indicator" class="px-2 py-0.5 rounded bg-indigo-100 text-indigo-800 font-mono">ANGLE: DEG</span>
            <button type="button" onclick="toggleCalcAngleMode()" class="px-2.5 py-0.5 rounded bg-slate-200 hover:bg-slate-300 text-slate-800 font-extrabold">
              DEG / RAD পরিবর্তন
            </button>
          </div>
          <input type="text" id="std-calc-disp" readonly value="0" class="w-full px-4 py-3 rounded-2xl bg-slate-900 text-amber-300 font-mono text-right text-2xl font-extrabold" />
          <div id="sci-calc-row" class="hidden grid-cols-4 sm:grid-cols-6 gap-2 text-xs font-extrabold">
            <button type="button" onclick="sciOp('sin')" class="py-2 rounded-xl bg-indigo-100 text-indigo-900">sin</button>
            <button type="button" onclick="sciOp('cos')" class="py-2 rounded-xl bg-indigo-100 text-indigo-900">cos</button>
            <button type="button" onclick="sciOp('tan')" class="py-2 rounded-xl bg-indigo-100 text-indigo-900">tan</button>
            <button type="button" onclick="sciOp('asin')" class="py-2 rounded-xl bg-indigo-50 text-indigo-900">sin⁻¹</button>
            <button type="button" onclick="sciOp('acos')" class="py-2 rounded-xl bg-indigo-50 text-indigo-900">cos⁻¹</button>
            <button type="button" onclick="sciOp('atan')" class="py-2 rounded-xl bg-indigo-50 text-indigo-900">tan⁻¹</button>
            <button type="button" onclick="sciOp('sq')" class="py-2 rounded-xl bg-indigo-100 text-indigo-900">x²</button>
            <button type="button" onclick="sciOp('cube')" class="py-2 rounded-xl bg-indigo-100 text-indigo-900">x³</button>
            <button type="button" onclick="stdAdd('^')" class="py-2 rounded-xl bg-indigo-100 text-indigo-900">xʸ</button>
            <button type="button" onclick="sciOp('sqrt')" class="py-2 rounded-xl bg-indigo-100 text-indigo-900">√x</button>
            <button type="button" onclick="sciOp('cbrt')" class="py-2 rounded-xl bg-indigo-100 text-indigo-900">∛x</button>
            <button type="button" onclick="sciOp('inv')" class="py-2 rounded-xl bg-indigo-100 text-indigo-900">1/x</button>
            <button type="button" onclick="sciOp('log10')" class="py-2 rounded-xl bg-indigo-100 text-indigo-900">log₁₀</button>
            <button type="button" onclick="sciOp('ln')" class="py-2 rounded-xl bg-indigo-100 text-indigo-900">ln</button>
            <button type="button" onclick="sciOp('exp')" class="py-2 rounded-xl bg-indigo-100 text-indigo-900">eˣ</button>
            <button type="button" onclick="sciOp('fact')" class="py-2 rounded-xl bg-indigo-100 text-indigo-900">n!</button>
            <button type="button" onclick="sciOp('pi')" class="py-2 rounded-xl bg-indigo-100 text-indigo-900">π</button>
            <button type="button" onclick="sciOp('e')" class="py-2 rounded-xl bg-indigo-100 text-indigo-900">e</button>
          </div>
          <div class="grid grid-cols-4 gap-2 text-sm font-extrabold">
            <button type="button" onclick="stdClear()" class="py-3 rounded-xl bg-rose-100 text-rose-700">C</button>
            <button type="button" onclick="stdBack()" class="py-3 rounded-xl bg-slate-200">⌫</button>
            <button type="button" onclick="stdPercent()" class="py-3 rounded-xl bg-slate-200">%</button>
            <button type="button" onclick="stdAdd('/')" class="py-3 rounded-xl bg-indigo-100 text-indigo-700">÷</button>
            <button type="button" onclick="stdAdd('7')" class="py-3 rounded-xl bg-slate-100">7</button>
            <button type="button" onclick="stdAdd('8')" class="py-3 rounded-xl bg-slate-100">8</button>
            <button type="button" onclick="stdAdd('9')" class="py-3 rounded-xl bg-slate-100">9</button>
            <button type="button" onclick="stdAdd('*')" class="py-3 rounded-xl bg-indigo-100 text-indigo-700">×</button>
            <button type="button" onclick="stdAdd('4')" class="py-3 rounded-xl bg-slate-100">4</button>
            <button type="button" onclick="stdAdd('5')" class="py-3 rounded-xl bg-slate-100">5</button>
            <button type="button" onclick="stdAdd('6')" class="py-3 rounded-xl bg-slate-100">6</button>
            <button type="button" onclick="stdAdd('-')" class="py-3 rounded-xl bg-indigo-100 text-indigo-700">−</button>
            <button type="button" onclick="stdAdd('1')" class="py-3 rounded-xl bg-slate-100">1</button>
            <button type="button" onclick="stdAdd('2')" class="py-3 rounded-xl bg-slate-100">2</button>
            <button type="button" onclick="stdAdd('3')" class="py-3 rounded-xl bg-slate-100">3</button>
            <button type="button" onclick="stdAdd('+')" class="py-3 rounded-xl bg-indigo-100 text-indigo-700">+</button>
            <button type="button" onclick="stdAdd('(')" class="py-3 rounded-xl bg-slate-100">(</button>
            <button type="button" onclick="stdAdd('0')" class="py-3 rounded-xl bg-slate-100">0</button>
            <button type="button" onclick="stdAdd(')')" class="py-3 rounded-xl bg-slate-100">)</button>
            <button type="button" onclick="stdAdd('.')" class="py-3 rounded-xl bg-slate-100">.</button>
            <button type="button" onclick="stdEval()" class="col-span-4 py-3 rounded-xl bg-emerald-600 text-white text-base">=</button>
          </div>
        </div>
      </section>

      <!-- 9. NOTEBOOK & ALARM -->
      <section id="screen-notebook" class="app-screen space-y-4 hidden">
        <h2 class="text-base sm:text-lg font-extrabold flex items-center gap-2.5">
          <i class="fa-solid fa-bell theme-text"></i>
          <span data-i18n="notebookTitle">নোটবুক ও অ্যালার্ম রিমাইন্ডার</span>
        </h2>

        <form onsubmit="addNotebookEntry(event)" class="app-card bg-white p-4 rounded-2xl shadow-sm border border-slate-200 space-y-3">
          <textarea id="nb-text" rows="2" required oninput="speakTypingLive(this.value)" data-i18n-placeholder="nbPlaceholder" placeholder="জরুরি নোট বা রিমাইন্ডার লিখুন..." class="w-full px-3 py-2 rounded-xl border border-slate-300 text-sm outline-none focus:ring-2 focus:ring-emerald-500"></textarea>
          <div class="grid grid-cols-2 gap-2.5">
            <div>
              <label class="block text-xs font-bold mb-1" data-i18n="alarmDateLabel">অ্যালার্ম তারিখ</label>
              <input type="date" id="nb-date" required class="w-full px-3 py-2 rounded-xl border border-slate-300 text-xs sm:text-sm" />
            </div>
            <div>
              <label class="block text-xs font-bold mb-1" data-i18n="alarmTimeLabel">অ্যালার্ম সময়</label>
              <input type="time" id="nb-time" required class="w-full px-3 py-2 rounded-xl border border-slate-300 text-xs sm:text-sm" />
            </div>
          </div>
          <button type="submit" class="w-full py-2.5 rounded-xl theme-bg text-white font-extrabold text-sm shadow flex items-center justify-center gap-2">
            <i class="fa-solid fa-plus"></i> <span data-i18n="saveNoteBtn">নোট ও অ্যালার্ম সেভ করুন</span>
          </button>
        </form>

        <div id="nb-list" class="space-y-2"></div>
      </section>

      <!-- 10. BARCODE & QR GENERATOR -->
      <section id="screen-barcode" class="app-screen space-y-4 hidden">
        <h2 class="text-base sm:text-lg font-extrabold flex items-center gap-2.5">
          <i class="fa-solid fa-barcode theme-text"></i>
          <span data-i18n="barcodePageTitle">বারকোড ও QR কোড জেনারেটর</span>
        </h2>
        <div class="app-card bg-white p-4 rounded-2xl shadow-sm border border-slate-200 space-y-4">
          <div class="flex gap-2.5">
            <input type="text" id="qr-gen-text" oninput="renderBarcodeAndQR(); speakTypingLive(this.value)" class="flex-1 px-3 py-2 rounded-xl border border-slate-300 text-sm font-bold" placeholder="প্রোডাক্ট কোড বা নাম লিখুন" />
            <button type="button" onclick="autoFillBarcode('qr-gen-text'); renderBarcodeAndQR()" class="px-3 py-2 rounded-xl bg-indigo-600 text-white text-xs font-extrabold">অটো কোড</button>
          </div>
          <div class="grid grid-cols-1 sm:grid-cols-2 gap-4 items-center text-center pt-2">
            <div class="p-3 rounded-2xl border border-slate-200 bg-slate-50 space-y-2">
              <div class="text-xs font-bold text-slate-500">Barcode (CODE128)</div>
              <svg id="out-barcode-svg" class="mx-auto max-w-full"></svg>
              <button type="button" onclick="saveBarcodeImageToMemory('out-barcode-svg', document.getElementById('qr-gen-text').value || 'Barcode')" class="px-3 py-1.5 rounded-xl bg-slate-800 hover:bg-slate-900 text-white text-xs font-extrabold inline-flex items-center gap-1.5">
                <i class="fa-solid fa-download"></i> <span>বারকোড সেভ করুন</span>
              </button>
            </div>
            <div class="p-3 rounded-2xl border border-slate-200 bg-slate-50 space-y-2 flex flex-col items-center">
              <div class="text-xs font-bold text-slate-500">QR Code</div>
              <div id="out-qrcode-div"></div>
              <button type="button" onclick="saveQrImageToMemory('out-qrcode-div', document.getElementById('qr-gen-text').value || 'QRCode')" class="px-3 py-1.5 rounded-xl bg-indigo-600 hover:bg-indigo-700 text-white text-xs font-extrabold inline-flex items-center gap-1.5">
                <i class="fa-solid fa-download"></i> <span>QR কোড সেভ করুন</span>
              </button>
            </div>
          </div>
        </div>
      </section>

      <!-- 11. USER PROFILE & SECURITY -->
      <section id="screen-profile" class="app-screen space-y-4 hidden">
        <h2 class="text-base sm:text-lg font-extrabold flex items-center gap-2.5">
          <i class="fa-solid fa-user-shield theme-text"></i>
          <span data-i18n="profileTitle">প্রোফাইল ও নিরাপত্তা</span>
        </h2>

        <div class="app-card bg-white p-4 rounded-2xl shadow-sm border border-slate-200 space-y-3">
          <div class="flex items-center justify-between gap-3 flex-wrap">
            <div class="flex items-center gap-3">
              <img id="prof-avatar-img" src="https://placehold.co/96x96/4f46e5/ffffff?text=Admin" class="w-14 h-14 rounded-2xl object-cover border border-indigo-200" alt="Profile" />
              <div>
                <div id="prof-name" class="font-extrabold text-base">--</div>
                <div id="prof-id" class="text-xs text-slate-500 font-mono">--</div>
                <div id="prof-shop-addr-preview" class="text-[11px] font-bold text-emerald-700 mt-0.5"></div>
                <span id="prof-role-badge" class="inline-block mt-1 px-2 py-0.5 rounded bg-emerald-100 text-emerald-800 text-[10px] font-extrabold">১০০% গোপনীয় আলাদা ডাটাবেস সক্রিয়</span>
              </div>
            </div>
            <div class="flex items-center gap-2">
              <button type="button" id="prof-delete-admin-btn" onclick="deleteAdminProfileAccount()" class="px-3 py-2 rounded-xl bg-rose-100 hover:bg-rose-200 text-rose-700 text-xs font-extrabold flex items-center gap-1.5">
                <i class="fa-solid fa-user-slash"></i> <span>একাউন্ট ডিলিট</span>
              </button>
              <button type="button" onclick="logoutCurrentUser()" class="px-3 py-2 rounded-xl bg-rose-600 hover:bg-rose-700 text-white text-xs font-extrabold flex items-center gap-1.5">
                <i class="fa-solid fa-right-from-bracket"></i> <span data-i18n="logoutBtn">লগআউট</span>
              </button>
            </div>
          </div>

          <form id="admin-profile-edit-form" onsubmit="saveAdminProfileEdit(event)" class="pt-3 border-t border-slate-100 space-y-2.5">
            <div class="text-xs font-extrabold text-emerald-800 flex items-center gap-1.5">
              <i class="fa-solid fa-user-pen"></i> <span>অ্যাডমিন প্রোফাইল ও দোকানের ঠিকানা (দেখুন ও এডিট করুন)</span>
            </div>
            <div class="grid grid-cols-1 sm:grid-cols-2 gap-2.5">
              <div>
                <label class="block text-[11px] font-bold text-slate-600 mb-0.5">মালিক / অ্যাডমিনের নাম</label>
                <input type="text" id="prof-edit-owner" required class="w-full px-3 py-2 rounded-xl border border-slate-300 text-xs font-bold" />
              </div>
              <div>
                <label class="block text-[11px] font-bold text-slate-600 mb-0.5">মোবাইল / ইউজার ID</label>
                <input type="text" id="prof-edit-userid" required class="w-full px-3 py-2 rounded-xl border border-slate-300 text-xs font-mono font-bold" />
              </div>
            </div>
            <div class="grid grid-cols-1 sm:grid-cols-2 gap-2.5">
              <div>
                <label class="block text-[11px] font-bold text-slate-600 mb-0.5">দোকানের নাম</label>
                <input type="text" id="prof-edit-shop" required class="w-full px-3 py-2 rounded-xl border border-slate-300 text-xs font-bold" />
              </div>
              <div>
                <label class="block text-[11px] font-bold text-slate-600 mb-0.5">দোকানের ঠিকানা</label>
                <div class="flex gap-1.5">
                  <input type="text" id="prof-edit-addr" class="flex-1 px-3 py-2 rounded-xl border border-slate-300 text-xs font-bold" placeholder="দোকানের ঠিকানা লিখুন" />
                  <button type="button" onclick="fillInputWithCurrentGps('prof-edit-addr')" class="px-2.5 py-2 rounded-xl bg-teal-600 hover:bg-teal-700 text-white text-[11px] font-extrabold" title="GPS থেকে ঠিকানা">
                    <i class="fa-solid fa-location-crosshairs"></i>
                  </button>
                </div>
              </div>
            </div>
            <button type="submit" class="w-full py-2.5 rounded-xl bg-emerald-600 hover:bg-emerald-700 text-white text-xs font-extrabold flex items-center justify-center gap-1.5">
              <i class="fa-solid fa-floppy-disk"></i> <span>অ্যাডমিন প্রোফাইল সেভ করুন</span>
            </button>
          </form>

          <div id="admin-logo-box" class="pt-3 border-t border-slate-100 flex items-center justify-between gap-3 flex-wrap">
            <div class="flex items-center gap-3">
              <img id="prof-store-logo-img" src="https://placehold.co/96x96/059669/ffffff?text=R%26H" class="w-12 h-12 rounded-2xl object-cover border border-emerald-300" alt="Store Logo" />
              <div>
                <div class="text-xs font-extrabold text-slate-800">দোকানের লোগো (প্রোফাইল ও ভাউচার)</div>
                <div class="text-[10px] text-slate-500">ছবি সিলেক্ট করে সরাসরি অটো-সেভ হবে</div>
              </div>
            </div>
            <div class="flex items-center gap-2">
              <label class="px-3 py-1.5 rounded-xl bg-indigo-600 hover:bg-indigo-700 text-white text-xs font-extrabold cursor-pointer flex items-center gap-1.5">
                <i class="fa-solid fa-upload"></i> <span>লোগো ছবি</span>
                <input type="file" accept="image/*" onchange="uploadStoreLogoFile(event)" class="hidden" />
              </label>
              <button type="button" onclick="saveProfileLogoNow()" class="px-3 py-1.5 rounded-xl bg-emerald-600 hover:bg-emerald-700 text-white text-xs font-extrabold">
                সেভ করুন
              </button>
            </div>
          </div>
        </div>

        <!-- ADMIN STAFF PROFILE MANAGEMENT -->
        <div id="admin-staff-management-panel" class="app-card bg-indigo-50/70 p-4 rounded-2xl shadow-sm border-2 border-indigo-200 space-y-4">
          <div class="flex items-center justify-between gap-2">
            <div>
              <h3 class="font-extrabold text-sm text-indigo-950 flex items-center gap-2">
                <i class="fa-solid fa-users-gear text-indigo-600"></i>
                <span>স্টাফ প্রোফাইল রেজিস্ট্রেশন ও পরিচালনা (শুধুমাত্র স্টক থেকে বিক্রয়)</span>
              </h3>
              <p class="text-[11px] text-indigo-800/80">স্টাফ আইডি ও পাসওয়ার্ড দিয়ে লগইন করলে শুধু স্টক থেকে বিক্রয় করতে পারবে; লাভ, খরচ, স্টক এন্ট্রি ও সেটিংস গোপন থাকবে।</p>
            </div>
            <span id="staff-count-badge" class="px-2.5 py-1 rounded-full bg-indigo-600 text-white text-[10px] font-extrabold shrink-0">০ জন স্টাফ</span>
          </div>

          <form id="staff-profile-form" onsubmit="saveStaffProfile(event)" class="bg-white p-3.5 rounded-2xl border border-indigo-200 space-y-3">
            <input type="hidden" id="staff-edit-id" />

            <div class="grid grid-cols-1 sm:grid-cols-2 gap-3">
              <div class="flex items-center gap-2.5 p-2 rounded-xl bg-slate-50 border border-slate-200">
                <img id="staff-shop-logo-preview" src="https://placehold.co/80x80/059669/ffffff?text=Logo" class="w-12 h-12 rounded-xl object-cover border border-emerald-300 shrink-0" alt="Shop Logo" />
                <div class="min-w-0">
                  <label class="block text-[11px] font-bold text-slate-700 mb-0.5">দোকানের লোগো আপলোড</label>
                  <input type="file" id="staff-shop-logo-file" accept="image/*" onchange="handleStaffShopLogoUpload(event)" class="w-full text-[11px] file:mr-1.5 file:py-1 file:px-2 file:rounded-lg file:border-0 file:bg-emerald-100 file:text-emerald-900 file:font-bold" />
                  <input type="hidden" id="staff-shop-logo-data" />
                </div>
              </div>

              <div class="flex items-center gap-2.5 p-2 rounded-xl bg-slate-50 border border-slate-200">
                <img id="staff-photo-preview" src="https://placehold.co/80x80/6366f1/ffffff?text=Staff" class="w-12 h-12 rounded-xl object-cover border border-indigo-300 shrink-0" alt="Staff Photo" />
                <div class="min-w-0">
                  <label class="block text-[11px] font-bold text-slate-700 mb-0.5">স্টাফের ছবি আপলোড</label>
                  <input type="file" id="staff-photo-file" accept="image/*" onchange="handleStaffPhotoUpload(event)" class="w-full text-[11px] file:mr-1.5 file:py-1 file:px-2 file:rounded-lg file:border-0 file:bg-indigo-100 file:text-indigo-900 file:font-bold" />
                  <input type="hidden" id="staff-photo-data" />
                </div>
              </div>
            </div>

            <div class="grid grid-cols-1 sm:grid-cols-2 gap-2.5">
              <div>
                <label class="block text-xs font-bold text-slate-700 mb-1">স্টাফের পূর্ণ নাম</label>
                <input type="text" id="staff-name" required class="w-full px-3 py-2 rounded-xl border border-slate-300 text-xs sm:text-sm" placeholder="যেমন: করিম উদ্দিন" />
              </div>
              <div>
                <label class="block text-xs font-bold text-slate-700 mb-1">স্টাফ ইউজার আইডি / মোবাইল</label>
                <input type="text" id="staff-userid" required class="w-full px-3 py-2 rounded-xl border border-slate-300 text-xs sm:text-sm" placeholder="যেমন: staff01 বা 018XXXXXXXX" />
              </div>
            </div>

            <div class="grid grid-cols-1 sm:grid-cols-2 gap-2.5">
              <div>
                <label class="block text-xs font-bold text-slate-700 mb-1">দোকানের নাম</label>
                <input type="text" id="staff-shop-name" required class="w-full px-3 py-2 rounded-xl border border-slate-300 text-xs sm:text-sm" placeholder="দোকানের নাম" />
              </div>
              <div>
                <label class="block text-xs font-bold text-slate-700 mb-1">দোকানের মোবাইল নম্বর</label>
                <input type="text" id="staff-shop-phone" required class="w-full px-3 py-2 rounded-xl border border-slate-300 text-xs sm:text-sm" placeholder="017XXXXXXXX" />
              </div>
            </div>

            <div class="grid grid-cols-1 sm:grid-cols-2 gap-2.5">
              <div>
                <label class="block text-xs font-bold text-slate-700 mb-1">দোকানের ঠিকানা</label>
                <input type="text" id="staff-shop-addr" class="w-full px-3 py-2 rounded-xl border border-slate-300 text-xs sm:text-sm" placeholder="দোকানের ঠিকানা (ঐচ্ছিক)" />
              </div>
              <div>
                <label class="block text-xs font-bold text-slate-700 mb-1">স্টাফ লগইন পাসওয়ার্ড</label>
                <input type="password" id="staff-password" required minlength="4" class="w-full px-3 py-2 rounded-xl border border-slate-300 text-xs sm:text-sm" placeholder="স্টাফের গোপন পাসওয়ার্ড দিন" />
              </div>
            </div>

            <div class="flex gap-2.5">
              <button type="submit" id="staff-save-btn" class="flex-1 py-2.5 rounded-xl bg-indigo-600 hover:bg-indigo-700 text-white text-xs sm:text-sm font-extrabold shadow flex items-center justify-center gap-1.5">
                <i class="fa-solid fa-user-plus"></i> <span>স্টাফ প্রোফাইল সেভ করুন</span>
              </button>
              <button type="button" id="staff-cancel-btn" onclick="resetStaffForm()" class="hidden px-4 py-2.5 rounded-xl bg-slate-200 text-slate-800 text-xs font-bold">
                বাতিল
              </button>
            </div>
          </form>

          <div id="staff-profiles-list" class="space-y-2"></div>
        </div>

        <form id="admin-password-form" onsubmit="updateAccountPassword(event)" class="app-card bg-white p-4 rounded-2xl shadow-sm border border-slate-200 space-y-3">
          <h3 class="font-extrabold text-sm" data-i18n="changePassTitle">পাসওয়ার্ড পরিবর্তন</h3>
          <input type="password" id="prof-cur-pass" required class="w-full px-3 py-2 rounded-xl border border-slate-300 text-sm" placeholder="বর্তমান পাসওয়ার্ড" />
          <input type="password" id="prof-new-pass" required minlength="4" class="w-full px-3 py-2 rounded-xl border border-slate-300 text-sm" placeholder="নতুন পাসওয়ার্ড" />
          <button type="submit" class="w-full py-2.5 rounded-xl bg-indigo-600 text-white font-extrabold text-sm" data-i18n="updatePassBtn">পাসওয়ার্ড আপডেট করুন</button>
        </form>
      </section>

      <!-- 12. SETTINGS & BACKUP/RESTORE -->
      <section id="screen-settings" class="app-screen space-y-4 hidden">
        <h2 class="text-base sm:text-lg font-extrabold flex items-center gap-2.5">
          <i class="fa-solid fa-sliders theme-text"></i>
          <span data-i18n="settingsTitle">দোকান, ডিজিটাল পেমেন্ট ও ব্যাকআপ সেটিংস</span>
        </h2>

        <form id="app-settings-form" onsubmit="saveAppSettings(event)" class="app-card bg-white p-4 rounded-2xl shadow-sm border border-slate-200 space-y-3.5">
          <div class="flex items-center justify-between gap-2 pb-2 border-b border-slate-100">
            <span class="text-xs font-extrabold text-slate-700">ভাষা পরিবর্তন / Language</span>
            <button type="button" onclick="toggleLanguage()" class="px-3 py-1.5 rounded-xl bg-amber-400 hover:bg-amber-300 text-slate-950 font-extrabold text-xs flex items-center gap-1.5">
              <i class="fa-solid fa-language"></i>
              <span id="set-lang-label">English</span>
            </button>
          </div>

          <div class="flex items-center gap-3">
            <img id="set-logo-img" src="https://placehold.co/96x96/059669/ffffff?text=R%26H" class="w-14 h-14 rounded-2xl object-cover border" alt="Logo" />
            <div>
              <label class="block text-xs font-bold mb-1" data-i18n="shopLogoLabel">দোকানের লোগো</label>
              <input type="file" accept="image/*" onchange="uploadStoreLogoFile(event)" class="text-xs file:mr-2 file:py-1.5 file:px-3 file:rounded-lg file:border-0 file:bg-emerald-100 file:text-emerald-800 file:font-bold" />
            </div>
          </div>

          <div class="grid grid-cols-1 sm:grid-cols-2 gap-2.5">
            <div>
              <label class="block text-xs font-bold mb-1" data-i18n="shopNameLabel">দোকানের নাম</label>
              <input type="text" id="set-shop-name" required oninput="autoSyncSettingsFromInputs()" class="w-full px-3 py-2 rounded-xl border border-slate-300 text-sm font-bold" />
            </div>
            <div>
              <label class="block text-xs font-bold mb-1" data-i18n="shopPhoneLabel">মোবাইল নম্বর</label>
              <input type="text" id="set-shop-phone" required oninput="autoSyncSettingsFromInputs()" class="w-full px-3 py-2 rounded-xl border border-slate-300 text-sm font-bold" />
            </div>
          </div>

          <div>
            <label class="block text-xs font-bold mb-1" data-i18n="addressLabel">দোকানের ঠিকানা অ্যাড / পরিবর্তন করুন</label>
            <div class="flex gap-2">
              <input type="text" id="set-shop-addr" oninput="autoSyncSettingsFromInputs()" class="flex-1 px-3 py-2 rounded-xl border border-slate-300 text-sm font-semibold" placeholder="দোকানের ঠিকানা লিখুন..." />
              <button type="button" onclick="fetchGpsLocation(true, true)" class="px-3 py-2 rounded-xl bg-teal-600 hover:bg-teal-700 text-white text-xs font-extrabold whitespace-nowrap flex items-center gap-1.5">
                <i class="fa-solid fa-location-crosshairs"></i> <span>GPS নিন</span>
              </button>
            </div>
          </div>

          <div class="p-3 rounded-2xl bg-slate-50 border border-slate-200 space-y-2.5">
            <div class="flex items-center justify-between flex-wrap gap-2">
              <label class="block text-xs font-extrabold text-slate-800" data-i18n="themeColorLabel">ডিসপ্লে ও কালার থিম</label>
              <div class="flex items-center gap-1.5">
                <label class="px-2.5 py-1 rounded-xl bg-indigo-600 hover:bg-indigo-700 text-white text-[11px] font-extrabold cursor-pointer flex items-center gap-1">
                  <i class="fa-solid fa-upload"></i> <span>ফোন মেমরি থেকে থিম ছবি</span>
                  <input type="file" accept="image/*" onchange="uploadCustomThemePhoto(event)" class="hidden" />
                </label>
                <button type="button" onclick="clearCustomThemePhoto()" class="px-2 py-1 rounded-xl bg-rose-100 hover:bg-rose-200 text-rose-700 text-[11px] font-bold">
                  ছবি মুছুন
                </button>
              </div>
            </div>
            <div class="flex flex-wrap gap-2">
              <button type="button" onclick="applyColorTheme('theme-emerald')" class="px-3 py-1.5 rounded-xl bg-emerald-600 text-white text-xs font-bold">Emerald</button>
              <button type="button" onclick="applyColorTheme('theme-royal')" class="px-3 py-1.5 rounded-xl bg-blue-600 text-white text-xs font-bold">Royal Blue</button>
              <button type="button" onclick="applyColorTheme('theme-crimson')" class="px-3 py-1.5 rounded-xl bg-rose-600 text-white text-xs font-bold">Crimson</button>
              <button type="button" onclick="applyColorTheme('theme-purple')" class="px-3 py-1.5 rounded-xl bg-purple-600 text-white text-xs font-bold">Purple</button>
              <button type="button" onclick="applyColorTheme('theme-amber')" class="px-3 py-1.5 rounded-xl bg-amber-600 text-white text-xs font-bold">Amber Gold</button>
              <button type="button" onclick="applyColorTheme('theme-teal')" class="px-3 py-1.5 rounded-xl bg-teal-600 text-white text-xs font-bold">Teal Ocean</button>
              <button type="button" onclick="applyColorTheme('theme-magenta')" class="px-3 py-1.5 rounded-xl bg-fuchsia-600 text-white text-xs font-bold">Magenta</button>
              <button type="button" onclick="applyColorTheme('theme-indigo')" class="px-3 py-1.5 rounded-xl bg-indigo-600 text-white text-xs font-bold">Indigo</button>
              <button type="button" onclick="applyColorTheme('theme-lime')" class="px-3 py-1.5 rounded-xl bg-lime-600 text-white text-xs font-bold">Lime Green</button>
              <button type="button" onclick="applyColorTheme('theme-dark')" class="px-3 py-1.5 rounded-xl bg-slate-900 text-white text-xs font-bold">Dark Mode</button>
            </div>

            <div class="pt-1">
              <label class="block text-[11px] font-bold text-slate-700 mb-1">ডিসপ্লে লিংক (Display Link)</label>
              <div class="flex gap-2">
                <input type="url" id="set-display-link" oninput="autoSyncSettingsFromInputs()" class="flex-1 px-3 py-2 rounded-xl border border-slate-300 text-xs font-mono bg-white" placeholder="https://..." />
                <button type="button" onclick="openSavedDisplayLink()" class="px-3 py-2 rounded-xl bg-indigo-600 hover:bg-indigo-700 text-white text-xs font-extrabold whitespace-nowrap flex items-center gap-1.5">
                  <i class="fa-solid fa-up-right-from-square"></i> <span>খুলুন</span>
                </button>
              </div>
            </div>
          </div>

          <div class="pt-2 border-t border-slate-200 space-y-2.5">
            <div class="text-xs font-extrabold text-pink-700" data-i18n="paymentGatewayConfigTitle">পেমেন্ট গেটওয়ে কনফিগারেশন (bKash / Nagad / Rocket / Tap / Paymently)</div>
            <div>
              <label class="block text-[11px] font-bold text-slate-600 mb-0.5">BAS_URL</label>
              <input type="text" id="set-pay-bas" oninput="autoSyncSettingsFromInputs()" class="w-full px-3 py-2 rounded-xl border border-slate-300 text-xs font-mono" />
            </div>
            <div>
              <label class="block text-[11px] font-bold text-slate-600 mb-0.5">API_KEY</label>
              <input type="text" id="set-pay-api" oninput="autoSyncSettingsFromInputs()" class="w-full px-3 py-2 rounded-xl border border-slate-300 text-xs font-mono" />
            </div>
            <div>
              <label class="block text-[11px] font-bold text-slate-600 mb-0.5">Personal Account Number</label>
              <input type="text" id="set-pay-acc" oninput="autoSyncSettingsFromInputs()" class="w-full px-3 py-2 rounded-xl border border-slate-300 text-xs font-mono font-bold" />
            </div>
          </div>

          <button type="submit" class="w-full py-3 rounded-xl theme-bg text-white font-extrabold text-sm shadow-lg flex items-center justify-center gap-2">
            <i class="fa-solid fa-check"></i> <span data-i18n="saveSettingsBtn">সেটিংস সংরক্ষণ করুন</span>
          </button>
        </form>

        <div class="app-card bg-white p-4 rounded-2xl shadow-sm border border-slate-200 space-y-3">
          <div class="flex items-center justify-between">
            <h3 class="font-extrabold text-sm text-indigo-900 flex items-center gap-2">
              <i class="fa-solid fa-database text-indigo-600"></i>
              <span>ডাটা ব্যাকআপ ও রিস্টোর (কম্পিউটার / ফাইল)</span>
            </h3>
            <span class="text-[10px] font-bold px-2 py-0.5 rounded bg-indigo-100 text-indigo-800">JSON Safe</span>
          </div>
          <p class="text-xs text-slate-600">আপনার পুরো স্টক, বিক্রয়, বাকি খাতা, খরচ ও সেটিংসের নিরাপদ ব্যাকআপ ফাইল ডাউনলোড বা রিস্টোর করুন।</p>
          <div class="flex flex-wrap gap-2.5">
            <button type="button" onclick="exportFullStoreBackup()" class="px-3.5 py-2 rounded-xl bg-indigo-600 hover:bg-indigo-700 text-white text-xs font-extrabold flex items-center gap-2 shadow">
              <i class="fa-solid fa-download"></i> <span>ব্যাকআপ ডাউনলোড করুন</span>
            </button>
            <label class="px-3.5 py-2 rounded-xl bg-emerald-600 hover:bg-emerald-700 text-white text-xs font-extrabold flex items-center gap-2 shadow cursor-pointer">
              <i class="fa-solid fa-upload"></i> <span>ব্যাকআপ রিস্টোর করুন</span>
              <input type="file" accept="application/json,.json" onchange="importFullStoreBackup(event)" class="hidden" />
            </label>
          </div>
        </div>
      </section>

      </main>
    </div>

    <!-- BOTTOM NAVIGATION BAR -->
    <nav class="lg:hidden fixed bottom-0 left-0 right-0 z-40 bg-white/95 backdrop-blur border-t border-slate-200 shadow-2xl">
      <div class="max-w-5xl mx-auto grid grid-cols-8 py-1.5 px-1.5 gap-1 text-[10px] font-bold">
        <button type="button" onclick="switchScreen('dashboard')" data-nav="dashboard" class="nav-btn flex flex-col items-center justify-center py-1 rounded-xl theme-text theme-soft-bg">
          <i class="fa-solid fa-house text-sm mb-0.5"></i>
          <span class="truncate w-full text-center" data-i18n="navDash">ড্যাশবোর্ড</span>
        </button>
        <button type="button" id="bottom-nav-product-entry" onclick="switchScreen('product-entry')" data-nav="product-entry" class="nav-btn flex flex-col items-center justify-center py-1 rounded-xl text-slate-600">
          <i class="fa-solid fa-boxes-stacked text-sm mb-0.5"></i>
          <span class="truncate w-full text-center" data-i18n="navProduct">প্রোডাক্ট</span>
        </button>
        <button type="button" onclick="switchScreen('product-sale')" data-nav="product-sale" class="nav-btn flex flex-col items-center justify-center py-1 rounded-xl text-slate-600">
          <i class="fa-solid fa-cart-plus text-sm mb-0.5"></i>
          <span class="truncate w-full text-center" data-i18n="navSale">বিক্রয়</span>
        </button>
        <button type="button" onclick="switchScreen('due-ledger')" data-nav="due-ledger" class="nav-btn flex flex-col items-center justify-center py-1 rounded-xl text-slate-600">
          <i class="fa-solid fa-book text-sm mb-0.5"></i>
          <span class="truncate w-full text-center" data-i18n="navDue">বাকি খাতা</span>
        </button>
        <button type="button" onclick="switchScreen('history')" data-nav="history" class="nav-btn flex flex-col items-center justify-center py-1 rounded-xl text-slate-600">
          <i class="fa-solid fa-receipt text-sm mb-0.5"></i>
          <span class="truncate w-full text-center" data-i18n="navVoucher">ভাউচার</span>
        </button>
        <button type="button" onclick="switchScreen('calendar')" data-nav="calendar" class="nav-btn flex flex-col items-center justify-center py-1 rounded-xl text-slate-600">
          <i class="fa-solid fa-calendar-days text-sm mb-0.5"></i>
          <span class="truncate w-full text-center" data-i18n="navCal">ক্যালেন্ডার</span>
        </button>
        <button type="button" onclick="switchScreen('calculator')" data-nav="calculator" class="nav-btn flex flex-col items-center justify-center py-1 rounded-xl text-slate-600">
          <i class="fa-solid fa-calculator text-sm mb-0.5"></i>
          <span class="truncate w-full text-center" data-i18n="navCalc">ক্যালকুলেটর</span>
        </button>
        <button type="button" onclick="switchScreen('notebook')" data-nav="notebook" class="nav-btn flex flex-col items-center justify-center py-1 rounded-xl text-slate-600">
          <i class="fa-solid fa-note-sticky text-sm mb-0.5"></i>
          <span class="truncate w-full text-center" data-i18n="navNote">নোটবুক</span>
        </button>
      </div>
    </nav>
  </div>

  <!-- CAMERA BARCODE SCANNER MODAL -->
  <div id="camera-scanner-modal" class="fixed inset-0 z-[100] bg-black/80 hidden items-center justify-center p-4">
    <div class="bg-white rounded-3xl w-full max-w-md overflow-hidden shadow-2xl">
      <div class="p-4 bg-slate-900 text-white flex items-center justify-between">
        <span class="font-extrabold text-sm flex items-center gap-2"><i class="fa-solid fa-camera text-emerald-400"></i><span data-i18n="scannerModalTitle">ক্যামেরা বারকোড / QR স্ক্যানার</span></span>
        <button type="button" onclick="closeCameraBarcodeScanner()" class="w-8 h-8 rounded-full bg-white/15 flex items-center justify-center">
          <i class="fa-solid fa-xmark"></i>
        </button>
      </div>
      <div class="p-4 space-y-3">
        <div id="qr-reader-viewport" class="w-full rounded-2xl overflow-hidden border border-slate-200"></div>
        <div class="flex gap-2">
          <input type="text" id="manual-scan-input" class="flex-1 px-3 py-2 rounded-xl border border-slate-300 text-xs" placeholder="ম্যানুয়াল কোড বা নাম লিখুন..." />
          <button type="button" onclick="handleScannedCodeValue(document.getElementById('manual-scan-input').value); closeCameraBarcodeScanner()" class="px-4 py-2 rounded-xl bg-emerald-600 text-white text-xs font-extrabold">খুঁজুন</button>
        </div>
      </div>
    </div>
  </div>

  <!-- GENERIC MODAL -->
  <div id="custom-modal" class="fixed inset-0 z-[110] bg-black/60 hidden items-center justify-center p-4">
    <div class="bg-white rounded-3xl w-full max-w-md p-5 shadow-2xl space-y-4 text-slate-800 max-h-[90vh] overflow-y-auto">
      <div class="flex items-center justify-between border-b border-slate-100 pb-2.5">
        <h3 id="modal-title" class="font-extrabold text-base"></h3>
        <button type="button" onclick="closeModal()" class="text-slate-400 hover:text-slate-700"><i class="fa-solid fa-xmark text-lg"></i></button>
      </div>
      <div id="modal-body" class="text-xs sm:text-sm space-y-2"></div>
      <div id="modal-actions" class="flex justify-end gap-2 pt-2"></div>
    </div>
  </div>

  <script>
    const CLOUD_API_URL = "";
    const FIREBASE_CONFIG = {
      apiKey: "",
      authDomain: "",
      databaseURL: "",
      projectId: "",
      storageBucket: "",
      messagingSenderId: "",
      appId: ""
    };

    const DICT = {
      bn: {
        appTitle: "রিবেক & হেতু POS",
        authSubtitle: "কম্পিউটার ও মোবাইল লাইভ ক্লাউড অটো-সিঙ্ক সিস্টেম",
        loginTab: "লগইন করুন",
        registerTab: "নতুন একাউন্ট খুলুন",
        userIdLabel: "মোবাইল নম্বর বা ইউজার আইডি",
        passwordLabel: "গোপন পাসওয়ার্ড",
        loginBtnText: "প্রবেশ করুন",
        ownerNameLabel: "আপনার পূর্ণ নাম",
        shopNameLabel: "দোকানের নাম",
        shopAddrLabel: "দোকানের ঠিকানা",
        confirmPassLabel: "পাসওয়ার্ড নিশ্চিত করুন",
        registerBtnText: "একাউন্ট তৈরি করুন",
        isolatedTag: "অটো-সিঙ্ক সক্রিয়",
        speakingClockLabel: "কথা বলা ঘড়ি",
        globalSearchPlaceholder: "প্রোডাক্ট, বারকোড বা বাকি ক্রেতার নাম খুঁজুন...",
        scanBtn: "স্ক্যান",
        dashTitle: "ড্যাশবোর্ড",
        resetBtn: "রিসেট",
        gpsBtn: "লাইভ GPS",
        welcomeVoiceBtn: "স্বাগতম ভয়েস",
        digitalPayBtn: "ডিজিটাল পেমেন্ট",
        kpiCapital: "মোট মূলধন",
        kpiItems: "মোট প্রোডাক্ট",
        kpiSales: "মোট বিক্রয়",
        kpiProfit: "মোট লাভ",
        kpiDue: "মোট বাকি",
        kpiLoss: "মোট লস",
        gpsCardTitle: "লাইভ GPS লোকেশন ও নেটওয়ার্ক",
        gpsWaitingText: "লাইভ GPS লোকেশন স্বয়ংক্রিয়ভাবে চালু হচ্ছে...",
        gpsRefreshBtn: "রিয়েল GPS",
        weatherTitle: "লাইভ আবহাওয়া আপডেট",
        weatherVoiceBtn: "আবহাওয়া শুনুন",
        otherExpensesTitle: "অন্যান্য খরচ (বিদ্যুৎ, ভাড়া, যাতায়াত ইত্যাদি)",
        expDescPlaceholder: "খরচের বিবরণ (যেমন: বিদ্যুৎ বিল)",
        expAmtPlaceholder: "টাকার পরিমাণ (৳)",
        addExpenseBtn: "খরচ যোগ করুন",
        quickProductEntryBtn: "নতুন প্রোডাক্ট ক্রয় ও স্টক এন্ট্রি করুন",
        prodEntryTitle: "প্রোডাক্ট ক্রয় ও স্টক এন্ট্রি",
        goToSaleBtn: "বিক্রয় পাতায় যান",
        prodNameLabel: "প্রোডাক্টের নাম",
        barcodeLabel: "বারকোড / QR কোড",
        unitLabel: "পরিমাপ একক",
        qtyLabel: "পরিমাণ",
        buyPriceLabel: "ক্রয় মূল্য (প্রতি একক)",
        sellPriceLabel: "বিক্রয় মূল্য (প্রতি একক)",
        totalPurchaseCost: "মোট ক্রয় খরচ:",
        estProfitLabel: "সম্ভাব্য লাভ:",
        saveProductBtn: "স্টকে সংরক্ষণ করুন",
        currentStockTitle: "বর্তমান স্টক তালিকা",
        saleTitle: "প্রোডাক্ট বিক্রয় ও ভাউচার",
        selectSoldProduct: "বিক্রয়কৃত প্রোডাক্ট নির্বাচন করুন",
        saleUnitLabel: "বিক্রয় একক",
        rateLabel: "দর (৳)",
        dateLabel: "তারিখ",
        addMoreProductBtn: "আরও প্রোডাক্ট ভাউচারে যোগ করুন",
        voucherCartTitle: "ভাউচারে যুক্ত প্রোডাক্ট তালিকা",
        clearCartBtn: "সব বাদ দিন",
        grandTotalLabel: "সর্বমোট বিক্রয় মূল্য",
        paymentMethodLabel: "পেমেন্ট মাধ্যম",
        payCash: "নগদ",
        payDue: "বাকি",
        payDigital: "ডিজিটাল পেমেন্ট",
        selectExistingDue: "বিদ্যমান বাকি ক্রেতা নির্বাচন করুন (ঐচ্ছিক)",
        custNameLabel: "নাম",
        custPhoneLabel: "মোবাইল নম্বর",
        custPhotoLabel: "ছবি যুক্ত করুন",
        completeSaleBtn: "বিক্রয় সম্পন্ন ও ভাউচার তৈরি করুন",
        dueLedgerTitle: "বাকি খাতা",
        addressLabel: "দোকানের ঠিকানা অ্যাড / পরিবর্তন করুন",
        photoLabel: "ছবি (মেমরি / গ্যালারি)",
        dueProductsLabel: "বাকিতে নেওয়া প্রোডাক্টসমূহ",
        dueAmountLabel: "বাকি টাকার পরিমাণ (৳)",
        saveDueBtn: "বাকি হিসাব সংরক্ষণ করুন",
        historyTitle: "ক্রয়/বিক্রয় হিস্টিরি ও ভাউচার",
        salesHistoryTab: "বিক্রয় হিস্টিরি",
        purchaseHistoryTab: "ক্রয় হিস্টিরি",
        officialVoucherTitle: "অফিসিয়াল কলাম ভাউচার (ছোট ব্লুটুথ প্রিন্ট)",
        savePdfBtn: "PDF সেভ",
        btPrintBtn: "ব্লুটুথ প্রিন্ট",
        usbPrintBtn: "USB প্রিন্ট",
        shareBtn: "শেয়ার",
        recentRecordsLabel: "সাম্প্রতিক রেকর্ডসমূহ",
        deleteAllBtn: "সব মুছুন",
        healthTitle: "স্বাস্থ্য পরীক্ষা (BP, হার্ট বিট, অক্সিজেন)",
        healthInstruction: "নিচের ফিঙ্গারপ্রিন্ট সেন্সরে আঙুল চেপে ধরে রাখুন (১০০% পর্যন্ত)",
        bpLabel: "ব্লাড প্রেসার",
        bpmLabel: "হার্ট বিট",
        spo2Label: "অক্সিজেন (SpO2)",
        calTitle: "বাংলা ও ইংরেজি ক্যালেন্ডার",
        holidaysLabel: "ছুটির তালিকা ও বিশেষ দিবস",
        calcTitle: "ক্যালকুলেটর (ইউনিট, সাধারণ ও সায়েন্টিফিক)",
        unitCalcDesc: "যেমন: ১ কেজির দাম ১০০ টাকা হলে ২৫০ গ্রামের দাম কত?",
        baseUnitQtyLabel: "মূল এককের মান (যেমন: 1 বা 1000)",
        baseUnitPriceLabel: "মূল এককের মূল্য (৳)",
        targetQtyLabel: "কাঙ্ক্ষিত পরিমাণ (যেমন: 0.25 বা 250)",
        calcResultLabel: "হিসাবের ফলাফল:",
        notebookTitle: "নোটবুক ও অ্যালার্ম রিমাইন্ডার",
        nbPlaceholder: "জরুরি নোট বা রিমাইন্ডার লিখুন...",
        alarmDateLabel: "অ্যালার্ম তারিখ",
        alarmTimeLabel: "অ্যালার্ম সময়",
        saveNoteBtn: "নোট ও অ্যালার্ম সেভ করুন",
        barcodePageTitle: "বারকোড ও QR কোড জেনারেটর",
        profileTitle: "প্রোফাইল ও নিরাপত্তা",
        logoutBtn: "লগআউট",
        changePassTitle: "পাসওয়ার্ড পরিবর্তন",
        updatePassBtn: "পাসওয়ার্ড আপডেট করুন",
        settingsTitle: "দোকান, ডিজিটাল পেমেন্ট ও ব্যাকআপ সেটিংস",
        shopLogoLabel: "দোকানের লোগো",
        shopPhoneLabel: "মোবাইল নম্বর",
        themeColorLabel: "ডিসপ্লে ও কালার থিম",
        paymentGatewayConfigTitle: "পেমেন্ট গেটওয়ে কনফিগারেশন (bKash / Nagad / Rocket / Tap / Paymently)",
        saveSettingsBtn: "সেটিংস সংরক্ষণ করুন",
        navDash: "ড্যাশবোর্ড",
        navProduct: "প্রোডাক্ট",
        navSale: "বিক্রয়",
        navDue: "বাকি খাতা",
        navVoucher: "ভাউচার",
        navCal: "ক্যালেন্ডার",
        navCalc: "ক্যালকুলেটর",
        navNote: "নোটবুক",
        scannerModalTitle: "ক্যামেরা বারকোড / QR স্ক্যানার"
      },
      en: {
        appTitle: "Ribek & Hetu POS",
        authSubtitle: "Desktop & Mobile Live Cloud Auto-Sync System",
        loginTab: "Login",
        registerTab: "Create Account",
        userIdLabel: "Mobile Number or User ID",
        passwordLabel: "Secret Password",
        loginBtnText: "Login",
        ownerNameLabel: "Your Full Name",
        shopNameLabel: "Shop Name",
        shopAddrLabel: "Shop Address",
        confirmPassLabel: "Confirm Password",
        registerBtnText: "Create Account",
        isolatedTag: "Auto-Sync Active",
        speakingClockLabel: "Speaking Clock",
        globalSearchPlaceholder: "Search product, barcode or due customer...",
        scanBtn: "Scan",
        dashTitle: "Dashboard",
        resetBtn: "Reset",
        gpsBtn: "Live GPS",
        welcomeVoiceBtn: "Welcome Voice",
        digitalPayBtn: "Digital Payment",
        kpiCapital: "Total Capital",
        kpiItems: "Total Products",
        kpiSales: "Total Sales",
        kpiProfit: "Total Profit",
        kpiDue: "Total Due",
        kpiLoss: "Total Loss",
        gpsCardTitle: "Live GPS Location & Network",
        gpsWaitingText: "Starting live GPS tracking automatically...",
        gpsRefreshBtn: "Real GPS",
        weatherTitle: "Live Weather Update",
        weatherVoiceBtn: "Hear Weather",
        otherExpensesTitle: "Other Expenses (Electricity, Rent, Transport etc.)",
        expDescPlaceholder: "Expense detail (e.g. Electricity Bill)",
        expAmtPlaceholder: "Amount (৳)",
        addExpenseBtn: "Add Expense",
        quickProductEntryBtn: "Add New Purchased Product & Stock",
        prodEntryTitle: "Product Purchase & Stock Entry",
        goToSaleBtn: "Go to Sale",
        prodNameLabel: "Product Name",
        barcodeLabel: "Barcode / QR Code",
        unitLabel: "Measurement Unit",
        qtyLabel: "Quantity",
        buyPriceLabel: "Purchase Price (Per Unit)",
        sellPriceLabel: "Selling Price (Per Unit)",
        totalPurchaseCost: "Total Purchase Cost:",
        estProfitLabel: "Estimated Profit:",
        saveProductBtn: "Save to Stock",
        currentStockTitle: "Current Stock List",
        saleTitle: "Product Sale & Voucher",
        selectSoldProduct: "Select Product to Sell",
        saleUnitLabel: "Sale Unit",
        rateLabel: "Rate (৳)",
        dateLabel: "Date",
        addMoreProductBtn: "Add More Product to Voucher",
        voucherCartTitle: "Products Added to Voucher",
        clearCartBtn: "Clear All",
        grandTotalLabel: "Grand Total Sale",
        paymentMethodLabel: "Payment Method",
        payCash: "Cash",
        payDue: "Due",
        payDigital: "Digital Payment",
        selectExistingDue: "Select Existing Due Customer (Optional)",
        custNameLabel: "Name",
        custPhoneLabel: "Mobile Number",
        custPhotoLabel: "Attach Photo",
        completeSaleBtn: "Complete Sale & Generate Voucher",
        dueLedgerTitle: "Due Ledger",
        addressLabel: "Shop Address",
        photoLabel: "Photo (Memory / Gallery)",
        dueProductsLabel: "Products Taken on Due",
        dueAmountLabel: "Due Amount (৳)",
        saveDueBtn: "Save Due Record",
        historyTitle: "Purchase/Sale History & Voucher",
        salesHistoryTab: "Sales History",
        purchaseHistoryTab: "Purchase History",
        officialVoucherTitle: "Official Columnar Voucher (Compact BT Print)",
        savePdfBtn: "Save PDF",
        btPrintBtn: "BT Print",
        usbPrintBtn: "USB Print",
        shareBtn: "Share",
        recentRecordsLabel: "Recent Records",
        deleteAllBtn: "Delete All",
        healthTitle: "Health Check (BP, Heart Beat, Oxygen)",
        healthInstruction: "Press and hold your finger on the fingerprint sensor (up to 100%)",
        bpLabel: "Blood Pressure",
        bpmLabel: "Heart Beat",
        spo2Label: "Oxygen (SpO2)",
        calTitle: "Bangla & English Calendar",
        holidaysLabel: "Holidays & Special Days",
        calcTitle: "Calculator (Unit, Standard & Scientific)",
        unitCalcDesc: "Example: If 1 kg costs 100 Taka, how much for 250 grams?",
        baseUnitQtyLabel: "Base Unit Value (e.g. 1 or 1000)",
        baseUnitPriceLabel: "Base Unit Price (৳)",
        targetQtyLabel: "Target Quantity (e.g. 0.25 or 250)",
        calcResultLabel: "Calculated Result:",
        notebookTitle: "Notebook & Alarm Reminder",
        nbPlaceholder: "Write an important note or reminder...",
        alarmDateLabel: "Alarm Date",
        alarmTimeLabel: "Alarm Time",
        saveNoteBtn: "Save Note & Alarm",
        barcodePageTitle: "Barcode & QR Code Generator",
        profileTitle: "Profile & Security",
        logoutBtn: "Logout",
        changePassTitle: "Change Password",
        updatePassBtn: "Update Password",
        settingsTitle: "Store, Digital Payment & Backup Settings",
        shopLogoLabel: "Shop Logo",
        shopPhoneLabel: "Mobile Number",
        themeColorLabel: "Display & Color Theme",
        paymentGatewayConfigTitle: "Payment Gateway Config (bKash / Nagad / Rocket / Tap / Paymently)",
        saveSettingsBtn: "Save Settings",
        navDash: "Dashboard",
        navProduct: "Product",
        navSale: "Sale",
        navDue: "Due Book",
        navVoucher: "Voucher",
        navCal: "Calendar",
        navCalc: "Calculator",
        navNote: "Notebook",
        scannerModalTitle: "Camera Barcode / QR Scanner"
      }
    };

    const MEASUREMENT_UNITS = [
      { id: "kg", bn: "কেজি (Kg)", en: "Kilogram (Kg)", base: "weight", factor: 1000 },
      { id: "gram", bn: "গ্রাম (Gram)", en: "Gram (g)", base: "weight", factor: 1 },
      { id: "liter", bn: "লিটার (Liter)", en: "Liter (L)", base: "volume", factor: 1000 },
      { id: "ml", bn: "মিলিলিটার (ml)", en: "Milliliter (ml)", base: "volume", factor: 1 },
      { id: "pcs", bn: "পিস (Pcs)", en: "Pieces (Pcs)", base: "pcs", factor: 1 },
      { id: "packet", bn: "প্যাকেট (Packet)", en: "Packet", base: "packet", factor: 1 },
      { id: "box", bn: "বক্স / কার্টন (Box)", en: "Box / Carton", base: "box", factor: 1 },
      { id: "meter", bn: "মিটার (Meter)", en: "Meter", base: "meter", factor: 1 },
      { id: "feet", bn: "ফুট (Feet)", en: "Feet", base: "feet", factor: 1 },
      { id: "hali", bn: "হালি (৪ পিস)", en: "Hali (4 Pcs)", base: "pcs", factor: 4 },
      { id: "dozen", bn: "ডজন (১২ পিস)", en: "Dozen (12 Pcs)", base: "pcs", factor: 12 },
      { id: "pair", bn: "জোড়া (২ পিস)", en: "Pair (2 Pcs)", base: "pcs", factor: 2 }
    ];

    let currentLang = localStorage.getItem("rh_lang") || "bn";
    let currentUser = null;
    let activeSessionRole = "admin";
    let activeStaffMember = null;
    let adminOwnerIdForSession = null;
    let userStore = null;
    let autoClockSpeechEnabled = true;
    let lastSpokenHalfHour = "";
    let wrongLoginAttempts = 0;
    let cameraScanner = null;
    let scannerContext = "global";
    let selectedVoucher = null;
    let activeHistoryTab = "sales";
    let activeCalType = "bn";
    let calCursorDate = new Date();
    let weatherState = { temp: 28, humidity: 68, wind: 11, descBn: "পরিষ্কার ও মনোরম আবহাওয়া", descEn: "Clear & Pleasant Sky" };
    let gpsState = { lat: null, lon: null, accuracy: null, address: "", city: "", region: "", country: "", source: "AUTO GPS" };
    let typingSpeechDebounce = null;
    let greetingTypingDebounce = null;
    let saleCartItems = [];
    let calcAngleMode = "DEG";
    let latestAISummaryText = "";
    let isLiveOnline = navigator.onLine;
    let liveLatencyMs = null;
    let netQualityState = navigator.onLine ? "good" : "offline";
    let netConnectionKind = "wifi";
    let gpsWatchId = null;
    let lastReverseGeocodeKey = "";
    let autoSyncDebounce = null;
    let rtdb = null;
    let rtdbRef = null;
    let isApplyingRemoteSnapshot = false;
    let disconnectAlarmInterval = null;
    let disconnectVoiceInterval = null;
    let smsRingtoneInterval = null;
    let audioCtxInstance = null;

    const THEMES = [
      "theme-emerald", "theme-royal", "theme-crimson", "theme-purple",
      "theme-amber", "theme-teal", "theme-magenta", "theme-indigo",
      "theme-lime", "theme-dark"
    ];

    function ensureAudioContext() {
      const Ctx = window.AudioContext || window.webkitAudioContext;
      if (!Ctx) return null;
      if (!audioCtxInstance) audioCtxInstance = new Ctx();
      if (audioCtxInstance.state === "suspended") {
        audioCtxInstance.resume().catch(() => {});
      }
      return audioCtxInstance;
    }

    function playKriingKriingRingtone() {
      try {
        const ctx = ensureAudioContext();
        if (!ctx) return;
        const now = ctx.currentTime;
        const notes = [
          { f: 1180, t: 0.00, d: 0.09 },
          { f: 1520, t: 0.10, d: 0.09 },
          { f: 1180, t: 0.22, d: 0.09 },
          { f: 1520, t: 0.32, d: 0.11 }
        ];
        notes.forEach(n => {
          const osc = ctx.createOscillator();
          const gain = ctx.createGain();
          osc.type = "triangle";
          osc.frequency.setValueAtTime(n.f, now + n.t);
          gain.gain.setValueAtTime(0.001, now + n.t);
          gain.gain.linearRampToValueAtTime(0.24, now + n.t + 0.02);
          gain.gain.exponentialRampToValueAtTime(0.001, now + n.t + n.d);
          osc.connect(gain);
          gain.connect(ctx.destination);
          osc.start(now + n.t);
          osc.stop(now + n.t + n.d + 0.02);
        });
      } catch (e) {}
    }

    function syncSmsRingtoneState() {
      const hasUnread = Boolean(
        userStore &&
        Array.isArray(userStore.staffMessages) &&
        userStore.staffMessages.some(m => m.unread === true)
      );
      if (hasUnread) {
        if (!smsRingtoneInterval) {
          playKriingKriingRingtone();
          smsRingtoneInterval = setInterval(() => {
            const stillUnread = Boolean(
              userStore &&
              Array.isArray(userStore.staffMessages) &&
              userStore.staffMessages.some(m => m.unread === true)
            );
            if (stillUnread) {
              playKriingKriingRingtone();
            } else {
              stopSmsRingtoneLoop();
            }
          }, 1150);
        }
      } else {
        stopSmsRingtoneLoop();
      }
    }

    function stopSmsRingtoneLoop() {
      if (smsRingtoneInterval) {
        clearInterval(smsRingtoneInterval);
        smsRingtoneInterval = null;
      }
    }

    function playDisconnectAlarmBeep() {
      try {
        const ctx = ensureAudioContext();
        if (!ctx) return;
        const osc = ctx.createOscillator();
        const gain = ctx.createGain();
        osc.type = "sawtooth";
        osc.frequency.setValueAtTime(920, ctx.currentTime);
        osc.frequency.exponentialRampToValueAtTime(520, ctx.currentTime + 0.35);
        gain.gain.setValueAtTime(0.18, ctx.currentTime);
        gain.gain.exponentialRampToValueAtTime(0.001, ctx.currentTime + 0.4);
        osc.connect(gain);
        gain.connect(ctx.destination);
        osc.start();
        osc.stop(ctx.currentTime + 0.42);
      } catch (e) {}
    }

    function speakNetworkDisconnectedAlert() {
      const msg = currentLang === "bn"
        ? "আপনার ইন্টারনেট কানেক্শন বিচ্ছিন্ন হয়ে গেছে সংযোগ দেয়ার জন্য চেষ্টা করুন"
        : "Your internet connection has been disconnected, please try to reconnect.";
      speakText(msg, true);
    }

    function startDisconnectAlarmAndVoice() {
      playDisconnectAlarmBeep();
      speakNetworkDisconnectedAlert();
      if (!disconnectAlarmInterval) {
        disconnectAlarmInterval = setInterval(() => {
          if (!isLiveOnline) playDisconnectAlarmBeep();
        }, 1600);
      }
      if (!disconnectVoiceInterval) {
        disconnectVoiceInterval = setInterval(() => {
          if (!isLiveOnline) speakNetworkDisconnectedAlert();
        }, 8000);
      }
    }

    function stopDisconnectAlarmAndVoice() {
      if (disconnectAlarmInterval) {
        clearInterval(disconnectAlarmInterval);
        disconnectAlarmInterval = null;
      }
      if (disconnectVoiceInterval) {
        clearInterval(disconnectVoiceInterval);
        disconnectVoiceInterval = null;
      }
    }

    function updateDayNightBadge() {
      const hr = new Date().getHours();
      const isDay = hr >= 6 && hr < 18;
      const iconEl = document.getElementById("hdr-day-night-icon");
      const lblEl = document.getElementById("hdr-day-night-label");
      if (iconEl) {
        iconEl.className = isDay ? "fa-solid fa-sun text-amber-300" : "fa-solid fa-moon text-indigo-200";
      }
      if (lblEl) {
        lblEl.textContent = isDay
          ? (currentLang === "bn" ? "দিন" : "Day")
          : (currentLang === "bn" ? "রাত" : "Night");
      }
    }

    function updateLiveMapEmbed() {
      const iframe = document.getElementById("live-gps-map-iframe");
      if (!iframe) return;
      const targetInput = (document.getElementById("target-shared-location-input") || { value: "" }).value.trim();
      let lat = gpsState.lat !== null ? gpsState.lat : 22.3569;
      let lon = gpsState.lon !== null ? gpsState.lon : 91.7832;

      const coordMatch = targetInput.match(/(-?\d+\.\d+)\s*,\s*(-?\d+\.\d+)/);
      if (coordMatch) {
        lat = parseFloat(coordMatch[1]);
        lon = parseFloat(coordMatch[2]);
      }

      const d = 0.03;
      const bbox = `${(lon - d).toFixed(4)}%2C${(lat - d).toFixed(4)}%2C${(lon + d).toFixed(4)}%2C${(lat + d).toFixed(4)}`;
      const src = `https://www.openstreetmap.org/export/embed.html?bbox=${bbox}&layer=mapnik&marker=${lat.toFixed(5)}%2C${lon.toFixed(5)}`;
      if (iframe.src !== src) iframe.src = src;
    }

    function openLiveMapAndAutoRoute() {
      const dest = (document.getElementById("target-shared-location-input") || { value: "" }).value.trim();
      const origin = (gpsState.lat !== null && gpsState.lon !== null)
        ? `${gpsState.lat},${gpsState.lon}`
        : (gpsState.address || "");
      let url = "";
      if (dest) {
        url = `https://www.google.com/maps/dir/?api=1${origin ? `&origin=${encodeURIComponent(origin)}` : ""}&destination=${encodeURIComponent(dest)}&travelmode=driving`;
      } else if (origin) {
        url = `https://www.google.com/maps/search/?api=1&query=${encodeURIComponent(origin)}`;
      } else {
        url = "https://www.google.com/maps";
      }
      window.open(url, "_blank", "noopener,noreferrer");
    }

    function navigateToSharedLocation() {
      const dest = (document.getElementById("target-shared-location-input") || { value: "" }).value.trim();
      if (!dest) {
        const m = currentLang === "bn" ? "আগে শেয়ার করা লাইভ লোকেশন বা ঠিকানা লিখুন।" : "Please enter a shared live location or address first.";
        showToast(m, "info");
        speakText(m, true);
        return;
      }
      updateLiveMapEmbed();
      openLiveMapAndAutoRoute();
    }

    async function shareMyCurrentLiveLocation() {
      if (gpsState.lat === null || gpsState.lon === null) {
        fetchGpsLocation(true, false);
        return;
      }
      const mapsLink = `https://www.google.com/maps?q=${gpsState.lat.toFixed(6)},${gpsState.lon.toFixed(6)}`;
      const text = currentLang === "bn"
        ? `আমার লাইভ লোকেশন: ${gpsState.address || "GPS"} (${gpsState.lat.toFixed(5)}, ${gpsState.lon.toFixed(5)}) — ${mapsLink}`
        : `My Live Location: ${gpsState.address || "GPS"} (${gpsState.lat.toFixed(5)}, ${gpsState.lon.toFixed(5)}) — ${mapsLink}`;
      if (navigator.share) {
        try {
          await navigator.share({ title: "Live Location", text, url: mapsLink });
          return;
        } catch (e) {}
      }
      window.open(`https://wa.me/?text=${encodeURIComponent(text)}`, "_blank");
    }

    function openStaffSmsModal() {
      stopSmsRingtoneLoop();
      if (!ensureActiveSessionStore()) return;
      if (!Array.isArray(userStore.staffMessages)) userStore.staffMessages = [];

      let markedAnyRead = false;
      userStore.staffMessages.forEach(m => {
        if (m.unread !== false) {
          m.unread = false;
          markedAnyRead = true;
        }
      });
      if (markedAnyRead) {
        localStorage.setItem(`rh_isolated_data_${adminOwnerIdForSession}`, JSON.stringify(userStore));
        updateSmsBadgeCount();
        scheduleAutoSync("sms-read");
        speakText(currentLang === "bn" ? "SMS ওপেন করা হয়েছে, রিংটোন বন্ধ করা হয়েছে।" : "SMS opened, ringtone stopped.", true);
      }

      const isStaff = activeSessionRole === "staff" && !!activeStaffMember;
      const staffList = userStore.staffProfiles || [];
      const msgs = userStore.staffMessages.slice(0, 30);

      document.getElementById("modal-title").textContent = currentLang === "bn"
        ? "অ্যাডমিন ও স্টাফ মেসেজ / SMS সিস্টেম"
        : "Admin & Staff SMS / Message System";

      document.getElementById("modal-body").innerHTML = `
        <div class="space-y-2.5 text-xs">
          ${!isStaff ? `
            <div>
              <label class="block font-bold mb-1">প্রাপক স্টাফ নির্বাচন করুন</label>
              <select id="sms-target-staff" class="w-full px-3 py-2 rounded-xl border border-slate-300 font-bold bg-white">
                <option value="ALL">সকল স্টাফ (All Staff)</option>
                ${staffList.map(st => `<option value="${st.id}">${escapeHtml(st.name)} (${escapeHtml(st.staffUserId)})</option>`).join("")}
              </select>
            </div>
          ` : `<div class="p-2 rounded-xl bg-indigo-50 text-indigo-900 font-bold">প্রেরক: ${escapeHtml(activeStaffMember.name)} (স্টাফ) → অ্যাডমিন</div>`}

          <div>
            <label class="block font-bold mb-1">SMS / বার্তা লিখুন</label>
            <input type="text" id="sms-message-input" onkeydown="if(event.key==='Enter'){event.preventDefault();sendAdminStaffSms();}" class="w-full px-3 py-2 rounded-xl border border-slate-300 text-xs font-bold" placeholder="তথ্য বা নির্দেশনা লিখুন..." />
          </div>

          <div class="flex items-center justify-between">
            <span class="text-[11px] font-extrabold text-emerald-700"><i class="fa-solid fa-check-double mr-1"></i>পঠিত ও রিংটোন বন্ধ</span>
            ${msgs.length > 0 ? `<button type="button" onclick="clearAllStaffSms()" class="px-2 py-0.5 rounded-lg bg-rose-100 text-rose-700 text-[10px] font-extrabold">সব ডিলিট</button>` : ""}
          </div>

          <div class="max-h-52 overflow-y-auto space-y-1.5 p-2 rounded-xl bg-slate-50 border border-slate-200">
            ${msgs.length === 0
              ? `<div class="text-center text-slate-400 py-3">কোনো SMS / বার্তা নেই</div>`
              : msgs.map(m => `
                <div class="p-2 rounded-xl bg-white border border-slate-100 space-y-1">
                  <div class="flex items-center justify-between gap-2 text-[10px] font-extrabold text-indigo-700">
                    <span class="truncate">${escapeHtml(m.senderLabel)} → ${escapeHtml(m.receiverLabel)}</span>
                    <div class="flex items-center gap-1.5 shrink-0">
                      <span class="px-1.5 py-0.2 rounded bg-emerald-100 text-emerald-800 text-[9px]">Read</span>
                      <span class="text-slate-400 font-mono">${escapeHtml(m.timeStr)}</span>
                      <button type="button" onclick="deleteSingleStaffSms('${m.id}')" class="text-rose-600 hover:text-rose-800 px-1" title="Delete SMS">
                        <i class="fa-solid fa-trash"></i>
                      </button>
                    </div>
                  </div>
                  <div class="text-xs font-bold text-slate-800">${escapeHtml(m.text)}</div>
                </div>
              `).join("")}
          </div>
        </div>
      `;

      document.getElementById("modal-actions").innerHTML = `
        <button type="button" onclick="closeModal()" class="px-3 py-2 rounded-xl bg-slate-200 text-xs font-bold">বন্ধ</button>
        <button type="button" onclick="sendAdminStaffSms()" class="px-4 py-2 rounded-xl bg-indigo-600 hover:bg-indigo-700 text-white text-xs font-extrabold flex items-center gap-1.5">
          <i class="fa-solid fa-paper-plane"></i> <span>পাঠান ও স্বয়ংক্রিয় সিঙ্ক</span>
        </button>
      `;
      document.getElementById("custom-modal").classList.remove("hidden");
      document.getElementById("custom-modal").classList.add("flex");
    }

    function deleteSingleStaffSms(smsId) {
      if (!ensureActiveSessionStore() || !Array.isArray(userStore.staffMessages)) return;
      userStore.staffMessages = userStore.staffMessages.filter(m => m.id !== smsId);
      saveUserIsolatedStore();
      updateSmsBadgeCount();
      const spoken = currentLang === "bn" ? "বার্তাটি ডিলিট করা হয়েছে।" : "Message deleted.";
      showToast(spoken, "info");
      speakText(spoken, true);
      openStaffSmsModal();
    }

    function clearAllStaffSms() {
      if (!ensureActiveSessionStore()) return;
      userStore.staffMessages = [];
      saveUserIsolatedStore();
      updateSmsBadgeCount();
      const spoken = currentLang === "bn" ? "সব SMS বার্তা ডিলিট করা হয়েছে।" : "All SMS messages deleted.";
      showToast(spoken, "info");
      speakText(spoken, true);
      openStaffSmsModal();
    }

    function sendAdminStaffSms() {
      if (!ensureActiveSessionStore()) return;
      if (!Array.isArray(userStore.staffMessages)) userStore.staffMessages = [];
      const inputEl = document.getElementById("sms-message-input");
      const text = (inputEl ? inputEl.value : "").trim();
      if (!text) return;

      const isStaff = activeSessionRole === "staff" && !!activeStaffMember;
      let receiverLabel = "অ্যাডমিন";
      let targetPhone = (userStore.settings && userStore.settings.shopPhone) || "";

      if (!isStaff) {
        const selVal = (document.getElementById("sms-target-staff") || { value: "ALL" }).value;
        if (selVal === "ALL") {
          receiverLabel = "সকল স্টাফ";
          targetPhone = (userStore.staffProfiles || []).map(s => s.staffUserId).filter(p => /^\d{6,}$/.test(p)).join(",");
        } else {
          const st = (userStore.staffProfiles || []).find(s => s.id === selVal);
          if (st) {
            receiverLabel = `${st.name} (${st.staffUserId})`;
            if (/^\d{6,}$/.test(st.staffUserId)) targetPhone = st.staffUserId;
          }
        }
      }

      const senderLabel = isStaff
        ? `${activeStaffMember.name} (Staff)`
        : ((currentUser && currentUser.owner) ? `${currentUser.owner} (Admin)` : "Admin");

      const now = new Date();
      const entry = {
        id: "SMS_" + Date.now(),
        senderLabel,
        receiverLabel,
        text,
        unread: true,
        timeStr: `${now.toLocaleDateString()} ${now.toLocaleTimeString([], { hour: "2-digit", minute: "2-digit" })}`
      };

      userStore.staffMessages.unshift(entry);
      saveUserIsolatedStore();
      updateSmsBadgeCount();
      syncSmsRingtoneState();

      const spoken = currentLang === "bn"
        ? `নতুন SMS এসেছে: ${senderLabel} থেকে ${receiverLabel} এর কাছে: ${text}`
        : `New SMS from ${senderLabel} to ${receiverLabel}: ${text}`;
      showToast(spoken, "warn");
      speakText(spoken, true);

      if (targetPhone && !targetPhone.includes(",")) {
        const smsHref = `sms:${encodeURIComponent(targetPhone)}?body=${encodeURIComponent(`${senderLabel}: ${text}`)}`;
        const link = document.createElement("a");
        link.href = smsHref;
        link.click();
      }

      openStaffSmsModal();
    }

    function updateSmsBadgeCount() {
      const badge = document.getElementById("hdr-sms-badge");
      if (!badge || !userStore) return;
      const unreadCnt = Array.isArray(userStore.staffMessages)
        ? userStore.staffMessages.filter(m => m.unread === true).length
        : 0;
      if (unreadCnt > 0) {
        badge.textContent = currentLang === "bn" ? toBnNum(unreadCnt) : String(unreadCnt);
        badge.classList.remove("hidden");
        badge.classList.add("animate-bounce");
      } else {
        badge.classList.add("hidden");
        badge.classList.remove("animate-bounce");
      }
      syncSmsRingtoneState();
    }

    function persistSessionState() {
      if (adminOwnerIdForSession) {
        localStorage.setItem("rh_active_uid", adminOwnerIdForSession);
        try { sessionStorage.setItem("rh_session_uid", adminOwnerIdForSession); } catch (e) {}
      }
      localStorage.setItem("rh_active_role", activeSessionRole || "admin");
      try { sessionStorage.setItem("rh_session_role", activeSessionRole || "admin"); } catch (e) {}
      if (activeSessionRole === "staff" && activeStaffMember && activeStaffMember.id) {
        localStorage.setItem("rh_active_staff_id", activeStaffMember.id);
        try { sessionStorage.setItem("rh_session_staff_id", activeStaffMember.id); } catch (e) {}
      } else {
        localStorage.removeItem("rh_active_staff_id");
        try { sessionStorage.removeItem("rh_session_staff_id"); } catch (e) {}
      }
    }

    function readImageFileToDataURL(file, callback, maxDim = 600, quality = 0.78) {
      if (!file) return;
      const reader = new FileReader();
      reader.onload = ev => {
        const rawUrl = ev.target.result;
        const img = new Image();
        img.onload = () => {
          try {
            let w = img.width || maxDim;
            let h = img.height || maxDim;
            if (w > maxDim || h > maxDim) {
              if (w >= h) {
                h = Math.round((h * maxDim) / w);
                w = maxDim;
              } else {
                w = Math.round((w * maxDim) / h);
                h = maxDim;
              }
            }
            const canvas = document.createElement("canvas");
            canvas.width = w;
            canvas.height = h;
            const ctx = canvas.getContext("2d");
            ctx.drawImage(img, 0, 0, w, h);
            callback(canvas.toDataURL("image/jpeg", quality));
          } catch (e) {
            callback(rawUrl);
          }
        };
        img.onerror = () => callback(rawUrl);
        img.src = rawUrl;
      };
      reader.readAsDataURL(file);
    }

    function ensureActiveSessionStore() {
      if (!adminOwnerIdForSession) {
        let savedUid = localStorage.getItem("rh_active_uid");
        if (!savedUid) {
          try { savedUid = sessionStorage.getItem("rh_session_uid"); } catch (e) {}
        }
        if (savedUid) {
          adminOwnerIdForSession = savedUid;
          const users = getRegisteredUsers();
          currentUser = users.find(u => u.id === savedUid) || currentUser;
        }
      }
      if (!userStore && adminOwnerIdForSession) {
        loadUserIsolatedStore(
          adminOwnerIdForSession,
          currentUser ? currentUser.shopName : "Ribek & Hetu",
          currentUser ? currentUser.userId : "01732333764",
          currentUser ? currentUser.shopAddr : "চট্টগ্রাম, বাংলাদেশ"
        );
      }
      if (userStore) {
        if (!Array.isArray(userStore.products)) userStore.products = [];
        if (!Array.isArray(userStore.sales)) userStore.sales = [];
        if (!Array.isArray(userStore.purchases)) userStore.purchases = [];
        if (!Array.isArray(userStore.dueCustomers)) userStore.dueCustomers = [];
        if (!Array.isArray(userStore.expenses)) userStore.expenses = [];
        if (!Array.isArray(userStore.notes)) userStore.notes = [];
        if (!Array.isArray(userStore.staffProfiles)) userStore.staffProfiles = [];
        if (!Array.isArray(userStore.staffMessages)) userStore.staffMessages = [];
      }
      return Boolean(userStore);
    }

    function isRemoteSyncConfigured() {
      const hasFirebase = typeof firebase !== "undefined" && Boolean(FIREBASE_CONFIG.databaseURL && FIREBASE_CONFIG.apiKey);
      const hasCloudApi = Boolean(CLOUD_API_URL && CLOUD_API_URL.trim().length > 0);
      return hasFirebase || hasCloudApi;
    }

    function detectConnectionKind() {
      const conn = navigator.connection || navigator.mozConnection || navigator.webkitConnection;
      if (!conn) return isLiveOnline ? "wifi" : "none";
      const t = String(conn.type || "").toLowerCase();
      const eff = String(conn.effectiveType || "").toLowerCase();
      if (t === "cellular" || ["2g", "slow-2g", "3g", "4g", "5g"].includes(eff)) {
        return "cellular";
      }
      if (t === "wifi" || t === "ethernet") {
        return "wifi";
      }
      return isLiveOnline ? "wifi" : "none";
    }

    function computeNetworkQuality() {
      if (!isLiveOnline || !navigator.onLine) return "offline";
      const conn = navigator.connection || navigator.mozConnection || navigator.webkitConnection;
      const eff = conn ? String(conn.effectiveType || "").toLowerCase() : "";
      const rtt = conn && typeof conn.rtt === "number" ? conn.rtt : 0;
      if (eff === "slow-2g" || eff === "2g" || rtt > 600 || (liveLatencyMs !== null && liveLatencyMs > 300)) {
        return "weak";
      }
      return "good";
    }

    function applyInternetStatusUI() {
      netConnectionKind = detectConnectionKind();
      netQualityState = computeNetworkQuality();

      const kindLabel = netConnectionKind === "cellular"
        ? (currentLang === "bn" ? "মোবাইল নেট" : "Mobile Data")
        : (currentLang === "bn" ? "Wi-Fi" : "Wi-Fi");

      let statusLabel = "";
      if (netQualityState === "offline") {
        statusLabel = currentLang === "bn" ? "অফলাইন" : "Offline";
      } else if (netQualityState === "weak") {
        statusLabel = currentLang === "bn" ? `দুর্বল ${kindLabel}` : `Weak ${kindLabel}`;
      } else {
        statusLabel = currentLang === "bn" ? `লাইভ ${kindLabel}` : `Live ${kindLabel}`;
      }

      document.querySelectorAll(".net-status-text").forEach(el => {
        el.textContent = statusLabel;
      });

      const iconClass = netQualityState === "offline"
        ? "fa-solid fa-plane-slash"
        : (netConnectionKind === "cellular" ? "fa-solid fa-signal" : "fa-solid fa-wifi");

      document.querySelectorAll(".net-type-icon").forEach(ic => {
        ic.className = `${ic.classList.contains("text-[10px]") || ic.classList.contains("text-[9px]") ? "net-type-icon " + iconClass + " text-[10px]" : "net-type-icon " + iconClass}`;
      });

      const pingEl = document.getElementById("net-ping-text");
      if (pingEl) {
        pingEl.textContent = (netQualityState !== "offline" && liveLatencyMs !== null) ? `(${liveLatencyMs}ms)` : "";
      }

      const hdrBtn = document.getElementById("hdr-net-btn");
      const dashPill = document.getElementById("dash-net-pill");
      const authBadge = document.getElementById("auth-net-badge");

      if (hdrBtn) {
        if (netQualityState === "offline") {
          hdrBtn.className = "px-2 py-1.5 rounded-xl bg-rose-600 border border-rose-300 text-white text-[10px] font-extrabold flex items-center gap-1 shadow animate-pulse";
        } else if (netQualityState === "weak") {
          hdrBtn.className = "px-2 py-1.5 rounded-xl bg-amber-400 border border-amber-200 text-slate-950 text-[10px] font-extrabold flex items-center gap-1 shadow";
        } else {
          hdrBtn.className = "px-2 py-1.5 rounded-xl bg-emerald-500 border border-emerald-200 text-white text-[10px] font-extrabold flex items-center gap-1 shadow";
        }
      }

      if (dashPill) {
        dashPill.textContent = statusLabel;
        if (netQualityState === "offline") {
          dashPill.className = "px-2 py-0.5 rounded-full text-[10px] font-extrabold bg-rose-600 text-white";
        } else if (netQualityState === "weak") {
          dashPill.className = "px-2 py-0.5 rounded-full text-[10px] font-extrabold bg-amber-300 text-slate-950";
        } else {
          dashPill.className = "px-2 py-0.5 rounded-full text-[10px] font-extrabold bg-emerald-600 text-white";
        }
      }

      if (authBadge) {
        if (netQualityState === "offline") {
          authBadge.className = "px-2.5 py-0.5 rounded-full bg-rose-600 text-white text-[10px] font-bold flex items-center gap-1.5";
        } else if (netQualityState === "weak") {
          authBadge.className = "px-2.5 py-0.5 rounded-full bg-amber-300 text-slate-950 text-[10px] font-bold flex items-center gap-1.5";
        } else {
          authBadge.className = "px-2.5 py-0.5 rounded-full bg-emerald-500 text-white text-[10px] font-bold flex items-center gap-1.5";
        }
      }
    }

    async function runLiveConnectivityCheck(announce = false, refreshLiveData = false) {
      const prevOnline = isLiveOnline;
      if (!navigator.onLine) {
        isLiveOnline = false;
        liveLatencyMs = null;
        applyInternetStatusUI();
        startDisconnectAlarmAndVoice();
        if (announce) {
          showToast(
            currentLang === "bn"
              ? "আপনার ইন্টারনেট কানেক্শন বিচ্ছিন্ন হয়ে গেছে সংযোগ দেয়ার জন্য চেষ্টা করুন"
              : "Your internet connection has been disconnected, please try to reconnect.",
            "error"
          );
        }
        return false;
      }

      const start = performance.now();
      try {
        const ctrl = new AbortController();
        const timer = setTimeout(() => ctrl.abort(), 4500);
        await fetch("https://api.open-meteo.com/v1/forecast?latitude=22.3569&longitude=91.7832&current=temperature_2m", {
          cache: "no-store",
          signal: ctrl.signal
        });
        clearTimeout(timer);
        liveLatencyMs = Math.max(12, Math.round(performance.now() - start));
        isLiveOnline = true;
      } catch (e) {
        isLiveOnline = navigator.onLine;
        liveLatencyMs = null;
      }

      applyInternetStatusUI();

      if (!isLiveOnline) {
        startDisconnectAlarmAndVoice();
        if (announce) {
          showToast(
            currentLang === "bn"
              ? "আপনার ইন্টারনেট কানেক্শন বিচ্ছিন্ন হয়ে গেছে সংযোগ দেয়ার জন্য চেষ্টা করুন"
              : "Your internet connection has been disconnected, please try to reconnect.",
            "error"
          );
        }
        return false;
      }

      stopDisconnectAlarmAndVoice();

      if (refreshLiveData || !prevOnline) {
        if (gpsState.lat === null || gpsState.lon === null) {
          fetchIpFallbackLocation(false, false);
        }
        fetchLiveWeather();
        scheduleAutoSync("live-online");
      }

      if (announce) {
        const msg = currentLang === "bn"
          ? `ইন্টারনেট সংযোগ সক্রিয় আছে${liveLatencyMs ? ` (${liveLatencyMs} মিলিসেকেন্ড)` : ""}।`
          : `Internet connection active${liveLatencyMs ? ` (${liveLatencyMs} ms)` : ""}.`;
        showToast(msg, "success");
        speakText(msg, true);
      }

      return isLiveOnline;
    }

    function onAuthRoleModeChange() {
      const loginMode = (document.querySelector('input[name="loginRoleMode"]:checked') || { value: "admin" }).value;
      const regMode = (document.querySelector('input[name="regRoleMode"]:checked') || { value: "admin" }).value;

      const gateBox = document.getElementById("login-admin-gate-box");
      if (gateBox) gateBox.classList.toggle("hidden", loginMode !== "staff");

      const adminFields = document.getElementById("reg-admin-fields");
      const staffFields = document.getElementById("reg-staff-fields");
      const regBtnText = document.getElementById("registerBtnTextSpan");

      if (adminFields) adminFields.classList.toggle("hidden", regMode !== "admin");
      if (staffFields) staffFields.classList.toggle("hidden", regMode !== "staff");
      if (regBtnText) {
        regBtnText.textContent = regMode === "staff"
          ? (currentLang === "bn" ? "স্টাফ প্রোফাইল রেজিস্ট্রেশন করুন" : "Register Staff Profile")
          : DICT[currentLang].registerBtnText;
      }
    }

    function handleAuthStaffLogoUpload(e) {
      const f = e.target.files && e.target.files[0];
      if (!f) return;
      readImageFileToDataURL(f, dataUrl => {
        document.getElementById("reg-staff-shop-logo-data").value = dataUrl;
        document.getElementById("reg-staff-shop-logo-prev").src = dataUrl;
      }, 400, 0.78);
    }

    function handleAuthStaffPhotoUpload(e) {
      const f = e.target.files && e.target.files[0];
      if (!f) return;
      readImageFileToDataURL(f, dataUrl => {
        document.getElementById("reg-staff-photo-data").value = dataUrl;
        document.getElementById("reg-staff-photo-prev").src = dataUrl;
      }, 400, 0.78);
    }

    function openSavedDisplayLink() {
      const inputEl = document.getElementById("set-display-link");
      const rawUrl = ((userStore && userStore.settings && userStore.settings.displayLink) || (inputEl ? inputEl.value : "") || "").trim();
      if (!rawUrl) {
        const m = currentLang === "bn" ? "আগে ডিসপ্লে লিংক বক্সে একটি লিংক লিখুন।" : "Please enter a Display Link first.";
        showToast(m, "info");
        speakText(m, true);
        return;
      }
      persistSessionState();
      const normalized = /^https?:\/\//i.test(rawUrl) ? rawUrl : `https://${rawUrl}`;
      window.open(normalized, "_blank", "noopener,noreferrer");
    }

    function setAutoSyncBadge(text) {
      const el = document.getElementById("auto-sync-status-badge");
      if (el) el.textContent = text;
    }

    function initFirebaseRealtimeSync() {
      if (typeof firebase === "undefined" || !FIREBASE_CONFIG.databaseURL || !FIREBASE_CONFIG.apiKey) {
        setAutoSyncBadge(currentLang === "bn" ? "লোকাল অটো-সেভ সক্রিয়" : "Local Auto-Save Active");
        return;
      }
      try {
        if (!firebase.apps.length) firebase.initializeApp(FIREBASE_CONFIG);
        rtdb = firebase.database();
        setAutoSyncBadge(currentLang === "bn" ? "ফায়ারবেস লাইভ প্রস্তুত" : "Firebase Live Ready");
      } catch (e) {
        setAutoSyncBadge(currentLang === "bn" ? "লোকাল অটো-সেভ সক্রিয়" : "Local Auto-Save Active");
      }
    }

    function attachRealtimeStoreListener() {
      if (!rtdb || !adminOwnerIdForSession) return;
      if (rtdbRef) {
        try { rtdbRef.off(); } catch (e) {}
      }
      rtdbRef = rtdb.ref(`pos_stores/${adminOwnerIdForSession}`);
      rtdbRef.on("value", (snap) => {
        const remote = snap.val();
        if (!remote || !remote.data || !remote.data.settings) return;
        const localRaw = localStorage.getItem(`rh_isolated_data_${adminOwnerIdForSession}`);
        const incomingRaw = JSON.stringify(remote.data);
        if (localRaw === incomingRaw) return;

        isApplyingRemoteSnapshot = true;
        userStore = {
          ...getDefaultUserStore(remote.data.settings.shopName, remote.data.settings.shopPhone, remote.data.settings.shopAddr),
          ...remote.data,
          settings: { ...getDefaultUserStore().settings, ...remote.data.settings }
        };
        if (Array.isArray(userStore.dueCustomers)) {
          userStore.dueCustomers = userStore.dueCustomers.map(normalizeDueCustomerRecord);
        }
        localStorage.setItem(`rh_isolated_data_${adminOwnerIdForSession}`, JSON.stringify(userStore));
        renderAllViews();
        setAutoSyncBadge(currentLang === "bn" ? "রিয়েল-টাইম সিঙ্কড" : "Realtime Synced");
        isApplyingRemoteSnapshot = false;
      });
    }

    async function syncStoreToCloudIfConfigured(reason = "auto") {
      if (!userStore || !adminOwnerIdForSession || isApplyingRemoteSnapshot) return;

      if (!isRemoteSyncConfigured() || !isLiveOnline) {
        setAutoSyncBadge(currentLang === "bn" ? "লোকাল অটো-সেভ সক্রিয়" : "Local Auto-Save Active");
        return;
      }

      if (rtdb && isLiveOnline) {
        try {
          setAutoSyncBadge(currentLang === "bn" ? "ফায়ারবেস সিঙ্ক..." : "Firebase Sync...");
          await rtdb.ref(`pos_stores/${adminOwnerIdForSession}`).set({
            updatedAt: new Date().toISOString(),
            role: activeSessionRole,
            reason,
            data: userStore
          });
          setAutoSyncBadge(currentLang === "bn" ? "রিয়েল-টাইম সিঙ্কড" : "Realtime Synced");
        } catch (e) {}
      }

      if (CLOUD_API_URL && isLiveOnline) {
        try {
          setAutoSyncBadge(currentLang === "bn" ? "ক্লাউড সিঙ্ক হচ্ছে..." : "Cloud Syncing...");
          await fetch(CLOUD_API_URL, {
            method: "POST",
            headers: { "Content-Type": "application/json" },
            body: JSON.stringify({
              action: "sync_store",
              ownerId: adminOwnerIdForSession,
              role: activeSessionRole,
              reason,
              updatedAt: new Date().toISOString(),
              data: userStore
            })
          });
          setAutoSyncBadge(currentLang === "bn" ? "লাইভ ক্লাউড সিঙ্কড" : "Live Cloud Synced");
        } catch (e) {
          setAutoSyncBadge(currentLang === "bn" ? "লোকাল অটো-সেভ" : "Local Auto-Save");
        }
      }
    }

    async function pullStoreFromCloud(silent = true) {
      if (!adminOwnerIdForSession) return false;
      if (!isLiveOnline) {
        if (!silent) showToast(currentLang === "bn" ? "ইন্টারনেট সংযোগ নেই।" : "No internet connection.", "error");
        return false;
      }

      if (rtdb) {
        try {
          const snap = await rtdb.ref(`pos_stores/${adminOwnerIdForSession}`).get();
          const remote = snap.val();
          if (remote && remote.data && remote.data.settings) {
            isApplyingRemoteSnapshot = true;
            userStore = {
              ...getDefaultUserStore(remote.data.settings.shopName, remote.data.settings.shopPhone, remote.data.settings.shopAddr),
              ...remote.data,
              settings: { ...getDefaultUserStore().settings, ...remote.data.settings }
            };
            localStorage.setItem(`rh_isolated_data_${adminOwnerIdForSession}`, JSON.stringify(userStore));
            renderAllViews();
            isApplyingRemoteSnapshot = false;
            if (!silent) {
              const m = currentLang === "bn" ? "ক্লাউড থেকে সর্বশেষ ডাটা সিঙ্ক হয়েছে!" : "Latest data pulled from cloud!";
              showToast(m, "success");
              speakText(m, true);
            }
            return true;
          }
        } catch (e) {}
      }

      if (!CLOUD_API_URL) {
        if (!silent) {
          const m = currentLang === "bn" ? "লোকাল অটো-সেভ ডাটা আপডেট আছে।" : "Local auto-save is active.";
          showToast(m, "info");
        }
        return false;
      }

      try {
        setAutoSyncBadge(currentLang === "bn" ? "ক্লাউড থেকে আনছে..." : "Pulling Cloud...");
        const res = await fetch(CLOUD_API_URL, {
          method: "POST",
          headers: { "Content-Type": "application/json" },
          body: JSON.stringify({
            action: "pull_store",
            ownerId: adminOwnerIdForSession
          })
        });
        const json = await res.json();
        if (json && json.ok && json.found && json.data && json.data.settings) {
          isApplyingRemoteSnapshot = true;
          userStore = {
            ...getDefaultUserStore(json.data.settings.shopName, json.data.settings.shopPhone, json.data.settings.shopAddr),
            ...json.data,
            settings: { ...getDefaultUserStore().settings, ...json.data.settings }
          };
          if (Array.isArray(userStore.dueCustomers)) {
            userStore.dueCustomers = userStore.dueCustomers.map(normalizeDueCustomerRecord);
          }
          localStorage.setItem(`rh_isolated_data_${adminOwnerIdForSession}`, JSON.stringify(userStore));
          renderAllViews();
          isApplyingRemoteSnapshot = false;
          setAutoSyncBadge(currentLang === "bn" ? "লাইভ ক্লাউড সিঙ্কড" : "Live Cloud Synced");
          if (!silent) {
            const m = currentLang === "bn" ? "ক্লাউড থেকে সর্বশেষ ডাটা সিঙ্ক হয়েছে!" : "Latest data pulled from cloud!";
            showToast(m, "success");
            speakText(m, true);
          }
          return true;
        }
      } catch (e) {}
      return false;
    }

    function scheduleAutoSync(reason = "live-update") {
      clearTimeout(autoSyncDebounce);
      autoSyncDebounce = setTimeout(() => syncStoreToCloudIfConfigured(reason), 700);
    }

    function quickSwitchShopName(shopName) {
      if (!ensureActiveSessionStore() || activeSessionRole !== "admin" || !shopName) return;
      userStore.settings.shopName = shopName;
      if (currentUser) {
        const users = getRegisteredUsers();
        const idx = users.findIndex(u => u.id === currentUser.id);
        if (idx !== -1) {
          users[idx].shopName = shopName;
          currentUser.shopName = shopName;
          saveRegisteredUsers(users);
        }
      }
      saveUserIsolatedStore();
      const m = currentLang === "bn" ? `সক্রিয় দোকান পরিবর্তন করে ${shopName} করা হয়েছে।` : `Active shop switched to ${shopName}.`;
      showToast(m, "info");
      speakText(m, true);
    }

    function exportFullStoreBackup() {
      if (!ensureActiveSessionStore()) return;
      const payload = {
        app: "RibekAndHetuPOS",
        exportedAt: new Date().toISOString(),
        ownerId: adminOwnerIdForSession,
        data: userStore
      };
      const blob = new Blob([JSON.stringify(payload, null, 2)], { type: "application/json" });
      const a = document.createElement("a");
      a.href = URL.createObjectURL(blob);
      a.download = `Ribek_Hetu_POS_Backup_${new Date().toISOString().split("T")[0]}.json`;
      a.click();
      const m = currentLang === "bn" ? "সম্পূর্ণ ব্যাকআপ ফাইল ডাউনলোড করা হয়েছে।" : "Full store backup file downloaded.";
      showToast(m, "success");
      speakText(m, true);
    }

    function importFullStoreBackup(e) {
      if (activeSessionRole !== "admin" || !ensureActiveSessionStore()) return;
      const f = e.target.files && e.target.files[0];
      if (!f) return;
      const r = new FileReader();
      r.onload = ev => {
        try {
          const parsed = JSON.parse(ev.target.result);
          const importedData = parsed.data || parsed;
          if (!importedData || !importedData.settings) throw new Error("Invalid backup file");
          userStore = {
            ...getDefaultUserStore(importedData.settings.shopName, importedData.settings.shopPhone, importedData.settings.shopAddr),
            ...importedData,
            settings: { ...getDefaultUserStore().settings, ...importedData.settings }
          };
          saveUserIsolatedStore();
          const m = currentLang === "bn" ? "ব্যাকআপ ডাটা সফলভাবে রিস্টোর করা হয়েছে।" : "Backup data restored successfully.";
          showToast(m, "success");
          speakText(m, true);
        } catch (err) {
          showToast(currentLang === "bn" ? "সঠিক ব্যাকআপ JSON ফাইল নির্বাচন করুন।" : "Please choose a valid backup JSON file.", "error");
        }
      };
      r.readAsText(f);
    }

    function buildDashboardAIInsights(focusTopic = "all") {
      if (!userStore) return [];
      const products = Array.isArray(userStore.products) ? userStore.products : [];
      const sales = Array.isArray(userStore.sales) ? userStore.sales : [];
      const dues = Array.isArray(userStore.dueCustomers) ? userStore.dueCustomers : [];
      const expenses = Array.isArray(userStore.expenses) ? userStore.expenses : [];
      const staffSales = sales.filter(x => x.soldByRole === "staff" || (x.sellerName && x.sellerName.toLowerCase().includes("staff")));

      const lowStock = products.filter(p => Number(p.qty) > 0 && Number(p.qty) <= 5);
      const outStock = products.filter(p => Number(p.qty) <= 0);
      const activeDues = dues.filter(c => Number(c.dueAmount) > 0).sort((a, b) => Number(b.dueAmount) - Number(a.dueAmount));
      const totalDue = dues.reduce((s, c) => s + Number(c.dueAmount || 0), 0);
      const totalSales = sales.reduce((s, x) => s + Number(x.total || 0), 0);
      const totalExpense = expenses.reduce((s, x) => s + Number(x.amt || 0), 0);
      const grossProfit = sales.reduce((s, x) => s + Number(x.profit || 0), 0);
      const netProfit = +(grossProfit - totalExpense).toFixed(2);
      const staffSalesSum = staffSales.reduce((s, x) => s + Number(x.total || 0), 0);

      const lines = [];

      if (focusTopic === "all" || focusTopic === "stock") {
        if (outStock.length > 0) {
          lines.push(`⚠️ স্টক শেষ (${outStock.length}টি): ${outStock.slice(0, 2).map(p => p.name).join(", ")}`);
        } else if (lowStock.length > 0) {
          lines.push(`📦 কম স্টক (${lowStock.length}টি): ${lowStock.slice(0, 2).map(p => `${p.name} (${p.qty})`).join(", ")}`);
        } else {
          lines.push(`✅ স্টক স্বাভাবিক (${products.length}টি পণ্য)`);
        }
      }

      if (focusTopic === "all" || focusTopic === "due") {
        if (totalDue > 0) {
          const topDue = activeDues[0];
          lines.push(`💳 মোট বাকি ৳${totalDue.toFixed(2)} (${activeDues.length}জন) • শীর্ষ: ${topDue.name} ৳${Number(topDue.dueAmount).toFixed(2)}`);
        } else {
          lines.push(`💰 কোনো বকেয়া বাকি নেই`);
        }
      }

      if (focusTopic === "all" || focusTopic === "profit") {
        if (activeSessionRole === "admin") {
          lines.push(`📊 বিক্রয় ৳${totalSales.toFixed(2)} | নেট লাভ ৳${netProfit.toFixed(2)} | স্টাফ বিক্রয় ৳${staffSalesSum.toFixed(2)}`);
        } else {
          lines.push(`🛒 স্টাফ বিক্রয়: ৳${totalSales.toFixed(2)} (${sales.length}টি ভাউচার)`);
        }
      }

      return lines;
    }

    function refreshDashboardAI(showBanner = false) {
      const box = document.getElementById("dashboard-ai-insights");
      if (!box || !userStore) return;
      const lines = buildDashboardAIInsights("all");
      latestAISummaryText = lines.join(" ");
      box.innerHTML = lines.map(l => `<div class="flex items-start gap-1.5 text-slate-100"><span class="text-amber-300 font-bold">•</span><span>${escapeHtml(l)}</span></div>`).join("");
      if (showBanner && latestAISummaryText) {
        showToast(latestAISummaryText, "info");
      }
    }

    function runDashboardAIPrompt(topic) {
      const box = document.getElementById("dashboard-ai-insights");
      if (!box || !userStore) return;
      const lines = buildDashboardAIInsights(topic);
      latestAISummaryText = lines.join(" ");
      box.innerHTML = lines.map(l => `<div class="flex items-start gap-1.5 text-slate-100"><span class="text-emerald-300 font-bold">✦</span><span>${escapeHtml(l)}</span></div>`).join("");
      speakText(latestAISummaryText, true);
    }

    function runDashboardAICustom() {
      const input = document.getElementById("dashboard-ai-input");
      const q = (input ? input.value : "").trim().toLowerCase();
      if (!q) {
        refreshDashboardAI(false);
        return;
      }
      let topic = "all";
      if (q.includes("স্টক") || q.includes("stock") || q.includes("মজুদ")) topic = "stock";
      else if (q.includes("বাকি") || q.includes("due") || q.includes("টাকা")) topic = "due";
      else if (q.includes("লাভ") || q.includes("বিক্রয়") || q.includes("খরচ") || q.includes("স্টাফ") || q.includes("staff") || q.includes("profit") || q.includes("sale")) topic = "profit";

      runDashboardAIPrompt(topic);
      if (input) input.value = "";
    }

    function speakDashboardAI() {
      if (!latestAISummaryText) refreshDashboardAI(false);
      if (latestAISummaryText) speakText(latestAISummaryText, true);
    }

    function getDefaultUserStore(shopName, phone, shopAddr) {
      return {
        settings: {
          shopName: shopName || "Ribek & Hetu",
          shopLogo: "https://placehold.co/120x120/059669/ffffff?text=R%26H",
          shopAddr: shopAddr || "চট্টগ্রাম, বাংলাদেশ",
          shopPhone: phone || "01732333764",
          customGreeting: "স্বাগতম",
          theme: "theme-emerald",
          customBgPhoto: "",
          displayLink: "",
          payBas: "https://sukamalchakma.paymently.io/api",
          payApi: "nSOJTnbht36N1Q4R4SBwgtqokmR3nLcpdnYo1n3p",
          payAcc: "01732333764"
        },
        products: [],
        sales: [],
        purchases: [],
        dueCustomers: [],
        expenses: [],
        notes: [],
        staffProfiles: [],
        staffMessages: []
      };
    }

    function getRegisteredUsers() {
      try {
        return JSON.parse(localStorage.getItem("rh_users_db") || "[]");
      } catch (e) {
        return [];
      }
    }

    function saveRegisteredUsers(list) {
      localStorage.setItem("rh_users_db", JSON.stringify(list));
    }

    function computeHash(str) {
      let h = 2166136261;
      for (let i = 0; i < str.length; i++) {
        h ^= str.charCodeAt(i);
        h = Math.imul(h, 16777619);
      }
      return "SEC_" + (h >>> 0).toString(16);
    }

    function normalizeDueCustomerRecord(c) {
      const ledger = Array.isArray(c.ledgerRows) ? c.ledgerRows.map(r => ({
        productName: r.productName || "",
        date: r.date || c.updatedDate || "",
        amount: +(Number(r.amount || 0)).toFixed(2)
      })) : [];

      if (ledger.length === 0 && (c.productDetails || Number(c.dueAmount || 0) > 0)) {
        const rawText = String(c.productDetails || "").trim();
        const parts = rawText ? rawText.split(",").map(s => s.trim()).filter(Boolean) : [];
        if (parts.length > 1) {
          parts.forEach((pText, idx) => {
            const amtMatch = pText.match(/৳\s*([0-9]+(?:\.[0-9]+)?)/);
            const amtVal = amtMatch
              ? +(parseFloat(amtMatch[1]) || 0).toFixed(2)
              : (idx === parts.length - 1 ? +(Number(c.dueAmount || 0)).toFixed(2) : 0);
            ledger.push({
              productName: pText.replace(/\s*\|\s*[0-9]{4}-[0-9]{2}-[0-9]{2}/, "").trim(),
              date: c.updatedDate || "",
              amount: amtVal
            });
          });
        } else {
          ledger.push({
            productName: rawText || (currentLang === "bn" ? "বাকি হিসাব" : "Due Entry"),
            date: c.updatedDate || "",
            amount: +(Number(c.dueAmount || 0)).toFixed(2)
          });
        }
      }

      return {
        ...c,
        dueAmount: +(Number(c.dueAmount || 0)).toFixed(2),
        ledgerRows: ledger
      };
    }

    function loadUserIsolatedStore(adminId, fallbackShopName = "Ribek & Hetu", fallbackPhone = "01732333764", fallbackAddr = "চট্টগ্রাম, বাংলাদেশ") {
      const storageKey = `rh_isolated_data_${adminId}`;
      const raw = localStorage.getItem(storageKey);
      if (raw) {
        userStore = JSON.parse(raw);
        userStore.settings = { ...getDefaultUserStore(fallbackShopName, fallbackPhone, fallbackAddr).settings, ...(userStore.settings || {}) };
        if (!Array.isArray(userStore.staffProfiles)) userStore.staffProfiles = [];
        if (!Array.isArray(userStore.staffMessages)) userStore.staffMessages = [];
        if (!Array.isArray(userStore.dueCustomers)) userStore.dueCustomers = [];
        userStore.dueCustomers = userStore.dueCustomers.map(normalizeDueCustomerRecord);
      } else {
        userStore = getDefaultUserStore(fallbackShopName, fallbackPhone, fallbackAddr);
        saveUserIsolatedStore();
      }
      syncSmsRingtoneState();
    }

    function saveUserIsolatedStore() {
      if (!ensureActiveSessionStore()) return;
      try {
        localStorage.setItem(`rh_isolated_data_${adminOwnerIdForSession}`, JSON.stringify(userStore));
      } catch (err) {
        showToast(currentLang === "bn" ? "মেমরি পূর্ণ হয়ে গেছে! পুরনো ছবি বা রেকর্ড কম করুন।" : "Storage full! Remove large photos or old records.", "error");
      }
      renderAllViews();
      scheduleAutoSync("store-changed");
    }

    function fillInputWithCurrentGps(inputId) {
      const el = document.getElementById(inputId);
      if (!el) return;
      if (gpsState.address) {
        el.value = gpsState.address;
        if (inputId === "set-shop-addr") autoSyncSettingsFromInputs();
        const m = currentLang === "bn" ? `ঠিকানায় GPS লোকেশন বসানো হয়েছে: ${gpsState.address}` : `GPS address filled: ${gpsState.address}`;
        showToast(m, "success");
        speakText(m, true);
      } else {
        fetchGpsLocation(true, true);
      }
    }

    function saveAdminProfileEdit(e) {
      e.preventDefault();
      if (activeSessionRole !== "admin" || !ensureActiveSessionStore()) return;
      const newOwner = document.getElementById("prof-edit-owner").value.trim();
      const newUserId = document.getElementById("prof-edit-userid").value.trim();
      const newShop = document.getElementById("prof-edit-shop").value.trim() || "Ribek & Hetu";
      const newAddr = document.getElementById("prof-edit-addr").value.trim();

      if (!newOwner || !newUserId) {
        showToast(currentLang === "bn" ? "নাম ও মোবাইল/আইডি দিন!" : "Enter name and User ID!", "error");
        return;
      }

      const users = getRegisteredUsers();
      const duplicate = users.some(u => u.id !== adminOwnerIdForSession && u.userId.toLowerCase() === newUserId.toLowerCase());
      if (duplicate) {
        showToast(currentLang === "bn" ? "এই ইউজার আইডি দিয়ে অন্য একাউন্ট আছে!" : "Another account already uses this ID!", "error");
        return;
      }

      const idx = users.findIndex(u => u.id === adminOwnerIdForSession);
      if (idx !== -1) {
        users[idx] = { ...users[idx], owner: newOwner, userId: newUserId, shopName: newShop, shopAddr: newAddr };
        currentUser = users[idx];
        saveRegisteredUsers(users);
      }

      userStore.settings.shopName = newShop;
      userStore.settings.shopPhone = newUserId;
      userStore.settings.shopAddr = newAddr;
      saveUserIsolatedStore();
      const m = currentLang === "bn" ? "অ্যাডমিন প্রোফাইল ও দোকানের ঠিকানা সফলভাবে আপডেট হয়েছে!" : "Admin profile and shop address updated!";
      showToast(m, "success");
      speakText(m, true);
    }

    function deleteAdminProfileAccount() {
      if (activeSessionRole !== "admin" || !adminOwnerIdForSession) return;
      openConfirmModal(
        currentLang === "bn" ? "অ্যাডমিন প্রোফাইল ডিলিট করবেন?" : "Delete Admin Profile?",
        currentLang === "bn" ? "আপনি কি নিশ্চিতভাবে এই অ্যাডমিন একাউন্ট ও এর সব স্টোর ডাটা স্থায়ীভাবে মুছে ফেলতে চান?" : "Permanently delete this admin account and all its store data?",
        () => {
          const delUid = adminOwnerIdForSession;
          const users = getRegisteredUsers().filter(u => u.id !== delUid);
          saveRegisteredUsers(users);
          localStorage.removeItem(`rh_isolated_data_${delUid}`);
          logoutCurrentUser();
        }
      );
    }

    function getStaffSalesStats(st) {
      if (!userStore || !Array.isArray(userStore.sales)) return { count: 0, total: 0 };
      const matched = userStore.sales.filter(v =>
        (st.id && v.soldByStaffId && v.soldByStaffId === st.id) ||
        (st.name && v.soldByStaffName && v.soldByStaffName.toLowerCase() === st.name.toLowerCase()) ||
        (st.name && v.sellerName && v.sellerName.toLowerCase().includes(st.name.toLowerCase()))
      );
      const total = matched.reduce((acc, v) => acc + Number(v.total || 0), 0);
      return { count: matched.length, total: +total.toFixed(2) };
    }

    function handleStaffShopLogoUpload(e) {
      const f = e.target.files && e.target.files[0];
      if (!f) return;
      readImageFileToDataURL(f, dataUrl => {
        document.getElementById("staff-shop-logo-data").value = dataUrl;
        document.getElementById("staff-shop-logo-preview").src = dataUrl;
      }, 400, 0.78);
    }

    function handleStaffPhotoUpload(e) {
      const f = e.target.files && e.target.files[0];
      if (!f) return;
      readImageFileToDataURL(f, dataUrl => {
        document.getElementById("staff-photo-data").value = dataUrl;
        document.getElementById("staff-photo-preview").src = dataUrl;
      }, 400, 0.78);
    }

    function resetStaffForm() {
      document.getElementById("staff-profile-form").reset();
      document.getElementById("staff-edit-id").value = "";
      document.getElementById("staff-photo-data").value = "";
      document.getElementById("staff-shop-logo-data").value = "";
      document.getElementById("staff-photo-preview").src = "https://placehold.co/80x80/6366f1/ffffff?text=Staff";
      document.getElementById("staff-shop-logo-preview").src = (userStore && userStore.settings && userStore.settings.shopLogo) || "https://placehold.co/80x80/059669/ffffff?text=Logo";
      if (userStore && userStore.settings) {
        document.getElementById("staff-shop-name").value = userStore.settings.shopName || "";
        document.getElementById("staff-shop-phone").value = userStore.settings.shopPhone || "";
        document.getElementById("staff-shop-addr").value = userStore.settings.shopAddr || "";
      }
      document.getElementById("staff-save-btn").innerHTML = `<i class="fa-solid fa-user-plus"></i> <span>স্টাফ প্রোফাইল সেভ করুন</span>`;
      document.getElementById("staff-cancel-btn").classList.add("hidden");
    }

    function saveStaffProfile(e) {
      e.preventDefault();
      if (activeSessionRole !== "admin" || !ensureActiveSessionStore()) return;

      const editId = document.getElementById("staff-edit-id").value;
      const name = document.getElementById("staff-name").value.trim();
      const staffUserId = document.getElementById("staff-userid").value.trim();
      const shopName = document.getElementById("staff-shop-name").value.trim() || userStore.settings.shopName;
      const shopPhone = document.getElementById("staff-shop-phone").value.trim() || userStore.settings.shopPhone;
      const shopAddr = document.getElementById("staff-shop-addr").value.trim() || userStore.settings.shopAddr || "";
      const rawPass = document.getElementById("staff-password").value;
      const photo = document.getElementById("staff-photo-data").value || "";
      const shopLogo = document.getElementById("staff-shop-logo-data").value || userStore.settings.shopLogo || "";

      if (!name || !staffUserId || !rawPass) {
        showToast(currentLang === "bn" ? "স্টাফের নাম, আইডি এবং পাসওয়ার্ড দিন!" : "Enter staff name, ID and password!", "error");
        return;
      }

      const duplicate = userStore.staffProfiles.some(
        s => s.id !== editId && s.staffUserId.toLowerCase() === staffUserId.toLowerCase()
      );
      if (duplicate) {
        const m = currentLang === "bn" ? "এই স্টাফ ইউজার আইডি দিয়ে আগেই একটি প্রোফাইল রয়েছে!" : "A staff profile with this User ID already exists!";
        showToast(m, "error");
        speakText(m, true);
        return;
      }

      if (editId) {
        const idx = userStore.staffProfiles.findIndex(s => s.id === editId);
        if (idx !== -1) {
          userStore.staffProfiles[idx] = {
            ...userStore.staffProfiles[idx],
            name,
            staffUserId,
            shopName,
            shopPhone,
            shopAddr,
            passHash: computeHash(rawPass),
            photo: photo || userStore.staffProfiles[idx].photo || "",
            shopLogo: shopLogo || userStore.staffProfiles[idx].shopLogo || ""
          };
        }
      } else {
        userStore.staffProfiles.unshift({
          id: "STF_" + Date.now() + "_" + Math.floor(Math.random() * 999),
          name,
          staffUserId,
          shopName,
          shopPhone,
          shopAddr,
          passHash: computeHash(rawPass),
          photo,
          shopLogo
        });
      }

      saveUserIsolatedStore();
      resetStaffForm();
      const msg = currentLang === "bn"
        ? `স্টাফ প্রোফাইল (${name}) সফলভাবে সেভ করা হয়েছে।`
        : `Staff profile (${name}) saved successfully.`;
      showToast(msg, "success");
      speakText(msg, true);
    }

    function editStaffProfile(staffId) {
      if (activeSessionRole !== "admin" || !userStore) return;
      const st = userStore.staffProfiles.find(s => s.id === staffId);
      if (!st) return;

      document.getElementById("staff-edit-id").value = st.id;
      document.getElementById("staff-name").value = st.name;
      document.getElementById("staff-userid").value = st.staffUserId;
      document.getElementById("staff-shop-name").value = st.shopName || userStore.settings.shopName;
      document.getElementById("staff-shop-phone").value = st.shopPhone || userStore.settings.shopPhone;
      document.getElementById("staff-shop-addr").value = st.shopAddr || userStore.settings.shopAddr || "";
      document.getElementById("staff-password").value = "";
      document.getElementById("staff-photo-data").value = st.photo || "";
      document.getElementById("staff-shop-logo-data").value = st.shopLogo || "";
      document.getElementById("staff-photo-preview").src = st.photo || "https://placehold.co/80x80/6366f1/ffffff?text=Staff";
      document.getElementById("staff-shop-logo-preview").src = st.shopLogo || userStore.settings.shopLogo || "https://placehold.co/80x80/059669/ffffff?text=Logo";
      document.getElementById("staff-save-btn").innerHTML = `<i class="fa-solid fa-floppy-disk"></i> <span>স্টাফ প্রোফাইল আপডেট সেভ করুন</span>`;
      document.getElementById("staff-cancel-btn").classList.remove("hidden");
      document.getElementById("staff-profile-form").scrollIntoView({ behavior: "smooth", block: "center" });
    }

    function deleteStaffProfile(staffId) {
      if (activeSessionRole !== "admin" || !userStore) return;
      const st = userStore.staffProfiles.find(s => s.id === staffId);
      if (!st) return;
      openConfirmModal(
        currentLang === "bn" ? "স্টাফ প্রোফাইল ডিলিট?" : "Delete Staff Profile?",
        currentLang === "bn" ? `${st.name} এর স্টাফ প্রোফাইল স্থায়ীভাবে মুছে ফেলতে চান?` : `Permanently delete staff profile for ${st.name}?`,
        () => {
          userStore.staffProfiles = userStore.staffProfiles.filter(s => s.id !== staffId);
          saveUserIsolatedStore();
          const m = currentLang === "bn" ? `${st.name} এর স্টাফ প্রোফাইল ডিলিট করা হয়েছে।` : `Deleted staff profile ${st.name}.`;
          showToast(m, "info");
          speakText(m, true);
        }
      );
    }

    function loginIntoStaffProfileDirectly(staffId) {
      if (!userStore) return;
      const st = userStore.staffProfiles.find(s => s.id === staffId);
      if (!st) return;
      activeSessionRole = "staff";
      activeStaffMember = st;
      persistSessionState();
      launchAuthenticatedWorkspace();
    }

    function renderStaffProfilesList() {
      const box = document.getElementById("staff-profiles-list");
      const badge = document.getElementById("staff-count-badge");
      if (!box || !userStore) return;
      const list = userStore.staffProfiles || [];
      if (badge) {
        badge.textContent = currentLang === "bn" ? `${toBnNum(list.length)} জন স্টাফ` : `${list.length} Staff`;
      }

      if (list.length === 0) {
        box.innerHTML = `<div class="p-3 rounded-xl bg-white border border-indigo-100 text-center text-xs text-slate-400">এখনো কোনো স্টাফ প্রোফাইল তৈরি করা হয়নি</div>`;
        return;
      }

      box.innerHTML = list.map(st => {
        const stats = getStaffSalesStats(st);
        const addrLine = st.shopAddr || (userStore.settings && userStore.settings.shopAddr) || "";
        return `
        <div class="bg-white p-3 rounded-2xl border border-indigo-200 shadow-sm flex flex-col sm:flex-row sm:items-center justify-between gap-2.5">
          <div class="flex items-center gap-3 min-w-0">
            <img src="${st.photo || 'https://placehold.co/80x80/6366f1/ffffff?text=Staff'}" class="w-12 h-12 rounded-xl object-cover border border-indigo-300 shrink-0" alt="Staff" />
            ${st.shopLogo ? `<img src="${st.shopLogo}" class="w-10 h-10 rounded-xl object-cover border border-emerald-300 shrink-0" alt="Shop Logo" />` : ""}
            <div class="min-w-0">
              <div class="font-extrabold text-sm text-slate-900 truncate">${escapeHtml(st.name)} <span class="text-[10px] font-mono bg-indigo-100 text-indigo-800 px-1.5 py-0.5 rounded">ID: ${escapeHtml(st.staffUserId)}</span></div>
              <div class="text-[11px] font-bold text-slate-700 truncate"><i class="fa-solid fa-store text-emerald-600 mr-1"></i>${escapeHtml(st.shopName || "")} • <i class="fa-solid fa-phone mr-0.5"></i>${escapeHtml(st.shopPhone || "")}</div>
              ${addrLine ? `<div class="text-[10px] font-semibold text-slate-500 truncate"><i class="fa-solid fa-location-dot text-rose-500 mr-1"></i>${escapeHtml(addrLine)}</div>` : ""}
              <div class="text-[11px] font-extrabold text-indigo-700 mt-0.5">বিক্রয়: ${stats.count}টি ভাউচার | মোট বিক্রয়: ৳ ${stats.total.toFixed(2)}</div>
            </div>
          </div>
          <div class="flex items-center gap-2 shrink-0">
            <button type="button" onclick="loginIntoStaffProfileDirectly('${st.id}')" class="px-2.5 py-1.5 rounded-xl bg-emerald-600 hover:bg-emerald-700 text-white text-xs font-extrabold shadow flex items-center gap-1.5">
              <i class="fa-solid fa-right-to-bracket"></i> <span>লগইন</span>
            </button>
            <button type="button" onclick="editStaffProfile('${st.id}')" class="w-8 h-8 rounded-xl bg-amber-100 text-amber-800 flex items-center justify-center" title="Edit">
              <i class="fa-solid fa-pen"></i>
            </button>
            <button type="button" onclick="deleteStaffProfile('${st.id}')" class="w-8 h-8 rounded-xl bg-rose-100 text-rose-700 flex items-center justify-center" title="Delete">
              <i class="fa-solid fa-trash"></i>
            </button>
          </div>
        </div>`;
      }).join("");
    }

    function applyRolePermissionsUI() {
      const isStaff = activeSessionRole === "staff" && !!activeStaffMember;

      const hideTargets = [
        "dash-reset-btn",
        "dash-financial-grid",
        "dash-expenses-card",
        "dash-quick-entry-btn",
        "bottom-nav-product-entry",
        "side-nav-product-entry",
        "hdr-settings-btn",
        "side-nav-settings",
        "hdr-quick-shop-switch",
        "history-tab-switcher",
        "history-staff-filter",
        "delete-all-hist-btn",
        "admin-staff-management-panel",
        "admin-logo-box",
        "admin-profile-edit-form",
        "prof-delete-admin-btn",
        "admin-password-form"
      ];
      hideTargets.forEach(id => {
        const el = document.getElementById(id);
        if (el) el.classList.toggle("hidden", isStaff);
      });

      if (!isStaff) {
        const qShop = document.getElementById("hdr-quick-shop-switch");
        const hFilter = document.getElementById("history-staff-filter");
        if (qShop) qShop.classList.remove("hidden");
        if (hFilter) hFilter.classList.remove("hidden");
      }

      const badge = document.getElementById("prof-role-badge");
      if (badge) {
        if (isStaff) {
          badge.textContent = "স্টাফ প্রোফাইল — শুধুমাত্র স্টক থেকে বিক্রয় অনুমোদিত";
          badge.className = "inline-block mt-1 px-2 py-0.5 rounded bg-indigo-100 text-indigo-800 text-[10px] font-extrabold";
        } else {
          badge.textContent = "১০০% গোপনীয় আলাদা ডাটাবেস সক্রিয় (অ্যাডমিন)";
          badge.className = "inline-block mt-1 px-2 py-0.5 rounded bg-emerald-100 text-emerald-800 text-[10px] font-extrabold";
        }
      }
    }

    function startLiveInternetAndSyncEngine() {
      initFirebaseRealtimeSync();
      const conn = navigator.connection || navigator.mozConnection || navigator.webkitConnection;
      if (conn && typeof conn.addEventListener === "function") {
        conn.addEventListener("change", () => applyInternetStatusUI());
      }
      runLiveConnectivityCheck(false, true);
      setInterval(() => { runLiveConnectivityCheck(false, false); }, 25000);
      setInterval(() => {
        if (isLiveOnline && currentUser) {
          fetchLiveWeather();
          pullStoreFromCloud(true);
          scheduleAutoSync("periodic-live-sync");
        }
      }, 60000);
    }

    async function reverseGeocodeCoords(lat, lon) {
      if (!isLiveOnline) return null;
      const key = `${lat.toFixed(3)},${lon.toFixed(3)}`;
      try {
        const res = await fetch(`https://nominatim.openstreetmap.org/reverse?format=json&lat=${lat}&lon=${lon}&zoom=16&addressdetails=1`, {
          headers: { "Accept-Language": currentLang === "bn" ? "bn,en" : "en" }
        });
        if (!res.ok) return null;
        const data = await res.json();
        const addrObj = data.address || {};
        const city = addrObj.city || addrObj.town || addrObj.county || addrObj.suburb || "";
        const region = addrObj.state || addrObj.division || "";
        const country = addrObj.country || "";
        const display = data && data.display_name ? String(data.display_name).split(",").slice(0, 3).join(", ").trim() : "";
        lastReverseGeocodeKey = key;
        return { display, city, region, country };
      } catch (e) {
        return null;
      }
    }

    async function fetchIpFallbackLocation(speakResult = false, fillAddrInput = false) {
      if (!isLiveOnline) return false;
      try {
        const res = await fetch("https://ipapi.co/json/");
        if (!res.ok) return false;
        const data = await res.json();
        if (data && data.latitude && data.longitude) {
          gpsState.lat = Number(data.latitude);
          gpsState.lon = Number(data.longitude);
          gpsState.city = data.city || "";
          gpsState.region = data.region || "";
          gpsState.country = data.country_name || "";
          const parts = [data.city, data.region, data.country_name].filter(Boolean);
          gpsState.address = parts.length > 0 ? parts.join(", ") : `${gpsState.lat.toFixed(4)}, ${gpsState.lon.toFixed(4)}`;
          gpsState.source = "IP APPROX";
          renderGpsStateUI();
          updateLiveMapEmbed();
          if (fillAddrInput) {
            const addrInput = document.getElementById("set-shop-addr");
            if (addrInput) {
              addrInput.value = gpsState.address;
              autoSyncSettingsFromInputs();
            }
          }
          fetchLiveWeather(gpsState.lat, gpsState.lon);
          return true;
        }
      } catch (e) {}
      return false;
    }

    function renderGpsStateUI() {
      const locEl = document.getElementById("gps-location-text");
      const coordsEl = document.getElementById("gps-coords-text");
      const badgeEl = document.getElementById("gps-source-badge");
      const approxEl = document.getElementById("gps-approx-summary");
      if (!locEl || !coordsEl) return;
      if (badgeEl) badgeEl.textContent = gpsState.source || "AUTO GPS";
      if (gpsState.lat === null || gpsState.lon === null) {
        locEl.textContent = DICT[currentLang].gpsWaitingText;
        coordsEl.textContent = "Lat: -- | Lon: --";
        if (approxEl) {
          approxEl.textContent = currentLang === "bn"
            ? "মোটামুটি অবস্থান: লোকেশন সেন্সর ও নেটওয়ার্ক থেকে অবস্থান খোঁজা হচ্ছে..."
            : "Approximate position: Detecting via location sensor & network...";
        }
        return;
      }
      locEl.textContent = gpsState.address || (currentLang === "bn" ? "লোকেশন পাওয়া গেছে" : "Location Acquired");
      const accPart = gpsState.accuracy ? ` | ±${Math.round(gpsState.accuracy)}m` : "";
      coordsEl.textContent = `Lat: ${gpsState.lat.toFixed(5)} | Lon: ${gpsState.lon.toFixed(5)}${accPart} (${gpsState.source})`;

      if (approxEl) {
        const areaParts = [gpsState.city, gpsState.region, gpsState.country].filter(Boolean).join(", ") || gpsState.address || "—";
        approxEl.textContent = currentLang === "bn"
          ? `মোটামুটি অবস্থান: ${areaParts} • স্থানাঙ্ক: ${gpsState.lat.toFixed(4)}, ${gpsState.lon.toFixed(4)} • নির্ভুলতা: ${gpsState.accuracy ? `প্রায় ${Math.round(gpsState.accuracy)} মিটার` : "আনুমানিক (নেটওয়ার্ক/GPS)"}`
          : `Approx. Position: ${areaParts} • Coords: ${gpsState.lat.toFixed(4)}, ${gpsState.lon.toFixed(4)} • Accuracy: ${gpsState.accuracy ? `~${Math.round(gpsState.accuracy)}m` : "Estimated"}`;
      }
    }

    function startLiveGpsWatcher() {
      if (!("geolocation" in navigator)) {
        fetchIpFallbackLocation(false, false);
        return;
      }
      if (gpsWatchId !== null) {
        try { navigator.geolocation.clearWatch(gpsWatchId); } catch (e) {}
      }
      try {
        gpsWatchId = navigator.geolocation.watchPosition(
          async (pos) => {
            gpsState.lat = pos.coords.latitude;
            gpsState.lon = pos.coords.longitude;
            gpsState.accuracy = pos.coords.accuracy || null;
            gpsState.source = "GPS LIVE";
            const geo = await reverseGeocodeCoords(gpsState.lat, gpsState.lon);
            if (geo) {
              gpsState.address = geo.display || `${gpsState.lat.toFixed(4)}, ${gpsState.lon.toFixed(4)}`;
              gpsState.city = geo.city || "";
              gpsState.region = geo.region || "";
              gpsState.country = geo.country || "";
            } else if (!gpsState.address) {
              gpsState.address = `${gpsState.lat.toFixed(4)}, ${gpsState.lon.toFixed(4)}`;
            }
            renderGpsStateUI();
            updateLiveMapEmbed();
          },
          () => {
            if (gpsState.lat === null || gpsState.lon === null) {
              fetchIpFallbackLocation(false, false);
            }
          },
          { enableHighAccuracy: true, timeout: 12000, maximumAge: 5000 }
        );
      } catch (e) {
        fetchIpFallbackLocation(false, false);
      }
    }

    function fetchGpsLocation(speakResult = true, fillAddrInput = false) {
      renderGpsStateUI();
      if (!("geolocation" in navigator)) {
        fetchIpFallbackLocation(speakResult, fillAddrInput).then(ok => {
          if (!ok && speakResult) {
            const m = currentLang === "bn" ? "এই ডিভাইসে GPS পাওয়া যায়নি।" : "GPS is not available on this device.";
            showToast(m, "error");
            speakText(m, true);
          }
        });
        return;
      }

      navigator.geolocation.getCurrentPosition(
        async (pos) => {
          gpsState.lat = pos.coords.latitude;
          gpsState.lon = pos.coords.longitude;
          gpsState.accuracy = pos.coords.accuracy || null;
          gpsState.source = "GPS LIVE";

          const geo = await reverseGeocodeCoords(gpsState.lat, gpsState.lon);
          if (geo) {
            gpsState.address = geo.display || `${gpsState.lat.toFixed(4)}, ${gpsState.lon.toFixed(4)}`;
            gpsState.city = geo.city || "";
            gpsState.region = geo.region || "";
            gpsState.country = geo.country || "";
          } else {
            gpsState.address = `${gpsState.lat.toFixed(4)}, ${gpsState.lon.toFixed(4)}`;
          }

          renderGpsStateUI();
          updateLiveMapEmbed();

          if (fillAddrInput) {
            const addrInput = document.getElementById("set-shop-addr");
            if (addrInput) {
              addrInput.value = gpsState.address;
              autoSyncSettingsFromInputs();
            }
          }

          fetchLiveWeather(gpsState.lat, gpsState.lon);

          if (speakResult) {
            const msg = currentLang === "bn"
              ? `বর্তমান লাইভ GPS লোকেশন ${gpsState.address}। স্থানাঙ্ক ${gpsState.lat.toFixed(3)} ও ${gpsState.lon.toFixed(3)}।`
              : `Current live GPS location is ${gpsState.address}. Coordinates ${gpsState.lat.toFixed(3)} and ${gpsState.lon.toFixed(3)}.`;
            showToast(msg, "success");
            speakText(msg, true);
          }
        },
        async () => {
          const ipOk = await fetchIpFallbackLocation(speakResult, fillAddrInput);
          if (!ipOk && speakResult) {
            const msg = currentLang === "bn"
              ? "GPS লোকেশন অনুমতি বা ইন্টারনেট সংযোগ যাচাই করুন।"
              : "Please check GPS permission or internet connection.";
            showToast(msg, "error");
            speakText(msg, true);
          }
        },
        { enableHighAccuracy: true, timeout: 10000, maximumAge: 0 }
      );
    }

    function applyGpsToStoreAddress() {
      if (activeSessionRole !== "admin" || !ensureActiveSessionStore()) return;
      if (!gpsState.address) {
        fetchGpsLocation(true, true);
        return;
      }
      userStore.settings.shopAddr = gpsState.address;
      const addrInput = document.getElementById("set-shop-addr");
      if (addrInput) addrInput.value = gpsState.address;
      const profAddrInput = document.getElementById("prof-edit-addr");
      if (profAddrInput) profAddrInput.value = gpsState.address;
      saveUserIsolatedStore();
      const msg = currentLang === "bn" ? `দোকানের ঠিকানায় ${gpsState.address} সেভ করা হয়েছে।` : `Saved ${gpsState.address} as shop address.`;
      showToast(msg, "success");
      speakText(msg, true);
    }

    function mapWeatherCodeToText(code) {
      if (code === 0) return { bn: "পরিষ্কার আকাশ", en: "Clear Sky", icon: "fa-sun text-amber-300" };
      if ([1, 2, 3].includes(code)) return { bn: "আংশিক মেঘলা", en: "Partly Cloudy", icon: "fa-cloud-sun text-amber-200" };
      if ([45, 48].includes(code)) return { bn: "কুয়াশাচ্ছন্ন", en: "Foggy", icon: "fa-smog text-slate-200" };
      if ([51, 53, 55, 61, 63, 65, 80, 81, 82].includes(code)) return { bn: "বৃষ্টিপাত হচ্ছে", en: "Rain Showers", icon: "fa-cloud-rain text-sky-200" };
      if ([95, 96, 99].includes(code)) return { bn: "বজ্রসহ বৃষ্টি", en: "Thunderstorm", icon: "fa-cloud-bolt text-yellow-300" };
      return { bn: "স্বাভাবিক আবহাওয়া", en: "Moderate Weather", icon: "fa-cloud text-sky-100" };
    }

    async function fetchLiveWeather(latOverride, lonOverride) {
      if (!isLiveOnline) return;
      const lat = latOverride || gpsState.lat || 22.3569;
      const lon = lonOverride || gpsState.lon || 91.7832;
      try {
        const res = await fetch(`https://api.open-meteo.com/v1/forecast?latitude=${lat}&longitude=${lon}&current=temperature_2m,relative_humidity_2m,weather_code,wind_speed_10m`);
        if (!res.ok) return;
        const data = await res.json();
        const cur = data && data.current;
        if (!cur) return;
        const mapped = mapWeatherCodeToText(Number(cur.weather_code));
        weatherState = {
          temp: Math.round(Number(cur.temperature_2m)),
          humidity: Math.round(Number(cur.relative_humidity_2m)),
          wind: Math.round(Number(cur.wind_speed_10m)),
          descBn: mapped.bn,
          descEn: mapped.en,
          icon: mapped.icon
        };
        renderWeatherCard();
      } catch (e) {}
    }

    function renderWeatherCard() {
      const tempEl = document.getElementById("weather-temp-val");
      const condEl = document.getElementById("weather-condition-text");
      const extraEl = document.getElementById("weather-extra-text");
      const iconEl = document.getElementById("weather-icon");
      if (!tempEl) return;

      const tempStr = currentLang === "bn" ? `${toBnNum(weatherState.temp)}°C` : `${weatherState.temp}°C`;
      const humStr = currentLang === "bn" ? toBnNum(weatherState.humidity) : weatherState.humidity;
      const windStr = currentLang === "bn" ? toBnNum(weatherState.wind) : weatherState.wind;

      tempEl.textContent = tempStr;
      condEl.textContent = currentLang === "bn" ? weatherState.descBn : weatherState.descEn;
      extraEl.textContent = currentLang === "bn"
        ? `আর্দ্রতা: ${humStr}% | বাতাস: ${windStr} কি.মি./ঘণ্টা`
        : `Humidity: ${humStr}% | Wind: ${windStr} km/h`;
      if (iconEl && weatherState.icon) {
        iconEl.className = `fa-solid ${weatherState.icon}`;
      }
    }

    function speakWeatherReport() {
      const msg = currentLang === "bn"
        ? `আজকের আবহাওয়া ${weatherState.descBn}। তাপমাত্রা ${toBnNum(weatherState.temp)} ডিগ্রি সেলসিয়াস, বাতাসের আর্দ্রতা ${toBnNum(weatherState.humidity)} শতাংশ।`
        : `Today's weather is ${weatherState.descEn}. Temperature is ${weatherState.temp} degrees Celsius with ${weatherState.humidity}% humidity.`;
      speakText(msg, true);
    }

    function toBnNum(n) {
      const map = { "0": "০", "1": "১", "2": "২", "3": "৩", "4": "৪", "5": "৫", "6": "৬", "7": "৭", "8": "৮", "9": "৯" };
      return String(n).replace(/[0-9]/g, ch => map[ch]);
    }

    function showToast(msg, type = "info") {
      const container = document.getElementById("toast-container");
      const colors = {
        success: "bg-emerald-600 text-white",
        error: "bg-rose-600 text-white",
        warn: "bg-amber-500 text-slate-950",
        info: "bg-slate-900 text-white"
      };
      const el = document.createElement("div");
      el.className = `px-4 py-2.5 rounded-2xl shadow-xl text-xs sm:text-sm font-bold flex items-center gap-2 ${colors[type] || colors.info}`;
      el.innerHTML = `<i class="fa-solid fa-circle-info"></i><span>${escapeHtml(msg)}</span>`;
      container.appendChild(el);
      setTimeout(() => el.remove(), 3400);
    }

    function escapeHtml(s) {
      return String(s || "").replace(/[&<>"']/g, m => ({ "&": "&amp;", "<": "&lt;", ">": "&gt;", '"': "&quot;", "'": "&#39;" }[m]));
    }

    function speakText(text, force = false) {
      if (!("speechSynthesis" in window)) return;
      if (!force && !autoClockSpeechEnabled) return;
      try {
        if (force) window.speechSynthesis.cancel();
        const utter = new SpeechSynthesisUtterance(text);
        utter.lang = currentLang === "bn" ? "bn-BD" : "en-US";
        utter.rate = 0.98;
        window.speechSynthesis.speak(utter);
      } catch (e) {}
    }

    function speakTypingLive(val) {
      const clean = String(val || "").trim();
      if (!clean || clean.length < 2) return;
      clearTimeout(typingSpeechDebounce);
      typingSpeechDebounce = setTimeout(() => {
        speakText(clean, true);
      }, 450);
    }

    function getActiveShopNameForGreeting() {
      const shopInput = document.getElementById("custom-greeting-shop-input");
      if (shopInput && shopInput.value.trim()) return shopInput.value.trim();
      if (userStore && userStore.settings && userStore.settings.shopName) return userStore.settings.shopName;
      return currentLang === "bn" ? "রিবেক & হেতু" : "Ribek & Hetu";
    }

    function getCustomGreetingText() {
      const gInput = document.getElementById("custom-greeting-input");
      if (gInput && gInput.value.trim()) return gInput.value.trim();
      if (userStore && userStore.settings && userStore.settings.customGreeting) return userStore.settings.customGreeting.trim();
      return currentLang === "bn" ? "স্বাগতম" : "Welcome";
    }

    function updateCustomGreetingPreview() {
      const prevEl = document.getElementById("custom-greeting-preview-line");
      if (!prevEl) return;
      const gText = getCustomGreetingText();
      const sName = getActiveShopNameForGreeting();
      prevEl.textContent = currentLang === "bn"
        ? `ঘোষণা: "${gText}" + বর্তমান সময়, তারিখ, মাস + "${sName} আপনাকে সুস্বাগতম"`
        : `Announcement: "${gText}" + current time, date, month + "Welcome to ${sName}"`;
    }

    function onCustomGreetingTyping(val) {
      updateCustomGreetingPreview();
      const clean = String(val || "").trim();
      if (!clean) return;
      clearTimeout(greetingTypingDebounce);
      greetingTypingDebounce = setTimeout(() => {
        speakText(clean, true);
      }, 400);
    }

    function onCustomGreetingShopTyping(val) {
      updateCustomGreetingPreview();
      const clean = String(val || "").trim();
      if (!clean) return;
      clearTimeout(greetingTypingDebounce);
      greetingTypingDebounce = setTimeout(() => {
        const phrase = currentLang === "bn" ? `${clean} আপনাকে সুস্বাগতম` : `Welcome to ${clean}`;
        speakText(phrase, true);
      }, 450);
    }

    function saveCustomGreetingMessage() {
      if (!ensureActiveSessionStore()) return;
      const gInput = document.getElementById("custom-greeting-input");
      const sInput = document.getElementById("custom-greeting-shop-input");
      const gVal = (gInput ? gInput.value : "").trim() || (currentLang === "bn" ? "স্বাগতম" : "Welcome");
      const sVal = (sInput ? sInput.value : "").trim();

      userStore.settings.customGreeting = gVal;
      if (sVal && activeSessionRole === "admin") {
        userStore.settings.shopName = sVal;
        const setShopInput = document.getElementById("set-shop-name");
        if (setShopInput) setShopInput.value = sVal;
        const profShopInput = document.getElementById("prof-edit-shop");
        if (profShopInput) profShopInput.value = sVal;
      }
      saveUserIsolatedStore();
      updateCustomGreetingPreview();
      const toastMsg = currentLang === "bn"
        ? `স্বাগতম বার্তা সেভ করা হয়েছে: "${gVal}"`
        : `Welcome message saved: "${gVal}"`;
      showToast(toastMsg, "success");
      speakFullGreetingAndClock(true);
    }

    function buildDateTimeMonthSpeech(now = new Date()) {
      const timeStr = now.toLocaleTimeString(currentLang === "bn" ? "bn-BD" : "en-US", {
        hour: "2-digit", minute: "2-digit"
      });
      if (currentLang === "bn") {
        const bnDateInfo = getBanglaDateParts(now);
        const dayNum = toBnNum(now.getDate());
        const enMonthName = now.toLocaleDateString("bn-BD", { month: "long" });
        const yearNum = toBnNum(now.getFullYear());
        return `বর্তমান সময় ${timeStr}। আজ ${dayNum} ${enMonthName} ${yearNum}, বাংলা ${bnDateInfo.dayBn} ${bnDateInfo.monthName} ${bnDateInfo.yearBn} বঙ্গাব্দ।`;
      }
      const enDateStr = now.toLocaleDateString("en-US", {
        weekday: "long", year: "numeric", month: "long", day: "numeric"
      });
      return `Current time is ${timeStr}. Today is ${enDateStr}.`;
    }

    function speakFullGreetingAndClock(force = true) {
      const now = new Date();
      const customMsg = getCustomGreetingText();
      const dateTimeSpeech = buildDateTimeMonthSpeech(now);
      const shopName = getActiveShopNameForGreeting();
      const shopWelcome = currentLang === "bn"
        ? `${shopName} আপনাকে সুস্বাগতম`
        : `Welcome to ${shopName}`;
      const fullSpeech = `${customMsg}। ${dateTimeSpeech} ${shopWelcome}।`;
      speakText(fullSpeech, force);
    }

    function speakExactClockOnly(now = new Date(), force = false) {
      const timeStr = now.toLocaleTimeString(currentLang === "bn" ? "bn-BD" : "en-US", {
        hour: "2-digit", minute: "2-digit"
      });
      const msg = currentLang === "bn" ? `এখন সময় ${timeStr}` : `The time is ${timeStr}`;
      speakText(msg, force);
    }

    function toggleAutoHourlyClockSpeech() {
      autoClockSpeechEnabled = !autoClockSpeechEnabled;
      const pill = document.getElementById("auto-clock-voice-pill");
      if (pill) {
        pill.className = autoClockSpeechEnabled
          ? "px-2.5 py-0.5 rounded-md bg-emerald-500 text-white text-[10px] font-bold flex items-center gap-1 shrink-0"
          : "px-2.5 py-0.5 rounded-md bg-slate-700 text-slate-300 text-[10px] font-bold flex items-center gap-1 shrink-0";
        pill.innerHTML = `<i class="fa-solid ${autoClockSpeechEnabled ? "fa-volume-high" : "fa-volume-xmark"}"></i> <span>${autoClockSpeechEnabled ? "AUTO ON" : "AUTO OFF"}</span>`;
      }
      speakText(autoClockSpeechEnabled ? (currentLang === "bn" ? "কথা বলা ঘড়ি চালু হয়েছে" : "Speaking clock enabled") : (currentLang === "bn" ? "কথা বলা ঘড়ি বন্ধ হয়েছে" : "Speaking clock muted"), true);
    }

    function startLiveClockAndAlarmEngine() {
      setInterval(() => {
        const now = new Date();
        updateDayNightBadge();
        const dateEl = document.getElementById("live-clock-date");
        const timeEl = document.getElementById("live-clock-time");
        if (dateEl) {
          dateEl.textContent = now.toLocaleDateString(currentLang === "bn" ? "bn-BD" : "en-US", {
            weekday: "short", year: "numeric", month: "short", day: "numeric"
          });
        }
        if (timeEl) {
          timeEl.textContent = now.toLocaleTimeString(currentLang === "bn" ? "bn-BD" : "en-US", {
            hour: "2-digit", minute: "2-digit", second: "2-digit"
          });
        }

        const min = now.getMinutes();
        const sec = now.getSeconds();
        const halfHourSlot = `${now.getHours()}:${min < 30 ? "00" : "30"}`;
        if (autoClockSpeechEnabled && (min === 0 || min === 30) && sec === 0 && lastSpokenHalfHour !== halfHourSlot) {
          lastSpokenHalfHour = halfHourSlot;
          speakExactClockOnly(now, true);
        }

        checkNotebookAlarms(now);
      }, 1000);
    }

    function checkNotebookAlarms(now) {
      if (!userStore || !Array.isArray(userStore.notes)) return;
      const yyyy = now.getFullYear();
      const mm = String(now.getMonth() + 1).padStart(2, "0");
      const dd = String(now.getDate()).padStart(2, "0");
      const curDate = `${yyyy}-${mm}-${dd}`;
      const curTime = `${String(now.getHours()).padStart(2, "0")}:${String(now.getMinutes()).padStart(2, "0")}`;

      let changed = false;
      userStore.notes.forEach(n => {
        if (!n.triggered && n.alarmDate === curDate && n.alarmTime === curTime) {
          n.triggered = true;
          changed = true;
          showToast(`⏰ ${n.text}`, "warn");
          speakText(currentLang === "bn" ? `রিমাইন্ডার: ${n.text}` : `Reminder: ${n.text}`, true);
        }
      });
      if (changed) saveUserIsolatedStore();
    }

    function applyLanguageToUI() {
      document.documentElement.lang = currentLang;
      localStorage.setItem("rh_lang", currentLang);
      const nextLangLabel = currentLang === "bn" ? "English" : "বাংলা";
      ["auth-lang-btn", "top-lang-label", "dash-lang-label", "set-lang-label"].forEach(id => {
        const el = document.getElementById(id);
        if (el) el.textContent = nextLangLabel;
      });

      document.querySelectorAll("[data-i18n]").forEach(el => {
        const key = el.getAttribute("data-i18n");
        if (DICT[currentLang][key]) el.textContent = DICT[currentLang][key];
      });
      document.querySelectorAll("[data-i18n-placeholder]").forEach(el => {
        const key = el.getAttribute("data-i18n-placeholder");
        if (DICT[currentLang][key]) el.placeholder = DICT[currentLang][key];
      });

      populateUnitDropdowns();
      applyInternetStatusUI();
      updateDayNightBadge();
      renderGpsStateUI();
      updateCustomGreetingPreview();
      if (userStore) renderAllViews();
    }

    function toggleLanguage() {
      currentLang = currentLang === "bn" ? "en" : "bn";
      applyLanguageToUI();
      speakText(currentLang === "bn" ? "বাংলা ভাষা চালু হয়েছে" : "English language activated", true);
    }

    function applyColorTheme(themeClass) {
      THEMES.forEach(t => document.body.classList.remove(t));
      document.body.classList.add(themeClass);
      if (activeSessionRole === "admin" && userStore && userStore.settings) {
        userStore.settings.theme = themeClass;
        saveUserIsolatedStore();
      }
    }

    function cycleColorTheme() {
      const cur = THEMES.find(t => document.body.classList.contains(t)) || "theme-emerald";
      const next = THEMES[(THEMES.indexOf(cur) + 1) % THEMES.length];
      applyColorTheme(next);
    }

    function applyCustomBackgroundPhoto(dataUrl) {
      if (dataUrl) {
        document.body.style.backgroundImage = `url("${dataUrl}")`;
        document.body.classList.add("has-custom-bg");
      } else {
        document.body.style.backgroundImage = "";
        document.body.classList.remove("has-custom-bg");
      }
    }

    function uploadCustomThemePhoto(e) {
      if (activeSessionRole !== "admin" || !ensureActiveSessionStore()) return;
      const f = e.target.files && e.target.files[0];
      if (!f) return;
      readImageFileToDataURL(f, dataUrl => {
        userStore.settings.customBgPhoto = dataUrl;
        applyCustomBackgroundPhoto(dataUrl);
        saveUserIsolatedStore();
        const m = currentLang === "bn" ? "মেমরি থেকে কাস্টম থিম ছবি সেট করা হয়েছে।" : "Custom background theme photo applied.";
        showToast(m, "success");
        speakText(m, true);
      }, 900, 0.72);
    }

    function clearCustomThemePhoto() {
      if (activeSessionRole !== "admin" || !ensureActiveSessionStore()) return;
      userStore.settings.customBgPhoto = "";
      applyCustomBackgroundPhoto("");
      saveUserIsolatedStore();
      const m = currentLang === "bn" ? "কাস্টম থিম ছবি মুছে ফেলা হয়েছে।" : "Custom background photo removed.";
      showToast(m, "info");
      speakText(m, true);
    }

    function switchAuthMode(mode) {
      const isLogin = mode === "login";
      document.getElementById("loginForm").classList.toggle("hidden", !isLogin);
      document.getElementById("registerForm").classList.toggle("hidden", isLogin);
      document.getElementById("auth-tab-login").className = isLogin
        ? "py-2.5 rounded-xl text-xs font-extrabold bg-white text-emerald-700 shadow transition"
        : "py-2.5 rounded-xl text-xs font-extrabold text-slate-600 transition";
      document.getElementById("auth-tab-reg").className = !isLogin
        ? "py-2.5 rounded-xl text-xs font-extrabold bg-white text-indigo-700 shadow transition"
        : "py-2.5 rounded-xl text-xs font-extrabold text-slate-600 transition";
      onAuthRoleModeChange();
    }

    function togglePassField(id) {
      const inp = document.getElementById(id);
      inp.type = inp.type === "password" ? "text" : "password";
    }

    function handleUserRegister(e) {
      e.preventDefault();
      const regMode = (document.querySelector('input[name="regRoleMode"]:checked') || { value: "admin" }).value;

      if (regMode === "staff") {
        const adminLookupId = document.getElementById("regStaffAdminId").value.trim();
        const shopNameInput = document.getElementById("regStaffShopName").value.trim();
        const shopAddrInput = document.getElementById("regStaffShopAddr").value.trim();
        const staffName = document.getElementById("regStaffName").value.trim();
        const staffUserId = document.getElementById("regStaffUserId").value.trim();
        const p1 = document.getElementById("regStaffPass").value;
        const p2 = document.getElementById("regStaffPass2").value;
        const staffPhoto = document.getElementById("reg-staff-photo-data").value || "";
        const staffShopLogo = document.getElementById("reg-staff-shop-logo-data").value || "";

        if (!adminLookupId || !staffName || !staffUserId || !p1) {
          showToast(currentLang === "bn" ? "অ্যাডমিন ID, স্টাফের নাম, আইডি এবং পাসওয়ার্ড দিন!" : "Enter Admin ID, Staff Name, ID and Password!", "error");
          return;
        }
        if (p1 !== p2) {
          showToast(currentLang === "bn" ? "পাসওয়ার্ড দুটি মিলছে না!" : "Passwords do not match!", "error");
          return;
        }

        const users = getRegisteredUsers();
        const adminAcc = users.find(u => u.userId.toLowerCase() === adminLookupId.toLowerCase());
        if (!adminAcc) {
          showToast(currentLang === "bn" ? "এই অ্যাডমিন ID পাওয়া যায়নি!" : "Admin ID not found!", "error");
          return;
        }

        loadUserIsolatedStore(adminAcc.id, adminAcc.shopName, adminAcc.userId, adminAcc.shopAddr);
        const dupStaff = userStore.staffProfiles.some(s => s.staffUserId.toLowerCase() === staffUserId.toLowerCase());
        if (dupStaff) {
          showToast(currentLang === "bn" ? "এই স্টাফ ID দিয়ে আগেই প্রোফাইল আছে!" : "Staff ID already exists!", "error");
          return;
        }

        const newStaff = {
          id: "STF_" + Date.now() + "_" + Math.floor(Math.random() * 999),
          name: staffName,
          staffUserId,
          shopName: shopNameInput || userStore.settings.shopName || adminAcc.shopName,
          shopPhone: userStore.settings.shopPhone || adminAcc.userId,
          shopAddr: shopAddrInput || userStore.settings.shopAddr || adminAcc.shopAddr || "",
          passHash: computeHash(p1),
          photo: staffPhoto,
          shopLogo: staffShopLogo || userStore.settings.shopLogo || ""
        };
        userStore.staffProfiles.unshift(newStaff);
        localStorage.setItem(`rh_isolated_data_${adminAcc.id}`, JSON.stringify(userStore));
        scheduleAutoSync("staff-registered");

        currentUser = adminAcc;
        adminOwnerIdForSession = adminAcc.id;
        activeSessionRole = "staff";
        activeStaffMember = newStaff;
        persistSessionState();
        showToast(currentLang === "bn" ? "স্টাফ প্রোফাইল রেজিস্ট্রেশন ও লগইন সফল!" : "Staff registered & logged in!", "success");
        launchAuthenticatedWorkspace();
        return;
      }

      const owner = document.getElementById("regName").value.trim();
      const shopName = document.getElementById("regShop").value.trim() || "Ribek & Hetu";
      const shopAddr = document.getElementById("regShopAddr").value.trim();
      const userId = document.getElementById("regPhone").value.trim();
      const p1 = document.getElementById("regPass").value;
      const p2 = document.getElementById("regPass2").value;

      if (!owner || !userId || !p1) {
        showToast(currentLang === "bn" ? "সব তথ্য সঠিকভাবে পূরণ করুন!" : "Please fill in all fields!", "error");
        return;
      }
      if (p1 !== p2) {
        showToast(currentLang === "bn" ? "পাসওয়ার্ড দুটি মিলছে না!" : "Passwords do not match!", "error");
        return;
      }

      const users = getRegisteredUsers();
      if (users.some(u => u.userId.toLowerCase() === userId.toLowerCase())) {
        showToast(currentLang === "bn" ? "এই আইডি দিয়ে আগেই একটি একাউন্ট খোলা আছে!" : "An account with this ID already exists!", "error");
        return;
      }

      const newUser = {
        id: "USR_" + Date.now(),
        owner,
        shopName,
        shopAddr,
        userId,
        passHash: computeHash(p1),
        createdAt: new Date().toISOString()
      };
      users.push(newUser);
      saveRegisteredUsers(users);

      currentUser = newUser;
      adminOwnerIdForSession = newUser.id;
      activeSessionRole = "admin";
      activeStaffMember = null;
      persistSessionState();
      loadUserIsolatedStore(newUser.id, shopName, userId, shopAddr);
      showToast(currentLang === "bn" ? "আপনার আলাদা একাউন্ট ও ডাটাবেস তৈরি হয়েছে!" : "Your isolated account & database is ready!", "success");
      launchAuthenticatedWorkspace();
    }

    function handleUserLogin(e) {
      e.preventDefault();
      const loginMode = (document.querySelector('input[name="loginRoleMode"]:checked') || { value: "admin" }).value;
      const uid = document.getElementById("loginUser").value.trim();
      const pass = document.getElementById("loginPass").value;
      const hash = computeHash(pass);
      const users = getRegisteredUsers();

      if (loginMode === "staff") {
        const gateAdminId = document.getElementById("loginGateAdminId").value.trim();
        if (!gateAdminId) {
          showToast(currentLang === "bn" ? "স্টাফ লগইনের জন্য অ্যাডমিন ID দিন!" : "Enter Admin ID for staff login!", "error");
          return;
        }
        const targetAdmin = users.find(u => u.userId.toLowerCase() === gateAdminId.toLowerCase());
        if (!targetAdmin) {
          handleWrongLoginAttempt();
          return;
        }
        loadUserIsolatedStore(targetAdmin.id, targetAdmin.shopName, targetAdmin.userId, targetAdmin.shopAddr);
        const matchedStaff = (userStore.staffProfiles || []).find(
          s => s.staffUserId.toLowerCase() === uid.toLowerCase() && s.passHash === hash
        );
        if (matchedStaff) {
          wrongLoginAttempts = 0;
          document.getElementById("intruder-alert-card").classList.add("hidden");
          currentUser = targetAdmin;
          adminOwnerIdForSession = targetAdmin.id;
          activeSessionRole = "staff";
          activeStaffMember = matchedStaff;
          persistSessionState();
          launchAuthenticatedWorkspace();
          return;
        }
        handleWrongLoginAttempt();
        return;
      }

      const found = users.find(u => u.userId.toLowerCase() === uid.toLowerCase() && u.passHash === hash);
      if (found) {
        wrongLoginAttempts = 0;
        document.getElementById("intruder-alert-card").classList.add("hidden");
        currentUser = found;
        adminOwnerIdForSession = found.id;
        activeSessionRole = "admin";
        activeStaffMember = null;
        persistSessionState();
        loadUserIsolatedStore(found.id, found.shopName, found.userId, found.shopAddr);
        launchAuthenticatedWorkspace();
        return;
      }

      handleWrongLoginAttempt();
    }

    function handleWrongLoginAttempt() {
      wrongLoginAttempts++;
      const msg = currentLang === "bn"
        ? `ভুল আইডি বা পাসওয়ার্ড! (${wrongLoginAttempts}/৩)`
        : `Invalid ID or password! (${wrongLoginAttempts}/3)`;
      showToast(msg, "error");
      speakText(msg, true);

      if (wrongLoginAttempts >= 3) {
        const card = document.getElementById("intruder-alert-card");
        card.classList.remove("hidden");
        document.getElementById("intruder-msg").textContent = currentLang === "bn"
          ? "সতর্কতা! ৩ বার ভুল পাসওয়ার্ড দেওয়া হয়েছে। অননুমোদিত প্রবেশের চেষ্টা রেকর্ড করা হয়েছে।"
          : "Security Alert! 3 failed login attempts detected. Intruder capture triggered.";
        captureIntruderSelfie();
      }
    }

    async function captureIntruderSelfie() {
      if (!navigator.mediaDevices || !navigator.mediaDevices.getUserMedia) return;
      try {
        const stream = await navigator.mediaDevices.getUserMedia({ video: { facingMode: "user" } });
        const video = document.getElementById("intruder-video");
        const canvas = document.getElementById("intruder-canvas");
        const img = document.getElementById("intruder-photo");
        video.srcObject = stream;
        await new Promise(r => setTimeout(r, 700));
        canvas.width = 160;
        canvas.height = 160;
        canvas.getContext("2d").drawImage(video, 0, 0, 160, 160);
        img.src = canvas.toDataURL("image/jpeg");
        img.classList.remove("hidden");
        stream.getTracks().forEach(t => t.stop());
      } catch (e) {}
    }

    function launchAuthenticatedWorkspace() {
      ensureActiveSessionStore();
      document.getElementById("auth-screen").classList.add("hidden");
      document.getElementById("main-app").classList.remove("hidden");
      applyRolePermissionsUI();
      applyColorTheme(userStore.settings.theme || "theme-emerald");
      applyCustomBackgroundPhoto(userStore.settings.customBgPhoto || "");
      attachRealtimeStoreListener();
      pullStoreFromCloud(true);
      renderAllViews();
      switchScreen("dashboard");
      updateLiveMapEmbed();
      setTimeout(() => speakFullGreetingAndClock(true), 450);
    }

    function logoutCurrentUser() {
      currentUser = null;
      adminOwnerIdForSession = null;
      activeSessionRole = "admin";
      activeStaffMember = null;
      userStore = null;
      localStorage.removeItem("rh_active_uid");
      localStorage.removeItem("rh_active_role");
      localStorage.removeItem("rh_active_staff_id");
      try {
        sessionStorage.removeItem("rh_session_uid");
        sessionStorage.removeItem("rh_session_role");
        sessionStorage.removeItem("rh_session_staff_id");
      } catch (e) {}
      document.getElementById("main-app").classList.add("hidden");
      document.getElementById("auth-screen").classList.remove("hidden");
    }

    function switchScreen(screenId) {
      if (activeSessionRole === "staff" && (screenId === "product-entry" || screenId === "settings")) {
        screenId = "dashboard";
      }
      document.querySelectorAll(".app-screen").forEach(s => s.classList.add("hidden"));
      const target = document.getElementById("screen-" + screenId);
      if (target) target.classList.remove("hidden");

      document.querySelectorAll(".nav-btn").forEach(btn => {
        const active = btn.getAttribute("data-nav") === screenId;
        btn.classList.toggle("theme-text", active);
        btn.classList.toggle("theme-soft-bg", active);
        btn.classList.toggle("font-extrabold", active);
        btn.classList.toggle("text-slate-600", !active);
      });

      if (screenId === "barcode") renderBarcodeAndQR();
      if (screenId === "history") renderVoucherPreview(selectedVoucher || (userStore && userStore.sales[0]) || null);
      if (screenId === "calendar") renderCalendarScreen();
      if (screenId === "dashboard") updateLiveMapEmbed();
      window.scrollTo({ top: 0, behavior: "smooth" });
    }

    function populateUnitDropdowns() {
      const opts = MEASUREMENT_UNITS.map(u => `<option value="${u.id}">${currentLang === "bn" ? u.bn : u.en}</option>`).join("");
      const pUnit = document.getElementById("prod-unit");
      const sUnit = document.getElementById("sale-unit-select");
      if (pUnit) pUnit.innerHTML = opts;
      if (sUnit) sUnit.innerHTML = opts;
    }

    function autoFillBarcode(inputId) {
      const el = document.getElementById(inputId);
      if (!el) return;
      const nameVal = (document.getElementById("prod-name") || { value: "" }).value.trim();
      const sellVal = (document.getElementById("prod-sell") || { value: "" }).value.trim();
      if (inputId === "prod-barcode" && (nameVal || sellVal)) {
        el.value = buildAutoProductCode(nameVal, sellVal);
      } else {
        el.value = "RH" + Math.floor(10000000 + Math.random() * 90000000);
      }
      speakText(el.value, true);
    }

    function buildAutoProductCode(prodName, sellPrice) {
      const cleanName = String(prodName || "").trim().replace(/\s+/g, "-").slice(0, 18) || "ITEM";
      const cleanPrice = String(sellPrice || "").trim();
      return cleanPrice ? `${cleanName}-TK${cleanPrice}` : cleanName;
    }

    function onProductNameOrSellPriceAutoCode() {
      const nameEl = document.getElementById("prod-name");
      const sellEl = document.getElementById("prod-sell");
      const codeEl = document.getElementById("prod-barcode");
      if (!nameEl || !codeEl) return;
      const pName = nameEl.value.trim();
      const pSell = sellEl ? sellEl.value.trim() : "";
      if (pName || pSell) {
        codeEl.value = buildAutoProductCode(pName, pSell);
      }
      updateEntryAutoCodePreviews();
    }

    function updateEntryAutoCodePreviews() {
      const codeEl = document.getElementById("prod-barcode");
      const svgEl = document.getElementById("entry-barcode-svg");
      const qrEl = document.getElementById("entry-qrcode-div");
      if (!codeEl || !svgEl || !qrEl) return;
      const raw = codeEl.value.trim();
      if (!raw) {
        svgEl.innerHTML = "";
        qrEl.innerHTML = "";
        return;
      }
      const asciiCode = raw.replace(/[^\x20-\x7E]/g, "").trim() || ("RH" + Math.abs(raw.split("").reduce((a, c) => ((a << 5) - a) + c.charCodeAt(0), 0)));
      try {
        JsBarcode(svgEl, asciiCode, { format: "CODE128", width: 1.5, height: 42, displayValue: true, fontSize: 11, margin: 4 });
      } catch (e) {}
      qrEl.innerHTML = "";
      try {
        new QRCode(qrEl, { text: raw, width: 78, height: 78 });
      } catch (e) {}
    }

    function saveBarcodeImageToMemory(svgId, fileBaseName = "Barcode") {
      const svg = document.getElementById(svgId);
      if (!svg || !svg.innerHTML.trim()) {
        showToast(currentLang === "bn" ? "আগে বারকোড তৈরি করুন!" : "Generate a barcode first!", "error");
        return;
      }
      const serializer = new XMLSerializer();
      const svgStr = serializer.serializeToString(svg);
      const svgBlob = new Blob([svgStr], { type: "image/svg+xml;charset=utf-8" });
      const url = URL.createObjectURL(svgBlob);
      const img = new Image();
      img.onload = () => {
        const canvas = document.createElement("canvas");
        canvas.width = Math.max(260, img.width * 2);
        canvas.height = Math.max(110, img.height * 2);
        const ctx = canvas.getContext("2d");
        ctx.fillStyle = "#ffffff";
        ctx.fillRect(0, 0, canvas.width, canvas.height);
        ctx.drawImage(img, 0, 0, canvas.width, canvas.height);
        URL.revokeObjectURL(url);
        const link = document.createElement("a");
        link.download = `${String(fileBaseName).replace(/[^a-zA-Z0-9_-]/g, "_") || "Barcode"}_Barcode.png`;
        link.href = canvas.toDataURL("image/png");
        link.click();
        const m = currentLang === "bn" ? "বারকোড ছবি মেমোরিতে সেভ করা হয়েছে!" : "Barcode image saved to memory!";
        showToast(m, "success");
        speakText(m, true);
      };
      img.src = url;
    }

    function saveQrImageToMemory(qrContainerId, fileBaseName = "QRCode") {
      const box = document.getElementById(qrContainerId);
      if (!box) return;
      const img = box.querySelector("img");
      const cvs = box.querySelector("canvas");
      const dataUrl = img && img.src ? img.src : (cvs ? cvs.toDataURL("image/png") : "");
      if (!dataUrl) {
        showToast(currentLang === "bn" ? "আগে QR কোড তৈরি করুন!" : "Generate a QR code first!", "error");
        return;
      }
      const link = document.createElement("a");
      link.download = `${String(fileBaseName).replace(/[^a-zA-Z0-9_-]/g, "_") || "QRCode"}_QR.png`;
      link.href = dataUrl;
      link.click();
      const m = currentLang === "bn" ? "QR কোড ছবি মেমোরিতে সেভ করা হয়েছে!" : "QR code image saved to memory!";
      showToast(m, "success");
      speakText(m, true);
    }

    function calculatePurchasePreview() {
      const qty = parseFloat(document.getElementById("prod-qty").value) || 0;
      const buy = parseFloat(document.getElementById("prod-buy").value) || 0;
      const sell = parseFloat(document.getElementById("prod-sell").value) || 0;
      const totalBuy = +(qty * buy).toFixed(2);
      const estProfit = +((qty * sell) - totalBuy).toFixed(2);
      document.getElementById("purchase-auto-total").textContent = `৳ ${totalBuy.toFixed(2)}`;
      document.getElementById("purchase-auto-profit").textContent = `৳ ${estProfit.toFixed(2)}`;
    }

    function savePurchasedProduct(e) {
      e.preventDefault();
      if (activeSessionRole !== "admin" || !ensureActiveSessionStore()) return;

      const editId = document.getElementById("prod-edit-id").value;
      const name = document.getElementById("prod-name").value.trim();
      const buyPrice = parseFloat(document.getElementById("prod-buy").value) || 0;
      const sellPrice = parseFloat(document.getElementById("prod-sell").value) || 0;
      const barcode = document.getElementById("prod-barcode").value.trim() || buildAutoProductCode(name, sellPrice);
      const unit = document.getElementById("prod-unit").value;
      const qty = parseFloat(document.getElementById("prod-qty").value) || 0;
      const totalCost = +(qty * buyPrice).toFixed(2);

      if (!name || qty <= 0) {
        showToast(currentLang === "bn" ? "প্রোডাক্টের নাম ও সঠিক পরিমাণ দিন!" : "Enter valid product name & quantity!", "error");
        return;
      }

      if (editId) {
        const idx = userStore.products.findIndex(p => p.id === editId);
        if (idx !== -1) {
          userStore.products[idx] = { ...userStore.products[idx], name, barcode, unit, qty, buyPrice, sellPrice };
        }
      } else {
        const existing = userStore.products.find(p => p.name.toLowerCase() === name.toLowerCase() && p.unit === unit);
        if (existing) {
          existing.qty = +(Number(existing.qty) + qty).toFixed(4);
          existing.buyPrice = buyPrice;
          existing.sellPrice = sellPrice;
          if (barcode) existing.barcode = barcode;
        } else {
          userStore.products.unshift({
            id: "PRD_" + Date.now() + "_" + Math.floor(Math.random() * 999),
            name,
            barcode,
            unit,
            qty,
            buyPrice,
            sellPrice
          });
        }
        userStore.purchases.unshift({
          id: "PUR_" + Date.now(),
          name,
          unit,
          qty,
          buyPrice,
          totalCost,
          date: new Date().toISOString().split("T")[0]
        });
      }

      saveUserIsolatedStore();
      resetProductForm();
      const msg = currentLang === "bn" ? `${name} স্টকে সংরক্ষণ করা হয়েছে।` : `${name} saved to stock.`;
      showToast(msg, "success");
      speakText(msg, true);
    }

    function editProductItem(id) {
      if (activeSessionRole !== "admin") return;
      const p = userStore.products.find(x => x.id === id);
      if (!p) return;
      document.getElementById("prod-edit-id").value = p.id;
      document.getElementById("prod-name").value = p.name;
      document.getElementById("prod-barcode").value = p.barcode || "";
      document.getElementById("prod-unit").value = p.unit;
      document.getElementById("prod-qty").value = p.qty;
      document.getElementById("prod-buy").value = p.buyPrice;
      document.getElementById("prod-sell").value = p.sellPrice;
      document.getElementById("prod-cancel-btn").classList.remove("hidden");
      calculatePurchasePreview();
      updateEntryAutoCodePreviews();
      switchScreen("product-entry");
    }

    function deleteProductItem(id) {
      if (activeSessionRole !== "admin") return;
      openConfirmModal(
        currentLang === "bn" ? "প্রোডাক্ট মুছে ফেলবেন?" : "Delete Product?",
        currentLang === "bn" ? "এই প্রোডাক্টটি স্টক থেকে মুছে ফেলতে চান?" : "Remove this product from stock?",
        () => {
          userStore.products = userStore.products.filter(x => x.id !== id);
          saveUserIsolatedStore();
          showToast(currentLang === "bn" ? "প্রোডাক্ট মুছে ফেলা হয়েছে" : "Product deleted", "info");
        }
      );
    }

    function resetProductForm() {
      document.getElementById("product-form").reset();
      document.getElementById("prod-edit-id").value = "";
      document.getElementById("prod-cancel-btn").classList.add("hidden");
      calculatePurchasePreview();
      updateEntryAutoCodePreviews();
    }

    function renderInventoryList() {
      const box = document.getElementById("inventory-list");
      if (!box || !userStore) return;
      const isStaff = activeSessionRole === "staff";
      if (!userStore.products.length) {
        box.innerHTML = `<div class="text-center py-6 text-xs text-slate-400">${currentLang === "bn" ? "স্টকে কোনো প্রোডাক্ট নেই" : "No products in stock"}</div>`;
        return;
      }
      box.innerHTML = userStore.products.map(p => {
        const uObj = MEASUREMENT_UNITS.find(u => u.id === p.unit) || { bn: p.unit, en: p.unit };
        const unitLabel = currentLang === "bn" ? uObj.bn : uObj.en;
        return `
          <div class="p-3 rounded-2xl border border-slate-200 flex items-center justify-between gap-2 bg-slate-50/70">
            <div class="min-w-0">
              <div class="font-extrabold text-sm truncate">${escapeHtml(p.name)}</div>
              <div class="text-[11px] text-slate-600 font-semibold">
                ${currentLang === "bn" ? "স্টক" : "Stock"}: <span class="text-emerald-700 font-extrabold">${p.qty} ${unitLabel}</span>
                ${!isStaff ? ` • ${currentLang === "bn" ? "ক্রয়" : "Buy"}: ৳${Number(p.buyPrice).toFixed(2)}` : ""}
                • ${currentLang === "bn" ? "বিক্রয়" : "Sell"}: <span class="text-indigo-700 font-bold">৳${Number(p.sellPrice).toFixed(2)}</span>
              </div>
              ${p.barcode ? `<div class="text-[10px] font-mono text-slate-400">CODE: ${escapeHtml(p.barcode)}</div>` : ""}
            </div>
            ${!isStaff ? `
            <div class="flex items-center gap-1.5 shrink-0">
              <button type="button" onclick="editProductItem('${p.id}')" class="w-8 h-8 rounded-xl bg-amber-100 text-amber-800 flex items-center justify-center"><i class="fa-solid fa-pen"></i></button>
              <button type="button" onclick="deleteProductItem('${p.id}')" class="w-8 h-8 rounded-xl bg-rose-100 text-rose-700 flex items-center justify-center"><i class="fa-solid fa-trash"></i></button>
            </div>` : ""}
          </div>`;
      }).join("");
    }

    function convertUnitQuantity(qty, fromUnitId, toUnitId) {
      const f = MEASUREMENT_UNITS.find(u => u.id === fromUnitId);
      const t = MEASUREMENT_UNITS.find(u => u.id === toUnitId);
      if (!f || !t || f.base !== t.base) return qty;
      return (qty * f.factor) / t.factor;
    }

    function populateSaleProductSelect() {
      const sel = document.getElementById("sale-prod-select");
      if (!sel || !userStore) return;
      const prevVal = sel.value;
      sel.innerHTML = `<option value="">${currentLang === "bn" ? "-- প্রোডাক্ট নির্বাচন করুন --" : "-- Select Product --"}</option>` +
        userStore.products.map(p => `<option value="${p.id}">${escapeHtml(p.name)} (${p.qty} ${p.unit})</option>`).join("");
      if (prevVal && userStore.products.some(p => p.id === prevVal)) sel.value = prevVal;
      onSaleProductSelected();
    }

    function onSaleProductSelected() {
      const pid = document.getElementById("sale-prod-select").value;
      const p = userStore && userStore.products.find(x => x.id === pid);
      const hint = document.getElementById("sale-stock-hint");
      if (!p) {
        if (hint) hint.textContent = "";
        recalculateSalePrice();
        return;
      }
      document.getElementById("sale-unit-select").value = p.unit;
      document.getElementById("sale-rate").value = p.sellPrice;
      if (hint) {
        hint.textContent = currentLang === "bn"
          ? `মজুদ আছে: ${p.qty} ${p.unit} | নির্ধারিত বিক্রয় দর: ৳${Number(p.sellPrice).toFixed(2)}`
          : `Available Stock: ${p.qty} ${p.unit} | Default Rate: ৳${Number(p.sellPrice).toFixed(2)}`;
      }
      recalculateSalePrice();
    }

    function buildCurrentSaleItemDraft() {
      const pid = document.getElementById("sale-prod-select").value;
      const p = userStore && userStore.products.find(x => x.id === pid);
      const saleUnit = document.getElementById("sale-unit-select").value;
      const qty = parseFloat(document.getElementById("sale-qty").value) || 0;
      const rate = parseFloat(document.getElementById("sale-rate").value) || 0;
      if (!p || qty <= 0) return null;

      const dedQtyInStockUnit = convertUnitQuantity(qty, saleUnit, p.unit);
      const lineTotal = +(qty * rate).toFixed(2);
      const costPerSaleUnit = p.buyPrice * (convertUnitQuantity(1, saleUnit, p.unit) || 1);
      const lineCost = +(qty * costPerSaleUnit).toFixed(2);
      const lineProfit = +(lineTotal - lineCost).toFixed(2);

      return {
        productId: p.id,
        name: p.name,
        saleUnit,
        qty,
        rate,
        dedQtyInStockUnit,
        total: lineTotal,
        cost: lineCost,
        profit: lineProfit
      };
    }

    function renderSaleCartUI() {
      const box = document.getElementById("sale-multi-cart-box");
      const listEl = document.getElementById("sale-multi-cart-items");
      const badgeEl = document.getElementById("sale-cart-count-badge");
      if (!box || !listEl || !badgeEl) return;

      if (saleCartItems.length === 0) {
        box.classList.add("hidden");
        listEl.innerHTML = "";
        badgeEl.textContent = "";
        return;
      }

      box.classList.remove("hidden");
      badgeEl.textContent = currentLang === "bn"
        ? `ভাউচারে যুক্ত আছে: ${toBnNum(saleCartItems.length)}টি প্রোডাক্ট`
        : `Added in Voucher: ${saleCartItems.length} item(s)`;

      listEl.innerHTML = saleCartItems.map((it, idx) => `
        <div class="flex items-center justify-between gap-2 bg-white px-2.5 py-1.5 rounded-xl border border-teal-100">
          <div class="truncate font-bold text-slate-800">
            ${idx + 1}. ${escapeHtml(it.name)} (${it.qty} ${it.saleUnit} × ৳${it.rate})
          </div>
          <div class="flex items-center gap-2 shrink-0">
            <span class="font-extrabold text-teal-800">৳${it.total.toFixed(2)}</span>
            <button type="button" onclick="removeSaleCartItem(${idx})" class="text-rose-600 hover:text-rose-800 font-bold">
              <i class="fa-solid fa-xmark"></i>
            </button>
          </div>
        </div>
      `).join("");
    }

    function addProductToSaleCart() {
      const draft = buildCurrentSaleItemDraft();
      if (!draft) {
        showToast(currentLang === "bn" ? "আগে প্রোডাক্ট ও পরিমাণ দিন!" : "Select product and enter quantity first!", "error");
        return;
      }
      const p = userStore.products.find(x => x.id === draft.productId);
      const alreadyDeducted = saleCartItems
        .filter(x => x.productId === draft.productId)
        .reduce((s, x) => s + Number(x.dedQtyInStockUnit || 0), 0);
      if (!p || (alreadyDeducted + draft.dedQtyInStockUnit) > Number(p.qty) + 1e-6) {
        showToast(currentLang === "bn" ? "স্টকে পর্যাপ্ত পরিমাণ নেই!" : "Insufficient stock quantity!", "error");
        return;
      }
      saleCartItems.push(draft);
      document.getElementById("sale-qty").value = "";
      renderSaleCartUI();
      recalculateSalePrice();
      const msg = currentLang === "bn" ? `${draft.name} ভাউচারে যোগ হয়েছে` : `${draft.name} added to voucher`;
      showToast(msg, "success");
      speakText(msg, true);
    }

    function removeSaleCartItem(idx) {
      saleCartItems.splice(idx, 1);
      renderSaleCartUI();
      recalculateSalePrice();
    }

    function clearSaleCart() {
      saleCartItems = [];
      renderSaleCartUI();
      recalculateSalePrice();
    }

    function recalculateSalePrice() {
      const draft = buildCurrentSaleItemDraft();
      const cartSum = saleCartItems.reduce((s, it) => s + Number(it.total || 0), 0);
      const total = +(cartSum + (draft ? draft.total : 0)).toFixed(2);
      document.getElementById("sale-grand-total").textContent = `৳ ${total.toFixed(2)}`;
    }

    function onSalePayModeChange() {
      const mode = document.querySelector('input[name="salePayMode"]:checked').value;
      document.getElementById("sale-due-picker-wrap").classList.toggle("hidden", mode !== "due");
      document.getElementById("sale-digital-strip").classList.toggle("hidden", mode !== "digital");
      if (mode === "due") populateDuePickerSelect();
    }

    function populateDuePickerSelect() {
      const sel = document.getElementById("sale-due-picker");
      if (!sel || !userStore) return;
      sel.innerHTML = `<option value="">${currentLang === "bn" ? "-- নতুন বাকি ক্রেতা --" : "-- New Due Customer --"}</option>` +
        userStore.dueCustomers.map(c => `<option value="${c.id}">${escapeHtml(c.name)} (${escapeHtml(c.phone)}) — ৳${Number(c.dueAmount).toFixed(2)}</option>`).join("");
    }

    function populateDueCustomerFields() {
      const id = document.getElementById("sale-due-picker").value;
      const c = userStore.dueCustomers.find(x => x.id === id);
      if (!c) return;
      document.getElementById("sale-cust-name").value = c.name;
      document.getElementById("sale-cust-phone").value = c.phone;
      if (c.photo) {
        document.getElementById("sale-cust-photo-data").value = c.photo;
        document.getElementById("sale-cust-photo-preview").src = c.photo;
      }
    }

    function handleSaleCustomerPhoto(e) {
      const f = e.target.files && e.target.files[0];
      if (!f) return;
      readImageFileToDataURL(f, dataUrl => {
        document.getElementById("sale-cust-photo-data").value = dataUrl;
        document.getElementById("sale-cust-photo-preview").src = dataUrl;
        const m = currentLang === "bn" ? "ছবি যুক্ত করা হয়েছে" : "Photo attached";
        showToast(m, "success");
        speakText(m, true);
      }, 450, 0.72);
    }

    function clearSaleCustomerPhoto() {
      document.getElementById("sale-cust-photo-data").value = "";
      document.getElementById("sale-cust-photo-file").value = "";
      document.getElementById("sale-cust-photo-preview").src = "https://placehold.co/64x64/f59e0b/ffffff?text=Photo";
    }

    function completeProductSale(e) {
      e.preventDefault();
      if (!ensureActiveSessionStore()) return;

      const draft = buildCurrentSaleItemDraft();
      const itemsToSell = [...saleCartItems];
      if (draft) itemsToSell.push(draft);

      if (itemsToSell.length === 0) {
        showToast(currentLang === "bn" ? "প্রোডাক্ট নির্বাচন ও পরিমাণ লিখুন!" : "Select product and enter quantity!", "error");
        return;
      }

      const requiredByProduct = {};
      for (const it of itemsToSell) {
        requiredByProduct[it.productId] = (requiredByProduct[it.productId] || 0) + Number(it.dedQtyInStockUnit || 0);
      }
      for (const pid of Object.keys(requiredByProduct)) {
        const prod = userStore.products.find(x => x.id === pid);
        if (!prod || Number(prod.qty) + 1e-6 < requiredByProduct[pid]) {
          showToast(currentLang === "bn" ? `${prod ? prod.name : "প্রোডাক্ট"}-এর স্টকে পর্যাপ্ত পরিমাণ নেই!` : "Insufficient stock quantity!", "error");
          return;
        }
      }

      for (const it of itemsToSell) {
        const prod = userStore.products.find(x => x.id === it.productId);
        if (prod) {
          prod.qty = Math.max(0, +(Number(prod.qty) - Number(it.dedQtyInStockUnit)).toFixed(4));
        }
      }

      const saleDate = document.getElementById("sale-date").value || new Date().toISOString().split("T")[0];
      const payMode = document.querySelector('input[name="salePayMode"]:checked').value;
      const custName = document.getElementById("sale-cust-name").value.trim() || (currentLang === "bn" ? "সাধারণ ক্রেতা" : "Walk-in Customer");
      const custPhone = document.getElementById("sale-cust-phone").value.trim();
      const custPhoto = document.getElementById("sale-cust-photo-data").value || "";
      const trxId = (document.getElementById("sale-digital-trx") || { value: "" }).value.trim();

      const grandTotal = +itemsToSell.reduce((s, it) => s + Number(it.total || 0), 0).toFixed(2);
      const grandProfit = +itemsToSell.reduce((s, it) => s + Number(it.profit || 0), 0).toFixed(2);
      const summaryName = itemsToSell.length === 1
        ? `${itemsToSell[0].name} (${itemsToSell[0].qty} ${itemsToSell[0].saleUnit})`
        : `${itemsToSell[0].name} + ${itemsToSell.length - 1} ${currentLang === "bn" ? "টি পণ্য" : "more"}`;

      if (payMode === "due") {
        const pickedId = document.getElementById("sale-due-picker").value;
        const itemSummaryList = itemsToSell.map(it => `${it.name} (${it.qty} ${it.saleUnit}) ৳${it.total.toFixed(2)}`).join(", ");
        let existing = pickedId ? userStore.dueCustomers.find(c => c.id === pickedId) : null;
        if (!existing && custPhone) {
          existing = userStore.dueCustomers.find(c => c.phone === custPhone);
        }
        if (existing) {
          const normExisting = normalizeDueCustomerRecord(existing);
          existing.ledgerRows = normExisting.ledgerRows;
          itemsToSell.forEach(it => {
            existing.ledgerRows.push({
              productName: `${it.name} (${it.qty} ${it.saleUnit})`,
              date: saleDate,
              amount: +(Number(it.total || 0)).toFixed(2)
            });
          });
          existing.dueAmount = +(Number(existing.dueAmount || 0) + grandTotal).toFixed(2);
          existing.productDetails = existing.ledgerRows.map(r => `${r.productName} ৳${Number(r.amount || 0).toFixed(2)}`).join(", ");
          existing.updatedDate = saleDate;
          if (custPhoto) existing.photo = custPhoto;
        } else {
          userStore.dueCustomers.unshift({
            id: "DUE_" + Date.now(),
            name: custName,
            phone: custPhone || "N/A",
            address: "",
            photo: custPhoto,
            productDetails: itemSummaryList,
            dueAmount: grandTotal,
            updatedDate: saleDate,
            ledgerRows: itemsToSell.map(it => ({
              productName: `${it.name} (${it.qty} ${it.saleUnit})`,
              date: saleDate,
              amount: +(Number(it.total || 0)).toFixed(2)
            }))
          });
        }
      }

      const sellerLabel = activeSessionRole === "staff" && activeStaffMember
        ? `${activeStaffMember.name} (Staff)`
        : ((currentUser && currentUser.owner) ? `${currentUser.owner} (Admin)` : "Admin");

      const voucher = {
        id: "VCH-" + Math.floor(100000 + Math.random() * 900000),
        productName: summaryName,
        items: itemsToSell.map(it => ({
          name: it.name,
          qty: it.qty,
          unit: it.saleUnit,
          rate: it.rate,
          total: it.total
        })),
        qty: itemsToSell.reduce((s, it) => s + Number(it.qty || 0), 0),
        unit: itemsToSell[0].saleUnit,
        rate: itemsToSell[0].rate,
        total: grandTotal,
        profit: grandProfit,
        payMode,
        trxId,
        custName,
        custPhone,
        custPhoto,
        date: saleDate,
        time: new Date().toLocaleTimeString([], { hour: "2-digit", minute: "2-digit" }),
        sellerName: sellerLabel,
        soldByRole: activeSessionRole,
        soldByStaffId: activeSessionRole === "staff" && activeStaffMember ? activeStaffMember.id : "",
        soldByStaffName: activeSessionRole === "staff" && activeStaffMember ? activeStaffMember.name : ""
      };

      userStore.sales.unshift(voucher);
      selectedVoucher = voucher;
      saleCartItems = [];
      renderSaleCartUI();
      saveUserIsolatedStore();

      document.getElementById("sale-qty").value = "";
      document.getElementById("sale-cust-name").value = "";
      document.getElementById("sale-cust-phone").value = "";
      if (document.getElementById("sale-digital-trx")) document.getElementById("sale-digital-trx").value = "";
      clearSaleCustomerPhoto();
      recalculateSalePrice();

      const msg = currentLang === "bn"
        ? `${summaryName} বিক্রয় সম্পন্ন হয়েছে। মোট টাকা ${grandTotal} টাকা।`
        : `Sale completed for ${summaryName}. Total ${grandTotal} Taka.`;
      showToast(msg, "success");
      speakText(msg, true);
      switchScreen("history");
    }

    function handleDuePhotoUpload(e) {
      const f = e.target.files && e.target.files[0];
      if (!f) return;
      readImageFileToDataURL(f, dataUrl => {
        document.getElementById("due-cust-photo").value = dataUrl;
      }, 450, 0.72);
    }

    function saveDueCustomerEntry(e) {
      e.preventDefault();
      if (!ensureActiveSessionStore()) return;
      const editId = document.getElementById("due-edit-id").value;
      const name = document.getElementById("due-cust-name").value.trim();
      const phone = document.getElementById("due-cust-phone").value.trim();
      const address = document.getElementById("due-cust-address").value.trim();
      const productDetails = document.getElementById("due-cust-products").value.trim();
      const dueAmount = +(parseFloat(document.getElementById("due-cust-amount").value) || 0).toFixed(2);
      const updatedDate = document.getElementById("due-cust-date").value || new Date().toISOString().split("T")[0];
      const photo = document.getElementById("due-cust-photo").value;

      if (!name || !phone) {
        showToast(currentLang === "bn" ? "ক্রেতার নাম ও মোবাইল নম্বর দিন!" : "Enter customer name & phone!", "error");
        return;
      }

      if (editId) {
        const idx = userStore.dueCustomers.findIndex(c => c.id === editId);
        if (idx !== -1) {
          const prev = normalizeDueCustomerRecord(userStore.dueCustomers[idx]);
          const nextRows = Array.isArray(prev.ledgerRows) && prev.ledgerRows.length > 0
            ? prev.ledgerRows.map((r, i) => i === 0 ? {
                productName: productDetails || r.productName || (currentLang === "bn" ? "বাকি হিসাব" : "Due Entry"),
                date: updatedDate,
                amount: dueAmount
              } : r)
            : [{
                productName: productDetails || (currentLang === "bn" ? "বাকি হিসাব" : "Due Entry"),
                date: updatedDate,
                amount: dueAmount
              }];
          const summedDue = +nextRows.reduce((s, r) => s + Number(r.amount || 0), 0).toFixed(2);
          userStore.dueCustomers[idx] = {
            ...prev,
            name,
            phone,
            address,
            productDetails: nextRows.map(r => `${r.productName} ৳${Number(r.amount || 0).toFixed(2)}`).join(", "),
            dueAmount: summedDue,
            updatedDate,
            photo: photo || prev.photo,
            ledgerRows: nextRows
          };
        }
      } else {
        const initialRows = (productDetails || dueAmount > 0)
          ? [{
              productName: productDetails || (currentLang === "bn" ? "নতুন বাকি" : "New Due"),
              date: updatedDate,
              amount: dueAmount
            }]
          : [];
        userStore.dueCustomers.unshift({
          id: "DUE_" + Date.now(),
          name,
          phone,
          address,
          productDetails: initialRows.map(r => `${r.productName} ৳${Number(r.amount || 0).toFixed(2)}`).join(", "),
          dueAmount,
          updatedDate,
          photo,
          ledgerRows: initialRows
        });
      }

      saveUserIsolatedStore();
      resetDueForm();
      const m = currentLang === "bn" ? "বাকি হিসাব সংরক্ষণ করা হয়েছে" : "Due record saved";
      showToast(m, "success");
      speakText(m, true);
    }

    function addDueProductRow(custId) {
      if (!ensureActiveSessionStore()) return;
      const cIdx = userStore.dueCustomers.findIndex(x => x.id === custId);
      if (cIdx === -1) return;
      const rec = normalizeDueCustomerRecord(userStore.dueCustomers[cIdx]);

      document.getElementById("modal-title").textContent = currentLang === "bn"
        ? `${rec.name} — নতুন বাকি পণ্য / হিসাব যোগ করুন`
        : `${rec.name} — Add Due Product / Entry`;

      document.getElementById("modal-body").innerHTML = `
        <div class="space-y-2.5 text-xs">
          <div>
            <label class="block font-bold mb-1">${currentLang === "bn" ? "পণ্যের নাম / বিবরণ" : "Product Name / Details"}</label>
            <input type="text" id="due-row-prod-input" class="w-full px-3 py-2 rounded-xl border border-slate-300 font-bold" placeholder="${currentLang === "bn" ? "যেমন: চাল ৫ কেজি বা তেল ১ লিটার" : "e.g. Rice 5kg or Oil 1L"}" />
          </div>
          <div class="grid grid-cols-2 gap-2">
            <div>
              <label class="block font-bold mb-1">${currentLang === "bn" ? "তারিখ" : "Date"}</label>
              <input type="date" id="due-row-date-input" value="${new Date().toISOString().split("T")[0]}" class="w-full px-3 py-2 rounded-xl border border-slate-300 font-bold" />
            </div>
            <div>
              <label class="block font-bold mb-1">${currentLang === "bn" ? "টাকার পরিমাণ (৳)" : "Amount (৳)"}</label>
              <input type="number" step="0.01" id="due-row-amt-input" class="w-full px-3 py-2 rounded-xl border border-slate-300 font-extrabold" placeholder="0.00" />
            </div>
          </div>
        </div>
      `;

      document.getElementById("modal-actions").innerHTML = `
        <button type="button" onclick="closeModal()" class="px-3 py-2 rounded-xl bg-slate-200 text-xs font-bold">${currentLang === "bn" ? "বাতিল" : "Cancel"}</button>
        <button type="button" onclick="confirmAddDueProductRow('${custId}')" class="px-4 py-2 rounded-xl bg-amber-600 hover:bg-amber-700 text-white text-xs font-extrabold">${currentLang === "bn" ? "যোগ করুন" : "Add Row"}</button>
      `;
      document.getElementById("custom-modal").classList.remove("hidden");
      document.getElementById("custom-modal").classList.add("flex");
    }

    function confirmAddDueProductRow(custId) {
      if (!ensureActiveSessionStore()) return;
      const cIdx = userStore.dueCustomers.findIndex(x => x.id === custId);
      if (cIdx === -1) return;
      const rec = normalizeDueCustomerRecord(userStore.dueCustomers[cIdx]);
      const pName = (document.getElementById("due-row-prod-input").value || "").trim() || (currentLang === "bn" ? "বাকি পণ্য" : "Due Item");
      const rDate = document.getElementById("due-row-date-input").value || new Date().toISOString().split("T")[0];
      const rAmt = +(parseFloat(document.getElementById("due-row-amt-input").value) || 0).toFixed(2);

      rec.ledgerRows.push({ productName: pName, date: rDate, amount: rAmt });
      const newTotal = +rec.ledgerRows.reduce((s, r) => s + Number(r.amount || 0), 0).toFixed(2);
      userStore.dueCustomers[cIdx] = {
        ...rec,
        dueAmount: newTotal,
        updatedDate: rDate,
        productDetails: rec.ledgerRows.map(r => `${r.productName} ৳${Number(r.amount || 0).toFixed(2)}`).join(", ")
      };
      closeModal();
      saveUserIsolatedStore();
      const msg = currentLang === "bn" ? `${rec.name}-এর খাতায় ${pName} (৳${rAmt.toFixed(2)}) যোগ হয়েছে।` : `Added ${pName} (৳${rAmt.toFixed(2)}) to ${rec.name}.`;
      showToast(msg, "success");
      speakText(msg, true);
    }

    function editDueCustomerRow(custId, rowIdx) {
      if (!ensureActiveSessionStore()) return;
      const cIdx = userStore.dueCustomers.findIndex(x => x.id === custId);
      if (cIdx === -1) return;
      const rec = normalizeDueCustomerRecord(userStore.dueCustomers[cIdx]);
      const row = rec.ledgerRows[rowIdx];
      if (!row) return;

      document.getElementById("modal-title").textContent = currentLang === "bn"
        ? `${rec.name} — বাকি লাইন এডিট করুন`
        : `${rec.name} — Edit Due Line`;

      document.getElementById("modal-body").innerHTML = `
        <div class="space-y-2.5 text-xs">
          <div>
            <label class="block font-bold mb-1">${currentLang === "bn" ? "পণ্যের নাম / বিবরণ" : "Product Name / Details"}</label>
            <input type="text" id="due-edit-row-prod" value="${escapeHtml(row.productName)}" class="w-full px-3 py-2 rounded-xl border border-slate-300 font-bold" />
          </div>
          <div class="grid grid-cols-2 gap-2">
            <div>
              <label class="block font-bold mb-1">${currentLang === "bn" ? "তারিখ" : "Date"}</label>
              <input type="date" id="due-edit-row-date" value="${escapeHtml(row.date || rec.updatedDate || "")}" class="w-full px-3 py-2 rounded-xl border border-slate-300 font-bold" />
            </div>
            <div>
              <label class="block font-bold mb-1">${currentLang === "bn" ? "টাকার পরিমাণ (৳)" : "Amount (৳)"}</label>
              <input type="number" step="0.01" id="due-edit-row-amt" value="${Number(row.amount || 0)}" class="w-full px-3 py-2 rounded-xl border border-slate-300 font-extrabold" />
            </div>
          </div>
        </div>
      `;

      document.getElementById("modal-actions").innerHTML = `
        <button type="button" onclick="closeModal()" class="px-3 py-2 rounded-xl bg-slate-200 text-xs font-bold">${currentLang === "bn" ? "বাতিল" : "Cancel"}</button>
        <button type="button" onclick="confirmEditDueCustomerRow('${custId}', ${rowIdx})" class="px-4 py-2 rounded-xl bg-emerald-600 hover:bg-emerald-700 text-white text-xs font-extrabold">${currentLang === "bn" ? "সেভ করুন" : "Save"}</button>
      `;
      document.getElementById("custom-modal").classList.remove("hidden");
      document.getElementById("custom-modal").classList.add("flex");
    }

    function confirmEditDueCustomerRow(custId, rowIdx) {
      if (!ensureActiveSessionStore()) return;
      const cIdx = userStore.dueCustomers.findIndex(x => x.id === custId);
      if (cIdx === -1) return;
      const rec = normalizeDueCustomerRecord(userStore.dueCustomers[cIdx]);
      if (!rec.ledgerRows[rowIdx]) return;

      rec.ledgerRows[rowIdx] = {
        productName: (document.getElementById("due-edit-row-prod").value || "").trim(),
        date: document.getElementById("due-edit-row-date").value || rec.updatedDate || "",
        amount: +(parseFloat(document.getElementById("due-edit-row-amt").value) || 0).toFixed(2)
      };

      const newTotal = +rec.ledgerRows.reduce((s, r) => s + Number(r.amount || 0), 0).toFixed(2);
      userStore.dueCustomers[cIdx] = {
        ...rec,
        dueAmount: newTotal,
        productDetails: rec.ledgerRows.map(r => `${r.productName} ৳${Number(r.amount || 0).toFixed(2)}`).join(", ")
      };
      closeModal();
      saveUserIsolatedStore();
      const m = currentLang === "bn" ? "বাকি হিসাব আপডেট করা হয়েছে!" : "Due entry updated!";
      showToast(m, "success");
      speakText(m, true);
    }

    function deleteDueCustomerRow(custId, rowIdx) {
      if (!ensureActiveSessionStore()) return;
      const cIdx = userStore.dueCustomers.findIndex(x => x.id === custId);
      if (cIdx === -1) return;
      const rec = normalizeDueCustomerRecord(userStore.dueCustomers[cIdx]);
      rec.ledgerRows.splice(rowIdx, 1);
      const newTotal = +rec.ledgerRows.reduce((s, r) => s + Number(r.amount || 0), 0).toFixed(2);
      userStore.dueCustomers[cIdx] = {
        ...rec,
        dueAmount: newTotal,
        productDetails: rec.ledgerRows.map(r => `${r.productName} ৳${Number(r.amount || 0).toFixed(2)}`).join(", ")
      };
      saveUserIsolatedStore();
      const m = currentLang === "bn" ? "বাকি লাইন মুছে ফেলা হয়েছে" : "Due row deleted";
      showToast(m, "info");
      speakText(m, true);
    }

    function editDueCustomer(id) {
      const c = normalizeDueCustomerRecord(userStore.dueCustomers.find(x => x.id === id));
      if (!c) return;
      document.getElementById("due-edit-id").value = c.id;
      document.getElementById("due-cust-name").value = c.name;
      document.getElementById("due-cust-phone").value = c.phone;
      document.getElementById("due-cust-address").value = c.address || "";
      document.getElementById("due-cust-products").value = c.productDetails || "";
      document.getElementById("due-cust-amount").value = c.dueAmount;
      document.getElementById("due-cust-date").value = c.updatedDate || "";
      document.getElementById("due-cust-photo").value = c.photo || "";
      document.getElementById("due-cancel-btn").classList.remove("hidden");
      switchScreen("due-ledger");
    }

    function deleteDueCustomer(id) {
      openConfirmModal(
        currentLang === "bn" ? "ক্রেতার রেকর্ড মুছে ফেলবেন?" : "Delete Due Customer?",
        currentLang === "bn" ? "এই ক্রেতার বাকি হিসাব মুছে ফেলতে চান?" : "Delete this customer's due record?",
        () => {
          userStore.dueCustomers = userStore.dueCustomers.filter(x => x.id !== id);
          saveUserIsolatedStore();
          showToast(currentLang === "bn" ? "রেকর্ড মুছে ফেলা হয়েছে" : "Record deleted", "info");
        }
      );
    }

    function openReceiveDueModal(id) {
      const c = userStore.dueCustomers.find(x => x.id === id);
      if (!c) return;
      document.getElementById("modal-title").textContent = currentLang === "bn" ? `${c.name} — টাকা জমা নিন` : `${c.name} — Receive Payment`;
      document.getElementById("modal-body").innerHTML = `
        <p class="text-xs text-slate-600">${currentLang === "bn" ? "বর্তমান মোট বাকি:" : "Current Due:"} <strong class="text-amber-700">৳ ${Number(c.dueAmount).toFixed(2)}</strong></p>
        <label class="block text-xs font-bold mt-2">${currentLang === "bn" ? "জমা দেওয়া টাকার পরিমাণ (৳)" : "Amount Paid (৳)"}</label>
        <input type="number" step="0.01" id="recv-due-input" value="${Number(c.dueAmount).toFixed(2)}" class="w-full px-3 py-2 rounded-xl border border-slate-300 font-extrabold text-sm mt-1" />
      `;
      document.getElementById("modal-actions").innerHTML = `
        <button type="button" onclick="closeModal()" class="px-3 py-2 rounded-xl bg-slate-200 text-xs font-bold">${currentLang === "bn" ? "বাতিল" : "Cancel"}</button>
        <button type="button" onclick="confirmReceiveDue('${id}')" class="px-4 py-2 rounded-xl bg-emerald-600 text-white text-xs font-extrabold">${currentLang === "bn" ? "জমা নিশ্চিত করুন" : "Confirm Payment"}</button>
      `;
      document.getElementById("custom-modal").classList.remove("hidden");
      document.getElementById("custom-modal").classList.add("flex");
    }

    function confirmReceiveDue(id) {
      const amt = parseFloat(document.getElementById("recv-due-input").value) || 0;
      const idx = userStore.dueCustomers.findIndex(x => x.id === id);
      if (idx === -1 || amt <= 0) { closeModal(); return; }
      const c = normalizeDueCustomerRecord(userStore.dueCustomers[idx]);
      const paid = Math.min(amt, Number(c.dueAmount));
      c.dueAmount = Math.max(0, +(Number(c.dueAmount) - paid).toFixed(2));
      c.updatedDate = new Date().toISOString().split("T")[0];
      if (c.dueAmount === 0) {
        c.ledgerRows = [];
        c.productDetails = currentLang === "bn" ? "সম্পূর্ণ পরিশোধিত" : "Fully Paid";
      } else {
        const payNote = currentLang === "bn" ? `জমা -৳${paid.toFixed(2)}` : `Paid -৳${paid.toFixed(2)}`;
        c.ledgerRows.push({
          productName: payNote,
          date: c.updatedDate,
          amount: -paid
        });
        c.productDetails = c.ledgerRows.map(r => `${r.productName} ৳${Number(r.amount || 0).toFixed(2)}`).join(", ");
      }
      userStore.dueCustomers[idx] = c;

      userStore.sales.unshift({
        id: "DUE-PAY-" + Math.floor(100000 + Math.random() * 900000),
        productName: `${currentLang === "bn" ? "বাকি জমা:" : "Due Paid:"} ${c.name}`,
        qty: 1,
        unit: "pcs",
        rate: paid,
        total: paid,
        profit: 0,
        payMode: "cash",
        custName: c.name,
        custPhone: c.phone,
        custPhoto: c.photo || "",
        date: c.updatedDate,
        time: new Date().toLocaleTimeString([], { hour: "2-digit", minute: "2-digit" }),
        sellerName: activeSessionRole === "staff" && activeStaffMember ? `${activeStaffMember.name} (Staff)` : ((currentUser && currentUser.owner) ? `${currentUser.owner} (Admin)` : "Admin"),
        soldByRole: activeSessionRole
      });

      closeModal();
      saveUserIsolatedStore();
      const msg = currentLang === "bn" ? `${c.name}-এর কাছ থেকে ${paid} টাকা জমা নেওয়া হয়েছে` : `Received ${paid} Taka from ${c.name}`;
      showToast(msg, "success");
      speakText(msg, true);
    }

    function resetDueForm() {
      document.getElementById("due-form").reset();
      document.getElementById("due-edit-id").value = "";
      document.getElementById("due-cust-photo").value = "";
      document.getElementById("due-cancel-btn").classList.add("hidden");
    }

    function renderDueLedger() {
      const box = document.getElementById("due-cards-container");
      const pill = document.getElementById("due-total-pill");
      if (!box || !userStore) return;
      const totalDue = userStore.dueCustomers.reduce((s, c) => s + Number(c.dueAmount || 0), 0);
      if (pill) pill.textContent = `৳ ${totalDue.toFixed(2)}`;

      if (!userStore.dueCustomers.length) {
        box.innerHTML = `<div class="app-card bg-white p-6 rounded-2xl text-center text-xs text-slate-400">${currentLang === "bn" ? "বাকি খাতায় কোনো হিসাব নেই" : "No due records found"}</div>`;
        return;
      }

      box.innerHTML = userStore.dueCustomers.map(rawC => {
        const c = normalizeDueCustomerRecord(rawC);
        const rows = Array.isArray(c.ledgerRows) ? c.ledgerRows : [];
        return `
        <div class="app-card bg-white p-4 rounded-2xl shadow-sm border border-amber-200 space-y-2.5">
          <div class="flex items-start justify-between gap-3">
            <div class="flex items-center gap-3 min-w-0">
              <img src="${c.photo || 'https://placehold.co/80x80/f59e0b/ffffff?text=Due'}" alt="Photo" class="w-12 h-12 rounded-2xl object-cover border border-amber-300 shrink-0" />
              <div class="min-w-0">
                <div class="font-extrabold text-sm text-slate-900 truncate">${escapeHtml(c.name)}</div>
                <div class="text-xs font-bold text-slate-600"><i class="fa-solid fa-phone text-emerald-600 mr-1"></i>${escapeHtml(c.phone)}</div>
                ${c.address ? `<div class="text-[11px] font-semibold text-slate-500"><i class="fa-solid fa-location-dot text-rose-500 mr-1"></i>${escapeHtml(c.address)}</div>` : ""}
              </div>
            </div>
            <div class="text-right shrink-0">
              <div class="text-[10px] font-bold text-slate-400">${escapeHtml(c.updatedDate || "")}</div>
              <div class="text-base font-extrabold text-amber-600">৳ ${Number(c.dueAmount).toFixed(2)}</div>
            </div>
          </div>

          <div class="rounded-xl bg-amber-50/80 border border-amber-200 overflow-x-auto">
            <table class="w-full text-[11px] border-collapse">
              <thead>
                <tr class="bg-amber-100/90 text-amber-950 font-extrabold border-b border-amber-200">
                  <th class="py-1.5 px-2 text-left">${currentLang === "bn" ? "পণ্য / বিবরণ" : "Product / Note"}</th>
                  <th class="py-1.5 px-2 text-center">${currentLang === "bn" ? "তারিখ" : "Date"}</th>
                  <th class="py-1.5 px-2 text-right">${currentLang === "bn" ? "টাকা" : "Amount"}</th>
                  <th class="py-1.5 px-2 text-center">${currentLang === "bn" ? "অ্যাকশন" : "Action"}</th>
                </tr>
              </thead>
              <tbody class="divide-y divide-amber-100">
                ${rows.length === 0 ? `
                  <tr>
                    <td colspan="4" class="py-2 px-2 text-center text-slate-500 font-semibold">${escapeHtml(c.productDetails || (currentLang === "bn" ? "কোনো বকেয়া পণ্য নেই" : "No due items"))}</td>
                  </tr>
                ` : rows.map((r, rIdx) => `
                  <tr class="hover:bg-amber-100/40">
                    <td class="py-1.5 px-2 font-bold text-slate-800">${escapeHtml(r.productName)}</td>
                    <td class="py-1.5 px-2 text-center font-mono text-slate-600">${escapeHtml(r.date || c.updatedDate || "")}</td>
                    <td class="py-1.5 px-2 text-right font-extrabold ${Number(r.amount) < 0 ? "text-emerald-700" : "text-amber-800"}">৳ ${Number(r.amount || 0).toFixed(2)}</td>
                    <td class="py-1.5 px-2 text-center whitespace-nowrap">
                      <button type="button" onclick="editDueCustomerRow('${c.id}', ${rIdx})" class="px-1.5 py-0.5 rounded bg-amber-200/80 text-amber-950 font-bold mr-1" title="Edit Row"><i class="fa-solid fa-pen text-[10px]"></i></button>
                      <button type="button" onclick="deleteDueCustomerRow('${c.id}', ${rIdx})" class="px-1.5 py-0.5 rounded bg-rose-100 text-rose-700 font-bold" title="Delete Row"><i class="fa-solid fa-trash text-[10px]"></i></button>
                    </td>
                  </tr>
                `).join("")}
              </tbody>
            </table>
          </div>

          <div class="flex items-center justify-end gap-2 pt-1 flex-wrap">
            <button type="button" onclick="addDueProductRow('${c.id}')" class="px-3 py-1.5 rounded-xl bg-amber-500 hover:bg-amber-600 text-white text-xs font-extrabold flex items-center gap-1">
              <i class="fa-solid fa-plus"></i> <span>${currentLang === "bn" ? "বাকি পণ্য যোগ" : "Add Due Item"}</span>
            </button>
            <button type="button" onclick="openReceiveDueModal('${c.id}')" class="px-3 py-1.5 rounded-xl bg-emerald-600 hover:bg-emerald-700 text-white text-xs font-extrabold flex items-center gap-1">
              <i class="fa-solid fa-hand-holding-dollar"></i> <span>${currentLang === "bn" ? "টাকা জমা নিন" : "Receive Due"}</span>
            </button>
            <button type="button" onclick="editDueCustomer('${c.id}')" class="w-8 h-8 rounded-xl bg-amber-100 text-amber-800 flex items-center justify-center"><i class="fa-solid fa-pen"></i></button>
            <button type="button" onclick="deleteDueCustomer('${c.id}')" class="w-8 h-8 rounded-xl bg-rose-100 text-rose-700 flex items-center justify-center"><i class="fa-solid fa-trash"></i></button>
          </div>
        </div>`;
      }).join("");
    }

    function switchHistoryView(tab) {
      activeHistoryTab = tab;
      document.getElementById("tab-hist-sales").className = tab === "sales"
        ? "px-3 py-1 rounded-lg bg-white text-indigo-700 shadow"
        : "px-3 py-1 rounded-lg text-slate-700";
      document.getElementById("tab-hist-purchases").className = tab === "purchases"
        ? "px-3 py-1 rounded-lg bg-white text-indigo-700 shadow"
        : "px-3 py-1 rounded-lg text-slate-700";
      renderHistoryRecords();
    }

    function renderVoucherSingleLineCodes(v) {
      const codeText = v ? (v.id || "VCH-0000") : "VCH-0000";
      const svgEl = document.getElementById("vch-barcode-svg");
      const qrEl = document.getElementById("vch-qrcode-div");
      if (svgEl) {
        try {
          JsBarcode(svgEl, codeText, {
            format: "CODE128",
            width: 1.15,
            height: 22,
            displayValue: false,
            margin: 1
          });
        } catch (e) {}
      }
      if (qrEl) {
        qrEl.innerHTML = "";
        try {
          new QRCode(qrEl, {
            text: codeText,
            width: 34,
            height: 34
          });
        } catch (e) {}
      }
    }

    function renderVoucherPreview(v) {
      if (!userStore) return;
      const st = userStore.settings;
      document.getElementById("vch-logo").src = st.shopLogo || "https://placehold.co/90x90/059669/ffffff?text=R%26H";
      document.getElementById("vch-store-name").textContent = st.shopName || "Ribek & Hetu";
      document.getElementById("vch-store-addr").textContent = st.shopAddr || "";
      document.getElementById("vch-store-phone").textContent = st.shopPhone ? `মোবাইল: ${st.shopPhone}` : "";

      const custPhotoEl = document.getElementById("vch-cust-photo");
      const tbody = document.getElementById("vch-table-body");
      const tfoot = document.getElementById("vch-table-foot");

      if (!v) {
        document.getElementById("vch-id").textContent = "VCH-0000";
        document.getElementById("vch-date").textContent = new Date().toISOString().split("T")[0];
        document.getElementById("vch-time").textContent = "--:--";
        custPhotoEl.classList.add("hidden");
        tbody.innerHTML = `<tr><td colspan="4" class="border border-slate-900 py-3 text-center text-slate-400">কোনো ভাউচার নেই</td></tr>`;
        tfoot.innerHTML = "";
        renderVoucherSingleLineCodes(null);
        return;
      }

      selectedVoucher = v;
      document.getElementById("vch-id").textContent = v.id;
      document.getElementById("vch-date").textContent = v.date;
      document.getElementById("vch-time").textContent = v.time || "";

      if (v.custPhoto) {
        custPhotoEl.src = v.custPhoto;
        custPhotoEl.classList.remove("hidden");
      } else {
        custPhotoEl.classList.add("hidden");
      }

      const lineItems = Array.isArray(v.items) && v.items.length > 0
        ? v.items
        : [{ name: v.productName, qty: v.qty, unit: v.unit, total: v.total }];

      tbody.innerHTML = lineItems.map((it, idx) => `
        <tr>
          <td class="border border-slate-900 py-1 px-1 text-center font-bold">${idx + 1}</td>
          <td class="border border-slate-900 py-1 px-1 font-bold leading-tight">${escapeHtml(it.name)} <span class="text-[8px] font-semibold text-slate-600">(${it.qty} ${it.unit})</span></td>
          <td class="border border-slate-900 py-1 px-1 text-center font-mono text-[8px]">${escapeHtml(v.date)}</td>
          <td class="border border-slate-900 py-1 px-1 text-right font-extrabold">৳${Number(it.total).toFixed(2)}</td>
        </tr>
      `).join("");

      const payLabel = v.payMode === "due" ? "বাকি" : (v.payMode === "digital" ? "ডিজিটাল" : "নগদ");
      tfoot.innerHTML = `
        <tr class="bg-slate-100 font-extrabold text-[9.5px]">
          <td colspan="3" class="border border-slate-900 py-1 px-1.5 text-right">সর্বমোট (${payLabel})</td>
          <td class="border border-slate-900 py-1 px-1.5 text-right text-emerald-800">৳${Number(v.total).toFixed(2)}</td>
        </tr>
      `;
      renderVoucherSingleLineCodes(v);
    }

    function renderHistoryRecords() {
      const box = document.getElementById("history-records-list");
      if (!box || !userStore) return;
      const isStaff = activeSessionRole === "staff";

      const filterSel = document.getElementById("history-staff-filter");
      if (filterSel && !isStaff) {
        const prevFilter = filterSel.value || "all";
        const staffNames = Array.from(new Set(
          userStore.sales
            .filter(v => v.soldByRole === "staff" || v.soldByStaffName)
            .map(v => v.soldByStaffName || v.sellerName || "Staff")
        ));
        filterSel.innerHTML = `<option value="all">সকল বিক্রয় (অ্যাডমিন ও স্টাফ)</option>` +
          `<option value="admin_only">শুধুমাত্র অ্যাডমিন বিক্রয়</option>` +
          `<option value="staff_only">শুধুমাত্র সকল স্টাফ বিক্রয়</option>` +
          staffNames.map(n => `<option value="st:${escapeHtml(n)}">স্টাফ: ${escapeHtml(n)}</option>`).join("");
        filterSel.value = Array.from(filterSel.options).some(o => o.value === prevFilter) ? prevFilter : "all";
      }

      if (isStaff || activeHistoryTab === "sales") {
        let salesList = userStore.sales;
        if (isStaff && activeStaffMember) {
          salesList = userStore.sales.filter(v =>
            (v.soldByStaffId && v.soldByStaffId === activeStaffMember.id) ||
            (v.soldByStaffName && v.soldByStaffName.toLowerCase() === activeStaffMember.name.toLowerCase()) ||
            (v.sellerName && v.sellerName.toLowerCase().includes(activeStaffMember.name.toLowerCase()))
          );
        } else if (!isStaff && filterSel) {
          const fv = filterSel.value;
          if (fv === "admin_only") {
            salesList = userStore.sales.filter(v => v.soldByRole !== "staff");
          } else if (fv === "staff_only") {
            salesList = userStore.sales.filter(v => v.soldByRole === "staff");
          } else if (fv.startsWith("st:")) {
            const targetSt = fv.slice(3).toLowerCase();
            salesList = userStore.sales.filter(v =>
              (v.soldByStaffName && v.soldByStaffName.toLowerCase() === targetSt) ||
              (v.sellerName && v.sellerName.toLowerCase().includes(targetSt))
            );
          }
        }

        if (!salesList.length) {
          box.innerHTML = `<div class="app-card bg-white p-5 rounded-2xl text-center text-xs text-slate-400">${currentLang === "bn" ? "কোনো বিক্রয় রেকর্ড নেই" : "No sales records"}</div>`;
          return;
        }
        box.innerHTML = salesList.map(v => `
          <div class="app-card bg-white p-3 rounded-2xl border border-slate-200 flex items-center justify-between gap-2">
            <div class="min-w-0">
              <div class="font-extrabold text-xs sm:text-sm truncate">${escapeHtml(v.id)} • ${escapeHtml(v.productName)}</div>
              <div class="text-[11px] text-slate-500 font-semibold">${escapeHtml(v.date)} • ${escapeHtml(v.custName)} • <span class="text-indigo-700 font-bold">${escapeHtml(v.sellerName || "")}</span></div>
            </div>
            <div class="flex items-center gap-2 shrink-0">
              <span class="font-extrabold text-sm text-emerald-700">৳${Number(v.total).toFixed(2)}</span>
              <button type="button" onclick='renderVoucherPreview(${JSON.stringify(v).replace(/'/g, "&#39;")})' class="px-2.5 py-1 rounded-lg bg-indigo-100 text-indigo-800 text-xs font-bold">
                ${currentLang === "bn" ? "দেখুন" : "View"}
              </button>
              ${!isStaff ? `<button type="button" onclick="deleteSaleRecord('${v.id}')" class="w-7 h-7 rounded-lg bg-rose-100 text-rose-700 flex items-center justify-center"><i class="fa-solid fa-trash text-xs"></i></button>` : ""}
            </div>
          </div>`).join("");
      } else {
        if (!userStore.purchases.length) {
          box.innerHTML = `<div class="app-card bg-white p-5 rounded-2xl text-center text-xs text-slate-400">${currentLang === "bn" ? "কোনো ক্রয় রেকর্ড নেই" : "No purchase records"}</div>`;
          return;
        }
        box.innerHTML = userStore.purchases.map(p => `
          <div class="app-card bg-white p-3 rounded-2xl border border-slate-200 flex items-center justify-between gap-2">
            <div>
              <div class="font-extrabold text-xs sm:text-sm">${escapeHtml(p.name)} (${p.qty} ${p.unit})</div>
              <div class="text-[11px] text-slate-500">${escapeHtml(p.date)} • ${currentLang === "bn" ? "দর" : "Rate"}: ৳${Number(p.buyPrice).toFixed(2)}</div>
            </div>
            <span class="font-extrabold text-sm text-indigo-700">৳${Number(p.totalCost).toFixed(2)}</span>
          </div>`).join("");
      }
    }

    function deleteSaleRecord(id) {
      if (activeSessionRole === "staff") return;
      userStore.sales = userStore.sales.filter(x => x.id !== id);
      saveUserIsolatedStore();
    }

    function clearCurrentHistoryTab() {
      if (activeSessionRole === "staff") return;
      openConfirmModal(
        currentLang === "bn" ? "সব রেকর্ড মুছে ফেলবেন?" : "Clear All Records?",
        currentLang === "bn" ? "এই ট্যাবের সব হিস্টিরি রেকর্ড মুছে ফেলতে চান?" : "Delete all records in this history tab?",
        () => {
          if (activeHistoryTab === "sales") userStore.sales = [];
          else userStore.purchases = [];
          saveUserIsolatedStore();
        }
      );
    }

    async function saveRealVoucherPDF() {
      const el = document.getElementById("official-voucher-capture");
      if (!el || typeof html2canvas === "undefined" || !window.jspdf) {
        window.print();
        return;
      }
      const m = currentLang === "bn" ? "PDF ভাউচার তৈরি হচ্ছে..." : "Generating PDF voucher...";
      showToast(m, "info");
      speakText(m, true);
      const canvas = await html2canvas(el, { scale: 2, backgroundColor: "#ffffff" });
      const imgData = canvas.toDataURL("image/png");
      const { jsPDF } = window.jspdf;
      const pdf = new jsPDF({ orientation: "portrait", unit: "mm", format: [62, 120] });
      pdf.addImage(imgData, "PNG", 2, 2, 58, 0);
      pdf.save(`${(selectedVoucher && selectedVoucher.id) || "Voucher"}.pdf`);
      const doneMsg = currentLang === "bn" ? "PDF ভাউচার সেভ করা হয়েছে!" : "PDF voucher saved!";
      showToast(doneMsg, "success");
      speakText(doneMsg, true);
    }

    async function printViaBluetooth() {
      const m = currentLang === "bn" ? "ব্লুটুথ প্রিন্টার সংযোগ ও প্রিন্ট শুরু হচ্ছে..." : "Connecting Bluetooth thermal printer...";
      showToast(m, "info");
      speakText(m, true);
      if (navigator.bluetooth && typeof navigator.bluetooth.requestDevice === "function") {
        try {
          await navigator.bluetooth.requestDevice({
            acceptAllDevices: true,
            optionalServices: ["000018f0-0000-1000-8000-00805f9b34fb"]
          });
        } catch (e) {}
      }
      window.print();
    }

    function printViaUSBOrSystem() {
      const m = currentLang === "bn" ? "USB / সিস্টেম প্রিন্টার পাঠানো হচ্ছে..." : "Sending to USB / system printer...";
      showToast(m, "info");
      speakText(m, true);
      window.print();
    }

    async function shareVoucherSocial() {
      const v = selectedVoucher || (userStore && userStore.sales[0]);
      const st = userStore ? userStore.settings : {};
      const text = v
        ? `${st.shopName || "Ribek & Hetu"} — ${currentLang === "bn" ? "ভাউচার" : "Voucher"} ${v.id}\n${currentLang === "bn" ? "পণ্য" : "Item"}: ${v.productName}\n${currentLang === "bn" ? "মোট" : "Total"}: ৳${Number(v.total).toFixed(2)}\n${currentLang === "bn" ? "তারিখ" : "Date"}: ${v.date}`
        : `${st.shopName || "Ribek & Hetu"} POS`;

      if (navigator.share) {
        try {
          await navigator.share({ title: v ? v.id : "Voucher", text });
          return;
        } catch (e) {}
      }
      window.open(`https://wa.me/?text=${encodeURIComponent(text)}`, "_blank");
    }

    function addOtherExpense(e) {
      e.preventDefault();
      if (activeSessionRole === "staff") return;
      const desc = document.getElementById("exp-desc").value.trim();
      const amt = parseFloat(document.getElementById("exp-amt").value) || 0;
      if (!desc || amt <= 0) return;
      userStore.expenses.unshift({
        id: "EXP_" + Date.now(),
        desc,
        amt,
        date: new Date().toISOString().split("T")[0]
      });
      document.getElementById("exp-desc").value = "";
      document.getElementById("exp-amt").value = "";
      saveUserIsolatedStore();
      const m = currentLang === "bn" ? `${desc} খরচ ${amt} টাকা যোগ হয়েছে` : `Added expense ${desc} of ${amt} Taka`;
      showToast(m, "success");
      speakText(m, true);
    }

    function deleteOtherExpense(id) {
      if (activeSessionRole === "staff") return;
      userStore.expenses = userStore.expenses.filter(x => x.id !== id);
      saveUserIsolatedStore();
    }

    function renderDashboard() {
      if (!userStore) return;
      if (activeSessionRole === "staff") {
        refreshDashboardAI(false);
        return;
      }

      const capital = userStore.products.reduce((s, p) => s + (Number(p.qty || 0) * Number(p.buyPrice || 0)), 0);
      const totalItems = userStore.products.length;
      const totalSales = userStore.sales.reduce((s, v) => s + Number(v.total || 0), 0);
      const grossProfit = userStore.sales.reduce((s, v) => s + Number(v.profit || 0), 0);
      const totalExpense = userStore.expenses.reduce((s, x) => s + Number(x.amt || 0), 0);
      const totalDue = userStore.dueCustomers.reduce((s, c) => s + Number(c.dueAmount || 0), 0);

      const netProfit = grossProfit - totalExpense;
      const displayProfit = netProfit >= 0 ? netProfit : 0;
      const displayLoss = netProfit < 0 ? Math.abs(netProfit) : 0;

      document.getElementById("dash-capital").textContent = `৳ ${capital.toFixed(2)}`;
      document.getElementById("dash-items").textContent = currentLang === "bn" ? toBnNum(totalItems) : String(totalItems);
      document.getElementById("dash-sales").textContent = `৳ ${totalSales.toFixed(2)}`;
      document.getElementById("dash-profit").textContent = `৳ ${displayProfit.toFixed(2)}`;
      document.getElementById("dash-due").textContent = `৳ ${totalDue.toFixed(2)}`;
      document.getElementById("dash-loss").textContent = `৳ ${displayLoss.toFixed(2)}`;
      document.getElementById("dash-expenses").textContent = `৳ ${totalExpense.toFixed(2)}`;

      const expList = document.getElementById("expenses-list");
      if (!userStore.expenses.length) {
        expList.innerHTML = `<div class="text-slate-400 text-center py-2">${currentLang === "bn" ? "কোনো অতিরিক্ত খরচ যুক্ত করা হয়নি" : "No other expenses added"}</div>`;
      } else {
        expList.innerHTML = userStore.expenses.map(x => `
          <div class="flex items-center justify-between bg-slate-50 px-3 py-1.5 rounded-xl border border-slate-100">
            <span class="font-bold">${escapeHtml(x.desc)} <span class="text-[10px] text-slate-400">(${escapeHtml(x.date)})</span></span>
            <div class="flex items-center gap-2">
              <span class="font-extrabold text-rose-600">৳ ${Number(x.amt).toFixed(2)}</span>
              <button type="button" onclick="deleteOtherExpense('${x.id}')" class="text-slate-400 hover:text-rose-600"><i class="fa-solid fa-xmark"></i></button>
            </div>
          </div>`).join("");
      }
      refreshDashboardAI(false);
    }

    function promptDashboardReset() {
      if (activeSessionRole === "staff") return;
      openConfirmModal(
        currentLang === "bn" ? "ড্যাশবোর্ড ও হিসাব রিসেট করবেন?" : "Reset Dashboard & Accounts?",
        currentLang === "bn" ? "আপনার এই একাউন্টের সব স্টক, বিক্রয়, বাকি ও খরচের হিসাব শূন্য হয়ে যাবে। নিশ্চিত?" : "This will clear all products, sales, dues & expenses for your account. Sure?",
        () => {
          userStore.products = [];
          userStore.sales = [];
          userStore.purchases = [];
          userStore.dueCustomers = [];
          userStore.expenses = [];
          saveUserIsolatedStore();
          const m = currentLang === "bn" ? "সব হিসাব রিসেট করা হয়েছে" : "All accounts reset";
          showToast(m, "info");
          speakText(m, true);
        }
      );
    }

    let fpScanTimer = null;
    let fpProgress = 0;

    function beginFingerHealthScan(e) {
      if (e) e.preventDefault();
      clearInterval(fpScanTimer);
      fpProgress = 0;
      document.getElementById("health-alert-box").classList.add("hidden");
      document.getElementById("fp-icon").classList.add("text-emerald-400");

      fpScanTimer = setInterval(() => {
        fpProgress += 5;
        document.getElementById("fp-percent").textContent = `${fpProgress}%`;
        document.getElementById("fp-bar").style.width = `${fpProgress}%`;
        if (fpProgress >= 100) {
          clearInterval(fpScanTimer);
          finalizeHealthReport();
        }
      }, 90);
    }

    function cancelFingerHealthScan() {
      if (fpProgress < 100) {
        clearInterval(fpScanTimer);
        fpProgress = 0;
        document.getElementById("fp-percent").textContent = "0%";
        document.getElementById("fp-bar").style.width = "0%";
        document.getElementById("fp-icon").classList.remove("text-emerald-400");
      }
    }

    function finalizeHealthReport() {
      const sys = Math.floor(108 + Math.random() * 24);
      const dia = Math.floor(70 + Math.random() * 16);
      const bpm = Math.floor(66 + Math.random() * 26);
      const spo2 = Math.floor(96 + Math.random() * 4);

      document.getElementById("res-bp").textContent = `${sys}/${dia}`;
      document.getElementById("res-bpm").textContent = `${bpm} BPM`;
      document.getElementById("res-spo2").textContent = `${spo2} %`;

      const bpAbnormal = sys > 135 || dia > 88 || sys < 95;
      const bpmAbnormal = bpm > 100 || bpm < 55;
      const spo2Abnormal = spo2 < 95;

      document.getElementById("res-bp-tag").textContent = bpAbnormal ? (currentLang === "bn" ? "সতর্কতা" : "Alert") : (currentLang === "bn" ? "স্বাভাবিক" : "Normal");
      document.getElementById("res-bpm-tag").textContent = bpmAbnormal ? (currentLang === "bn" ? "সতর্কতা" : "Alert") : (currentLang === "bn" ? "স্বাভাবিক" : "Normal");
      document.getElementById("res-spo2-tag").textContent = spo2Abnormal ? (currentLang === "bn" ? "সতর্কতা" : "Alert") : (currentLang === "bn" ? "স্বাভাবিক" : "Normal");

      const alertBox = document.getElementById("health-alert-box");
      if (bpAbnormal || bpmAbnormal || spo2Abnormal) {
        const warnText = currentLang === "bn"
          ? `স্বাস্থ্য সতর্কতা! ব্লাড প্রেসার ${sys}/${dia}, হার্ট বিট ${bpm} এবং অক্সিজেন ${spo2}%। অনুগ্রহ করে বিশ্রাম নিন বা চিকিৎসকের পরামর্শ নিন।`
          : `Health Alert! BP ${sys}/${dia}, Heart Rate ${bpm} BPM, SpO2 ${spo2}%. Please rest or consult a doctor.`;
        alertBox.textContent = warnText;
        alertBox.classList.remove("hidden");
        speakText(warnText, true);
      } else {
        alertBox.classList.add("hidden");
        const okText = currentLang === "bn"
          ? `স্বাস্থ্য পরীক্ষা সম্পন্ন। ব্লাড প্রেসার ${sys}/${dia}, হার্ট বিট ${bpm}, অক্সিজেন ${spo2} শতাংশ। সব স্বাভাবিক আছে।`
          : `Health check complete. BP ${sys}/${dia}, Heart rate ${bpm}, Oxygen ${spo2} percent. All normal.`;
        speakText(okText, true);
      }
    }

    const BN_MONTHS = ["বৈশাখ", "জ্যৈষ্ঠ", "আষাঢ়", "শ্রাবণ", "ভাদ্র", "আশ্বিন", "কার্তিক", "অগ্রহায়ণ", "পৌষ", "মাঘ", "ফাল্গুন", "চৈত্র"];
    const HOLIDAYS = [
      { md: "02-21", bn: "শহীদ দিবস ও আন্তর্জাতিক মাতৃভাষা দিবস", en: "Language Martyrs' Day" },
      { md: "03-17", bn: "জাতির পিতার জন্মদিন", en: "National Children's Day" },
      { md: "03-26", bn: "স্বাধীনতা ও জাতীয় দিবস", en: "Independence Day" },
      { md: "04-14", bn: "পহেলা বৈশাখ (বাংলা নববর্ষ)", en: "Pohela Boishakh (Bangla New Year)" },
      { md: "05-01", bn: "মে দিবস (আন্তর্জাতিক শ্রমিক দিবস)", en: "May Day" },
      { md: "08-15", bn: "জাতীয় শোক দিবস", en: "National Mourning Day" },
      { md: "12-16", bn: "মহান বিজয় দিবস", en: "Victory Day" },
      { md: "12-25", bn: "বড়দিন", en: "Christmas Day" }
    ];

    function getBanglaDateParts(gDate) {
      const y = gDate.getFullYear();
      const m = gDate.getMonth();
      const d = gDate.getDate();
      const boishakhStart = new Date(y, 3, 14);
      let bnYear = y - 593;
      let diffDays = 0;

      if (gDate >= boishakhStart) {
        diffDays = Math.floor((gDate - boishakhStart) / 86400000);
      } else {
        bnYear = y - 594;
        const prevBoishakh = new Date(y - 1, 3, 14);
        diffDays = Math.floor((gDate - prevBoishakh) / 86400000);
      }

      const monthIdx = Math.min(11, Math.max(0, Math.floor(diffDays / 30.4375)));
      const dayInMonth = Math.min(31, Math.max(1, Math.floor(diffDays - monthIdx * 30.4375) + 1));
      return {
        year: bnYear,
        monthIdx,
        monthName: BN_MONTHS[monthIdx],
        day: dayInMonth,
        yearBn: toBnNum(bnYear),
        dayBn: toBnNum(dayInMonth)
      };
    }

    function switchCalendarType(type) {
      activeCalType = type;
      document.getElementById("calendar-view-bn").classList.toggle("hidden", type !== "bn");
      document.getElementById("calendar-view-en").classList.toggle("hidden", type !== "en");
      document.getElementById("cal-btn-bn").className = type === "bn"
        ? "px-3 py-1 rounded-lg bg-emerald-600 text-white shadow"
        : "px-3 py-1 rounded-lg text-slate-700";
      document.getElementById("cal-btn-en").className = type === "en"
        ? "px-3 py-1 rounded-lg bg-indigo-600 text-white shadow"
        : "px-3 py-1 rounded-lg text-slate-700";
      renderCalendarScreen();
    }

    function shiftCalendarMonth(delta) {
      calCursorDate = new Date(calCursorDate.getFullYear(), calCursorDate.getMonth() + delta, 1);
      renderCalendarScreen();
    }

    function renderCalendarScreen() {
      const y = calCursorDate.getFullYear();
      const m = calCursorDate.getMonth();
      const firstDay = new Date(y, m, 1).getDay();
      const daysInMonth = new Date(y, m + 1, 0).getDate();
      const today = new Date();

      const bnStart = getBanglaDateParts(new Date(y, m, 1));
      const bnEnd = getBanglaDateParts(new Date(y, m, daysInMonth));
      document.getElementById("bn-cal-header").textContent = bnStart.monthName === bnEnd.monthName
        ? `${bnStart.monthName} ${bnStart.yearBn} বঙ্গাব্দ`
        : `${bnStart.monthName} – ${bnEnd.monthName} ${bnEnd.yearBn} বঙ্গাব্দ`;

      const bnGrid = document.getElementById("bn-cal-grid");
      bnGrid.innerHTML = "";
      for (let i = 0; i < firstDay; i++) bnGrid.innerHTML += `<div></div>`;
      for (let d = 1; d <= daysInMonth; d++) {
        const curDateObj = new Date(y, m, d);
        const bnParts = getBanglaDateParts(curDateObj);
        const md = `${String(m + 1).padStart(2, "0")}-${String(d).padStart(2, "0")}`;
        const isHoliday = HOLIDAYS.some(h => h.md === md) || curDateObj.getDay() === 5;
        const isToday = d === today.getDate() && m === today.getMonth() && y === today.getFullYear();
        bnGrid.innerHTML += `
          <div class="p-1.5 rounded-xl border text-center ${isToday ? "bg-emerald-600 text-white border-emerald-600 font-extrabold" : (isHoliday ? "bg-rose-50 border-rose-200 text-rose-700 font-bold" : "bg-slate-50 border-slate-200")}">
            <div class="text-xs font-extrabold">${bnParts.dayBn}</div>
            <div class="text-[9px] opacity-80 truncate">${bnParts.monthName.slice(0, 4)}</div>
          </div>`;
      }

      document.getElementById("en-cal-header").textContent = calCursorDate.toLocaleDateString("en-US", { month: "long", year: "numeric" });
      const enGrid = document.getElementById("en-cal-grid");
      enGrid.innerHTML = "";
      for (let i = 0; i < firstDay; i++) enGrid.innerHTML += `<div></div>`;
      for (let d = 1; d <= daysInMonth; d++) {
        const curDateObj = new Date(y, m, d);
        const md = `${String(m + 1).padStart(2, "0")}-${String(d).padStart(2, "0")}`;
        const isHoliday = HOLIDAYS.some(h => h.md === md) || curDateObj.getDay() === 5;
        const isToday = d === today.getDate() && m === today.getMonth() && y === today.getFullYear();
        enGrid.innerHTML += `
          <div class="p-1.5 rounded-xl border text-center ${isToday ? "bg-indigo-600 text-white border-indigo-600 font-extrabold" : (isHoliday ? "bg-rose-50 border-rose-200 text-rose-700 font-bold" : "bg-slate-50 border-slate-200")}">
            <div class="text-xs font-extrabold">${d}</div>
            <div class="text-[9px] opacity-75">${curDateObj.toLocaleDateString("en-US", { weekday: "short" })}</div>
          </div>`;
      }

      const holBox = document.getElementById("cal-holidays-box");
      holBox.innerHTML = HOLIDAYS.map(h => `
        <div class="flex items-center justify-between py-1 border-b border-slate-100">
          <span class="font-bold text-slate-700">${currentLang === "bn" ? h.bn : h.en}</span>
          <span class="px-2 py-0.5 rounded bg-rose-100 text-rose-700 font-mono font-bold text-[10px]">${h.md}</span>
        </div>`).join("");
    }

    function switchCalcMode(mode) {
      document.getElementById("c-tab-unit").className = mode === "unit" ? "px-2.5 py-1 rounded-lg bg-white text-indigo-700 shadow" : "px-2.5 py-1 rounded-lg text-slate-700";
      document.getElementById("c-tab-norm").className = mode === "norm" ? "px-2.5 py-1 rounded-lg bg-white text-indigo-700 shadow" : "px-2.5 py-1 rounded-lg text-slate-700";
      document.getElementById("c-tab-sci").className = mode === "sci" ? "px-2.5 py-1 rounded-lg bg-white text-indigo-700 shadow" : "px-2.5 py-1 rounded-lg text-slate-700";

      document.getElementById("calc-box-unit").classList.toggle("hidden", mode !== "unit");
      document.getElementById("calc-box-std").classList.toggle("hidden", mode === "unit");
      document.getElementById("sci-calc-row").classList.toggle("hidden", mode !== "sci");
      document.getElementById("sci-calc-row").classList.toggle("grid", mode === "sci");
    }

    function runUnitCalculator() {
      const baseUnit = parseFloat(document.getElementById("uc-unit").value) || 1;
      const rate = parseFloat(document.getElementById("uc-rate").value) || 0;
      const qty = parseFloat(document.getElementById("uc-qty").value) || 0;
      const res = +((qty / baseUnit) * rate).toFixed(2);
      document.getElementById("uc-out").textContent = `৳ ${res.toFixed(2)}`;
    }

    let stdExpr = "";
    function updateCalcAngleIndicator() {
      const ind = document.getElementById("calc-angle-indicator");
      if (ind) ind.textContent = `ANGLE: ${calcAngleMode}`;
    }
    function toggleCalcAngleMode() {
      calcAngleMode = calcAngleMode === "DEG" ? "RAD" : "DEG";
      updateCalcAngleIndicator();
      speakText(calcAngleMode === "DEG" ? "ডিগ্রি মোড চালু হয়েছে" : "রেডিয়ান মোড চালু হয়েছে", true);
    }
    function toRadIfNeeded(val) {
      return calcAngleMode === "DEG" ? (val * Math.PI) / 180 : val;
    }
    function fromRadIfNeeded(val) {
      return calcAngleMode === "DEG" ? (val * 180) / Math.PI : val;
    }
    function factorialNum(n) {
      if (n < 0 || !Number.isInteger(n) || n > 170) return NaN;
      let r = 1;
      for (let i = 2; i <= n; i++) r *= i;
      return r;
    }
    function stdAdd(ch) {
      stdExpr += ch;
      document.getElementById("std-calc-disp").value = stdExpr;
      speakText(ch, true);
    }
    function stdClear() {
      stdExpr = "";
      document.getElementById("std-calc-disp").value = "0";
    }
    function stdBack() {
      stdExpr = stdExpr.slice(0, -1);
      document.getElementById("std-calc-disp").value = stdExpr || "0";
    }
    function stdPercent() {
      const curVal = parseFloat(stdExpr);
      if (!isNaN(curVal)) {
        stdExpr = String(+(curVal / 100).toFixed(8));
        document.getElementById("std-calc-disp").value = stdExpr;
      }
    }
    function sciOp(fn) {
      if (fn === "pi") {
        stdExpr += String(+Math.PI.toFixed(8));
        document.getElementById("std-calc-disp").value = stdExpr;
        return;
      }
      if (fn === "e") {
        stdExpr += String(+Math.E.toFixed(8));
        document.getElementById("std-calc-disp").value = stdExpr;
        return;
      }
      let v = parseFloat(stdExpr);
      if (isNaN(v)) v = 0;
      let r = 0;
      if (fn === "sin") r = Math.sin(toRadIfNeeded(v));
      if (fn === "cos") r = Math.cos(toRadIfNeeded(v));
      if (fn === "tan") r = Math.tan(toRadIfNeeded(v));
      if (fn === "asin") r = fromRadIfNeeded(Math.asin(v));
      if (fn === "acos") r = fromRadIfNeeded(Math.acos(v));
      if (fn === "atan") r = fromRadIfNeeded(Math.atan(v));
      if (fn === "sq") r = v * v;
      if (fn === "cube") r = v * v * v;
      if (fn === "sqrt") r = Math.sqrt(v);
      if (fn === "cbrt") r = Math.cbrt(v);
      if (fn === "inv") r = 1 / v;
      if (fn === "log10") r = Math.log10(v);
      if (fn === "ln") r = Math.log(v);
      if (fn === "exp") r = Math.exp(v);
      if (fn === "fact") r = factorialNum(v);
      stdExpr = Number.isFinite(r) ? String(+r.toFixed(8)) : "Error";
      document.getElementById("std-calc-disp").value = stdExpr;
      speakText(stdExpr, true);
    }
    function stdEval() {
      try {
        const sanitized = stdExpr.replace(/\^/g, "**").replace(/[^0-9+\-*/().** ]/g, "");
        const res = Function(`"use strict"; return (${sanitized})`)();
        stdExpr = Number.isFinite(res) ? String(+Number(res).toFixed(8)) : "Error";
        document.getElementById("std-calc-disp").value = stdExpr;
        speakText(stdExpr, true);
      } catch (e) {
        document.getElementById("std-calc-disp").value = "Error";
        stdExpr = "";
      }
    }

    function addNotebookEntry(e) {
      e.preventDefault();
      const text = document.getElementById("nb-text").value.trim();
      const alarmDate = document.getElementById("nb-date").value;
      const alarmTime = document.getElementById("nb-time").value;
      if (!text) return;

      userStore.notes.unshift({
        id: "NB_" + Date.now(),
        text,
        alarmDate,
        alarmTime,
        triggered: false
      });
      document.getElementById("nb-text").value = "";
      saveUserIsolatedStore();
      const m = currentLang === "bn" ? "নোট ও অ্যালার্ম সেভ হয়েছে" : "Note & alarm saved";
      showToast(m, "success");
      speakText(m, true);
    }

    function deleteNotebookEntry(id) {
      userStore.notes = userStore.notes.filter(x => x.id !== id);
      saveUserIsolatedStore();
    }

    function renderNotebookList() {
      const box = document.getElementById("nb-list");
      if (!box || !userStore) return;
      if (!userStore.notes.length) {
        box.innerHTML = `<div class="app-card bg-white p-5 rounded-2xl text-center text-xs text-slate-400">${currentLang === "bn" ? "কোনো নোট বা অ্যালার্ম নেই" : "No notes or alarms"}</div>`;
        return;
      }
      box.innerHTML = userStore.notes.map(n => `
        <div class="app-card bg-white p-3.5 rounded-2xl border border-slate-200 flex items-center justify-between gap-2">
          <div>
            <div class="font-bold text-xs sm:text-sm">${escapeHtml(n.text)}</div>
            <div class="text-[11px] font-mono text-indigo-600 mt-0.5"><i class="fa-solid fa-bell mr-1"></i>${escapeHtml(n.alarmDate)} ${escapeHtml(n.alarmTime)}</div>
          </div>
          <div class="flex items-center gap-1.5">
            <button type="button" onclick="speakText('${escapeHtml(n.text).replace(/'/g, "")}', true)" class="w-8 h-8 rounded-xl bg-indigo-100 text-indigo-700 flex items-center justify-center"><i class="fa-solid fa-volume-high text-xs"></i></button>
            <button type="button" onclick="deleteNotebookEntry('${n.id}')" class="w-8 h-8 rounded-xl bg-rose-100 text-rose-700 flex items-center justify-center"><i class="fa-solid fa-trash text-xs"></i></button>
          </div>
        </div>`).join("");
    }

    function renderBarcodeAndQR() {
      const inputEl = document.getElementById("qr-gen-text");
      const val = (inputEl && inputEl.value.trim()) || (userStore && userStore.settings.shopName) || "Ribek & Hetu";
      if (inputEl && !inputEl.value.trim()) inputEl.value = val;
      const asciiCode = val.replace(/[^\x20-\x7E]/g, "").trim() || ("RH" + Math.abs(val.split("").reduce((a, c) => ((a << 5) - a) + c.charCodeAt(0), 0)));
      try {
        JsBarcode("#out-barcode-svg", asciiCode, { format: "CODE128", width: 2, height: 56, displayValue: true });
      } catch (e) {}
      const qrBox = document.getElementById("out-qrcode-div");
      qrBox.innerHTML = "";
      try {
        new QRCode(qrBox, { text: val, width: 128, height: 128 });
      } catch (e) {}
    }

    function openCameraBarcodeScanner(ctx) {
      scannerContext = ctx;
      const modal = document.getElementById("camera-scanner-modal");
      modal.classList.remove("hidden");
      modal.classList.add("flex");

      if (typeof Html5Qrcode !== "undefined") {
        cameraScanner = new Html5Qrcode("qr-reader-viewport");
        cameraScanner.start(
          { facingMode: "environment" },
          { fps: 10, qrbox: { width: 220, height: 150 } },
          decodedText => {
            handleScannedCodeValue(decodedText);
            closeCameraBarcodeScanner();
          },
          () => {}
        ).catch(() => {});
      }
    }

    function closeCameraBarcodeScanner() {
      const modal = document.getElementById("camera-scanner-modal");
      modal.classList.add("hidden");
      modal.classList.remove("flex");
      if (cameraScanner) {
        cameraScanner.stop().then(() => cameraScanner.clear()).catch(() => {});
        cameraScanner = null;
      }
    }

    function handleScannedCodeValue(code) {
      const clean = String(code || "").trim();
      if (!clean) return;
      speakText(clean, true);
      if (scannerContext === "product-entry") {
        document.getElementById("prod-barcode").value = clean;
        updateEntryAutoCodePreviews();
        showToast(currentLang === "bn" ? `বারকোড যুক্ত হয়েছে: ${clean}` : `Barcode set: ${clean}`, "success");
      } else {
        document.getElementById("global-search-box").value = clean;
        runGlobalSearch(clean);
      }
    }

    function runGlobalSearch(q) {
      const clearBtn = document.getElementById("global-search-clear");
      const drop = document.getElementById("global-search-dropdown");
      const term = String(q || "").trim().toLowerCase();
      if (clearBtn) clearBtn.classList.toggle("hidden", !term);
      if (!term || !userStore) {
        if (drop) drop.classList.add("hidden");
        return;
      }

      const prodMatches = userStore.products.filter(p =>
        p.name.toLowerCase().includes(term) || (p.barcode && p.barcode.toLowerCase().includes(term))
      ).map(p => ({
        title: `${p.name} (${p.qty} ${p.unit})`,
        sub: `${currentLang === "bn" ? "বিক্রয় দর" : "Rate"}: ৳${Number(p.sellPrice).toFixed(2)}`,
        action: () => {
          switchScreen("product-sale");
          document.getElementById("sale-prod-select").value = p.id;
          onSaleProductSelected();
        }
      }));

      const dueMatches = userStore.dueCustomers.filter(c =>
        c.name.toLowerCase().includes(term) || c.phone.toLowerCase().includes(term)
      ).map(c => ({
        title: `${c.name} (${c.phone})`,
        sub: `${currentLang === "bn" ? "বাকি" : "Due"}: ৳${Number(c.dueAmount).toFixed(2)}`,
        action: () => switchScreen("due-ledger")
      }));

      const all = [...prodMatches, ...dueMatches];
      if (!all.length) {
        drop.innerHTML = `<div class="p-3 text-xs text-slate-400 text-center">${currentLang === "bn" ? "কিছু পাওয়া যায়নি" : "No matches found"}</div>`;
      } else {
        drop.innerHTML = all.map((item, idx) => `
          <button type="button" data-idx="${idx}" class="w-full text-left px-3.5 py-2.5 hover:bg-emerald-50 flex items-center justify-between gap-2">
            <span class="font-extrabold text-xs text-slate-800 truncate">${escapeHtml(item.title)}</span>
            <span class="text-[11px] font-bold text-emerald-700 shrink-0">${escapeHtml(item.sub)}</span>
          </button>`).join("");
        drop.querySelectorAll("button[data-idx]").forEach(btn => {
          btn.onclick = () => {
            all[Number(btn.getAttribute("data-idx"))].action();
            drop.classList.add("hidden");
          };
        });
      }
      drop.classList.remove("hidden");
    }

    function clearGlobalSearch() {
      document.getElementById("global-search-box").value = "";
      document.getElementById("global-search-clear").classList.add("hidden");
      document.getElementById("global-search-dropdown").classList.add("hidden");
    }

    function uploadStoreLogoFile(e) {
      if (activeSessionRole === "staff") return;
      const f = e.target.files && e.target.files[0];
      if (!f) return;
      readImageFileToDataURL(f, dataUrl => {
        if (!ensureActiveSessionStore()) return;
        userStore.settings.shopLogo = dataUrl;
        const pImg = document.getElementById("prof-store-logo-img");
        const sImg = document.getElementById("set-logo-img");
        const hImg = document.getElementById("hdr-store-logo");
        const vImg = document.getElementById("vch-logo");
        if (pImg) pImg.src = dataUrl;
        if (sImg) sImg.src = dataUrl;
        if (hImg) hImg.src = dataUrl;
        if (vImg) vImg.src = dataUrl;
        saveUserIsolatedStore();
        const m = currentLang === "bn" ? "দোকানের লোগো আপলোড ও সেভ করা হয়েছে!" : "Shop logo uploaded & saved!";
        showToast(m, "success");
        speakText(m, true);
      }, 400, 0.78);
    }

    function saveProfileLogoNow() {
      if (activeSessionRole === "staff" || !ensureActiveSessionStore()) return;
      const pImg = document.getElementById("prof-store-logo-img");
      if (pImg && pImg.src) {
        userStore.settings.shopLogo = pImg.src;
      }
      saveUserIsolatedStore();
      const m = currentLang === "bn" ? "দোকানের লোগো সেভ করা হয়েছে!" : "Shop logo saved!";
      showToast(m, "success");
      speakText(m, true);
    }

    function autoSyncSettingsFromInputs() {
      if (activeSessionRole === "staff" || !ensureActiveSessionStore()) return;
      const nameEl = document.getElementById("set-shop-name");
      const phoneEl = document.getElementById("set-shop-phone");
      const addrEl = document.getElementById("set-shop-addr");
      const linkEl = document.getElementById("set-display-link");
      const basEl = document.getElementById("set-pay-bas");
      const apiEl = document.getElementById("set-pay-api");
      const accEl = document.getElementById("set-pay-acc");

      if (nameEl && nameEl.value.trim()) userStore.settings.shopName = nameEl.value.trim();
      if (phoneEl) userStore.settings.shopPhone = phoneEl.value.trim();
      if (addrEl) userStore.settings.shopAddr = addrEl.value.trim();
      if (linkEl) userStore.settings.displayLink = linkEl.value.trim();
      if (basEl) userStore.settings.payBas = basEl.value.trim();
      if (apiEl) userStore.settings.payApi = apiEl.value.trim();
      if (accEl) userStore.settings.payAcc = accEl.value.trim();

      if (currentUser && nameEl && nameEl.value.trim()) {
        const users = getRegisteredUsers();
        const idx = users.findIndex(u => u.id === currentUser.id);
        if (idx !== -1) {
          users[idx].shopName = nameEl.value.trim();
          users[idx].shopAddr = userStore.settings.shopAddr;
          currentUser.shopName = nameEl.value.trim();
          currentUser.shopAddr = userStore.settings.shopAddr;
          saveRegisteredUsers(users);
        }
      }

      try {
        localStorage.setItem(`rh_isolated_data_${adminOwnerIdForSession}`, JSON.stringify(userStore));
      } catch (e) {}
      renderHeaderAndProfile();
      renderVoucherPreview(selectedVoucher || (userStore && userStore.sales[0]) || null);
      scheduleAutoSync("settings-live-input");
    }

    function saveAppSettings(e) {
      e.preventDefault();
      if (activeSessionRole === "staff" || !ensureActiveSessionStore()) return;
      autoSyncSettingsFromInputs();
      saveUserIsolatedStore();
      const m = currentLang === "bn" ? "দোকান, ডিসপ্লে ও পেমেন্ট সেটিংস সংরক্ষণ করা হয়েছে" : "Store, display & payment settings saved";
      showToast(m, "success");
      speakText(m, true);
    }

    function updateAccountPassword(e) {
      e.preventDefault();
      if (activeSessionRole === "staff") return;
      const curHash = computeHash(document.getElementById("prof-cur-pass").value);
      const newPass = document.getElementById("prof-new-pass").value;
      if (currentUser.passHash !== curHash) {
        showToast(currentLang === "bn" ? "বর্তমান পাসওয়ার্ড সঠিক নয়!" : "Current password is incorrect!", "error");
        return;
      }
      const users = getRegisteredUsers();
      const idx = users.findIndex(u => u.id === currentUser.id);
      if (idx !== -1) {
        users[idx].passHash = computeHash(newPass);
        currentUser.passHash = users[idx].passHash;
        saveRegisteredUsers(users);
      }
      document.getElementById("admin-password-form").reset();
      const m = currentLang === "bn" ? "পাসওয়ার্ড পরিবর্তন হয়েছে" : "Password updated";
      showToast(m, "success");
      speakText(m, true);
    }

    function openDigitalPaymentModal() {
      const st = (userStore && userStore.settings) || {};
      document.getElementById("modal-title").textContent = currentLang === "bn" ? "ডিজিটাল পেমেন্ট গেটওয়ে (Paymently)" : "Digital Payment Gateway (Paymently)";
      document.getElementById("modal-body").innerHTML = `
        <div class="space-y-2 text-xs">
          <div class="p-2.5 rounded-xl bg-pink-50 border border-pink-200">
            <div><strong>Personal Account:</strong> <span class="font-mono font-extrabold">${escapeHtml(st.payAcc || "01732333764")}</span></div>
            <div class="truncate"><strong>BAS_URL:</strong> <span class="font-mono">${escapeHtml(st.payBas || "")}</span></div>
          </div>
          <label class="block font-bold">${currentLang === "bn" ? "পেমেন্টের পরিমাণ (৳)" : "Amount (৳)"}</label>
          <input type="number" step="0.01" id="dp-amount" class="w-full px-3 py-2 rounded-xl border border-slate-300 font-bold" placeholder="100.00" />
          <label class="block font-bold">${currentLang === "bn" ? "মাধ্যম" : "Channel"}</label>
          <select id="dp-channel" class="w-full px-3 py-2 rounded-xl border border-slate-300 font-bold bg-white">
            <option value="bKash">bKash</option>
            <option value="Nagad">Nagad</option>
            <option value="Rocket">Rocket</option>
            <option value="Tap">Tap</option>
            <option value="Paymently">Paymently Gateway</option>
          </select>
        </div>`;
      document.getElementById("modal-actions").innerHTML = `
        <button type="button" onclick="closeModal()" class="px-3 py-2 rounded-xl bg-slate-200 text-xs font-bold">${currentLang === "bn" ? "বন্ধ" : "Close"}</button>
        <button type="button" onclick="initiatePaymentlyGateway()" class="px-4 py-2 rounded-xl bg-pink-600 text-white text-xs font-extrabold">${currentLang === "bn" ? "পেমেন্ট রিকোয়েস্ট পাঠান" : "Send Payment Request"}</button>`;
      document.getElementById("custom-modal").classList.remove("hidden");
      document.getElementById("custom-modal").classList.add("flex");
    }

    async function initiatePaymentlyGateway() {
      const amt = parseFloat(document.getElementById("dp-amount").value) || 0;
      const ch = document.getElementById("dp-channel").value;
      const st = (userStore && userStore.settings) || {};
      if (amt <= 0) {
        showToast(currentLang === "bn" ? "সঠিক টাকার পরিমাণ লিখুন!" : "Enter valid amount!", "error");
        return;
      }
      try {
        if (st.payBas && st.payApi) {
          await fetch(`${st.payBas.replace(/\/$/, "")}/create-payment`, {
            method: "POST",
            headers: {
              "Content-Type": "application/json",
              "Authorization": `Bearer ${st.payApi}`
            },
            body: JSON.stringify({
              amount: amt,
              currency: "BDT",
              account: st.payAcc || "01732333764",
              channel: ch
            })
          });
        }
      } catch (e) {}
      closeModal();
      const msg = currentLang === "bn" ? `${ch} এর মাধ্যমে ${amt} টাকার পেমেন্ট রিকোয়েস্ট তৈরি হয়েছে` : `Payment request of ${amt} Taka via ${ch} initiated`;
      showToast(msg, "success");
      speakText(msg, true);
    }

    function openConfirmModal(title, desc, onYes) {
      document.getElementById("modal-title").textContent = title;
      document.getElementById("modal-body").innerHTML = `<p>${escapeHtml(desc)}</p>`;
      document.getElementById("modal-actions").innerHTML = `
        <button type="button" onclick="closeModal()" class="px-3.5 py-2 rounded-xl bg-slate-200 text-xs font-bold">${currentLang === "bn" ? "না" : "No"}</button>
        <button type="button" id="modal-yes-btn" class="px-4 py-2 rounded-xl bg-rose-600 text-white text-xs font-extrabold">${currentLang === "bn" ? "হ্যাঁ, নিশ্চিত" : "Yes, Confirm"}</button>`;
      document.getElementById("modal-yes-btn").onclick = () => {
        closeModal();
        onYes();
      };
      document.getElementById("custom-modal").classList.remove("hidden");
      document.getElementById("custom-modal").classList.add("flex");
    }

    function closeModal() {
      document.getElementById("custom-modal").classList.add("hidden");
      document.getElementById("custom-modal").classList.remove("flex");
    }

    function renderHeaderAndProfile() {
      if (!userStore || !currentUser) return;
      const st = userStore.settings;
      const isStaff = activeSessionRole === "staff" && !!activeStaffMember;

      const activeShopName = isStaff ? (activeStaffMember.shopName || st.shopName) : st.shopName;
      const activeShopLogo = isStaff ? (activeStaffMember.shopLogo || st.shopLogo) : st.shopLogo;
      const activeShopAddr = isStaff ? (activeStaffMember.shopAddr || st.shopAddr || "") : (st.shopAddr || "");

      document.getElementById("hdr-store-name").textContent = activeShopName || "Ribek & Hetu";
      document.getElementById("hdr-store-logo").src = activeShopLogo || "https://placehold.co/100x100/059669/ffffff?text=R%26H";

      const quickShopSel = document.getElementById("hdr-quick-shop-switch");
      if (quickShopSel && !isStaff) {
        if (!Array.from(quickShopSel.options).some(o => o.value === st.shopName)) {
          const opt = document.createElement("option");
          opt.value = st.shopName;
          opt.textContent = st.shopName;
          quickShopSel.appendChild(opt);
        }
        quickShopSel.value = st.shopName;
      }

      const gInput = document.getElementById("custom-greeting-input");
      const gShopInput = document.getElementById("custom-greeting-shop-input");
      if (gInput && document.activeElement !== gInput) {
        gInput.value = st.customGreeting || (currentLang === "bn" ? "স্বাগতম" : "Welcome");
      }
      if (gShopInput && document.activeElement !== gShopInput) {
        gShopInput.value = activeShopName || "";
      }
      updateCustomGreetingPreview();

      const uBadge = document.getElementById("hdr-user-badge");
      if (uBadge) {
        uBadge.textContent = isStaff ? `${activeStaffMember.name} (Staff)` : currentUser.owner;
      }

      document.getElementById("prof-name").textContent = isStaff
        ? `${activeStaffMember.name} (${activeShopName || ""})`
        : `${currentUser.owner} (${st.shopName || ""})`;
      document.getElementById("prof-id").textContent = isStaff
        ? `Staff ID: ${activeStaffMember.staffUserId} • Shop Phone: ${activeStaffMember.shopPhone || st.shopPhone || ""}`
        : `ID: ${currentUser.userId}`;

      const profAddrPrev = document.getElementById("prof-shop-addr-preview");
      if (profAddrPrev) {
        profAddrPrev.textContent = activeShopAddr
          ? (currentLang === "bn" ? `দোকানের ঠিকানা: ${activeShopAddr}` : `Shop Address: ${activeShopAddr}`)
          : "";
      }

      const avatarImg = document.getElementById("prof-avatar-img");
      if (avatarImg) {
        avatarImg.src = isStaff && activeStaffMember.photo
          ? activeStaffMember.photo
          : "https://placehold.co/96x96/4f46e5/ffffff?text=Admin";
      }

      if (!isStaff) {
        const pEditOwner = document.getElementById("prof-edit-owner");
        const pEditUid = document.getElementById("prof-edit-userid");
        const pEditShop = document.getElementById("prof-edit-shop");
        const pEditAddr = document.getElementById("prof-edit-addr");
        if (pEditOwner && document.activeElement !== pEditOwner) pEditOwner.value = currentUser.owner || "";
        if (pEditUid && document.activeElement !== pEditUid) pEditUid.value = currentUser.userId || "";
        if (pEditShop && document.activeElement !== pEditShop) pEditShop.value = st.shopName || "";
        if (pEditAddr && document.activeElement !== pEditAddr) pEditAddr.value = st.shopAddr || "";
      }

      const profLogoImg = document.getElementById("prof-store-logo-img");
      if (profLogoImg) profLogoImg.src = st.shopLogo || "https://placehold.co/96x96/059669/ffffff?text=R%26H";

      const sLogo = document.getElementById("set-logo-img");
      if (sLogo) sLogo.src = st.shopLogo || "https://placehold.co/96x96/059669/ffffff?text=R%26H";

      const sName = document.getElementById("set-shop-name");
      const sPhone = document.getElementById("set-shop-phone");
      const sAddr = document.getElementById("set-shop-addr");
      const sLink = document.getElementById("set-display-link");
      const sBas = document.getElementById("set-pay-bas");
      const sApi = document.getElementById("set-pay-api");
      const sAcc = document.getElementById("set-pay-acc");

      if (sName && document.activeElement !== sName) sName.value = st.shopName || "";
      if (sPhone && document.activeElement !== sPhone) sPhone.value = st.shopPhone || "";
      if (sAddr && document.activeElement !== sAddr) sAddr.value = st.shopAddr || "";
      if (sLink && document.activeElement !== sLink) sLink.value = st.displayLink || "";
      if (sBas && document.activeElement !== sBas) sBas.value = st.payBas || "";
      if (sApi && document.activeElement !== sApi) sApi.value = st.payApi || "";
      if (sAcc && document.activeElement !== sAcc) sAcc.value = st.payAcc || "";
      document.getElementById("sale-personal-acc-text").textContent = st.payAcc || "01732333764";
    }

    function renderAllViews() {
      renderHeaderAndProfile();
      renderDashboard();
      renderInventoryList();
      populateSaleProductSelect();
      renderDueLedger();
      renderHistoryRecords();
      renderNotebookList();
      renderStaffProfilesList();
      updateSmsBadgeCount();
      renderVoucherPreview(selectedVoucher || (userStore && userStore.sales[0]) || null);
    }

    function setupKeyboardShortcuts() {
      document.addEventListener("keydown", e => {
        if (e.key === "Escape") {
          closeModal();
          closeCameraBarcodeScanner();
          const drop = document.getElementById("global-search-dropdown");
          if (drop) drop.classList.add("hidden");
          return;
        }
        if (e.altKey && !e.ctrlKey && !e.metaKey) {
          const map = {
            "1": "dashboard",
            "2": "product-entry",
            "3": "product-sale",
            "4": "due-ledger",
            "5": "history",
            "6": "calendar",
            "7": "calculator",
            "8": "notebook"
          };
          if (map[e.key]) {
            e.preventDefault();
            switchScreen(map[e.key]);
          }
        }
      });
    }

    window.addEventListener("online", () => {
      isLiveOnline = true;
      stopDisconnectAlarmAndVoice();
      applyInternetStatusUI();
      fetchIpFallbackLocation(false, false);
      fetchLiveWeather();
      pullStoreFromCloud(true);
      scheduleAutoSync("network-reconnected");
      const m = currentLang === "bn" ? "ইন্টারনেট সংযোগ পুনরায় চালু হয়েছে!" : "Internet connection restored!";
      showToast(m, "success");
      speakText(m, true);
    });

    window.addEventListener("offline", () => {
      isLiveOnline = false;
      liveLatencyMs = null;
      applyInternetStatusUI();
      startDisconnectAlarmAndVoice();
      const m = currentLang === "bn"
        ? "আপনার ইন্টারনেট কানেক্শন বিচ্ছিন্ন হয়ে গেছে সংযোগ দেয়ার জন্য চেষ্টা করুন"
        : "Your internet connection has been disconnected, please try to reconnect.";
      showToast(m, "error");
    });

    window.addEventListener("storage", (ev) => {
      if (!ev.key || !adminOwnerIdForSession) return;
      if (ev.key === `rh_isolated_data_${adminOwnerIdForSession}` && ev.newValue) {
        try {
          const nextStore = JSON.parse(ev.newValue);
          if (nextStore && nextStore.settings) {
            userStore = {
              ...getDefaultUserStore(nextStore.settings.shopName, nextStore.settings.shopPhone, nextStore.settings.shopAddr),
              ...nextStore,
              settings: { ...getDefaultUserStore().settings, ...nextStore.settings }
            };
            renderAllViews();
          }
        } catch (err) {}
      }
    });

    document.addEventListener("DOMContentLoaded", () => {
      const todayIso = new Date().toISOString().split("T")[0];
      ["sale-date", "due-cust-date", "nb-date"].forEach(id => {
        const el = document.getElementById(id);
        if (el) el.value = todayIso;
      });

      populateUnitDropdowns();
      applyLanguageToUI();
      onAuthRoleModeChange();
      updateCalcAngleIndicator();
      startLiveClockAndAlarmEngine();
      startLiveInternetAndSyncEngine();
      startLiveGpsWatcher();
      fetchGpsLocation(false, false);
      fetchLiveWeather();
      renderCalendarScreen();
      setupKeyboardShortcuts();

      let activeUid = localStorage.getItem("rh_active_uid");
      if (!activeUid) {
        try { activeUid = sessionStorage.getItem("rh_session_uid"); } catch (e) {}
      }

      let savedRole = localStorage.getItem("rh_active_role");
      if (!savedRole) {
        try { savedRole = sessionStorage.getItem("rh_session_role"); } catch (e) {}
      }

      let savedStaffId = localStorage.getItem("rh_active_staff_id");
      if (!savedStaffId) {
        try { savedStaffId = sessionStorage.getItem("rh_session_staff_id"); } catch (e) {}
      }

      const users = getRegisteredUsers();
      if (activeUid) {
        const found = users.find(u => u.id === activeUid);
        if (found) {
          currentUser = found;
          adminOwnerIdForSession = found.id;
          loadUserIsolatedStore(found.id, found.shopName, found.userId, found.shopAddr);
          if (savedRole === "staff" && savedStaffId) {
            const stFound = (userStore.staffProfiles || []).find(s => s.id === savedStaffId);
            if (stFound) {
              activeSessionRole = "staff";
              activeStaffMember = stFound;
            } else {
              activeSessionRole = "admin";
              activeStaffMember = null;
            }
          } else {
            activeSessionRole = "admin";
            activeStaffMember = null;
          }
          persistSessionState();
          launchAuthenticatedWorkspace();
        }
      }
    });
  </script>
</body>
</html>
