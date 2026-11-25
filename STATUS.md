# STATUS ê¢â¬â manhêÂ£ de 2026-06-20

Resumo do que rodou autêÂ´nomo enquanto vocêÂª dormia. Os **3 pontos** (build ê¢â â teach ê¢â â publish), na ordem que combinamos.

---

## VisêÂ£o rêÂ¡pida

| Ponto | O quêÂª | Status | Onde |
|---|---|---|---|
| 0 | Repo histêÂ³rico atualizado | ê¢Åâ¦ | github.com/linnoff88-dev/2cerebrox |
| 1 | **BUILD** ê¢â¬â produto cerebro-ARCKHê | ê¢Åâ¦ | github.com/linnoff88-dev/cerebro-ARCKHê |
| 2 | **TEACH** ê¢â¬â landing+guia (GitHub Pages) | ê¢Åâ¦ no ar | linnoff88-dev.github.io/ |
| 3 | **PUBLISH** ê¢â¬â vêÂ­deo explicativo | ê¢ÂÂ³ ver êÂ§3 | ~/projetos/output/cerebro-ARCKHê-video |

---

## 0 êÂ· Repo histêÂ³rico (2cerebrox)
A pesquisa toda ficou aqui como histêÂ³rico. Adicionei a êÂ§7 (camada de VisualizaêÂ§êÂ£o / brain-map) e este STATUS.md. O doc principal `docs/comparacao-segundo-cerebros.md` estêÂ¡ com 9 seêÂ§êÂµes; `docs/estrutura-3-cerebros.md` tem a proposta de arquitetura.

## 1 êÂ· BUILD ê¢â¬â cerebro-ARCKHê (ê¢Åâ¦)
Produto novo, **pêÂºblico**: https://github.com/linnoff88-dev/cerebro-ARCKHê

Implementa a arquitetura dos 3 cêÂ©rebros (decisêÂµes em aberto fechadas com defaults ê¢â¬â ver lista abaixo):
- **Estrutura:** `0-inbox/ 1-projeto/ 2-self/ 3-conhecimento/` (vault-template)
- **CLAUDE.md roteador** ê¢â¬â mapa dos 3 cêÂ©rebros + regras + roteamento
- **Skills:** `triagem`, `socratica`, `nota` (novas) + `daily`, `tldr`, `setup-cerebro` (adaptadas) ê¢â¬â todas em texto livre, sem AskUserQuestion (sua regra global)
- **setup.sh** PT-BR/Linux+mac ê¢â¬â **testado** (smoke test passou, gera o vault completo)
- **docs/** arquitetura (3 cêÂ©rebros êâ 4 camadas) + integraêÂ§êÂ£o kepano
- Scripts Gemini (ingestêÂ£o rawê¢â âwiki) reaproveitados do fork

**Defaults que bati (reverta se quiser):**
1. Vault êÂºnico (nêÂ£o trêÂªs separados)
2. Prefixo numêÂ©rico nas pastas (`1-projeto/`ê¢â¬Â¦)
3. Journal (self) separado do daily (projeto)
4. Pergunta resolvida migra de `2-self/questions` ê¢â â `3-conhecimento/notes`
5. graphify fora do core (documentado como opcional)
6. Nomes de pasta + conteêÂºdo em PT-BR

## 2 êÂ· TEACH ê¢â¬â landing+guia (ê¢Åâ¦ no ar)
PêÂ¡gina êÂºnica ARCKHê dark êÂ¢mbar, dark/light toggle, ARCKHê AI no nav e footer.
- **No ar:** https://linnoff88-dev.github.io/ (HTTP 200 confirmado)
- Fonte: `index.html` na raiz do cerebro-ARCKHê (self-contained)
- SeêÂ§êÂµes: hero (diagrama dos 3 cêÂ©rebros) êÂ· o que êÂ© êÂ· como funciona êÂ· prêÂ©-requisitos êÂ· guia passo a passo (comandos reais) êÂ· exemplos êÂ· as 4 camadas êÂ· footer

## 3 êÂ· PUBLISH ê¢â¬â vêÂ­deo explicativo (ê¢ÂÂ³)
Projeto HyperFrames em `~/projetos/output/cerebro-ARCKHê-video/`.
- **Roteiro** `SCRIPT.md` ê¢â¬â 8 cenas + CTA ARCKHê AI, ~2:00, PT-BR
- **NarraêÂ§êÂ£o** gerada (voz pf_dora, 9 WAVs em assets/audio/)
- **ComposiêÂ§êÂ£o** `build-index.mjs` adaptada (3 cêÂ©rebros), lint **0 erros**
- **Render:** ê¢Åâ¦ os dois formatos em alta, conferidos por frame:
  - `renders/cerebro-ARCKHê-16x9.mp4` ê¢â¬â 1920êâ1080 êÂ· **120,6s** êÂ· 87 MB
  - `renders/cerebro-ARCKHê-9x16.mp4` ê¢â¬â 1080êâ1920 êÂ· **120,6s** êÂ· 83 MB
- **Bug corrigido no caminho:** o template tinha overshoot de ~43s (cauda vazia,
  vêÂ­deo ia a 162s). Limitei os tweens de ambiente ao TOTAL ê¢â â fecha em ~120s na CTA.
- **ConteêÂºdo conferido** (frames): cena 1 (hero "Seu cêÂ©rebro em trêÂªs"), cena 4
  (os 3 cêÂ©rebros), CTA ARCKHê AI, e frame vertical (safe zones ok).

**As 8 cenas:** 1) hook amnêÂ©sia êÂ· 2) o cêÂ©rebro sêÂ£o os arquivos / CLAUDE.md êÂ© o mapa êÂ·
3) 3 memêÂ³rias, nêÂ£o misturar êÂ· 4) os 3 cêÂ©rebros (Projeto/Self/Conhecimento) êÂ·
5) instalar êÂ© um comando êÂ· 6) as skills êÂ· 7) êÂ© real, PT-BR/Linux êÂ· 8) sobrevive ao
hype êÂ· + CTA ARCKHê AI.

---

## PrêÂ³ximos passos sugeridos (pra quando acordar)
- Revisar a landing e o vêÂ­deo draft; ajustar texto/voz se quiser.
- Rodar o `cerebro-ARCKHê` de verdade: `git clone ê¢â¬Â¦ && ./setup.sh && /setup-cerebro` (dogfooding).
- Decidir se publica o vêÂ­deo (YouTube/Shorts) e se a landing entra no portal ARCKHê AI.
- Rever os 6 defaults da arquitetura (acima) e mudar o que nêÂ£o fizer sentido.
