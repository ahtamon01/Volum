<!DOCTYPE html>
<html lang="ro">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Calculator Siloz - 9 Celule</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            margin: 20px;
            background-color: #f0f2f5;
        }
        .container {
            display: flex;
            flex-wrap: wrap;
            gap: 20px;
            justify-content: center;
        }
        .siloz-container {
            background-color: white;
            border-radius: 10px;
            padding: 20px;
            width: 300px;
            box-shadow: 0 2px 4px rgba(0,0,0,0.1);
        }
        .siloz-header {
            background-color: #2c3e50;
            color: white;
            padding: 10px;
            border-radius: 8px;
            text-align: center;
            margin-bottom: 15px;
        }
        .input-group {
            margin: 10px 0;
        }
        .input-group label {
            display: block;
            margin-bottom: 5px;
            font-weight: bold;
            color: #34495e;
        }
        .input-group input, .input-group select {
            width: 100%;
            padding: 8px;
            border: 1px solid #bdc3c7;
            border-radius: 6px;
            font-size: 14px;
        }
        .progress-container {
            text-align: center;
            margin: 15px 0;
        }
        .progress-container svg {
            display: block;
            margin: 0 auto;
        }
        .progress-labels {
            display: flex;
            justify-content: space-between;
            margin-top: 5px;
            font-weight: bold;
        }
        .progress-labels .ocupat {
            color: #e67e22;
        }
        .progress-labels .liber {
            color: #27ae60;
        }
        .results {
            margin-top: 10px;
            font-size: 14px;
        }
        .results p {
            margin: 5px 0;
            font-weight: bold;
            color: #2c3e50;
        }
        .results .value {
            color: #2980b9;
        }
    </style>
</head>
<body>
    <div class="container">
        <!-- Cele 9 silozuri -->
        <script>
            for (let i = 1; i <= 9; i++) {
                document.write(`
                    <div class="siloz-container">
                        <div class="siloz-header">
                            <h3>Siloz #${i}</h3>
                        </div>
                        <div class="input-group">
                            <label>Rază cilindru (m):</label>
                            <input type="number" id="razaCilindru${i}" value="11.17" onchange="calculeazaSiloz(${i})">
                        </div>
                        <div class="input-group">
                            <label>Înălțime cilindru (m):</label>
                            <input type="number" id="inaltimeCilindru${i}" value="16.40" onchange="calculeazaSiloz(${i})">
                        </div>
                        <div class="input-group">
                            <label>Rază con (m):</label>
                            <input type="number" id="razaCon${i}" value="11.17" onchange="calculeazaSiloz(${i})">
                        </div>
                        <div class="input-group">
                            <label>Înălțime con (m):</label>
                            <input type="number" id="inaltimeCon${i}" value="6.50" onchange="calculeazaSiloz(${i})">
                        </div>
                        <div class="input-group">
                            <label>Tip Produs:</label>
                            <select id="tipProdus${i}" onchange="calculeazaSiloz(${i})">
                                <option value="grau">Grâu</option>
                                <option value="porumb">Porumb</option>
                                <option value="rapita">Rapiță</option>
                                <option value="orz">Orz</option>
                                <option value="floarea-soarelui">Floarea Soarelui</option>
                            </select>
                        </div>
                        <div class="input-group">
                            <label>Greutate Hectolitrică (kg/hl):</label>
                            <input type="number" id="greutateHL${i}" value="0" onchange="calculeazaSiloz(${i})">
                        </div>
                        <div class="input-group">
                            <label>Spațiu Gol Măsurat (m):</label>
                            <input type="number" id="spatiuGol${i}" value="0" onchange="calculeazaSiloz(${i})">
                        </div>
                        <div class="progress-container">
                            <svg width="200" height="120" viewBox="0 0 300 180">
                                <path id="baseArc${i}" d="M 20,150 A 130,130 0 0,1 280,150"
                                    fill="none" stroke="#2ecc71" stroke-width="20"/>
                                <path id="progressArc${i}" d="M 20,150 A 130,130 0 0,1 280,150"
                                    fill="none" stroke="#f1c40f" stroke-width="20" stroke-dasharray="0 410"/>
                                <text x="150" y="80" text-anchor="middle" font-size="18" font-weight="bold" id="percentageText${i}">0%</text>
                            </svg>
                            <div class="progress-labels">
                                <span class="ocupat" id="ocupatText${i}">Ocupat: 0 t</span>
                                <span class="liber" id="liberText${i}">Liber: 0 t</span>
                            </div>
                        </div>
                        <div class="results">
                            <p>Volum Total: <span id="volumTotal${i}" class="value">0</span> m³</p>
                            <p>Volum Ocupat: <span id="volumOcupat${i}" class="value">0</span> m³</p>
                            <p>Cantitate: <span id="cantitate${i}" class="value">0</span> tone</p>
                        </div>
                    </div>
                `);
            }
        </script>
    </div>

    <script>
        const CULORI_PRODUSE = {
            'grau': '#f1c40f',
            'porumb': '#e67e22',
            'rapita': '#3498db',
            'orz': '#ff69b4',
            'floarea-soarelui': '#95a5a6'
        };

        function calculeazaSiloz(index) {
            const razaCilindru = parseFloat(document.getElementById(`razaCilindru${index}`).value);
            const inaltimeCilindru = parseFloat(document.getElementById(`inaltimeCilindru${index}`).value);
            const razaCon = parseFloat(document.getElementById(`razaCon${index}`).value);
            const inaltimeCon = parseFloat(document.getElementById(`inaltimeCon${index}`).value);
            const spatiuGol = parseFloat(document.getElementById(`spatiuGol${index}`).value);
            const greutateHL = parseFloat(document.getElementById(`greutateHL${index}`).value);
            const tipProdus = document.getElementById(`tipProdus${index}`).value;

            const volumCilindru = Math.PI * Math.pow(razaCilindru, 2) * inaltimeCilindru;
            const volumCon = (1/3) * Math.PI * Math.pow(razaCon, 2) * inaltimeCon;
            const volumTotal = volumCilindru + volumCon;
            const volumGol = Math.PI * Math.pow(razaCilindru, 2) * spatiuGol;
            const volumOcupat = volumTotal - volumGol;

            const procentUmplere = (volumOcupat / volumTotal) * 100;
            const hectolitri = volumOcupat * 10;
            const tone = (hectolitri * greutateHL) / 1000;

            const progressArc = document.getElementById(`progressArc${index}`);
            progressArc.style.strokeDasharray = `${(410 * procentUmplere) / 100} 410`;
            progressArc.style.stroke = CULORI_PRODUSE[tipProdus];

            document.getElementById(`percentageText${index}`).textContent = `${procentUmplere.toFixed(1)}%`;
            document.getElementById(`ocupatText${index}`).textContent = `Ocupat: ${tone.toFixed(1)} t`;
            document.getElementById(`liberText${index}`).textContent = `Liber: ${(volumTotal - volumOcupat).toFixed(1)} m³`;
            document.getElementById(`volumTotal${index}`).textContent = volumTotal.toFixed(2);
            document.getElementById(`volumOcupat${index}`).textContent = volumOcupat.toFixed(2);
            document.getElementById(`cantitate${index}`).textContent = tone.toFixed(1);
        }
    </script>
</body>
</html>
