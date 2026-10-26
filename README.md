<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>CrickBox Pro - Ultimate Turf & Corporate Cricket Tournament Manager</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <script src="https://unpkg.com/lucide@latest"></script>
    <script src="https://cdn.jsdelivr.net/npm/@supabase/supabase-js@2"></script>
    <script src="https://checkout.razorpay.com/v1/checkout.js"></script>
    <link href="https://fonts.googleapis.com/css2?family=Plus+Jakarta+Sans:wght@400;500;600;700;800&display=swap" rel="stylesheet">
    <style>
        body { font-family: 'Plus Jakarta Sans', sans-serif; }
        .glass { background: rgba(255, 255, 255, 0.05); backdrop-filter: blur(12px); border: 1px solid rgba(255, 255, 255, 0.1); }
        .glass-card { background: rgba(15, 23, 42, 0.75); backdrop-filter: blur(16px); border: 1px solid rgba(255, 255, 255, 0.08); }
    </style>
</head>
<body class="bg-slate-950 text-slate-100 min-h-screen">
    <div class="max-w-7xl mx-auto p-4 sm:p-6 lg:p-8">
        <header class="flex flex-col md:flex-row items-center justify-between pb-6 border-b border-slate-800 gap-4 mb-8">
            <div class="flex items-center gap-3">
                <div class="bg-gradient-to-tr from-emerald-500 to-teal-400 p-3 rounded-2xl shadow-lg shadow-emerald-500/20">
                    <i data-lucide="trophy" class="w-8 h-8 text-slate-950"></i>
                </div>
                <div>
                    <h1 class="text-2xl sm:text-3xl font-extrabold tracking-tight bg-gradient-to-r from-white via-slate-200 to-emerald-400 bg-clip-text text-transparent">CrickBox Pro</h1>
                    <p class="text-xs sm:text-sm text-slate-400">Turf & Corporate Tournament Operating System</p>
                </div>
            </div>
            <div class="flex items-center gap-3">
                <span id="pro-badge" class="px-3 py-1 bg-amber-500/10 border border-amber-500/30 text-amber-400 text-xs font-bold rounded-full flex items-center gap-1.5">
                    <i data-lucide="sparkles" class="w-3.5 h-3.5"></i> Free Tier
                </span>
                <button onclick="openProModal()" class="px-4 py-2 bg-gradient-to-r from-amber-500 to-orange-500 hover:from-amber-600 hover:to-orange-600 text-slate-950 font-bold text-sm rounded-xl shadow-lg shadow-amber-500/20 transition-all flex items-center gap-2">
                    <i data-lucide="zap" class="w-4 h-4"></i> Upgrade to Pro (₹999)
                </button>
            </div>
        </header>

        <!-- Main App Dashboard -->
        <div class="grid grid-cols-1 lg:grid-cols-3 gap-8">
            <div class="lg:col-span-1 space-y-6">
                <div class="glass-card p-6 rounded-2xl border border-slate-800/80 shadow-xl">
                    <h2 class="text-lg font-bold mb-4 flex items-center gap-2 text-emerald-400">
                        <i data-lucide="users" class="w-5 h-5"></i> Manage Teams
                    </h2>
                    <div class="space-y-3">
                        <input type="text" id="team-name-input" placeholder="Enter Team Name" class="w-full bg-slate-900 border border-slate-700/60 rounded-xl px-4 py-2.5 text-sm focus:outline-none focus:border-emerald-500 text-slate-100">
                        <button onclick="addTeam()" class="w-full py-2.5 bg-emerald-600 hover:bg-emerald-500 text-slate-950 font-bold rounded-xl transition-all shadow-lg shadow-emerald-600/20 text-sm">Add Team</button>
                    </div>
                    <div id="teams-list" class="mt-4 space-y-2 max-h-48 overflow-y-auto">
                        <!-- Populated dynamically -->
                    </div>
                </div>

                <div class="glass-card p-6 rounded-2xl border border-slate-800/80 shadow-xl">
                    <h2 class="text-lg font-bold mb-4 flex items-center gap-2 text-teal-400">
                        <i data-lucide="calendar" class="w-5 h-5"></i> Fixture Generator
                    </h2>
                    <button onclick="generateFixtures()" class="w-full py-3 bg-gradient-to-r from-teal-600 to-cyan-600 hover:from-teal-500 hover:to-cyan-500 text-white font-bold rounded-xl transition-all shadow-lg shadow-teal-600/20 text-sm flex items-center justify-center gap-2">
                        <i data-lucide="shuffle" class="w-4 h-4"></i> Generate Round-Robin Schedule
                    </button>
                </div>
            </div>

            <div class="lg:col-span-2 space-y-6">
                <div class="glass-card p-6 rounded-2xl border border-slate-800/80 shadow-xl">
                    <h2 class="text-lg font-bold mb-4 flex items-center gap-2 text-amber-400">
                        <i data-lucide="award" class="w-5 h-5"></i> Live Points Table & NRR Engine
                    </h2>
                    <div class="overflow-x-auto">
                        <table class="w-full text-left text-sm">
                            <thead class="bg-slate-900/80 text-slate-400 uppercase text-xs">
                                <tr>
                                    <th class="p-3">Team</th>
                                    <th class="p-3 text-center">P</th>
                                    <th class="p-3 text-center">W</th>
                                    <th class="p-3 text-center">L</th>
                                    <th class="p-3 text-center">Pts</th>
                                    <th class="p-3 text-right">NRR</th>
                                </tr>
                            </thead>
                            <tbody id="points-table-body" class="divide-y divide-slate-800/60">
                                <!-- Populated dynamically -->
                            </tbody>
                        </table>
                    </div>
                </div>

                <div class="glass-card p-6 rounded-2xl border border-slate-800/80 shadow-xl">
                    <h2 class="text-lg font-bold mb-4 flex items-center gap-2 text-indigo-400">
                        <i data-lucide="play-circle" class="w-5 h-5"></i> Matches & Score Updates
                    </h2>
                    <div id="matches-list" class="space-y-4">
                        <!-- Populated dynamically -->
                    </div>
                </div>
            </div>
        </div>
    </div>

    <!-- Pro Upgrade Modal -->
    <div id="pro-modal" class="fixed inset-0 bg-slate-950/80 backdrop-blur-md hidden items-center justify-center p-4 z-50">
        <div class="glass-card max-w-md w-full p-6 rounded-3xl border border-amber-500/30 shadow-2xl relative">
            <button onclick="closeProModal()" class="absolute top-4 right-4 text-slate-400 hover:text-white"><i data-lucide="x" class="w-5 h-5"></i></button>
            <div class="text-center space-y-3 mb-6">
                <div class="w-12 h-12 bg-amber-500/20 rounded-2xl flex items-center justify-center mx-auto text-amber-400 border border-amber-500/30">
                    <i data-lucide="crown" class="w-6 h-6"></i>
                </div>
                <h3 class="text-xl font-bold text-white">Unlock CrickBox Pro Pass</h3>
                <p class="text-sm text-slate-400">Get unlimited teams, advanced NRR algorithms, live stream integration, match clip highlights, and cloud sync for just ₹999.</p>
            </div>
            <div class="bg-slate-900/80 p-4 rounded-2xl border border-slate-800 mb-6 space-y-2 text-xs text-slate-300">
                <div class="flex items-center gap-2"><i data-lucide="check" class="w-4 h-4 text-emerald-400"></i> Unlimited Teams & Tournaments</div>
                <div class="flex items-center gap-2"><i data-lucide="check" class="w-4 h-4 text-emerald-400"></i> Real-time Supabase Cloud Sync</div>
                <div class="flex items-center gap-2"><i data-lucide="check" class="w-4 h-4 text-emerald-400"></i> Live Stream Hub & Highlight Reels</div>
            </div>
            <button onclick="payWithRazorpay()" class="w-full py-3.5 bg-gradient-to-r from-amber-500 to-orange-500 hover:from-amber-600 hover:to-orange-600 text-slate-950 font-extrabold rounded-xl shadow-lg shadow-amber-500/20 text-sm">Pay ₹999 & Upgrade Now</button>
        </div>
    </div>

    <script>
        lucide.createIcons();
        let teams = [];
        let matches = [];

        function addTeam() {
            const input = document.getElementById('team-name-input');
            const name = input.value.trim();
            if (!name) return;
            teams.push({ id: Date.now().toString(), name, played: 0, won: 0, lost: 0, points: 0, nrr: 0.000 });
            input.value = '';
            renderTeams();
            renderPointsTable();
        }

        function renderTeams() {
            const list = document.getElementById('teams-list');
            list.innerHTML = teams.map(t => `<div class="flex items-center justify-between p-2.5 bg-slate-900/60 rounded-xl border border-slate-800/60 text-sm"><span class="font-medium text-slate-200">${t.name}</span><button onclick="removeTeam('${t.id}')" class="text-rose-400 hover:text-rose-300 text-xs">Remove</button></div>`).join('');
        }

        function removeTeam(id) {
            teams = teams.filter(t => t.id !== id);
            renderTeams();
            renderPointsTable();
        }

        function generateFixtures() {
            if (teams.length < 2) {
                alert('Add at least 2 teams to generate fixtures.');
                return;
            }
            matches = [];
            for (let i = 0; i < teams.length; i++) {
                for (let j = i + 1; j < teams.length; j++) {
                    matches.push({ id: Math.random().toString(), team1: teams[i], team2: teams[j], status: 'Upcoming', score: 'Yet to start' });
                }
            }
            renderMatches();
        }

        function renderMatches() {
            const container = document.getElementById('matches-list');
            container.innerHTML = matches.map(m => `
                <div class="p-4 bg-slate-900/60 rounded-xl border border-slate-800/80 space-y-3">
                    <div class="flex justify-between items-center text-sm font-semibold">
                        <span class="text-slate-200">${m.team1.name} vs ${m.team2.name}</span>
                        <span class="px-2.5 py-0.5 bg-slate-800 text-slate-300 text-xs rounded-full border border-slate-700">${m.status}</span>
                    </div>
                    <div class="text-xs text-slate-400">Score: ${m.score}</div>
                    <div class="flex gap-2">
                        <button onclick="updateMatchScore('${m.id}')" class="px-3 py-1.5 bg-indigo-600/80 hover:bg-indigo-600 text-white rounded-lg text-xs font-semibold">Update Score</button>
                    </div>
                </div>
            `).join('');
        }

        function updateMatchScore(id) {
            const match = matches.find(m => m.id === id);
            if (!match) return;
            const runs1 = prompt(`Enter runs scored by ${match.team1.name}:`, '120');
            const overs1 = prompt(`Enter overs faced by ${match.team1.name} (e.g. 10):`, '10');
            const runs2 = prompt(`Enter runs scored by ${match.team2.name}:`, '110');
            const overs2 = prompt(`Enter overs faced by ${match.team2.name} (e.g. 9.2):`, '9.2');
            if (runs1 && overs1 && runs2 && overs2) {
                match.status = 'Completed';
                match.score = `${match.team1.name} ${runs1} (${overs1} ov) - ${match.team2.name} ${runs2} (${overs2} ov)`;
                
                match.team1.played += 1;
                match.team2.played += 1;
                if (Number(runs1) > Number(runs2)) {
                    match.team1.won += 1;
                    match.team1.points += 2;
                    match.team2.lost += 1;
                } else {
                    match.team2.won += 1;
                    match.team2.points += 2;
                    match.team1.lost += 1;
                }
                match.team1.nrr = (Number(runs1)/10 - Number(runs2)/10);
                match.team2.nrr = (Number(runs2)/10 - Number(runs1)/10);

                renderMatches();
                renderPointsTable();
            }
        }

        function renderPointsTable() {
            const tbody = document.getElementById('points-table-body');
            tbody.innerHTML = teams.sort((a,b) => b.points - a.points || b.nrr - a.nrr).map(t => `
                <tr class="border-b border-slate-800/40">
                    <td class="p-3 font-medium text-white">${t.name}</td>
                    <td class="p-3 text-center">${t.played}</td>
                    <td class="p-3 text-center">${t.won}</td>
                    <td class="p-3 text-center">${t.lost}</td>
                    <td class="p-3 text-center font-bold text-emerald-400">${t.points}</td>
                    <td class="p-3 text-right font-mono text-slate-300">${t.nrr.toFixed(3)}</td>
                </tr>
            `).join('');
        }

        function openProModal() { document.getElementById('pro-modal').classList.remove('hidden'); document.getElementById('pro-modal').classList.add('flex'); }
        function closeProModal() { document.getElementById('pro-modal').classList.remove('flex'); document.getElementById('pro-modal').classList.add('hidden'); }
        function payWithRazorpay() {
            var options = {
                "key": "rzp_test_mockkey",
                "amount": "99900",
                "currency": "INR",
                "name": "CrickBox Pro",
                "description": "Pro Tournament Pass",
                "handler": function (response){
                    alert('Payment Successful! Pro features unlocked.');
                    document.getElementById('pro-badge').innerHTML = '<i data-lucide="crown" class="w-3.5 h-3.5"></i> PRO PASS ACTIVE';
                    document.getElementById('pro-badge').className = 'px-3 py-1 bg-amber-500/20 border border-amber-500/50 text-amber-400 text-xs font-bold rounded-full flex items-center gap-1.5';
                    closeProModal();
                    lucide.createIcons();
                },
                "theme": { "color": "#f59e0b" }
            };
            var rzp = new Razorpay(options);
            rzp.open();
        }
    </script>
</body>
</html>
