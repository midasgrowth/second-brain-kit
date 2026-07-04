# Plugins — o motor de skills do Claude Code

O vault e as skills deste kit (`/brain-setup`, `/diario`, `/tldr`) já funcionam sozinhos.
Mas o que deixa o Claude Code **realmente poderoso** — dezenas de skills prontas para pensar,
planejar, debugar e trabalhar com método — vem de um plugin chamado **Superpowers**.

Se você quer a experiência completa (a mesma que fez quem te passou este kit virar fã do
Claude), instale os Superpowers. É o passo que transforma o Claude "puro" no Claude que
sabe COMO fazer as coisas, não só o quê.

## O que são plugins

Plugins são pacotes de skills, comandos e agentes que você instala no Claude Code a partir de
um **marketplace** (um repositório que lista plugins disponíveis). Uma vez instalado, o
plugin carrega suas skills automaticamente em toda sessão.

- Um **marketplace** é a "loja" — um repo no GitHub com uma lista de plugins.
- Um **plugin** é o pacote que você instala daquela loja.

## Superpowers — o plugin essencial

Os **Superpowers** trazem um conjunto de skills de *processo* — como brainstormar antes de
construir, escrever planos, debugar de forma sistemática, revisar código, e muito mais. São
elas que fazem o Claude trabalhar com disciplina em vez de sair fazendo.

### Como instalar

Dois comandos, rodados no terminal (não dentro do Claude):

```
claude plugin marketplace add anthropics/claude-plugins-official
claude plugin install superpowers@claude-plugins-official
```

O primeiro adiciona o marketplace **oficial da Anthropic**. O segundo instala os Superpowers
a partir dele. Reinicie o Claude Code depois de instalar para as skills carregarem.

### Verificar e gerenciar

```
claude plugin list                      # ver o que está instalado
claude plugin marketplace list          # ver os marketplaces adicionados
claude plugin update superpowers@claude-plugins-official   # atualizar
claude plugin uninstall superpowers@claude-plugins-official
```

## Frontend / design — outro plugin que vale muito

Se você constrói interfaces (páginas, apps, componentes, dashboards) ou só quer que o que o
Claude gera seja **bonito** em vez de genérico, instale o plugin **frontend-design**. Ele é
uma skill oficial da Anthropic que dá ao Claude senso de design de verdade — layout, tipografia,
cor, responsividade.

Depois de já ter adicionado o marketplace oficial (passo acima), instalar é um comando:

```
claude plugin install frontend-design@claude-plugins-official
```

Não é obrigatório para o segundo cérebro funcionar — mas se design faz parte do que você faz,
é dos plugins que mais mudam o resultado.

## Explorar outros plugins

O marketplace oficial tem mais do que os Superpowers. Depois de adicionar o marketplace, você
pode explorar o que há disponível pelo próprio Claude Code — rode `/plugin` numa sessão para
navegar e instalar pela interface, sem decorar comandos.

Existem também marketplaces de terceiros (qualquer repo GitHub no formato certo). Para adicionar
um, é o mesmo comando: `claude plugin marketplace add <usuario/repo>`. Use só marketplaces em
que você confia — um plugin roda no seu ambiente.

## Ordem recomendada de setup

1. Rode o `/brain-setup` deste kit e monte seu vault (o cérebro onde tudo é guardado).
2. Instale os **Superpowers** (os dois comandos acima) — o motor de skills.
3. Conecte os **MCPs** que fizerem sentido para você (veja `docs/mcp.md`) — as pontes para
   suas ferramentas externas (planilhas, notas, drive).

Com os três, você tem o setup completo: um cérebro que não esquece + skills que sabem
trabalhar + conexões com o mundo lá fora.
