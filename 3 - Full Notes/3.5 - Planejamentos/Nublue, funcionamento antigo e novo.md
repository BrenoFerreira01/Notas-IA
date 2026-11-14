## Visão geral

O repositório contém um **frontend React com Vite e TypeScript**. A tela inicial é montada em [src/main.tsx](D:/BRENO/Documentos/_0 PROGRAMACAO/NuBlue/src/main.tsx), que carrega o componente principal [src/app/App.tsx](D:/BRENO/Documentos/_0 PROGRAMACAO/NuBlue/src/app/App.tsx). Não encontrei backend, banco de dados, autenticação nem chamadas HTTP no projeto; o README também deixa essas tecnologias como “a definir”.

As telas e regras principais estão concentradas em `App.tsx`:

- **Aprender:** caminho com níveis sequenciais. Os três níveis jogáveis estão em `LEVELS`; outros dois aparecem como bloqueados.
- **Exercício:** mostra uma grade de equações cruzadas e um banco de números. A pessoa preenche os espaços e confere as respostas.
- **Desafio:** gera cruzadinhas aleatórias, aumenta a dificuldade por rodada e termina após três erros.
- **Ranking:** alterna entre pontuação semanal e geral.
- **Perfil:** mostra XP, nível, sequência de dias, exercícios resolvidos e conquistas.

A interface e as animações já estão implementadas, mas boa parte dos dados e do progresso existe apenas enquanto a página está aberta. Por exemplo, XP, níveis concluídos e recorde ficam em estados React e são reiniciados ao recarregar.

## Como implementar o backend

Uma estrutura simples para este projeto seria manter o Vite como frontend e criar uma API independente, por exemplo com **Node.js/TypeScript, Express e PostgreSQL**. Essa é uma sugestão de arquitetura; nenhuma dessas tecnologias está configurada no repositório hoje.

A API poderia cuidar de:

- Cadastro, login e sessão do usuário.
- Lista de capítulos e níveis disponíveis.
- Progresso, XP, recordes e tentativas.
- Desafios gerados ou selecionados.
- Ranking semanal e geral.
- Conquistas.

Um contrato inicial poderia ser:

|Rota|Uso|
|---|---|
|`POST /auth/register`, `POST /auth/login`|Criar conta e autenticar|
|`GET /me`|Buscar perfil e progresso atual|
|`GET /chapters/:id/levels`|Carregar níveis e status de desbloqueio|
|`GET /levels/:id`|Buscar os dados de um nível|
|`POST /attempts`|Registrar uma resposta ou tentativa|
|`POST /challenges/start`|Iniciar um desafio|
|`POST /challenges/:id/attempts`|Enviar resposta de uma rodada|
|`GET /leaderboard?period=weekly`|Carregar ranking|

O ponto mais importante é que **o servidor deve conferir as respostas e calcular o XP**. Atualmente, o objeto `answers` dos níveis está no código enviado ao navegador e a função `checkAnswers` compara as respostas no próprio frontend. Se o cliente também enviar “ganhei 20 XP”, seria fácil adulterar esse valor. Uma abordagem mais segura é enviar as respostas preenchidas ao servidor e deixar que ele confira o resultado, registre a tentativa e devolva o XP concedido e o progresso atualizado.

O caminho de integração seria:

1. Criar a API e o banco de dados.
2. Definir os modelos e rotas para usuários, níveis, tentativas e progresso.
3. Criar um módulo de acesso à API no frontend, em vez de chamar `fetch` espalhado pelas telas.
4. Carregar perfil, níveis e ranking quando o usuário entra.
5. Enviar tentativas ao backend e atualizar as telas com a resposta da API.
6. Persistir no banco os dados que hoje ficam somente em `useState`.

## Como criar níveis usando os atuais como base

Os níveis estáticos estão no início de `src/app/App.tsx`, no array `LEVELS`. Cada item tem este formato:

- `id`, `title`, `subtitle`, `difficulty`, `xpReward` e `emoji`: identificação e apresentação.
- `grid`: grade de células. Cada célula pode ser número, operador, igualdade ou `null`; as células que o jogador preenche têm um `inputId`.
- `bank`: números oferecidos para preencher os espaços.
- `answers`: mapa dos `inputId`s para seus valores corretos.

Para adicionar um nível manualmente, copie um dos objetos existentes, atribua um `id` novo e mantenha coerentes os `inputId`s em `grid` e `answers`. O número de espaços preenchíveis deve corresponder às respostas esperadas. O `bank` deve incluir os valores corretos e outros números que funcionem como alternativas.

A grade atual usa equações horizontais e verticais. Confira as contas nos dois sentidos antes de incluir um nível; o jogo valida atualmente apenas os espaços contra `answers`, não recalcula a consistência matemática da grade.

O caminho visual é construído em `SkillPathTab` a partir de `LEVELS` e `LOCKED_STUBS`. Os itens bloqueados estão em `LOCKED_STUBS`; hoje, concluir um nível libera o próximo de acordo com a posição no caminho. Para um protótipo, adicionar itens aos arrays é suficiente. Com backend, é melhor guardar capítulos e níveis no banco e retornar também o estado de desbloqueio de cada usuário.

Há também o gerador de desafios, `generatePuzzle(round)`, que cria níveis em tempo de execução com `Math.random()`. Ele serve às rodadas do desafio e é independente dos níveis fixos.

## Onde substituir os dados mockados

|Dado atual|Onde está|O que substituir|
|---|---|---|
|Níveis fixos|`LEVELS`, no início de `src/app/App.tsx`|Carregar níveis da API ou de um arquivo de conteúdo separado|
|Níveis bloqueados|`LOCKED_STUBS`, perto de `LEVELS`|Carregar capítulos/níveis reais e estado de desbloqueio|
|Jogadores fictícios do ranking|`MOCK_PLAYERS`, perto dos dados dos níveis|Buscar ranking na API; `RankingTab` hoje combina esses jogadores com o XP local|
|XP, níveis concluídos e exercícios resolvidos|estados `completedLevels`, `totalXp` e `totalSolved` dentro de `App`|Inicializar com `GET /me` e persistir resultados no backend|
|Sequência de dias|`const [streak] = useState(7)`|Buscar do perfil e atualizar segundo uma regra de atividade diária|
|Recorde do desafio|estado `bestRound` dentro de `App`|Carregar e persistir no perfil|
|Dados do usuário exibidos no perfil|`ProfileTab`, que mostra o nome fixo “Jogador” e um avatar fixo|Usar nome e avatar retornados pelo perfil|
|Rodadas aleatórias|`generatePuzzle`, `getRoundMeta` e `applyOp`|Manter no cliente para protótipo ou mover a geração/seleção para o servidor|

O componente não usa `localStorage` nem `fetch` no estado atual. Isso significa que trocar os mocks por respostas de API exige alterar não só as constantes, mas também o carregamento inicial e o fluxo de conclusão de exercício em `App`.

**Prioridade recomendada:** começar persistindo perfil e progresso; em seguida migrar níveis e ranking; por fim, levar a verificação de respostas e a concessão de XP para o servidor. Assim, a interface pode ser aproveitada enquanto os dados deixam de depender do navegador.

---
---
## Plano recomendado

O [App.tsx](D:/BRENO/Documentos/_0 PROGRAMACAO/NuBlue/src/app/App.tsx) já usa componentes funcionais do React. A refatoração deve organizar melhor esses componentes e separar estado, dados e regras do jogo — sem transformar telas em classes.

### Organização sugerida

```
src/app/
  App.tsx
  types/game.ts
  data/
    levels.ts
    mockPlayers.ts
  features/
    game/
      components/
        Cell.tsx
        PuzzleScreen.tsx
        ResultScreen.tsx
      useGameController.ts
    learning/SkillPathTab.tsx
    challenges/
      ChallengesTab.tsx
      generatePuzzle.ts
    ranking/RankingTab.tsx
    profile/ProfileTab.tsx
    navigation/MainScreen.tsx
```

- **`types/game.ts`**: reunir tipos como `Level`, `GridCell`, `Difficulty` e `CellStatus`.
- **`data/`**: mover os níveis, níveis bloqueados e jogadores de exemplo sem mudar seus formatos.
- **`generatePuzzle.ts`**: separar `generatePuzzle` e suas funções auxiliares.
- **`useGameController.ts`**: reunir estado e ações da partida, como selecionar números, conferir respostas, controlar vidas e avançar rodadas.
- **Componentes das funcionalidades**: receber dados e callbacks por props; cuidar da apresentação de cada tela.
- **`App.tsx`**: conectar o controlador às telas e decidir qual renderizar.

### Ordem de migração

1. Extrair tipos e dados.
2. Mover o gerador de desafios.
3. Separar `Cell`, `PuzzleScreen` e `ResultScreen`.
4. Separar as abas e a navegação.
5. Extrair o estado e os handlers para `useGameController`.
6. Substituir as cores literais repetidas por tokens do tema.

Essa ordem reduz o risco de alterar o comportamento: cada etapa move uma parte do código antes de mudar sua lógica.

## Tema e estilos

O [theme.css](D:/BRENO/Documentos/_0 PROGRAMACAO/NuBlue/src/styles/theme.css) já define tokens e os expõe ao Tailwind. O plano é centralizar nele as cores da marca, tipografia, raios e cores semânticas — como sucesso, aviso e erro — e usar esses tokens nas telas.

As classes Tailwind de layout, espaçamento e animação continuam nos componentes. Assim, `theme.css` concentra o tema geral sem acumular regras específicas de cada tela.

## Opções de organização

| Abordagem                                                              | Vantagem                                                    | Custo                                                                   |
| ---------------------------------------------------------------------- | ----------------------------------------------------------- | ----------------------------------------------------------------------- |
| Pastas planas, como `components/` e `game/`                            | Menos arquivos e imports diretos                            | As responsabilidades podem ficar menos claras conforme o projeto cresce |
| Pastas por funcionalidade, como `features/game/` e `features/ranking/` | Código de cada área fica agrupado e mais fácil de localizar | Exige mover mais arquivos e ajustar imports                             |

**Recomendo a organização por funcionalidade**, mantendo-a simples e sem adicionar camadas extras antes de existir um backend. Nenhum arquivo foi alterado; este é somente o plano solicitado.

---

Plano aplicado. `App.tsx` agora só conecta o controlador às telas. Mantive os componentes como funções React; a mudança organiza o código por funcionalidade em vez de convertê-lo para classes.

- O estado e as ações do jogo foram movidos para [useGameController.ts](D:/BRENO/Documentos/_0 PROGRAMACAO/NuBlue/src/app/features/game/useGameController.ts).
- Tipos, níveis, jogadores de exemplo e gerador de desafios foram separados em módulos próprios, incluindo [game.ts](D:/BRENO/Documentos/_0 PROGRAMACAO/NuBlue/src/app/types/game.ts), [levels.ts](D:/BRENO/Documentos/_0 PROGRAMACAO/NuBlue/src/app/data/levels.ts) e [generatePuzzle.ts](D:/BRENO/Documentos/_0 PROGRAMACAO/NuBlue/src/app/features/challenges/generatePuzzle.ts).
- As telas foram distribuídas em pastas de jogo, aprendizagem, desafios, ranking, perfil e navegação.
- [theme.css](D:/BRENO/Documentos/_0 PROGRAMACAO/NuBlue/src/styles/theme.css) agora centraliza tokens semânticos, superfícies e gradientes usados pela interface; layout e espaçamento continuam nas classes Tailwind dos componentes.

A revisão estática não encontrou problemas funcionais ou de importação. Não executei build nem testes.

---

## Leitura do diagrama

O anexo descreve principalmente um **fluxo de atividades**: tem início, decisões “sim/não” e etapas em sequência. Ele ajuda a extrair requisitos, mas não é ainda um diagrama UML de casos de uso. Para esse diagrama, os atores e seus objetivos ficam fora/dentro da fronteira do sistema; os fluxos de cadastro, login e conclusão de lição podem ficar em diagramas de atividades separados.

Também há IDs de requisitos repetidos no anexo: RF01, RF03, RF05, RF06 e RF10 aparecem associados a mais de uma função. Uma issue deve corrigir essa rastreabilidade e acrescentar casos ausentes, como responsável, gestão de conteúdo e privacidade do ranking.

A implementação atual confirma várias lacunas: o app abre diretamente na área de aprendizagem; não há cadastro nem login; perfil e progresso usam valores locais; o ranking usa jogadores de exemplo; e há apenas três níveis jogáveis. Isso aparece em [App.tsx](D:/BRENO/Documentos/_0 PROGRAMACAO/NuBlue/src/app/App.tsx), [useGameController.ts](D:/BRENO/Documentos/_0 PROGRAMACAO/NuBlue/src/app/features/game/useGameController.ts), [mockPlayers.ts](D:/BRENO/Documentos/_0 PROGRAMACAO/NuBlue/src/app/data/mockPlayers.ts) e [levels.ts](D:/BRENO/Documentos/_0 PROGRAMACAO/NuBlue/src/app/data/levels.ts).

## Backlog revisado

### Fundação e requisitos

1. **Atualizar casos de uso e IDs de requisitos**  
    Separar atores e objetivos dos fluxos de atividade. Incluir **Aluno/Criança**, **Responsável**, **Administrador/Editor de conteúdo** e, se for externo, **provedor de notificações**. Dar IDs únicos e rastrear cada caso de uso até uma issue.  
    **Aceite:** cadastro, recuperação, trilha, lição, progresso, XP, ranking e desafio têm ator, objetivo e fluxo alternativo definidos.
    
2. **Definir conta infantil e privacidade**  
    Decidir como o responsável participa do cadastro, recuperação e configurações; quais dados são necessários; e como funcionam apelido, visibilidade e exclusão do perfil.  
    **Aceite:** os fluxos para criança e responsável estão definidos; nome e foto reais não são públicos por padrão; requisitos de privacidade são revistos antes da publicação.
    
3. **Definir regras do MVP e arquitetura**  
    Especificar tamanho do primeiro curso, regras de conclusão e repetição, XP, streak, ranking e janela do desafio diário. Escolher arquitetura do backend, banco e ambientes.  
    **Aceite:** decisões registradas e usadas pelo modelo de dados e pelos critérios das issues seguintes.
    

### Entrega contínua e plataforma

4. **Implementar Continuous Delivery a partir do CI existente**  
    Já existe [ci.yml](D:/BRENO/Documentos/_0 PROGRAMACAO/NuBlue/.github/workflows/ci.yml): instala dependências e executa o build em pushes e pull requests. Falta a entrega contínua.  
    **Aceite:** mudanças aprovadas são publicadas automaticamente em homologação; produção tem etapa de promoção definida; segredos ficam fora do frontend; deploy inclui verificação de saúde e procedimento de rollback. A pipeline deve acomodar migrations do banco quando o backend entrar.
    
5. **Criar backend, banco, migrations e contratos da API**  
    Modelar contas, cursos, unidades, lições, tentativas e progresso. Definir contratos para autenticação, catálogo, tentativas e estatísticas.  
    **Aceite:** migrations podem ser executadas em ambiente limpo; dados iniciais incluem os níveis atuais; chaves de resposta não são expostas nos endpoints públicos.
    
6. **Implementar cadastro, login, recuperação e sessão**  
    Incluir os fluxos de criança e responsável definidos na issue 2 e proteger as rotas da aplicação.  
    **Aceite:** usuário consegue criar ou acessar a conta, recuperar o acesso e manter a sessão; dados da conta são validados e associados corretamente.
    
7. **Criar site público e navegação para a aplicação**  
    Construir landing page com apresentação do Nublue e caminhos para cadastro e login.  
    **Aceite:** rotas públicas e autenticadas funcionam; usuário não autenticado não acessa o painel; o fluxo funciona em dispositivos móveis.
    

### Aprendizagem, progresso e gamificação

8. **Modelar cursos e validar conteúdo**  
    Suportar a hierarquia **curso → unidade → lição → atividade**, com ordem, pré-requisitos, objetivo e versão de conteúdo. Criar validação para grades, campos de resposta e recompensas.  
    **Aceite:** níveis novos podem ser adicionados como conteúdo sem alterar JSX; conteúdo inválido é rejeitado com erro compreensível.
    
9. **Produzir o primeiro curso completo**  
    Organizar os níveis existentes e criar novas lições com instruções, dificuldade gradual e feedback pedagógico. Definir a quantidade do curso piloto na issue 3.  
    **Aceite:** curso, unidades e lições seguem a matriz aprovada; questões e respostas passam por revisão pedagógica e matemática.
    
10. **Construir trilha dinâmica e desbloqueios**  
    Substituir capítulo fixo, stubs e desbloqueio pela posição por dados do catálogo e progresso do aluno.  
    **Aceite:** a tela apresenta lições bloqueadas, disponíveis, iniciadas e concluídas conforme as regras definidas e retoma o progresso após novo login.
    
11. **Registrar tentativas e calcular progresso/XP no servidor**  
    Migrar a conferência de respostas e a concessão de XP do navegador para a API. Registrar tentativas e atualizar progresso de forma consistente.  
    **Aceite:** servidor valida as respostas; tentativas duplicadas não concedem XP repetidamente; progresso continua correto entre sessões e dispositivos.
    
12. **Tornar perfil, nível e sequência válidos**  
    Carregar do backend nome/apelido, avatar, XP, lições concluídas, nível do usuário e sequência diária.  
    **Aceite:** os dados persistem e são derivados das atividades registradas, sem valores fixos como o streak atual.
    
13. **Implementar ranking real e seguro para crianças**  
    Substituir a lista mockada por posições calculadas a partir de XP registrado. Definir períodos, desempates e participação.  
    **Aceite:** ranking semanal e geral seguem regras documentadas; usa apelido; participação e visibilidade obedecem às decisões de privacidade.
    
14. **Implementar ciclo do desafio diário e notificações**  
    Definir geração, validade de 24 horas, fuso horário, recordes e atualização da sequência. A imagem também propõe lembretes e avisos de risco de streak.  
    **Aceite:** desafio diário é único e tem expiração previsível; notificações são opcionais, configuráveis pelo responsável e usam linguagem neutra, sem pressionar a criança.
    
15. **Criar gestão de cursos e níveis**  
    Definir quem pode criar, revisar e publicar conteúdo e como essa publicação chega ao catálogo.  
    **Aceite:** um administrador/editor consegue publicar conteúdo validado sem alterar o código da interface.
    
16. **Adicionar qualidade e operação para lançamento**  
    Planejar testes automatizados, acessibilidade, logs, backups, configuração de produção e recuperação de falhas. O `package.json` hoje só oferece scripts de desenvolvimento e build.  
    **Aceite:** fluxos principais têm verificações automatizadas na pipeline; interface é utilizável por teclado e leitor de tela; operação e recuperação estão documentadas.
    

## Dependências principais

**Regras do MVP e privacidade → arquitetura e banco → autenticação → catálogo e conteúdo → validação de lições e progresso → perfil e ranking.** A Continuous Delivery deve começar junto da fundação da API para validar e publicar cada etapa; não é necessário esperar o produto inteiro ficar pronto.

O backlog está atualizado com o significado de **CD = Continuous Delivery**. A pipeline de CI existente foi considerada para evitar abrir uma issue duplicada. Não alterei arquivos.