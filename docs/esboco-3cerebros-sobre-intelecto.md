# EsboÃ§o: `3cerebros` evoluindo a partir do `intelecto`

> Arquitetura proposta. NÃ£o Ã© build â€” Ã© o desenho. Baseado na **arquitetura real**
> do `linnoff88-dev/intelecto` (lida em 2026-06-20), nÃ£o em palpite.
> Liga com [[ideia-3cerebros-portavel]] (gating) e [[estrutura-3-cerebros]].

## Por que o intelecto Ã© a base certa
O `intelecto` jÃ¡ tem, prontos, 4 dos pilares que o `3cerebros` precisaria construir:

- **Stack de identidade em `.md`** â€” `workspace/SOUL.md` (personalidade/valores),
  `AGENTS.md` (regras), `USER.md` (perfil), `MEMORY.md` (seed). Carregados por
  `agent/context.py` a cada mensagem. â†’ **jÃ¡ Ã© o cÃ©rebro Self.**
- **MemÃ³ria SQLite FTS5 + BM25** com categorias `fact / conversation / solution /
  preference`, dedup >80%, tools `save_memory / search_memory / forget_memory`. â†’
  **jÃ¡ Ã© o motor do cÃ©rebro Conhecimento.**
- **Interfaces abstratas** (`BaseProvider`, `BaseChannel`, `BaseTool` + registry):
  adicionar canal/ferramenta = arquivo novo, sem tocar no existente. â†’ **Ã© o
  encaixe pros conectores e pras skills dos 3 cÃ©rebros.**
- **Wizard de entrevista** (`wizard.py`, 6 passos) + `security/safety.py`
  (blocklist, confirmaÃ§Ã£o, audit log). â†’ **Ã© o lugar da polÃ­tica de automaÃ§Ã£o.**

E o **roadmap do prÃ³prio intelecto** jÃ¡ lista: `tools/cron.py` (autonomia),
`integrations/mcp.py` (conectores), `integrations/obsidian.py` (vault). Ou seja:
`3cerebros` Ã© **quase um preenchimento do roadmap do intelecto** + a camada nova de
3 cÃ©rebros e polÃ­tica.

## A grande ideia: 1 cÃ©rebro, 2 frontends
```
                â”Œâ”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€ O CÃ‰REBRO (formato Ãºnico) â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”
                â”‚  workspace/ (.md, 3 cÃ©rebros)  +  memory.db (FTS5) â”‚
                â””â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”¬â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”¬â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”˜
                                â”‚                   â”‚
                 â”Œâ”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”´â”€â”€â”€â”      â”Œâ”€â”€â”€â”€â”€â”€â”€â”€â”´â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”
                 â”‚  cerebro-ARCKHÉ    â”‚      â”‚      intelecto         â”‚
                 â”‚  (Claude Code,    â”‚      â”‚  (agente Python 24/7,  â”‚
                 â”‚   por comando,    â”‚      â”‚   Telegram, autÃ´nomo)  â”‚
                 â”‚   Agent Skills)   â”‚      â”‚                        â”‚
                 â””â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”˜      â””â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”˜
```
O `cerebro-ARCKHÉ` (que jÃ¡ construÃ­mos) Ã© o **frontend Claude-Code, por comando**.
O `intelecto` Ã© o **frontend standalone, autÃ´nomo, always-on**. O `3cerebros` Ã© a
**unificaÃ§Ã£o**: os dois leem/escrevem o **mesmo formato de cÃ©rebro** (pasta `.md` +
`memory.db`), e ganham por cima a **polÃ­tica de automaÃ§Ã£o** (o gap real do gating).

## Mapa: recurso do 3cerebros â†’ mÃ³dulo do intelecto

| Recurso 3cerebros | Onde encaixa no intelecto | Reusar / Adicionar |
|---|---|---|
| **CÃ©rebro Self** | `workspace/SOUL.md` + `USER.md` + `AGENTS.md` | **reusar** (+ `values.md`, `questions/`) |
| **CÃ©rebro Conhecimento** | `memory/store.py` (FTS5) + `integrations/obsidian.py` (roadmap) | reusar memÃ³ria; **add** vault |
| **CÃ©rebro Projeto** | categoria `conversation` + `workspace/projeto/` (daily, decisions) | **add** (pequeno) |
| **Triagem / nota (sem comando)** | `tools/registry.py` (novas tools) + `agent/loop.py` | **add** tools `triagem`, `nota` |
| **Modo autÃ´nomo** | `tools/cron.py` (roadmap Fase 2) | **add** jobs (briefing, triagem, saÃºde) |
| **Conectores** (email/TG/WA/ChatGPT) | `channels/` + `integrations/mcp.py` (roadmap Fase 3) | **reusar** MCP servers + importadores |
| **PolÃ­tica de automaÃ§Ã£o** | `wizard.py` (entrevista) + `security/safety.py` | **add** `POLICY` + policy-engine â† novidade |
| **Painel operacional** | (intelecto nÃ£o tem web por filosofia) | **add** HTML local (brain-map) + status no Telegram |
| **Portabilidade** | formato `.md`+SQLite neutro; Agent Skills no lado Claude | **reusar** (2 frontends, 1 formato) |

## O diferencial, em detalhe: a PolÃ­tica de AutomaÃ§Ã£o
Ã‰ o Ãºnico gap real do gating â€” entÃ£o Ã© o coraÃ§Ã£o do `3cerebros`. Mecanismo:

1. **Entrevista (estende o `wizard.py`):** alÃ©m de nome/modelo/personalidade, pergunta
   *o que o cÃ©rebro pode fazer sozinho*. Gera `workspace/POLICY.md` (legÃ­vel/editÃ¡vel).
2. **NÃ­veis por tipo de aÃ§Ã£o:** `observar` â†’ `sugerir` â†’ `agir com confirmaÃ§Ã£o` â†’
   `agir sozinho`. Ex.:
   ```
   arquivar nota / atualizar daily      â†’ agir sozinho
   registrar decisÃ£o / mover de cÃ©rebro â†’ agir com confirmaÃ§Ã£o
   enviar email / postar / rodar shell  â†’ confirmaÃ§Ã£o sempre
   apagar / pagar / comprar             â†’ nunca (bloqueado)
   ```
3. **Enforcement (estende `security/safety.py`):** antes de cada tool-call no
   `agent/loop.py`, consulta a POLICY. JÃ¡ existe o gancho (blocklist + audit log) â€”
   vira um **policy-engine** que decide: executa / pede confirmaÃ§Ã£o no Telegram / nega.
4. **Tudo logado** no `audit.log` que jÃ¡ existe. A polÃ­tica Ã© dado do **cÃ©rebro Self**
   (deriva dos valores) â€” fecha o ciclo com a skill `socratica`/valores.

## Como os 3 cÃ©rebros assentam sobre o intelecto
```
~/.intelecto/                      (jÃ¡ existe)
  memory.db        â†’ Conhecimento (fatos) + Projeto (conversation/solution)
  audit.log        â†’ trilha de tudo que a autonomia fez

workspace/                          (jÃ¡ existe, .md)
  SOUL.md AGENTS.md USER.md         â†’ cÃ©rebro SELF (identidade)
  values.md  questions/             â†’ SELF (add)
  POLICY.md                         â†’ polÃ­tica de automaÃ§Ã£o (add) â† novidade
  projeto/ daily/ decisions.md      â†’ cÃ©rebro PROJETO (add)
  conhecimento/ (vault Obsidian)    â†’ cÃ©rebro CONHECIMENTO (add, via integrations/obsidian.py)
  0-inbox/                          â†’ captura â†’ triagem autÃ´noma (add)
```

## Plano em fases (piggyback no roadmap do intelecto)
- **Fase 0 â€” formato:** definir a pasta `workspace/` dos 3 cÃ©rebros + categorias do
  `memory.db`. Fazer o `cerebro-ARCKHÉ` e o `intelecto` lerem o mesmo layout.
- **Fase 1 â€” polÃ­tica (o diferencial):** `POLICY.md` + entrevista no wizard +
  policy-engine no `safety.py`/`loop.py`. EntregÃ¡vel testÃ¡vel sozinho.
- **Fase 2 â€” autonomia:** `tools/cron.py` (roadmap) â†’ jobs de triagem, briefing diÃ¡rio,
  destilar inbox, health â€” todos **gated pela POLICY**.
- **Fase 3 â€” conectores:** `integrations/mcp.py` (roadmap) + importadores
  (ChatGPT export, Telegram, email) â†’ caem em `0-inbox/` â†’ triagem.
- **Fase 4 â€” painel:** HTML local estilo `brain-map` sobre `workspace/` + comandos de
  status no Telegram ("o que estÃ¡ pendente de aprovaÃ§Ã£o?", "saÃºde do cÃ©rebro").

## Perguntas em aberto (pra pensar)
1. **`3cerebros` = repo novo ou branch/v2 do `intelecto`?** Voto: **evoluir o intelecto**
   (preenche o roadmap dele) e deixar `cerebro-ARCKHÉ` como o frontend Claude-Code.
2. **macOS-only:** o intelecto usa `launchd` + UUID de hardware (Fernet). Pra Linux
   (sua mÃ¡quina) precisa de `systemd` + outra derivaÃ§Ã£o de chave. Quanto custa portar?
3. **Painel:** local-only (gera HTML on-demand, fiel Ã  filosofia "sem web") vs serviÃ§o.
   Voto: on-demand.
4. **Categorias da memÃ³ria vs 3 cÃ©rebros:** mapear `fact/solution`â†’Conhecimento,
   `conversation`â†’Projeto, `preference`â†’Self? Ou adicionar uma coluna `cerebro`?
5. **ConfianÃ§a:** comeÃ§ar a POLICY **conservadora** (quase tudo "sugerir") e o usuÃ¡rio
   solta gradualmente? (recomendado).
