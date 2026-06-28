# second-brain-kit Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Construir um repositório público `second-brain-kit` com uma skill `/brain-setup` que configura um "segundo cérebro" (vault Obsidian + memória + skills + CLAUDE.md) para qualquer pessoa, em diálogo, sem nenhum dado pessoal do autor.

**Architecture:** Repositório de conteúdo (markdown), não código executável. A inteligência mora dentro do `SKILL.md` do `/brain-setup` — instruções que o Claude Code segue em runtime. Templates e docs são arquivos estáticos. Não há build nem testes unitários; cada tarefa é verificada por inspeção do conteúdo + grep de privacidade.

**Tech Stack:** Markdown, skills do Claude Code (formato `.claude/skills/<nome>/SKILL.md` com frontmatter `name`/`description`), git.

## Global Constraints

- **Zero dados pessoais do autor.** Proibido em qualquer arquivo do kit: `Thiago`, `Garbo`, `midasgrowth`, `C:\Users\Thiago`, nomes de negócios/clientes (Flowly, Semente Energética, Valeria, Tu Guía Cósmica, Remédios Ancestrais, Midas, AIO, Legging), IDs/tokens/pixel/planilhas/contas Meta. (Spec §7)
- **Cross-plataforma:** instruções funcionam em Windows e Mac. Quando houver comando de SO, mostrar as duas variantes (`start` vs `open`; paths `~` vs `%USERPROFILE%`). (Spec §9.5)
- **Nada é construído sem confirmação explícita; nada é sobrescrito sem backup `.bak`.** (Spec §3, §9.3)
- **Idioma do conteúdo:** português (pt-BR), neutro e genérico. Sem referência ao autor.
- **Skills usam frontmatter** `---\nname: <kebab>\ndescription: <uma linha>\n---` no topo do `SKILL.md`.
- Cada commit não assina GPG (`-c commit.gpgsign=false` já é o default do repo) e termina com a linha `Co-Authored-By: Claude Opus 4.8 (1M context) <noreply@anthropic.com>`.

---

### Task 1: Scaffolding do repositório (estrutura + arquivos de borda)

**Files:**
- Create: `.gitignore`
- Create: `LICENSE`
- Create: `add-ons/.gitkeep`, `templates/.gitkeep`, `docs/.gitkeep` (placeholders de pasta)

**Interfaces:**
- Produces: a árvore de pastas da Spec §2 existindo no disco. Tarefas seguintes assumem que `templates/`, `docs/`, `add-ons/`, `.claude/skills/` existem.

- [ ] **Step 1: Criar as pastas**

```bash
cd "C:/Users/Thiago/Desktop/second-brain-kit"
mkdir -p .claude/skills/brain-setup .claude/skills/diario .claude/skills/tldr \
  templates docs add-ons/marketing add-ons/conteudo add-ons/deals
```

- [ ] **Step 2: Escrever `.gitignore`**

```
# Sistema
.DS_Store
Thumbs.db
desktop.ini

# Editor
.obsidian/
*.bak
*.tmp
*~
```

- [ ] **Step 3: Escrever `LICENSE` (MIT, sem nome do autor)**

Usar o texto MIT padrão com `Copyright (c) 2026 second-brain-kit contributors` no lugar do nome — para não vazar identidade.

- [ ] **Step 4: Verificar estrutura e privacidade**

Run: `find "C:/Users/Thiago/Desktop/second-brain-kit" -type d | sort` — esperado: as pastas da §2 presentes.
Run grep de privacidade (deve retornar VAZIO):
`grep -rniE "thiago|garbo|midasgrowth|flowly|valeria|semente energ" "C:/Users/Thiago/Desktop/second-brain-kit" --include="*.md" --include="LICENSE" --include=".gitignore" | grep -v "docs/superpowers/specs"`
Esperado: nenhuma linha (o spec em specs/ é o único que cita o autor e fica fora do grep; será tratado na Task 9).

- [ ] **Step 5: Commit**

```bash
git add -A && git commit -m "$(printf 'chore: scaffold repo structure\n\nCo-Authored-By: Claude Opus 4.8 (1M context) <noreply@anthropic.com>')"
```

---

### Task 2: Skill-núcleo `/diario`

**Files:**
- Create: `.claude/skills/diario/SKILL.md`

**Interfaces:**
- Produces: comando `/diario` genérico. O `/brain-setup` (Task 7) referencia esta skill como uma das duas skills-núcleo instaladas.

- [ ] **Step 1: Escrever o SKILL.md**

```markdown
---
name: diario
description: Começa o dia com contexto do vault — abre/cria a nota de hoje, lista pendências e prioridades.
---

# Skill: /diario

Abrir ou criar a nota diária de hoje em `diario/YYYY-MM-DD.md`.

1. Verificar se existe nota para hoje — se não, criar com cabeçalho de data.
2. Verificar `inbox/` para arquivos não processados — listar o que tem lá e perguntar se quer organizar agora.
3. Varrer as pastas de trabalho (projetos/, e quaisquer pastas de área do vault) por itens com algo pendente, vencido ou urgente.
4. Destacar as 3 principais prioridades do dia com base no contexto encontrado.
5. Perguntar: "No que vamos trabalhar hoje?"
```

- [ ] **Step 2: Verificar conteúdo e privacidade**

Run: `grep -niE "ofertas|campanhas|thiago|garbo" ".claude/skills/diario/SKILL.md"`
Esperado: VAZIO (a versão genérica não cita pastas específicas de marketing nem o autor).

- [ ] **Step 3: Commit**

```bash
git add .claude/skills/diario/SKILL.md && git commit -m "$(printf 'feat: add generic /diario skill\n\nCo-Authored-By: Claude Opus 4.8 (1M context) <noreply@anthropic.com>')"
```

---

### Task 3: Skill-núcleo `/tldr`

**Files:**
- Create: `.claude/skills/tldr/SKILL.md`

**Interfaces:**
- Consumes: o formato de memória definido na Task 5 (frontmatter `name`/`description`/`metadata.type`; índice `MEMORY.md`).
- Produces: comando `/tldr` que fecha a sessão, salva resumo na pasta certa e alimenta `MEMORY.md`. Referenciado pelo `/brain-setup` (Task 7) como o ritual que faz a memória crescer.

- [ ] **Step 1: Escrever o SKILL.md**

```markdown
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
   - Criar/atualizar um arquivo de memória (frontmatter `name`/`description`/`metadata.type`) na pasta de memória.
   - Adicionar uma linha-índice em `MEMORY.md`.
   - Convenções de quando salvar memória estão em `docs/conceitos.md`.

4. Perguntar: "Tem mais alguma coisa para registrar antes de fechar?"
```

- [ ] **Step 2: Verificar conteúdo e privacidade**

Run: `grep -niE "ofertas|campanhas|persona|thiago|garbo" ".claude/skills/tldr/SKILL.md"`
Esperado: VAZIO.

- [ ] **Step 3: Commit**

```bash
git add .claude/skills/tldr/SKILL.md && git commit -m "$(printf 'feat: add generic /tldr skill\n\nCo-Authored-By: Claude Opus 4.8 (1M context) <noreply@anthropic.com>')"
```

---

### Task 4: Templates de CLAUDE.md (global + vault)

**Files:**
- Create: `templates/CLAUDE.global.md`
- Create: `templates/CLAUDE.vault.md`

**Interfaces:**
- Produces: dois templates com placeholders `[ENTRE_COLCHETES]`. O `/brain-setup` (Task 7) lê estes templates, preenche os placeholders com o que inferiu da pessoa, e grava nos lugares certos.

- [ ] **Step 1: Escrever `templates/CLAUDE.global.md`**

```markdown
## Meu Contexto Pessoal
No início de cada sessão, leia [CAMINHO_ABSOLUTO_DO_VAULT]/CLAUDE.md para contexto sobre quem sou, meu trabalho e minhas convenções.

## Como Quero Que Você Trabalhe — Sempre
Regra permanente de comportamento, válida para todas as sessões e projetos.

**1. Precisão e honestidade (sem alucinar)**
- Quando não tiver certeza, diga "não sei" ou "não tenho como confirmar" — nunca invente.
- Marque o que é fato verificado, o que é inferência e o que é opinião/suposição.
- Ao analisar meus arquivos, baseie-se no conteúdo real deles (cite a origem), não na memória geral.
- Em decisões importantes, mostre o raciocínio passo a passo.

**2. Diretivo — aponte erros**
- Discorde quando for o caso; não concorde só para agradar.
- Aponte erros nas minhas ideias, premissas ou pedidos quando existirem.
- Se um caminho que propus tem um problema, diga claramente — antes de executar.
```

- [ ] **Step 2: Escrever `templates/CLAUDE.vault.md`**

```markdown
# CLAUDE.md — Segundo Cérebro de [PAPEL]

## Quem Sou
[2-3 FRASES SOBRE A PESSOA — em primeira pessoa, escrito como o Claude descrevendo seu dono. Inferido da conversa.]

## Estrutura do Meu Vault
```
inbox/        Tudo novo entra aqui primeiro, antes de ser classificado
diario/       Capturas rápidas do dia, contexto de sessão
[PASTA_AREA]/ [PROPÓSITO BASEADO NO PAPEL]
[PASTA_AREA]/ [PROPÓSITO BASEADO NO PAPEL]
projetos/     Trabalho ativo com status e próximas ações
arquivo/      Tudo concluído — nunca deletado, só movido
```

## Como Trabalho
- [3-4 PONTOS INFERIDOS: estilo de captura, principal ponto de dor, escopo, o que quero da IA]

## Ancorar Detalhes em Arquivos (não só na conversa)
Em trabalho longo, o que fica só na conversa pode ser comprimido e perder detalhe; o que está num arquivo do vault nunca se perde.
- Decisões, números, configs e nomes exatos → registrar numa nota do vault na hora.
- Em tarefas longas, manter uma nota de status atualizada.

## Regras de Contexto
Quando mencionar um [PROJETO/ÁREA] → procurar na pasta correspondente para carregar histórico e decisões.
Quando algo cair no inbox/ → perguntar se quero organizar agora.
Quando pedir para escrever → ler notas recentes de diario/ para combinar com o ângulo já definido.
```

- [ ] **Step 3: Verificar privacidade**

Run: `grep -rniE "thiago|garbo|marketing de performance|tráfego pago|ofertas/" templates/`
Esperado: VAZIO (templates são genéricos; "marketing" só aparece como exemplo de placeholder, não como conteúdo fixo).

- [ ] **Step 4: Commit**

```bash
git add templates/CLAUDE.global.md templates/CLAUDE.vault.md && git commit -m "$(printf 'feat: add CLAUDE.md templates (global + vault)\n\nCo-Authored-By: Claude Opus 4.8 (1M context) <noreply@anthropic.com>')"
```

---

### Task 5: Seed do sistema de memória (`templates/MEMORY.md`)

**Files:**
- Create: `templates/MEMORY.md`

**Interfaces:**
- Produces: o índice-semente da memória, com 3 exemplos INVENTADOS genéricos. O `/tldr` (Task 3) e o `/brain-setup` (Task 7) referenciam este formato.

- [ ] **Step 1: Escrever `templates/MEMORY.md`**

```markdown
# Memória — Índice

Uma linha por memória. Cada memória é um arquivo separado nesta pasta, com frontmatter
(`name`, `description`, `metadata.type: user | feedback | project | reference`) e um corpo curto.
Link entre memórias com [[nome-do-arquivo]].

- [Perfil](perfil.md) — quem sou, o que faço, minhas preferências de trabalho
- [Como gosto de trabalhar](como-trabalhar.md) — regras de comportamento que dei ao Claude
- [Projeto exemplo](projeto-exemplo.md) — contexto de um projeto ativo que não está no código

> Estes três são exemplos. Apague-os e crie os seus conforme o /tldr for rodando.
```

- [ ] **Step 2: Verificar privacidade**

Run: `grep -niE "thiago|garbo|flowly|valeria|semente" templates/MEMORY.md`
Esperado: VAZIO.

- [ ] **Step 3: Commit**

```bash
git add templates/MEMORY.md && git commit -m "$(printf 'feat: add memory index seed with generic examples\n\nCo-Authored-By: Claude Opus 4.8 (1M context) <noreply@anthropic.com>')"
```

---

### Task 6: Documentação didática (`conceitos.md`, `mcp.md`, `add-ons.md`)

**Files:**
- Create: `docs/conceitos.md`
- Create: `docs/mcp.md`
- Create: `docs/add-ons.md`

**Interfaces:**
- Produces: conteúdo de leitura humana. O `/brain-setup` (Task 7) aponta a pessoa para `docs/conceitos.md` ao explicar e o `/tldr` referencia as convenções de memória que vivem aqui.

- [ ] **Step 1: Escrever `docs/conceitos.md`**

Conteúdo cobrindo (texto corrido didático, sem citar o autor):
- **O que é o segundo cérebro** — vault Obsidian + memória + skills + CLAUDE.md.
- **O problema** — Claude Code começa cada sessão do zero.
- **Por que é melhor que Claude puro** — contexto persistente; o arquivo nunca comprime, a conversa sim.
- **Como funciona a memória** — formato de arquivo + frontmatter + MEMORY.md como índice; quando salvar (fato não-óbvio, preferência, contexto de projeto); quando NÃO salvar (o que o código já registra).
- **O ritual** — /diario de manhã, /tldr no fim; por que o /tldr é o que faz a memória crescer.
- **Princípios modeláveis** — separar por projeto; memórias transversais referenciadas no CLAUDE.md geral; é um esqueleto, molde do seu jeito.

- [ ] **Step 2: Escrever `docs/mcp.md`**

Conteúdo: o que são MCPs (conectores), exemplos úteis e neutros (Google Sheets, Notion, Drive) e o comando genérico de conexão. **Regra explícita no doc:** "use SUAS credenciais; nunca compartilhe tokens." Sem IDs/tokens reais.

- [ ] **Step 3: Escrever `docs/add-ons.md`**

Conteúdo: o que são add-ons (skills de nicho opcionais), como ativá-los (mover de `add-ons/<nome>/` para `.claude/skills/`), e a lista dos disponíveis (marketing, conteúdo, deals) com uma linha cada.

- [ ] **Step 4: Verificar privacidade**

Run: `grep -rniE "thiago|garbo|midasgrowth|flowly|valeria|semente|19c1cJ|912786|dsuqbhaq" docs/`
Esperado: VAZIO.

- [ ] **Step 5: Commit**

```bash
git add docs/conceitos.md docs/mcp.md docs/add-ons.md && git commit -m "$(printf 'docs: add conceitos, mcp e add-ons guides\n\nCo-Authored-By: Claude Opus 4.8 (1M context) <noreply@anthropic.com>')"
```

---

### Task 7: A skill mentor `/brain-setup` (o motor)

**Files:**
- Create: `.claude/skills/brain-setup/SKILL.md`

**Interfaces:**
- Consumes: `templates/CLAUDE.global.md`, `templates/CLAUDE.vault.md`, `templates/MEMORY.md` (Tasks 4-5); skills `/diario`, `/tldr` (Tasks 2-3); `docs/conceitos.md` (Task 6).
- Produces: o comando `/brain-setup` completo. É o entregável central.

- [ ] **Step 1: Escrever o frontmatter + Abertura (entender a pessoa primeiro)**

O SKILL.md começa com:
```markdown
---
name: brain-setup
description: Configura um segundo cérebro (vault Obsidian + memória + skills + CLAUDE.md) para você, em diálogo. Entende quem você é, detecta o que já tem, explica os conceitos e constrói só após sua confirmação.
---
```
Seção "PASSO 0 — Entender a pessoa": apresentar-se em 2 linhas; fazer a pergunta-mãe (o que faz / o que escapa / trabalho ou vida / arquivos existentes); deixar visível a porta "quer que eu explique antes de montar?". Inferir papel/dor/escopo e devolver pra confirmar. (Spec §3.1)

- [ ] **Step 2: Escrever a seção "FAQ ao vivo"**

Bloco com as 4 respostas prontas (o que você faz / como funciona a memória / por que melhor que Claude puro / explica cada skill), instruindo a skill a responder na hora e voltar ao fluxo. (Spec §3.2)

- [ ] **Step 3: Escrever "PASSO 1 — Detecção técnica (read-only)"**

Instruções para: checar `~/.claude/CLAUDE.md` (Mac) / `%USERPROFILE%\.claude\CLAUDE.md` (Win) e propor preservar+somar ou começar limpo (sempre backup `.bak`); procurar pasta de notas/vault existente; detectar SO e ajustar comandos. (Spec §3.3)

- [ ] **Step 4: Escrever "PASSO 2 — Proposta em diálogo"**

Mostrar estrutura proposta + as 3 melhorias modeláveis (separar por projeto / memória referenciada no CLAUDE.md geral / /tldr religioso); frase "é um esqueleto, molda do teu jeito"; aceitar remodelagem. **Aguardar confirmação explícita.** (Spec §3.4)

- [ ] **Step 5: Escrever "PASSO 3 — Construção (só após sim)"**

Instruções cross-plataforma para: criar pastas; gravar CLAUDE.md do vault a partir de `templates/CLAUDE.vault.md` preenchido; instalar skills `/diario` e `/tldr`; criar pasta de memória com `templates/MEMORY.md`; migrar (sem sobrescrever) o CLAUDE.md que já existia. Explicar cada peça enquanto cria. (Spec §3.5)

- [ ] **Step 6: Escrever "PASSO 4 — Fechamento"**

Injetar contexto no CLAUDE.md global (opcional, com consentimento, usando `templates/CLAUDE.global.md`); ensinar o ritual /diario+/tldr; explicar por que o /tldr faz a memória crescer; avisar do passo manual do Obsidian (habilitar CLI); sugerir add-ons conforme o perfil inferido. (Spec §3.6, §5)

- [ ] **Step 7: Verificar fluxo completo e privacidade**

Ler o SKILL.md inteiro e conferir contra Spec §3 (todos os passos presentes, ordem correta, "nada sem confirmação", "nada sobrescrito sem backup").
Run: `grep -niE "thiago|garbo|midasgrowth|flowly|valeria|semente|C:\\\\Users\\\\Thiago" ".claude/skills/brain-setup/SKILL.md"`
Esperado: VAZIO.

- [ ] **Step 8: Commit**

```bash
git add .claude/skills/brain-setup/SKILL.md && git commit -m "$(printf 'feat: add /brain-setup mentor skill (the engine)\n\nCo-Authored-By: Claude Opus 4.8 (1M context) <noreply@anthropic.com>')"
```

---

### Task 8: Add-ons esqueleto (marketing / conteúdo / deals)

**Files:**
- Create: `add-ons/marketing/SKILL.md` (skill `/oferta` esqueleto)
- Create: `add-ons/conteudo/SKILL.md` (skill `/conteudo` esqueleto)
- Create: `add-ons/deals/SKILL.md` (skill `/deal` esqueleto)

**Interfaces:**
- Consumes: nada (são moldes independentes).
- Produces: três skills-molde com placeholders. O `docs/add-ons.md` (Task 6) e o `/brain-setup` (Task 7) as referenciam.

> **NOTA DE EXECUÇÃO:** o autor pediu para decidir cada add-on UMA POR UMA. Antes de criar cada arquivo, mostrar o conteúdo proposto ao autor e pedir OK (incluir / esvaziar mais / deixar de fora). Não criar os três de enfiada sem checkpoint.

- [ ] **Step 1: Propor e (após OK) escrever `add-ons/marketing/SKILL.md`**

Molde `/oferta`: carregar contexto de uma oferta (copy, funil, status, histórico de testes). Corpo com placeholders `[descreva como você estrutura suas ofertas]`. Sem nomes de ofertas reais.

- [ ] **Step 2: Propor e (após OK) escrever `add-ons/conteudo/SKILL.md`**

Molde `/conteudo`: desenvolver ideia de conteúdo calibrando com a voz do criador. Placeholders `[onde mora seu perfil de voz]`.

- [ ] **Step 3: Propor e (após OK) escrever `add-ons/deals/SKILL.md`**

Molde `/deal`: carregar contexto de uma oportunidade/parceria (histórico, pessoas, próximos passos). Placeholders.

- [ ] **Step 4: Verificar privacidade dos add-ons**

Run: `grep -rniE "thiago|garbo|flowly|valeria|semente|remédios|tu guía|legging|midas" add-ons/`
Esperado: VAZIO.

- [ ] **Step 5: Commit**

```bash
git add add-ons/ && git commit -m "$(printf 'feat: add skeleton add-on skills (marketing, conteudo, deals)\n\nCo-Authored-By: Claude Opus 4.8 (1M context) <noreply@anthropic.com>')"
```

---

### Task 9: README + varredura final de privacidade + tratar o spec

**Files:**
- Create: `README.md`
- Modify: `docs/superpowers/specs/2026-06-28-second-brain-kit-design.md` (remover path pessoal da linha de "Local de construção")

**Interfaces:**
- Consumes: todo o repositório (descreve cada peça).
- Produces: a porta de entrada pública + garantia de que o grep de privacidade passa limpo no repo inteiro.

- [ ] **Step 1: Escrever `README.md`**

Seções (Spec §6): (1) o que é; (2) por que — problema (Claude esquece a cada sessão) + solução; (3) pré-requisitos (Claude Code; Obsidian opcional mas recomendado); (4) instalação (clonar → abrir Claude Code na pasta → rodar `/brain-setup`); (5) tabela do que cada peça faz (vault, memória, /diario, /tldr, add-ons, docs); (6) "como modelar do seu jeito". Sem nome do autor.

- [ ] **Step 2: Neutralizar o path pessoal no spec**

No spec, trocar `C:\Users\Thiago\Desktop\second-brain-kit` por `<pasta-do-projeto>` na tabela de decisões. (O spec fica no repo como documentação de design, então não pode conter o path pessoal.)

- [ ] **Step 3: VARREDURA FINAL de privacidade — repo inteiro, sem exceção**

Run:
```bash
grep -rniE "thiago|garbo|midasgrowth|flowly|valeria|semente energ|tu guía|tuguia|remédios ancestrais|legging|midas|C:\\\\Users\\\\Thiago|912786|dsuqbhaq|19c1cJ|kirvano|elena" "C:/Users/Thiago/Desktop/second-brain-kit" --include="*.md" --include="*.json" --include="LICENSE"
```
Esperado: **ZERO linhas.** Se aparecer qualquer coisa, corrigir o arquivo apontado antes de prosseguir.

- [ ] **Step 4: Commit**

```bash
git add -A && git commit -m "$(printf 'docs: add README and pass final privacy sweep\n\nCo-Authored-By: Claude Opus 4.8 (1M context) <noreply@anthropic.com>')"
```

- [ ] **Step 5: Relatório final ao autor**

Listar: estrutura criada, o que cada skill faz, resultado do grep (deve ser limpo), e os próximos passos manuais que SÓ o autor pode fazer (criar repo no GitHub, `git remote add`, `git push`, tornar público). Não publicar automaticamente (Spec §8).

---

## Notas de verificação (substituem o ciclo de testes unitários)

Como o kit é conteúdo, não código, a verificação de cada tarefa é:
1. O arquivo existe e tem o conteúdo descrito (inspeção).
2. O grep de privacidade da tarefa retorna vazio.
3. O fluxo da skill (Task 7) bate seção-a-seção com a Spec §3.
A varredura final (Task 9, Step 3) é o portão que garante o critério de sucesso §9.4 antes de qualquer publicação.
