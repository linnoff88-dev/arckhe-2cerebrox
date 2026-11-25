# Design: `3cerebros` isolado (embutêÂ­vel) + integraêÂ§êÂ£o no `intelecto`

> Foco pedido: implementar no **intelecto**, mas com o `3cerebros` como **repo
> isolado** que qualquer um incorpora no seu sistema via repo. Ancorado no cêÂ³digo
> real do `intelecto-testes` (FastAPI + WhatsApp/Evolution + OpenRouter, `soul.md`,
> mêÂ³dulos-stub `memory/knowledge/scheduler`). Ver [[analise-3cerebros-sobre-openpcbot]],
> [[esboco-3cerebros-sobre-intelecto]], [[ideia-3cerebros-portavel]].

## A separaêÂ§êÂ£o que vocêÂª pediu
```
ê¢âÅê¢ââ¬ê¢ââ¬ê¢ââ¬ê¢ââ¬ê¢ââ¬ê¢ââ¬ê¢ââ¬ê¢ââ¬ê¢ââ¬ê¢ââ¬ê¢ââ¬ê¢ââ¬ê¢ââ¬ê¢ââ¬ê¢ââ¬ê¢ââ¬ê¢ââ¬ê¢ââ¬ê¢ââ¬ê¢ââ¬ê¢ââ¬ê¢ââ¬ê¢ââ¬ê¢ââ¬ê¢ââ¬ê¢ââ¬ê¢ââ¬ê¢ââ¬ê¢ââ¬ê¢ââ¬ê¢ââ¬ê¢ââ¬ê¢ââ¬ê¢ââ¬ê¢ââ¬ê¢ââ¬ê¢ââ¬ê¢ââ¬ê¢ââ¬ê¢ââ¬ê¢ââ¬ê¢ââ¬ê¢ââ¬ê¢ââ¬ê¢ââ¬ê¢ââ¬ê¢ââ¬ê¢ââ¬ê¢ââ¬ê¢ââ¬ê¢ââ¬ê¢ââ¬ê¢ââ¬ê¢ââ¬ê¢ââ¬ê¢ââ¬ê¢ââ¬ê¢ââ¬ê¢ââ¬ê¢ââ¬ê¢âÂ
ê¢ââ  REPO ISOLADO: 3cerebros   (o Cêâ°REBRO ê¢â¬â embutêÂ­vel por qualquer um) ê¢ââ
ê¢ââ  ê¢â¬Â¢ formato (pasta .md + SQLite)  ê¢â¬Â¢ memêÂ³ria  ê¢â¬Â¢ polêÂ­tica       ê¢ââ
ê¢ââ  ê¢â¬Â¢ ingestêÂ£o (conectores)         ê¢â¬Â¢ skills triagem/nota/socratica ê¢ââ
ê¢ââ  exposto como: SDK (py/ts) êÂ· MCP server êÂ· Agent Skills êÂ· spec ê¢ââ
ê¢ââê¢ââ¬ê¢ââ¬ê¢ââ¬ê¢ââ¬ê¢ââ¬ê¢ââ¬ê¢ââ¬ê¢ââ¬ê¢ââ¬ê¢ââ¬ê¢ââ¬ê¢ââ¬ê¢ââ¬ê¢ââ¬ê¢ââ¬ê¢âÂ¬ê¢ââ¬ê¢ââ¬ê¢ââ¬ê¢ââ¬ê¢ââ¬ê¢ââ¬ê¢ââ¬ê¢ââ¬ê¢ââ¬ê¢ââ¬ê¢ââ¬ê¢ââ¬ê¢ââ¬ê¢ââ¬ê¢ââ¬ê¢ââ¬ê¢ââ¬ê¢ââ¬ê¢ââ¬ê¢ââ¬ê¢ââ¬ê¢ââ¬ê¢ââ¬ê¢ââ¬ê¢ââ¬ê¢ââ¬ê¢ââ¬ê¢ââ¬ê¢ââ¬ê¢ââ¬ê¢ââ¬ê¢ââ¬ê¢ââ¬ê¢ââ¬ê¢ââ¬ê¢ââ¬ê¢ââ¬ê¢ââ¬ê¢ââ¬ê¢ââ¬ê¢ââ¬ê¢ââ¬ê¢ââ¬ê¢ââ¬ê¢âË
                ê¢ââ incorporado via repo (submodule / pip / npm / MCP)
   ê¢âÅê¢ââ¬ê¢ââ¬ê¢ââ¬ê¢ââ¬ê¢ââ¬ê¢ââ¬ê¢ââ¬ê¢ââ¬ê¢ââ¬ê¢ââ¬ê¢ââ¬ê¢ââ¬ê¢âÂ¼ê¢ââ¬ê¢ââ¬ê¢ââ¬ê¢ââ¬ê¢ââ¬ê¢ââ¬ê¢ââ¬ê¢ââ¬ê¢ââ¬ê¢ââ¬ê¢ââ¬ê¢ââ¬ê¢ââ¬ê¢âÂ¬ê¢ââ¬ê¢ââ¬ê¢ââ¬ê¢ââ¬ê¢ââ¬ê¢ââ¬ê¢ââ¬ê¢ââ¬ê¢ââ¬ê¢ââ¬ê¢ââ¬ê¢ââ¬ê¢ââ¬ê¢ââ¬ê¢ââ¬ê¢ââ¬ê¢ââ¬ê¢ââ¬ê¢ââ¬ê¢ââ¬ê¢ââ¬ê¢ââ¬ê¢ââ¬ê¢âÂ
   ê¢âÂ¼            ê¢âÂ¼             ê¢âÂ¼                       ê¢âÂ¼
intelecto   openpcbot   cerebro-ARCKHê           sistema de terceiros
(jarvis,    (bot TS)    (Claude Code)           (qualquer agente)
 o "corpo")
```
O **3cerebros** êÂ© o cêÂ©rebro, isolado e reutilizêÂ¡vel. O **intelecto** êÂ© um **corpo de
jarvis** que o incorpora. Outros sistemas incorporam o mesmo repo do seu jeito.

## O repo `3cerebros` (isolado) ê¢â¬â o que tem dentro
```
3cerebros/
ê¢âÅê¢ââ¬ê¢ââ¬ spec/
ê¢ââ   ê¢âÅê¢ââ¬ê¢ââ¬ FORMAT.md            # a "constituiêÂ§êÂ£o": layout da pasta + schema SQLite
ê¢ââ   ê¢ââê¢ââ¬ê¢ââ¬ POLICY.md            # formato da polêÂ­tica de automaêÂ§êÂ£o
ê¢âÅê¢ââ¬ê¢ââ¬ core/                    # nêÂºcleo runtime-agnêÂ³stico (lêÂ³gica, sem I/O de canal)
ê¢ââ   ê¢âÅê¢ââ¬ê¢ââ¬ brain.py             # classe Brain (a API pêÂºblica) ê¢â¬â ver contrato abaixo
ê¢ââ   ê¢âÅê¢ââ¬ê¢ââ¬ memory.py            # SQLite FTS5/BM25, categoria por cêÂ©rebro (sector)
ê¢ââ   ê¢âÅê¢ââ¬ê¢ââ¬ policy.py            # policy-engine: decide agir/perguntar/negar
ê¢ââ   ê¢âÅê¢ââ¬ê¢ââ¬ triage.py            # roteia item ê¢â â cêÂ©rebro certo
ê¢ââ   ê¢âÅê¢ââ¬ê¢ââ¬ distill.py           # raw ê¢â â nota-wiki (cêÂ©rebro Conhecimento)
ê¢ââ   ê¢ââê¢ââ¬ê¢ââ¬ ingest/              # importadores: chatgpt_export, telegram, email, ê¢â¬Â¦
ê¢âÅê¢ââ¬ê¢ââ¬ sdk/
ê¢ââ   ê¢âÅê¢ââ¬ê¢ââ¬ python/              # `pip install cerebros` ê¢â¬â cliente fino sobre core/
ê¢ââ   ê¢ââê¢ââ¬ê¢ââ¬ ts/                  # `npm i @ARCKHê/cerebros` ê¢â¬â idem p/ hosts TS (openpcbot)
ê¢âÅê¢ââ¬ê¢ââ¬ mcp/
ê¢ââ   ê¢ââê¢ââ¬ê¢ââ¬ server.py            # expêÂµe o Brain como MCP server (qualquer runtime pluga)
ê¢âÅê¢ââ¬ê¢ââ¬ skills/                  # Agent Skills (Claude/Codex/ê¢â¬Â¦): triagem, nota, socratica
ê¢ââê¢ââ¬ê¢ââ¬ examples/                # host mêÂ­nimo de 30 linhas mostrando a integraêÂ§êÂ£o
```
DistribuiêÂ§êÂ£o: **git submodule** (`git submodule add ê¢â¬Â¦/3cerebros`), **pip/npm**, ou
sêÂ³ subir o **MCP server**. Tudo opcional ê¢â¬â o host escolhe a via.

## O contrato pêÂºblico (a êÂºnica coisa que o host precisa saber)
Uma superfêÂ­cie mêÂ­nima. Mesma semêÂ¢ntica em py, ts e MCP:

```python
brain = Brain(path="~/.cerebros")          # abre/cria o cêÂ©rebro (pasta + db)

ctx = brain.build_context(query, who)      # ê¢â â system prompt: Self + memêÂ³rias relevantes
brain.observe(who, user_msg, ai_reply)     # autêÂ´nomo: triagem + save no cêÂ©rebro certo
brain.ingest(source)                       # importa export (chatgpt/telegram/email) ê¢â â inbox
verdict = brain.policy.check(action)       # ê¢â â 'allow' | 'confirm' | 'deny'  (o diferencial)
brain.run_jobs()                           # tarefas de manutenêÂ§êÂ£o (cron chama isto)
```
Isso êÂ© tudo. O resto (formato, FTS5, 3 cêÂ©rebros, polêÂ­tica) fica **dentro** do repo.

## IntegraêÂ§êÂ£o no `intelecto-testes` (cirêÂºrgica, no `core.py`)
Hoje (`src/agent/core.py`):
```python
soul = load_soul(settings.soul_path)
response = await _provider.chat(messages=history, system=soul)
```
Com 3cerebros (o `soul.md` vira o seed do cêÂ©rebro **Self**):
```python
from cerebros import Brain
brain = Brain(path=settings.cerebros_path)             # incorpora o repo isolado

# 1. contexto = Self (inclui soul.md) + memêÂ³rias relevantes dos 3 cêÂ©rebros
system = brain.build_context(query=text, who=remote_jid)
response = await _provider.chat(messages=history, system=system)

# 2. observa e roteia sozinho (preenche os stubs memory/ e knowledge/)
brain.observe(who=remote_jid, user_msg=text, ai_reply=response)
```
E os mêÂ³dulos-stub do intelecto deixam de ser vazios ê¢â¬â viram **cascas finas**:
- `src/memory/` ê¢â â delega pra `brain` (save/search)
- `src/knowledge/` ê¢â â delega pra `brain.distill`/`ingest`
- `src/scheduler/` ê¢â â no tick do cron, chama `brain.run_jobs()`
- **antes de qualquer aêÂ§êÂ£o externa** (enviar WhatsApp proativo, rodar tool) ê¢â â
  `brain.policy.check(...)` decide. ê¢â Â a peêÂ§a nova que ninguêÂ©m do gating tem.

Resultado: o intelecto vira o **jarvis** (WhatsApp + LLM + agendador), e **toda a
inteligêÂªncia de memêÂ³ria/organizaêÂ§êÂ£o/autonomia governada** vem do repo 3cerebros.

## Como **qualquer um** incorpora (o requisito central)
| Host | Como incorpora |
|---|---|
| Python (intelecto, jarvis caseiro) | `pip install cerebros` ou submodule ê¢â â `from cerebros import Brain` |
| TypeScript (openpcbot) | `npm i @ARCKHê/cerebros` (SDK ts) **ou** falar com o MCP server |
| Claude Code / Codex / Gemini CLI | instalar as **Agent Skills** do `skills/` + (opcional) MCP |
| Qualquer outro runtime | subir `mcp/server.py` e usar as tools via **MCP** |
| SêÂ³ quer o formato | apontar o tool pro `~/.cerebros/` (pasta .md + db) ê¢â¬â êÂ© spec aberta |

O **MCP server** êÂ© o curinga: êÂ© o que torna o 3cerebros incorporêÂ¡vel por *qualquer*
sistema sem SDK ê¢â¬â exatamente o caminho que o gating mostrou ser o reuso padrêÂ£o.

## Plano em fases (casa com a "Fase 2 = SQLite" do intelecto)
- **F0 ê¢â¬â repo isolado:** criar `3cerebros` com `spec/FORMAT.md` + `core/brain.py` +
  `core/memory.py` (o intelecto jêÂ¡ ia fazer SQLite na Fase 2 ê¢â¬â fazer **dentro** do
  3cerebros e importar). Entrega: `Brain` com build_context/observe/save/search.
- **F1 ê¢â¬â polêÂ­tica (o diferencial):** `core/policy.py` + `spec/POLICY.md` + entrevista
  que gera a `POLICY.md` do usuêÂ¡rio. O intelecto passa a chamar `policy.check`.
- **F2 ê¢â¬â autonomia:** `run_jobs()` (triagem/briefing/saêÂºde) + `scheduler/` do intelecto
  chamando no cron, tudo sob polêÂ­tica.
- **F3 ê¢â¬â conectores:** `core/ingest/*` (chatgpt export, telegram, email via MCP) ê¢â â
  `0-inbox/` ê¢â â triagem. (WhatsApp o intelecto jêÂ¡ tem como canal.)
- **F4 ê¢â¬â skills + MCP + SDK ts:** empacotar as Agent Skills, subir o MCP server, e o
  SDK ts pro openpcbot ê¢â¬â fechando "um cêÂ©rebro, vêÂ¡rios frontends".

## Perguntas em aberto
1. **NêÂºcleo sêÂ³ Python ou Python+TS?** Voto: **core em Python** (intelecto êÂ© py) +
   **MCP** como ponte universal; SDK ts sêÂ³ se o openpcbot entrar de fato.
2. **`soul.md` do intelecto** vira o `2-self/SOUL.md` do cêÂ©rebro (mesmo arquivo) ê¢â¬â ok?
   Assim a personalidade do jarvis **êÂ©** o cêÂ©rebro Self. (recomendado)
3. **Onde mora o cêÂ©rebro:** `~/.cerebros/` (compartilhêÂ¡vel entre frontends) vs dentro
   do projeto. Voto: `~/.cerebros/` pra um cêÂ©rebro, vêÂ¡rios corpos.
4. **PolêÂ­tica default conservadora** (quase tudo "confirmar") e o usuêÂ¡rio solta? (sim).
5. **Nome do pacote/repo:** `3cerebros` / `cerebros` / `@ARCKHê/cerebros`?
