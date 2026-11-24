# Ideia para pensar: projeto `3cerebros` â€” portÃ¡til, autÃ´nomo, com painel

> Status: **ideia / parking lot**. NÃ£o Ã© pra construir agora â€” Ã© um ponto pra
> pensarmos. EvoluÃ§Ã£o do `cerebro-ARCKHÉ` (que hoje Ã© Claude-Code-first e por
> comando) para algo **portÃ¡til entre runtimes, autÃ´nomo e monitorÃ¡vel**.
> Capturado: 2026-06-20.

## A semente (o que o Nei pediu)
Um projeto novo, talvez `3cerebros`, que:
1. seja **portado pra rodar em vÃ¡rios runtimes**: Claude Code, Codex, e agentes
   como **Hermes, AgentZero, OpenClaw, Intelecto**â€¦
2. tenha **dicas de instalar e de usar** (por runtime);
3. **funcione sem comandos** â€” em vez de vocÃª disparar `/triagem`, `/nota` etc.,
   ele **se autogerencia**;
4. para isso, faÃ§a uma **entrevista** que define **o que pode e o que nÃ£o pode
   ser automatizado**;
5. tenha um **painel pra monitorar** o que tem dentro dele;
6. tenha **importadores de dados** pras bases: e-mail, Telegram, WhatsApp,
   export do ChatGPT, redes sociais, e outros docs.

## âš ï¸ ANTES de construir: os sistemas jÃ¡ nÃ£o fazem isso? (gating)

Regra do Nei: **checar se os sistemas existentes jÃ¡ tÃªm esses recursos** antes de
construir. Pesquisa feita (3 agentes web, 2026-06-20). Legenda de confianÃ§a:
**âœ… verificado** (evidÃªncia/observaÃ§Ã£o direta) Â· **âš ï¸ alegaÃ§Ã£o web nÃ£o confirmada**
(nÃºmero/data veio de agente, tratar com ceticismo). CorreÃ§Ã£o: `zubair-trabzada/brain-map`
**existe** (clonado nesta sessÃ£o, 16â­) â€” um agente disse que nÃ£o; ignore.

### Matriz A â€” projetos de second-brain (com evidÃªncia/URL)

| Projeto | AutÃ´nomo | Conectores | Painel | Cross-runtime | Entrevistaâ†’polÃ­tica |
|---|---|---|---|---|---|
| khoj-ai/khoj | parcial (cronâ†’email) | parcial (PDF/Notion/GitHub) | parcial (admin Django) | **nÃ£o** (Ã© o runtime) | nÃ£o |
| AgriciDaniel/claude-obsidian | parcial (`/autoresearch`, auto-commit 15min) | parcial (drop + Web Clipper) | **sim** (`dashboard.base`/Dataview) | sim (exp.: Codex/Cursor/Gemini) | nÃ£o ("what is this vault for?") |
| SamurAIGPT/llm-wiki-agent | nÃ£o | parcial (markitdown: PDF/DOCXâ€¦) | nÃ£o (`graph.html` estÃ¡tico) | **sim** (Claude/Codex/Gemini/OpenCode) | nÃ£o |
| huytieu/COG-second-brain | nÃ£o (17 skills manuais) | parcial (Linear/GitHub/Jira/Slack/Notion) | parcial (Tasks) | **sim** (6 surfaces) | **parcial** (`/onboarding` â†’ integraÃ§Ãµes) |
| henrydaum/second-brain | **sim** (cron Timekeeper + watcher) | parcial (Telegram nativo, OCR, whisper) | parcial (`/debug`, heartbeat) | sim (AGENTS.md) | parcial (`/setup`, limites hardcoded) |
| eugeniughelbur/obsidian-second-brain | **sim** (4 cron + PostCompact, "mantÃ©m enquanto vocÃª dorme") | parcial (Telegram, Calendar, X, YouTube) | parcial (`/obsidian-health`) | **sim** ("one codebase, four platforms") | nÃ£o |

### Matriz B â€” runtimes/agentes (âš ï¸ vÃ¡rios nÃºmeros/datas nÃ£o confirmados)

| Runtime | MemÃ³ria nativa | AutÃ´nomo (cron/daemon) | Mensageria nativa | MCP | Acoplar skills .md |
|---|---|---|---|---|---|
| Claude Code | parcial (arquivos+tasks) | sim (tasks+hooks) | nÃ£o (via MCP) | âœ… sim | âœ… excelente (padrÃ£o SKILL.md) |
| Codex CLI | parcial (AGENTS.md) | parcial (modos approval) | nÃ£o | âœ… sim | parcial (AGENTS.md) |
| Hermes âš ï¸ | sim (alegado) | sim (cron) | sim (TG/WA/Discordâ€¦) | sim | sim |
| agent-zero âš ï¸ | sim (vetorial) | sim (multi-agente Docker) | parcial (plugin) | sim | sim (SKILL.md) |
| OpenClaw âš ï¸ | parcial (plugins) | sim (daemon, computer-use) | sim (30+ plataformas) | sim | sim (registry) |
| **Intelecto** = `linnoff88-dev/intelecto` âš ï¸ | sim (SQLite FTS5 + SOUL/AGENTS/USER.md) | nÃ£o | sim (Telegram) | ambÃ­guo | âœ… excelente (jÃ¡ Ã© .md-driven) â€” **Ã© seu prÃ³prio projeto** |

### Infra pronta pra REUSAR (nÃ£o construir)
- **Conectores via MCP:** Gmail (`GongRzhe/Gmail-MCP-Server`), Telegram (`chigwell/telegram-mcp`),
  WhatsApp (`lharries/whatsapp-mcp` â€” âš ï¸ bridge nÃ£o-oficial, risco de ban), redes sociais (**Postiz**, OSS).
- **Importar exports â†’ Markdown:** ChatGPT (`gavi/chatgpt-markdown`, `Nexus AI Chat Importer`),
  Telegram (`tg2obsidian`), WhatsApp (plugin Obsidian). ChatGPT export = `conversations.json` (Ã¡rvore).
- **Portabilidade:** **Agent Skills spec** (agentskills.io, padrÃ£o aberto Anthropic) â€” mesma skill em ~20 runtimes;
  vÃ¡rios projetos jÃ¡ fazem via `AGENTS.md` + `build.sh --platform`.
- **Painel base:** `brain-map` (grafo + timeline + click-to-inspect) e `graphify` (grafo queryÃ¡vel + MCP).

### Veredito (responde "os sistemas jÃ¡ nÃ£o fazem isso?")
**JÃ¡ fazem â€” quase tudo.** Dos 5 recursos:
- **Portabilidade** â†’ âœ… **resolvido** (Agent Skills spec + AGENTS.md). NÃ£o construir; seguir o padrÃ£o.
- **Modo autÃ´nomo** â†’ âœ… **jÃ¡ existe pronto** (henrydaum e eugeniughelbur rodam cron/background "enquanto vocÃª dorme"). Copiar o padrÃ£o, nÃ£o reinventar.
- **Conectores** â†’ âœ… **reusÃ¡veis** via MCP + importadores .md (gap nos projetos de brain, mas a infra existe). SÃ³ plugar.
- **Painel** â†’ parcial (saÃºde em texto + brain-map/graphify visual). Falta sÃ³ a camada **operacional** (o que foi automatizado / pendente de aprovaÃ§Ã£o).
- **Entrevista â†’ polÃ­tica de automaÃ§Ã£o** â†’ â— **GAP REAL.** Nenhum dos 6 tem polÃ­tica de *permissÃµes de autonomia* (COG chega perto, mas Ã© mapa de integraÃ§Ãµes, nÃ£o de limites).

**ConclusÃ£o:** o que sobra de genuinamente novo Ã© fino â€” **(1) a separaÃ§Ã£o em 3 cÃ©rebros** (Projeto/Self/Conhecimento, que ninguÃ©m faz explÃ­cito) + **(2) autonomia governada por uma entrevista que define o que pode/nÃ£o pode** + **(3) um painel operacional**. Todo o resto Ã© **integrar/configurar** peÃ§as existentes. EntÃ£o `3cerebros`, se acontecer, Ã© **integration-first**: monta sobre MCP + Agent Skills spec + um background-agent (estilo eugeniughelbur) + brain-map, e constrÃ³i sÃ³ a camada fina de polÃ­tica+3-cÃ©rebros+painel.

**Atalho possÃ­vel:** como **Intelecto** (`linnoff88-dev/intelecto`) jÃ¡ Ã© um agente .md-driven com memÃ³ria e Telegram **e Ã© seu**, o `3cerebros` pode nascer como uma *evoluÃ§Ã£o dele* (adicionar os 3 cÃ©rebros + polÃ­tica), em vez de projeto do zero.

## Como eu leio isso (5 frentes)

### 1. Portabilidade cross-runtime
- A base jÃ¡ ajuda: os 3 cÃ©rebros sÃ£o **arquivos .md + pastas** â€” formato neutro
  que qualquer agente lÃª. O que muda por runtime Ã© sÃ³ a **camada de skills/comandos**.
- Caminho: seguir a **[Agent Skills spec](https://agentskills.io)** (a mesma que o
  kepano usa) â†’ uma fonte, vÃ¡rios runtimes. A skill **`polyskill`** (jÃ¡ instalada
  aqui) empacota skill pra rodar em Claude Code **e** Codex a partir de um fonte
  portÃ¡til. Para Hermes/AgentZero/OpenClaw/Intelecto, mapear o equivalente de
  "skill/hook" de cada um (alguns leem `AGENTS.md`, outros tÃªm formato prÃ³prio).
- EntregÃ¡vel: um `core/` neutro (os 3 cÃ©rebros + regras) + adaptadores finos por
  runtime (`adapters/claude`, `adapters/codex`, `adapters/hermes`â€¦).

### 2. Onboarding (instalar + usar por runtime)
- Um instalador que detecta/pergunta o runtime e escreve os arquivos certos
  (no Claude vira `.claude/skills`, no Codex `~/.codex/skills`, no Hermesâ€¦).
- Guia de uso por runtime â€” provavelmente uma landing+guia (como a do cerebro-ARCKHÉ)
  com abas por agente.

### 3. Modo autÃ´nomo (sem comandos) â€” **o pulo do gato**
- Hoje: vocÃª roda `/triagem`, `/nota`, `/tldr`. No `3cerebros`: o agente faz isso
  **sozinho** â€” observa o que entra e roteia/destila/registra sem vocÃª pedir.
- O risco Ã³bvio: autonomia sem limite assusta e erra. Por isso a **entrevista**:
  ela gera uma **polÃ­tica de automaÃ§Ã£o** â€” o que ele PODE fazer sozinho (ex.:
  arquivar nota, atualizar daily) vs o que **pede confirmaÃ§Ã£o** (ex.: mandar
  e-mail, mover decisÃ£o, apagar). 
- Onde isso mora: essa polÃ­tica Ã© parte do **cÃ©rebro Self** (valores â†’ permissÃµes).
  Liga com [[estrutura-3-cerebros]] e a skill `socratica`/`setup-cerebro`.
- NÃ­veis de autonomia (sugestÃ£o): **observar** â†’ **sugerir** â†’ **agir com
  confirmaÃ§Ã£o** â†’ **agir sozinho** (por tipo de aÃ§Ã£o). Tudo logado.

### 4. Painel de monitoramento
- Um dashboard local (estilo brain-map, mas com estado, nÃ£o sÃ³ grafo) que mostra:
  o que tem em cada cÃ©rebro, o que foi automatizado, o que entrou de cada fonte,
  o que estÃ¡ esperando confirmaÃ§Ã£o, e a "saÃºde" (nota Ã³rfÃ£, inbox acumulando).
- Self-contained, lÃª os `.md` â€” pode nascer de um `brain-map` + uma camada de stats.

### 5. Importadores / conectores
- Trazer dados crus pras bases (cÃ©rebro 3, `sources/` â†’ `/nota` destila):
  **e-mail, Telegram, WhatsApp, export do ChatGPT, redes sociais, outros docs.**
- Reaproveita o processador Gemini (`file-intel`) pra docs; cada fonte vira um
  **conector** (export â†’ normaliza pra .md â†’ entra na inbox â†’ triagem/nota).
- ComeÃ§ar pelos exports fÃ¡ceis (ChatGPT JSON, e-mail .mbox, Telegram JSON) antes
  dos que pedem API/auth (WhatsApp, redes sociais).

## TensÃµes / perguntas pra pensar
1. **`3cerebros` Ã© um projeto novo ou a v2 do `cerebro-ARCKHÉ`?** (portÃ¡til = reescrever
   a camada de comando como adaptadores; talvez o cerebro-ARCKHÉ vire o `adapters/claude`).
2. **Autonomia x confianÃ§a:** qual o default â€” conservador (sÃ³ sugere) ou ativo?
   Quem aprova o quÃª? A polÃ­tica da entrevista resolve, mas precisa ser fÃ¡cil de mudar.
3. **Painel:** local-only (lÃª arquivos) ou serviÃ§o (pra Hermes 24/7 numa VPS)?
4. **Conectores:** quais primeiro? (voto: ChatGPT export + e-mail + Telegram â€” exports
   sem API). WhatsApp e redes sociais sÃ£o os mais chatos (auth/ToS).
5. **Privacidade:** importar e-mail/WhatsApp pro vault Ã© dado sensÃ­vel â€” criptografia?
   o que NUNCA sobe pro git? (liga com o 1Password/segredos que o vÃ­deo do Dudu citou).
6. **Multi-runtime de verdade vale o custo?** ou focar em Claude+Codex (polyskill jÃ¡
   cobre) e deixar os agentes (Hermes/AgentZeroâ€¦) como adaptadores futuros?

## RelaÃ§Ã£o com o que jÃ¡ temos
- `cerebro-ARCKHÉ` = a base (3 cÃ©rebros, Claude-Code, por comando). O `3cerebros`
  pega essa base e adiciona **portabilidade + autonomia + painel + conectores**.
- Camadas que jÃ¡ mapeamos servem de peÃ§a: kepano (formato), graphify (recuperaÃ§Ã£o),
  brain-map (visualizaÃ§Ã£o â†’ vira o painel). Ver [[comparacao-segundo-cerebros]].
