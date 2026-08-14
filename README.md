<!DOCTYPE html>
<html lang="bn">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>VIP Gaming App - Smart Referral & Turnover System</title>
    <style>
        * { margin: 0; padding: 0; box-sizing: border-box; font-family: 'Segoe UI', Arial, sans-serif; }
        body { background: #0f172a; color: #333; display: flex; justify-content: center; align-items: center; min-height: 100vh; padding: 10px; }
        
        .mobile-frame {
            width: 360px; height: 740px; background: #f8fafc; border-radius: 24px;
            box-shadow: 0 15px 35px rgba(0,0,0,0.6); overflow: hidden; position: relative;
            display: flex; flex-direction: column; border: 4px solid #1e293b;
        }

        .screen-content { flex: 1; overflow-y: auto; padding: 16px; padding-bottom: 75px; display: flex; flex-direction: column; }
        .hidden { display: none !important; }

        /* Top Bar */
        .top-user-bar {
            background: #1e293b; color: #fff; padding: 10px 16px; display: flex;
            justify-content: space-between; align-items: center; border-bottom: 2px solid #334155;
            font-size: 11px; font-weight: bold;
        }
        .top-balance { background: #059669; color: #fff; padding: 4px 10px; border-radius: 20px; font-size: 12px; }
        .top-level { background: #f59e0b; color: #000; padding: 2px 8px; border-radius: 10px; font-size: 10px; }

        /* Bottom Navigation Bar */
        .bottom-nav {
            position: absolute; bottom: 0; left: 0; width: 100%; height: 65px;
            background: #1e293b; display: flex; justify-content: space-around; align-items: center;
            border-top: 2px solid #334155; z-index: 100;
        }
        .nav-item { flex: 1; display: flex; flex-direction: column; align-items: center; color: #94a3b8; font-size: 10px; font-weight: bold; cursor: pointer; text-decoration: none; }
        .nav-item.active { color: #38bdf8; }
        .nav-item .icon { font-size: 18px; margin-bottom: 2px; }
        .nav-item.ref-btn { color: #f59e0b; }

        /* Auth Screen */
        .auth-box { text-align: center; margin: auto 0; }
        .auth-title { font-size: 22px; font-weight: bold; color: #1e293b; margin-bottom: 16px; }
        
        .input-group { position: relative; width: 100%; margin-bottom: 10px; }
        .input-box { width: 100%; padding: 11px; padding-right: 35px; border-radius: 8px; border: 1px solid #cbd5e1; font-size: 12px; outline: none; }
        .toggle-password { position: absolute; right: 10px; top: 50%; transform: translateY(-50%); cursor: pointer; font-size: 14px; user-select: none; }

        .btn-primary { width: 100%; background: #2563eb; color: #fff; border: none; padding: 11px; border-radius: 8px; font-weight: bold; cursor: pointer; font-size: 13px; margin-top: 4px; }
        .toggle-auth-mode { color: #2563eb; font-size: 12px; font-weight: bold; cursor: pointer; margin-top: 12px; display: inline-block; text-decoration: underline; }

        /* Profile Card */
        .profile-card {
            background: linear-gradient(135deg, #e2b083 0%, #a8794c 100%);
            border-radius: 16px; padding: 16px; color: #fff; position: relative; margin-bottom: 12px;
        }
        .user-info { display: flex; align-items: center; gap: 12px; }
        .avatar { width: 45px; height: 45px; border-radius: 50%; background: #fff; border: 2px solid #fce7f3; display: flex; justify-content: center; align-items: center; font-size: 20px; }
        .vip-badge { background: #332211; color: #fbbf24; font-size: 11px; padding: 3px 10px; border-radius: 10px; font-weight: bold; }
        .balance-val { font-size: 24px; font-weight: 800; margin: 8px 0 4px; text-shadow: 0 2px 4px rgba(0,0,0,0.2); }
        
        .card-btns { display: flex; gap: 6px; }
        .c-btn { flex: 1; background: rgba(255,255,255,0.9); color: #432818; border: none; padding: 7px; border-radius: 20px; font-size: 10px; font-weight: bold; cursor: pointer; text-align: center; }

        /* Turnover Tracker Box */
        .turnover-box { background: #fff; border-radius: 12px; padding: 12px; margin-bottom: 12px; border-left: 4px solid #ef4444; box-shadow: 0 2px 5px rgba(0,0,0,0.05); }
        .turnover-title { display: flex; justify-content: space-between; font-size: 11px; font-weight: bold; color: #1e293b; }
        .progress-bg { background: #e2e8f0; height: 8px; border-radius: 4px; overflow: hidden; margin-top: 6px; }
        .progress-fill { background: #ef4444; height: 100%; width: 0%; transition: width 0.3s; }

        /* Referral Box */
        .referral-section { background: #fff; border-radius: 14px; padding: 14px; margin-bottom: 12px; text-align: center; border: 1px solid #e2e8f0; }
        .share-grid { display: grid; grid-template-columns: repeat(3, 1fr); gap: 8px; margin-top: 10px; }
        .share-btn { padding: 8px; border-radius: 8px; color: #fff; font-size: 11px; font-weight: bold; cursor: pointer; text-align: center; border: none; }
        .share-whatsapp { background: #25D366; }
        .share-imo { background: #0088cc; }
        .share-copy { background: #475569; }

        /* Games Grid */
        .banner { background: linear-gradient(90deg, #b91c1c, #f59e0b); color: #fff; border-radius: 12px; padding: 10px; text-align: center; font-weight: bold; font-size: 13px; margin-bottom: 12px; }
        .game-grid { display: grid; grid-template-columns: 1fr 1fr; gap: 10px; }
        .game-card { background: #1e293b; color: #fff; border-radius: 12px; padding: 15px; text-align: center; border: 1px solid #334155; }

        .profile-list { background: #fff; border-radius: 12px; padding: 12px; margin-bottom: 12px; }
        .profile-item { display: flex; justify-content: space-between; padding: 10px 0; border-bottom: 1px solid #f1f5f9; font-size: 12px; align-items: center; }
        .profile-item:last-child { border-bottom: none; }

        .back-btn { background: #e2e8f0; color: #334155; border: none; padding: 6px 12px; border-radius: 6px; font-size: 12px; font-weight: bold; margin-bottom: 10px; cursor: pointer; width: fit-content; }
        
        .payment-methods { display: flex; gap: 10px; margin-bottom: 12px; }
        .pay-card { flex: 1; border: 2px solid #cbd5e1; border-radius: 10px; padding: 8px; text-align: center; cursor: pointer; background: #fff; }
        .pay-card.selected { border-color: #2563eb; background: #eff6ff; }
        .pay-logo { width: 35px; height: 35px; object-fit: contain; margin-bottom: 4px; }
        
        .amt-grid { display: grid; grid-template-columns: repeat(3, 1fr); gap: 8px; margin-bottom: 12px; }
        .amt-btn { background: #fff; border: 1px solid #cbd5e1; padding: 8px; border-radius: 6px; font-weight: bold; font-size: 11px; color: #1e293b; cursor: pointer; text-align: center; }
        .amt-btn:hover { background: #2563eb; color: #fff; border-color: #2563eb; }
    </style>
</head>
<body>

<div class="mobile-frame">

    <!-- Header Bar -->
    <div id="topUserBar" class="top-user-bar hidden">
        <div>👤 <span id="topUserName">ইউজার</span> (<span id="topUserId">নম্বর</span>)</div>
        <div style="display: flex; gap: 6px; align-items: center;">
            <span class="top-level" id="topLevelBadge">VIP 1</span>
            <span class="top-balance">৳ <span id="topUserBalance">0.00</span></span>
        </div>
    </div>

    <!-- Main Screens Container -->
    <div class="screen-content">

        <!-- Auth Screen -->
        <div id="authScreen" style="margin: auto 0;">
            <div class="auth-box">
                <div class="auth-title" id="authTitle">👑 VIP Gaming App</div>
                
                <div id="regNameGroup" class="input-group hidden">
                    <input type="text" id="regName" class="input-box" placeholder="আপনার নাম">
                </div>

                <div class="input-group">
                    <input type="text" id="authPhone" class="input-box" placeholder="মোবাইল নম্বর">
                </div>

                <div class="input-group">
                    <input type="password" id="authPass" class="input-box" placeholder="পাসওয়ার্ড">
                    <span class="toggle-password" onclick="togglePasswordVisibility('authPass', this)">👁️</span>
                </div>

                <div id="regRefGroup" class="input-group hidden">
                    <input type="text" id="regRefCode" class="input-box" placeholder="রেফারেল নম্বর/কোড (যদি থাকে)">
                </div>

                <button class="btn-primary" id="authSubmitBtn" onclick="handleAuthSubmit()">লগইন করুন</button>
                <br>
                <span class="toggle-auth-mode" id="toggleAuthModeBtn" onclick="toggleAuthMode()">নতুন একাউন্ট তৈরি করুন (রেজিস্ট্রেশন)</span>
            </div>
        </div>

        <!-- Home Screen -->
        <div id="homePage" class="hidden">
            <div class="profile-card">
                <div class="user-info">
                    <div class="avatar">👤</div>
                    <div>
                        <span class="vip-badge" id="uLevelBadge">👑 VIP 1</span>
                        <div style="font-weight: bold; font-size: 14px; margin-top: 2px;" id="uNameDisplay">ইউজার নেম</div>
                        <div style="font-size: 11px; opacity: 0.9;" id="uPhoneDisplay">017XXXXXXXX</div>
                    </div>
                </div>
                <div class="balance-val">৳ <span id="uBalance">0.00</span></div>
                
                <div class="card-btns">
                    <div class="c-btn" onclick="openDepositScreen()">Deposit</div>
                    <div class="c-btn" onclick="openWithdrawScreen()">Withdrawal</div>
                    <div class="c-btn" onclick="navigateTo('securityScreen')">Security</div>
                </div>
            </div>

            <div class="turnover-box">
                <div class="turnover-title">
                    <span>🔄 উইথড্র টার্নওভার (Turnover)</span>
                    <span>৳ <span id="turnoverDone">0</span> / ৳ <span id="turnoverTarget">0</span></span>
                </div>
                <div class="progress-bg"><div class="progress-fill" id="turnoverProgress"></div></div>
                <small style="font-size: 10px; color: #64748b; display: block; margin-top: 4px;">*বোনাস পেলে বোনাসের সমপরিমাণ টার্নওভার পূরণ করার পরেই উইথড্র করতে পারবেন!</small>
            </div>

            <div class="banner">🔥 POPULAR GAMES LOBBY</div>

            <div class="game-grid">
                <div class="game-card">
                    <div style="font-size: 24px;">🎲</div>
                    <div style="font-size: 13px; font-weight: bold; margin: 4px 0;">লুডু লবি</div>
                    <button onclick="playGame(500)" style="background:#22c55e; color:#fff; border:none; padding:5px 10px; border-radius:4px; font-size:10px; font-weight:bold; cursor:pointer;">৳৫০০ প্লে করুন</button>
                </div>
                <div class="game-card">
                    <div style="font-size: 24px;">🚀</div>
                    <div style="font-size: 13px; font-weight: bold; margin: 4px 0;">Aviator</div>
                    <button onclick="playGame(1000)" style="background:#22c55e; color:#fff; border:none; padding:5px 10px; border-radius:4px; font-size:10px; font-weight:bold; cursor:pointer;">৳১,০০০ প্লে করুন</button>
                </div>
            </div>
        </div>

        <!-- Refer Screen -->
        <div id="referPage" class="hidden">
            <h3 style="margin-bottom: 12px; color: #1e293b;">🎁 রেফার অ্যান্ড আর্ন (৳৩০০ বোনাস)</h3>
            
            <div class="referral-section">
                <div style="font-size: 32px; margin-bottom: 6px;">📢</div>
                <h4 style="color: #1e293b; margin-bottom: 4px;">বন্ধুকে রেফার করে আয় করুন ৳৩০০!</h4>
                <p style="font-size: 11px; color: #64748b; margin-bottom: 12px;">
                    আপনার রেফার লিংকে কেউ অ্যাকাউন্ট খুলে **৳২,০০০ ডিপোজিট** করে **৳২,০০০ টার্নওভার** সম্পূর্ণ করলে আপনি পাবেন **৳৩০০ ইনস্ট্যান্ট বোনাস**।
                </p>

                <div style="background: #f1f5f9; padding: 8px; border-radius: 6px; font-size: 11px; font-weight: bold; color: #2563eb; word-break: break-all; margin-bottom: 10px;" id="referLinkTxt">
                    https://vipgamingapp.com/join?ref=YOURCODE
                </div>

                <div class="share-grid">
                    <button class="share-btn share-whatsapp" onclick="shareToPlatform('whatsapp')">WhatsApp</button>
                    <button class="share-btn share-imo" onclick="shareToPlatform('imo')">IMO / All</button>
                    <button class="share-btn share-copy" onclick="copyReferralLink()">Copy Link</button>
                </div>
            </div>

            <div style="background: #fff; border-radius: 12px; padding: 12px;">
                <h5 style="font-size: 12px; margin-bottom: 6px; color: #1e293b;">📌 বোনাসের নিয়মাবলী:</h5>
                <ul style="font-size: 11px; color: #64748b; padding-left: 16px; line-height: 1.6;">
                    <li>নতুন ব্যবহারকারী কোনো জয়েনিং বোনাস পাবে না।</li>
                    <li>নতুন ইউজারকে সর্বনিম্ন ৳২,০০০ ডিপোজিট করতে হবে এবং ৳২,০০০ টাকার গেমিং টার্নওভার সম্পূর্ণ করতে হবে।</li>
                    <li>টার্নওভার সম্পন্ন হওয়ার পর মূল রেফরার বোনাস হিসেবে ৳৩০০ পাবেন।</li>
                    <li>বোনাস প্রাপ্তির পর সেই ৳৩০০ বোনাসের সমপরিমাণ টার্নওভার সম্পন্ন করলেই টাকা বের/উইথড্র করা যাবে।</li>
                </ul>
            </div>
        </div>

        <!-- Window/Categories Screen -->
        <div id="windowPage" class="hidden">
            <h3 style="margin-bottom: 12px; color: #1e293b;">🪟 গেম উইন্ডো (Categories)</h3>
            <div class="game-grid" style="grid-template-columns: 1fr;">
                <div class="game-card" style="display: flex; align-items: center; justify-content: space-between; padding: 12px;">
                    <div style="display: flex; align-items: center; gap: 10px;">
                        <span style="font-size: 28px;">🎰</span>
                        <div style="text-align: left;">
                            <div style="font-weight: bold; font-size: 14px;">Slots Lobby</div>
                            <small style="color: #94a3b8; font-size: 10px;">১০০+ জনপ্রিয় স্লট গেম</small>
                        </div>
                    </div>
                    <button onclick="playGame(200)" style="background:#2563eb; color:#fff; border:none; padding:6px 12px; border-radius:6px; font-size:11px; font-weight:bold; cursor:pointer;">প্লে করুন</button>
                </div>
            </div>
        </div>

        <!-- Profile Screen -->
        <div id="profilePage" class="hidden">
            <h3 style="margin-bottom: 12px; color: #1e293b;">👤 মাই প্রোফাইল</h3>
            <div class="profile-list">
                <div class="profile-item"><span style="color: #64748b;">ইউজার নেম:</span><b id="profUserName">-</b></div>
                <div class="profile-item"><span style="color: #64748b;">মোবাইল নম্বর:</span><b id="profUserPhone">-</b></div>
                <div class="profile-item"><span style="color: #64748b;">রেফার কোড:</span><b id="profRefCode">-</b></div>
                <div class="profile-item"><span style="color: #64748b;">বর্তমান টার্নওভার:</span><b style="color:#ef4444;" id="profTurnoverStat">৳০ / ৳০</b></div>
                <div class="profile-item"><span style="color: #64748b;">উইথড্র নম্বর:</span><b id="profSavedNumber" style="color:#2563eb;">সেটআপ করা নেই</b></div>
            </div>
            <button class="btn-primary" style="margin-bottom: 8px;" onclick="navigateTo('securityScreen')">🔒 সিকিউরিটি সেটিং</button>
            <button class="btn-primary" style="background: #ef4444;" onclick="logout()">অ্যাকাউন্ট লগআউট করুন</button>
        </div>

        <!-- Security Screen -->
        <div id="securityScreen" class="hidden">
            <button class="back-btn" onclick="navigateTo('homePage')">❮ Back</button>
            <h3 style="margin-bottom: 12px; color: #1e293b;">🔒 একাউন্ট সিকিউরিটি সেটিং</h3>

            <div id="secLoginBox">
                <p style="font-size: 11px; color: #64748b; margin-bottom: 10px;">লগইন পাসওয়ার্ড পরিবর্তন করুন:</p>
                <div class="input-group">
                    <input type="password" id="oldLoginPass" class="input-box" placeholder="পুরাতন (বর্তমান) পাসওয়ার্ড">
                    <span class="toggle-password" onclick="togglePasswordVisibility('oldLoginPass', this)">👁️</span>
                </div>
                <div class="input-group">
                    <input type="password" id="newLoginPass" class="input-box" placeholder="নতুন পাসওয়ার্ড দিন">
                    <span class="toggle-password" onclick="togglePasswordVisibility('newLoginPass', this)">👁️</span>
                </div>
                <button class="btn-primary" onclick="changeLoginPassword()">লগইন পাসওয়ার্ড আপডেট করুন</button>
            </div>
        </div>

        <!-- Deposit Screen -->
        <div id="depScreen" class="hidden">
            <button class="back-btn" onclick="navigateTo('homePage')">❮ Back</button>
            <h3 style="margin-bottom: 8px;">টাকা ডিপোজিট করুন</h3>
            <p style="font-size: 11px; color: #64748b; margin-bottom: 10px;">পেমেন্ট মেথড বেছে নিন:</p>

            <div class="payment-methods">
                <div class="pay-card selected" id="payBkash" onclick="selectPayMethod('bKash')">
                    <svg class="pay-logo" viewBox="0 0 100 100"><rect width="100" height="100" fill="#e2136e" rx="15"/><path d="M20,30 L50,75 L80,30 L50,50 Z" fill="#fff"/></svg>
                    <div style="font-size: 11px; font-weight: bold;">bKash</div>
                </div>
                <div class="pay-card" id="payNagad" onclick="selectPayMethod('Nagad')">
                    <svg class="pay-logo" viewBox="0 0 100 100"><rect width="100" height="100" fill="#f7921e" rx="15"/><circle cx="50" cy="50" r="30" fill="#fff"/><circle cx="50" cy="50" r="18" fill="#f7921e"/></svg>
                    <div style="font-size: 11px; font-weight: bold;">Nagad</div>
                </div>
                <div class="pay-card" id="payRocket" onclick="selectPayMethod('Rocket')">
                    <svg class="pay-logo" viewBox="0 0 100 100"><rect width="100" height="100" fill="#8c3494" rx="15"/><polygon points="50,15 85,85 15,85" fill="#fff"/></svg>
                    <div style="font-size: 11px; font-weight: bold;">Rocket</div>
                </div>
            </div>

            <p style="font-size: 11px; color: #64748b; margin-bottom: 6px;">পরিমাণ সিলেক্ট করুন (১০০ - ২৫,০০০ টাকা):</p>
            <div class="amt-grid">
                <div class="amt-btn" onclick="setDepositAmt(100)">৳ ১০০</div>
                <div class="amt-btn" onclick="setDepositAmt(500)">৳ ৫০০</div>
                <div class="amt-btn" onclick="setDepositAmt(1000)">৳ ১,০০০</div>
                <div class="amt-btn" onclick="setDepositAmt(2000)">🔥 ৳ ২,০০০</div>
                <div class="amt-btn" onclick="setDepositAmt(5000)">৳ ৫,০০০</div>
                <div class="amt-btn" onclick="setDepositAmt(25000)">৳ ২৫,০০০</div>
            </div>

            <input type="number" id="depAmountInput" class="input-box" style="margin-bottom:10px;" placeholder="পরিমাণ (১০০ - ২৫০০০)">
            <input type="text" id="depTrxInput" class="input-box" style="margin-bottom:10px;" placeholder="TrxID (ট্রানজেকশন আইডি)">
            <button class="btn-primary" onclick="processDeposit()">ডিপোজিট জমা দিন</button>
        </div>

        <!-- Withdraw Account Setup Screen -->
        <div id="withdrawAccountSetupScreen" class="hidden">
            <button class="back-btn" onclick="navigateTo('homePage')">❮ Back</button>
            <h3 style="margin-bottom: 6px; color: #e11d48;">📱 উইথড্র নাম্বার যোগ করুন</h3>
            <p style="font-size: 12px; color: #64748b; margin-bottom: 12px;">টাকা তোলার জন্য বিকাশ/নগদ/রকেট নাম্বার সেভ করুন:</p>

            <select id="accMethod" class="input-box" style="margin-bottom: 10px;">
                <option value="bKash">bKash (বিকাশ)</option>
                <option value="Nagad">Nagad (নগদ)</option>
                <option value="Rocket">Rocket (রকেট)</option>
            </select>
            <input type="text" id="accNumberInput" class="input-box" style="margin-bottom: 10px;" placeholder="আপনার মোবাইল নাম্বার দিন">
            <button class="btn-primary" style="background:#059669;" onclick="saveWithdrawAccount()">নাম্বার সেভ করুন</button>
        </div>

        <!-- Withdraw Screen -->
        <div id="withScreen" class="hidden">
            <button class="back-btn" onclick="navigateTo('homePage')">❮ Back</button>
            <h3 style="margin-bottom: 10px;">টাকা উইথড্র করুন</h3>
            
            <div style="background: #e2e8f0; padding: 10px; border-radius: 8px; margin-bottom: 12px; font-size: 12px;">
                <span>সেভ করা অ্যাকাউন্ট:</span><br>
                <b id="savedAccDetails" style="color: #2563eb; font-size: 13px;">-</b>
                <span style="color: #e11d48; cursor: pointer; float: right; font-weight: bold;" onclick="navigateTo('withdrawAccountSetupScreen')">[পরিবর্তন]</span>
            </div>

            <input type="number" id="withAmountInput" class="input-box" style="margin-bottom:10px;" placeholder="পরিমাণ (২০০ - ২৫,০০০)">
            
            <div class="input-group">
                <input type="password" id="withPinInput" class="input-box" placeholder="আপনার পাসওয়ার্ড">
                <span class="toggle-password" onclick="togglePasswordVisibility('withPinInput', this)">👁️</span>
            </div>

            <button class="btn-primary" style="background:#d97706;" onclick="processWithdraw()">উইথড্র রিকোয়েস্ট দিন</button>
        </div>

    </div>

    <!-- Bottom Navigation Bar -->
    <div id="bottomNav" class="bottom-nav hidden">
        <div class="nav-item active" id="navHome" onclick="navigateTo('homePage')">
            <span class="icon">🏠</span>
            <span>হোমপেজ</span>
        </div>
        <div class="nav-item ref-btn" id="navRefer" onclick="navigateTo('referPage')">
            <span class="icon">🎁</span>
            <span>রেফার</span>
        </div>
        <div class="nav-item" id="navWindow" onclick="navigateTo('windowPage')">
            <span class="icon">🪟</span>
            <span>উইন্ডো</span>
        </div>
        <div class="nav-item" id="navProfile" onclick="navigateTo('profilePage')">
            <span class="icon">👤</span>
            <span>প্রোফাইল</span>
        </div>
    </div>

</div>

<!-- Firebase SDKs -->
<script src="https://www.gstatic.com/firebasejs/9.22.1/firebase-app-compat.js"></script>
<script src="https://www.gstatic.com/firebasejs/9.22.1/firebase-database-compat.js"></script>

<script>
    // Firebase Configuration
    const firebaseConfig = {
        apiKey: "AIzaSyDlmi7Gs0G7MnOtQXBmpI8TxBs1CfjhlUo",
        authDomain: "g6bdwin.firebaseapp.com",
        databaseURL: "https://g6bdwin-default-rtdb.asia-southeast1.firebasedatabase.app",
        projectId: "g6bdwin",
        storageBucket: "g6bdwin.firebasestorage.app",
        messagingSenderId: "872964112407",
        appId: "1:872964112407:android:64a5e002b09872688a1c52"
    };

    // Firebase Initialization
    firebase.initializeApp(firebaseConfig);
    const database = firebase.database();

    let currentUser = null;
    let isRegisterMode = false;
    let selectedPaymentMethod = 'bKash';

    function togglePasswordVisibility(inputId, eyeIcon) {
        let input = document.getElementById(inputId);
        if (input.type === "password") {
            input.type = "text";
            eyeIcon.innerText = "🙈";
        } else {
            input.type = "password";
            eyeIcon.innerText = "👁️";
        }
    }

    function toggleAuthMode() {
        isRegisterMode = !isRegisterMode;
        if(isRegisterMode) {
            document.getElementById('authTitle').innerText = "📝 New Registration";
            document.getElementById('regNameGroup').classList.remove('hidden');
            document.getElementById('regRefGroup').classList.remove('hidden');
            document.getElementById('authSubmitBtn').innerText = "রেজিস্ট্রেশন করুন";
            document.getElementById('toggleAuthModeBtn').innerText = "পুরাতন একাউন্ট? লগইন করুন";
        } else {
            document.getElementById('authTitle').innerText = "👑 VIP Gaming Login";
            document.getElementById('regNameGroup').classList.add('hidden');
            document.getElementById('regRefGroup').classList.add('hidden');
            document.getElementById('authSubmitBtn').innerText = "লগইন করুন";
            document.getElementById('toggleAuthModeBtn').innerText = "নতুন একাউন্ট তৈরি করুন (রেজিস্ট্রেশন)";
        }
    }

    function saveUserData() {
        if (currentUser && currentUser.phone) {
            database.ref('users/' + currentUser.phone).set(currentUser);
        }
    }

    function handleAuthSubmit() {
        let phone = document.getElementById('authPhone').value.trim();
        let pass = document.getElementById('authPass').value.trim();

        if(!phone || !pass) {
            alert('মোবাইল নম্বর ও পাসওয়ার্ড প্রদান করুন!');
            return;
        }

        let userRef = database.ref('users/' + phone);

        if(isRegisterMode) {
            let name = document.getElementById('regName').value.trim();
            let refCodeUsed = document.getElementById('regRefCode').value.trim();

            if(!name) {
                alert('আপনার নাম লিখুন!');
                return;
            }

            userRef.once('value', snapshot => {
                if(snapshot.exists()) {
                    alert('এই নম্বরটি দিয়ে ইতিমধ্যেই একাউন্ট খোলা হয়েছে! লগইন করুন।');
                } else {
                    let myRefCode = "REF" + phone.slice(-6);
                    currentUser = {
                        name: name,
                        phone: phone,
                        pass: pass,
                        referredBy: refCodeUsed || null,
                        referralRewardClaimed: false,
                        withdrawAccount: null,
                        balance: 0,
                        refCode: myRefCode,
                        totalDeposited: 0,
                        turnoverDone: 0,
                        turnoverTarget: 0
                    };

                    saveUserData();
                    alert('🎉 অ্যাকাউন্ট রেজিস্ট্রেশন সফল হয়েছে!');
                    completeLogin();
                }
            });

        } else {
            userRef.once('value', snapshot => {
                if(snapshot.exists()) {
                    let data = snapshot.val();
                    if(data.pass === pass) {
                        currentUser = data;
                        alert('🎉 সফলভাবে লগইন করেছেন!');
                        completeLogin();
                    } else {
                        alert('❌ ভুল পাসওয়ার্ড দিয়েছেন!');
                    }
                } else {
                    alert('❌ কোনো অ্যাকাউন্ট পাওয়া যায়নি! আগে রেজিস্ট্রেশন করুন।');
                }
            });
        }
    }

    function completeLogin() {
        document.getElementById('authScreen').classList.add('hidden');
        document.getElementById('topUserBar').classList.remove('hidden');
        document.getElementById('bottomNav').classList.remove('hidden');
        navigateTo('homePage');
    }

    function navigateTo(pageId) {
        const pages = ['homePage', 'referPage', 'windowPage', 'profilePage', 'securityScreen', 'depScreen', 'withScreen', 'withdrawAccountSetupScreen'];
        pages.forEach(p => document.getElementById(p).classList.add('hidden'));

        document.getElementById(pageId).classList.remove('hidden');

        document.querySelectorAll('.nav-item').forEach(el => el.classList.remove('active'));
        if(pageId === 'homePage') document.getElementById('navHome').classList.add('active');
        if(pageId === 'referPage') document.getElementById('navRefer').classList.add('active');
        if(pageId === 'windowPage') document.getElementById('navWindow').classList.add('active');
        if(pageId === 'profilePage') document.getElementById('navProfile').classList.add('active');

        updateUI();
    }

    function getReferralLink() {
        return `https://vipgamingapp.com/join?ref=${currentUser ? currentUser.refCode : ''}`;
    }

    function shareToPlatform(platform) {
        let link = getReferralLink();
        let text = `👑 VIP Gaming App-এ যোগ দিন এবং গেম খেলে টাকা জিতুন!\nরেফার লিংক: ${link}`;

        if(platform === 'whatsapp') {
            window.open(`https://api.whatsapp.com/send?text=${encodeURIComponent(text)}`, '_blank');
        } else {
            if (navigator.share) {
                navigator.share({
                    title: 'VIP Gaming App',
                    text: text,
                    url: link,
                }).catch(() => {});
            } else {
                copyReferralLink();
            }
        }
    }

    function copyReferralLink() {
        let link = getReferralLink();
        navigator.clipboard.writeText(link);
        alert(`🎉 আপনার রেফার লিঙ্ক কপি করা হয়েছে!\n\n${link}`);
    }

    function playGame(bet) {
        if(currentUser.balance < bet) {
            alert('পর্যাপ্ত ব্যালেন্স নেই!');
            return;
        }
        currentUser.balance -= bet;
        currentUser.turnoverDone += bet;

        let isWin = Math.random() > 0.4; 
        if(isWin) {
            let winAmt = bet * 1.9;
            currentUser.balance += winAmt;
            alert('🎉 আপনি জিতেছেন! ৳' + winAmt.toFixed(2) + ' ব্যালেন্সে যোগ হয়েছে।');
        } else {
            alert('❌ এই রাউন্ডে হেরে গেছেন!');
        }

        checkReferralRewardTrigger();
        saveUserData();
        updateUI();
    }

    function checkReferralRewardTrigger() {
        if(currentUser.referredBy && !currentUser.referralRewardClaimed) {
            if(currentUser.totalDeposited >= 2000 && currentUser.turnoverDone >= 2000) {
                database.ref('users').once('value', snapshot => {
                    let users = snapshot.val();
                    for(let ph in users) {
                        if(users[ph].refCode === currentUser.referredBy || ph === currentUser.referredBy) {
                            let referrer = users[ph];
                            referrer.balance += 300;
                            referrer.turnoverTarget += 300;
                            database.ref('users/' + ph).set(referrer);

                            currentUser.referralRewardClaimed = true;
                            saveUserData();
                            alert(`🎉 আপনার ৳২০০০ ডিপোজিট ও টার্নওভার সফল হওয়ায় আপনাকে রেফার করা ব্যবহারকারী ৳৩০০ বোনাস পেয়েছেন!`);
                            break;
                        }
                    }
                });
            }
        }
    }

    function selectPayMethod(method) {
        selectedPaymentMethod = method;
        document.querySelectorAll('.pay-card').forEach(c => c.classList.remove('selected'));
        if(method === 'bKash') document.getElementById('payBkash').classList.add('selected');
        if(method === 'Nagad') document.getElementById('payNagad').classList.add('selected');
        if(method === 'Rocket') document.getElementById('payRocket').classList.add('selected');
    }

    function setDepositAmt(amt) {
        document.getElementById('depAmountInput').value = amt;
    }

    function processDeposit() {
        let amt = parseFloat(document.getElementById('depAmountInput').value);
        let trx = document.getElementById('depTrxInput').value.trim();

        if(!amt || amt < 100 || amt > 25000) {
            alert('ডিপোজিটের পরিমাণ ১০০ থেকে ২৫,০০০ টাকার মধ্যে হতে হবে!');
            return;
        }

        if(!trx) {
            alert('TrxID প্রদান করুন!');
            return;
        }

        currentUser.balance += amt;
        currentUser.totalDeposited += amt;

        checkReferralRewardTrigger();
        saveUserData();
        updateUI();
        alert(`🎉 ${selectedPaymentMethod}-এর মাধ্যমে ৳${amt} ডিপোজিট সফল হয়েছে!`);
        document.getElementById('depAmountInput').value = '';
        document.getElementById('depTrxInput').value = '';
        navigateTo('homePage');
    }

    function openWithdrawScreen() {
        if(!currentUser.withdrawAccount) {
            alert('টাকা তোলার আগে আপনার বিকাশ/নগদ/রকেট নম্বর সেভ করুন!');
            navigateTo('withdrawAccountSetupScreen');
        } else {
            navigateTo('withScreen');
        }
    }

    function saveWithdrawAccount() {
        let m = document.getElementById('accMethod').value;
        let num = document.getElementById('accNumberInput').value.trim();

        if(!num || num.length < 11) {
            alert('সঠিক মোবাইল নম্বর দিন!');
            return;
        }

        currentUser.withdrawAccount = { method: m, number: num };
        saveUserData();
        alert('🎉 উইথড্র অ্যাকাউন্ট সেভ করা হয়েছে!');
        navigateTo('withScreen');
    }

    function processWithdraw() {
        let amt = parseFloat(document.getElementById('withAmountInput').value);
        let pin = document.getElementById('withPinInput').value.trim();

        if(!amt || amt < 200 || amt > 25000) {
            alert('২০০ থেকে ২৫,০০০ টাকার মধ্যে উইথড্র করুন!');
            return;
        }

        if(amt > currentUser.balance) {
            alert('পর্যাপ্ত ব্যালেন্স নেই!');
            return;
        }

        if(currentUser.turnoverDone < currentUser.turnoverTarget) {
            let remaining = currentUser.turnoverTarget - currentUser.turnoverDone;
            alert(`❌ টার্নওভার অসম্পূর্ণ! টাকা বের করার আগে আরও ৳${remaining} গেম খেলতে হবে।`);
            return;
        }

        if(pin !== currentUser.pass) {
            alert('❌ ভুল পাসওয়ার্ড দিয়েছেন!');
            return;
        }

        currentUser.balance -= amt;
        saveUserData();
        updateUI();
        alert(`🎉 ${currentUser.withdrawAccount.number} নম্বরে ৳${amt} উইথড্র রিকোয়েস্ট সফল হয়েছে!`);
        document.getElementById('withAmountInput').value = '';
        document.getElementById('withPinInput').value = '';
        navigateTo('homePage');
    }

    function changeLoginPassword() {
        let oldP = document.getElementById('oldLoginPass').value.trim();
        let newP = document.getElementById('newLoginPass').value.trim();

        if(oldP !== currentUser.pass) {
            alert('❌ ভুল পুরাতন পাসওয়ার্ড দিয়েছেন!');
            return;
        }

        if(!newP || newP.length < 4) {
            alert('নতুন পাসওয়ার্ড সর্বনিম্ন ৪ ডিজিটের দিন!');
            return;
        }

        currentUser.pass = newP;
        saveUserData();
        alert('🎉 পাসওয়ার্ড সফলভাবে আপডেট করা হয়েছে!');
        document.getElementById('oldLoginPass').value = '';
        document.getElementById('newLoginPass').value = '';
        navigateTo('homePage');
    }

    function updateUI() {
        if(!currentUser) return;

        document.getElementById('topUserName').innerText = currentUser.name;
        document.getElementById('topUserId').innerText = currentUser.phone;
        document.getElementById('topUserBalance').innerText = currentUser.balance.toFixed(2);
        
        document.getElementById('uNameDisplay').innerText = currentUser.name;
        document.getElementById('uPhoneDisplay').innerText = currentUser.phone;
        document.getElementById('uBalance').innerText = currentUser.balance.toFixed(2);

        document.getElementById('profUserName').innerText = currentUser.name;
        document.getElementById('profUserPhone').innerText = currentUser.phone;
        document.getElementById('profRefCode').innerText = currentUser.refCode;

        document.getElementById('turnoverDone').innerText = currentUser.turnoverDone;
        document.getElementById('turnoverTarget').innerText = currentUser.turnoverTarget;
        
        let pct = currentUser.turnoverTarget > 0 ? Math.min(100, (currentUser.turnoverDone / currentUser.turnoverTarget) * 100) : 100;
        document.getElementById('turnoverProgress').style.width = pct + '%';
        document.getElementById('profTurnoverStat').innerText = `৳${currentUser.turnoverDone} / ৳${currentUser.turnoverTarget}`;

        document.getElementById('referLinkTxt').innerText = getReferralLink();

        if(currentUser.withdrawAccount) {
            let str = `${currentUser.withdrawAccount.method} (${currentUser.withdrawAccount.number})`;
            document.getElementById('profSavedNumber').innerText = str;
            document.getElementById('savedAccDetails').innerText = str;
        } else {
            document.getElementById('profSavedNumber').innerText = "সেটআপ করা নেই";
            document.getElementById('savedAccDetails').innerText = "কোনো অ্যাকাউন্ট যোগ করা নেই";
        }
    }

    function openDepositScreen() { navigateTo('depScreen'); }
    
    function logout() { 
        currentUser = null;
        document.getElementById('topUserBar').classList.add('hidden');
        document.getElementById('bottomNav').classList.add('hidden');
        const pages = ['homePage', 'referPage', 'windowPage', 'profilePage', 'securityScreen', 'depScreen', 'withScreen', 'withdrawAccountSetupScreen'];
        pages.forEach(p => document.getElementById(p).classList.add('hidden'));
        document.getElementById('authScreen').classList.remove('hidden');
    }
</script>

</body>
</html>
