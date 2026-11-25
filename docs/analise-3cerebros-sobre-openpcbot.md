# AnêÂ¡lise: implementar o `3cerebros` no `openpcbot`

> AnêÂ¡lise ancorada na arquitetura **real** do `openpcbot` (lida em 2026-06-20:
> `CLAUDE.md`, `ARQUITETURA_SECOND_BRAIN.md`, êÂ¡rvore do projeto).
> Compara com o esboêÂ§o sobre o intelecto ([[esboco-3cerebros-sobre-intelecto]]).
> Gating: [[ideia-3cerebros-portavel]].

## TL;DR
**O openpcbot êÂ© o melhor hospedeiro dos dois.** Ele jêÂ¡ entrega, prontos, **4 dos 5
recursos** que o gating apontou como necessêÂ¡rios ê¢â¬â e **roda no Linux** (a sua
mêÂ¡quina), ao contrêÂ¡rio do intelecto (macOS/launchd). Implementar o `3cerebros` aqui
êÂ©, na prêÂ¡tica: **(1) trocar o vault plano pelos 3 cêÂ©rebros** + **(2) adicionar a
polêÂ­tica de automaêÂ§êÂ£o** (o êÂºnico gap real). O resto jêÂ¡ existe.

## O que o openpcbot JêÂ êÂ© (verificado no cêÂ³digo)
Bot Telegram em **TypeScript** que usa o **Claude Code como motor** (escreve no vault
via `fs`, lêÂª/sintetiza delegando ao Claude). JêÂ¡ tem:

| Recurso do gating | No openpcbot (evidêÂªncia) | Status |
|---|---|---|
| **Modo autêÂ´nomo / cron** | `dist/schedule-cli.js create "PROMPT" "CRON"` ê¢â¬â tarefas agendadas que rodam o Claude | ê¢Åâ¦ **existe** |
| **Painel / dashboard** | `dist/dashboard.js` + `dashboard-html.js` (+ assets de preview) | ê¢Åâ¦ **existe** |
| **MemêÂ³ria estruturada** | SQLite `store/openpcbot.db`, tabela `memories(content, sector, salience, ê¢â¬Â¦)` + sessions + token_usage | ê¢Åâ¦ **existe** (jêÂ¡ com setores!) |
| **Conectores** | Telegram nativo, **WhatsApp bridge**, voz (ARCKHêVOX/Whisper), Gmail, Google Calendar, Skool, YouTube/TikTok | ê¢Åâ¦ **muitos** |
| **Multi-agente** | `agents/{comms,content,ops,research}/` (agent.yaml + CLAUDE.md) + `_template` | ê¢Åâ¦ **existe** |
| **Vault second-brain** | `vault-template/` (inbox/daily/projects/research/archive + memory.md) + skills `daily`/`tldr`/`file-intel`/`vault-setup` | ê¢Åâ¦ (mas êÂ© o **vault plano**, nêÂ£o 3 cêÂ©rebros) |
| **Sem comando (triggers naturais)** | regex em `bot.ts` ("guarda isso", "salva no brain") salva direto no inbox | ê¢Åâ¦ parcial |
| **Roda no Linux** | serviêÂ§o persistente Mac/Linux; `cd /home/nmaldaner/projetos/openpcbot` | ê¢Åâ¦ |
| **PolêÂ­tica de automaêÂ§êÂ£o** | ê¢â¬â nêÂ£o existe (tem cron + execuêÂ§êÂ£o, mas sem camada de permissêÂµes) | ê¢ÂÅ **GAP** |
| **SeparaêÂ§êÂ£o em 3 cêÂ©rebros** | ê¢â¬â o vault êÂ© plano (inbox/daily/projects/research/archive) | ê¢ÂÅ **a fazer** |

## O que falta ê¢â¬â e êÂ© sêÂ³ isto
1. **3 cêÂ©rebros:** hoje o vault êÂ© plano. Migrar `inbox/daily/projects/research/archive`
   ê¢â â `0-inbox/ 1-projeto/ 2-self/ 3-conhecimento/`. A tabela `memories` **jêÂ¡ tem a
   coluna `sector`** ê¢â¬â dêÂ¡ pra etiquetar cada memêÂ³ria por cêÂ©rebro sem mudar schema.
2. **PolêÂ­tica de automaêÂ§êÂ£o:** o diferencial do gating. openpcbot jêÂ¡ age sozinho (cron +
   triggers), mas **sem permissêÂµes** ê¢â¬â exatamente o risco. Adicionar `POLICY.md` +
   um gate que toda aêÂ§êÂ£o autêÂ´noma consulta. êâ° a peêÂ§a nova de verdade.

## Mapa: recurso do 3cerebros ê¢â â onde encaixa no openpcbot

| Recurso | Onde encaixa | Reusar / Add |
|---|---|---|
| CêÂ©rebro **Self** | `CLAUDE.md` (personalidade/regras jêÂ¡ existe) + `2-self/values.md` | reusar + add |
| CêÂ©rebro **Conhecimento** | `memories`(sector) + vault `3-conhecimento/` + `file-intel` | reusar + reorganizar |
| CêÂ©rebro **Projeto** | vault `1-projeto/` (daily/decisions/projects) + `memories` | reorganizar |
| **Triagem/nota sem comando** | regex de trigger em `bot.ts` (jêÂ¡ existe) + cron classificando inbox | **add** (rota pro cêÂ©rebro certo) |
| **Modo autêÂ´nomo** | `schedule-cli` (jêÂ¡ existe) ê¢â â jobs de triagem/briefing/saêÂºde | **reusar** |
| **Conectores** | Telegram/WhatsApp/voz/Gmail/Calendar (jêÂ¡ existem) ê¢â â caem no `0-inbox/` | **reusar** |
| **PolêÂ­tica de automaêÂ§êÂ£o** | `CLAUDE.md` + novo `POLICY.md` + gate antes de cada aêÂ§êÂ£o/cron | **add** ê¢â Â novidade |
| **Painel operacional** | `dashboard.js` (jêÂ¡ existe) ê¢â â adicionar visêÂ£o dos 3 cêÂ©rebros + fila de aprovaêÂ§êÂ£o | **estender** |
| **Multi-agente** | `agents/{comms,content,ops,research}` ê¢â â workers operando os cêÂ©rebros sob polêÂ­tica | **reaproveitar** |

## O êÂ¢ngulo multi-agente (exclusivo do openpcbot)
O openpcbot jêÂ¡ tem **agentes especializados** (comms, content, ops, research). No
`3cerebros` eles viram os **trabalhadores autêÂ´nomos** que mantêÂªm os cêÂ©rebros:
- `research` ê¢â â destila fontes pro **Conhecimento** (`/nota`)
- `ops` ê¢â â roteia inbox, arquiva, roda saêÂºde (**Projeto**)
- `comms` ê¢â â conectores de mensageria (ingestêÂ£o pro `0-inbox/`)
- `content` ê¢â â usa o cêÂ©rebro pra produzir (ponte com o portal/ARCKHê)
Cada um **governado pela mesma POLICY**. Isso êÂ© algo que nem o intelecto nem os
projetos do gating têÂªm.

## openpcbot vs intelecto ê¢â¬â qual hospedeiro?

| CritêÂ©rio | **openpcbot** | **intelecto** |
|---|---|---|
| Linguagem | TypeScript (mais cêÂ³digo) | Python (~3k linhas, enxuto) |
| JêÂ¡ tem autonomia (cron) | ê¢Åâ¦ sim | ê¢ÂÅ (roadmap) |
| JêÂ¡ tem dashboard | ê¢Åâ¦ sim | ê¢ÂÅ (filosofia "sem web") |
| JêÂ¡ tem conectores | ê¢Åâ¦ muitos | sêÂ³ Telegram |
| JêÂ¡ tem multi-agente | ê¢Åâ¦ sim | ê¢ÂÅ |
| MemêÂ³ria | SQLite com sector/salience | SQLite FTS5/BM25 |
| Roda no Linux | ê¢Åâ¦ sim | ê¢ÂÅ (macOS launchd) |
| Simplicidade / clareza | menor (mais peêÂ§as) | **maior** (limpo, interfaces abstratas) |
| EsforêÂ§o pro 3cerebros | **menor** (sêÂ³ vault+policy) | maior (construir autonomia+painel+conectores) |

**RecomendaêÂ§êÂ£o:** para um `3cerebros` **funcional rêÂ¡pido**, o **openpcbot** êÂ© o caminho
ê¢â¬â jêÂ¡ tem quase tudo e roda no seu Linux; falta sêÂ³ os 3 cêÂ©rebros + a polêÂ­tica. O
**intelecto** êÂ© a base mais limpa se o objetivo for um produto **enxuto e portêÂ¡vel**
pra distribuir (menos peêÂ§as, cêÂ³digo legêÂ­vel). NêÂ£o sêÂ£o excludentes: o **formato de
cêÂ©rebro** (pasta `.md` + SQLite) pode ser **o mesmo** nos dois, com o `cerebro-ARCKHê`
(Claude Code) como terceiro frontend. Um cêÂ©rebro, trêÂªs frontends.

## Plano em fases (openpcbot)
- **F0 ê¢â¬â vault ê¢â â 3 cêÂ©rebros:** migrar `vault-template/` e o `vault-setup` pra estrutura
  dos 3 cêÂ©rebros; etiquetar `memories.sector` por cêÂ©rebro. Instalar as skills
  `triagem`/`nota`/`socratica` (do `cerebro-ARCKHê`) em `~/.claude/skills/`.
- **F1 ê¢â¬â polêÂ­tica (o diferencial):** `POLICY.md` + gate consultado por toda aêÂ§êÂ£o
  autêÂ´noma (cron + triggers) + fila de aprovaêÂ§êÂ£o no Telegram.
- **F2 ê¢â¬â autonomia governada:** registrar jobs no `schedule-cli` (triagem da inbox,
  briefing diêÂ¡rio, saêÂºde do cêÂ©rebro), todos sob POLICY.
- **F3 ê¢â¬â painel:** estender `dashboard.js` com a visêÂ£o dos 3 cêÂ©rebros + o que estêÂ¡
  pendente de aprovaêÂ§êÂ£o + saêÂºde (notas êÂ³rfêÂ£s, inbox acumulando).
- **F4 ê¢â¬â multi-agente:** ligar comms/content/ops/research como workers dos cêÂ©rebros.

## Perguntas em aberto
1. **Host êÂºnico ou os dois?** Voto: prototipar no **openpcbot** (rêÂ¡pido, Linux), manter
   o **formato de cêÂ©rebro** compatêÂ­vel com intelecto e cerebro-ARCKHê.
2. **`memories.sector`** jêÂ¡ existe ê¢â¬â usar pra marcar cêÂ©rebro (projeto/self/conhecimento)
   ou criar coluna nova? (provêÂ¡vel: reusar `sector`).
3. **PolêÂ­tica x triggers naturais:** hoje "guarda isso" salva direto. Sob POLICY isso
   continua livre (baixo risco), mas enviar email/postar passa a pedir confirmaêÂ§êÂ£o.
4. **Quanto da autonomia herda do cron atual** sem reescrever? (provêÂ¡vel: muito).
