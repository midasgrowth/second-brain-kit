# second-brain-kit — Design / Spec

**Data:** 2026-06-28
**Status:** Aprovado (brainstorming concluído)

## 1. Visão

Um repositório GitHub público que qualquer pessoa clona e, dentro do Claude Code, roda `/brain-setup`. A skill é um **mentor de configuração**: entende a pessoa, detecta o que ela já tem, explica os conceitos quando perguntada, propõe uma estrutura de "segundo cérebro" com melhorias, confirma em diálogo e então constrói (vault Obsidian + sistema de memória + skills + CLAUDE.md inteligente) adaptado a ela.

O kit entrega o **motor** (sistema genérico). O conteúdo da pessoa é pintado por cima pelo `/brain-setup`. **Nenhum dado pessoal do autor entra no repositório.**

### Problema que resolve
Claude Code começa cada sessão do zero. Quem trabalha em projetos longos perde o fio entre sessões: o que foi decidido, testado, está pendente. Um vault Obsidian + sistema de memória ancorado em arquivos resolve isso — contexto que persiste e nunca comprime, ao contrário da conversa.

### Decisões travadas
| Tema | Decisão |
|---|---|
| Nome do repo / skill | `second-brain-kit` / `/brain-setup` |
| Núcleo | Motor genérico (vault + memória + /diario + /tldr + CLAUDE.md). Skills de nicho = add-ons opcionais |
| Migração do que a pessoa já tem | Detectar + adaptar **em diálogo**: mostrar o que entendeu, propor melhorias, confirmar antes de seguir |
| Trigger | Skill `/brain-setup` rodada dentro do Claude Code |
| MCP | Só documentar + sugerir (a pessoa põe as credenciais dela) |
| Abertura | Vai direto pro diálogo, mas entende a PESSOA primeiro; ensina conceitos se/quando perguntarem |
| Local de construção | `C:\Users\Thiago\Desktop\second-brain-kit` |
| Skills de nicho (add-ons) | Decidir uma por uma na implementação; versões esqueleto, sem conhecimento do autor |

## 2. Arquitetura do repositório

```
second-brain-kit/
├── README.md                    # porta de entrada: o que é, como rodar, o que cada peça faz
├── LICENSE                      # MIT
├── .gitignore
├── .claude/
│   └── skills/
│       ├── brain-setup/SKILL.md # O MOTOR — skill mentor que monta tudo
│       ├── diario/SKILL.md      # skill-núcleo genérica (começar o dia com contexto)
│       └── tldr/SKILL.md        # skill-núcleo genérica (fechar sessão + alimentar memória)
├── templates/
│   ├── CLAUDE.global.md         # regras de comportamento (preciso + diretivo) com placeholders
│   ├── CLAUDE.vault.md          # template do CLAUDE.md do vault, com placeholders por perfil
│   └── MEMORY.md                # seed do índice de memória + 3 exemplos genéricos
├── docs/
│   ├── conceitos.md             # didático: o que é vault, memória, skills, MCP, por que é melhor
│   ├── mcp.md                   # MCPs úteis e como conectar (credenciais da própria pessoa)
│   └── add-ons.md               # skills opcionais de nicho e como ativar
└── add-ons/                     # skills de nicho ESQUELETO (marketing, conteúdo, deals)
    ├── marketing/
    ├── conteudo/
    └── deals/
```

**Princípio de isolamento:** cada peça tem um propósito único. `brain-setup` orquestra; `diario`/`tldr` são independentes; `templates/` são dados; `docs/` é leitura humana; `add-ons/` são moldes opcionais.

## 3. Fluxo do `/brain-setup`

A skill tem dois modos que coexistem; a pessoa transita livremente e **nada é construído sem confirmação explícita**.

### 3.1 Abertura — entender a PESSOA primeiro
Apresenta-se em 2 linhas e faz a pergunta-mãe (texto livre):
- O que você faz no trabalho/vida?
- O que mais escapa — o que queria acompanhar melhor e hoje perde o fio?
- Só trabalho, ou vida pessoal também?
- Tem arquivos/notas que já usa hoje?

Deixa visível a porta: *"quer que eu explique como isso funciona antes de montar? é só dizer."*

A partir da resposta, infere **papel** (marketeiro/criador/consultor/dev/estudante/dono de negócio), **dor principal**, **escopo**, **material existente**. Devolve pra confirmar: *"Te entendi assim: [...]. É isso?"*

### 3.2 FAQ ao vivo (conteúdo fixo dentro da skill)
A skill responde na hora, a qualquer momento:
- **O que você consegue fazer?** → carregar contexto entre sessões, nunca começar do zero, manter o fio de múltiplos projetos.
- **Como funciona a memória?** → arquivos com frontmatter (`name`, `description`, `type`) + `MEMORY.md` como índice + `/tldr` alimentando.
- **Por que é melhor que Claude puro sem vault?** → contexto persistente vs. cada sessão do zero; o arquivo nunca comprime, a conversa sim.
- **Me explica cada skill** → `/diario`, `/tldr`, e os add-ons.

### 3.3 Detecção técnica (enriquece, não substitui)
Read-only, depois de entender a pessoa:
- `~/.claude/CLAUDE.md` existe? → propõe **preservar e somar** ou começar limpo (faz backup `.bak`, nunca sobrescreve).
- Já existe pasta de notas / vault Obsidian? → pergunta se o cérebro nasce ali ou em outro lugar.
- SO (Win/Mac) → ajusta comandos e paths (`start` vs `open`).

### 3.4 Proposta em diálogo
Mostra a estrutura proposta + sugere melhorias modeláveis:
- **Separar por projeto** — cada projeto/oferta é uma pasta; todo o conhecimento dele mora ali.
- **Memórias referenciadas no CLAUDE.md geral** — conhecimento transversal (ex: "como faço criativos") vira memória linkada, não presa a um projeto.
- **/tldr religioso** — é o /tldr no fim da sessão que faz a memória crescer.

Deixa explícito: *"isso é um esqueleto, molda do teu jeito"*. Aceita remodelagem antes de construir.

### 3.5 Construção (só após "sim")
Migra o contexto existente (sem sobrescrever; backup). Cria pastas + CLAUDE.md (mesclado) + skills + MEMORY.md. Explica cada peça enquanto cria. Cross-plataforma.

### 3.6 Fechamento
Injeta contexto no `~/.claude/CLAUDE.md` global (opcional, com consentimento). Ensina o ritual: `/diario` de manhã, `/tldr` no fim. Explica por que o `/tldr` é o que faz a memória crescer. Avisa do passo manual do Obsidian (habilitar CLI).

## 4. Sistema de memória (entregue vazio)

- `MEMORY.md` — índice, uma linha por memória. Vem com **3 exemplos genéricos inventados** só pra ilustrar o formato.
- Cada memória: frontmatter (`name`, `description`, `metadata.type: user|feedback|project|reference`) + corpo com `[[links]]`.
- A skill explica o porquê do formato e como o `/tldr` o alimenta.

## 5. Add-ons de nicho

Em `add-ons/`, **desativados por padrão**. No fim do setup, a skill sugere conforme o perfil inferido. Cada add-on é um molde (espinha da skill + placeholders `[descreva seu processo]`), derivado dos sabores existentes mas **esvaziado de conhecimento específico**:
- **Marketing** — `/oferta`, estrutura de copy genérica.
- **Conteúdo** — `/conteudo`, fluxo de criação.
- **Deals/Consultor** — `/deal`, `/reuniao`.

Decisão skill-por-skill acontece na implementação: cada uma é mostrada ao autor antes de incluir.

## 6. README

Pra quem nunca viu: (1) o que é, (2) por que (problema + solução), (3) pré-requisitos (Claude Code + Obsidian opcional), (4) instalação (clonar + abrir Claude Code na pasta + `/brain-setup`), (5) tabela do que cada peça faz, (6) como modelar do seu jeito.

## 7. Garantia de privacidade (checkpoint inegociável)

Antes de publicar, varredura final com grep por:

| Categoria | Exemplos a eliminar |
|---|---|
| Nome / handles do autor | "Thiago", "midasgrowth", "Garbo" |
| Paths pessoais | `C:\Users\Thiago`, nomes de vault reais |
| Negócios / clientes | Flowly, Semente Energética, Valeria, ofertas reais |
| Credenciais / IDs | tokens, pixel IDs, IDs de planilha, contas Meta |
| Memórias reais | nenhuma; só exemplos inventados neutros |

Roda como passo de verificação no plano, com grep final antes do "pronto pra publicar".

## 8. Fora de escopo (YAGNI)

- Script bash/ps1 de instalação automática (a skill faz o trabalho).
- Automação de `claude mcp add` (só documentação).
- Publicação automática no GitHub (autor publica quando quiser).
- Suporte a outras ferramentas de notas além de Obsidian (Obsidian é o alvo; estrutura é markdown puro, então funciona em qualquer editor, mas a doc foca Obsidian).

## 9. Critérios de sucesso

1. Uma pessoa sem contexto clona, roda `/brain-setup`, e em diálogo sai com um vault funcional adaptado a ela.
2. A skill responde às 4 perguntas-FAQ corretamente sem inventar.
3. Nada é construído sem confirmação; nada é sobrescrito sem backup.
4. Grep de privacidade passa limpo (zero ocorrências das categorias da seção 7).
5. Funciona em Windows e Mac.
