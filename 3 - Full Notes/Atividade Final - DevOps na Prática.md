
data: 12.11.2026
horário: 16:18
Tags: [[DevOps]]

---

# Atividade Final - DevOps na Prática

> Conteúdo convertido da apresentação **DEVOPS TOOLS**. A avaliação aparece nos slides 3-9; os slides seguintes trazem material de apoio sobre logs, APM e Elastic Stack.

## Dados da disciplina

- **Professor:** Rodrigo Silva Marques
- **E-mail:** rmarques26@gmail.com
- **Lattes:** http://lattes.cnpq.br/3349784222975236
- **LinkedIn:** Rodrigo Marques
- **Instagram:** @rodrigo.s.marques
- **Contato:** (91) 98747-1977
- **Cursos:** Ciência da Computação / Análise e Desenvolvimento de Sistemas
- **Carga horária total:** 60 h

## Atividade avaliativa - DevOps na Prática

### Objetivos

- Consolidar os conceitos de DevOps, suas práticas e sua relação com metodologias ágeis.
- Implementar um pipeline de CI/CD com GitHub Actions ou Jenkins.
- Trabalhar com containers, registries e orquestração com Kubernetes e Helm.
- Implementar estratégias de monitoramento, logging e métricas.

### Descrição e avaliação

Construir em grupo um projeto prático de DevOps que integre os conceitos, as ferramentas e as práticas estudadas. O trabalho será avaliado pelo funcionamento prático e pela explicação técnica. Os grupos podem ter até 5 alunos. Criatividade, como uma aplicação própria, dashboards ou automações extras, pode render até 0,5 ponto extra.

| Etapa                                     |   Pontos |
| ----------------------------------------- | -------: |
| 1. Conceitos fundamentais de DevOps       |      1,5 |
| 2. Integração contínua e entrega contínua |      2,0 |
| 3. Continuous Deployment e orquestração   |      2,5 |
| 4. Logging e métricas                     |      2,0 |
| 5. Relatório final e apresentação         |      2,0 |
| **Total**                                 | **10,0** |

### Etapa 1 - Conceitos fundamentais de DevOps (1,5 pontos)

Escrever um relatório que explique:

- O que é DevOps e sua relação com metodologias ágeis.
- A diferença entre CI, CD (Delivery) e CD (Deployment).
- Os conceitos de máquinas virtuais e containers, incluindo as vantagens dos containers.
- Incluir exemplos práticos ou analogias.

### Etapa 2 - Integração contínua e entrega contínua (2 pontos)

- Criar no GitHub um repositório com uma aplicação simples, que pode ser feita em Python, Node.js ou Java.
- Configurar um pipeline de CI com GitHub Actions ou Jenkins para:
  1. Executar testes automatizados.
  2. Fazer o build da aplicação em um container Docker.
  3. Enviar a imagem para um Container Registry, como Docker Hub ou GitHub Container Registry.

**Entregas:** arquivo `Jenkinsfile` ou `.github/workflows/main.yml` e captura de tela do pipeline executado com sucesso.

### Etapa 3 - Continuous Deployment e orquestração (2,5 pontos)

- Criar um manifesto Helm para fazer deploy da aplicação em um cluster Kubernetes, que pode ser Minikube ou Kind.
- Configurar o pipeline da Etapa 2 para publicar automaticamente no cluster a nova versão da aplicação (CD).
- Expor a aplicação por meio de um serviço NodePort ou Ingress.

**Entregas:** diretório `/helm` com os charts definidos e captura de tela do serviço em execução no Kubernetes.

### Etapa 4 - Logging e métricas (2 pontos)

- Configurar Filebeat, Elasticsearch e Kibana, que podem ser executados em containers, para coletar os logs da aplicação.
- Criar no Kibana um dashboard simples que mostre:
  1. Logs da aplicação em tempo real.
  2. Métricas de requisições, incluindo status code e tempo de resposta.

**Entregas:** captura de tela do dashboard configurado no Kibana e explicação de como os Beats coletaram os dados.

### Etapa 5 - Relatório final e apresentação (2 pontos)

Entregar um relatório com:

- Passo a passo da implementação.
- Desafios enfrentados.
- Reflexão crítica sobre como DevOps agrega valor ao ciclo de vida do software.

Fazer uma apresentação oral de até 10 minutos demonstrando o pipeline e o monitoramento.

## Material de apoio - Logs, APM e Elastic Stack

### Logs e APM no contexto de DevOps

**Logs** são registros gerados por aplicações e serviços, contendo informações sobre eventos, erros, execuções e estados da aplicação.

O gerenciamento de logs ajuda no diagnóstico de erros, auditoria e segurança, monitoramento de performance e análise de comportamento.

**APM (Application Performance Monitoring)** é o conjunto de práticas e ferramentas que monitora a disponibilidade, o tempo de resposta, as taxas de erro e outras métricas-chave de desempenho de uma aplicação. O material também ilustra um serviço APM com dashboards, exploração de traces, monitores, RUM e servidores de aplicação, recebendo traces, spans e métricas.

### Componentes da Elastic Stack

A Elastic Stack é apresentada como um conjunto de ferramentas para coletar, armazenar, pesquisar e visualizar logs e métricas.

| Ferramenta | Função |
| --- | --- |
| Beats | Agentes leves para coleta de logs e métricas |
| Elasticsearch | Motor de busca e indexação de dados |
| Kibana | Dashboards para visualização e análise |
| Logstash (opcional) | Pipeline para transformar logs antes de armazená-los |

Fluxo simplificado mostrado na apresentação:

```text
Aplicação -> Filebeat/Metricbeat -> Elasticsearch -> Kibana
```

A arquitetura ilustrada do Elasticsearch organiza os dados em um cluster com nós; os índices são divididos em shards, que contêm documentos.

### Beats para DevOps

Beats são agentes leves de envio de dados. Os tipos citados são:

| Beat | Função |
| --- | --- |
| Filebeat | Coleta e envia logs |
| Metricbeat | Coleta métricas de sistema e aplicações |
| Packetbeat | Coleta dados de rede |
| Auditbeat | Monitora alterações de segurança |

### Exemplo de instalação e configuração do Filebeat

Exemplo para Ubuntu apresentado nos slides:

```bash
# Instalar o Filebeat
sudo apt install filebeat

# Configurar o Filebeat
sudo nano /etc/filebeat/filebeat.yml

# Habilitar o módulo system
sudo filebeat modules enable system

# Testar a configuração
sudo filebeat test config

# Iniciar o Filebeat
sudo systemctl start filebeat
```

Fluxo dos dados: o Filebeat lê os logs, envia-os ao Elasticsearch e os dados ficam disponíveis no Kibana.

### Visualização e análise com Kibana

Kibana é uma ferramenta web para explorar e visualizar dados do Elasticsearch e criar dashboards. As funcionalidades citadas incluem dashboards customizados, buscas e filtros por período, alertas e visualização de métricas de APM.

Um dashboard de exemplo, com dados de voos, mostra filtros por origem e destino, distribuição por companhia aérea, contagem de voos, preço médio de passagem e gráficos ao longo do tempo.

Um slide com o título **Kubernetes: Orquestração de Containers e Definição de Ambientes com Helm** apresenta, no conteúdo, um exemplo de visualização de logs no Kibana:

1. Abrir **Discover**.
2. Filtrar por campos, por exemplo `log.level` e `host.name`.
3. Criar um gráfico de erros por minuto.

### Elastic APM

Agentes APM para Node.js, Java, Python e outras linguagens conectam-se ao APM Server. O servidor envia dados de desempenho ao Elasticsearch, para visualização no Kibana.

Métricas monitoradas nos exemplos:

- Tempo de resposta por transação.
- Número de requisições.
- Taxa de erros.
- Uso de recursos.

Exemplo de configuração para Node.js mostrado nos slides:

```javascript
var apm = require('elastic-apm-node').start({
  serviceName: 'minha-app-node',
  serverUrl: 'http://localhost:8200'
});
```

### Conclusão do material

Logs e métricas são essenciais em DevOps para diagnosticar e prevenir falhas. A Elastic Stack é apresentada como uma solução open source para monitoramento, análise e APM. A aula destaca que os alunos experimentaram uma stack profissional usada em empresas.

### Referências apresentadas

- [Elastic Official Documentation](https://www.elastic.co/guide/)
- Basha, Pranav. *Elastic Stack Essentials*. Packt Publishing, 2016.
- Grover, Anurag. *Learning Elastic Stack 7.0*. Packt Publishing, 2019.
- Curso da Udemy: *Elastic Stack - Elasticsearch, Logstash, Kibana & Beats*.
- Tutoriais da DigitalOcean: *Introduction to the Elastic Stack*.

## Atividade prática - Monitoramento de logs e performance com Elastic Stack

**Objetivo:** coletar logs de uma aplicação simples e criar um dashboard no Kibana.

1. Subir uma aplicação simples, por exemplo uma aplicação Node.js ou um servidor Apache local.
2. Instalar o Filebeat na máquina.
3. Configurar o Filebeat para enviar logs ao Elasticsearch.
4. Acessar o Kibana e criar uma visualização personalizada com os logs.
5. **Extra opcional:** adicionar um agente APM à aplicação, se houver tempo.
