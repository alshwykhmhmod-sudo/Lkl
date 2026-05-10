‏<!DOCTYPE html>
‏<html lang="ar" dir="rtl">
‏<head>
‏    <meta charset="UTF-8">
‏    <meta name="viewport" content="width=device-width, initial-scale=1.0">
‏    <title>محرك قوانين الفيزياء الذكي</title>
‏    <script src="https://cdn.tailwindcss.com"></script>
‏    <script id="MathJax-script" async src="https://cdn.jsdelivr.net/npm/mathjax@3/es5/tex-mml-chtml.js"></script>
‏    <style>
‏        @import url('https://fonts.googleapis.com/css2?family=Tajawal:wght@400;500;700&display=swap');
‏        body {
‏            font-family: 'Tajawal', sans-serif;
‏            background-color: #f0f4f8;
‏        }
‏        .law-card { transition: all 0.3s ease; }
‏        .law-card:hover { transform: translateY(-5px); box-shadow: 0 10px 20px rgba(0,0,0,0.1); }
‏        .math-container { overflow-x: auto; }
‏    </style>
‏</head>
‏<body class="min-h-screen p-4 md:p-10">
‏
‏    <div class="max-w-4xl mx-auto">
‏        <header class="text-center mb-10">
‏            <h1 class="text-3xl font-extrabold text-blue-800 mb-2">مساعد الفيزياء الذكي</h1>
‏            <p class="text-gray-600">ابحث بالرموز (m, v, a) أو اسم القانون</p>
‏        </header>
‏
‏        <div class="bg-white p-6 rounded-2xl shadow-lg mb-8 border border-blue-50">
‏            <div class="flex flex-col md:flex-row gap-3">
‏                <input
‏                    type="text"
‏                    id="symbolInput"
‏                    placeholder="أدخل الرموز.. مثال: m, a"
‏                    class="flex-1 p-4 border-2 border-gray-100 rounded-xl focus:border-blue-500 outline-none text-lg"
‏                >
‏                <button
‏                    onclick="searchLaws()"
‏                    class="bg-blue-600 hover:bg-blue-700 text-white px-8 py-4 rounded-xl font-bold transition-all"
‏                >
‏                    بحث
‏                </button>
‏            </div>
‏        </div>
‏
‏        <div id="resultsArea" class="grid grid-cols-1 md:grid-cols-2 gap-6"></div>
‏
‏        <div id="emptyState" class="hidden text-center py-10 text-gray-500">
‏            <p>لم نجد قوانين مطابقة.. حاول إدخال رموز أخرى</p>
‏        </div>
‏    </div>
‏
‏    <script>
‏        const physicsDB = [
‏            {
‏                name: "قانون نيوتن الثاني",
‏                formula: "F = m \\cdot a",
‏                symbols: { F: "القوة", m: "الكتلة", a: "التسارع" },
‏                tags: ["ميكانيكا"]
‏            },
‏            {
‏                name: "طاقة الحركة",
‏                formula: "E_k = \\frac{1}{2} m v^2",
‏                symbols: { Ek: "طاقة الحركة", m: "الكتلة", v: "السرعة" },
‏                tags: ["طاقة"]
‏            },
‏            {
‏                name: "قانون أوم",
‏                formula: "V = I \\cdot R",
‏                symbols: { V: "الجهد", I: "التيار", R: "المقاومة" },
‏                tags: ["كهرباء"]
‏            },
‏            {
‏                name: "السرعة",
‏                formula: "v = \\frac{d}{t}",
‏                symbols: { v: "السرعة", d: "المسافة", t: "الزمن" },
‏                tags: ["حركة"]
‏            },
‏            {
‏                name: "الضغط",
‏                formula: "P = \\frac{F}{A}",
‏                symbols: { P: "الضغط", F: "القوة", A: "المساحة" },
‏                tags: ["ميكانيكا"]
‏            }
‏        ];
‏
‏        function searchLaws() {
‏            const input = document.getElementById("symbolInput").value.trim().toLowerCase();
‏            const resultsArea = document.getElementById("resultsArea");
‏            const emptyState = document.getElementById("emptyState");
‏
‏            resultsArea.innerHTML = "";
‏            
‏            if (!input) return;
‏
‏            const userTerms = input.split(",").map(t => t.trim()).filter(t => t !== "");
‏
‏            const foundLaws = physicsDB.filter(law => {
‏                const lawSymbols = Object.keys(law.symbols).map(s => s.toLowerCase());
‏                const lawName = law.name.toLowerCase();
‏                return userTerms.some(term => 
‏                    lawSymbols.some(s => s.includes(term)) || lawName.includes(term)
‏                );
‏            });
‏
‏            if (foundLaws.length === 0) {
‏                emptyState.classList.remove("hidden");
‏                return;
‏            }
‏
‏            emptyState.classList.add("hidden");
‏
‏            foundLaws.forEach(law => {
‏                const card = document.createElement("div");
‏                card.className = "law-card bg-white p-6 rounded-2xl shadow-md border-r-4 border-blue-500";
‏
‏                let symbolsHtml = "";
‏                for (const sym in law.symbols) {
‏                    symbolsHtml += `<li><span class="font-bold text-blue-600" dir="ltr">${sym}</span>: ${law.symbols[sym]}</li>`;
‏                }
‏
‏                card.innerHTML = `
‏                    <h3 class="text-xl font-bold mb-3">${law.name}</h3>
‏                    <div class="math-container bg-gray-50 p-4 rounded-lg mb-4 text-center text-blue-900" dir="ltr">
‏                        \\[ ${law.formula} \\]
‏                    </div>
‏                    <ul class="text-sm text-gray-600 space-y-1">${symbolsHtml}</ul>
‏                `;
‏                resultsArea.appendChild(card);
‏            });
‏
‏            if (window.MathJax) MathJax.typesetPromise([resultsArea]);
‏        }
‏    </script>
‏</body>
‏</html>
‏
