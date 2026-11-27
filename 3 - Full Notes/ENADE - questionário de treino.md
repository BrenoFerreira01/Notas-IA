
data: 27.11.2026
horário: 17:44
Tags: [[Estudo]], [[Programação]]

---

# ENADE - questionário de treino

Simulado no estilo do ENADE, montado a partir da pesquisa em [[ENADE - assuntos mais cobrados]]. São **23 questões objetivas** (5 de Formação Geral e 18 de Componente Específico de Computação) e **3 discursivas**. O gabarito comentado está no fim, recolhido.

Como usar: marque a alternativa trocando `[ ]` por `[x]`. Tente fazer em até 2h30 sem consultar nada.

---

## Parte I: Formação Geral

### Questão 1

> A circulação de notícias falsas em redes sociais ganhou escala nos últimos anos. Pesquisas indicam que conteúdos sensacionalistas recebem mais interações do que notícias checadas, o que os favorece nos sistemas de recomendação das plataformas. Esse cenário levanta debates sobre os limites entre moderação de conteúdo e liberdade de expressão.

Considerando o texto, avalie as afirmações a seguir.

I. Algoritmos de recomendação que priorizam o engajamento podem ampliar o alcance de conteúdos sensacionalistas, inclusive falsos.
II. A educação midiática ajuda os cidadãos a verificar fontes antes de compartilhar informações.
III. A remoção de todo conteúdo considerado controverso, sem critérios transparentes, é a medida que melhor preserva a liberdade de expressão.

É correto o que se afirma em

[ ] a) I, apenas.
[ ] b) III, apenas.
[ ] c) I e II, apenas.
[ ] d) II e III, apenas.
[ ] e) I, II e III.

### Questão 2

> O Brasil está entre os maiores geradores de lixo eletrônico do mundo, e apenas uma pequena parte desse material passa por reciclagem formal.

Considerando o tema, avalie as asserções a seguir e a relação proposta entre elas.

I. O descarte inadequado de equipamentos eletrônicos representa risco ao meio ambiente e à saúde humana

PORQUE

II. esses equipamentos contêm metais pesados, como chumbo, cádmio e mercúrio, que podem contaminar o solo e a água.

A respeito dessas asserções, assinale a opção correta.

[ ] a) As asserções I e II são proposições verdadeiras, e a II é uma justificativa correta da I.
[ ] b) As asserções I e II são proposições verdadeiras, mas a II não é uma justificativa correta da I.
[ ] c) A asserção I é uma proposição verdadeira, e a II é uma proposição falsa.
[ ] d) A asserção I é uma proposição falsa, e a II é uma proposição verdadeira.
[ ] e) As asserções I e II são proposições falsas.

### Questão 3

> A tabela abaixo apresenta o percentual de domicílios com acesso à internet em três regiões (dados hipotéticos).

| Região | 2019 | 2023 |
|---|---|---|
| A | 70% | 84% |
| B | 55% | 77% |
| C | 80% | 88% |

Com base na tabela, conclui-se que

[ ] a) a região C teve o maior crescimento em pontos percentuais no período.
[ ] b) a região B teve crescimento relativo de 40% no período.
[ ] c) a diferença entre a região com maior e a com menor acesso aumentou de 2019 para 2023.
[ ] d) a região A dobrou seu percentual de domicílios conectados.
[ ] e) as três regiões cresceram a mesma quantidade de pontos percentuais.

### Questão 4

> O trabalho mediado por plataformas digitais, como os aplicativos de transporte e entrega, cresceu rapidamente no Brasil. Esse modelo é chamado por alguns pesquisadores de "uberização do trabalho".

A respeito desse modelo de trabalho, avalie as afirmações a seguir.

I. A flexibilidade de horários costuma ser apontada pelos próprios trabalhadores como uma vantagem.
II. A ausência de vínculo empregatício formal pode reduzir o acesso a direitos como férias remuneradas e aposentadoria.
III. A renda desses trabalhadores é garantida por lei, independentemente do número de horas trabalhadas.

É correto o que se afirma em

[ ] a) I, apenas.
[ ] b) II, apenas.
[ ] c) III, apenas.
[ ] d) I e II, apenas.
[ ] e) I, II e III.

### Questão 5

> Uma prefeitura vai reformular seu portal de serviços para que pessoas com deficiência consigam usá-lo sem ajuda de terceiros, conforme a Lei Brasileira de Inclusão.

Uma prática adequada de acessibilidade digital para esse portal é

[ ] a) transmitir informações importantes apenas pela cor dos elementos.
[ ] b) desabilitar a navegação por teclado para evitar cliques acidentais.
[ ] c) inserir textos dentro de imagens, sem versão textual, para preservar o layout.
[ ] d) usar um CAPTCHA visual como única forma de verificação.
[ ] e) fornecer texto alternativo descritivo para imagens e legendas para vídeos.

---

## Parte II: Componente Específico (Computação)

### Questão 6

Considere o algoritmo a seguir, em pseudocódigo.

```
para i de 1 até n faça
    j ← 1
    enquanto j < n faça
        j ← j * 2
    fim enquanto
fim para
```

A complexidade de tempo desse algoritmo, em notação O, é

[ ] a) O(log n).
[ ] b) O(n).
[ ] c) O(n log n).
[ ] d) O(n²).
[ ] e) O(2ⁿ).

### Questão 7

Uma pilha inicialmente vazia recebe a seguinte sequência de operações:

`empilha(1), empilha(2), desempilha(), empilha(3), empilha(4), desempilha(), desempilha(), empilha(5)`

A sequência de valores desempilhados e o elemento no topo da pilha ao final são, respectivamente,

[ ] a) 1, 3, 4 e topo 5.
[ ] b) 2, 4, 3 e topo 5.
[ ] c) 2, 3, 4 e topo 1.
[ ] d) 4, 3, 2 e topo 5.
[ ] e) 2, 4, 3 e topo 1.

### Questão 8

As chaves `50, 30, 70, 20, 40, 60, 80, 35` são inseridas, nessa ordem, em uma árvore binária de busca inicialmente vazia, sem balanceamento.

O percurso em **pós-ordem** dessa árvore é

[ ] a) 20, 30, 35, 40, 50, 60, 70, 80.
[ ] b) 50, 30, 20, 40, 35, 70, 60, 80.
[ ] c) 20, 35, 40, 30, 60, 80, 70, 50.
[ ] d) 50, 30, 70, 20, 40, 60, 80, 35.
[ ] e) 35, 40, 20, 30, 80, 60, 70, 50.

### Questão 9

> Uma empresa de logística modela sua malha de entregas como um grafo direcionado e ponderado, em que os vértices são centros de distribuição e os pesos são custos de transporte. Alguns trechos têm custo negativo, porque geram crédito fiscal.

Avalie as afirmações a seguir.

I. O algoritmo de Dijkstra garante caminhos mínimos corretos nesse grafo, mesmo com arestas de peso negativo.
II. O algoritmo de Bellman-Ford consegue detectar ciclos de peso negativo alcançáveis a partir do vértice de origem.
III. Em um grafo não ponderado, a busca em largura (BFS) encontra o caminho com o menor número de arestas entre a origem e os demais vértices.

É correto o que se afirma em

[ ] a) I, apenas.
[ ] b) II, apenas.
[ ] c) I e III, apenas.
[ ] d) II e III, apenas.
[ ] e) I, II e III.

### Questão 10

Avalie as asserções a seguir e a relação proposta entre elas.

I. O Merge Sort tem complexidade de tempo O(n log n) no pior caso

PORQUE

II. o Merge Sort é um algoritmo de ordenação *in-place*, que não precisa de memória auxiliar proporcional ao tamanho da entrada.

A respeito dessas asserções, assinale a opção correta.

[ ] a) As asserções I e II são proposições verdadeiras, e a II é uma justificativa correta da I.
[ ] b) As asserções I e II são proposições verdadeiras, mas a II não é uma justificativa correta da I.
[ ] c) A asserção I é uma proposição verdadeira, e a II é uma proposição falsa.
[ ] d) A asserção I é uma proposição falsa, e a II é uma proposição verdadeira.
[ ] e) As asserções I e II são proposições falsas.

### Questão 11

Considere as tabelas a seguir.

**Aluno**

| id | nome | curso |
|---|---|---|
| 1 | Ana | CC |
| 2 | Bruno | SI |
| 3 | Carla | CC |
| 4 | Diego | ADS |

**Matricula**

| aluno_id | disciplina | nota |
|---|---|---|
| 1 | BD | 8.0 |
| 1 | Redes | 6.5 |
| 2 | BD | 7.0 |
| 3 | BD | 9.0 |
| 3 | Redes | 5.0 |
| 3 | IA | 7.5 |

```sql
SELECT a.nome, AVG(m.nota) AS media
FROM Aluno a
JOIN Matricula m ON a.id = m.aluno_id
WHERE a.curso = 'CC'
GROUP BY a.nome
HAVING COUNT(*) > 2;
```

O resultado da consulta é

[ ] a) duas linhas: Ana (7.25) e Carla (7.17).
[ ] b) nenhuma linha.
[ ] c) três linhas: Ana (7.25), Bruno (7.0) e Carla (7.17).
[ ] d) duas linhas: Carla (7.17) e Diego (sem média).
[ ] e) uma linha: Carla (7.17).

### Questão 12

Considere a relação `Pedido(num_pedido, cod_produto, data, cod_cliente, nome_cliente, qtd)`, cuja chave primária é `(num_pedido, cod_produto)`, com as dependências funcionais:

- `num_pedido → data, cod_cliente`
- `cod_cliente → nome_cliente`
- `(num_pedido, cod_produto) → qtd`

Sobre essa relação, é correto afirmar que

[ ] a) ela viola a 2FN, pois há atributos que dependem apenas de parte da chave primária.
[ ] b) ela está na 3FN, pois todos os atributos dependem da chave primária.
[ ] c) ela viola a 1FN, porque tem chave primária composta.
[ ] d) para chegar à 3FN basta remover o atributo `qtd`.
[ ] e) o atributo `nome_cliente` depende diretamente de `cod_produto`.

### Questão 13

> Em um sistema bancário, duas transações acessam a mesma conta, cujo saldo é R$ 100,00:
> 1. T1 lê o saldo (100).
> 2. T2 lê o saldo (100).
> 3. T1 grava 100 − 30 = 70 e confirma (commit).
> 4. T2 grava 100 + 50 = 150 e confirma (commit).
>
> O saldo final esperado era R$ 120,00, mas ficou R$ 150,00.

A anomalia descrita e a propriedade ACID violada são, respectivamente,

[ ] a) leitura fantasma e atomicidade.
[ ] b) leitura suja e consistência.
[ ] c) deadlock e durabilidade.
[ ] d) atualização perdida e isolamento.
[ ] e) violação de integridade referencial e consistência.

### Questão 14

> Uma startup adotou o Scrum para desenvolver um aplicativo de agendamento. No meio da Sprint, a equipe descobriu que uma funcionalidade era mais complexa do que o previsto.

Com base no Guia do Scrum, avalie as afirmações a seguir.

I. O Product Owner é responsável por maximizar o valor do produto e por gerenciar o Product Backlog.
II. A Daily Scrum é um evento de até 15 minutos para os desenvolvedores inspecionarem o progresso em direção à meta da Sprint.
III. O Scrum Master define as tarefas de cada desenvolvedor e cobra prazos individualmente.
IV. O escopo da Sprint pode ser esclarecido e renegociado com o Product Owner, desde que a meta da Sprint não seja comprometida.

É correto o que se afirma em

[ ] a) I e II, apenas.
[ ] b) I e III, apenas.
[ ] c) II e IV, apenas.
[ ] d) II, III e IV, apenas.
[ ] e) I, II e IV, apenas.

### Questão 15

> Um e-commerce calcula o frete de formas diferentes (PAC, Sedex, retirada na loja), e novas formas são adicionadas com frequência. Hoje o cálculo é feito com uma longa cadeia de `if/else`. A equipe quer trocar o algoritmo de cálculo em tempo de execução e adicionar novas formas sem alterar o código existente.

O padrão de projeto mais adequado para essa situação é o

[ ] a) Strategy.
[ ] b) Singleton.
[ ] c) Observer.
[ ] d) Adapter.
[ ] e) Composite.

### Questão 16

Um campo de cadastro aceita idades inteiras de **18 a 60 anos, inclusive**. Um testador vai aplicar a técnica de **análise de valor-limite**.

O conjunto de valores de teste mais adequado a essa técnica é

[ ] a) 0, 30, 100.
[ ] b) 17, 18, 60, 61.
[ ] c) 18, 39, 60.
[ ] d) 19, 59.
[ ] e) −1, 0, 1.

### Questão 17

Avalie as asserções a seguir e a relação proposta entre elas.

I. Aplicações de voz e vídeo em tempo real costumam usar o protocolo UDP na camada de transporte

PORQUE

II. o UDP não faz retransmissão de segmentos nem controle de congestionamento, o que reduz o atraso, e essas aplicações toleram perdas ocasionais de pacotes.

A respeito dessas asserções, assinale a opção correta.

[ ] a) As asserções I e II são proposições verdadeiras, e a II é uma justificativa correta da I.
[ ] b) As asserções I e II são proposições verdadeiras, mas a II não é uma justificativa correta da I.
[ ] c) A asserção I é uma proposição verdadeira, e a II é uma proposição falsa.
[ ] d) A asserção I é uma proposição falsa, e a II é uma proposição verdadeira.
[ ] e) As asserções I e II são proposições falsas.

### Questão 18

Um administrador de redes recebeu o bloco `192.168.10.0/26` para um laboratório.

O número de endereços utilizáveis por hosts e a máscara de sub-rede correspondente são, respectivamente,

[ ] a) 64 hosts e 255.255.255.192.
[ ] b) 62 hosts e 255.255.255.224.
[ ] c) 30 hosts e 255.255.255.224.
[ ] d) 126 hosts e 255.255.255.128.
[ ] e) 62 hosts e 255.255.255.192.

### Questão 19

Sobre impasses (*deadlocks*) em sistemas operacionais, avalie as afirmações a seguir.

I. Para ocorrer um deadlock, as condições de exclusão mútua, posse e espera, não preempção e espera circular precisam valer ao mesmo tempo.
II. Impor uma ordem global para a aquisição de recursos impede a espera circular.
III. O algoritmo do banqueiro é uma técnica de detecção, aplicada somente depois que o deadlock acontece.

É correto o que se afirma em

[ ] a) I, apenas.
[ ] b) III, apenas.
[ ] c) I e II, apenas.
[ ] d) II e III, apenas.
[ ] e) I, II e III.

### Questão 20

Considere a proposição: "Se o sistema está online, então o backup foi executado."

A negação lógica dessa proposição é

[ ] a) Se o sistema não está online, então o backup não foi executado.
[ ] b) O sistema está online e o backup não foi executado.
[ ] c) O sistema não está online ou o backup foi executado.
[ ] d) Se o backup não foi executado, então o sistema não está online.
[ ] e) O sistema não está online e o backup foi executado.

### Questão 21

Considere a linguagem L = { aⁿbⁿ | n ≥ 0 } sobre o alfabeto {a, b}. Avalie as afirmações a seguir.

I. L não é regular, e isso pode ser demonstrado pelo lema do bombeamento para linguagens regulares.
II. Toda linguagem reconhecida por um autômato finito não determinístico (AFN) também é reconhecida por algum autômato finito determinístico (AFD).
III. O problema da parada é decidível, embora qualquer algoritmo que o resolva tenha complexidade exponencial.
IV. L pode ser gerada por uma gramática livre de contexto.

É correto o que se afirma em

[ ] a) I e II, apenas.
[ ] b) I e III, apenas.
[ ] c) III e IV, apenas.
[ ] d) I, II e IV, apenas.
[ ] e) II, III e IV, apenas.

### Questão 22

> Uma equipe treinou um modelo de classificação para detectar fraudes em cartões. O modelo atingiu 99% de acurácia no conjunto de treino e 62% no conjunto de validação.

Sobre esse resultado, é correto afirmar que

[ ] a) o modelo sofre de *underfitting*, e a solução é aumentar indefinidamente a taxa de aprendizado.
[ ] b) o modelo generaliza bem, pois a acurácia de treino é alta.
[ ] c) a diferença prova que houve vazamento de dados no conjunto de validação, que deve ser descartado.
[ ] d) o modelo sofre de *overfitting*, e regularização, mais dados de treino ou um modelo menos complexo podem ajudar.
[ ] e) deveria ser usado aprendizado não supervisionado, pois os dados têm rótulos.

### Questão 23

> Alice precisa enviar um contrato a Bob de modo que Bob tenha certeza de que foi Alice quem o enviou e de que o conteúdo não foi alterado no caminho.

O procedimento correto de assinatura digital, nesse caso, é

[ ] a) Alice gera o hash do contrato e o cifra com a sua chave privada; Bob verifica a assinatura com a chave pública de Alice, o que garante autenticidade, integridade e não repúdio.
[ ] b) Alice cifra o contrato com a chave pública de Bob, o que garante o não repúdio.
[ ] c) Alice e Bob usam uma chave simétrica compartilhada, o que garante o não repúdio.
[ ] d) Bob verifica a assinatura com a chave privada de Alice.
[ ] e) A assinatura digital garante a confidencialidade do contrato e dispensa qualquer cifragem.

---

## Parte III: Questões discursivas

### Discursiva 1 (Formação Geral)

> Ferramentas de inteligência artificial generativa já escrevem textos, geram imagens e programam. Para alguns, elas aumentam a produtividade; para outros, ameaçam empregos e ampliam desigualdades.

Redija um texto dissertativo sobre o impacto da inteligência artificial no mundo do trabalho, abordando:

a) um impacto positivo da IA para trabalhadores ou empresas;
b) um risco social causado pela adoção da IA;
c) uma proposta de política pública para reduzir esse risco.

*(Até 15 linhas)*

### Discursiva 2 (Componente Específico)

> Um sistema recebe diariamente um vetor com até 1 milhão de códigos de transação e precisa informar se há algum código repetido. A versão atual compara cada elemento com todos os outros.

a) Qual é a complexidade de tempo da solução atual? Justifique.
b) Escreva, em pseudocódigo, uma solução mais eficiente.
c) Informe a complexidade de tempo e de espaço da sua solução e explique o compromisso (*trade-off*) entre elas.

### Discursiva 3 (Componente Específico)

> Uma clínica vai lançar um aplicativo para agendar consultas, que armazenará nome, CPF, telefone e histórico de atendimentos dos pacientes.

Considerando a Lei Geral de Proteção de Dados (LGPD) e boas práticas de segurança:

a) explique por que parte desses dados é classificada como **dado pessoal sensível**;
b) proponha **três medidas técnicas** de segurança para o sistema, justificando cada uma;
c) cite **um direito do titular** previsto na LGPD que o sistema precisa atender e como implementá-lo.

---

## Gabarito comentado

> [!success]- Gabarito objetivas (clique para abrir)
> | Questão | Resposta | Comentário |
> |---|---|---|
> | 1 | **c** | I e II estão corretas. III é falsa: remoção sem critério transparente vira censura e não preserva a liberdade de expressão. |
> | 2 | **a** | As duas são verdadeiras, e a presença de metais pesados é justamente o motivo do risco. |
> | 3 | **b** | B foi de 55% para 77%: 22/55 = 0,40, ou seja, 40%. Em pontos percentuais: A +14, B +22, C +8. A diferença entre maior e menor caiu de 25 para 11 pontos. |
> | 4 | **d** | III é falsa: não há renda mínima garantida por lei independente das horas trabalhadas. |
> | 5 | **e** | Texto alternativo e legendas fazem parte das diretrizes WCAG. As demais alternativas criam barreiras. |
> | 6 | **c** | O laço externo roda n vezes e o interno dobra j até n, ou seja, log n vezes. Total: O(n log n). |
> | 7 | **b** | Pilha: [1,2] → sai 2 → [1,3,4] → sai 4, sai 3 → [1,5]. Topo = 5. |
> | 8 | **c** | Árvore: 50 (30 (20, 40 (35, –)), 70 (60, 80)). Pós-ordem: esquerda, direita, raiz. A alternativa a é a em-ordem, a b é a pré-ordem e a d é o percurso por nível. |
> | 9 | **d** | Dijkstra não funciona com pesos negativos. Bellman-Ford detecta ciclos negativos. BFS dá o menor número de arestas. |
> | 10 | **c** | I é verdadeira. II é falsa: o Merge Sort clássico usa O(n) de memória auxiliar. |
> | 11 | **e** | O filtro CC deixa Ana (2 notas) e Carla (3 notas). O HAVING COUNT(*) > 2 elimina Ana. Média de Carla: (9 + 5 + 7,5)/3 ≈ 7,17. |
> | 12 | **a** | `data` e `cod_cliente` dependem só de `num_pedido`, que é parte da chave: dependência parcial, viola a 2FN. Também há dependência transitiva (`nome_cliente`). |
> | 13 | **d** | A escrita de T2 sobrescreveu a de T1: atualização perdida (*lost update*), falha de isolamento. |
> | 14 | **e** | III é falsa: no Scrum, os desenvolvedores se auto-organizam, e o Scrum Master não distribui tarefas. |
> | 15 | **a** | O Strategy encapsula algoritmos intercambiáveis atrás de uma mesma interface (princípio aberto/fechado). |
> | 16 | **b** | Valor-limite testa as bordas e seus vizinhos imediatos: 17 (inválido), 18, 60 (válidos) e 61 (inválido). |
> | 17 | **a** | As duas são verdadeiras, e a II explica a escolha do UDP. |
> | 18 | **e** | /26 deixa 6 bits para hosts: 2⁶ − 2 = 62. Máscara: 255.255.255.192. |
> | 19 | **c** | III é falsa: o algoritmo do banqueiro **evita** deadlocks antes que ocorram; não é técnica de detecção. |
> | 20 | **b** | ¬(p → q) ≡ p ∧ ¬q. |
> | 21 | **d** | III é falsa: o problema da parada é **indecidível** (Turing). L é gerada por S → aSb \| ε. |
> | 22 | **d** | Uma distância grande entre treino e validação é o sinal típico de *overfitting*. |
> | 23 | **a** | A assinatura usa a chave privada do remetente, e a verificação usa a chave pública dele. Assinatura não dá confidencialidade. |

> [!example]- Padrão de resposta das discursivas (clique para abrir)
> **Discursiva 1.** (a) Automação de tarefas repetitivas, aumento de produtividade, apoio à tomada de decisão, novas profissões. (b) Desemprego em funções automatizáveis, concentração de renda, viés discriminatório nos algoritmos, precarização. (c) Requalificação profissional gratuita, regulação da IA com transparência e auditoria, proteção social para quem perder o emprego na transição. O texto precisa ter os três itens, coesão e norma-padrão.
>
> **Discursiva 2.** (a) O(n²), porque cada um dos n elementos é comparado com até n − 1 outros (cerca de n(n − 1)/2 comparações). (b) Exemplo com tabela hash:
> ```
> funcao temRepetido(v):
>     vistos ← conjunto vazio
>     para cada x em v faça
>         se x ∈ vistos então retorne VERDADEIRO
>         adicione x em vistos
>     retorne FALSO
> ```
> (c) Tempo O(n) no caso médio, espaço O(n). Gasta-se memória para ganhar tempo. Alternativa aceita: ordenar e comparar vizinhos, com tempo O(n log n) e espaço O(1) ou O(n), conforme o algoritmo de ordenação.
>
> **Discursiva 3.** (a) Dados de saúde (o histórico de atendimentos) são **dados pessoais sensíveis** pelo art. 5º, II da LGPD, porque seu vazamento pode levar a discriminação. Nome, CPF e telefone são dados pessoais comuns. (b) Exemplos: criptografia em trânsito (TLS) e em repouso; controle de acesso por perfil (RBAC) com autenticação multifator; registro de auditoria (logs) de quem acessou o quê; minimização e pseudonimização dos dados; backups protegidos; testes contra SQL injection e XSS. Cada medida precisa de uma justificativa. (c) Exemplos: acesso aos dados (tela ou exportação dos dados do paciente), correção, eliminação (exclusão mediante solicitação, respeitando a guarda obrigatória de prontuário), portabilidade e revogação do consentimento.
