# microservice-saga

# Introdução 

Repositório dedicado a documentar e exemplificar a implementação do padrão Saga em arquiteturas de microserviços. O padrão Saga é uma estratégia para gerenciar transações distribuídas, garantindo a consistência dos dados em sistemas compostos por múltiplos serviços independentes.


# Microserviços

É uma abordagem arquitetural que visa desenvolver aplicações como um conjunto de serviços pequenos e independentes, que se comunicam entre si por meio de APIs. Cada microserviço é responsável por uma funcionalidade específica e pode ser desenvolvido, implantado e escalado de forma independente.

Esses pequenos microserviços devem pertencer a uma equipe autosuficiente, que tenha todas as habilidades necessárias para desenvolver, testar, implantar e operar o serviço. Isso promove a agilidade e a inovação, permitindo que as equipes trabalhem de forma mais autônoma e rápida.

Os microserviços permitem resolver problemas como:

- Delimitação por domínio;
- Escalabilidade e alta disponibilidade;
- Entrega contínua;
- Resiliência e tolerância a falhas;
- Diminui o acoplamento entre os domínios de negócio;
- São altamente sustentáveis e testáveis.

# Transações Distribuídas

Transações distribuídas são operações que iniciam em um microserviço, mas que só são concluídas em outro microserviço de maneira conceitual. Existem alguns padrões de projetos para trabalhar com transações distribuídas, como:

- Padrão saga orquestrado;
- Padrão saga coreografado;
- Padrão outbox.
- 2PC (Two-Phase Commit).

# Padrão saga

É um padrão que tem por objetivo garantir a execução e sucesso de um fluxo e transações e também garantir que em caso de inconsistência ou falha, todas as operações sejam desfeitas na sequência que foram realizadas. Existem dois tipos de padrões saga:

- Saga orquestrado: Utiliza um componente central (orquestrador) para coordenar a execução das transações e suas compensações. O orquestrador é responsável por iniciar as transações, monitorar seu progresso e acionar as operações de compensação quando necessário.
- Saga coreografado: Cada microserviço é responsável por executar sua própria transação e, em caso de falha, acionar a compensação localmente. A comunicação entre os microserviços é feita por meio de eventos, onde cada serviço escuta eventos relevantes e reage a eles conforme necessário.

Em ambos os padrões não é utilizado o 2PC (Two-Phase Commit), que é um protocolo de consenso para garantir a atomicidade de transações distribuídas, mas que pode introduzir complexidade e latência no sistema. O padrão saga não precisa ser aplicado e não deve ser aplicado em todos os cenários, é importante avaliar se o padrão é adequado para o contexto do sistema e dos requisitos de negócio.


## Padrão saga orquestrado

Consiste na ideia de um orquestrador, um agente externo, que fica responsável por orquestrar, isto é, determinar qual será a ordem dos eventos baseados em casos de sucesso. Nessa estratégia apenas o orquestrador conhece os microserviços que serão executados na ordem conforme o fluxo de negócio. Quando utilizamos o padrão saga orquestrado:

- estratégia recomendada para microserviços que já existem e não utilizam o padrão saga;


## Padrão saga coreografado

Consiste de um coreografia existente entre os microserviços, ou seja, o próprio serviço possui a lógica para saber qual será o próximo passo a ser decidido o fluxo com base nos resultados da interação atual, seja sucesso ou falha. Nessa estratégia cada microserviço conhece os outros microserviços que serão executados na ordem conforme o fluxo de negócio. O saga coreogrado é recomendado para uma arquitetura nova, onde os microserviços serão desenvolvidos com o padrão saga coreografado desde o início. O próprio serviço deve garantir o rollback em caso de falha.


# Event-driven architecture (EDA)

É uma arquitetura orientada a eventos onde cada evento representa uma mudança de estado ou uma ação que ocorreu no sistema. Os eventos são produzidos por produtores (produtores de eventos) e consumidos por consumidores (consumidores de eventos). A comunicação entre produtores e consumidores é assíncrona, o que significa que o produtor não espera uma resposta imediata do consumidor.

## Apache Kafka

É uma plataforma de streaming distribuída que permite a construção de aplicações baseadas em eventos. O Kafka é projetado para ser altamente escalável, tolerante a falhas e de baixa latência, tornando-o ideal para cenários de processamento de dados em tempo real. Conceitos pertinentes ao utilizar kafka:

- Tópicos: São categorias ou canais onde os eventos são publicados pelo producer. Cada tópico pode ter múltiplas partições para permitir paralelismo e escalabilidade;
- Producers: São responsáveis por publicar eventos em tópicos específicos. Eles enviam mensagens para o Kafka, que as armazena de forma durável;
- Consumers: São responsáveis por ler eventos de tópicos específicos. Eles se inscrevem em tópicos e processam as mensagens recebidas;
- Consumer groups: É um conjunto de consumidores que trabalham juntos para consumir eventos de um ou mais tópicos, de forma coordenada, permitindo uma maior escalabilidade e tolerância a falhas de leitura de eventos kafka;
- Replicas - Cada partição de um tópico pode ter múltiplas réplicas, que são cópias da partição armazenadas em diferentes brokers. Isso garante alta disponibilidade e tolerância a falhas;