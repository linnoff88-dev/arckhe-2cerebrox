ï»¿# 2cerebrox ê¢â¬â Estudo sobre o uso de "Segundo CêÂ©rebro"

<div align="center">
  <img src="capa/logo.svg" alt="Arckhé AI" height="48" />
  <p><strong>Arckhé AI</strong></p>
</div>


RepositêÂ³rio de **pesquisa e notas** sobre como construir e usar um *second brain* com LLMs (Claude Code) + Obsidian. NêÂ£o êÂ© um produto ê¢â¬â êÂ© um caderno de estudo: comparaêÂ§êÂµes de projetos existentes, frameworks mentais e uma proposta de arquitetura prêÂ³pria.

## O que estamos estudando

A pergunta central: **qual a melhor forma de montar um "segundo cêÂ©rebro" usando uma LLM?** Para responder, comparamos projetos open source, entendemos a lêÂ³gica por trêÂ¡s de cada abordagem (incl. a visêÂ£o do Karpathy de *raw ê¢â â LLM ê¢â â wiki*), e destilamos um frame prêÂ³prio.

### Principais conclusêÂµes atêÂ© aqui

1. **TrêÂªs filosofias de second brain:**
   - *Conversa pura* (prompt que te entrevista, ex. `Luispitik/claude-code-second-brain`)
   - *raw ê¢â â LLM ê¢â â wiki* (a LLM lêÂª o cru e organiza a wiki ê¢â¬â a "linha Karpathy", ex. `linnoff88-dev/second-brain`, `AgriciDaniel/claude-obsidian`)
   - *Obsidian manual* (vocêÂª cura os links êÂ  mêÂ£o ê¢â¬â Zettelkasten clêÂ¡ssico)

2. **TrêÂªs camadas, nêÂ£o concorrentes:**
   - **OrganizaêÂ§êÂ£o** (o cêÂ©rebro): Luispitik, linnoff88-dev
   - **Formato** (as mêÂ£os): `kepano/obsidian-skills` ê¢â¬â escrever `.md`/`.base`/`.canvas` vêÂ¡lidos
   - **RecuperaêÂ§êÂ£o** (a busca): `safishamsi/graphify` ê¢â¬â grafo + MCP que o agente consulta (code-first)

3. **TrêÂªs TIPOS de cêÂ©rebro** (o frame que organiza tudo):
   - **Projeto** (episêÂ³dico) ê¢â¬â o que rolou, decisêÂµes, estado
   - **Self** (identidade) ê¢â¬â valores, pensamentos, dêÂºvidas abertas
   - **Conhecimento** (referêÂªncia) ê¢â¬â fatos do mundo, o que aprendi

   ê¢â â Cada um quer espaêÂ§o, regra e ferramenta prêÂ³prios. Misturar = cêÂ©rebro inchado.

## Documentos

| Doc | O que êÂ© |
|---|---|
| [`docs/comparacao-segundo-cerebros.md`](docs/comparacao-segundo-cerebros.md) | AnêÂ¡lise comparativa completa: 3 projetos ê¢â â panorama do GitHub ê¢â â Karpathy vs Obsidian ê¢â â kepano (formato) ê¢â â graphify (recuperaêÂ§êÂ£o) ê¢â â os 3 tipos de cêÂ©rebro ê¢â â conclusêÂ£o. |
| [`docs/estrutura-3-cerebros.md`](docs/estrutura-3-cerebros.md) | Proposta de **estrutura de pastas** dos 3 cêÂ©rebros (Projeto/Self/Conhecimento), skills por zona, fluxo de uso e 6 decisêÂµes em aberto pra pensar. |
| [`docs/ideia-3cerebros-portavel.md`](docs/ideia-3cerebros-portavel.md) | **Parking lot:** ideia de um `3cerebros` portêÂ¡til (Claude/Codex/Hermes/AgentZero/OpenClawê¢â¬Â¦), autêÂ´nomo (entrevista ê¢â â polêÂ­tica de automaêÂ§êÂ£o), com painel e conectores (email/Telegram/WhatsApp/ChatGPT export). Inclui gating "os sistemas jêÂ¡ nêÂ£o fazem isso?". |

## Status

ê°Å¸âÂ¬ Em estudo. PrêÂ³ximo passo provêÂ¡vel: escolher as decisêÂµes em aberto da proposta de arquitetura e montar o esqueleto de pastas real.

---

<div align="center">
  <img src="capa/logo.svg" alt="Arckhé AI" height="32" />
  <p><strong>Arckhé AI</strong></p>
</div>