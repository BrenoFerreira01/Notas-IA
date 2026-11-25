---
name: organizacao-de-notas
description: Use quando for criar, mover, renomear ou salvar qualquer nota, arquivo ou imagem neste vault do Obsidian (Notas-IA): notas de estudo, pesquisas, resumos, planejamentos, backlogs, anotações de aula, ou quando o usuário pedir para "guardar", "anotar" ou "colocar no Obsidian", mesmo sem citar tags, template ou pastas. Use também ao criar arquivos temporários ou de rascunho dentro do vault.
---

# Organização de Notas (vault Notas-IA)

Este vault é organizado por **pastas numeradas** e por **tags que são notas-índice**. Cada nota nova precisa entrar nos dois sistemas: ficar na pasta certa e ser linkada a partir de pelo menos um índice. Uma nota sem tag fica órfã: ninguém a encontra navegando pelos índices.

## Estrutura do vault

| Pasta | O que guarda |
|---|---|
| `1 - Templates/` | Templates do Obsidian. Não crie notas aqui. |
| `2 - Indexes/` | Uma nota por tag. Cada uma lista as notas daquela tag. |
| `3 - Full Notes/` | Notas definitivas (destino padrão de toda nota nova). |
| `3 - Full Notes/3.1 imagens/` | Imagens usadas nas notas. |
| `3 - Full Notes/3.5 - Planejamentos/` | Planos, backlogs e planejamentos de projeto. |
| `4 - Temp Notes/` | Arquivos e pastas temporários, rascunhos, saídas intermediárias. |

Não crie arquivos novos na raiz do vault nem em `.obsidian/`. Os arquivos que já estão na raiz (`README.md`, `Snippets Commands.md`) ficam onde estão.

Para preencher `data` e `horário`, use o relógio do sistema (ex.: `date` no terminal).

## Criar uma nota nova

### 1. Escolha o nome e a pasta

- O nome do arquivo é o título da nota, em português, descritivo: `Pesquisar certificados gratuitos.md`.
- Espaços, acentos e pontos (`Next.js`) são permitidos. Não use `# ^ [ ] | \ / :` no nome, porque quebram os links do Obsidian.
- Para uma série de notas sobre o mesmo assunto, use um prefixo comum para que fiquem juntas no índice: `Next.js - como usar o básico`, `Next.js - como criar o backend de um projeto`. Ao criar várias notas de uma vez, linke-as entre si com wikilinks.
- Pasta: `3 - Full Notes/`, ou `3 - Full Notes/3.5 - Planejamentos/` se for um plano ou backlog.
- Antes de criar, procure se já existe nota com nome ou assunto parecido. Se existir, complemente a nota existente em vez de duplicar.

### 2. Preencha o template

O template fica em `1 - Templates/Template para Notas Novas.md`. Ele usa variáveis do Obsidian (`{{date}}`, `{{time}}`) que **só funcionam quando o Obsidian insere o template**. Ao escrever o arquivo diretamente, substitua pelos valores reais. A nota deve começar exatamente assim (incluindo a linha em branco inicial):

```markdown

data: 25.11.2026
horário: 14:30
Tags: [[Estudo]]

---

# Título da nota

Conteúdo...
```

- `data`: data atual no formato `DD.MM.AAAA`.
- `horário`: hora atual no formato `HH:mm` (24h).
- `Tags`: uma ou mais tags como links, separadas por vírgula: `Tags: [[Estudo]], [[Programação]]` (veja o passo 3).
- Isto **não** é frontmatter YAML: não envolva os campos com `---` no topo. O `---` aparece só uma vez, separando o cabeçalho do conteúdo.

### 3. Atribua a tag (obrigatório)

Toda nota nova tem pelo menos uma tag. Uma tag é um link `[[NomeDaTag]]` que aponta para a nota `2 - Indexes/NomeDaTag.md`.

Uma nota pode (e deve, quando fizer sentido) ter **mais de uma tag**: uma ampla, que diz o propósito (`Estudo`), e outras que dizem o assunto (`Programação`, `DevOps`). Todas ficam na mesma linha, separadas por vírgula e espaço, da mais ampla para a mais específica:

```markdown
Tags: [[Estudo]], [[Programação]]
```

1. Liste `2 - Indexes/` e use as tags existentes que se encaixarem no assunto (prefira reaproveitar a criar tags quase iguais).
2. Se nenhuma servir, crie `2 - Indexes/NomeDaTag.md`. Nome curto, em Title Case, sem template, contendo apenas a lista de links.
3. Adicione o link da nova nota na nota-índice de **cada** tag usada.

### 4. Atualize o índice em ordem alfabética

Uma nota-índice é só uma lista de links, um por linha, ordenada alfabeticamente pelo nome da nota:

```markdown
- [[Atividade Final - DevOps na Prática]]
- [[Pesquisar certificados gratuitos]]
- [[Projetos para enriquecer meu portfólio de programação]]
```

- Insira o novo link na posição alfabética correta; não o coloque simplesmente no fim. Ordene como o explorador de arquivos do Obsidian: sem diferenciar maiúsculas/minúsculas e tratando letras acentuadas como a letra base (`básico` fica junto de `basico`).
- Use `- ` como marcador. Se o índice ainda usar `* `, padronize para `- ` ao editá-lo.
- O link é o nome do arquivo sem `.md` e sem caminho de pasta.

## Links e imagens

- Para citar outra nota do vault, use wikilink: `[[Nome da nota]]`.
- Imagens vão em `3 - Full Notes/3.1 imagens/` e são embutidas com `![[nome-da-imagem.png]]` (só o nome do arquivo, sem caminho).
- Links para arquivos fora do vault ou sites usam markdown normal: `[texto](url)`.

## Arquivos temporários

Tudo que é temporário (rascunhos, scripts auxiliares, saídas intermediárias, arquivos de teste) vai em `4 - Temp Notes/`, inclusive subpastas. Esses arquivos não precisam de template nem de tag. Se um rascunho virar nota definitiva, mova-o para `3 - Full Notes/` e aplique os passos acima.

## Checklist antes de terminar

- [ ] Nota está em `3 - Full Notes/` (ou `3.5 - Planejamentos/`), com nome descritivo e sem caracteres proibidos.
- [ ] Cabeçalho com `data`, `horário` e `Tags` preenchidos com valores reais (nenhum `{{...}}` sobrando).
- [ ] Pelo menos uma tag `[[...]]`, e cada uma existe em `2 - Indexes/`.
- [ ] Cada índice usado lista a nota nova, em ordem alfabética.
- [ ] Imagens em `3.1 imagens/`; temporários em `4 - Temp Notes/`.

## Erros comuns

| Erro | Correção |
|---|---|
| Deixar `{{date:DD.MM.YYYY}}` no arquivo | Escrever a data real, ex. `25.11.2026`. |
| `Tags: #estudo` ou `tags: [estudo]` | Tags são wikilinks: `Tags: [[Estudo]]`. |
| Várias tags em linhas separadas ou sem vírgula | Uma linha só: `Tags: [[Estudo]], [[Programação]]`. |
| Adicionar uma segunda tag só no cabeçalho | A nota também entra no índice dessa tag. |
| Criar a nota mas esquecer o índice | Sempre editar `2 - Indexes/<Tag>.md` junto. |
| Adicionar o link no fim do índice | Inserir na posição alfabética. |
| Criar `Devops.md` quando já existe `DevOps.md` | Conferir os índices existentes antes de criar tag. |
| Salvar rascunho em `3 - Full Notes/` | Rascunhos vão para `4 - Temp Notes/`. |
