# Conceitos — o que é um segundo cérebro

Se você nunca viu isso, comece por aqui. Este documento explica a ideia inteira em
texto corrido, sem jargão. Leia uma vez do começo ao fim e o resto do kit faz sentido.

## O que é o segundo cérebro

O "segundo cérebro" é a combinação de quatro peças que trabalham juntas:

1. **Um vault do Obsidian** — uma pasta de arquivos de texto (`.md`) onde mora todo o
   seu conhecimento: decisões, números, configs, histórico de cada projeto. É só texto,
   legível por você e pela IA, sem banco de dados nem nuvem obrigatória.
2. **Um sistema de memória** — um conjunto de arquivos curtos que guardam fatos
   duradouros sobre você e seus projetos, com um índice que a IA lê no começo da sessão.
3. **Skills (slash commands)** — pequenos comandos como `/diario` e `/tldr` que disparam
   rotinas prontas (carregar contexto, resumir e salvar). Você digita o comando, a skill
   faz o trabalho. Cada skill mora numa pasta `.claude/skills/<nome>/SKILL.md` dentro do
   vault.
4. **Um `CLAUDE.md` inteligente** — um arquivo de instruções que diz à IA quem você é,
   como você trabalha e onde procurar cada coisa. É a primeira coisa que ela lê.

Juntas, essas quatro peças transformam o Claude Code de uma ferramenta que esquece tudo
em um parceiro que lembra do que importa.

## O problema que isso resolve

O Claude Code começa **cada sessão do zero**. Ele não tem lembrança nativa do que vocês
conversaram ontem, da decisão que vocês tomaram na semana passada, ou do teste que já
deu errado. Em trabalho curto isso não incomoda. Em trabalho longo — um projeto que dura
semanas, várias frentes acontecendo ao mesmo tempo — você perde o fio: o que foi
decidido, o que já foi testado, o que ainda está pendente. Você acaba reexplicando
contexto toda vez, ou pior, repetindo um erro que já tinha resolvido.

O segundo cérebro existe para que esse contexto **não dependa da sua memória nem da
memória da conversa**. Ele vive em arquivos.

## Por que isso é melhor que usar o Claude "puro"

A diferença é **contexto persistente entre sessões** em vez de recomeçar do zero.

O ponto-chave é este: **o que fica só na conversa é comprimido e perde detalhe.** Conforme
uma conversa fica longa, a IA resume o passado para caber no que ela consegue segurar de
uma vez — e nesse resumo os números exatos, os nomes precisos e as decisões finas se
diluem. Já **o que está ancorado num arquivo do vault nunca se perde**: está escrito, fica
escrito, e é relido na íntegra sempre que for preciso.

Por isso a regra de ouro do método é: decisão importante, número, config, nome exato →
**escreva num arquivo na hora**, não deixe só no chat. O chat é onde você pensa; o vault é
onde você guarda.

## Como funciona a memória

Cada memória é um **arquivo separado** com duas partes:

- **Frontmatter** (o bloco no topo) com os campos `name`, `description` e
  `metadata.type`. O tipo pode ser:
  - `user` — fatos sobre você (quem é, o que faz).
  - `feedback` — preferências suas de como a IA deve trabalhar.
  - `project` — contexto de um projeto que não está no código.
  - `reference` — conhecimento transversal reutilizável (ex.: "como eu faço X").
- **Um corpo curto** — poucas linhas, direto ao ponto. Memória não é redação.

Um arquivo `MEMORY.md` funciona como **índice**: uma linha por memória, com um link para
ela. É esse índice que a IA lê no início para saber o que existe. As memórias se ligam
entre si com `[[links]]`, do mesmo jeito que notas se conectam no Obsidian.

### Quando salvar uma memória

- Um **fato não-óbvio** que você vai querer de volta depois (uma decisão, um padrão).
- Uma **preferência sua de trabalho** (como você gosta que a IA responda ou execute).
- **Contexto de projeto** que não está escrito em nenhum código ou arquivo do projeto.

### Quando NÃO salvar

- O que o **próprio código ou os arquivos do projeto já registram** — não duplique.
- O que só importa **naquela conversa específica** e morre com ela.

Memória boa é curta e tem poucas entradas que importam de verdade. Mais não é melhor.

## O ritual: `/diario` de manhã, `/tldr` no fim

O método tem dois momentos por sessão:

- **`/diario` no começo** — carrega o contexto e as prioridades. A IA lê seu `CLAUDE.md`,
  o índice de memória e suas notas recentes, e te devolve onde você parou e o que faz
  sentido atacar agora. Você começa com o fio na mão, não do zero.
- **`/tldr` no fim** — resume o que aconteceu na sessão e **salva** o que vale guardar:
  atualiza notas do vault e cria ou ajusta memórias.

Deixe isto explícito na sua cabeça: **é o `/tldr` no fim que faz a memória CRESCER.**
Sem `/tldr`, a sessão termina e o aprendizado evapora junto com a conversa — o cérebro não
aprende. O `/diario` colhe; o `/tldr` planta. Os dois juntos fecham o ciclo.

## Princípios que você vai moldar

Três ideias guiam como o cérebro se organiza:

- **Separe por projeto.** Cada projeto ou área é uma pasta, e todo o conhecimento dele
  mora ali dentro — copy, decisões, números, histórico. Quando você volta a um projeto,
  abre a pasta dele e tem tudo junto, sem caçar.
- **Conhecimento transversal vira memória, não fica preso a um projeto.** Coisas do tipo
  "como eu faço X", que valem para qualquer projeto, viram uma memória do tipo `reference`
  e são referenciadas no `CLAUDE.md` geral. Assim você as reaproveita em todo lugar.
- **Isto é um esqueleto.** O kit te entrega a estrutura, os comandos e os moldes, mas o
  formato final é seu. Renomeie pastas, mude o ritual, adapte os campos. **A pessoa molda
  do jeito dela** — e o cérebro fica bom à medida que você o ajusta ao seu trabalho real.

Pronto. Com esses conceitos na mão, rode `/diario`, trabalhe, e termine com `/tldr`. O
resto vem com o uso.
