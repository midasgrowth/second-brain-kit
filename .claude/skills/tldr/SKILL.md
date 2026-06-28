---
name: tldr
description: Fecha a sessão — resume decisões e próximas ações, salva na pasta certa e atualiza a memória.
---

# Skill: /tldr

Resumir esta sessão e salvar nas pastas certas.

1. Extrair da conversa:
   - Decisões tomadas
   - O que foi feito, testado ou alterado
   - Próximas ações (com responsável e prazo, se houver)
   - Informações novas que valem ser lembradas em sessões futuras

2. Salvar o resumo na pasta mais relevante do vault:
   - Se foi sobre um projeto específico → `projetos/<nome>/YYYY-MM-DD-sessao.md` (ou a pasta de área correspondente)
   - Se foi geral → `diario/YYYY-MM-DD.md`

3. Atualizar a memória se algo aprendido nesta sessão valer persistir:
   - Criar/atualizar um arquivo de memória (frontmatter `name` / `description` / `metadata.type`) na pasta de memória.
   - Adicionar uma linha-índice em `MEMORY.md`.
   - As convenções de quando salvar (e quando não salvar) memória estão em `docs/conceitos.md`.

4. Perguntar: "Tem mais alguma coisa para registrar antes de fechar?"
