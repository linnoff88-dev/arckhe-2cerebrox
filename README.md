# 2cerebrox â€” Estudo sobre o uso de "Segundo CÃ©rebro"

<div align="center">
  <img src="capa/logo.svg" alt="Arckhé AI" height="48" />
  <p><strong>Arckhé AI</strong></p>
</div>


RepositÃ³rio de **pesquisa e notas** sobre como construir e usar um *second brain* com LLMs (Claude Code) + Obsidian. NÃ£o Ã© um produto â€” Ã© um caderno de estudo: comparaÃ§Ãµes de projetos existentes, frameworks mentais e uma proposta de arquitetura prÃ³pria.

## O que estamos estudando

A pergunta central: **qual a melhor forma de montar um "segundo cÃ©rebro" usando uma LLM?** Para responder, comparamos projetos open source, entendemos a lÃ³gica por trÃ¡s de cada abordagem (incl. a visÃ£o do Karpathy de *raw â†’ LLM â†’ wiki*), e destilamos um frame prÃ³prio.

### Principais conclusÃµes atÃ© aqui

1. **TrÃªs filosofias de second brain:**
   - *Conversa pura* (prompt que te entrevista, ex. `Luispitik/claude-code-second-brain`)
   - *raw â†’ LLM â†’ wiki* (a LLM lÃª o cru e organiza a wiki â€” a "linha Karpathy", ex. `inematds/second-brain`, `AgriciDaniel/claude-obsidian`)
   - *Obsidian manual* (vocÃª cura os links Ã  mÃ£o â€” Zettelkasten clÃ¡ssico)

2. **TrÃªs camadas, nÃ£o concorrentes:**
   - **OrganizaÃ§Ã£o** (o cÃ©rebro): Luispitik, inematds
   - **Formato** (as mÃ£os): `kepano/obsidian-skills` â€” escrever `.md`/`.base`/`.canvas` vÃ¡lidos
   - **RecuperaÃ§Ã£o** (a busca): `safishamsi/graphify` â€” grafo + MCP que o agente consulta (code-first)

3. **TrÃªs TIPOS de cÃ©rebro** (o frame que organiza tudo):
   - **Projeto** (episÃ³dico) â€” o que rolou, decisÃµes, estado
   - **Self** (identidade) â€” valores, pensamentos, dÃºvidas abertas
   - **Conhecimento** (referÃªncia) â€” fatos do mundo, o que aprendi

   â†’ Cada um quer espaÃ§o, regra e ferramenta prÃ³prios. Misturar = cÃ©rebro inchado.

## Documentos

| Doc | O que Ã© |
|---|---|
| [`docs/comparacao-segundo-cerebros.md`](docs/comparacao-segundo-cerebros.md) | AnÃ¡lise comparativa completa: 3 projetos â†’ panorama do GitHub â†’ Karpathy vs Obsidian â†’ kepano (formato) â†’ graphify (recuperaÃ§Ã£o) â†’ os 3 tipos de cÃ©rebro â†’ conclusÃ£o. |
| [`docs/estrutura-3-cerebros.md`](docs/estrutura-3-cerebros.md) | Proposta de **estrutura de pastas** dos 3 cÃ©rebros (Projeto/Self/Conhecimento), skills por zona, fluxo de uso e 6 decisÃµes em aberto pra pensar. |
| [`docs/ideia-3cerebros-portavel.md`](docs/ideia-3cerebros-portavel.md) | **Parking lot:** ideia de um `3cerebros` portÃ¡til (Claude/Codex/Hermes/AgentZero/OpenClawâ€¦), autÃ´nomo (entrevista â†’ polÃ­tica de automaÃ§Ã£o), com painel e conectores (email/Telegram/WhatsApp/ChatGPT export). Inclui gating "os sistemas jÃ¡ nÃ£o fazem isso?". |

## Status

ðŸ”¬ Em estudo. PrÃ³ximo passo provÃ¡vel: escolher as decisÃµes em aberto da proposta de arquitetura e montar o esqueleto de pastas real.

---

<div align="center">
  <img src="capa/logo.svg" alt="Arckhé AI" height="32" />
  <p><strong>Arckhé AI</strong></p>
</div>