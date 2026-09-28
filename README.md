<!DOCTYPE html>
<html lang="vi">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Quán Bánh Tráng Trộn Tạo Nghiệp</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <link href="https://fonts.googleapis.com/css2?family=Lexend:wght@400;600;700;800&display=swap" rel="stylesheet">
    <style>
        body {
            font-family: 'Lexend', sans-serif;
            user-select: none;
            -webkit-user-select: none;
            touch-action: manipulation;
        }
        .custom-scrollbar::-webkit-scrollbar {
            width: 6px;
            height: 6px;
        }
        .custom-scrollbar::-webkit-scrollbar-track {
            background: #f1f1f1;
            border-radius: 10px;
        }
        .custom-scrollbar::-webkit-scrollbar-thumb {
            background: #f87171;
            border-radius: 10px;
        }
        @keyframes float {
            0% { transform: translateY(0px); }
            50% { transform: translateY(-6px); }
            100% { transform: translateY(0px); }
        }
        .animate-float {
            animation: float 2.5s ease-in-out infinite;
        }
        .btn-press {
            transition: all 0.1s ease;
        }
        .btn-press:active {
            transform: scale(0.95);
        }
    </style>
</head>
<body class="bg-amber-50 text-slate-800 flex justify-center min-h-screen">

    <!-- Container cho toàn bộ ứng dụng game (Tối ưu giao diện Mobile/Tablet) -->
    <div id="app" class="w-full max-w-md bg-amber-100 min-h-screen flex flex-col shadow-2xl relative overflow-hidden border-x border-amber-200">
        
        <!-- HEADER BAR: Ngày, Tiền, Đánh giá & Level/XP -->
        <header class="bg-gradient-to-r from-red-500 to-amber-600 text-white p-3 shadow-md relative z-10">
            <div class="flex justify-between items-center mb-1">
                <div class="flex items-center gap-1.5">
                    <span class="bg-red-700/60 text-xs px-2 py-0.5 rounded-full font-bold border border-red-300/30" id="header-day">Ngày 1</span>
                    <span class="text-xs text-amber-200" id="header-time">Mở cửa</span>
                </div>
                <div class="text-right">
                    <div class="text-xs text-amber-100 uppercase tracking-wider">Két Tiền</div>
                    <div class="text-lg font-black text-yellow-300" id="header-money">50.000đ</div>
                </div>
            </div>

            <div class="flex justify-between items-end mt-2 pt-2 border-t border-red-400/50">
                <div class="flex items-center gap-1.5">
                    <span class="text-yellow-300 text-sm">★</span>
                    <span class="text-xs font-bold" id="header-rating">5.0</span>
                    <span class="text-[10px] text-red-100" id="header-reviews-count">(0 đánh giá)</span>
                </div>
                <div class="w-1/2">
                    <div class="flex justify-between text-[10px] font-bold mb-0.5">
                        <span id="header-level" class="text-yellow-200">Cấp 1: Tập Sự</span>
                        <span id="header-xp-text" class="text-amber-100">0/100 XP</span>
                    </div>
                    <div class="w-full bg-red-800/60 h-2 rounded-full overflow-hidden p-0.5">
                        <div id="header-xp-bar" class="bg-yellow-400 h-full rounded-full transition-all duration-300" style="width: 0%"></div>
                    </div>
                </div>
            </div>
        </header>

        <!-- TÊN QUÁN BANNER -->
        <div class="bg-gradient-to-r from-red-600 via-orange-500 to-red-600 text-white p-3 text-center relative shadow-inner">
            <div class="inline-block relative">
                <h1 id="shop-name" class="text-xl font-extrabold tracking-tight drop-shadow">Quán Mì & Bánh Tráng Bã Táo</h1>
                <button onclick="openChangeNameModal()" class="absolute -right-6 top-1 text-xs text-amber-200 hover:text-white bg-black/20 p-1 rounded-full">✏️</button>
            </div>
            <p class="text-[11px] text-amber-100 italic mt-0.5">"Đặc sản cay xé lưỡi - Trộn ngon tạo nghiệp!"</p>
        </div>

        <!-- THANH CÔNG CỤ MENU NHANH -->
        <div class="grid grid-cols-6 gap-1 p-2 bg-amber-200/80 border-b border-amber-300 text-[11px] text-center font-bold">
            <button onclick="openTab('kho')" class="btn-press bg-white p-1.5 rounded-lg shadow-sm border border-amber-300 flex flex-col items-center">
                <span>📦</span> <span class="text-slate-700">Kho</span>
            </button>
            <button onclick="openTab('giaban')" class="btn-press bg-white p-1.5 rounded-lg shadow-sm border border-amber-300 flex flex-col items-center">
                <span>🏷️</span> <span class="text-slate-700">Giá bán</span>
            </button>
            <button onclick="openTab('nangcap')" class="btn-press bg-white p-1.5 rounded-lg shadow-sm border border-amber-300 flex flex-col items-center">
                <span>🚀</span> <span class="text-slate-700">Nâng cấp</span>
            </button>
            <button onclick="openTab('trangtri')" class="btn-press bg-white p-1.5 rounded-lg shadow-sm border border-amber-300 flex flex-col items-center">
                <span>🏮</span> <span class="text-slate-700">Trang trí</span>
            </button>
            <button onclick="openTab('danhgia')" class="btn-press bg-white p-1.5 rounded-lg shadow-sm border border-amber-300 flex flex-col items-center relative">
                <span>⭐</span> <span class="text-slate-700">Đánh giá</span>
                <span id="badge-reviews" class="hidden absolute -top-1 -right-1 bg-red-500 text-white text-[9px] w-4 h-4 rounded-full flex items-center justify-center">0</span>
            </button>
            <button onclick="openTab('sosach')" class="btn-press bg-white p-1.5 rounded-lg shadow-sm border border-amber-300 flex flex-col items-center">
                <span>📖</span> <span class="text-slate-700">Sổ sách</span>
            </button>
        </div>

        <!-- NỘI DUNG CHÍNH (KHU VỰC CHƠI GAME & KHÁCH HÀNG) -->
        <main class="flex-1 overflow-y-auto p-3 flex flex-col gap-3 custom-scrollbar">

            <!-- THÔNG BÁO & NHIỆM VỤ NGẮN -->
            <div id="quest-banner" class="bg-amber-50 rounded-xl p-2.5 border border-amber-300 shadow-sm flex items-center justify-between">
                <div class="flex items-center gap-2">
                    <span class="text-xl">📜</span>
                    <div>
                        <div class="text-[10px] uppercase tracking-wider font-bold text-amber-700">Nhiệm vụ hôm nay</div>
                        <div id="quest-desc" class="text-xs font-semibold text-slate-800">Phục vụ 3 khách 5 sao liên tiếp</div>
                    </div>
                </div>
                <div class="text-right">
                    <span id="quest-progress" class="text-xs font-extrabold text-red-600 bg-red-100 px-2 py-1 rounded-md">0/3</span>
                </div>
            </div>

            <!-- SỰ KIỆN MINIGAME: ĐI CHỢ TRẢ GIÁ -->
            <div class="bg-gradient-to-r from-orange-100 to-amber-100 border border-orange-300 rounded-xl p-2.5 flex items-center justify-between shadow-sm">
                <div class="flex items-center gap-2">
                    <div class="w-9 h-9 bg-orange-200 rounded-full flex items-center justify-center text-lg shadow-inner">👵</div>
                    <div>
                        <div class="text-xs font-bold text-slate-800">Đi chợ trả giá cô Hai</div>
                        <div class="text-[10px] text-slate-600">Bớt tiền nhập hàng tới 20% hôm nay!</div>
                    </div>
                </div>
                <button onclick="startHaggleMiniGame()" class="btn-press bg-orange-500 hover:bg-orange-600 text-white text-xs font-bold px-3 py-1.5 rounded-lg shadow">
                    Trả giá
                </button>
            </div>

            <!-- KHU VỰC KHÁCH HÀNG & YÊU CẦU -->
            <div class="bg-white rounded-2xl p-3 border-2 border-amber-300 shadow-md relative min-h-[160px] flex flex-col justify-between">
                
                <!-- Khách hàng xuất hiện -->
                <div id="customer-container" class="flex items-start gap-3">
                    <div class="relative">
                        <div id="customer-avatar" class="text-4xl p-2 bg-amber-100 rounded-2xl border border-amber-200 shadow-sm animate-float">👧</div>
                        <div id="customer-mood" class="absolute -top-1 -right-1 text-sm bg-white rounded-full shadow p-0.5">😊</div>
                    </div>

                    <div class="flex-1">
                        <div class="flex justify-between items-center">
                            <span id="customer-name" class="text-xs font-bold text-slate-700">Em Học Sinh Ngoan</span>
                            <span id="customer-patience" class="text-[10px] font-bold text-emerald-600 bg-emerald-50 px-1.5 py-0.5 rounded border border-emerald-200">100% Vui</span>
                        </div>
                        
                        <!-- Thanh kiên nhẫn -->
                        <div class="w-full bg-slate-100 h-1.5 rounded-full overflow-hidden mt-1 mb-2">
                            <div id="patience-bar" class="bg-emerald-500 h-full rounded-full transition-all duration-200" style="width: 100%"></div>
                        </div>

                        <!-- Bong bóng gọi món -->
                        <div class="bg-amber-50 p-2 rounded-xl border border-amber-200 relative text-xs">
                            <div class="font-bold text-slate-700 mb-1 text-[11px] flex items-center gap-1">
                                <span>🥣</span> Món muốn gọi:
                            </div>
                            <div id="customer-order-list" class="flex flex-wrap gap-1">
                                <!-- Order items generated by JS -->
                            </div>
                        </div>
                    </div>
                </div>

                <!-- Khung báo trạng thái khi không có khách -->
                <div id="no-customer-msg" class="hidden absolute inset-0 bg-white/90 rounded-2xl flex flex-col items-center justify-center p-4 text-center z-10">
                    <span class="text-3xl mb-1">🧹</span>
                    <p class="text-xs font-bold text-slate-600">Đang chờ khách hàng tiếp theo...</p>
                    <button onclick="spawnCustomer()" class="mt-2 text-xs bg-red-500 hover:bg-red-600 text-white font-bold px-3 py-1.5 rounded-lg shadow btn-press">
                        🔔 Kéo chuông gọi khách
                    </button>
                </div>
            </div>

            <!-- CHẬU TRỘN BÁNH TRÁNG (BÁT NGUYÊN LIỆU) -->
            <div class="bg-amber-50 rounded-2xl p-3 border-2 border-amber-300 shadow-md">
                <div class="flex justify-between items-center mb-2">
                    <span class="text-xs font-extrabold text-amber-900 flex items-center gap-1">
                        <span>🍲</span> Tô Bánh Tráng Đang Trộn
                    </span>
                    <button onclick="clearBowl()" class="text-[11px] text-red-600 hover:underline font-bold">
                        🗑️ Đổ đi làm lại
                    </button>
                </div>

                <!-- Danh sách nguyên liệu trong bát -->
                <div id="bowl-contents" class="bg-white p-2.5 rounded-xl border border-amber-200 min-h-[50px] flex flex-wrap gap-1.5 items-center">
                    <span class="text-xs text-slate-400 italic" id="empty-bowl-text">Chưa có nguyên liệu nào. Bấm nguyên liệu phía dưới để cho vào tô!</span>
                </div>

                <!-- NÚT PHỤC VỤ (TRỘN & BÁN) -->
                <button onclick="serveCustomer()" class="w-full mt-3 bg-gradient-to-r from-red-500 to-amber-500 hover:from-red-600 hover:to-amber-600 text-white font-extrabold text-sm py-2.5 rounded-xl shadow-md btn-press flex items-center justify-center gap-2">
                    <span>🔥</span> TRỘN & BÁN CHO KHÁCH
                </button>
            </div>

            <!-- DANH SÁCH NGUYÊN LIỆU ĐỂ CHỌN -->
            <div class="bg-white rounded-2xl p-3 border border-amber-200 shadow-sm">
                <div class="text-xs font-bold text-slate-700 mb-2 flex justify-between items-center">
                    <span>🧂 Chọn nguyên liệu cho vào tô:</span>
                    <span class="text-[10px] text-amber-600 font-normal">Mỗi click = +1 phần</span>
                </div>
                <div id="ingredients-grid" class="grid grid-cols-4 gap-2">
                    <!-- Dynamic Grid Item by JS -->
                </div>
            </div>

        </main>
    </div>

    <!-- MODAL / TAB POPUPS -->

    <!-- MODAL ĐỔI TÊN QUÁN -->
    <div id="modal-name" class="hidden fixed inset-0 bg-black/50 backdrop-blur-sm z-50 flex items-center justify-center p-4">
        <div class="bg-white rounded-2xl p-4 w-full max-w-xs shadow-2xl border border-amber-300">
            <h3 class="font-extrabold text-sm text-slate-800 mb-2">✏️ Đổi Tên Quán Của Bạn</h3>
            <input type="text" id="input-shop-name" maxlength="25" class="w-full border border-amber-300 rounded-lg p-2 text-xs mb-3 focus:outline-none focus:ring-2 focus:ring-red-500" placeholder="Nhập tên quán mới...">
            <div class="flex justify-end gap-2">
                <button onclick="closeModal('modal-name')" class="text-xs px-3 py-1.5 bg-slate-100 rounded-lg font-bold text-slate-600">Hủy</button>
                <button onclick="saveShopName()" class="text-xs px-3 py-1.5 bg-red-500 text-white rounded-lg font-bold">Lưu tên</button>
            </div>
        </div>
    </div>

    <!-- MODAL CHUNG CHO CÁC MỤC TAB (Kho, Giá bán, Nâng cấp...) -->
    <div id="modal-tab" class="hidden fixed inset-0 bg-black/50 backdrop-blur-sm z-50 flex items-center justify-center p-4">
        <div class="bg-amber-50 rounded-2xl w-full max-w-sm max-h-[80vh] flex flex-col shadow-2xl border-2 border-amber-300 overflow-hidden">
            <div class="bg-red-500 text-white p-3 flex justify-between items-center">
                <h3 id="tab-title" class="font-extrabold text-sm">Tiêu đề Tab</h3>
                <button onclick="closeModal('modal-tab')" class="text-white hover:text-amber-200 text-lg font-bold">✕</button>
            </div>
            <div id="tab-content" class="p-3 overflow-y-auto flex-1 custom-scrollbar text-xs">
                <!-- Nội dung động -->
            </div>
        </div>
    </div>

    <!-- MODAL THÔNG BÁO TẠO NGHIỆP / THÀNH CÔNG -->
    <div id="modal-alert" class="hidden fixed inset-0 bg-black/50 backdrop-blur-sm z-50 flex items-center justify-center p-4">
        <div class="bg-white rounded-2xl p-4 w-full max-w-xs text-center shadow-2xl border-2 border-amber-400">
            <div id="alert-icon" class="text-4xl mb-2">🎉</div>
            <h3 id="alert-title" class="font-black text-sm text-slate-800 mb-1">Tiêu đề</h3>
            <p id="alert-msg" class="text-xs text-slate-600 mb-4">Nội dung thông báo...</p>
            <button onclick="closeModal('modal-alert')" class="w-full bg-red-500 text-white font-bold text-xs py-2 rounded-xl btn-press shadow">
                Đã hiểu!
            </button>
        </div>
    </div>

    <!-- JAVASCRIPT GAME LOGIC -->
    <script>
        // --- ÂM THANH HIỆU ỨNG VỚI WEB AUDIO API ---
        const AudioCtx = window.AudioContext || window.webkitAudioContext;
        let audioCtx;

        function initAudio() {
            if (!audioCtx) {
                audioCtx = new AudioCtx();
            }
        }

        function playSound(type) {
            try {
                initAudio();
                if (!audioCtx) return;

                const osc = audioCtx.createOscillator();
                const gain = audioCtx.createGain();
                osc.connect(gain);
                gain.connect(audioCtx.destination);

                const now = audioCtx.currentTime;

                if (type === 'pop') {
                    osc.type = 'sine';
                    osc.frequency.setValueAtTime(400, now);
                    osc.frequency.exponentialRampToValueAtTime(800, now + 0.08);
                    gain.gain.setValueAtTime(0.3, now);
                    gain.gain.linearRampToValueAtTime(0.01, now + 0.08);
                    osc.start(now);
                    osc.stop(now + 0.08);
                } else if (type === 'coin') {
                    osc.type = 'triangle';
                    osc.frequency.setValueAtTime(987.77, now); // B5
                    osc.frequency.setValueAtTime(1318.51, now + 0.08); // E6
                    gain.gain.setValueAtTime(0.3, now);
                    gain.gain.linearRampToValueAtTime(0.01, now + 0.25);
                    osc.start(now);
                    osc.stop(now + 0.25);
                } else if (type === 'error') {
                    osc.type = 'sawtooth';
                    osc.frequency.setValueAtTime(180, now);
                    osc.frequency.setValueAtTime(110, now + 0.1);
                    gain.gain.setValueAtTime(0.3, now);
                    gain.gain.linearRampToValueAtTime(0.01, now + 0.2);
                    osc.start(now);
                    osc.stop(now + 0.2);
                }
            } catch (e) {
                console.log("Audio not allowed yet");
            }
        }

        // --- DỮ LIỆU NGUYÊN LIỆU ---
        const INGREDIENTS = {
            banhtrang: { id: 'banhtrang', name: 'Bánh Tráng', emoji: '🫓', cost: 1000, price: 3000, stock: 30 },
            xoai: { id: 'xoai', name: 'Xoài Bào', emoji: '🥭', cost: 1500, price: 4000, stock: 20 },
            bokho: { id: 'bokho', name: 'Bò Khô', emoji: '🥩', cost: 3000, price: 8000, stock: 15 },
            trungcut: { id: 'trungcut', name: 'Trứng Cút', emoji: '🥚', cost: 1000, price: 3000, stock: 20 },
            lacrang: { id: 'lacrang', name: 'Lạc Rang', emoji: '🥜', cost: 500, price: 2000, stock: 25 },
            hanhphi: { id: 'hanhphi', name: 'Hành Phi', emoji: '🧄', cost: 500, price: 2000, stock: 25 },
            sotot: { id: 'sotot', name: 'Sốt Ớt Cay', emoji: '🌶️', cost: 500, price: 2000, stock: 30 },
            tac: { id: 'tac', name: 'Quả Tắc/Quất', emoji: '🍊', cost: 500, price: 1500, stock: 30 }
        };

        // --- DANH SÁCH KHÁCH HÀNG MẪU ---
        const CUSTOMER_TYPES = [
            { name: "Học Sinh Cấp 3", avatar: "👧", patience: 25, tipBonus: 2000 },
            { name: "Dân Văn Phòng", avatar: "👨‍💼", patience: 18, tipBonus: 5000 },
            { name: "Anh Shipper Đang Vội", avatar: "🛵", patience: 12, tipBonus: 8000 },
            { name: "Blogger Ẩm Thực", avatar: "📸", patience: 20, tipBonus: 10000 },
            { name: "Bà Cùng Xóm Nhiều Chuyện", avatar: "👵", patience: 15, tipBonus: 3000 }
        ];

        // --- TRẠNG THÁI GAME (GAME STATE) ---
        let gameState = {
            shopName: "Quán Mì & Bánh Tráng Bã Táo",
            day: 1,
            money: 50000,
            xp: 0,
            level: 1,
            totalReviews: 0,
            avgRating: 5.0,
            streak5Star: 0,
            
            // Nhiệm vụ
            questTarget: 3,
            questProgress: 0,
            questRewardMoney: 30000,
            questRewardXP: 50,
            questDone: false,

            // Giảm giá đi chợ hôm nay (%)
            discountHaggle: 0,

            // Khách hiện tại
            currentCustomer: null,
            customerTimer: null,
            patienceLeft: 100,

            // Bát đang trộn
            bowl: {},

            // Đánh giá đã nhận
            reviewsList: [],

            // Nhật ký doanh thu
            history: []
        };

        // --- NÂNG CẤP ---
        let upgrades = {
            tables: { name: "Bàn Ghế Xịn Hơn", level: 1, max: 5, basePrice: 50000, effect: "Tăng 10% kiên nhẫn khách" },
            speed: { name: "Tay Trộn Siêu Tốc", level: 1, max: 5, basePrice: 40000, effect: "Tăng 10% XP mỗi đơn hàng" },
            marketing: { name: "Biển Hiệu Đèn LED", level: 1, max: 5, basePrice: 60000, effect: "Tăng 15% tiền tip" }
        };

        // --- KHỞI CHẠY GAME ---
        window.onload = function() {
            renderIngredientsGrid();
            updateUI();
            spawnCustomer();
        };

        // --- CẬP NHẬT GIAO DIỆN HEADER & TRẠNG THÁI ---
        function updateUI() {
            document.getElementById('header-day').innerText = `Ngày ${gameState.day}`;
            document.getElementById('header-money').innerText = gameState.money.toLocaleString('vi-VN') + 'đ';
            document.getElementById('header-rating').innerText = gameState.avgRating.toFixed(1);
            document.getElementById('header-reviews-count').innerText = `(${gameState.totalReviews} đánh giá)`;
            
            // Level & XP
            const xpNeeded = gameState.level * 100;
            document.getElementById('header-level').innerText = `Cấp ${gameState.level}: ${getLevelTitle(gameState.level)}`;
            document.getElementById('header-xp-text').innerText = `${gameState.xp}/${xpNeeded} XP`;
            const xpPercent = Math.min(100, (gameState.xp / xpNeeded) * 100);
            document.getElementById('header-xp-bar').style.width = `${xpPercent}%`;

            // Tên quán
            document.getElementById('shop-name').innerText = gameState.shopName;

            // Nhiệm vụ
            document.getElementById('quest-progress').innerText = `${gameState.questProgress}/${gameState.questTarget}`;
            if (gameState.questDone) {
                document.getElementById('quest-progress').innerText = "Đã xong ✨";
                document.getElementById('quest-progress').className = "text-xs font-extrabold text-emerald-600 bg-emerald-100 px-2 py-1 rounded-md";
            }

            renderBowl();
            renderIngredientsGrid();
        }

        function getLevelTitle(lvl) {
            if (lvl < 2) return "Tập Sự";
            if (lvl < 4) return "Bán Dạo";
            if (lvl < 7) return "Vua Vỉa Hè";
            if (lvl < 10) return "Trùm Bánh Tráng";
            return "Thánh Trộn Tạo Nghiệp";
        }

        // --- HIỂN THỊ LƯỚI NGUYÊN LIỆU ĐỂ BỎ VÀO TÔ ---
        function renderIngredientsGrid() {
            const container = document.getElementById('ingredients-grid');
            container.innerHTML = '';

            Object.keys(INGREDIENTS).forEach(key => {
                const item = INGREDIENTS[key];
                const btn = document.createElement('button');
                btn.onclick = () => addToBowl(key);
                btn.className = `btn-press p-2 rounded-xl border text-center flex flex-col items-center justify-between relative shadow-sm ${
                    item.stock > 0 ? 'bg-amber-50 border-amber-300 hover:bg-amber-100' : 'bg-slate-100 border-slate-300 opacity-60'
                }`;

                btn.innerHTML = `
                    <span class="text-2xl mb-1">${item.emoji}</span>
                    <span class="text-[11px] font-bold text-slate-800 leading-tight">${item.name}</span>
                    <span class="text-[10px] font-semibold text-slate-500 mt-0.5">Kho: ${item.stock}</span>
                `;
                container.appendChild(btn);
            });
        }

        // --- QUẢN LÝ TÔ BÁNH TRÁNG ---
        function addToBowl(ingKey) {
            playSound('pop');
            if (INGREDIENTS[ingKey].stock <= 0) {
                showAlert("Hết Hàng!", `Kho đã hết ${INGREDIENTS[ingKey].name}. Hãy vào KHO để nhập thêm hàng!`, "📦");
                return;
            }

            // Trừ kho & Thêm vào bát
            INGREDIENTS[ingKey].stock--;
            gameState.bowl[ingKey] = (gameState.bowl[ingKey] || 0) + 1;

            updateUI();
        }

        function clearBowl() {
            playSound('pop');
            // Hoàn lại nguyên liệu vào kho
            Object.keys(gameState.bowl).forEach(key => {
                INGREDIENTS[key].stock += gameState.bowl[key];
            });
            gameState.bowl = {};
            updateUI();
        }

        function renderBowl() {
            const container = document.getElementById('bowl-contents');
            const emptyText = document.getElementById('empty-bowl-text');
            
            const keys = Object.keys(gameState.bowl);
            if (keys.length === 0) {
                emptyText.classList.remove('hidden');
                container.innerHTML = '';
                container.appendChild(emptyText);
                return;
            }

            emptyText.classList.add('hidden');
            container.innerHTML = '';

            keys.forEach(key => {
                const count = gameState.bowl[key];
                const item = INGREDIENTS[key];

                const tag = document.createElement('div');
                tag.className = 'bg-amber-200 text-amber-900 text-[11px] font-bold px-2 py-1 rounded-lg flex items-center gap-1 border border-amber-300 shadow-sm animate-pulse';
                tag.innerHTML = `<span>${item.emoji}</span> <span>${item.name}</span> <span class="bg-amber-500 text-white rounded-full text-[9px] w-4 h-4 flex items-center justify-center">x${count}</span>`;
                container.appendChild(tag);
            });
        }

        // --- KHÁCH HÀNG & TẠO ĐƠN HÀNG ---
        function spawnCustomer() {
            if (gameState.currentCustomer) return;

            document.getElementById('no-customer-msg').classList.add('hidden');

            const type = CUSTOMER_TYPES[Math.floor(Math.random() * CUSTOMER_TYPES.length)];
            
            // Tạo order ngẫu nhiên từ 2-4 loại nguyên liệu
            const ingKeys = Object.keys(INGREDIENTS);
            const numItems = Math.floor(Math.random() * 3) + 2; 
            const order = {};

            for (let i = 0; i < numItems; i++) {
                const randomIng = ingKeys[Math.floor(Math.random() * ingKeys.length)];
                order[randomIng] = (order[randomIng] || 0) + Math.floor(Math.random() * 2) + 1;
            }

            gameState.currentCustomer = {
                ...type,
                order: order,
                maxPatience: type.patience + (upgrades.tables.level * 2)
            };

            gameState.patienceLeft = 100;

            // Render giao diện khách
            document.getElementById('customer-avatar').innerText = type.avatar;
            document.getElementById('customer-name').innerText = type.name;
            document.getElementById('customer-mood').innerText = "😊";

            renderCustomerOrder(order);
            startCustomerTimer();
        }

        function renderCustomerOrder(order) {
            const listContainer = document.getElementById('customer-order-list');
            listContainer.innerHTML = '';

            Object.keys(order).forEach(key => {
                const count = order[key];
                const item = INGREDIENTS[key];

                const badge = document.createElement('span');
                badge.className = 'bg-white border border-amber-300 px-2 py-0.5 rounded-md font-bold text-[10px] text-slate-700 shadow-sm';
                badge.innerText = `${item.emoji} ${item.name} x${count}`;
                listContainer.appendChild(badge);
            });
        }

        function startCustomerTimer() {
            clearInterval(gameState.customerTimer);
            const intervalTime = (gameState.currentCustomer.maxPatience * 1000) / 100;

            gameState.customerTimer = setInterval(() => {
                gameState.patienceLeft -= 1;
                
                const bar = document.getElementById('patience-bar');
                const text = document.getElementById('customer-patience');
                const mood = document.getElementById('customer-mood');

                bar.style.width = `${gameState.patienceLeft}%`;

                if (gameState.patienceLeft > 60) {
                    bar.className = 'bg-emerald-500 h-full rounded-full transition-all duration-200';
                    text.innerText = `${gameState.patienceLeft}% Vui vẻ`;
                    text.className = 'text-[10px] font-bold text-emerald-600 bg-emerald-50 px-1.5 py-0.5 rounded border border-emerald-200';
                    mood.innerText = "😊";
                } else if (gameState.patienceLeft > 25) {
                    bar.className = 'bg-amber-500 h-full rounded-full transition-all duration-200';
                    text.innerText = `${gameState.patienceLeft}% Sắp giận`;
                    text.className = 'text-[10px] font-bold text-amber-600 bg-amber-50 px-1.5 py-0.5 rounded border border-amber-200';
                    mood.innerText = "😐";
                } else {
                    bar.className = 'bg-red-500 h-full rounded-full transition-all duration-200';
                    text.innerText = `${gameState.patienceLeft}% Bực mình!`;
                    text.className = 'text-[10px] font-bold text-red-600 bg-red-50 px-1.5 py-0.5 rounded border border-red-200';
                    mood.innerText = "😡";
                }

                if (gameState.patienceLeft <= 0) {
                    clearInterval(gameState.customerTimer);
                    customerLeaveAngry();
                }
            }, intervalTime);
        }

        // --- KHÁCH BỎ VỀ VÌ ĐỜI QuÁ LÂU ---
        function customerLeaveAngry() {
            playSound('error');
            addReview(1, `${gameState.currentCustomer.name}: "Làm lâu muốn xỉu! Chờ 80 năm chưa có bánh tráng. 1 sao tạo nghiệp!"`);
            
            gameState.streak5Star = 0;
            gameState.currentCustomer = null;
            clearBowl();

            showAlert("Khách Bỏ Về! 😡", "Khách hàng bực mình bỏ đi và để lại 1 sao đánh giá tệ hại!", "💔");

            document.getElementById('no-customer-msg').classList.remove('hidden');
        }

        // --- TRỘN & BÁN CHO KHÁCH ---
        function serveCustomer() {
            if (!gameState.currentCustomer) {
                showAlert("Chưa Có Khách!", "Hiện không có khách nào trong quán cả.", "🤔");
                return;
            }

            const customer = gameState.currentCustomer;
            const targetOrder = customer.order;
            const currentBowl = gameState.bowl;

            // Kiểm tra món trong tô có đúng chính xác order không
            let isCorrect = true;

            const targetKeys = Object.keys(targetOrder);
            const bowlKeys = Object.keys(currentBowl);

            if (targetKeys.length !== bowlKeys.length) {
                isCorrect = false;
            } else {
                for (let key of targetKeys) {
                    if (targetOrder[key] !== currentBowl[key]) {
                        isCorrect = false;
                        break;
                    }
                }
            }

            clearInterval(gameState.customerTimer);

            if (isCorrect) {
                playSound('coin');
                
                // Tính tiền bán được
                let totalBill = 0;
                Object.keys(currentBowl).forEach(key => {
                    totalBill += INGREDIENTS[key].price * currentBowl[key];
                });

                // Tính tiền tip dựa trên kiên nhẫn & nâng cấp
                let tip = 0;
                let stars = 5;

                if (gameState.patienceLeft > 60) {
                    stars = 5;
                    tip = Math.round(customer.tipBonus * (1 + (upgrades.marketing.level * 0.15)));
                } else if (gameState.patienceLeft > 25) {
                    stars = 4;
                    tip = Math.round(customer.tipBonus * 0.5);
                } else {
                    stars = 3;
                    tip = 0;
                }

                const finalEarnings = totalBill + tip;
                gameState.money += finalEarnings;

                // Tăng XP
                const xpGained = Math.round(15 * (1 + (upgrades.speed.level * 0.1)));
                addXP(xpGained);

                // Cập nhật nhiệm vụ
                if (stars === 5) {
                    gameState.streak5Star++;
                    if (!gameState.questDone) {
                        gameState.questProgress++;
                        if (gameState.questProgress >= gameState.questTarget) {
                            completeQuest();
                        }
                    }
                } else {
                    gameState.streak5Star = 0;
                }

                // Thêm review
                const positiveReviews = [
                    "Bánh tráng trộn ngon ngất ngây, sốt ớt cay xé lưỡi phê luôn!",
                    "Chủ quán trộn đúng công thức, cho 5 sao ủng hộ quán tạo nghiệp!",
                    "Giao hàng nhanh, ngon và nhiều bò khô xịn xịn!",
                    "Quá đã! Lần sau sẽ dắt cả hội bạn tới ăn tiếp."
                ];
                const randRev = positiveReviews[Math.floor(Math.random() * positiveReviews.length)];
                addReview(stars, `${customer.name}: "${randRev}"`);

                showAlert("Thành Công! 🎉", `Bạn đã thu được <b>${finalEarnings.toLocaleString('vi-VN')}đ</b> (Tiền món: ${totalBill.toLocaleString('vi-VN')}đ + Tip: ${tip.toLocaleString('vi-VN')}đ)<br>+${xpGained} XP`, "💰");

            } else {
                playSound('error');
                // Làm sai món
                gameState.streak5Star = 0;
                addReview(2, `${customer.name}: "Trộn sai nguyên liệu hết trôi! Yêu cầu một đằng làm một nẻo!"`);
                showAlert("Sai Công Thức! 🤮", "Khách hàng nhăn mặt chê dở và chỉ trả 0đ!", "💩");
            }

            // Dọn tô & Chuẩn bị khách mới
            gameState.bowl = {};
            gameState.currentCustomer = null;
            updateUI();

            document.getElementById('no-customer-msg').classList.remove('hidden');
        }

        // --- CỘNG XP & LÊN LEVEL ---
        function addXP(amount) {
            gameState.xp += amount;
            const xpNeeded = gameState.level * 100;

            if (gameState.xp >= xpNeeded) {
                gameState.xp -= xpNeeded;
                gameState.level++;
                showAlert("LÊN CẤP MỚI! 🚀", `Chúc mừng! Quán của bạn đã đạt <b>Cấp ${gameState.level}: ${getLevelTitle(gameState.level)}</b>`, "🏆");
            }
        }

        // --- HỆ THỐNG ĐÁNH GIÁ (REVIEWS) ---
        function addReview(stars, comment) {
            gameState.reviewsList.unshift({ stars, comment, day: gameState.day });
            gameState.totalReviews++;

            // Tính điểm trung bình
            const sum = gameState.reviewsList.reduce((acc, r) => acc + r.stars, 0);
            gameState.avgRating = sum / gameState.reviewsList.length;

            const badge = document.getElementById('badge-reviews');
            badge.innerText = gameState.reviewsList.length;
            badge.classList.remove('hidden');

            updateUI();
        }

        // --- HOÀN THÀNH NHIỆM VỤ HÀNG NGÀY ---
        function completeQuest() {
            gameState.questDone = true;
            gameState.money += gameState.questRewardMoney;
            addXP(gameState.questRewardXP);
            showAlert("Nhiệm Vụ Hoàn Thành! 📜", `Bạn nhận được thưởng: <b>+${gameState.questRewardMoney.toLocaleString('vi-VN')}đ</b> & <b>+${gameState.questRewardXP} XP</b>`, "🎁");
        }

        // --- SỰ KIỆN MINIGAME: ĐI CHỢ TRẢ GIÁ ---
        function startHaggleMiniGame() {
            if (gameState.discountHaggle > 0) {
                showAlert("Hôm Nay Đã Trả Giá!", "Cô Hai đã bớt giá cho bạn rồi, đừng tham quá cô Hai chửi đó!", "👵");
                return;
            }

            const choice = confirm("👵 Cô Hai bán rau: 'Mày muốn trả giá nhiêu?'\n\n[OK] = Xin bớt 20% (Tỷ lệ thành công 60%)\n[Cancel] = Xin bớt 40% (Tỷ lệ thành công 25%)");

            if (choice) {
                // Trả giá 20%
                if (Math.random() < 0.6) {
                    gameState.discountHaggle = 0.20;
                    playSound('coin');
                    showAlert("Thành Công! 👵", "Cô Hai vui vẻ: 'Thôi được rồi, bớt cho mày 20% tiền nhập hàng hôm nay đó!'", "🎉");
                } else {
                    showAlert("Thất Bại! 👵", "Cô Hai lườm: 'Bán đúng giá không bớt! Đi chỗ khác mua đi!'", "😤");
                }
            } else {
                // Trả giá 40%
                if (Math.random() < 0.25) {
                    gameState.discountHaggle = 0.40;
                    playSound('coin');
                    showAlert("Siêu Thành Công! 👵", "Cô Hai thở dài: 'Khéo miệng quá! Bớt luôn 40% cho mày vui đó!'", "🌟");
                } else {
                    showAlert("Bị Chửi! 👵", "Cô Hai quăng cái rổ: 'Trả giá tao lao! Tao không bán cho mày nữa!'", "💢");
                }
            }
        }

        // --- HIỂN THỊ CÁC TAB CHỨC NĂNG (MODAL TAB) ---
        function openTab(tabName) {
            playSound('pop');
            const modal = document.getElementById('modal-tab');
            const title = document.getElementById('tab-title');
            const content = document.getElementById('tab-content');

            modal.classList.remove('hidden');

            if (tabName === 'kho') {
                title.innerText = "📦 Kho & Nhập Nguyên Liệu";
                let html = '<div class="space-y-2">';
                Object.keys(INGREDIENTS).forEach(key => {
                    const item = INGREDIENTS[key];
                    const discountCost = Math.round(item.cost * (1 - gameState.discountHaggle));

                    html += `
                        <div class="bg-white p-2 rounded-xl border border-amber-200 flex items-center justify-between shadow-sm">
                            <div class="flex items-center gap-2">
                                <span class="text-2xl">${item.emoji}</span>
                                <div>
                                    <div class="font-bold text-slate-800">${item.name}</div>
                                    <div class="text-[10px] text-slate-500">Tồn kho: <b>${item.stock}</b> | Giá gốc nhập: ${item.cost.toLocaleString('vi-VN')}đ</div>
                                </div>
                            </div>
                            <button onclick="buyStock('${key}')" class="btn-press bg-emerald-500 text-white font-bold px-2.5 py-1.5 rounded-lg text-[11px] shadow">
                                +10 phần (${(discountCost * 10).toLocaleString('vi-VN')}đ)
                            </button>
                        </div>
                    `;
                });
                html += '</div>';
                content.innerHTML = html;

            } else if (tabName === 'giaban') {
                title.innerText = "🏷️ Cấu Hình Giá Bán Món";
                let html = '<div class="space-y-2">';
                Object.keys(INGREDIENTS).forEach(key => {
                    const item = INGREDIENTS[key];
                    html += `
                        <div class="bg-white p-2 rounded-xl border border-amber-200 flex items-center justify-between">
                            <div class="flex items-center gap-2">
                                <span class="text-2xl">${item.emoji}</span>
                                <span class="font-bold text-slate-800">${item.name}</span>
                            </div>
                            <div class="flex items-center gap-1">
                                <button onclick="changeIngredientPrice('${key}', -500)" class="bg-slate-200 px-2 py-0.5 rounded font-bold">-</button>
                                <span class="font-extrabold text-red-600 px-1">${item.price.toLocaleString('vi-VN')}đ</span>
                                <button onclick="changeIngredientPrice('${key}', 500)" class="bg-slate-200 px-2 py-0.5 rounded font-bold">+</button>
                            </div>
                        </div>
                    `;
                });
                html += '</div>';
                content.innerHTML = html;

            } else if (tabName === 'nangcap') {
                title.innerText = "🚀 Nâng Cấp Quán Bánh Tráng";
                let html = '<div class="space-y-2.5">';
                Object.keys(upgrades).forEach(key => {
                    const up = upgrades[key];
                    const price = up.basePrice * up.level;
                    html += `
                        <div class="bg-white p-2.5 rounded-xl border border-amber-200">
                            <div class="flex justify-between items-center mb-1">
                                <span class="font-bold text-slate-800">${up.name} (Cấp ${up.level}/${up.max})</span>
                                <span class="text-[10px] text-amber-700 bg-amber-100 px-1.5 py-0.5 rounded font-bold">${up.effect}</span>
                            </div>
                            <div class="flex justify-between items-center mt-2">
                                <span class="text-xs font-black text-emerald-600">${price.toLocaleString('vi-VN')}đ</span>
                                <button onclick="buyUpgrade('${key}')" class="btn-press bg-red-500 text-white font-bold px-3 py-1 rounded-lg text-xs shadow">
                                    Nâng Cấp
                                </button>
                            </div>
                        </div>
                    `;
                });
                html += '</div>';
                content.innerHTML = html;

            } else if (tabName === 'trangtri') {
                title.innerText = "🏮 Trang Trí Quán";
                content.innerHTML = `
                    <div class="text-center py-6">
                        <span class="text-4xl mb-2 block">🎨</span>
                        <p class="font-bold text-slate-700">Tính năng Trang Trí Quán</p>
                        <p class="text-[11px] text-slate-500 mt-1">Sẽ ra mắt thêm bàn ghế nhựa đỏ, đèn lồng Hội An trong phiên bản nâng cấp tiếp theo!</p>
                    </div>
                `;

            } else if (tabName === 'danhgia') {
                title.innerText = "⭐ Nhật Ký Đánh Giá Từ Khách";
                document.getElementById('badge-reviews').classList.add('hidden');

                if (gameState.reviewsList.length === 0) {
                    content.innerHTML = '<p class="text-center text-slate-400 py-6">Chưa có đánh giá nào từ khách hàng.</p>';
                } else {
                    let html = '<div class="space-y-2">';
                    gameState.reviewsList.forEach(r => {
                        html += `
                            <div class="bg-white p-2.5 rounded-xl border border-amber-200 shadow-sm">
                                <div class="flex justify-between text-xs mb-1">
                                    <span class="text-yellow-500 font-bold">${'★'.repeat(r.stars)}${'☆'.repeat(5 - r.stars)}</span>
                                    <span class="text-[10px] text-slate-400">Ngày ${r.day}</span>
                                </div>
                                <p class="text-xs text-slate-700 italic">${r.comment}</p>
                            </div>
                        `;
                    });
                    html += '</div>';
                    content.innerHTML = html;
                }

            } else if (tabName === 'sosach') {
                title.innerText = "📖 Sổ Sách Thống Kê Quán";
                content.innerHTML = `
                    <div class="bg-white p-3 rounded-xl border border-amber-200 space-y-2">
                        <div class="flex justify-between">
                            <span class="text-slate-600">Tổng doanh thu hiện tại:</span>
                            <span class="font-bold text-emerald-600">${gameState.money.toLocaleString('vi-VN')}đ</span>
                        </div>
                        <div class="flex justify-between">
                            <span class="text-slate-600">Số đơn phục vụ:</span>
                            <span class="font-bold text-slate-800">${gameState.totalReviews} đơn</span>
                        </div>
                        <div class="flex justify-between">
                            <span class="text-slate-600">Đánh giá trung bình:</span>
                            <span class="font-bold text-yellow-600">${gameState.avgRating.toFixed(1)} / 5.0 ⭐</span>
                        </div>
                    </div>
                `;
            }
        }

        // --- MUA NGUYÊN LIỆU TRONG KHO ---
        function buyStock(key) {
            const item = INGREDIENTS[key];
            const discountCost = Math.round(item.cost * (1 - gameState.discountHaggle));
            const totalPrice = discountCost * 10;

            if (gameState.money < totalPrice) {
                showAlert("Không Đủ Tiền!", "Két tiền không đủ để nhập thêm lô hàng này!", "💸");
                return;
            }

            playSound('coin');
            gameState.money -= totalPrice;
            item.stock += 10;

            updateUI();
            openTab('kho'); // refresh
        }

        function changeIngredientPrice(key, amount) {
            if (INGREDIENTS[key].price + amount < 1000) return;
            INGREDIENTS[key].price += amount;
            openTab('giaban');
        }

        function buyUpgrade(key) {
            const up = upgrades[key];
            const price = up.basePrice * up.level;

            if (up.level >= up.max) {
                showAlert("Đã Tối Đa!", "Nâng cấp này đã đạt cấp tối đa.", "🛡️");
                return;
            }

            if (gameState.money < price) {
                showAlert("Không Đủ Tiền!", "Cần thêm tiền để nâng cấp hạng mục này!", "💸");
                return;
            }

            playSound('coin');
            gameState.money -= price;
            up.level++;

            updateUI();
            openTab('nangcap');
        }

        // --- ĐỔI TÊN QUÁN ---
        function openChangeNameModal() {
            document.getElementById('modal-name').classList.remove('hidden');
            document.getElementById('input-shop-name').value = gameState.shopName;
        }

        function saveShopName() {
            const val = document.getElementById('input-shop-name').value.trim();
            if (val) {
                gameState.shopName = val;
                updateUI();
            }
            closeModal('modal-name');
        }

        // --- ĐÓNG/MỞ MODAL HỖ TRỢ ---
        function closeModal(id) {
            playSound('pop');
            document.getElementById(id).classList.add('hidden');
        }

        function showAlert(title, msg, icon = "🎉") {
            document.getElementById('alert-icon').innerText = icon;
            document.getElementById('alert-title').innerText = title;
            document.getElementById('alert-msg').innerHTML = msg;
            document.getElementById('modal-alert').classList.remove('hidden');
        }
    </script>
</body>
</html>
