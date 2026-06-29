# big4.cloud — Strategic Decision Engine

Motore di consulenza strategica institutional-grade in Go. 97 gate cognitivi (structured-thinking + 20 consulting frameworks) + systematic inquiry, blackboard append-only, MCP su HTTP. Ricerca automatica (11 fonti), cross-reference analysis, adversarial stress-test, quality gate, meta-curator con selezione struttura adversariale, curator panel multi-modello, audience simulation multi-round con debate, pre-mortem con calibrazione Polymarket, deep audit. AES-256-GCM encrypted gate files at rest, run persistence SQLite, Prometheus metrics.

## Percorso

```
~/Desktop/Udec/src
```

## Comandi

```bash
# Build
cd ~/Desktop/Udec/src && go build ./...

# Test (40 package)
cd ~/Desktop/Udec/src && go test ./pantheondecision/... -race -count=1

# Server MCP HTTP
cd ~/Desktop/Udec/src && go run ./cmd/decisiond -nodes-dir nodes/files

# Server MCP mock (no API key)
cd ~/Desktop/Udec/src && go run ./cmd/decisiond -mock -nodes-dir nodes/files

# Server MCP con ricerca web
cd ~/Desktop/Udec/src && BRAVE_SEARCH_API_KEY=xxx go run ./cmd/decisiond -search -nodes-dir nodes/files

# Gateway chat mock
cd ~/Desktop/Udec/src && GATEWAY_MOCK=1 go run ./cmd/gateway

# Converti documento a Markdown
cd ~/Desktop/Udec/src && go run ./cmd/markitdown file.pdf -o output.md
```

## Autenticazione

L'accesso al server MCP richiede un'API key valida passata all'inizializzazione della sessione. L'API key è associata a un tenant (organizzazione) e determina:
- Il **piano** attivo (basic / professional / enterprise)
- I **tier di analisi** disponibili (superfast, fast, normal, deep24)
- La **quota mensile** di decision runs

Senza API key configurata, il server opera in modalità demo (tier limitati, nessuna persistenza cross-sessione).

### Sicurezza Autenticazione
- JWT + API key authentication
- bcrypt password hashing (cost 12)
- Refresh token rotation with family theft detection
- Rate limiting: 200 req/min/tenant

## MCP Tools

L'endpoint HTTP MCP espone i seguenti tool:

### Decision Engine (core)

| Tool | Cosa fa |
|---|---|
| `decision_start` | Decisione asincrona con ricerca automatica su 11 fonti (`skip_research: true` per disabilitare) |
| `decision_status` | running / completed / failed |
| `decision_result` | Deliverable + confidence score + assumption register + adversarial verdict |
| `decision_audit_deep` | Deep audit 10 fasi: fact-check + audience sim + enhancement + kill criteria + time-travel + drift monitor. Costa 15 credits. |

### Account & Quota

| Tool | Cosa fa |
|---|---|
| `quota_usage` | Mostra consumo corrente: decision runs usate, limite piano, giorni rimanenti nel ciclo di billing |

### Document Conversion

| Tool | Cosa fa |
|---|---|
| `convert_document` | Converte un singolo documento (PDF, DOCX, XLSX, PPTX, HTML, RTF, CSV, JSON, XML) in Markdown |
| `convert_documents` | Batch-convert fino a 20 documenti in una chiamata (path[] o content_base64[]) → markdown[] senza echo |

### Knowledge & Memory

| Tool | Cosa fa |
|---|---|
| `memory_ingest` | Ingerisci contenuto nel knowledge graph del tenant |
| `memory_ingest_batch` | Ingerisci multipli documenti in batch nel knowledge graph |
| `memory_add` | Aggiungi knowledge atomico (fatto, relazione, entità) |
| `memory_search` | Search ibrido (semantic + keyword) sul knowledge graph |
| `ingest_from_url` | Scarica e ingerisci una pagina web nel knowledge graph (con SSRF protection) |

### Audience Simulation

| Tool | Cosa fa |
|---|---|
| `simulate_audience` | Simula la reazione di un target audience (investitori, board, clienti) a una raccomandazione strategica. Multi-round debate (2-3 round), stance shift tracking, consensus level + adoption probability. |

## Consulting Frameworks (20 gate)

### Original 13 (Strategic Foundations)

| Gate | Framework |
|---|---|
| `consulting.mece` | MECE Decomposition |
| `consulting.issue_tree` | Hypothesis-Driven Issue Trees |
| `consulting.pyramid` | Pyramid Principle (answer-first) |
| `consulting.pareto` | 80/20 Pareto Analysis |
| `consulting.value_chain` | Value Chain Analysis |
| `consulting.five_forces` | Five Forces (competitive intensity) |
| `consulting.tam_sam_som` | TAM/SAM/SOM Market Sizing |
| `consulting.scenario_planning` | Scenario Planning (2x2 matrix) |
| `consulting.due_diligence` | Due Diligence Framework |
| `consulting.stakeholder_map` | Stakeholder Mapping |
| `consulting.business_case` | Business Case (SCR) |
| `consulting.adversarial` | Adversarial Red Team |
| `consulting.assumption_audit` | Assumption Audit & Validation |

### New 7 Specialized Gates

| Gate | Framework | Dimensioni |
|---|---|---|
| `consulting.technical_dd` | Technical Due Diligence | 12 dimensioni: IP, bus factor, architecture, security, data integrity, code hygiene, integration, AI/ML, compliance, roadmap, vendor lock-in, supply chain |
| `consulting.ai_dd` | AI/LLM System Due Diligence | 10 aree: system definition, prompt control, input handling, output quality, tool safety, security, evaluation, observability, model dependency, human oversight |
| `consulting.ai_act` | EU AI Act Readiness | 9 aree: scope, prohibited practices, AI literacy, classification, high-risk requirements, GPAI obligations, transparency, documentation, governance |
| `consulting.risk_score` | Quantitative Risk Scoring | 9 dimensioni × scala 1-5, rating bands, automatic escalation rules, probabilistic loss quantification (Netflix riskquant lognormal model) |
| `consulting.stablecoin_dd` | Stablecoin Due Diligence | 8 dimensioni: peg stability, reserve backing, smart contract risk/scam detection, liquidity depth, issuer verification, on-chain forensics, regulatory, contagion risk |
| `consulting.aml_dd` | AML Due Diligence & Compliance | 10 dimensioni: sanctions screening, PEP status, beneficial ownership, source of funds, transaction patterns, adverse media, jurisdiction risk, industry risk, KYC completeness, compliance program + documentation framework |
| `consulting.premortem` | Pre-Mortem Analysis | 7 step: assume failure narrative, failure modes with P×I scoring, kill criteria, Polymarket calibration, mitigation playbook, confidence calibration |

## Research Sources (11 fonti, pre-loop, parallel fan-out)

| # | Source | API Key? | Data |
|---|--------|----------|------|
| 1 | Brave Web | Yes | Global web search — full snippets + deep results |
| 2 | Reddit | No | Community discussions, opinions, trends |
| 3 | Hacker News | No | Tech community, startup insights |
| 4 | Polymarket | No | Prediction market probabilities (real money signals) |
| 5 | Firecrawl | Yes | Full page crawling — returns actual page content in Markdown |
| 6 | Academic Papers | No | Semantic Scholar, 200M+ papers, citations, abstracts |
| 7 | Patents | No | USPTO + WIPO PATENTSCOPE + EPO OPS (optional) |
| 8 | SEC EDGAR | No | US financial filings 10-K/10-Q/8-K |
| 9 | Corporate Registry | No | OpenCorporates, 200M+ companies worldwide |
| 10 | DeFi Market | No | DeFiLlama — stablecoin/protocol data, TVL, flows |
| 11 | Sanctions/AML | No | OpenSanctions + FinCrimeRadar, 2M+ entities |

La ricerca è automatica: il sistema rileva patent queries, AML queries, e financial queries e augmenta automaticamente le sorgenti appropriate.

## Pipeline Istituzionale (updated)

```
Topic → Pre-Loop Research (11 sources, parallel fan-out)
     → Patent/AML/Financial detection (auto-augments queries)
     → Gate Loop (97 gates, semantic selection, 3 rounds, MaxConcurrent=48)
     → Cross-Reference Analysis (inter-gate connections: reinforcements, contradictions, emergent patterns, blind spots)
     → Quality Gate (blocks if <3 gate outputs)
     → Meta-Curator (3 blueprint candidates → adversarial scoring → best structure)
     → Curator/Panel (standard or 3-model panel + assembler)
     → Auto-ingest conclusions into tenant memory
     → Deliverable (adaptive format, not forced consulting-deck)
```

### Pipeline Details

- **Semantic Selection**: il sistema sceglie i gate rilevanti per il topic, non li esegue tutti
- **MaxConcurrent=48**: massimo parallelismo gate per round
- **Cross-Reference Analysis**: post-loop, trova linkages tra output di gate diversi
- **Meta-Curator**: genera 3 strutture candidate per il deliverable, le valuta su utility/coverage/specificity/coherence, seleziona la migliore — non forza più il consulting-deck su ogni topic
- **Quality Gate**: blocca run con profondità analitica insufficiente (<3 gate output)
- **Auto-ingest**: le conclusioni vengono salvate automaticamente nel knowledge graph del tenant

## Specialized Analysis Gates — Dettagli

### consulting.technical_dd — Technical Due Diligence

12 dimensioni di valutazione per acquisizioni, investimenti, o partnership tecnologiche:

1. **IP & Proprietary Tech** — brevetti, trade secrets, difendibilità
2. **Bus Factor** — concentrazione knowledge, key-person risk
3. **Architecture** — scalabilità, debt tecnico, modernità stack
4. **Security** — posture, vulnerabilità note, incident history
5. **Data Integrity** — qualità dati, pipeline, governance
6. **Code Hygiene** — test coverage, CI/CD, documentation
7. **Integration** — API surface, interoperabilità, migration cost
8. **AI/ML** — maturity, reproducibility, model governance
9. **Compliance** — GDPR, SOC2, certificazioni settoriali
10. **Roadmap** — fattibilità, alignment col mercato
11. **Vendor Lock-in** — dipendenze critiche, exit strategy
12. **Supply Chain** — OSS dependencies, SBOM, vulnerability exposure

### consulting.ai_dd — AI/LLM System Due Diligence

10 aree di valutazione per sistemi AI in produzione:

1. **System Definition** — boundaries, scope, failure modes
2. **Prompt Control** — versioning, injection defense, template governance
3. **Input Handling** — validation, sanitization, rate limiting
4. **Output Quality** — hallucination rate, consistency, grounding
5. **Tool Safety** — permission model, blast radius, rollback
6. **Security** — data leakage, model extraction, adversarial attacks
7. **Evaluation** — benchmarks, A/B testing, regression detection
8. **Observability** — logging, tracing, alerting, cost tracking
9. **Model Dependency** — vendor lock-in, fallback strategy, cost exposure
10. **Human Oversight** — escalation paths, override mechanisms, audit trail

### consulting.ai_act — EU AI Act Readiness

9 aree di compliance per il Regolamento Europeo sull'AI (2024/1689):

1. **Scope** — il sistema rientra nella definizione di AI system?
2. **Prohibited Practices** — Art. 5 (social scoring, subliminal manipulation, etc.)
3. **AI Literacy** — Art. 4 (formazione adeguata per operatori)
4. **Classification** — Annex III (il sistema è high-risk?)
5. **High-Risk Requirements** — Art. 8-15 (risk management, data governance, transparency, accuracy, robustness, cybersecurity)
6. **GPAI Obligations** — Art. 51-56 (se il sistema usa/è un modello GPAI)
7. **Transparency** — Art. 50 (disclosure, marking, watermarking)
8. **Documentation** — technical documentation, EU Declaration of Conformity
9. **Governance** — monitoring, incident reporting, post-market surveillance

### consulting.risk_score — Quantitative Risk Scoring

9 dimensioni × scala 1-5 con aggregazione quantitativa:

| Dimensione | Descrizione |
|---|---|
| Probability | Likelihood of occurrence (1=rare, 5=near-certain) |
| Impact | Severity if realized (1=negligible, 5=catastrophic) |
| Velocity | Speed of onset (1=years, 5=instant) |
| Detectability | Ability to detect early (1=obvious, 5=hidden) |
| Controllability | Ability to mitigate once detected (1=full control, 5=uncontrollable) |
| Reversibility | Ability to reverse damage (1=fully reversible, 5=permanent) |
| Scope | Breadth of affected stakeholders (1=individual, 5=systemic) |
| Precedent | Historical occurrence frequency (1=unprecedented, 5=recurring) |
| Interconnection | Cascade/contagion potential (1=isolated, 5=highly connected) |

**Rating Bands**: Low (9-18), Medium (19-27), High (28-36), Critical (37-45)

**Automatic Escalation**: Critical → immediate escalation + kill criteria evaluation

**Probabilistic Loss Quantification**: Netflix riskquant lognormal model — genera distribuzione probabilistica delle perdite attese (P10/P50/P90) in unità monetarie.

### consulting.stablecoin_dd — Stablecoin Due Diligence

8 dimensioni per valutazione stablecoin/crypto asset:

1. **Peg Stability** — historical deviation, depeg events, recovery time
2. **Reserve Backing** — composition, attestation frequency, overcollateralization ratio
3. **Smart Contract Risk** — audit status, scam detection patterns, upgrade mechanisms
4. **Liquidity Depth** — DEX/CEX liquidity, slippage at scale, redemption capacity
5. **Issuer Verification** — corporate identity, jurisdiction, regulatory licenses
6. **On-Chain Forensics** — whale concentration, flow patterns, mixer interactions
7. **Regulatory** — MiCA compliance, US classification, sanctions exposure
8. **Contagion Risk** — DeFi integrations, cascading liquidation exposure

Dati live da DeFiLlama + on-chain analytics.

### consulting.aml_dd — AML Due Diligence & Compliance

10 dimensioni + documentation framework per compliance antiriciclaggio:

1. **Sanctions Screening** — OFAC/EU/UN consolidated lists + OpenSanctions
2. **PEP Status** — Politically Exposed Person identification + family/associates
3. **Beneficial Ownership** — UBO identification, shell company detection
4. **Source of Funds** — origin verification, legitimacy assessment
5. **Transaction Patterns** — anomaly detection, structuring, layering indicators
6. **Adverse Media** — negative news, legal proceedings, regulatory actions
7. **Jurisdiction Risk** — FATF grey/black list, Transparency International CPI
8. **Industry Risk** — high-risk sectors (gambling, crypto, weapons, extractives)
9. **KYC Completeness** — documentation gaps, verification status
10. **Compliance Program** — policies, training, SAR filing, audit trail

**Documentation Framework**: genera template SAR, risk assessment matrix, e customer risk rating con evidenze strutturate.

Dati da OpenSanctions (2M+ entities) + FinCrimeRadar + OpenCorporates.

### consulting.premortem — Pre-Mortem Analysis

7-step structured analysis che assume il fallimento e lavora a ritroso:

1. **Assume Failure Narrative** — "È passato 1 anno e il progetto è fallito completamente. Cosa è successo?"
2. **Failure Mode Generation** — brainstorm strutturato di tutti i modi di fallimento (tecnico, mercato, team, regolatorio, finanziario, competitivo)
3. **P×I Scoring** — ogni failure mode riceve Probability (1-5) × Impact (1-5) = Risk Score
4. **Kill Criteria** — condizioni oggettive che dovrebbero terminare il progetto (leading indicators, not lagging)
5. **Polymarket Calibration** — confronta le probabilità stimate con prediction market data reali (quando disponibili) per de-biasare overconfidence
6. **Mitigation Playbook** — per ogni failure mode Top-5: trigger, owner, azione, timeline, costo
7. **Confidence Calibration** — confidence interval finale con explicit uncertainty bands

## Audience Simulation (Multi-Round Debate)

La simulazione audience opera in modalità debate multi-round:

### Funzionamento

1. **Persona Generation** — crea 4-6 personas rappresentative del target audience (es: CFO scettico, CTO early-adopter, board member risk-averse)
2. **Round 1: Initial Reaction** — ogni persona esprime posizione iniziale (support/neutral/oppose) con reasoning
3. **Round 2: Cross-Influence** — le personas leggono le posizioni altrui e possono shift stance (cambio tracciato)
4. **Round 3: Final Position** — posizione consolidata dopo debate, con strength of conviction
5. **Verdict** — consensus level (unanimous/majority/split/polarized) + adoption probability (0-100%) + key objections unresolved

### Output Strutturato

```json
{
  "personas": [...],
  "rounds": [
    {"round": 1, "positions": [...]},
    {"round": 2, "shifts": [...], "positions": [...]},
    {"round": 3, "final_positions": [...]}
  ],
  "verdict": {
    "consensus_level": "majority",
    "adoption_probability": 72,
    "key_objections": [...],
    "recommendation": "..."
  }
}
```

## Tier di Analisi

Il tier determina la profondità dell'analisi. Viene risolto automaticamente dal piano, oppure può essere specificato esplicitamente in `decision_start`:

| Tier | Piano Minimo | Caratteristiche |
|---|---|---|
| `superfast` | basic | 1 round, 3 gate, no research, 10s budget |
| `fast` | basic | 1 round, 5 gate, light research, 45s budget |
| `normal` | professional | 3 round, 10 gate, full pipeline + adversarial, 5m budget |
| `deep24` | enterprise | 5 round, 15 gate, exhaustive research + assumption audit, 30m budget (queued) |

## Modelli LLM

Il sistema usa 3 modelli con 1M di context ciascuno, orchestrati per ruoli diversi:

| Ruolo | Modello | Context | Costo |
|-------|---------|---------|-------|
| **Primary (Strategic)** | DeepSeek V4 Flash | 1M token | $0.14/M input, $0.28/M output |
| **Critical** | Gemini 2.5 Flash | 1M token | Free / $0.15/M input, $0.60/M output |
| **Operational** | Qwen 3.5 Plus (via OpenRouter) | 1M token | Free* |
| **Demo** | qwen3:30b-a3b (local Ollama) | — | Free (local) |

### Curator Panel (3+1 modelli)

Quando attivato (`curator_mode: panel`), tutti e 3 i modelli analizzano in parallelo:
- **DeepSeek** → Prospettiva strategica
- **Gemini** → Prospettiva critica
- **Qwen** → Prospettiva operativa
- **Assembler** (DeepSeek) → Fonde i 3 output senza ripetizioni

Costa 5 credits aggiuntivi. Disponibile anche automaticamente nel Deep Audit.

### Costo per run (stimato)

| Tier | Token stimati | Costo DeepSeek | Costo Gemini |
|------|---------------|----------------|--------------|
| superfast | ~15K | $0.006 | Free |
| fast | ~50K | $0.02 | Free |
| normal | ~200K | $0.08 | Free |
| deep24 | ~800K | $0.34 | Free (entro rate limit) |

Con il context caching di DeepSeek (cache hit = $0.003/M), il costo reale scende del 50-90% dopo la prima chiamata nello stesso contesto.

## Demo (senza autenticazione)

Endpoint pubblico `POST /api/v1/demo` — analisi strategica gratuita con pipeline completa:

- **Input**: `{"topic": "...", "email": "user@example.com"}`
- **Pipeline**: ricerca (tutte 11 fonti) → 10 gate × 2 round → semantic selector → curator
- **Output**: HTML email con logo e formatting professionale, consegnata entro 5 minuti
- **Rate limit**: 3 analisi per IP per ora
- **Modello**: qwen3:30b-a3b via Ollama locale (gates + curator)
- **No auth richiesta**: niente API key, niente account

## Sicurezza

| Aspetto | Implementazione |
|---|---|
| Gate files at rest | AES-256-GCM encryption (GATE_ENCRYPTION_KEY env var) |
| SSRF protection | su `ingest_from_url` — whitelist/blacklist, no internal IPs |
| Authentication | JWT + API key dual-mode |
| Rate limiting | 200 req/min/tenant |
| Password | bcrypt hashing (cost 12) |
| Token rotation | Refresh token rotation con family theft detection |
| Prompt defense | `promptdefense.Sanitize` su tutti gli input LLM |
| Input validation | Topic 50KB max, documents 100KB each, max 10 per request |
| Observability | Prometheus metrics endpoint (`/metrics`) |

## Regole

- Go puro, no CGo. SQLite via modernc.org/sqlite.
- De-branded: zero nomi di firm nel codice o corpus.
- Punto-10: azioni esterne solo via outbox + autorizzazione umana.
- Append-only: blackboard e runstore non cancellano mai.
- `promptdefense.Sanitize` su tutti gli input LLM.
- Retry con exponential backoff su tutte le chiamate LLM critiche.
- Tool calling (structured output) per assumptions, adversarial, quality, cross-framework.
- Ogni modifica deve passare `go test ./pantheondecision/... -race -count=1`.
- **Quota**: ogni `decision_start` consuma 1 run dalla quota mensile del tenant. Usa `quota_usage` per verificare il residuo prima di lanciare analisi lunghe.
- **Tier enforcement**: se il piano non include il tier richiesto, `decision_start` restituisce errore. Il tier di default è determinato dal piano.
- **Run Persistence**: SQLite-backed, survives restarts. 2h RAM cache + disk fallback.
- **Encrypted gates**: 140 gate files (97 cognitive + config) encrypted AES-256-GCM at rest.
- **Metrics**: Prometheus endpoint con run lifecycle, LLM calls, duration buckets, HTTP stats.

## Gestione File Grandi

**REGOLA CRITICA**: quando devi lavorare con file di qualsiasi dimensione (PDF, DOCX, XLSX, PPTX, HTML, o qualsiasi formato non-testo), segui SEMPRE questa pipeline:

### Pipeline obbligatoria: Convert → Ingest → Analyze

```bash
# 1. Converti qualsiasi documento a Markdown
cd ~/Desktop/Udec/src && go run ./cmd/markitdown <file_input> -o <file_output>.md

# 2. Ingerisci nel knowledge base (via MCP tool)
memory_ingest(content: "<contenuto MD>", container_tag: "nome-progetto")

# 3. Usa in analisi o audit (via MCP tool)
decision_audit_deep(memory_tag: "nome-progetto")
```

### Procedura obbligatoria

1. **Ricevi un file** → converti subito con `markitdown` (o `convert_document` via MCP)
2. **Ingerisci il Markdown** nel knowledge base con `memory_ingest` usando un `container_tag` descrittivo
3. **Referenzia per tag** — usa `memory_tag` in `decision_audit_deep` per documenti grandi (>50K token)
4. **Se il file è enorme** (>100K token) → spezzalo in chunk logici e ingerisci ogni chunk con lo STESSO `container_tag`
5. **Mai passare >50K token** come parametro diretto di un tool — usa sempre memory_ingest + memory_tag

### Formati supportati
PDF, DOCX, XLSX, PPTX, HTML, RTF, CSV, JSON, XML (NO OCR per immagini in questa pipeline)

### Cosa NON fare

- ❌ Troncare il contenuto senza dirlo
- ❌ Dire "non posso processare file così grandi"
- ❌ Passare 100K+ token come parametro `deliverable`
- ❌ Lavorare sul file originale binario senza conversione

### Cosa fare

- ✅ Convertire sempre in .md prima di tutto
- ✅ Ingerire con `memory_ingest` per documenti grandi
- ✅ Usare `memory_tag` in decision_audit_deep per bypassare limiti di parametro
- ✅ Processare il file completo, chunk per chunk se necessario, stesso container_tag

### Pipeline per HTTP (Claude/Cursor via MCP)

Quando l'utente lavora via HTTP (non stdio locale), i file non sono accessibili dal server. La procedura è:

1. `convert_documents` (batch) — converti fino a 20 file in una sola chiamata (minimo token, zero echo base64)
2. `memory_ingest(content: "<testo MD>", container_tag: "tag")` — invia il testo al server via HTTP
3. `decision_audit_deep(memory_tag: "tag")` — il server recupera il contenuto internamente

**Per file multipli, usare SEMPRE `convert_documents` (batch) anziché N chiamate singole a `convert_document`.**

```json
// Esempio: batch convert via HTTP
convert_documents({
  "items": [
    {"content_base64": "<b64>", "filename": "report.pdf"},
    {"content_base64": "<b64>", "filename": "financials.xlsx"},
    {"content_base64": "<b64>", "filename": "deck.pptx"}
  ]
})
// → {"results": [{"filename": "report.pdf", "markdown": "..."}, ...]}
```

```json
// Esempio: batch convert via stdio (locale)
convert_documents({"paths": ["/path/a.pdf", "/path/b.docx", "/path/c.html"]})
// → {"results": [{"path": "/path/a.pdf", "markdown": "..."}, ...]}
```

Se il testo convertito è troppo grande per un singolo messaggio, fare chunked ingestion:
```bash
# Più chiamate memory_ingest con lo stesso container_tag
memory_ingest(content: chunk1, container_tag: "analisi-x")
memory_ingest(content: chunk2, container_tag: "analisi-x")
# Poi:
decision_audit_deep(memory_tag: "analisi-x")
```

## Struttura

```
src/
├── cmd/decisiond/        — server MCP HTTP endpoint (tools via JSON-RPC)
├── cmd/gateway/          — gateway chat multi-canale + HTTP API (auth, billing, quota)
├── cmd/markitdown/       — CLI converter documenti
├── pantheondecision/     — 45 package (il motore)
│   ├── auth/             — signup, login, JWT, password reset, API keys, refresh rotation
│   ├── tenant/           — multi-tenancy, membership, roles
│   ├── billing/          — Stripe integration, plans, webhooks
│   ├── quota/            — usage tracking, limit enforcement, overage
│   ├── authmiddleware/   — HTTP middleware (JWT + API key + quota)
│   ├── consulting/       — 20 framework strategici (institutional-grade)
│   ├── gate/             — 97 gate cognitivi (AES-256-GCM encrypted at rest)
│   ├── blackboard/       — store append-only DAG
│   ├── loop/             — orchestratore (consulting boost, semantic selectors, MaxConcurrent=48)
│   ├── crossref/         — cross-reference analysis (post-loop)
│   ├── metacurator/      — adversarial structure selection (3 candidates → best)
│   ├── audience/         — multi-round debate simulation
│   ├── premortem/        — pre-mortem analysis + Polymarket calibration
│   ├── research/         — 11 sources parallel fan-out
│   ├── runstore/         — SQLite persistence (survives restarts, 2h RAM cache)
│   ├── metrics/          — Prometheus endpoint
│   └── ...
├── nodes/files/          — 140 gate files (97 cognitive + config), AES-256-GCM encrypted
└── docs/adr/             — 28+ ADR
```

## Env Vars

```bash
PANTHEON_LLM_BASE_URL=https://api.deepseek.com/v1
PANTHEON_LLM_MODEL=deepseek-v4-flash
PANTHEON_LLM_API_KEY=<key>
PANTHEON_LLM_TIMEOUT_SECONDS=120
GATE_ENCRYPTION_KEY=<32-byte-hex-key>
BRAVE_SEARCH_API_KEY=<key>
FIRECRAWL_API_KEY=<key>
# Opzionale: fallback su Gemini free
# PANTHEON_LLM_FALLBACK_MODELS=gemini-2.5-flash
```

## Prometheus Metrics

Endpoint: `GET /metrics`

Metriche esposte:
- `decision_runs_total` — counter per status (completed/failed/timeout)
- `decision_run_duration_seconds` — histogram con buckets (10s, 30s, 60s, 120s, 300s, 600s, 1800s)
- `llm_calls_total` — counter per model/status
- `llm_call_duration_seconds` — histogram
- `gate_executions_total` — counter per gate name/status
- `http_requests_total` — counter per method/path/status
- `http_request_duration_seconds` — histogram

## License

Proprietary — big4.cloud
