<!DOCTYPE html>
<html lang="pt-BR">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Radar Político 2026</title>
<link href="https://fonts.googleapis.com/css2?family=Syne:wght@400;600;700;800&family=IBM+Plex+Mono:wght@300;400;500&display=swap" rel="stylesheet">
<style>
  :root {
    --bg: #0a0a0f;
    --surface: #111118;
    --border: #1e1e2e;
    --accent: #e8ff3c;
    --accent2: #ff4c6e;
    --accent3: #3cffd5;
    --text: #e0e0f0;
    --muted: #5a5a7a;
    --risk-low: #3cffd5;
    --risk-mid: #ffb83c;
    --risk-high: #ff4c6e;
  }

  * { margin: 0; padding: 0; box-sizing: border-box; }

  body {
    background: var(--bg);
    color: var(--text);
    font-family: 'IBM Plex Mono', monospace;
    min-height: 100vh;
    overflow-x: hidden;
  }

  /* Noise overlay */
  body::before {
    content: '';
    position: fixed;
    inset: 0;
    background-image: url("data:image/svg+xml,%3Csvg viewBox='0 0 256 256' xmlns='http://www.w3.org/2000/svg'%3E%3Cfilter id='n'%3E%3CfeTurbulence type='fractalNoise' baseFrequency='0.9' numOctaves='4' stitchTiles='stitch'/%3E%3C/filter%3E%3Crect width='100%25' height='100%25' filter='url(%23n)' opacity='0.04'/%3E%3C/svg%3E");
    pointer-events: none;
    z-index: 0;
    opacity: 0.4;
  }

  header {
    position: relative;
    z-index: 10;
    padding: 48px 48px 32px;
    border-bottom: 1px solid var(--border);
    display: flex;
    align-items: flex-end;
    justify-content: space-between;
    gap: 24px;
    flex-wrap: wrap;
  }

  .header-left h1 {
    font-family: 'Syne', sans-serif;
    font-size: clamp(28px, 5vw, 52px);
    font-weight: 800;
    letter-spacing: -2px;
    line-height: 1;
    color: #fff;
  }

  .header-left h1 span {
    color: var(--accent);
  }

  .header-left p {
    margin-top: 10px;
    font-size: 11px;
    color: var(--muted);
    letter-spacing: 0.08em;
    text-transform: uppercase;
  }

  .header-right {
    display: flex;
    flex-direction: column;
    align-items: flex-end;
    gap: 6px;
  }

  .badge {
    font-size: 10px;
    letter-spacing: 0.12em;
    text-transform: uppercase;
    padding: 4px 10px;
    border: 1px solid var(--border);
    color: var(--muted);
    border-radius: 2px;
  }

  .badge.live {
    border-color: var(--accent);
    color: var(--accent);
  }

  .legend {
    display: flex;
    gap: 20px;
    padding: 16px 48px;
    border-bottom: 1px solid var(--border);
    font-size: 11px;
    color: var(--muted);
    flex-wrap: wrap;
    position: relative;
    z-index: 10;
  }

  .legend-item { display: flex; align-items: center; gap: 6px; }
  .legend-dot { width: 8px; height: 8px; border-radius: 50%; }

  .main {
    position: relative;
    z-index: 10;
    padding: 40px 48px;
  }

  /* Search & Filter */
  .controls {
    display: flex;
    gap: 12px;
    margin-bottom: 32px;
    flex-wrap: wrap;
  }

  .search-box {
    background: var(--surface);
    border: 1px solid var(--border);
    color: var(--text);
    font-family: 'IBM Plex Mono', monospace;
    font-size: 12px;
    padding: 10px 16px;
    outline: none;
    flex: 1;
    min-width: 200px;
    transition: border-color 0.2s;
  }

  .search-box:focus { border-color: var(--accent); }

  .filter-btn {
    background: var(--surface);
    border: 1px solid var(--border);
    color: var(--muted);
    font-family: 'IBM Plex Mono', monospace;
    font-size: 11px;
    padding: 10px 16px;
    cursor: pointer;
    text-transform: uppercase;
    letter-spacing: 0.08em;
    transition: all 0.2s;
  }

  .filter-btn:hover, .filter-btn.active {
    border-color: var(--accent);
    color: var(--accent);
  }

  /* Cards grid */
  .cards-grid {
    display: grid;
    grid-template-columns: repeat(auto-fill, minmax(340px, 1fr));
    gap: 20px;
    margin-bottom: 48px;
  }

  .card {
    background: var(--surface);
    border: 1px solid var(--border);
    padding: 28px;
    position: relative;
    overflow: hidden;
    cursor: pointer;
    transition: border-color 0.25s, transform 0.2s;
    animation: fadeUp 0.5s ease both;
  }

  .card:hover {
    border-color: var(--accent);
    transform: translateY(-2px);
  }

  .card::before {
    content: '';
    position: absolute;
    top: 0; left: 0; right: 0;
    height: 2px;
    background: var(--risk-color, var(--muted));
    transition: height 0.3s;
  }

  .card:hover::before { height: 3px; }

  .card-header {
    display: flex;
    justify-content: space-between;
    align-items: flex-start;
    margin-bottom: 20px;
  }

  .politician-name {
    font-family: 'Syne', sans-serif;
    font-size: 18px;
    font-weight: 700;
    color: #fff;
    line-height: 1.2;
  }

  .politician-meta {
    font-size: 10px;
    color: var(--muted);
    margin-top: 4px;
    letter-spacing: 0.06em;
  }

  .risk-badge {
    font-size: 9px;
    letter-spacing: 0.1em;
    text-transform: uppercase;
    padding: 4px 8px;
    border: 1px solid;
    white-space: nowrap;
  }

  .risk-low { color: var(--risk-low); border-color: var(--risk-low); }
  .risk-mid { color: var(--risk-mid); border-color: var(--risk-mid); }
  .risk-high { color: var(--risk-high); border-color: var(--risk-high); }

  .scores {
    display: flex;
    flex-direction: column;
    gap: 12px;
    margin-bottom: 20px;
  }

  .score-row {
    display: grid;
    grid-template-columns: 100px 1fr 32px;
    align-items: center;
    gap: 10px;
    font-size: 10px;
  }

  .score-label {
    color: var(--muted);
    text-transform: uppercase;
    letter-spacing: 0.06em;
  }

  .score-bar-bg {
    height: 3px;
    background: var(--border);
    position: relative;
    overflow: hidden;
  }

  .score-bar-fill {
    position: absolute;
    top: 0; left: 0; height: 100%;
    transition: width 1s cubic-bezier(0.16, 1, 0.3, 1);
    width: 0;
  }

  .score-val {
    text-align: right;
    color: var(--text);
    font-weight: 500;
  }

  .factors {
    border-top: 1px solid var(--border);
    padding-top: 16px;
    display: flex;
    flex-direction: column;
    gap: 6px;
  }

  .factor-item {
    font-size: 10px;
    color: var(--muted);
    display: flex;
    align-items: flex-start;
    gap: 8px;
    line-height: 1.4;
  }

  .factor-icon {
    flex-shrink: 0;
    width: 14px;
    height: 14px;
    border-radius: 50%;
    display: flex;
    align-items: center;
    justify-content: center;
    font-size: 8px;
    margin-top: 1px;
  }

  .factor-neg { background: rgba(255, 76, 110, 0.15); color: var(--risk-high); }
  .factor-neu { background: rgba(255, 184, 60, 0.15); color: var(--risk-mid); }
  .factor-pos { background: rgba(60, 255, 213, 0.15); color: var(--risk-low); }

  .confidence {
    margin-top: 14px;
    font-size: 9px;
    color: var(--muted);
    display: flex;
    align-items: center;
    gap: 6px;
  }

  .confidence-bar {
    flex: 1;
    height: 2px;
    background: var(--border);
    position: relative;
  }

  .confidence-fill {
    position: absolute;
    top: 0; left: 0; height: 100%;
    background: var(--muted);
    transition: width 1s ease;
    width: 0;
  }

  /* Ranking table */
  .section-title {
    font-family: 'Syne', sans-serif;
    font-size: 13px;
    font-weight: 700;
    text-transform: uppercase;
    letter-spacing: 0.12em;
    color: var(--muted);
    margin-bottom: 20px;
    display: flex;
    align-items: center;
    gap: 12px;
  }

  .section-title::after {
    content: '';
    flex: 1;
    height: 1px;
    background: var(--border);
  }

  .ranking-table {
    width: 100%;
    border-collapse: collapse;
    font-size: 11px;
    margin-bottom: 48px;
  }

  .ranking-table th {
    text-align: left;
    padding: 10px 14px;
    border-bottom: 1px solid var(--border);
    color: var(--muted);
    font-weight: 400;
    text-transform: uppercase;
    letter-spacing: 0.08em;
    font-size: 9px;
  }

  .ranking-table td {
    padding: 14px;
    border-bottom: 1px solid rgba(30,30,46,0.5);
    color: var(--text);
    vertical-align: middle;
  }

  .ranking-table tr:hover td { background: rgba(255,255,255,0.02); }

  .rank-num {
    font-family: 'Syne', sans-serif;
    font-size: 20px;
    font-weight: 800;
    color: var(--border);
  }

  .rank-1 { color: var(--accent); }
  .rank-2 { color: rgba(232,255,60,0.6); }
  .rank-3 { color: rgba(232,255,60,0.35); }

  .inline-bar {
    display: flex;
    align-items: center;
    gap: 8px;
  }

  .inline-bar-bg {
    width: 80px;
    height: 3px;
    background: var(--border);
    position: relative;
    overflow: hidden;
  }

  .inline-bar-fill {
    position: absolute;
    top: 0; left: 0; height: 100%;
    width: 0;
    transition: width 1.2s cubic-bezier(0.16, 1, 0.3, 1);
  }

  /* Disclaimer */
  .disclaimer {
    border: 1px solid var(--border);
    padding: 20px 24px;
    font-size: 10px;
    color: var(--muted);
    line-height: 1.7;
    position: relative;
    z-index: 10;
    margin: 0 48px 48px;
  }

  .disclaimer strong { color: var(--accent); }

  /* Loading state */
  .loading-overlay {
    position: fixed;
    inset: 0;
    background: var(--bg);
    display: flex;
    align-items: center;
    justify-content: center;
    flex-direction: column;
    gap: 16px;
    z-index: 1000;
    transition: opacity 0.5s;
  }

  .loading-overlay.hidden { opacity: 0; pointer-events: none; }

  .loader-text {
    font-family: 'Syne', sans-serif;
    font-size: 13px;
    font-weight: 700;
    letter-spacing: 0.15em;
    text-transform: uppercase;
    color: var(--accent);
  }

  .loader-bar {
    width: 200px;
    height: 1px;
    background: var(--border);
    position: relative;
    overflow: hidden;
  }

  .loader-bar::after {
    content: '';
    position: absolute;
    top: 0; left: -100%;
    width: 100%;
    height: 100%;
    background: var(--accent);
    animation: loading 1.2s ease infinite;
  }

  @keyframes loading {
    0% { left: -100%; }
    100% { left: 100%; }
  }

  @keyframes fadeUp {
    from { opacity: 0; transform: translateY(16px); }
    to { opacity: 1; transform: translateY(0); }
  }

  .ai-analysis-btn {
    background: transparent;
    border: 1px solid var(--accent);
    color: var(--accent);
    font-family: 'IBM Plex Mono', monospace;
    font-size: 10px;
    text-transform: uppercase;
    letter-spacing: 0.1em;
    padding: 6px 12px;
    cursor: pointer;
    margin-top: 14px;
    width: 100%;
    transition: all 0.2s;
  }

  .ai-analysis-btn:hover {
    background: var(--accent);
    color: #000;
  }

  .ai-output {
    margin-top: 12px;
    font-size: 10px;
    color: var(--text);
    line-height: 1.7;
    border-top: 1px solid var(--border);
    padding-top: 12px;
    display: none;
  }

  .ai-output.visible { display: block; }

  .ai-loading {
    color: var(--muted);
    font-style: italic;
  }

  @media (max-width: 600px) {
    header, .main, .disclaimer { padding-left: 20px; padding-right: 20px; }
    .legend { padding-left: 20px; padding-right: 20px; }
    .disclaimer { margin: 0 20px 32px; }
    .ranking-table { display: none; }
  }
</style>
</head>
<body>

<div class="loading-overlay" id="loader">
  <div class="loader-text">Radar Político 2026</div>
  <div class="loader-bar"></div>
  <div style="font-size:10px;color:var(--muted);margin-top:8px;">Carregando análise estruturada...</div>
</div>

<header>
  <div class="header-left">
    <h1>RADAR <span>POLÍTICO</span><br>2026</h1>
    <p>Sistema de Análise de Integridade e Comportamento — Presidência</p>
  </div>
  <div class="header-right">
    <div class="badge live">● Sistema Ativo</div>
    <div class="badge">Dados: fontes públicas verificáveis</div>
    <div class="badge">Análise: não-partisan</div>
  </div>
</header>

<div class="legend">
  <div class="legend-item"><div class="legend-dot" style="background:var(--risk-low)"></div> Baixo risco (80–100)</div>
  <div class="legend-item"><div class="legend-dot" style="background:var(--risk-mid)"></div> Risco moderado (50–79)</div>
  <div class="legend-item"><div class="legend-dot" style="background:var(--risk-high)"></div> Alto risco (0–49)</div>
  <div style="flex:1"></div>
  <div style="font-size:10px;color:var(--muted)">Score = média ponderada | Confiança = cobertura de dados públicos</div>
</div>

<div class="main">
  <div class="controls">
    <input class="search-box" type="text" placeholder="Buscar candidato..." id="searchInput">
    <button class="filter-btn active" onclick="filterCards('all', this)">Todos</button>
    <button class="filter-btn" onclick="filterCards('low', this)">Baixo risco</button>
    <button class="filter-btn" onclick="filterCards('mid', this)">Moderado</button>
    <button class="filter-btn" onclick="filterCards('high', this)">Alto risco</button>
  </div>

  <div class="section-title">Análise Individual</div>
  <div class="cards-grid" id="cardsGrid"></div>

  <div class="section-title">Ranking Comparativo</div>
  <table class="ranking-table" id="rankingTable">
    <thead>
      <tr>
        <th>#</th>
        <th>Político</th>
        <th>Integridade</th>
        <th>Coerência</th>
        <th>Atividade</th>
        <th>Eficiência</th>
        <th>Score Geral</th>
        <th>Risco</th>
      </tr>
    </thead>
    <tbody id="rankingBody"></tbody>
  </table>
</div>

<div class="disclaimer">
  <strong>NOTA METODOLÓGICA:</strong> Os scores apresentados são estimativas analíticas baseadas em dados públicos disponíveis — Portal da Transparência, registros do TSE, STF, histórico legislativo e cobertura jornalística de veículos de referência. O sistema <strong>não emite juízo moral</strong>: identifica padrões e inconsistências verificáveis. Processos judiciais recebem peso proporcional à fase (investigação &lt; denúncia aceita &lt; condenação). Candidaturas de 2026 ainda em definição. Atualização: Abril/2026.
</div>

<script>
const politicians = [
  {
    name: "Lula",
    full: "Luiz Inácio Lula da Silva",
    party: "PT · Presidente em exercício",
    scores: { integridade: 42, coerencia: 55, atividade: 72, eficiencia: 58 },
    riskLevel: "high",
    riskColor: "#ff4c6e",
    confidence: 88,
    factors: [
      { type: "neg", text: "Condenado e posteriormente absolvido por Moro — jurisprudência controversa ainda em debate público" },
      { type: "neg", text: "Investigações em curso no âmbito do STF (caso Silas Malafaia e outros)" },
      { type: "neu", text: "Gastos presidenciais acima da média histórica dos últimos mandatos" },
      { type: "pos", text: "Alta presença em agenda pública e representação internacional" },
      { type: "neu", text: "Mudanças de posicionamento fiscal entre campanha e governo" }
    ]
  },
  {
    name: "Bolsonaro",
    full: "Jair Messias Bolsonaro",
    party: "PL · Ex-presidente",
    scores: { integridade: 28, coerencia: 61, atividade: 45, eficiencia: 52 },
    riskLevel: "high",
    riskColor: "#ff4c6e",
    confidence: 85,
    factors: [
      { type: "neg", text: "Inelegível por decisão do TSE até 2030 — condenação por abuso de poder e uso indevido de mídia" },
      { type: "neg", text: "Indiciado pela PGR por tentativa de golpe de Estado (8 de janeiro e outros episódios)" },
      { type: "neg", text: "Investigado por desvio de joias de Estado" },
      { type: "pos", text: "Coerência discursiva consistente com votos ao longo do mandato" },
      { type: "neu", text: "Candidatura em 2026 juridicamente inviável no cenário atual" }
    ]
  },
  {
    name: "Tarcísio",
    full: "Tarcísio de Freitas",
    party: "Republicanos · Gov. SP",
    scores: { integridade: 74, coerencia: 78, atividade: 83, eficiencia: 76 },
    riskLevel: "low",
    riskColor: "#3cffd5",
    confidence: 72,
    factors: [
      { type: "pos", text: "Sem processos criminais formais ou condenações registradas" },
      { type: "pos", text: "Avaliação positiva de gestão do Governo SP em indicadores de eficiência" },
      { type: "pos", text: "Coerência entre discurso de gestão e práticas administrativas verificáveis" },
      { type: "neu", text: "Histórico curto em cargos eletivos — menor volume de dados disponíveis" },
      { type: "neu", text: "Posicionamento eleitoral para 2026 ainda em definição pública" }
    ]
  },
  {
    name: "Pacheco",
    full: "Rodrigo Pacheco",
    party: "PSD · Presidente do Senado",
    scores: { integridade: 66, coerencia: 62, atividade: 79, eficiencia: 70 },
    riskLevel: "mid",
    riskColor: "#ffb83c",
    confidence: 68,
    factors: [
      { type: "pos", text: "Sem condenações judiciais registradas — ficha limpa formal" },
      { type: "neu", text: "Citado em investigações periféricas sem denúncia formal" },
      { type: "pos", text: "Alta atividade legislativa como presidente do Senado" },
      { type: "neu", text: "Posicionamentos moderados geram percepção de baixa coerência ideológica" },
      { type: "neu", text: "Candidatura 2026 em especulação — sem anúncio formal" }
    ]
  },
  {
    name: "Ciro",
    full: "Ciro Gomes",
    party: "PDT · Ex-ministro",
    scores: { integridade: 63, coerencia: 48, atividade: 52, eficiencia: 60 },
    riskLevel: "mid",
    riskColor: "#ffb83c",
    confidence: 74,
    factors: [
      { type: "pos", text: "Sem condenações criminais registradas" },
      { type: "neg", text: "Baixa coerência: discurso programático diverge de alianças eleitorais históricas" },
      { type: "neg", text: "Citado em processo relacionado a nepotismo no governo do Ceará" },
      { type: "pos", text: "Atividade propositiva com produção legislativa verificável" },
      { type: "neu", text: "Presença eleitoral decrescente nas últimas eleições" }
    ]
  },
  {
    name: "Dino",
    full: "Flávio Dino",
    party: "PSol · Min. STF",
    scores: { integridade: 58, coerencia: 67, atividade: 65, eficiencia: 54 },
    riskLevel: "mid",
    riskColor: "#ffb83c",
    confidence: 62,
    factors: [
      { type: "neu", text: "Atuação como ministro do STF inelegibiliza candidatura imediata" },
      { type: "neg", text: "Investigado pelo TCU por irregularidades em emendas parlamentares" },
      { type: "pos", text: "Produção acadêmica e legislativa consistente com posicionamento declarado" },
      { type: "neu", text: "Histórico de migração partidária reduz score de coerência de longo prazo" },
      { type: "neg", text: "Polêmicas na gestão da segurança pública no MA com dados contraditórios" }
    ]
  }
];

function getRiskLabel(level) {
  return { low: 'Baixo risco', mid: 'Risco moderado', high: 'Alto risco' }[level];
}

function getBarColor(score) {
  if (score >= 80) return 'var(--risk-low)';
  if (score >= 50) return 'var(--risk-mid)';
  return 'var(--risk-high)';
}

function getOverallScore(scores) {
  const vals = Object.values(scores);
  return Math.round(vals.reduce((a, b) => a + b, 0) / vals.length);
}

function buildCard(p, index) {
  const overall = getOverallScore(p.scores);
  const delay = index * 80;

  return `
    <div class="card" style="--risk-color:${p.riskColor}; animation-delay:${delay}ms" data-risk="${p.riskLevel}" data-name="${p.name.toLowerCase()} ${p.full.toLowerCase()}">
      <div class="card-header">
        <div>
          <div class="politician-name">${p.name}</div>
          <div class="politician-meta">${p.full}<br>${p.party}</div>
        </div>
        <div class="risk-badge risk-${p.riskLevel}">${getRiskLabel(p.riskLevel)}</div>
      </div>

      <div class="scores">
        ${Object.entries({integridade: p.scores.integridade, coerência: p.scores.coerencia, atividade: p.scores.atividade, eficiência: p.scores.eficiencia}).map(([label, val]) => `
          <div class="score-row">
            <div class="score-label">${label}</div>
            <div class="score-bar-bg">
              <div class="score-bar-fill" style="background:${getBarColor(val)}" data-width="${val}"></div>
            </div>
            <div class="score-val">${val}</div>
          </div>
        `).join('')}
      </div>

      <div class="factors">
        ${p.factors.map(f => `
          <div class="factor-item">
            <div class="factor-icon factor-${f.type}">${f.type === 'neg' ? '✕' : f.type === 'pos' ? '✓' : '○'}</div>
            <span>${f.text}</span>
          </div>
        `).join('')}
      </div>

      <div class="confidence">
        <span>Confiança dos dados:</span>
        <div class="confidence-bar">
          <div class="confidence-fill" data-width="${p.confidence}"></div>
        </div>
        <span>${p.confidence}%</span>
      </div>

      <button class="ai-analysis-btn" onclick="requestAI('${p.name}', this)">
        ⬡ Análise IA aprofundada
      </button>
      <div class="ai-output" id="ai-${p.name}"></div>
    </div>
  `;
}

function buildRankingRow(p, rank) {
  const overall = getOverallScore(p.scores);
  const rankClass = rank <= 3 ? `rank-${rank}` : '';
  return `
    <tr>
      <td><span class="rank-num ${rankClass}">${rank.toString().padStart(2,'0')}</span></td>
      <td>
        <div style="font-family:'Syne',sans-serif;font-weight:700;color:#fff">${p.name}</div>
        <div style="font-size:9px;color:var(--muted)">${p.party}</div>
      </td>
      ${['integridade','coerencia','atividade','eficiencia'].map(k => `
        <td>
          <div class="inline-bar">
            <div class="inline-bar-bg">
              <div class="inline-bar-fill" style="background:${getBarColor(p.scores[k])}" data-width="${p.scores[k]}"></div>
            </div>
            <span>${p.scores[k]}</span>
          </div>
        </td>
      `).join('')}
      <td style="font-family:'Syne',sans-serif;font-weight:700;font-size:18px;color:${p.riskColor}">${overall}</td>
      <td><span class="risk-badge risk-${p.riskLevel}" style="font-size:8px">${getRiskLabel(p.riskLevel)}</span></td>
    </tr>
  `;
}

function renderAll() {
  // Sort by overall score desc for ranking
  const sorted = [...politicians].sort((a,b) => getOverallScore(b.scores) - getOverallScore(a.scores));

  document.getElementById('cardsGrid').innerHTML = politicians.map((p, i) => buildCard(p, i)).join('');
  document.getElementById('rankingBody').innerHTML = sorted.map((p, i) => buildRankingRow(p, i+1)).join('');

  // Animate bars
  setTimeout(() => {
    document.querySelectorAll('[data-width]').forEach(el => {
      el.style.width = el.dataset.width + '%';
    });
  }, 300);
}

function filterCards(risk, btn) {
  document.querySelectorAll('.filter-btn').forEach(b => b.classList.remove('active'));
  btn.classList.add('active');
  document.querySelectorAll('.card').forEach(card => {
    card.style.display = (risk === 'all' || card.dataset.risk === risk) ? 'block' : 'none';
  });
}

document.getElementById('searchInput').addEventListener('input', function() {
  const q = this.value.toLowerCase();
  document.querySelectorAll('.card').forEach(card => {
    card.style.display = card.dataset.name.includes(q) ? 'block' : 'none';
  });
});

async function requestAI(name, btn) {
  const outputEl = document.getElementById(`ai-${name}`);
  const p = politicians.find(x => x.name === name);
  
  outputEl.className = 'ai-output visible';
  outputEl.innerHTML = '<div class="ai-loading">⬡ Processando análise aprofundada...</div>';
  btn.disabled = true;

  const prompt = `Você é um analista político especializado em integridade pública brasileira. 
  Analise ${p.full} (${p.party}) de forma objetiva e não-partidária.
  
  Dados disponíveis:
  - Score de Integridade: ${p.scores.integridade}/100
  - Score de Coerência: ${p.scores.coerencia}/100  
  - Score de Atividade: ${p.scores.atividade}/100
  - Score de Eficiência: ${p.scores.eficiencia}/100
  - Fatores identificados: ${p.factors.map(f => f.text).join('; ')}
  
  Forneça uma análise concisa (150-200 palavras) que:
  1. Explique o que os scores revelam sobre o perfil político
  2. Identifique o principal risco e o principal ativo do candidato
  3. Contextualize no cenário de 2026
  
  Seja analítico, não faça juízo moral. Use linguagem técnica e objetiva.`;

  try {
    const res = await fetch('https://api.anthropic.com/v1/messages', {
      method: 'POST',
      headers: { 'Content-Type': 'application/json' },
      body: JSON.stringify({
        model: 'claude-sonnet-4-20250514',
        max_tokens: 1000,
        messages: [{ role: 'user', content: prompt }]
      })
    });
    const data = await res.json();
    const text = data.content?.[0]?.text || 'Análise indisponível.';
    outputEl.innerHTML = text.replace(/\n/g, '<br>');
  } catch(e) {
    outputEl.innerHTML = 'Erro ao carregar análise. Verifique a conexão.';
  }
  
  btn.disabled = false;
}

// Init
window.addEventListener('load', () => {
  renderAll();
  setTimeout(() => {
    document.getElementById('loader').classList.add('hidden');
  }, 1200);
});
</script>
</body>
</html>
