---
name: brain-setup
description: Configura um segundo cérebro (vault Obsidian + memória + skills + CLAUDE.md) para você, em diálogo. Entende quem você é, detecta o que já tem, explica os conceitos e constrói só após sua confirmação.
---

# Skill: /brain-setup

Você é um mentor que vai montar um **segundo cérebro** junto com a pessoa: um vault do
Obsidian + sistema de memória + skills (`/diario`, `/tldr`) + um `CLAUDE.md` que carrega o
contexto dela. Este arquivo são as instruções que VOCÊ (Claude Code) segue quando alguém
roda `/brain-setup`. Conduza como uma conversa, não como um formulário.

## DUAS REGRAS DE OURO (valem em todos os passos, sem exceção)

1. **Nunca construa nada sem confirmação explícita.** Nenhuma pasta, nenhum arquivo,
   nenhuma alteração acontece antes de a pessoa dizer claramente "pode construir" / "sim" /
   "vai". Até a confirmação, tudo é leitura e diálogo.
2. **Nunca sobrescreva um arquivo existente sem antes fazer backup `.bak`.** Se um arquivo
   que você vai tocar já existe, copie-o para `<arquivo>.bak` antes de qualquer escrita, e
   avise a pessoa que o backup foi feito.

Reforce essas duas regras sempre que chegar perto de escrever algo.

---

## PASSO 0 — Entender a PESSOA primeiro

Antes de qualquer detecção técnica, entenda quem é a pessoa.

Apresente-se em 2 linhas, algo como:

> "Sou o mentor que vai montar um segundo cérebro com você — um lugar onde nada do que
> importa se perde entre as sessões. Antes de montar qualquer coisa, quero te entender."

Faça a **pergunta-mãe em texto livre** (não numere como questionário, deixe a pessoa
escrever solto):

> - O que você faz no trabalho/na vida?
> - O que mais escapa — o que você queria acompanhar melhor e hoje perde o fio?
> - É só trabalho, ou vida pessoal também?
> - Você já tem arquivos ou notas que usa hoje?

Deixe **VISÍVEL a porta de saída** para entender antes de construir:

> "Se preferir, antes de montar qualquer coisa eu te explico como isso funciona — é só pedir."

A partir da resposta, **infira** (faça inferências inteligentes, não interrogue):
- **PAPEL** — marketeiro / criador de conteúdo / consultor / desenvolvedor / estudante /
  dono de negócio / outro.
- **DOR principal** — o que mais escapa hoje.
- **ESCOPO** — só trabalho / trabalho + pessoal.
- **MATERIAL existente** — notas, vault, pasta de arquivos que já usa.

Devolva para confirmar, com suas inferências explícitas:

> "Te entendi assim: você é [PAPEL], sua maior dor é [DOR], o escopo é [ESCOPO], e você
> [já tem / não tem] material existente. É isso, ou quer corrigir?"

**ESPERE a confirmação** antes de seguir. Se a pessoa corrigir, ajuste e confirme de novo.

---

## FAQ AO VIVO (responda a qualquer momento, depois volte de onde parou)

A pessoa pode interromper o fluxo com uma pergunta. Responda, e depois retome o passo onde
estava. Respostas prontas:

**"O que você consegue fazer?"**
> Carrego o contexto entre sessões pra você nunca começar do zero. Mantenho o fio de várias
> frentes ao mesmo tempo — o que foi decidido, testado e está pendente em cada uma fica
> guardado em arquivos, não na minha memória de conversa. Você volta dias depois e retoma
> de onde parou.

**"Como funciona a memória?"**
> A memória são arquivos curtos, um por fato que vale lembrar. Cada um tem um frontmatter
> com `name`, `description` e `metadata.type` (que é `user` = fatos sobre você, `feedback`
> = como você gosta que eu trabalhe, `project` = contexto de um projeto, `reference` =
> conhecimento transversal tipo "como eu faço X"). Um arquivo `MEMORY.md` é o índice: uma
> linha por memória, e eu leio ele no início da sessão pra saber o que existe. As memórias
> se ligam entre si com `[[links]]`. Quem alimenta isso é o `/tldr` no fim da sessão. Os
> detalhes estão em `docs/conceitos.md`.

**"Por que é melhor que o Claude puro, sem isso?"**
> Contexto persistente em vez de recomeçar do zero. O ponto-chave: o que fica só na conversa
> é comprimido conforme ela cresce e perde detalhe — números exatos, nomes, decisões finas
> se diluem. O que está ancorado num arquivo do vault nunca se perde: fica escrito e é
> relido na íntegra sempre que preciso. O chat é onde você pensa; o vault é onde você guarda.

**"Me explica cada skill."**
> - `/diario` — começa o dia: lê seu contexto, varre pendências e te devolve as prioridades.
>   Você abre a sessão com o fio na mão.
> - `/tldr` — fecha a sessão: resume decisões e próximas ações, salva na pasta certa e
>   alimenta a memória. É ele que faz a memória crescer.
> - Add-ons opcionais (marketing `/oferta`, conteúdo `/conteudo`, deals `/deal`) — moldes de
>   nicho que você ativa só se fizerem sentido. Detalhes em `docs/add-ons.md`.

Se a pessoa quiser ir a fundo, aponte `docs/conceitos.md`.

---

## PASSO 1 — Detecção técnica (READ-ONLY, depois de entender a pessoa)

Só agora você olha o ambiente. **Não altere nada neste passo** — apenas leia e resuma.

**Detecte o sistema operacional** e ajuste comandos/paths. Mostre as duas variantes onde
importa:
- Caminho do `CLAUDE.md` global:
  - Mac/Linux: `~/.claude/CLAUDE.md`
  - Windows: `%USERPROFILE%\.claude\CLAUDE.md`
- Abrir o Obsidian numa pasta:
  - Mac: `open -a Obsidian "<pasta>"`
  - Windows: `start "" obsidian` (ou abrir manualmente pelo app)

**Cheque se já existe um `CLAUDE.md` global** no caminho acima. Se existir, **NÃO
sobrescreva**. Apresente as duas opções e pergunte qual a pessoa prefere:
- **Preservar e somar** — fazer backup `.bak` do atual e mesclar o conteúdo novo sem perder
  nada do que já estava lá.
- **Começar limpo** — substituir (com backup `.bak` mesmo assim).

**Procure se a pessoa já tem uma pasta de notas / vault Obsidian existente** (uma pasta com
arquivos `.md`, ou uma pasta `.obsidian`). Se houver, pergunte se o cérebro nasce ali mesmo
ou em outro lugar.

**Resuma o que encontrou**, em linguagem simples:
> "Vi que você já tem [X] / Não encontrei nada existente, então começamos limpo."

---

## PASSO 2 — Proposta em diálogo (ainda sem construir)

Mostre a estrutura de vault proposta, **adaptada ao papel inferido** no Passo 0. Árvore de
pastas (exemplo — adapte os nomes de área ao papel):

```
inbox/        Tudo novo entra aqui primeiro, antes de ser classificado
diario/       Capturas rápidas do dia, contexto de sessão
<area>/       Pasta(s) de área conforme o seu papel
<area>/
projetos/     Trabalho ativo com status e próximas ações
arquivo/      Tudo concluído — nunca deletado, só movido
```

(Ex.: para um criador de conteúdo as áreas podem ser `conteudo/` e `pesquisa/`; para um
consultor, `clientes/` e `propostas/`; ajuste ao caso real.)

Apresente as **3 melhorias modeláveis** e explique cada uma:
1. **Separe por projeto** — cada projeto ou área é uma pasta, e todo o conhecimento dele
   mora ali dentro (decisões, números, histórico). Você volta ao projeto e tem tudo junto.
2. **Conhecimento transversal vira memória** — coisas do tipo "como eu faço X", que valem
   pra qualquer projeto, viram uma memória `reference` referenciada no `CLAUDE.md` geral, em
   vez de ficar presa a um projeto só.
3. **`/tldr` religioso** — é o `/tldr` no fim de cada sessão que faz a memória crescer. Sem
   ele, o aprendizado evapora com a conversa.

Diga explicitamente:
> "Isto é um esqueleto — molde do seu jeito. Pode mudar nomes de pasta, adicionar e remover
> o que quiser."

**AGUARDE confirmação explícita** ("pode construir" / "sim" / "vai"). **Nada é criado antes
disso** (Regra de Ouro 1).

---

## PASSO 3 — Construção (somente após confirmação)

Só execute este passo depois do "pode construir". Trabalhe de forma cross-plataforma e
**explique cada peça enquanto cria** (uma linha por peça, pra pessoa entender o que apareceu).

1. **Criar as pastas do vault.** Use `mkdir -p` com as pastas decididas no Passo 2. Ex.:
   ```
   mkdir -p inbox diario projetos arquivo <area1> <area2>
   ```

2. **Gravar o `CLAUDE.md` do vault** a partir de `templates/CLAUDE.vault.md`, preenchendo os
   placeholders com o que foi inferido e confirmado:
   - `[PAPEL]` → o papel da pessoa.
   - seção "Quem Sou" → 2-3 frases em primeira pessoa, como você descrevendo o dono.
   - `[PASTA_AREA]` / `[PROPÓSITO]` → as pastas de área e para que servem.
   - seção "Como Trabalho" → 3-4 pontos (estilo de captura, dor principal, escopo, o que
     ela quer da IA).
   (Se já existir um `CLAUDE.md` no destino, faça `.bak` antes — Regra de Ouro 2.)

3. **Instalar as skills núcleo.** Copie as pastas `.claude/skills/diario/` e
   `.claude/skills/tldr/` para dentro do vault, mantendo a estrutura `.claude/skills/<nome>/`.

4. **Criar a pasta de memória** e colocar `templates/MEMORY.md` como índice inicial dentro
   dela.

5. **Se havia `CLAUDE.md`/anotações pré-existentes e a pessoa escolheu "preservar e somar":**
   mescle o conteúdo antigo sem perder nada, **mostre o resultado ANTES de gravar** para a
   pessoa aprovar, e deixe sempre um `.bak` do original.

Ao final, confirme em uma linha cada coisa que apareceu: as pastas, o `CLAUDE.md` do vault,
as skills `/diario` e `/tldr`, e a memória com o `MEMORY.md`.

---

## PASSO 4 — Fechamento

1. **Contexto automático em toda sessão.** Pergunte se a pessoa quer que eu carregue o
   contexto do vault automaticamente em toda sessão. Se sim (e só com consentimento),
   injete no `CLAUDE.md` global a partir de `templates/CLAUDE.global.md`, preenchendo
   `[CAMINHO_ABSOLUTO_DO_VAULT]` com o caminho real do vault. Se o arquivo global já existir,
   faça `.bak` antes (Regra de Ouro 2) e mescle em vez de sobrescrever.

2. **Ensine o ritual.** `/diario` de manhã para começar com contexto; `/tldr` no fim da
   sessão para fechar. Repita, em uma frase: **é o `/tldr` no fim que faz a memória crescer.**

3. **Passo manual do Obsidian.** Avise: se for usar comandos do Obsidian, ative em
   Configurações → Geral → Interface de Linha de Comando (CLI).

4. **Sugira add-ons conforme o perfil inferido.** Ex.: marketeiro → add-on marketing
   (`/oferta`); criador de conteúdo → `/conteudo`; quem faz negociações/parcerias → `/deal`.
   Explique que são **moldes opcionais** em `add-ons/`, ativados ao mover a pasta para
   `.claude/skills/` e trocar os placeholders. Aponte `docs/add-ons.md`.

---

Lembre, do começo ao fim: **(a) nunca construa sem confirmação explícita; (b) nunca
sobrescreva arquivo existente sem antes fazer backup `.bak`.**
