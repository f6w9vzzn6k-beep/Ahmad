<!DOCTYPE html>
<html lang="ar" dir="rtl">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>منصة عقارات الخفجي 3D - الخريطة العقارية التفاعلية</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/three.js/r128/three.min.js"></script>
    <link href="https://fonts.googleapis.com/css2?family=Tajawal:wght@400;500;700;800;900&display=swap" rel="stylesheet">
    <style>
        * { -webkit-tap-highlight-color: transparent; touch-action: manipulation; }
        body { font-family: 'Tajawal', sans-serif; background-color: #0b132b; color: #f8fafc; overflow-x: hidden; }
        #canvas-container { width: 100%; height: 55vh; position: relative; border-radius: 20px; overflow: hidden; }
        .glass-card { background: rgba(28, 37, 65, 0.85); backdrop-filter: blur(16px); border: 1px solid rgba(255, 255, 255, 0.12); }
    </style>
</head>
<body class="p-3 md:p-6 pb-24">

    <!-- Header Navigation with Custom Logos -->
    <header class="glass-card p-3.5 rounded-2xl flex justify-between items-center mb-3 shadow-lg">
        <div class="flex items-center gap-3">
            <div class="relative flex items-center justify-center">
                <div class="w-12 h-12 bg-gradient-to-tr from-emerald-600 via-teal-500 to-blue-600 rounded-2xl flex flex-col items-center justify-center font-black text-white shadow-md border border-emerald-400/40 p-1 text-center leading-none">
                    <span class="text-[9px] text-amber-300 font-extrabold tracking-tight">عقارات</span>
                    <span class="text-xs text-white font-black">الخفجي</span>
                    <span class="text-[8px] text-emerald-200">3D</span>
                </div>
                <span class="absolute -bottom-1 -right-1 flex h-3 w-3">
                    <span class="animate-ping absolute inline-flex h-full w-full rounded-full bg-emerald-400 opacity-75"></span>
                    <span class="relative inline-flex rounded-full h-3 w-3 bg-emerald-500"></span>
                </span>
            </div>
            
            <div>
                <h1 class="font-black text-base text-white leading-tight flex items-center gap-1.5">
                    منصة عقارات الخفجي
                </h1>
                <p class="text-[11px] text-blue-300 font-bold flex items-center gap-1">
                    🏛️ الشعار الرسمي والمخططات المعتمدة
                </p>
            </div>
        </div>

        <button onclick="toggleAdminModal()" class="bg-gradient-to-r from-blue-600 to-indigo-600 active:scale-95 text-white px-3.5 py-2.5 rounded-xl text-xs font-black shadow-lg transition flex items-center gap-1.5">
            <span>👑</span>
            <span>دخول الإدارة</span>
        </button>
    </header>

    <!-- Main Mobile Viewer Area -->
    <main class="space-y-3">

        <!-- Plan Selector Dropdown -->
        <div class="glass-card p-3 rounded-2xl shadow-md">
            <div class="flex justify-between items-center mb-1.5">
                <label class="block text-[11px] font-bold text-gray-400">اختر المخطط المعروض (10 مخططات):</label>
                <span class="text-[10px] bg-blue-500/20 text-blue-300 px-2 py-0.5 rounded-full font-bold">مدينة الخفجي</span>
            </div>
            <select id="planSelect" onchange="changePlan()" class="w-full bg-slate-900/90 border border-slate-700/80 rounded-xl p-3 text-sm font-bold text-white focus:outline-none focus:border-blue-500">
                <option value="amal">📍 مخطط حي الأمل (المخطط 8/46)</option>
                <option value="aziziyah">📍 مخطط حي العزيزية</option>
                <option value="king_fahd">📍 مخطط حي الملك فهد</option>
                <option value="rayyan">📍 مخطط حي الريان</option>
                <option value="wurood">📍 مخطط حي الورود</option>
                <option value="hamra">📍 مخطط حي الحمراء</option>
                <option value="faisaliyah">📍 مخطط حي الفيصلية</option>
                <option value="jowharah">📍 مخطط حي الجوهرة</option>
                <option value="salam">📍 مخطط حي السلام</option>
                <option value="yasmeen">📍 مخطط حي الياسمين</option>
            </select>
        </div>

        <!-- 3D Interactive Viewport -->
        <div class="glass-card p-2 rounded-2xl relative shadow-2xl">
            <div class="absolute top-4 right-4 z-10 bg-slate-950/70 backdrop-blur-md px-3 py-1.5 rounded-xl border border-slate-800 flex items-center gap-2">
                <span class="text-xs font-black text-amber-400">شعار عقارات الخفجي 3D</span>
            </div>

            <div id="canvas-container"></div>
            
            <div class="absolute bottom-3 left-3 right-3 bg-slate-900/80 backdrop-filter blur-md border border-slate-700/50 p-2 rounded-xl text-center text-[11px] text-blue-300 font-medium">
                👆 حرّك بلمس إسبعك لتدوير خريطة الـ 3D والتكبير/التصغير
            </div>
        </div>

        <!-- Status Legend -->
        <div class="grid grid-cols-3 gap-2 text-center text-xs font-bold">
            <div class="glass-card p-2.5 rounded-xl border-b-2 border-emerald-500 text-emerald-400">
                متاحة (أخضر)
            </div>
            <div class="glass-card p-2.5 rounded-xl border-b-2 border-amber-500 text-amber-400">
                محجوزة (أصفر)
            </div>
            <div class="glass-card p-2.5 rounded-xl border-b-2 border-rose-500 text-rose-400">
                مبيعة (أحمر)
            </div>
        </div>

        <!-- Selected Plot & Contact Card for Visitors -->
        <div class="glass-card p-4 rounded-2xl border-r-4 border-emerald-500 shadow-md space-y-3">
            <div class="flex justify-between items-center">
                <span class="text-xs font-bold text-gray-300">بيانات القطعة المحددة:</span>
                <span id="plotBadge" class="px-2.5 py-0.5 rounded-full text-xs font-black bg-emerald-500/20 text-emerald-400 border border-emerald-500/30">متاحة للبيع</span>
            </div>

            <div class="grid grid-cols-2 gap-2 text-xs">
                <div class="bg-slate-900/60 p-2.5 rounded-xl">
                    <span class="block text-gray-400 text-[10px]">رقم القطعة</span>
                    <span id="plotNo" class="font-extrabold text-sm text-blue-400">107 / أ</span>
                </div>
                <div class="bg-slate-900/60 p-2.5 rounded-xl">
                    <span class="block text-gray-400 text-[10px]">المساحة</span>
                    <span id="plotArea" class="font-extrabold text-sm text-white">488 م²</span>
                </div>
                <div class="bg-slate-900/60 p-2.5 rounded-xl">
                    <span class="block text-gray-400 text-[10px]">عرض الشارع</span>
                    <span id="plotStreet" class="font-extrabold text-sm text-emerald-400">شارع 25م</span>
                </div>
                <div class="bg-slate-900/60 p-2.5 rounded-xl">
                    <span class="block text-gray-400 text-[10px]">المكتب المسؤول</span>
                    <span id="officeName" class="font-extrabold text-xs text-amber-400">مكتب فرصة العقاري</span>
                </div>
            </div>

            <!-- Direct Contact Buttons -->
            <div class="pt-2 border-t border-slate-700/80 space-y-2">
                <div class="grid grid-cols-2 gap-2">
                    <a id="whatsappBtn" href="https://wa.me/966508788666?text=السلام%20عليكم%20استفسر%20عن%20القطعة107أ" target="_blank" class="bg-emerald-600 hover:bg-emerald-700 active:scale-95 text-white font-bold py-2.5 px-3 rounded-xl text-xs flex items-center justify-center gap-1.5 shadow-lg transition">
                        <span>💬</span>
                        <span>واتساب المكتب</span>
                    </a>
                    <a id="phoneBtn" href="tel:0508788666" class="bg-blue-600 hover:bg-blue-700 active:scale-95 text-white font-bold py-2.5 px-3 rounded-xl text-xs flex items-center justify-center gap-1.5 shadow-lg transition">
                        <span>📞</span>
                        <span>اتصال بمكتب العقار</span>
                    </a>
                </div>

                <!-- Platform Support Contact -->
                <a href="https://wa.me/966508788666?text=استفسار%20خدمة%20عملاء%20عقارات%20الخفجي" target="_blank" class="w-full bg-slate-800 hover:bg-slate-700 text-gray-300 font-bold py-2 px-3 rounded-xl text-[11px] flex items-center justify-center gap-2 transition border border-slate-700">
                    <span>🎧</span>
                    <span>تواصل مع خدمة عملاء المنصة الرئيسية</span>
                </a>
            </div>
        </div>

    </main>

    <!-- Admin & Office Multi-User Control Modal -->
    <div id="adminModal" class="fixed inset-0 bg-slate-950/95 backdrop-blur-xl z-50 p-4 hidden flex-col justify-between overflow-y-auto">
        <div>
            <div class="flex justify-between items-center pb-4 border-b border-slate-800">
                <div class="flex items-center gap-2">
                    <span class="text-2xl">👑</span>
                    <div>
                        <h2 class="font-black text-lg text-white">مركز إدارة عقارات الخفجي</h2>
                        <p class="text-xs text-emerald-400">المدير الرئيسي + مكاتب العقار المضافة</p>
                    </div>
                </div>
                <button onclick="toggleAdminModal()" class="w-8 h-8 rounded-full bg-slate-800 text-gray-400 font-bold flex items-center justify-center">✕</button>
            </div>

            <div class="mt-4 space-y-4">
                <div class="bg-slate-900/90 p-4 rounded-2xl border border-blue-500/30">
                    <h3 class="font-bold text-xs text-blue-400 mb-2 flex items-center gap-1">
                        <span>➕</span> إضافة مكتب عقاري جديد (صلاحية المدير الرئيسي)
                    </h3>
                    <div class="space-y-2">
                        <input id="newOfficeName" type="text" placeholder="اسم مكتب العقار (مثال: مكتب الفرسان)" class="w-full bg-slate-950 border border-slate-700 rounded-xl p-2.5 text-xs font-bold text-white focus:outline-none focus:border-blue-500">
                        <input id="newOfficeUser" type="text" placeholder="اسم المستخدم (اليوزر)" class="w-full bg-slate-950 border border-slate-700 rounded-xl p-2.5 text-xs font-bold text-white focus:outline-none focus:border-blue-500">
                        <input id="newOfficePass" type="password" placeholder="كلمة المرور" class="w-full bg-slate-950 border border-slate-700 rounded-xl p-2.5 text-xs font-bold text-white focus:outline-none focus:border-blue-500">
                        <button onclick="addOfficeUser()" class="w-full bg-blue-600 hover:bg-blue-700 text-white font-bold py-2.5 rounded-xl text-xs transition">
                            حفظ وإنشاء يوزر للمكتب
                        </button>
                    </div>
                </div>

                <div class="bg-slate-900/90 p-4 rounded-2xl border border-slate-800">
                    <label class="block text-xs font-bold text-gray-300 mb-2">تحديث حالة القطعة بالخريطة الـ 3D:</label>
                    <div class="grid grid-cols-3 gap-2 mb-3">
                        <button onclick="updateStatus('available')" class="p-2.5 rounded-xl text-xs font-bold bg-emerald-600/30 border border-emerald-500 text-emerald-300 active:bg-emerald-600">متاحة 🟩</button>
                        <button onclick="updateStatus('reserved')" class="p-2.5 rounded-xl text-xs font-bold bg-amber-600/30 border border-amber-500 text-amber-300 active:bg-amber-600">محجوزة 🟧</button>
                        <button onclick="updateStatus('sold')" class="p-2.5 rounded-xl text-xs font-bold bg-rose-600/30 border border-rose-500 text-rose-300 active:bg-rose-600">مبيعة 🟥</button>
                    </div>
                </div>
            </div>
        </div>

        <button onclick="toggleAdminModal()" class="w-full bg-slate-800 text-gray-300 font-bold py-3 rounded-xl text-xs mt-6">
            إغلاق شاشة الإدارة
        </button>
    </div>

    <!-- Three.js Interactive Engine -->
    <script>
        const container = document.getElementById('canvas-container');
        const scene = new THREE.Scene();
        scene.background = new THREE.Color(0x0b132b);

        const camera = new THREE.PerspectiveCamera(45, container.clientWidth / container.clientHeight, 0.1, 1000);
        camera.position.set(0, 25, 35);
        camera.lookAt(0, 0, 0);

        const renderer = new THREE.WebGLRenderer({ antialias: true, alpha: true });
        renderer.setPixelRatio(Math.min(window.devicePixelRatio, 2));
        renderer.setSize(container.clientWidth, container.clientHeight);
        container.appendChild(renderer.domElement);

        const ambientLight = new THREE.AmbientLight(0xffffff, 0.8);
        scene.add(ambientLight);

        const dirLight = new THREE.DirectionalLight(0xffffff, 0.9);
        dirLight.position.set(15, 30, 15);
        scene.add(dirLight);

        let plotsGroup = new THREE.Group();
        const colors = [0x10b981, 0xf59e0b, 0xef4444];

        function create3DGrid() {
            scene.remove(plotsGroup);
            plotsGroup = new THREE.Group();

            for (let x = -12; x <= 12; x += 3.2) {
                for (let z = -8; z <= 8; z += 3.2) {
                    if (Math.abs(x) < 1 || Math.abs(z) < 1) continue;
                    
                    const height = 0.4 + Math.random() * 0.4;
                    const geometry = new THREE.BoxGeometry(2.8, height, 2.8);
                    const color = colors[Math.floor(Math.random() * colors.length)];
                    const material = new THREE.MeshStandardMaterial({ color: color, roughness: 0.2, metalness: 0.1 });
                    const mesh = new THREE.Mesh(geometry, material);
                    mesh.position.set(x, height / 2, z);
                    plotsGroup.add(mesh);
                }
            }
            scene.add(plotsGroup);
        }
        create3DGrid();

        let isTouching = false;
        let previousTouchX = 0;

        container.addEventListener('touchstart', (e) => {
            isTouching = true;
            previousTouchX = e.touches[0].clientX;
        });

        container.addEventListener('touchmove', (e) => {
            if (!isTouching) return;
            const deltaX = e.touches[0].clientX - previousTouchX;
            plotsGroup.rotation.y += deltaX * 0.01;
            previousTouchX = e.touches[0].clientX;
        });

        container.addEventListener('touchend', () => { isTouching = false; });

        function animate() {
            requestAnimationFrame(animate);
            if (!isTouching) {
                plotsGroup.rotation.y += 0.002;
            }
            renderer.render(scene, camera);
        }
        animate();

        function toggleAdminModal() {
            const modal = document.getElementById('adminModal');
            modal.classList.toggle('hidden');
            modal.classList.toggle('flex');
        }

        function changePlan() {
            create3DGrid();
        }

        function addOfficeUser() {
            const name = document.getElementById('newOfficeName').value;
            if(!name) { alert('يرجى كتابة اسم المكتب العقاري'); return; }
            alert('تم إنشاء يوزر خاص للمكتب: ' + name + ' بنجاح!');
            document.getElementById('newOfficeName').value = '';
        }

        function updateStatus(st) {
            alert('تم تحديث حالة القطعة فوراً باللون الجديد في خريطة الخفجي الـ 3D!');
            toggleAdminModal();
        }
    </script>
</body>
</html>

