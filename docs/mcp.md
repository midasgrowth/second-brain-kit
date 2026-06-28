# MCP — conectando ferramentas externas ao Claude

## O que é um MCP

MCP (Model Context Protocol) é o padrão que permite plugar **conectores** no Claude Code.
Um conector MCP dá à IA acesso a uma ferramenta externa — uma planilha, suas notas, um
drive de arquivos — para que ela possa **ler e às vezes editar** essas ferramentas direto,
sem você ficar copiando e colando dados na conversa.

Na prática: em vez de você exportar uma planilha e colar o conteúdo aqui, você conecta a
planilha via MCP e pede "leia a aba de resultados". A IA acessa, lê e responde. O conector
é a ponte entre o Claude e o mundo lá fora.

## Exemplos úteis

Alguns conectores que costumam render bastante no dia a dia:

- **Google Sheets** — ler e editar planilhas. Bom para acompanhar números, atualizar
  tabelas de controle, puxar dados sem export manual.
- **Notion** — ler e escrever páginas e bancos de dados. Útil se sua base de notas ou
  documentação vive lá.
- **Google Drive** — buscar e ler arquivos (documentos, PDFs, planilhas) guardados no seu
  drive.

Esses são só exemplos. O ecossistema de MCPs é grande e cresce; a ideia é a mesma para
qualquer um: um conector que expõe uma ferramenta para a IA usar.

## Como conectar um MCP no Claude Code

O comando geral, de forma ilustrativa, é:

```
claude mcp add <nome-do-conector> -- <comando-que-inicia-o-servidor>
```

Por exemplo, de maneira **genérica** (sem credenciais reais), conectar um servidor seria
algo como:

```
claude mcp add minhas-planilhas -- npx -y algum-servidor-mcp-de-planilhas
```

Cada conector tem suas próprias instruções de instalação e seu próprio jeito de receber
autenticação (variáveis de ambiente, arquivo de credencial, login no navegador, etc.).
Consulte a documentação do conector que você quer usar e siga o passo a passo dele. Depois
de adicionado, você pode verificar com `claude mcp list` e remover com
`claude mcp remove <nome-do-conector>`.

Um detalhe que pega iniciante: o conector é um **servidor** — um processo que precisa estar
no ar para funcionar. O comando que você passa depois do `--` é justamente o que inicia esse
servidor; o Claude Code o sobe quando precisa. Se um conector parar de responder, na maioria
das vezes é porque o servidor não está rodando ou a credencial expirou — confira esses dois
pontos primeiro.

## Regra de segurança

**Use SUAS próprias credenciais; nunca compartilhe ou versione tokens.**

Os exemplos acima são propositalmente genéricos e não contêm nenhuma credencial. Quando for
configurar de verdade:

- Gere e use **suas** chaves, tokens e IDs — vindos das suas próprias contas.
- **Nunca** cole tokens reais em arquivos que vão para o Git. Mantenha-os fora do
  versionamento (por exemplo, em variáveis de ambiente ou em arquivos ignorados pelo
  `.gitignore`).
- **Nunca** compartilhe um token com outra pessoa nem em chat público. Um token vazado é um
  acesso vazado.

Tratando credencial como segredo desde o começo, você ganha toda a conveniência dos MCPs
sem abrir a porta para problema nenhum.
