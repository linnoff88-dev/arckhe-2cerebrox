# EsboêÂ§o: `3cerebros` evoluindo a partir do `intelecto`

> Arquitetura proposta. NêÂ£o êÂ© build ê¢â¬â êÂ© o desenho. Baseado na **arquitetura real**
> do `linnoff88-dev/intelecto` (lida em 2026-06-20), nêÂ£o em palpite.
> Liga com [[ideia-3cerebros-portavel]] (gating) e [[estrutura-3-cerebros]].

## Por que o intelecto êÂ© a base certa
O `intelecto` jêÂ¡ tem, prontos, 4 dos pilares que o `3cerebros` precisaria construir:

- **Stack de identidade em `.md`** ê¢â¬â `workspace/SOUL.md` (personalidade/valores),
  `AGENTS.md` (regras), `USER.md` (perfil), `MEMORY.md` (seed). Carregados por
  `agent/context.py` a cada mensagem. ê¢â â **jêÂ¡ êÂ© o cêÂ©rebro Self.**
- **MemêÂ³ria SQLite FTS5 + BM25** com categorias `fact / conversation / solution /
  preference`, dedup >80%, tools `save_memory / search_memory / forget_memory`. ê¢â â
  **jêÂ¡ êÂ© o motor do cêÂ©rebro Conhecimento.**
- **Interfaces abstratas** (`BaseProvider`, `BaseChannel`, `BaseTool` + registry):
  adicionar canal/ferramenta = arquivo novo, sem tocar no existente. ê¢â â **êÂ© o
  encaixe pros conectores e pras skills dos 3 cêÂ©rebros.**
- **Wizard de entrevista** (`wizard.py`, 6 passos) + `security/safety.py`
  (blocklist, confirmaêÂ§êÂ£o, audit log). ê¢â â **êÂ© o lugar da polêÂ­tica de automaêÂ§êÂ£o.**

E o **roadmap do prêÂ³prio intelecto** jêÂ¡ lista: `tools/cron.py` (autonomia),
`integrations/mcp.py` (conectores), `integrations/obsidian.py` (vault). Ou seja:
`3cerebros` êÂ© **quase um preenchimento do roadmap do intelecto** + a camada nova de
3 cêÂ©rebros e polêÂ­tica.

## A grande ideia: 1 cêÂ©rebro, 2 frontends
```
                ê¢âÅê¢ââ¬ê¢ââ¬ê¢ââ¬ê¢ââ¬ê¢ââ¬ê¢ââ¬ê¢ââ¬ê¢ââ¬ê¢ââ¬ê¢ââ¬ê¢ââ¬ O Cêâ°REBRO (formato êÂºnico) ê¢ââ¬ê¢ââ¬ê¢ââ¬ê¢ââ¬ê¢ââ¬ê¢ââ¬ê¢ââ¬ê¢ââ¬ê¢ââ¬ê¢ââ¬ê¢ââ¬ê¢âÂ
                ê¢ââ  workspace/ (.md, 3 cêÂ©rebros)  +  memory.db (FTS5) ê¢ââ
                ê¢ââê¢ââ¬ê¢ââ¬ê¢ââ¬ê¢ââ¬ê¢ââ¬ê¢ââ¬ê¢ââ¬ê¢ââ¬ê¢ââ¬ê¢ââ¬ê¢ââ¬ê¢ââ¬ê¢ââ¬ê¢ââ¬ê¢ââ¬ê¢âÂ¬ê¢ââ¬ê¢ââ¬ê¢ââ¬ê¢ââ¬ê¢ââ¬ê¢ââ¬ê¢ââ¬ê¢ââ¬ê¢ââ¬ê¢ââ¬ê¢ââ¬ê¢ââ¬ê¢ââ¬ê¢ââ¬ê¢ââ¬ê¢ââ¬ê¢ââ¬ê¢ââ¬ê¢ââ¬ê¢âÂ¬ê¢ââ¬ê¢ââ¬ê¢ââ¬ê¢ââ¬ê¢ââ¬ê¢ââ¬ê¢ââ¬ê¢ââ¬ê¢ââ¬ê¢ââ¬ê¢ââ¬ê¢ââ¬ê¢ââ¬ê¢ââ¬ê¢ââ¬ê¢âË
                                ê¢ââ                   ê¢ââ
                 ê¢âÅê¢ââ¬ê¢ââ¬ê¢ââ¬ê¢ââ¬ê¢ââ¬ê¢ââ¬ê¢ââ¬ê¢ââ¬ê¢ââ¬ê¢ââ¬ê¢ââ¬ê¢ââ¬ê¢ââ¬ê¢ââ¬ê¢âÂ´ê¢ââ¬ê¢ââ¬ê¢ââ¬ê¢âÂ      ê¢âÅê¢ââ¬ê¢ââ¬ê¢ââ¬ê¢ââ¬ê¢ââ¬ê¢ââ¬ê¢ââ¬ê¢ââ¬ê¢âÂ´ê¢ââ¬ê¢ââ¬ê¢ââ¬ê¢ââ¬ê¢ââ¬ê¢ââ¬ê¢ââ¬ê¢ââ¬ê¢ââ¬ê¢ââ¬ê¢ââ¬ê¢ââ¬ê¢ââ¬ê¢ââ¬ê¢âÂ
                 ê¢ââ  cerebro-ARCKHê    ê¢ââ      ê¢ââ      intelecto         ê¢ââ
                 ê¢ââ  (Claude Code,    ê¢ââ      ê¢ââ  (agente Python 24/7,  ê¢ââ
                 ê¢ââ   por comando,    ê¢ââ      ê¢ââ   Telegram, autêÂ´nomo)  ê¢ââ
                 ê¢ââ   Agent Skills)   ê¢ââ      ê¢ââ                        ê¢ââ
                 ê¢ââê¢ââ¬ê¢ââ¬ê¢ââ¬ê¢ââ¬ê¢ââ¬ê¢ââ¬ê¢ââ¬ê¢ââ¬ê¢ââ¬ê¢ââ¬ê¢ââ¬ê¢ââ¬ê¢ââ¬ê¢ââ¬ê¢ââ¬ê¢ââ¬ê¢ââ¬ê¢ââ¬ê¢ââ¬ê¢âË      ê¢ââê¢ââ¬ê¢ââ¬ê¢ââ¬ê¢ââ¬ê¢ââ¬ê¢ââ¬ê¢ââ¬ê¢ââ¬ê¢ââ¬ê¢ââ¬ê¢ââ¬ê¢ââ¬ê¢ââ¬ê¢ââ¬ê¢ââ¬ê¢ââ¬ê¢ââ¬ê¢ââ¬ê¢ââ¬ê¢ââ¬ê¢ââ¬ê¢ââ¬ê¢ââ¬ê¢ââ¬ê¢âË
```
O `cerebro-ARCKHê` (que jêÂ¡ construêÂ­mos) êÂ© o **frontend Claude-Code, por comando**.
O `intelecto` êÂ© o **frontend standalone, autêÂ´nomo, always-on**. O `3cerebros` êÂ© a
**unificaêÂ§êÂ£o**: os dois leem/escrevem o **mesmo formato de cêÂ©rebro** (pasta `.md` +
`memory.db`), e ganham por cima a **polêÂ­tica de automaêÂ§êÂ£o** (o gap real do gating).

## Mapa: recurso do 3cerebros ê¢â â mêÂ³dulo do intelecto

| Recurso 3cerebros | Onde encaixa no intelecto | Reusar / Adicionar |
|---|---|---|
| **CêÂ©rebro Self** | `workspace/SOUL.md` + `USER.md` + `AGENTS.md` | **reusar** (+ `values.md`, `questions/`) |
| **CêÂ©rebro Conhecimento** | `memory/store.py` (FTS5) + `integrations/obsidian.py` (roadmap) | reusar memêÂ³ria; **add** vault |
| **CêÂ©rebro Projeto** | categoria `conversation` + `workspace/projeto/` (daily, decisions) | **add** (pequeno) |
| **Triagem / nota (sem comando)** | `tools/registry.py` (novas tools) + `agent/loop.py` | **add** tools `triagem`, `nota` |
| **Modo autêÂ´nomo** | `tools/cron.py` (roadmap Fase 2) | **add** jobs (briefing, triagem, saêÂºde) |
| **Conectores** (email/TG/WA/ChatGPT) | `channels/` + `integrations/mcp.py` (roadmap Fase 3) | **reusar** MCP servers + importadores |
| **PolêÂ­tica de automaêÂ§êÂ£o** | `wizard.py` (entrevista) + `security/safety.py` | **add** `POLICY` + policy-engine ê¢â Â novidade |
| **Painel operacional** | (intelecto nêÂ£o tem web por filosofia) | **add** HTML local (brain-map) + status no Telegram |
| **Portabilidade** | formato `.md`+SQLite neutro; Agent Skills no lado Claude | **reusar** (2 frontends, 1 formato) |

## O diferencial, em detalhe: a PolêÂ­tica de AutomaêÂ§êÂ£o
êâ° o êÂºnico gap real do gating ê¢â¬â entêÂ£o êÂ© o coraêÂ§êÂ£o do `3cerebros`. Mecanismo:

1. **Entrevista (estende o `wizard.py`):** alêÂ©m de nome/modelo/personalidade, pergunta
   *o que o cêÂ©rebro pode fazer sozinho*. Gera `workspace/POLICY.md` (legêÂ­vel/editêÂ¡vel).
2. **NêÂ­veis por tipo de aêÂ§êÂ£o:** `observar` ê¢â â `sugerir` ê¢â â `agir com confirmaêÂ§êÂ£o` ê¢â â
   `agir sozinho`. Ex.:
   ```
   arquivar nota / atualizar daily      ê¢â â agir sozinho
   registrar decisêÂ£o / mover de cêÂ©rebro ê¢â â agir com confirmaêÂ§êÂ£o
   enviar email / postar / rodar shell  ê¢â â confirmaêÂ§êÂ£o sempre
   apagar / pagar / comprar             ê¢â â nunca (bloqueado)
   ```
3. **Enforcement (estende `security/safety.py`):** antes de cada tool-call no
   `agent/loop.py`, consulta a POLICY. JêÂ¡ existe o gancho (blocklist + audit log) ê¢â¬â
   vira um **policy-engine** que decide: executa / pede confirmaêÂ§êÂ£o no Telegram / nega.
4. **Tudo logado** no `audit.log` que jêÂ¡ existe. A polêÂ­tica êÂ© dado do **cêÂ©rebro Self**
   (deriva dos valores) ê¢â¬â fecha o ciclo com a skill `socratica`/valores.

## Como os 3 cêÂ©rebros assentam sobre o intelecto
```
~/.intelecto/                      (jêÂ¡ existe)
  memory.db        ê¢â â Conhecimento (fatos) + Projeto (conversation/solution)
  audit.log        ê¢â â trilha de tudo que a autonomia fez

workspace/                          (jêÂ¡ existe, .md)
  SOUL.md AGENTS.md USER.md         ê¢â â cêÂ©rebro SELF (identidade)
  values.md  questions/             ê¢â â SELF (add)
  POLICY.md                         ê¢â â polêÂ­tica de automaêÂ§êÂ£o (add) ê¢â Â novidade
  projeto/ daily/ decisions.md      ê¢â â cêÂ©rebro PROJETO (add)
  conhecimento/ (vault Obsidian)    ê¢â â cêÂ©rebro CONHECIMENTO (add, via integrations/obsidian.py)
  0-inbox/                          ê¢â â captura ê¢â â triagem autêÂ´noma (add)
```

## Plano em fases (piggyback no roadmap do intelecto)
- **Fase 0 ê¢â¬â formato:** definir a pasta `workspace/` dos 3 cêÂ©rebros + categorias do
  `memory.db`. Fazer o `cerebro-ARCKHê` e o `intelecto` lerem o mesmo layout.
- **Fase 1 ê¢â¬â polêÂ­tica (o diferencial):** `POLICY.md` + entrevista no wizard +
  policy-engine no `safety.py`/`loop.py`. EntregêÂ¡vel testêÂ¡vel sozinho.
- **Fase 2 ê¢â¬â autonomia:** `tools/cron.py` (roadmap) ê¢â â jobs de triagem, briefing diêÂ¡rio,
  destilar inbox, health ê¢â¬â todos **gated pela POLICY**.
- **Fase 3 ê¢â¬â conectores:** `integrations/mcp.py` (roadmap) + importadores
  (ChatGPT export, Telegram, email) ê¢â â caem em `0-inbox/` ê¢â â triagem.
- **Fase 4 ê¢â¬â painel:** HTML local estilo `brain-map` sobre `workspace/` + comandos de
  status no Telegram ("o que estêÂ¡ pendente de aprovaêÂ§êÂ£o?", "saêÂºde do cêÂ©rebro").

## Perguntas em aberto (pra pensar)
1. **`3cerebros` = repo novo ou branch/v2 do `intelecto`?** Voto: **evoluir o intelecto**
   (preenche o roadmap dele) e deixar `cerebro-ARCKHê` como o frontend Claude-Code.
2. **macOS-only:** o intelecto usa `launchd` + UUID de hardware (Fernet). Pra Linux
   (sua mêÂ¡quina) precisa de `systemd` + outra derivaêÂ§êÂ£o de chave. Quanto custa portar?
3. **Painel:** local-only (gera HTML on-demand, fiel êÂ  filosofia "sem web") vs serviêÂ§o.
   Voto: on-demand.
4. **Categorias da memêÂ³ria vs 3 cêÂ©rebros:** mapear `fact/solution`ê¢â âConhecimento,
   `conversation`ê¢â âProjeto, `preference`ê¢â âSelf? Ou adicionar uma coluna `cerebro`?
5. **ConfianêÂ§a:** comeêÂ§ar a POLICY **conservadora** (quase tudo "sugerir") e o usuêÂ¡rio
   solta gradualmente? (recomendado).
