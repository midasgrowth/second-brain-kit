---
name: diario
description: Começa o dia com contexto do vault — abre/cria a nota de hoje, lista pendências e prioridades.
---

# Skill: /diario

Abrir ou criar a nota diária de hoje em `diario/YYYY-MM-DD.md`.

1. Verificar se existe nota para hoje — se não, criar com cabeçalho de data.
2. Verificar `inbox/` para arquivos não processados — listar o que tem lá e perguntar se quer organizar agora.
3. Varrer as pastas de trabalho (`projetos/` e quaisquer pastas de área do vault) por itens com algo pendente, vencido ou urgente.
4. Destacar as 3 principais prioridades do dia com base no contexto encontrado.
5. Perguntar: "No que vamos trabalhar hoje?"
