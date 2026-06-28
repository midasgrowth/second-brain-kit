# second-brain-kit

Transforme seu Claude Code num **segundo cérebro**: um parceiro que lembra do que importa
entre sessões, em vez de começar do zero toda vez.

Você clona este repositório, abre o Claude Code dentro dele e roda **`/brain-setup`**. A
skill conversa com você, entende quem você é e o que você faz, e monta um vault do Obsidian +
sistema de memória + skills + um `CLAUDE.md` inteligente — tudo adaptado a você, em diálogo, e
sem construir nada sem a sua confirmação.

---

## O problema que isso resolve

O Claude Code começa **cada sessão do zero**. Em trabalho longo — projetos que duram semanas,
várias frentes ao mesmo tempo — você perde o fio: o que foi decidido, o que já foi testado, o
que está pendente. Acaba reexplicando tudo de novo.

Um vault de notas + um sistema de memória ancorado em arquivos resolve isso: o contexto
**persiste**. O que fica só na conversa pode ser comprimido e perder detalhe; o que está num
arquivo do vault nunca se perde — é relido quando precisa.

> Quer entender a ideia inteira antes de instalar? Leia [docs/conceitos.md](docs/conceitos.md).

---

## Pré-requisitos

- **Claude Code** instalado ([claude.com/code](https://claude.com/code)).
- **Obsidian** (opcional, mas recomendado) — o vault é só uma pasta de arquivos `.md`, então
  funciona em qualquer editor; o Obsidian deixa a navegação e os links muito melhores.

---

## Instalação

1. **Clone** este repositório onde quiser que o seu cérebro viva:
   ```
   git clone <url-deste-repo> meu-segundo-cerebro
   ```
2. **Abra o Claude Code** dentro da pasta:
   ```
   cd meu-segundo-cerebro
   claude
   ```
3. **Rode a skill**:
   ```
   /brain-setup
   ```
4. Converse. A skill pergunta sobre você, propõe uma estrutura, e constrói tudo **só depois
   que você confirmar**. Se quiser, peça antes: *"me explica como isso funciona."*

---

## O que cada peça faz

| Peça | Para que serve |
|---|---|
| **`/brain-setup`** | A skill mentor. Entende você, detecta o que já tem, explica os conceitos e monta o cérebro em diálogo. |
| **Vault** | Uma pasta de arquivos `.md` onde mora todo o seu conhecimento: decisões, números, histórico de cada projeto. |
| **Sistema de memória** | Arquivos curtos com fatos duradouros + um `MEMORY.md` que indexa tudo e a IA lê no começo da sessão. |
| **`/diario`** | Começa o dia: carrega contexto, lista pendências e destaca as 3 prioridades. |
| **`/tldr`** | Fecha a sessão: resume decisões e próximas ações, salva na pasta certa e **alimenta a memória**. |
| **Add-ons** | Skills de nicho opcionais (marketing, conteúdo, deals). Veja [docs/add-ons.md](docs/add-ons.md). |
| **MCPs** | Conectores opcionais (planilhas, notas, drives). Veja [docs/mcp.md](docs/mcp.md). |

O ritual é simples: **`/diario` de manhã, `/tldr` no fim.** É o `/tldr` que faz a memória
crescer — sem ele, o cérebro não aprende.

---

## É um esqueleto — molde do seu jeito

Nada aqui é fixo. As pastas, os nomes, as skills, as regras do `CLAUDE.md` — tudo é ponto de
partida. O `/brain-setup` propõe melhorias (separar por projeto, deixar conhecimento
transversal como memória referenciada no `CLAUDE.md` geral, manter o `/tldr` religioso), mas
você pode aceitar, mudar ou ignorar. O objetivo é te dar uma base boa e deixar você adaptá-la.

---

## Licença

[MIT](LICENSE). Use, modifique e compartilhe à vontade.
