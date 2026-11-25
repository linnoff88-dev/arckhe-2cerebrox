# Ideia para pensar: projeto `3cerebros` ê¢â¬â portêÂ¡til, autêÂ´nomo, com painel

> Status: **ideia / parking lot**. NêÂ£o êÂ© pra construir agora ê¢â¬â êÂ© um ponto pra
> pensarmos. EvoluêÂ§êÂ£o do `cerebro-ARCKHê` (que hoje êÂ© Claude-Code-first e por
> comando) para algo **portêÂ¡til entre runtimes, autêÂ´nomo e monitorêÂ¡vel**.
> Capturado: 2026-06-20.

## A semente (o que o Nei pediu)
Um projeto novo, talvez `3cerebros`, que:
1. seja **portado pra rodar em vêÂ¡rios runtimes**: Claude Code, Codex, e agentes
   como **Hermes, AgentZero, OpenClaw, Intelecto**ê¢â¬Â¦
2. tenha **dicas de instalar e de usar** (por runtime);
3. **funcione sem comandos** ê¢â¬â em vez de vocêÂª disparar `/triagem`, `/nota` etc.,
   ele **se autogerencia**;
4. para isso, faêÂ§a uma **entrevista** que define **o que pode e o que nêÂ£o pode
   ser automatizado**;
5. tenha um **painel pra monitorar** o que tem dentro dele;
6. tenha **importadores de dados** pras bases: e-mail, Telegram, WhatsApp,
   export do ChatGPT, redes sociais, e outros docs.

## ê¢Å¡Â ê¯Â¸Â ANTES de construir: os sistemas jêÂ¡ nêÂ£o fazem isso? (gating)

Regra do Nei: **checar se os sistemas existentes jêÂ¡ têÂªm esses recursos** antes de
construir. Pesquisa feita (3 agentes web, 2026-06-20). Legenda de confianêÂ§a:
**ê¢Åâ¦ verificado** (evidêÂªncia/observaêÂ§êÂ£o direta) êÂ· **ê¢Å¡Â ê¯Â¸Â alegaêÂ§êÂ£o web nêÂ£o confirmada**
(nêÂºmero/data veio de agente, tratar com ceticismo). CorreêÂ§êÂ£o: `zubair-trabzada/brain-map`
**existe** (clonado nesta sessêÂ£o, 16ê¢Â­Â) ê¢â¬â um agente disse que nêÂ£o; ignore.

### Matriz A ê¢â¬â projetos de second-brain (com evidêÂªncia/URL)

| Projeto | AutêÂ´nomo | Conectores | Painel | Cross-runtime | Entrevistaê¢â âpolêÂ­tica |
|---|---|---|---|---|---|
| khoj-ai/khoj | parcial (cronê¢â âemail) | parcial (PDF/Notion/GitHub) | parcial (admin Django) | **nêÂ£o** (êÂ© o runtime) | nêÂ£o |
| AgriciDaniel/claude-obsidian | parcial (`/autoresearch`, auto-commit 15min) | parcial (drop + Web Clipper) | **sim** (`dashboard.base`/Dataview) | sim (exp.: Codex/Cursor/Gemini) | nêÂ£o ("what is this vault for?") |
| SamurAIGPT/llm-wiki-agent | nêÂ£o | parcial (markitdown: PDF/DOCXê¢â¬Â¦) | nêÂ£o (`graph.html` estêÂ¡tico) | **sim** (Claude/Codex/Gemini/OpenCode) | nêÂ£o |
| huytieu/COG-second-brain | nêÂ£o (17 skills manuais) | parcial (Linear/GitHub/Jira/Slack/Notion) | parcial (Tasks) | **sim** (6 surfaces) | **parcial** (`/onboarding` ê¢â â integraêÂ§êÂµes) |
| henrydaum/second-brain | **sim** (cron Timekeeper + watcher) | parcial (Telegram nativo, OCR, whisper) | parcial (`/debug`, heartbeat) | sim (AGENTS.md) | parcial (`/setup`, limites hardcoded) |
| eugeniughelbur/obsidian-second-brain | **sim** (4 cron + PostCompact, "mantêÂ©m enquanto vocêÂª dorme") | parcial (Telegram, Calendar, X, YouTube) | parcial (`/obsidian-health`) | **sim** ("one codebase, four platforms") | nêÂ£o |

### Matriz B ê¢â¬â runtimes/agentes (ê¢Å¡Â ê¯Â¸Â vêÂ¡rios nêÂºmeros/datas nêÂ£o confirmados)

| Runtime | MemêÂ³ria nativa | AutêÂ´nomo (cron/daemon) | Mensageria nativa | MCP | Acoplar skills .md |
|---|---|---|---|---|---|
| Claude Code | parcial (arquivos+tasks) | sim (tasks+hooks) | nêÂ£o (via MCP) | ê¢Åâ¦ sim | ê¢Åâ¦ excelente (padrêÂ£o SKILL.md) |
| Codex CLI | parcial (AGENTS.md) | parcial (modos approval) | nêÂ£o | ê¢Åâ¦ sim | parcial (AGENTS.md) |
| Hermes ê¢Å¡Â ê¯Â¸Â | sim (alegado) | sim (cron) | sim (TG/WA/Discordê¢â¬Â¦) | sim | sim |
| agent-zero ê¢Å¡Â ê¯Â¸Â | sim (vetorial) | sim (multi-agente Docker) | parcial (plugin) | sim | sim (SKILL.md) |
| OpenClaw ê¢Å¡Â ê¯Â¸Â | parcial (plugins) | sim (daemon, computer-use) | sim (30+ plataformas) | sim | sim (registry) |
| **Intelecto** = `linnoff88-dev/intelecto` ê¢Å¡Â ê¯Â¸Â | sim (SQLite FTS5 + SOUL/AGENTS/USER.md) | nêÂ£o | sim (Telegram) | ambêÂ­guo | ê¢Åâ¦ excelente (jêÂ¡ êÂ© .md-driven) ê¢â¬â **êÂ© seu prêÂ³prio projeto** |

### Infra pronta pra REUSAR (nêÂ£o construir)
- **Conectores via MCP:** Gmail (`GongRzhe/Gmail-MCP-Server`), Telegram (`chigwell/telegram-mcp`),
  WhatsApp (`lharries/whatsapp-mcp` ê¢â¬â ê¢Å¡Â ê¯Â¸Â bridge nêÂ£o-oficial, risco de ban), redes sociais (**Postiz**, OSS).
- **Importar exports ê¢â â Markdown:** ChatGPT (`gavi/chatgpt-markdown`, `Nexus AI Chat Importer`),
  Telegram (`tg2obsidian`), WhatsApp (plugin Obsidian). ChatGPT export = `conversations.json` (êÂ¡rvore).
- **Portabilidade:** **Agent Skills spec** (agentskills.io, padrêÂ£o aberto Anthropic) ê¢â¬â mesma skill em ~20 runtimes;
  vêÂ¡rios projetos jêÂ¡ fazem via `AGENTS.md` + `build.sh --platform`.
- **Painel base:** `brain-map` (grafo + timeline + click-to-inspect) e `graphify` (grafo queryêÂ¡vel + MCP).

### Veredito (responde "os sistemas jêÂ¡ nêÂ£o fazem isso?")
**JêÂ¡ fazem ê¢â¬â quase tudo.** Dos 5 recursos:
- **Portabilidade** ê¢â â ê¢Åâ¦ **resolvido** (Agent Skills spec + AGENTS.md). NêÂ£o construir; seguir o padrêÂ£o.
- **Modo autêÂ´nomo** ê¢â â ê¢Åâ¦ **jêÂ¡ existe pronto** (henrydaum e eugeniughelbur rodam cron/background "enquanto vocêÂª dorme"). Copiar o padrêÂ£o, nêÂ£o reinventar.
- **Conectores** ê¢â â ê¢Åâ¦ **reusêÂ¡veis** via MCP + importadores .md (gap nos projetos de brain, mas a infra existe). SêÂ³ plugar.
- **Painel** ê¢â â parcial (saêÂºde em texto + brain-map/graphify visual). Falta sêÂ³ a camada **operacional** (o que foi automatizado / pendente de aprovaêÂ§êÂ£o).
- **Entrevista ê¢â â polêÂ­tica de automaêÂ§êÂ£o** ê¢â â ê¢Ââ **GAP REAL.** Nenhum dos 6 tem polêÂ­tica de *permissêÂµes de autonomia* (COG chega perto, mas êÂ© mapa de integraêÂ§êÂµes, nêÂ£o de limites).

**ConclusêÂ£o:** o que sobra de genuinamente novo êÂ© fino ê¢â¬â **(1) a separaêÂ§êÂ£o em 3 cêÂ©rebros** (Projeto/Self/Conhecimento, que ninguêÂ©m faz explêÂ­cito) + **(2) autonomia governada por uma entrevista que define o que pode/nêÂ£o pode** + **(3) um painel operacional**. Todo o resto êÂ© **integrar/configurar** peêÂ§as existentes. EntêÂ£o `3cerebros`, se acontecer, êÂ© **integration-first**: monta sobre MCP + Agent Skills spec + um background-agent (estilo eugeniughelbur) + brain-map, e constrêÂ³i sêÂ³ a camada fina de polêÂ­tica+3-cêÂ©rebros+painel.

**Atalho possêÂ­vel:** como **Intelecto** (`linnoff88-dev/intelecto`) jêÂ¡ êÂ© um agente .md-driven com memêÂ³ria e Telegram **e êÂ© seu**, o `3cerebros` pode nascer como uma *evoluêÂ§êÂ£o dele* (adicionar os 3 cêÂ©rebros + polêÂ­tica), em vez de projeto do zero.

## Como eu leio isso (5 frentes)

### 1. Portabilidade cross-runtime
- A base jêÂ¡ ajuda: os 3 cêÂ©rebros sêÂ£o **arquivos .md + pastas** ê¢â¬â formato neutro
  que qualquer agente lêÂª. O que muda por runtime êÂ© sêÂ³ a **camada de skills/comandos**.
- Caminho: seguir a **[Agent Skills spec](https://agentskills.io)** (a mesma que o
  kepano usa) ê¢â â uma fonte, vêÂ¡rios runtimes. A skill **`polyskill`** (jêÂ¡ instalada
  aqui) empacota skill pra rodar em Claude Code **e** Codex a partir de um fonte
  portêÂ¡til. Para Hermes/AgentZero/OpenClaw/Intelecto, mapear o equivalente de
  "skill/hook" de cada um (alguns leem `AGENTS.md`, outros têÂªm formato prêÂ³prio).
- EntregêÂ¡vel: um `core/` neutro (os 3 cêÂ©rebros + regras) + adaptadores finos por
  runtime (`adapters/claude`, `adapters/codex`, `adapters/hermes`ê¢â¬Â¦).

### 2. Onboarding (instalar + usar por runtime)
- Um instalador que detecta/pergunta o runtime e escreve os arquivos certos
  (no Claude vira `.claude/skills`, no Codex `~/.codex/skills`, no Hermesê¢â¬Â¦).
- Guia de uso por runtime ê¢â¬â provavelmente uma landing+guia (como a do cerebro-ARCKHê)
  com abas por agente.

### 3. Modo autêÂ´nomo (sem comandos) ê¢â¬â **o pulo do gato**
- Hoje: vocêÂª roda `/triagem`, `/nota`, `/tldr`. No `3cerebros`: o agente faz isso
  **sozinho** ê¢â¬â observa o que entra e roteia/destila/registra sem vocêÂª pedir.
- O risco êÂ³bvio: autonomia sem limite assusta e erra. Por isso a **entrevista**:
  ela gera uma **polêÂ­tica de automaêÂ§êÂ£o** ê¢â¬â o que ele PODE fazer sozinho (ex.:
  arquivar nota, atualizar daily) vs o que **pede confirmaêÂ§êÂ£o** (ex.: mandar
  e-mail, mover decisêÂ£o, apagar). 
- Onde isso mora: essa polêÂ­tica êÂ© parte do **cêÂ©rebro Self** (valores ê¢â â permissêÂµes).
  Liga com [[estrutura-3-cerebros]] e a skill `socratica`/`setup-cerebro`.
- NêÂ­veis de autonomia (sugestêÂ£o): **observar** ê¢â â **sugerir** ê¢â â **agir com
  confirmaêÂ§êÂ£o** ê¢â â **agir sozinho** (por tipo de aêÂ§êÂ£o). Tudo logado.

### 4. Painel de monitoramento
- Um dashboard local (estilo brain-map, mas com estado, nêÂ£o sêÂ³ grafo) que mostra:
  o que tem em cada cêÂ©rebro, o que foi automatizado, o que entrou de cada fonte,
  o que estêÂ¡ esperando confirmaêÂ§êÂ£o, e a "saêÂºde" (nota êÂ³rfêÂ£, inbox acumulando).
- Self-contained, lêÂª os `.md` ê¢â¬â pode nascer de um `brain-map` + uma camada de stats.

### 5. Importadores / conectores
- Trazer dados crus pras bases (cêÂ©rebro 3, `sources/` ê¢â â `/nota` destila):
  **e-mail, Telegram, WhatsApp, export do ChatGPT, redes sociais, outros docs.**
- Reaproveita o processador Gemini (`file-intel`) pra docs; cada fonte vira um
  **conector** (export ê¢â â normaliza pra .md ê¢â â entra na inbox ê¢â â triagem/nota).
- ComeêÂ§ar pelos exports fêÂ¡ceis (ChatGPT JSON, e-mail .mbox, Telegram JSON) antes
  dos que pedem API/auth (WhatsApp, redes sociais).

## TensêÂµes / perguntas pra pensar
1. **`3cerebros` êÂ© um projeto novo ou a v2 do `cerebro-ARCKHê`?** (portêÂ¡til = reescrever
   a camada de comando como adaptadores; talvez o cerebro-ARCKHê vire o `adapters/claude`).
2. **Autonomia x confianêÂ§a:** qual o default ê¢â¬â conservador (sêÂ³ sugere) ou ativo?
   Quem aprova o quêÂª? A polêÂ­tica da entrevista resolve, mas precisa ser fêÂ¡cil de mudar.
3. **Painel:** local-only (lêÂª arquivos) ou serviêÂ§o (pra Hermes 24/7 numa VPS)?
4. **Conectores:** quais primeiro? (voto: ChatGPT export + e-mail + Telegram ê¢â¬â exports
   sem API). WhatsApp e redes sociais sêÂ£o os mais chatos (auth/ToS).
5. **Privacidade:** importar e-mail/WhatsApp pro vault êÂ© dado sensêÂ­vel ê¢â¬â criptografia?
   o que NUNCA sobe pro git? (liga com o 1Password/segredos que o vêÂ­deo do Dudu citou).
6. **Multi-runtime de verdade vale o custo?** ou focar em Claude+Codex (polyskill jêÂ¡
   cobre) e deixar os agentes (Hermes/AgentZeroê¢â¬Â¦) como adaptadores futuros?

## RelaêÂ§êÂ£o com o que jêÂ¡ temos
- `cerebro-ARCKHê` = a base (3 cêÂ©rebros, Claude-Code, por comando). O `3cerebros`
  pega essa base e adiciona **portabilidade + autonomia + painel + conectores**.
- Camadas que jêÂ¡ mapeamos servem de peêÂ§a: kepano (formato), graphify (recuperaêÂ§êÂ£o),
  brain-map (visualizaêÂ§êÂ£o ê¢â â vira o painel). Ver [[comparacao-segundo-cerebros]].
