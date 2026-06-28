# Add-ons — skills de nicho opcionais

## O que são

Add-ons são **skills de nicho, opcionais e desativadas por padrão**. Elas ficam guardadas
na pasta `add-ons/` do kit e não entram em ação até você ativá-las de propósito. A ideia é
manter o cérebro base enxuto: você liga só o que faz sentido para o seu tipo de trabalho.

Todas as add-ons são **moldes com placeholders** — esqueletos prontos que você preenche com
o seu contexto real. Elas dão a estrutura do comando; você adapta o conteúdo ao seu caso.

## Como ativar uma add-on

Ativar é mover a pasta da skill de dentro de `add-ons/` para dentro de `.claude/skills/`:

```
add-ons/<nome>/   →   .claude/skills/<nome>/
```

Por exemplo, para ativar a add-on **marketing**, mova a pasta `add-ons/marketing/` para
`.claude/skills/marketing/`. Assim que a skill estiver em `.claude/skills/`, o comando dela
fica disponível no Claude Code. Para desativar de novo, é só mover a pasta de volta para
`add-ons/`.

Depois de ativar, abra os arquivos da skill e **troque os placeholders** pelo seu contexto
de verdade antes de usar.

## Add-ons disponíveis

- **marketing** — skill `/oferta`: carrega o contexto de uma oferta (histórico, decisões e
  testes daquela oferta) para você retomar de onde parou.
- **conteudo** — skill `/conteudo`: desenvolve uma ideia de conteúdo na voz do criador,
  partindo do tom e dos ângulos que você já definiu.
- **deals** — skill `/deal`: carrega o contexto de uma oportunidade ou parceria (status,
  partes envolvidas e próximos passos) para conduzir a negociação com o fio na mão.

Cada uma é um ponto de partida. Ative as que servem ao seu trabalho, preencha os
placeholders e molde do seu jeito.
