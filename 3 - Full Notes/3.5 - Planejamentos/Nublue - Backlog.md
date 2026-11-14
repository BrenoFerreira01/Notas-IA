## Diagnóstico

O projeto já tem uma interface de jogo funcional e está separado por funcionalidades. Ainda é um protótipo: não encontrei API, banco de dados, autenticação nem persistência. O progresso é mantido em estado local e a sequência de dias começa fixada em 7 em [useGameController.ts](D:/BRENO/Documentos/_0 PROGRAMACAO/NuBlue/src/app/features/game/useGameController.ts). O perfil usa dados fixos, e o ranking ainda inclui jogadores de exemplo de [mockPlayers.ts](D:/BRENO/Documentos/_0 PROGRAMACAO/NuBlue/src/app/data/mockPlayers.ts).

A trilha também não representa cursos ainda: há três níveis jogáveis e dois bloqueios de exemplo em [levels.ts](D:/BRENO/Documentos/_0 PROGRAMACAO/NuBlue/src/app/data/levels.ts), enquanto [SkillPathTab.tsx](D:/BRENO/Documentos/_0 PROGRAMACAO/NuBlue/src/app/features/learning/SkillPathTab.tsx) mostra um capítulo fixo e libera níveis pela posição na lista. As respostas são verificadas e o XP é concedido no navegador. Não há landing page, cadastro, login ou rotas protegidas; o app abre diretamente na interface de aprendizagem.

## Issues recomendadas para o MVP

### Fundação

1. **Definir escopo e regras do MVP**  
    Decidir público e tipos de conta, tamanho do primeiro curso, regras de XP e repetição de lições, sequência diária, períodos do ranking e critérios para considerar o MVP concluído. Essas decisões orientam o modelo de dados e evitam implementar regras diferentes em cada tela.
    
2. **Registrar decisões de arquitetura**  
    Definir backend, banco, autenticação, ambientes de desenvolvimento e produção, hospedagem e configuração de segredos. O README ainda marca backend e banco como “a definir”. Não é necessário escolher um fornecedor antes de comparar as opções.
    
3. **Criar API, banco, migrações e dados iniciais**  
    Criar a estrutura inicial para usuários, cursos, unidades, lições, atividades, tentativas e progresso. Incluir migrações reproduzíveis e dados iniciais baseados nos três níveis atuais.
    

### Site e contas (Feito)

4. **Criar landing page pública e navegação**  
    Apresentar a plataforma e oferecer caminhos claros para cadastro e login. Configurar rotas para a área pública e a área da aplicação; atualmente a entrada carrega diretamente o app, embora `react-router` já esteja entre as dependências.
    
5. **Implementar cadastro, login, logout e sessão**  
    Validar formulários, proteger rotas autenticadas e definir recuperação de senha. O aceite deve incluir persistência da sessão ao recarregar a página e encerramento correto do acesso no logout.
    
6. **Criar e carregar o perfil real do aluno**  
    Substituir nome, avatar e estatísticas fixos por dados associados à conta. O perfil deve continuar correto depois de logout e login em outro dispositivo.
    

### Cursos e conteúdo

7. **Modelar a estrutura de aprendizagem**  
    Definir curso → unidade → lição → atividade, incluindo ordem, objetivos, pré-requisitos, recompensas e estados de conclusão. O modelo deve permitir adicionar cursos sem editar componentes React.
    
8. **Validar e migrar o conteúdo atual**  
    Definir formato e validação para grades, espaços de resposta, gabaritos e recompensas; migrar os níveis existentes para esse formato. O gabarito deve ficar disponível para validação no servidor, sem ser enviado junto com os dados públicos da atividade.
    
9. **Construir catálogo e trilha dinâmica**  
    Substituir capítulo fixo e bloqueios de exemplo por cursos e estados vindos do catálogo e do progresso do aluno. A tela deve representar conteúdo bloqueado, disponível, iniciado e concluído de acordo com regras explícitas.
    
10. **Produzir o primeiro curso completo**  
    Criar uma sequência de lições com instruções, exercícios e feedback, cobrindo a progressão pedagógica definida na issue de escopo. A quantidade de novos níveis deve ser definida nessa issue, em vez de deixar o objetivo aberto como “criar mais níveis”.
    

### Progresso e gamificação

11. **Validar tentativas e conceder XP no servidor**  
    O cliente envia as respostas; o servidor valida, registra a tentativa e calcula a recompensa. O sistema precisa evitar que respostas ou XP enviados pelo navegador sejam tratados como confiáveis e impedir crédito duplicado por conclusão repetida.
    
12. **Persistir progresso, estatísticas e sequência diária**  
    Registrar lições concluídas, tentativas, XP, atividades resolvidas e melhor resultado de desafio. Definir a regra de dia e fuso horário da sequência; substituir os valores locais e fixos existentes.
    
13. **Implementar ranking semanal e geral com dados reais**  
    Calcular as posições a partir do XP registrado no servidor, com período e desempates definidos. Remover os jogadores fictícios e garantir que os valores exibidos correspondam às regras escolhidas para o ranking.
    
14. **Definir como o desafio participa da conta do aluno**  
    Decidir se as rodadas geradas no cliente continuam sendo uma atividade sem pontuação competitiva ou se serão registradas e validadas pelo servidor. Se concederem XP ou recordes, salvar sessão e resultado no backend.
    

### Qualidade e publicação

15. **Tratar estados de carregamento, erros e sessão expirada**  
    Incluir respostas visuais para carregamento, falta de conexão, catálogo vazio, falha ao enviar tentativa e sessão expirada. Essas situações passam a existir quando as telas dependem da API.
    
16. **Preparar acessibilidade, privacidade e publicação**  
    Revisar navegação por teclado e leitor de tela, testar telas pequenas, planejar deploy de frontend/API/banco, logs e backups. Como o público inclui estudantes do ensino fundamental, incluir uma revisão apropriada de privacidade e dos dados coletados antes do lançamento.
    

## Ordem sugerida

A sequência principal é: **escopo → arquitetura → banco/API → autenticação → modelo e conteúdo de cursos → tentativas validadas → progresso → perfil e ranking → publicação**. A landing page pode avançar em paralelo com backend e banco.

Não alterei arquivos; este é o backlog derivado da estrutura atual do repositório.