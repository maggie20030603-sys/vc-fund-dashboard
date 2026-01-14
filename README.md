# vc-fund-dashboard
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Fund Investment Dashboard - Obsidian Sensors</title>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/Chart.js/3.9.1/chart.min.js"></script>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }
        
        body {
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            min-height: 100vh;
            padding: 20px;
        }
        
        .container {
            max-width: 1400px;
            margin: 0 auto;
        }
        
        header {
            background: white;
            padding: 30px;
            border-radius: 12px;
            margin-bottom: 30px;
            box-shadow: 0 4px 6px rgba(0,0,0,0.1);
        }
        
        h1 {
            color: #333;
            margin-bottom: 10px;
            font-size: 32px;
        }
        
        .subtitle {
            color: #666;
            font-size: 14px;
        }
        
        .metrics-grid {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(250px, 1fr));
            gap: 20px;
            margin-bottom: 30px;
        }
        
        .metric-card {
            background: white;
            padding: 25px;
            border-radius: 12px;
            box-shadow: 0 4px 6px rgba(0,0,0,0.1);
            border-left: 5px solid #667eea;
        }
        
        .metric-card.dpi {
            border-left-color: #10b981;
        }
        
        .metric-card.tvpi {
            border-left-color: #f59e0b;
        }
        
        .metric-card.irr {
            border-left-color: #ef4444;
        }
        
        .metric-label {
            font-size: 12px;
            color: #999;
            text-transform: uppercase;
            letter-spacing: 1px;
            margin-bottom: 8px;
        }
        
        .metric-value {
            font-size: 36px;
            font-weight: bold;
            color: #333;
            margin-bottom: 5px;
        }
        
        .metric-desc {
            font-size: 13px;
            color: #666;
            line-height: 1.4;
        }
        
        .charts-grid {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(450px, 1fr));
            gap: 20px;
            margin-bottom: 30px;
        }
        
        .chart-card {
            background: white;
            padding: 25px;
            border-radius: 12px;
            box-shadow: 0 4px 6px rgba(0,0,0,0.1);
        }
        
        .chart-title {
            font-size: 16px;
            font-weight: 600;
            margin-bottom: 20px;
            color: #333;
        }
        
        .table-card {
            background: white;
            padding: 25px;
            border-radius: 12px;
            box-shadow: 0 4px 6px rgba(0,0,0,0.1);
            overflow-x: auto;
        }
        
        table {
            width: 100%;
            border-collapse: collapse;
            font-size: 13px;
        }
        
        th {
            background: #f5f5f5;
            padding: 12px;
            text-align: left;
            font-weight: 600;
            color: #333;
            border-bottom: 2px solid #ddd;
        }
        
        td {
            padding: 12px;
            border-bottom: 1px solid #eee;
            color: #666;
        }
        
        tr:hover {
            background: #f9f9f9;
        }
        
        .round-label {
            font-weight: 600;
            color: #667eea;
        }
        
        .amount {
            color: #10b981;
            font-weight: 500;
        }
        
        .valuation {
            color: #667eea;
            font-weight: 500;
        }
        
        .input-group {
            background: white;
            padding: 25px;
            border-radius: 12px;
            box-shadow: 0 4px 6px rgba(0,0,0,0.1);
            margin-bottom: 30px;
        }
        
        .input-group h3 {
            margin-bottom: 15px;
            color: #333;
            font-size: 16px;
        }
        
        .form-row {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
            gap: 15px;
            margin-bottom: 15px;
        }
        
        .form-group {
            display: flex;
            flex-direction: column;
        }
        
        label {
            font-size: 12px;
            font-weight: 600;
            color: #666;
            margin-bottom: 5px;
            text-transform: uppercase;
        }
        
        input, select {
            padding: 10px;
            border: 1px solid #ddd;
            border-radius: 6px;
            font-size: 14px;
            font-family: inherit;
        }
        
        input:focus, select:focus {
            outline: none;
            border-color: #667eea;
            box-shadow: 0 0 0 3px rgba(102, 126, 234, 0.1);
        }
        
        button {
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            color: white;
            padding: 12px 30px;
            border: none;
            border-radius: 6px;
            font-weight: 600;
            cursor: pointer;
            font-size: 14px;
            transition: transform 0.2s, box-shadow 0.2s;
        }
        
        button:hover {
            transform: translateY(-2px);
            box-shadow: 0 4px 12px rgba(102, 126, 234, 0.4);
        }
        
        .legend {
            display: flex;
            gap: 20px;
            margin-top: 15px;
            font-size: 12px;
            flex-wrap: wrap;
        }
        
        .legend-item {
            display: flex;
            align-items: center;
            gap: 5px;
        }
        
        .legend-color {
            width: 12px;
            height: 12px;
            border-radius: 2px;
        }
    </style>
</head>
<body>
    <div class="container">
        <header>
            <h1>🚀 Fund Investment Dashboard</h1>
            <p class="subtitle">Obsidian Sensors - VC Performance Metrics (DPI, TVPI, IRR)</p>
        </header>
        
        <div class="input-group">
            <h3>📊 Scenario Simulation - Input Your Assumptions</h3>
            <div class="form-row">
                <div class="form-group">
                    <label>Exit Valuation ($M)</label>
                    <input type="number" id="exitValuation" value="200" min="0">
                </div>
                <div class="form-group">
                    <label>LP's Total Capital Committed ($M)</label>
                    <input type="number" id="lpCapital" value="19.03" step="0.01" min="0">
                </div>
                <div class="form-group">
                    <label>Current Interim Distributions ($M)</label>
                    <input type="number" id="currentDistributions" value="0" min="0">
                </div>
                <div class="form-group">
                    <label>Investment Start Year</label>
                    <input type="number" id="startYear" value="2018" min="2000" max="2026">
                </div>
                <div class="form-group">
                    <label>Exit Year (Assumption)</label>
                    <input type="number" id="exitYear" value="2027" min="2000" max="2050">
                </div>
            </div>
            <button onclick="calculateMetrics()">Calculate Metrics</button>
        </div>
        
        <div class="metrics-grid">
            <div class="metric-card dpi">
                <div class="metric-label">DPI (Distribution to Paid-in)</div>
                <div class="metric-value" id="dpiValue">0.00x</div>
                <div class="metric-desc">已實現分配 / 實際投入資本<br><strong>Score:</strong> <span id="dpiScore">--</span></div>
            </div>
            <div class="metric-card tvpi">
                <div class="metric-label">TVPI (Total Value to Paid-in)</div>
                <div class="metric-value" id="tvpiValue">0.00x</div>
                <div class="metric-desc">總價值 / 實際投入資本<br><strong>Score:</strong> <span id="tvpiScore">--</span></div>
            </div>
            <div class="metric-card irr">
                <div class="metric-label">IRR (Internal Rate of Return)</div>
                <div class="metric-value" id="irrValue">0.00%</div>
                <div class="metric-desc">年化報酬率<br><strong>Score:</strong> <span id="irrScore">--</span></div>
            </div>
        </div>
        
        <div class="charts-grid">
            <div class="chart-card">
                <div class="chart-title">💰 Capital Raised Over Time</div>
                <canvas id="capitalChart"></canvas>
                <div class="legend">
                    <div class="legend-item">
                        <div class="legend-color" style="background: #667eea;"></div>
                        <span>Cumulative Capital</span>
                    </div>
                    <div class="legend-item">
                        <div class="legend-color" style="background: #10b981;"></div>
                        <span>Round Amount</span>
                    </div>
                </div>
            </div>
            
            <div class="chart-card">
                <div class="chart-title">📈 Valuation Growth</div>
                <canvas id="valuationChart"></canvas>
                <div class="legend">
                    <div class="legend-item">
                        <div class="legend-color" style="background: #f59e0b;"></div>
                        <span>Post-Val</span>
                    </div>
                    <div class="legend-item">
                        <div class="legend-color" style="background: #ef4444;"></div>
                        <span>Pre-Val</span>
                    </div>
                </div>
            </div>
            
            <div class="chart-card">
                <div class="chart-title">🎯 DPI, TVPI, IRR Scenarios</div>
                <canvas id="metricsChart"></canvas>
                <div class="legend">
                    <div class="legend-item">
                        <div class="legend-color" style="background: #10b981;"></div>
                        <span>Conservative Exit ($120M)</span>
                    </div>
                    <div class="legend-item">
                        <div class="legend-color" style="background: #f59e0b;"></div>
                        <span>Base Case ($200M)</span>
                    </div>
                    <div class="legend-item">
                        <div class="legend-color" style="background: #ef4444;"></div>
                        <span>Upside ($400M)</span>
                    </div>
                </div>
            </div>
        </div>
        
        <div class="table-card">
            <div class="chart-title">📋 Obsidian Sensors - Financing Rounds Summary</div>
            <table>
                <thead>
                    <tr>
                        <th>Round #</th>
                        <th>Type</th>
                        <th>Date</th>
                        <th>Amount ($M)</th>
                        <th>Pre-Val ($M)</th>
                        <th>Post-Val ($M)</th>
                        <th>Cumulative ($M)</th>
                    </tr>
                </thead>
                <tbody id="roundsTableBody">
                    <!-- JS will inject rows -->
                </tbody>
            </table>
        </div>
        
        <div class="table-card" style="margin-top: 30px;">
            <div class="chart-title">📌 Key Metrics Explanation</div>
            <table>
                <thead>
                    <tr>
                        <th>Metric</th>
                        <th>Formula</th>
                        <th>Interpretation</th>
                    </tr>
                </thead>
                <tbody>
                    <tr>
                        <td><strong>DPI</strong> (Distribution to Paid-in)</td>
                        <td>已分配現金 / 實際投入資本</td>
                        <td>已實現的現金回報倍數。DPI=1 代表回本，>1 代表獲利已落袋。只看已確實分配的部分，最實在的指標。</td>
                    </tr>
                    <tr>
                        <td><strong>TVPI</strong> (Total Value to Paid-in)</td>
                        <td>(已分配現金 + 投資目前價值) / 實際投入資本</td>
                        <td>總價值倍數（含帳面未實現），反映未來潛力。TVPI=2 代表投資有 2 倍回報潛力。</td>
                    </tr>
                    <tr>
                        <td><strong>IRR</strong> (Internal Rate of Return)</td>
                        <td>使現金流折現值=0 的年化折現率</td>
                        <td>年化報酬率。VC 基金通常目標 25-35% IRR。反映投資的時間價值和複利效果。</td>
                    </tr>
                </tbody>
            </table>
        </div>
    </div>
    
    <script>
        // 通用輪次資料：換公司只要改這裡
        let rounds = [
            { date: '2017-07-25', type: 'Accelerator/Incubator', amount: 0,    pre: null,  post: null },
            { date: '2018-07-06', type: 'Seed (Series A)',        amount: 8.6, pre: 29.4,  post: 38.0 },
            { date: '2019-09-25', type: 'Early Stage VC',         amount: 2.53,pre: 44.7,  post: 47.2 },
            { date: '2020-07-10', type: 'Grant (DoD)',            amount: 1.6, pre: null,  post: null },
            { date: '2020-05-01', type: 'Grant (NSF)',            amount: 0,   pre: null,  post: null },
            { date: '2022-07-07', type: 'Later Stage VC',         amount: 3.0, pre: null,  post: 47.2 },
            { date: '2022-06-27', type: 'Later Stage VC',         amount: 3.0, pre: null,  post: null },
            { date: '2024-10-01', type: 'Later Stage VC',         amount: 1.9, pre: 48.1,  post: 50.0 }
        ];

        let capitalChart, valuationChart, metricsChart;
        
        function calculateMetrics() {
            const exitValuation = parseFloat(document.getElementById('exitValuation').value) || 200;
            const lpCapital = parseFloat(document.getElementById('lpCapital').value) || 19.03;
            const currentDistributions = parseFloat(document.getElementById('currentDistributions').value) || 0;
            const startYear = parseInt(document.getElementById('startYear').value) || 2018;
            const exitYear = parseInt(document.getElementById('exitYear').value) || 2027;
            
            const years = exitYear - startYear;
            
            // DPI = Distributions / Paid-in Capital
            const dpi = currentDistributions / lpCapital;
            
            // TVPI = (Distributions + Current Value) / Paid-in Capital
            const tvpi = (currentDistributions + exitValuation) / lpCapital;
            
            // IRR calculation using simplified method
            const irr = calculateIRR(lpCapital, exitValuation, years);
            
            // Display metrics
            document.getElementById('dpiValue').textContent = dpi.toFixed(2) + 'x';
            document.getElementById('tvpiValue').textContent = tvpi.toFixed(2) + 'x';
            document.getElementById('irrValue').textContent = (irr * 100).toFixed(2) + '%';
            
            // Score interpretation
            document.getElementById('dpiScore').textContent = scoreDPI(dpi);
            document.getElementById('tvpiScore').textContent = scoreTVPI(tvpi);
            document.getElementById('irrScore').textContent = scoreIRR(irr);
            
            // Update charts
            updateCharts(exitValuation, lpCapital);
        }
        
        function calculateIRR(investment, exitValue, years) {
            // Simplified IRR: (exitValue / investment) ^ (1/years) - 1
            if (years <= 0 || investment <= 0) return 0;
            return Math.pow(exitValue / investment, 1 / years) - 1;
        }
        
        function scoreDPI(dpi) {
            if (dpi >= 1) return '✅ Realized (落袋為安)';
            return '⏳ Pending Exit';
        }
        
        function scoreTVPI(tvpi) {
            if (tvpi >= 3) return '🌟 Excellent (>3x)';
            if (tvpi >= 2) return '👍 Good (2-3x)';
            if (tvpi >= 1.5) return '📈 Fair (1.5-2x)';
            return '⚠️ Below Benchmark';
        }
        
        function scoreIRR(irr) {
            const irrPercent = irr * 100;
            if (irrPercent >= 35) return '🚀 Excellent (>35%)';
            if (irrPercent >= 25) return '👍 Good (25-35%)';
            if (irrPercent >= 15) return '📊 Fair (15-25%)';
            return '⚠️ Below Target';
        }

        function formatDateDisplay(iso) {
            if (!iso) return '-';
            const d = new Date(iso);
            if (isNaN(d)) return iso;
            const day = String(d.getDate()).padStart(2, '0');
            const monthNames = ['Jan','Feb','Mar','Apr','May','Jun','Jul','Aug','Sep','Oct','Nov','Dec'];
            const mon = monthNames[d.getMonth()];
            const year = d.getFullYear();
            return `${day}-${mon}-${year}`;
        }

        function renderRoundsTable() {
            const tbody = document.getElementById('roundsTableBody');
            tbody.innerHTML = '';

            const sorted = [...rounds].sort((a, b) => new Date(a.date) - new Date(b.date));
            let cumulative = 0;

            sorted.forEach((r, index) => {
                const tr = document.createElement('tr');
                const amount = r.amount || 0;
                cumulative += amount;

                tr.innerHTML = `
                    <td class="round-label">${index + 1}</td>
                    <td>${r.type}</td>
                    <td>${formatDateDisplay(r.date)}</td>
                    <td class="amount">${amount ? amount.toFixed(2) : '-'}</td>
                    <td class="valuation">${r.pre ?? '-'}</td>
                    <td class="valuation">${r.post ?? '-'}</td>
                    <td class="amount">${cumulative.toFixed(2)}</td>
                `;

                tbody.appendChild(tr);
            });
        }

        function buildCapitalDataFromRounds(rounds) {
            const sorted = [...rounds].sort((a, b) => new Date(a.date) - new Date(b.date));
            const labels = [];
            const cumulative = [];
            const roundAmounts = [];
            let cum = 0;

            sorted.forEach(r => {
                const year = new Date(r.date).getFullYear();
                labels.push(String(year));
                const amt = r.amount || 0;
                cum += amt;
                roundAmounts.push(amt);
                cumulative.push(cum);
            });

            return { labels, cumulative, roundAmounts };
        }

        function buildValuationDataFromRounds(rounds) {
            const sorted = [...rounds].sort((a, b) => new Date(a.date) - new Date(b.date));
            const labels = [];
            const preVal = [];
            const postVal = [];

            sorted.forEach(r => {
                if (r.pre != null || r.post != null) {
                    const year = new Date(r.date).getFullYear();
                    labels.push(String(year));
                    preVal.push(r.pre ?? null);
                    postVal.push(r.post ?? null);
                }
            });

            return { labels, preVal, postVal };
        }
        
        function updateCharts(exitValuation, lpCapital) {
            // Capital raised data from rounds
            const capitalData = buildCapitalDataFromRounds(rounds);
            
            if (capitalChart) capitalChart.destroy();
            capitalChart = new Chart(document.getElementById('capitalChart'), {
                type: 'line',
                data: {
                    labels: capitalData.labels,
                    datasets: [
                        {
                            label: 'Cumulative Capital ($M)',
                            data: capitalData.cumulative,
                            borderColor: '#667eea',
                            backgroundColor: 'rgba(102, 126, 234, 0.1)',
                            borderWidth: 3,
                            fill: true,
                            tension: 0.4,
                            pointRadius: 5,
                            pointBackgroundColor: '#667eea',
                            pointBorderColor: 'white',
                            pointBorderWidth: 2,
                            yAxisID: 'y'
                        },
                        {
                            label: 'Round Amount ($M)',
                            data: capitalData.roundAmounts,
                            borderColor: '#10b981',
                            type: 'bar',
                            backgroundColor: 'rgba(16, 185, 129, 0.3)',
                            yAxisID: 'y'
                        }
                    ]
                },
                options: {
                    responsive: true,
                    maintainAspectRatio: true,
                    plugins: {
                        legend: { position: 'bottom', labels: { padding: 15, font: { size: 12 } } }
                    },
                    scales: {
                        y: {
                            type: 'linear',
                            position: 'left',
                            title: { display: true, text: 'Amount ($M)' }
                        }
                    }
                }
            });
            
            // Valuation growth from rounds
            const valuationData = buildValuationDataFromRounds(rounds);
            
            if (valuationChart) valuationChart.destroy();
            valuationChart = new Chart(document.getElementById('valuationChart'), {
                type: 'line',
                data: {
                    labels: valuationData.labels,
                    datasets: [
                        {
                            label: 'Post-Valuation ($M)',
                            data: valuationData.postVal,
                            borderColor: '#f59e0b',
                            backgroundColor: 'rgba(245, 158, 11, 0.1)',
                            borderWidth: 3,
                            fill: true,
                            tension: 0.4,
                            pointRadius: 5,
                            pointBackgroundColor: '#f59e0b'
                        },
                        {
                            label: 'Pre-Valuation ($M)',
                            data: valuationData.preVal,
                            borderColor: '#ef4444',
                            backgroundColor: 'rgba(239, 68, 68, 0.1)',
                            borderWidth: 3,
                            fill: false,
                            tension: 0.4,
                            pointRadius: 5,
                            pointBackgroundColor: '#ef4444'
                        }
                    ]
                },
                options: {
                    responsive: true,
                    maintainAspectRatio: true,
                    plugins: {
                        legend: { position: 'bottom', labels: { padding: 15, font: { size: 12 } } }
                    },
                    scales: {
                        y: {
                            title: { display: true, text: 'Valuation ($M)' }
                        }
                    }
                }
            });
            
            // Scenario comparison (先沿用你的 3 個情境，年數先固定 9 年)
            const scenarios = [
                { name: 'Conservative\n($120M)', exit: 120 },
                { name: 'Base Case\n($200M)', exit: 200 },
                { name: 'Upside\n($400M)', exit: 400 }
            ];
            
            const tvpiScenarios = scenarios.map(s => ((0 + s.exit) / lpCapital).toFixed(2));
            const irrScenarios = scenarios.map(s => (calculateIRR(lpCapital, s.exit, 9) * 100).toFixed(2));
            
            if (metricsChart) metricsChart.destroy();
            metricsChart = new Chart(document.getElementById('metricsChart'), {
                type: 'bar',
                data: {
                    labels: scenarios.map(s => s.name),
                    datasets: [
                        {
                            label: 'TVPI (倍數)',
                            data: tvpiScenarios,
                            backgroundColor: ['#10b981', '#f59e0b', '#ef4444'],
                            yAxisID: 'y'
                        },
                        {
                            label: 'IRR (%)',
                            data: irrScenarios,
                            borderColor: ['#10b981', '#f59e0b', '#ef4444'],
                            borderWidth: 2,
                            type: 'line',
                            yAxisID: 'y1',
                            pointRadius: 6,
                            pointBackgroundColor: ['#10b981', '#f59e0b', '#ef4444']
                        }
                    ]
                },
                options: {
                    responsive: true,
                    maintainAspectRatio: true,
                    plugins: {
                        legend: { position: 'bottom', labels: { padding: 15, font: { size: 12 } } }
                    },
                    scales: {
                        y: {
                            type: 'linear',
                            position: 'left',
                            title: { display: true, text: 'TVPI (倍數)' }
                        },
                        y1: {
                            type: 'linear',
                            position: 'right',
                            title: { display: true, text: 'IRR (%)' },
                            grid: { drawOnChartArea: false }
                        }
                    }
                }
            });
        }
        
        // Initialize on load
        renderRoundsTable();
        calculateMetrics();
    </script>
</body>
</html>
