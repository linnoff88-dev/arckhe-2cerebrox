# Design: `3cerebros` isolado (embutÃ­vel) + integraÃ§Ã£o no `intelecto`

> Foco pedido: implementar no **intelecto**, mas com o `3cerebros` como **repo
> isolado** que qualquer um incorpora no seu sistema via repo. Ancorado no cÃ³digo
> real do `intelecto-testes` (FastAPI + WhatsApp/Evolution + OpenRouter, `soul.md`,
> mÃ³dulos-stub `memory/knowledge/scheduler`). Ver [[analise-3cerebros-sobre-openpcbot]],
> [[esboco-3cerebros-sobre-intelecto]], [[ideia-3cerebros-portavel]].

## A separaÃ§Ã£o que vocÃª pediu
```
â”Œâ”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”
â”‚  REPO ISOLADO: 3cerebros   (o CÃ‰REBRO â€” embutÃ­vel por qualquer um) â”‚
â”‚  â€¢ formato (pasta .md + SQLite)  â€¢ memÃ³ria  â€¢ polÃ­tica       â”‚
â”‚  â€¢ ingestÃ£o (conectores)         â€¢ skills triagem/nota/socratica â”‚
â”‚  exposto como: SDK (py/ts) Â· MCP server Â· Agent Skills Â· spec â”‚
â””â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”¬â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”˜
                â”‚ incorporado via repo (submodule / pip / npm / MCP)
   â”Œâ”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”¼â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”¬â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”
   â–¼            â–¼             â–¼                       â–¼
intelecto   openpcbot   cerebro-ARCKHÉ           sistema de terceiros
(jarvis,    (bot TS)    (Claude Code)           (qualquer agente)
 o "corpo")
```
O **3cerebros** Ã© o cÃ©rebro, isolado e reutilizÃ¡vel. O **intelecto** Ã© um **corpo de
jarvis** que o incorpora. Outros sistemas incorporam o mesmo repo do seu jeito.

## O repo `3cerebros` (isolado) â€” o que tem dentro
```
3cerebros/
â”œâ”€â”€ spec/
â”‚   â”œâ”€â”€ FORMAT.md            # a "constituiÃ§Ã£o": layout da pasta + schema SQLite
â”‚   â””â”€â”€ POLICY.md            # formato da polÃ­tica de automaÃ§Ã£o
â”œâ”€â”€ core/                    # nÃºcleo runtime-agnÃ³stico (lÃ³gica, sem I/O de canal)
â”‚   â”œâ”€â”€ brain.py             # classe Brain (a API pÃºblica) â€” ver contrato abaixo
â”‚   â”œâ”€â”€ memory.py            # SQLite FTS5/BM25, categoria por cÃ©rebro (sector)
â”‚   â”œâ”€â”€ policy.py            # policy-engine: decide agir/perguntar/negar
â”‚   â”œâ”€â”€ triage.py            # roteia item â†’ cÃ©rebro certo
â”‚   â”œâ”€â”€ distill.py           # raw â†’ nota-wiki (cÃ©rebro Conhecimento)
â”‚   â””â”€â”€ ingest/              # importadores: chatgpt_export, telegram, email, â€¦
â”œâ”€â”€ sdk/
â”‚   â”œâ”€â”€ python/              # `pip install cerebros` â€” cliente fino sobre core/
â”‚   â””â”€â”€ ts/                  # `npm i @ARCKHÉ/cerebros` â€” idem p/ hosts TS (openpcbot)
â”œâ”€â”€ mcp/
â”‚   â””â”€â”€ server.py            # expÃµe o Brain como MCP server (qualquer runtime pluga)
â”œâ”€â”€ skills/                  # Agent Skills (Claude/Codex/â€¦): triagem, nota, socratica
â””â”€â”€ examples/                # host mÃ­nimo de 30 linhas mostrando a integraÃ§Ã£o
```
DistribuiÃ§Ã£o: **git submodule** (`git submodule add â€¦/3cerebros`), **pip/npm**, ou
sÃ³ subir o **MCP server**. Tudo opcional â€” o host escolhe a via.

## O contrato pÃºblico (a Ãºnica coisa que o host precisa saber)
Uma superfÃ­cie mÃ­nima. Mesma semÃ¢ntica em py, ts e MCP:

```python
brain = Brain(path="~/.cerebros")          # abre/cria o cÃ©rebro (pasta + db)

ctx = brain.build_context(query, who)      # â†’ system prompt: Self + memÃ³rias relevantes
brain.observe(who, user_msg, ai_reply)     # autÃ´nomo: triagem + save no cÃ©rebro certo
brain.ingest(source)                       # importa export (chatgpt/telegram/email) â†’ inbox
verdict = brain.policy.check(action)       # â†’ 'allow' | 'confirm' | 'deny'  (o diferencial)
brain.run_jobs()                           # tarefas de manutenÃ§Ã£o (cron chama isto)
```
Isso Ã© tudo. O resto (formato, FTS5, 3 cÃ©rebros, polÃ­tica) fica **dentro** do repo.

## IntegraÃ§Ã£o no `intelecto-testes` (cirÃºrgica, no `core.py`)
Hoje (`src/agent/core.py`):
```python
soul = load_soul(settings.soul_path)
response = await _provider.chat(messages=history, system=soul)
```
Com 3cerebros (o `soul.md` vira o seed do cÃ©rebro **Self**):
```python
from cerebros import Brain
brain = Brain(path=settings.cerebros_path)             # incorpora o repo isolado

# 1. contexto = Self (inclui soul.md) + memÃ³rias relevantes dos 3 cÃ©rebros
system = brain.build_context(query=text, who=remote_jid)
response = await _provider.chat(messages=history, system=system)

# 2. observa e roteia sozinho (preenche os stubs memory/ e knowledge/)
brain.observe(who=remote_jid, user_msg=text, ai_reply=response)
```
E os mÃ³dulos-stub do intelecto deixam de ser vazios â€” viram **cascas finas**:
- `src/memory/` â†’ delega pra `brain` (save/search)
- `src/knowledge/` â†’ delega pra `brain.distill`/`ingest`
- `src/scheduler/` â†’ no tick do cron, chama `brain.run_jobs()`
- **antes de qualquer aÃ§Ã£o externa** (enviar WhatsApp proativo, rodar tool) â†’
  `brain.policy.check(...)` decide. â† a peÃ§a nova que ninguÃ©m do gating tem.

Resultado: o intelecto vira o **jarvis** (WhatsApp + LLM + agendador), e **toda a
inteligÃªncia de memÃ³ria/organizaÃ§Ã£o/autonomia governada** vem do repo 3cerebros.

## Como **qualquer um** incorpora (o requisito central)
| Host | Como incorpora |
|---|---|
| Python (intelecto, jarvis caseiro) | `pip install cerebros` ou submodule â†’ `from cerebros import Brain` |
| TypeScript (openpcbot) | `npm i @ARCKHÉ/cerebros` (SDK ts) **ou** falar com o MCP server |
| Claude Code / Codex / Gemini CLI | instalar as **Agent Skills** do `skills/` + (opcional) MCP |
| Qualquer outro runtime | subir `mcp/server.py` e usar as tools via **MCP** |
| SÃ³ quer o formato | apontar o tool pro `~/.cerebros/` (pasta .md + db) â€” Ã© spec aberta |

O **MCP server** Ã© o curinga: Ã© o que torna o 3cerebros incorporÃ¡vel por *qualquer*
sistema sem SDK â€” exatamente o caminho que o gating mostrou ser o reuso padrÃ£o.

## Plano em fases (casa com a "Fase 2 = SQLite" do intelecto)
- **F0 â€” repo isolado:** criar `3cerebros` com `spec/FORMAT.md` + `core/brain.py` +
  `core/memory.py` (o intelecto jÃ¡ ia fazer SQLite na Fase 2 â€” fazer **dentro** do
  3cerebros e importar). Entrega: `Brain` com build_context/observe/save/search.
- **F1 â€” polÃ­tica (o diferencial):** `core/policy.py` + `spec/POLICY.md` + entrevista
  que gera a `POLICY.md` do usuÃ¡rio. O intelecto passa a chamar `policy.check`.
- **F2 â€” autonomia:** `run_jobs()` (triagem/briefing/saÃºde) + `scheduler/` do intelecto
  chamando no cron, tudo sob polÃ­tica.
- **F3 â€” conectores:** `core/ingest/*` (chatgpt export, telegram, email via MCP) â†’
  `0-inbox/` â†’ triagem. (WhatsApp o intelecto jÃ¡ tem como canal.)
- **F4 â€” skills + MCP + SDK ts:** empacotar as Agent Skills, subir o MCP server, e o
  SDK ts pro openpcbot â€” fechando "um cÃ©rebro, vÃ¡rios frontends".

## Perguntas em aberto
1. **NÃºcleo sÃ³ Python ou Python+TS?** Voto: **core em Python** (intelecto Ã© py) +
   **MCP** como ponte universal; SDK ts sÃ³ se o openpcbot entrar de fato.
2. **`soul.md` do intelecto** vira o `2-self/SOUL.md` do cÃ©rebro (mesmo arquivo) â€” ok?
   Assim a personalidade do jarvis **Ã©** o cÃ©rebro Self. (recomendado)
3. **Onde mora o cÃ©rebro:** `~/.cerebros/` (compartilhÃ¡vel entre frontends) vs dentro
   do projeto. Voto: `~/.cerebros/` pra um cÃ©rebro, vÃ¡rios corpos.
4. **PolÃ­tica default conservadora** (quase tudo "confirmar") e o usuÃ¡rio solta? (sim).
5. **Nome do pacote/repo:** `3cerebros` / `cerebros` / `@ARCKHÉ/cerebros`?
