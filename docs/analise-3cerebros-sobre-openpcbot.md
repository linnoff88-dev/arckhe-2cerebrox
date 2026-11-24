# AnÃ¡lise: implementar o `3cerebros` no `openpcbot`

> AnÃ¡lise ancorada na arquitetura **real** do `openpcbot` (lida em 2026-06-20:
> `CLAUDE.md`, `ARQUITETURA_SECOND_BRAIN.md`, Ã¡rvore do projeto).
> Compara com o esboÃ§o sobre o intelecto ([[esboco-3cerebros-sobre-intelecto]]).
> Gating: [[ideia-3cerebros-portavel]].

## TL;DR
**O openpcbot Ã© o melhor hospedeiro dos dois.** Ele jÃ¡ entrega, prontos, **4 dos 5
recursos** que o gating apontou como necessÃ¡rios â€” e **roda no Linux** (a sua
mÃ¡quina), ao contrÃ¡rio do intelecto (macOS/launchd). Implementar o `3cerebros` aqui
Ã©, na prÃ¡tica: **(1) trocar o vault plano pelos 3 cÃ©rebros** + **(2) adicionar a
polÃ­tica de automaÃ§Ã£o** (o Ãºnico gap real). O resto jÃ¡ existe.

## O que o openpcbot JÃ Ã© (verificado no cÃ³digo)
Bot Telegram em **TypeScript** que usa o **Claude Code como motor** (escreve no vault
via `fs`, lÃª/sintetiza delegando ao Claude). JÃ¡ tem:

| Recurso do gating | No openpcbot (evidÃªncia) | Status |
|---|---|---|
| **Modo autÃ´nomo / cron** | `dist/schedule-cli.js create "PROMPT" "CRON"` â€” tarefas agendadas que rodam o Claude | âœ… **existe** |
| **Painel / dashboard** | `dist/dashboard.js` + `dashboard-html.js` (+ assets de preview) | âœ… **existe** |
| **MemÃ³ria estruturada** | SQLite `store/openpcbot.db`, tabela `memories(content, sector, salience, â€¦)` + sessions + token_usage | âœ… **existe** (jÃ¡ com setores!) |
| **Conectores** | Telegram nativo, **WhatsApp bridge**, voz (ARCKHÉVOX/Whisper), Gmail, Google Calendar, Skool, YouTube/TikTok | âœ… **muitos** |
| **Multi-agente** | `agents/{comms,content,ops,research}/` (agent.yaml + CLAUDE.md) + `_template` | âœ… **existe** |
| **Vault second-brain** | `vault-template/` (inbox/daily/projects/research/archive + memory.md) + skills `daily`/`tldr`/`file-intel`/`vault-setup` | âœ… (mas Ã© o **vault plano**, nÃ£o 3 cÃ©rebros) |
| **Sem comando (triggers naturais)** | regex em `bot.ts` ("guarda isso", "salva no brain") salva direto no inbox | âœ… parcial |
| **Roda no Linux** | serviÃ§o persistente Mac/Linux; `cd /home/nmaldaner/projetos/openpcbot` | âœ… |
| **PolÃ­tica de automaÃ§Ã£o** | â€” nÃ£o existe (tem cron + execuÃ§Ã£o, mas sem camada de permissÃµes) | âŒ **GAP** |
| **SeparaÃ§Ã£o em 3 cÃ©rebros** | â€” o vault Ã© plano (inbox/daily/projects/research/archive) | âŒ **a fazer** |

## O que falta â€” e Ã© sÃ³ isto
1. **3 cÃ©rebros:** hoje o vault Ã© plano. Migrar `inbox/daily/projects/research/archive`
   â†’ `0-inbox/ 1-projeto/ 2-self/ 3-conhecimento/`. A tabela `memories` **jÃ¡ tem a
   coluna `sector`** â€” dÃ¡ pra etiquetar cada memÃ³ria por cÃ©rebro sem mudar schema.
2. **PolÃ­tica de automaÃ§Ã£o:** o diferencial do gating. openpcbot jÃ¡ age sozinho (cron +
   triggers), mas **sem permissÃµes** â€” exatamente o risco. Adicionar `POLICY.md` +
   um gate que toda aÃ§Ã£o autÃ´noma consulta. Ã‰ a peÃ§a nova de verdade.

## Mapa: recurso do 3cerebros â†’ onde encaixa no openpcbot

| Recurso | Onde encaixa | Reusar / Add |
|---|---|---|
| CÃ©rebro **Self** | `CLAUDE.md` (personalidade/regras jÃ¡ existe) + `2-self/values.md` | reusar + add |
| CÃ©rebro **Conhecimento** | `memories`(sector) + vault `3-conhecimento/` + `file-intel` | reusar + reorganizar |
| CÃ©rebro **Projeto** | vault `1-projeto/` (daily/decisions/projects) + `memories` | reorganizar |
| **Triagem/nota sem comando** | regex de trigger em `bot.ts` (jÃ¡ existe) + cron classificando inbox | **add** (rota pro cÃ©rebro certo) |
| **Modo autÃ´nomo** | `schedule-cli` (jÃ¡ existe) â†’ jobs de triagem/briefing/saÃºde | **reusar** |
| **Conectores** | Telegram/WhatsApp/voz/Gmail/Calendar (jÃ¡ existem) â†’ caem no `0-inbox/` | **reusar** |
| **PolÃ­tica de automaÃ§Ã£o** | `CLAUDE.md` + novo `POLICY.md` + gate antes de cada aÃ§Ã£o/cron | **add** â† novidade |
| **Painel operacional** | `dashboard.js` (jÃ¡ existe) â†’ adicionar visÃ£o dos 3 cÃ©rebros + fila de aprovaÃ§Ã£o | **estender** |
| **Multi-agente** | `agents/{comms,content,ops,research}` â†’ workers operando os cÃ©rebros sob polÃ­tica | **reaproveitar** |

## O Ã¢ngulo multi-agente (exclusivo do openpcbot)
O openpcbot jÃ¡ tem **agentes especializados** (comms, content, ops, research). No
`3cerebros` eles viram os **trabalhadores autÃ´nomos** que mantÃªm os cÃ©rebros:
- `research` â†’ destila fontes pro **Conhecimento** (`/nota`)
- `ops` â†’ roteia inbox, arquiva, roda saÃºde (**Projeto**)
- `comms` â†’ conectores de mensageria (ingestÃ£o pro `0-inbox/`)
- `content` â†’ usa o cÃ©rebro pra produzir (ponte com o portal/ARCKHÉ)
Cada um **governado pela mesma POLICY**. Isso Ã© algo que nem o intelecto nem os
projetos do gating tÃªm.

## openpcbot vs intelecto â€” qual hospedeiro?

| CritÃ©rio | **openpcbot** | **intelecto** |
|---|---|---|
| Linguagem | TypeScript (mais cÃ³digo) | Python (~3k linhas, enxuto) |
| JÃ¡ tem autonomia (cron) | âœ… sim | âŒ (roadmap) |
| JÃ¡ tem dashboard | âœ… sim | âŒ (filosofia "sem web") |
| JÃ¡ tem conectores | âœ… muitos | sÃ³ Telegram |
| JÃ¡ tem multi-agente | âœ… sim | âŒ |
| MemÃ³ria | SQLite com sector/salience | SQLite FTS5/BM25 |
| Roda no Linux | âœ… sim | âŒ (macOS launchd) |
| Simplicidade / clareza | menor (mais peÃ§as) | **maior** (limpo, interfaces abstratas) |
| EsforÃ§o pro 3cerebros | **menor** (sÃ³ vault+policy) | maior (construir autonomia+painel+conectores) |

**RecomendaÃ§Ã£o:** para um `3cerebros` **funcional rÃ¡pido**, o **openpcbot** Ã© o caminho
â€” jÃ¡ tem quase tudo e roda no seu Linux; falta sÃ³ os 3 cÃ©rebros + a polÃ­tica. O
**intelecto** Ã© a base mais limpa se o objetivo for um produto **enxuto e portÃ¡vel**
pra distribuir (menos peÃ§as, cÃ³digo legÃ­vel). NÃ£o sÃ£o excludentes: o **formato de
cÃ©rebro** (pasta `.md` + SQLite) pode ser **o mesmo** nos dois, com o `cerebro-ARCKHÉ`
(Claude Code) como terceiro frontend. Um cÃ©rebro, trÃªs frontends.

## Plano em fases (openpcbot)
- **F0 â€” vault â†’ 3 cÃ©rebros:** migrar `vault-template/` e o `vault-setup` pra estrutura
  dos 3 cÃ©rebros; etiquetar `memories.sector` por cÃ©rebro. Instalar as skills
  `triagem`/`nota`/`socratica` (do `cerebro-ARCKHÉ`) em `~/.claude/skills/`.
- **F1 â€” polÃ­tica (o diferencial):** `POLICY.md` + gate consultado por toda aÃ§Ã£o
  autÃ´noma (cron + triggers) + fila de aprovaÃ§Ã£o no Telegram.
- **F2 â€” autonomia governada:** registrar jobs no `schedule-cli` (triagem da inbox,
  briefing diÃ¡rio, saÃºde do cÃ©rebro), todos sob POLICY.
- **F3 â€” painel:** estender `dashboard.js` com a visÃ£o dos 3 cÃ©rebros + o que estÃ¡
  pendente de aprovaÃ§Ã£o + saÃºde (notas Ã³rfÃ£s, inbox acumulando).
- **F4 â€” multi-agente:** ligar comms/content/ops/research como workers dos cÃ©rebros.

## Perguntas em aberto
1. **Host Ãºnico ou os dois?** Voto: prototipar no **openpcbot** (rÃ¡pido, Linux), manter
   o **formato de cÃ©rebro** compatÃ­vel com intelecto e cerebro-ARCKHÉ.
2. **`memories.sector`** jÃ¡ existe â€” usar pra marcar cÃ©rebro (projeto/self/conhecimento)
   ou criar coluna nova? (provÃ¡vel: reusar `sector`).
3. **PolÃ­tica x triggers naturais:** hoje "guarda isso" salva direto. Sob POLICY isso
   continua livre (baixo risco), mas enviar email/postar passa a pedir confirmaÃ§Ã£o.
4. **Quanto da autonomia herda do cron atual** sem reescrever? (provÃ¡vel: muito).
