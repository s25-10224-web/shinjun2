# shinjun2
<!DOCTYPE html>
<html lang="ko">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>ADMIT AI - 입시 & 유학 통합 분석기</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <script src="https://unpkg.com/lucide@latest"></script>
    <link href="https://fonts.googleapis.com/css2?family=Pretendard:wght@400;600;800;900&display=swap" rel="stylesheet">
    <style>
        body { font-family: 'Pretendard', sans-serif; }
        .glass-header { background: rgba(255, 255, 255, 0.9); backdrop-filter: blur(12px); }
        .animate-fade-in { animation: fadeIn 0.5s ease-out forwards; }
        @keyframes fadeIn { from { opacity: 0; transform: translateY(10px); } to { opacity: 1; transform: translateY(0); } }
        
        /* 스크롤바 디자인 */
        ::-webkit-scrollbar { width: 8px; }
        ::-webkit-scrollbar-track { background: #f1f1f1; }
        ::-webkit-scrollbar-thumb { background: #cbd5e1; border-radius: 10px; }
        ::-webkit-scrollbar-thumb:hover { background: #94a3b8; }
    </style>
</head>
<body class="bg-slate-50 text-slate-800 min-h-screen">

    <div id="app">
        <!-- 로그인 화면 -->
        <div id="login-screen" class="min-h-screen flex items-center justify-center p-4">
            <div class="max-w-md w-full bg-white rounded-3xl shadow-2xl p-10 space-y-8 border border-slate-100">
                <div class="text-center space-y-3">
                    <div class="inline-flex p-4 bg-indigo-600 rounded-2xl text-white shadow-xl shadow-indigo-100">
                        <i data-lucide="sparkles" size="32"></i>
                    </div>
                    <h1 class="text-3xl font-black text-slate-900 tracking-tighter">ADMIT AI</h1>
                    <p class="text-slate-500 text-sm font-bold">국내외 통합 입시 컨설팅 시스템</p>
                </div>
                <div class="space-y-4">
                    <input id="login-id" type="text" class="w-full px-5 py-4 bg-slate-50 border border-slate-200 rounded-2xl outline-none focus:ring-2 focus:ring-indigo-500 font-bold" placeholder="아이디 (admin)">
                    <input id="login-pw" type="password" class="w-full px-5 py-4 bg-slate-50 border border-slate-200 rounded-2xl outline-none focus:ring-2 focus:ring-indigo-500 font-bold" placeholder="비밀번호 (1234)">
                    <p id="login-error" class="text-red-500 text-xs font-bold text-center hidden">아이디 또는 비밀번호가 올바르지 않습니다.</p>
                    <button onclick="handleLogin()" class="w-full bg-indigo-600 hover:bg-indigo-700 text-white font-black py-5 rounded-2xl transition-all shadow-xl shadow-indigo-100">분석 시스템 시작</button>
                </div>
            </div>
        </div>

        <!-- 메인 화면 -->
        <div id="main-screen" class="hidden flex flex-col min-h-screen">
            <header class="glass-header border-b border-slate-200 sticky top-0 z-50 px-8 py-4 flex items-center justify-between">
                <div class="flex items-center gap-3">
                    <div class="bg-indigo-600 p-2 rounded-lg text-white"><i data-lucide="globe" size="20"></i></div>
                    <h2 class="text-xl font-black tracking-tighter">ADMIT <span class="text-indigo-600">AI GLOBAL</span></h2>
                </div>
                <div class="flex items-center gap-4">
                    <button onclick="switchTab('editor')" id="tab-editor" class="px-4 py-2 rounded-xl text-sm font-black transition-all bg-indigo-600 text-white">입시 분석기</button>
                    <button onclick="switchTab('profile')" id="tab-profile" class="px-4 py-2 rounded-xl text-sm font-black transition-all text-slate-400 hover:bg-slate-100">성적 및 진로설정</button>
                    <div class="h-6 w-px bg-slate-200 mx-2"></div>
                    <button onclick="logout()" class="text-slate-400 hover:text-red-500 text-xs font-black uppercase">로그아웃</button>
                </div>
            </header>

            <main id="tab-content-editor" class="flex-1 p-8 lg:p-12 overflow-y-auto animate-fade-in">
                <div class="max-w-6xl mx-auto space-y-12">
                    <section class="flex flex-col md:flex-row md:items-end justify-between gap-4">
                        <div>
                            <h3 class="text-3xl font-black text-slate-900 tracking-tighter">통합 생기부 에디터</h3>
                            <p class="text-slate-500 font-medium italic">입력하신 데이터는 Gemini AI가 보안 처리 후 실시간 분석합니다.</p>
                        </div>
                        <div class="flex gap-2">
                            <span id="badge-major" class="px-4 py-2 bg-white rounded-full border text-[11px] font-black text-indigo-600 shadow-sm">🎯 희망학과: 미설정</span>
                            <span id="badge-gpa" class="px-4 py-2 bg-white rounded-full border text-[11px] font-black text-emerald-600 shadow-sm">📊 내신: -등급</span>
                        </div>
                    </section>

                    <div class="bg-white rounded-[2.5rem] shadow-2xl shadow-slate-200 border border-slate-200 overflow-hidden">
                        <textarea id="input-content" class="w-full p-10 h-80 outline-none text-slate-700 leading-relaxed resize-none text-xl placeholder:text-slate-200 font-medium" placeholder="학생부 내용을 자유롭게 입력하세요. (교과세특, 자율활동 등)"></textarea>
                        <div class="px-10 py-6 bg-slate-50 border-t border-slate-100 flex justify-between items-center">
                            <p id="char-count" class="text-xs font-bold text-slate-400">현재 글자 수: 0자</p>
                            <button id="btn-analyze" onclick="analyzeContent()" class="bg-indigo-600 hover:bg-indigo-700 text-white px-10 py-4 rounded-2xl font-black flex items-center gap-3 transition-all">
                                <i id="icon-analyze" data-lucide="sparkles" size="18"></i>
                                <span id="text-analyze">AI 정밀 분석 시작</span>
                                <i data-lucide="arrow-right" size="18"></i>
                            </button>
                        </div>
                    </div>

                    <!-- 결과 표시 영역 -->
                    <div id="results-area" class="hidden grid grid-cols-1 lg:grid-cols-12 gap-8 animate-fade-in">
                        <div class="lg:col-span-7 space-y-8" id="analysis-left"></div>
                        <div class="lg:col-span-5 space-y-8" id="analysis-right"></div>
                    </div>
                </div>
            </main>

            <main id="tab-content-profile" class="hidden flex-1 p-8 lg:p-12 overflow-y-auto animate-fade-in">
                <div class="max-w-4xl mx-auto space-y-10">
                    <h3 class="text-3xl font-black text-slate-900 tracking-tighter text-center">성적 및 진로 데이터 설정</h3>
                    <div class="grid grid-cols-1 md:grid-cols-2 gap-8">
                        <div class="bg-white p-8 rounded-3xl border border-slate-200 space-y-6">
                            <h4 class="font-black text-lg flex items-center gap-2 text-indigo-600"><i data-lucide="target" size="20"></i> 희망 진로 설정</h4>
                            <div class="space-y-4">
                                <select id="select-field" onchange="updateMajors()" class="w-full p-4 bg-slate-50 border rounded-2xl font-bold">
                                    <option value="">계열 선택</option>
                                    <option value="공학계열">공학계열</option>
                                    <option value="의학계열">의학계열</option>
                                    <option value="인문/사회">인문/사회</option>
                                    <option value="자연과학">자연과학</option>
                                </select>
                                <select id="select-major" class="w-full p-4 bg-slate-50 border rounded-2xl font-bold">
                                    <option value="">학과 선택</option>
                                </select>
                            </div>
                        </div>
                        <div class="bg-white p-8 rounded-3xl border border-slate-200 space-y-6">
                            <h4 class="font-black text-lg flex items-center gap-2 text-emerald-600"><i data-lucide="trending-up" size="20"></i> 현재 성적 지표</h4>
                            <div class="space-y-4">
                                <input id="input-gpa" type="number" step="0.1" placeholder="내신 평균 등급 (예: 1.5)" class="w-full p-4 bg-slate-50 border rounded-2xl font-bold">
                                <input id="input-mock" type="number" placeholder="모평 백분위 (예: 96)" class="w-full p-4 bg-slate-50 border rounded-2xl font-bold">
                                <input id="input-eng" type="text" placeholder="공인영어성적 (예: TOEFL 105)" class="w-full p-4 bg-slate-50 border rounded-2xl font-bold">
                            </div>
                        </div>
                    </div>
                    <button onclick="switchTab('editor')" class="w-full bg-indigo-600 text-white py-5 rounded-3xl font-black shadow-2xl hover:scale-[1.01] transition-all">설정 완료 및 분석 시작</button>
                </div>
            </main>
        </div>
    </div>

    <script>
        const apiKey = ""; 
        const LOGIN_CREDENTIALS = { id: 'admin', pw: '1234' };
        
        const MAJORS_BY_FIELD = {
            "공학계열": ["컴퓨터공학", "AI공학", "기계공학", "전기전자", "화학공학", "데이터사이언스"],
            "의학계열": ["의예과", "치의예", "약학", "간호학", "수의예"],
            "인문/사회": ["경영학", "경제학", "심리학", "정치외교", "미디어학", "교육학"],
            "자연과학": ["물리학", "생명과학", "수학", "천문학"]
        };

        window.onload = () => {
            lucide.createIcons();
            document.getElementById('input-content').addEventListener('input', (e) => {
                document.getElementById('char-count').innerText = `현재 글자 수: ${e.target.value.length}자`;
            });
        };

        function handleLogin() {
            const id = document.getElementById('login-id').value;
            const pw = document.getElementById('login-pw').value;
            if (id === LOGIN_CREDENTIALS.id && pw === LOGIN_CREDENTIALS.pw) {
                document.getElementById('login-screen').classList.add('hidden');
                document.getElementById('main-screen').classList.remove('hidden');
                lucide.createIcons();
            } else {
                const err = document.getElementById('login-error');
                err.classList.remove('hidden');
            }
        }

        function logout() {
            location.reload();
        }

        function switchTab(tab) {
            document.getElementById('tab-content-editor').classList.toggle('hidden', tab !== 'editor');
            document.getElementById('tab-content-profile').classList.toggle('hidden', tab !== 'profile');
            
            const btnEditor = document.getElementById('tab-editor');
            const btnProfile = document.getElementById('tab-profile');
            
            if (tab === 'editor') {
                btnEditor.className = "px-4 py-2 rounded-xl text-sm font-black transition-all bg-indigo-600 text-white shadow-lg";
                btnProfile.className = "px-4 py-2 rounded-xl text-sm font-black transition-all text-slate-400 hover:bg-slate-100";
                
                const major = document.getElementById('select-major').value;
                const gpa = document.getElementById('input-gpa').value;
                document.getElementById('badge-major').innerText = `🎯 희망학과: ${major || '미설정'}`;
                document.getElementById('badge-gpa').innerText = `📊 내신: ${gpa || '-'}등급`;
            } else {
                btnProfile.className = "px-4 py-2 rounded-xl text-sm font-black transition-all bg-indigo-600 text-white shadow-lg";
                btnEditor.className = "px-4 py-2 rounded-xl text-sm font-black transition-all text-slate-400 hover:bg-slate-100";
            }
        }

        function updateMajors() {
            const field = document.getElementById('select-field').value;
            const selectMajor = document.getElementById('select-major');
            selectMajor.innerHTML = '<option value="">학과 선택</option>';
            if (field && MAJORS_BY_FIELD[field]) {
                MAJORS_BY_FIELD[field].forEach(m => {
                    const opt = document.createElement('option');
                    opt.value = m;
                    opt.innerText = m;
                    selectMajor.appendChild(opt);
                });
            }
        }

        async function analyzeContent() {
            const content = document.getElementById('input-content').value.trim();
            const major = document.getElementById('select-major').value;
            const gpa = document.getElementById('input-gpa').value || "미입력";
            const mock = document.getElementById('input-mock').value || "미입력";
            const eng = document.getElementById('input-eng').value || "없음";

            if (!content || !major) {
                alert('학생부 내용과 희망학과를 모두 입력해주세요.');
                return;
            }

            const btn = document.getElementById('btn-analyze');
            const text = document.getElementById('text-analyze');
            btn.disabled = true;
            text.innerText = "AI 정밀 분석 중...";
            
            try {
                const systemPrompt = `당신은 입시 컨설턴트입니다. 다음 구조의 JSON만 반환하세요: 
                {
                  "suggestions": [{"original": "문장", "improved": "개선문장", "reason": "이유"}],
                  "alignment": {"score": 85, "aiRecommendations": ["주제1", "주제2"]},
                  "domesticPlan": {
                    "challenge": {"name": "서울대학교", "type": "학종", "strategy": "적극추천"},
                    "target": {"name": "연세대학교", "type": "학종", "strategy": "도전가능"},
                    "safety": {"name": "고려대학교", "type": "교과", "strategy": "안정권"}
                  },
                  "globalPlan": {
                    "challenge": {"name": "Stanford", "location": "USA", "tuition": "$50k"},
                    "target": {"name": "UC Berkeley", "location": "USA", "tuition": "$45k"},
                    "safety": {"name": "Purdue", "location": "USA", "tuition": "$30k"}
                  },
                  "globalRequirements": ["TOEFL 100", "SAT 1500"],
                  "roadmap": ["진로 탐구 보강", "심화 보고서 작성"]
                }`;
                
                const userQuery = `내용: ${content}, 학과: ${major}, 내신: ${gpa}, 모평: ${mock}, 영어: ${eng}`;

                const response = await fetch(`https://generativelanguage.googleapis.com/v1beta/models/gemini-2.5-flash-preview-09-2025:generateContent?key=${apiKey}`, {
                    method: 'POST',
                    headers: { 'Content-Type': 'application/json' },
                    body: JSON.stringify({
                        contents: [{ parts: [{ text: userQuery }] }],
                        systemInstruction: { parts: [{ text: systemPrompt }] },
                        generationConfig: { responseMimeType: "application/json" }
                    })
                });

                const data = await response.json();
                if (!data.candidates || data.candidates.length === 0) throw new Error("No data");
                
                const resultText = data.candidates[0].content.parts[0].text;
                const result = JSON.parse(resultText);
                
                renderResults(result);
            } catch (err) {
                console.error(err);
                alert('분석 데이터를 처리하는 중 오류가 발생했습니다. 잠시 후 다시 시도해 주세요.');
            } finally {
                btn.disabled = false;
                text.innerText = "AI 정밀 분석 시작";
                lucide.createIcons();
            }
        }

        function renderResults(data) {
            const resultsArea = document.getElementById('results-area');
            const left = document.getElementById('analysis-left');
            const right = document.getElementById('analysis-right');
            
            resultsArea.classList.remove('hidden');

            // 좌측: 문장 개선 및 전공 적합성
            left.innerHTML = `
                <div class="bg-white p-8 rounded-3xl border border-slate-200 shadow-sm animate-fade-in">
                    <h4 class="text-xl font-black mb-6 flex items-center gap-2 text-indigo-600"><i data-lucide="sparkles"></i> AI 문장 고도화 제안</h4>
                    <div class="space-y-6">
                        ${(data.suggestions || []).length > 0 ? data.suggestions.map(s => `
                            <div class="p-5 bg-slate-50 rounded-2xl border border-slate-100">
                                <p class="text-xs text-slate-400 mb-2 font-bold italic line-through">기존: "${s.original || '내용 없음'}"</p>
                                <p class="text-base font-black text-indigo-900 mb-2">변경: "${s.improved || '개선 중...'}"</p>
                                <p class="text-[11px] text-slate-500 font-bold bg-white p-2 rounded-lg inline-block border border-slate-100">이유: ${s.reason || '전공 적합성 강화'}</p>
                            </div>
                        `).join('') : '<p class="text-center text-slate-400 font-bold py-10">개선 제안 항목이 없습니다.</p>'}
                    </div>
                </div>
                <div class="bg-white p-8 rounded-3xl border border-slate-200 shadow-sm animate-fade-in">
                    <h4 class="text-xl font-black mb-6 flex items-center gap-2 text-red-500"><i data-lucide="target"></i> 전공 적합성 분석</h4>
                    <div class="flex items-center gap-6 mb-6">
                        <div class="text-4xl font-black text-indigo-600">${data.alignment?.score || 0}%</div>
                        <div class="h-2 flex-1 bg-slate-100 rounded-full overflow-hidden">
                            <div class="h-full bg-indigo-600 transition-all duration-1000" style="width: ${data.alignment?.score || 0}%"></div>
                        </div>
                    </div>
                    <div class="grid grid-cols-1 gap-3">
                        ${(data.alignment?.aiRecommendations || []).map(rec => `<div class="p-4 bg-indigo-50 border border-indigo-100 rounded-xl text-xs font-bold text-indigo-900">✨ ${rec}</div>`).join('')}
                    </div>
                </div>
            `;

            // 우측: 국내/해외 대학 추천
            right.innerHTML = `
                <div class="bg-white p-8 rounded-3xl border border-slate-200 shadow-xl animate-fade-in">
                    <h4 class="text-xl font-black mb-6 flex items-center gap-2 text-indigo-600"><i data-lucide="school"></i> 국내 대학 라인업</h4>
                    <div class="space-y-4">
                        ${renderUni('상향', data.domesticPlan?.challenge || {name: '상향 대학 탐색 중', strategy: '분석 결과 부족', type: '종합'}, 'amber')}
                        ${renderUni('적정', data.domesticPlan?.target || {name: '적정 대학 탐색 중', strategy: '분석 결과 부족', type: '학종'}, 'indigo')}
                        ${renderUni('안정', data.domesticPlan?.safety || {name: '안정 대학 탐색 중', strategy: '분석 결과 부족', type: '교과'}, 'emerald')}
                    </div>
                </div>
                <div class="bg-indigo-900 p-8 rounded-3xl shadow-2xl text-white animate-fade-in">
                    <h4 class="text-xl font-black mb-6 flex items-center gap-2 text-indigo-400"><i data-lucide="globe"></i> GLOBAL 해외 명문대</h4>
                    <div class="space-y-4 mb-8">
                        ${renderGlobalUni('상향', data.globalPlan?.challenge || {name: 'Elite Univ', tuition: 'TBD', location: 'USA'})}
                        ${renderGlobalUni('적정', data.globalPlan?.target || {name: 'Global Target', tuition: 'TBD', location: 'UK'})}
                        ${renderGlobalUni('안정', data.globalPlan?.safety || {name: 'Global Safety', tuition: 'TBD', location: 'Canada'})}
                    </div>
                    <div class="bg-white/10 p-5 rounded-2xl border border-white/20 space-y-4">
                        <p class="text-[11px] font-black text-indigo-300 flex items-center gap-2"><i data-lucide="languages" size="14"></i> 유학 필수 준비 항목</p>
                        <div class="space-y-3">
                            ${(data.globalRequirements || ['공인 어학 성적 확보', '에세이(Personal Statement)', '교사 추천서']).map(req => `
                                <div class="flex gap-2 items-start">
                                    <i data-lucide="check-circle" size="14" class="mt-0.5 text-indigo-400"></i>
                                    <p class="text-xs font-bold leading-relaxed">${req}</p>
                                </div>
                            `).join('')}
                        </div>
                    </div>
                </div>
            `;
            lucide.createIcons();
            resultsArea.scrollIntoView({ behavior: 'smooth' });
        }

        function renderUni(label, data, color) {
            const colors = {
                amber: 'border-amber-100 bg-amber-50 text-amber-700',
                indigo: 'border-indigo-100 bg-indigo-50 text-indigo-700',
                emerald: 'border-emerald-100 bg-emerald-50 text-emerald-700'
            };
            return `
                <div class="p-4 rounded-2xl border ${colors[color]}">
                    <div class="flex justify-between items-center mb-1">
                        <span class="text-[10px] font-black uppercase tracking-widest">${label}</span>
                        <span class="text-[9px] font-bold px-2 py-0.5 bg-white rounded-md">${data.type || '분석'}</span>
                    </div>
                    <p class="text-sm font-black text-slate-900">${data.name || '추천 대학 없음'}</p>
                    <p class="text-[10px] font-bold opacity-70 mt-1">${data.strategy || '데이터가 충분하지 않습니다.'}</p>
                </div>
            `;
        }

        function renderGlobalUni(label, data) {
            return `
                <div class="p-4 rounded-2xl bg-white/5 border border-white/10 group hover:bg-white/10 transition-all cursor-default">
                    <div class="flex justify-between items-center mb-1">
                        <span class="text-[9px] font-black text-indigo-400 uppercase tracking-widest">${label}</span>
                        <span class="text-[9px] font-bold opacity-50">${data.location || '분석 중'}</span>
                    </div>
                    <p class="text-sm font-black text-white group-hover:text-indigo-300 transition-colors">${data.name || 'Elite Choice'}</p>
                    <p class="text-[10px] font-bold text-indigo-200/60 mt-1">예상 학비: ${data.tuition || '정보 확인 중'}</p>
                </div>
            `;
        }
    </script>
</body>
</html>
