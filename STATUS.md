# STATUS â€” manhÃ£ de 2026-06-20

Resumo do que rodou autÃ´nomo enquanto vocÃª dormia. Os **3 pontos** (build â†’ teach â†’ publish), na ordem que combinamos.

---

## VisÃ£o rÃ¡pida

| Ponto | O quÃª | Status | Onde |
|---|---|---|---|
| 0 | Repo histÃ³rico atualizado | âœ… | github.com/linnoff88-dev/2cerebrox |
| 1 | **BUILD** â€” produto cerebro-ARCKHÉ | âœ… | github.com/linnoff88-dev/cerebro-ARCKHÉ |
| 2 | **TEACH** â€” landing+guia (GitHub Pages) | âœ… no ar | linnoff88-dev.github.io/ |
| 3 | **PUBLISH** â€” vÃ­deo explicativo | â³ ver Â§3 | ~/projetos/output/cerebro-ARCKHÉ-video |

---

## 0 Â· Repo histÃ³rico (2cerebrox)
A pesquisa toda ficou aqui como histÃ³rico. Adicionei a Â§7 (camada de VisualizaÃ§Ã£o / brain-map) e este STATUS.md. O doc principal `docs/comparacao-segundo-cerebros.md` estÃ¡ com 9 seÃ§Ãµes; `docs/estrutura-3-cerebros.md` tem a proposta de arquitetura.

## 1 Â· BUILD â€” cerebro-ARCKHÉ (âœ…)
Produto novo, **pÃºblico**: https://github.com/linnoff88-dev/cerebro-ARCKHÉ

Implementa a arquitetura dos 3 cÃ©rebros (decisÃµes em aberto fechadas com defaults â€” ver lista abaixo):
- **Estrutura:** `0-inbox/ 1-projeto/ 2-self/ 3-conhecimento/` (vault-template)
- **CLAUDE.md roteador** â€” mapa dos 3 cÃ©rebros + regras + roteamento
- **Skills:** `triagem`, `socratica`, `nota` (novas) + `daily`, `tldr`, `setup-cerebro` (adaptadas) â€” todas em texto livre, sem AskUserQuestion (sua regra global)
- **setup.sh** PT-BR/Linux+mac â€” **testado** (smoke test passou, gera o vault completo)
- **docs/** arquitetura (3 cÃ©rebros Ã— 4 camadas) + integraÃ§Ã£o kepano
- Scripts Gemini (ingestÃ£o rawâ†’wiki) reaproveitados do fork

**Defaults que bati (reverta se quiser):**
1. Vault Ãºnico (nÃ£o trÃªs separados)
2. Prefixo numÃ©rico nas pastas (`1-projeto/`â€¦)
3. Journal (self) separado do daily (projeto)
4. Pergunta resolvida migra de `2-self/questions` â†’ `3-conhecimento/notes`
5. graphify fora do core (documentado como opcional)
6. Nomes de pasta + conteÃºdo em PT-BR

## 2 Â· TEACH â€” landing+guia (âœ… no ar)
PÃ¡gina Ãºnica ARCKHÉ dark Ã¢mbar, dark/light toggle, ARCKHÉ AI no nav e footer.
- **No ar:** https://linnoff88-dev.github.io/ (HTTP 200 confirmado)
- Fonte: `index.html` na raiz do cerebro-ARCKHÉ (self-contained)
- SeÃ§Ãµes: hero (diagrama dos 3 cÃ©rebros) Â· o que Ã© Â· como funciona Â· prÃ©-requisitos Â· guia passo a passo (comandos reais) Â· exemplos Â· as 4 camadas Â· footer

## 3 Â· PUBLISH â€” vÃ­deo explicativo (â³)
Projeto HyperFrames em `~/projetos/output/cerebro-ARCKHÉ-video/`.
- **Roteiro** `SCRIPT.md` â€” 8 cenas + CTA ARCKHÉ AI, ~2:00, PT-BR
- **NarraÃ§Ã£o** gerada (voz pf_dora, 9 WAVs em assets/audio/)
- **ComposiÃ§Ã£o** `build-index.mjs` adaptada (3 cÃ©rebros), lint **0 erros**
- **Render:** âœ… os dois formatos em alta, conferidos por frame:
  - `renders/cerebro-ARCKHÉ-16x9.mp4` â€” 1920Ã—1080 Â· **120,6s** Â· 87 MB
  - `renders/cerebro-ARCKHÉ-9x16.mp4` â€” 1080Ã—1920 Â· **120,6s** Â· 83 MB
- **Bug corrigido no caminho:** o template tinha overshoot de ~43s (cauda vazia,
  vÃ­deo ia a 162s). Limitei os tweens de ambiente ao TOTAL â†’ fecha em ~120s na CTA.
- **ConteÃºdo conferido** (frames): cena 1 (hero "Seu cÃ©rebro em trÃªs"), cena 4
  (os 3 cÃ©rebros), CTA ARCKHÉ AI, e frame vertical (safe zones ok).

**As 8 cenas:** 1) hook amnÃ©sia Â· 2) o cÃ©rebro sÃ£o os arquivos / CLAUDE.md Ã© o mapa Â·
3) 3 memÃ³rias, nÃ£o misturar Â· 4) os 3 cÃ©rebros (Projeto/Self/Conhecimento) Â·
5) instalar Ã© um comando Â· 6) as skills Â· 7) Ã© real, PT-BR/Linux Â· 8) sobrevive ao
hype Â· + CTA ARCKHÉ AI.

---

## PrÃ³ximos passos sugeridos (pra quando acordar)
- Revisar a landing e o vÃ­deo draft; ajustar texto/voz se quiser.
- Rodar o `cerebro-ARCKHÉ` de verdade: `git clone â€¦ && ./setup.sh && /setup-cerebro` (dogfooding).
- Decidir se publica o vÃ­deo (YouTube/Shorts) e se a landing entra no portal ARCKHÉ AI.
- Rever os 6 defaults da arquitetura (acima) e mudar o que nÃ£o fizer sentido.
