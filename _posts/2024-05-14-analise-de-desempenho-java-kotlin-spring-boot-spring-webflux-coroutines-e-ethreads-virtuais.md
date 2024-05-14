---
layout: post
title: "Análise de Desempenho: Java, Kotlin, Spring Boot, Spring WebFlux, Coroutines e Threads Virtuais"
canonical_url: https://blog.edermatos.dev/an%C3%A1lise-de-desempenho-java-kotlin-spring-boot-spring-webflux-coroutines-e-threads-virtuais
author: eder
categories: [ Java, Kotlin, Virtual Threads, Coroutines, WebFlux, K6, LoadTest, Benchmark ]
image: https://miro.medium.com/v2/resize:fit:720/format:webp/1*lPbUg8vtwq6rpVkeFoXqBg.png
featured: true
---

No desenvolvimento de aplicações back-end, a escolha das tecnologias certas pode ter um impacto significativo no desempenho do sistema. Neste artigo, vamos realizar uma análise comparativa de diferentes cenários de desenvolvimento de APIs em Java e Kotlin, utilizando o frameworks Spring Boot, WebFlux, Coroutines do Kotlin e Threads Virtuais do Java 21. <br>
Vamos investigar como essas tecnologias se comportam em termos de consumo de recursos, throughput, latência e outros aspectos.

# Sumário

1. [Kotlin vs Java](#kotlin-vs-java)
2. [Spring Framework](#spring-framework)<br>
    2.1. [Funcionalidades e Benefícios do Spring](#funcionalidades-e-benefícios-do-spring)<br>
3. [Programação Reativa com WebFlux](#programação-reativa-com-webflux)<br>
    3.1. [Modelo assíncrono e sem bloqueio](#modelo-assíncrono-e-sem-bloqueio)<br>
    3.2. [Fluxos de dados baseados em eventos](#fluxos-de-dados-baseados-em-eventos)<br>
    3.3. [Integração de biblioteca reativa](#integração-de-biblioteca-reativa)<br>
    3.4. [Tratamento de dados com Mono e Flux](#tratamento-de-dados-com-mono-e-flux)<br>
    3.5. [Ferramentas de desenvolvimento para WebFlux](#ferramentas-de-desenvolvimento-para-webflux)<br>
4. [Coroutines com Kotlin](#coroutines-com-kotlin)<br>
    4.1. [Principais recursos e implementação](#principais-recursos-e-implementação)<br>
    4.2. [Comparação com Threading Tradicional](#principais-recursos-e-implementação)<br>
5. Threads Virtuais
6. Estrutura da Aplicação
7. Execução dos Testes
8. Análise dos Resultados
9. Resumo da Análise
10. Perguntas e Respostas
11. Código Fonte


<br>

## Kotlin vs Java
Kotlin é uma linguagem de programação moderna e estaticamente tipada, projetada para superar as limitações do Java, como verbosidade e NullPointerException. Ela roda na Máquina Virtual Java (JVM) e oferece total interoperabilidade com Java. Essa interoperabilidade garante que Kotlin e Java possam trabalhar juntos sem problemas, permitindo que os desenvolvedores aproveitem os melhores recursos de ambas as linguagens sem problemas de compatibilidade.
<br>

Apesar de serem interoperáveis, Kotlin se destaca ao oferecer uma sintaxe mais concisa e expressiva, reduzindo significativamente o código repetitivo e aumentando a legibilidade dos programas. Além disso, Kotlin resolve o problema comum como NullPointerException introduzindo um sistema de tipos que diferencia entre tipos nulos e não nulos, aumentando a segurança e a confiabilidade do programa.
<br>

Como tanto Kotlin quanto Java operam na JVM e compartilham uma performance comparável devido ao mesmo ambiente de execução subjacente, uma comparação entre eles apresenta um aspecto intrigante para análise. Esta discussão não é apenas teórica, mas se estende para benchmarks de desempenho práticos e aplicações do mundo real, tornando-a essencial para desenvolvedores que utilizam Kotlin ou Java em seus projetos.
<br>

A introdução de Coroutines em Kotlin e Threads Virtuais em Java representa avanços significativos na programação assíncrona, prometendo impactar como as aplicações são desenvolvidas e executadas.
<br>

## Spring Framework
Atualmente, no desenvolvimento de APIs utilizando as linguagens Java e Kotlin temos um framework muito popular, o Spring.
<br>

O Spring é um framework que oferece uma infraestrutura leve e um conjunto de ferramentas para gerenciamento de transações, segurança e acesso a dados, aprimorando o desenvolvimento de aplicações de negócios grande escala.
<br>

Ele é amplamente utilizado e oferece suporte para diferentes abordagens de desenvolvimento, incluindo o uso de threads tradicionais e o paradigma reativo. Além disso, o Spring oferece o Spring WebFlux, que permite o desenvolvimento de APIs reativas utilizando o servidor Netty. Por outro lado, o Kotlin oferece recursos como Coroutines, que são alternativas mais leves às threads tradicionais.
<br>

### Funcionalidades e Benefícios do Spring
**Autoconfiguração:** Spring Boot detecta as bibliotecas presentes no classpath do projeto e configura automaticamente os componentes necessários, minimizando esforços de configuração manual.
<br>
**Servidores Embarcados:** Suporta servidores embarcados como Tomcat, Jetty, Undertow e Netty, que facilitam a execução de aplicativos diretamente de um arquivo JAR independente, simplificando assim os processos de implantação e teste.
<br>
**Spring Initializer:** Esta ferramenta oferece uma interface baseada na web para gerar uma estrutura inicial de um projeto adaptados a necessidades específicas, fornecendo um início rápido para projetos com uma configuração de linha de base funcional.
<br>
**Gerenciamento de Dependências:** Spring Boot aprimora o gerenciamento de dependências por meio de suas dependências “iniciais”, que pré-definem um conjunto de versões de bibliotecas compatíveis para evitar conflitos e facilitar a manutenção.
<br>
**Arquitetura de microsserviços:** fornece recursos essenciais para o desenvolvimento de microsserviços, como tolerância a falhas, configuração externalizada e descoberta de serviços, tornando-a ideal para arquiteturas de aplicativos modernas.
<br>
**Actuator e DevTools:** Spring Boot Actuator oferece recursos prontos para produção para ajudar a monitorar e gerenciar a integridade de seu aplicativo, enquanto Spring Boot DevTools aumenta a produtividade do desenvolvedor com recursos como reinicializações automáticas e hot swapping.
<br>
**Suporte GraalVM:** O suporte para imagem nativa GraalVM no Spring Boot permite que os desenvolvedores compilem aplicativos em executáveis ​​nativos, o que resulta em tempos de inicialização mais rápidos e redução no consumo de memória


## Programação Reativa com WebFlux
### Modelo assíncrono e sem bloqueio
O Spring WebFlux introduz uma abordagem não-bloqueante e orientada a eventos para processamento de dados, que é fundamentalmente diferente da estrutura Spring MVC tradicional baseada em servlet.
<br>

Ao modelar dados e eventos como fluxos observáveis, o WebFlux permite que os desenvolvedores implementem rotinas que reagem dinamicamente às mudanças nesses fluxos. Este modelo é particularmente benéfico para aplicações que requerem alta escalabilidade e sistemas responsivos.

### Fluxos de dados baseados em eventos
**Fluxos de dados observáveis:** Dados e eventos são tratados como fluxos contínuos, que podem ser observados e manipulados usando técnicas de programação funcional.<br>
**Modelo de simultaneidade de loop de eventos:** O WebFlux utiliza um modelo de loop de eventos, gerenciando múltiplas conexões simultâneas de forma eficiente com menos threads, aumentando assim a capacidade de escalabilidade do aplicativo.<br>
**Estilo de programação funcional:** Enfatiza uma abordagem de codificação declarativa, permitindo um código mais conciso e legível, que é mais fácil de depurar e manter.

### Integração de biblioteca reativa
O WebFlux é executado com o servidor Netty, que oferece suporte a operações sem bloqueio e altamente escaláveis. Ele usa o Project Reactor como sua biblioteca reativa básica, projetada para lidar com fluxos de dados assíncronos com suporte para contrapressão (backpressure). A contrapressão é um mecanismo que ajuda a gerenciar o fluxo de dados para evitar a sobrecarga dos consumidores.

### Tratamento de dados com Mono e Flux
WebFlux apresenta duas formas para lidar com fluxos de dados:

**Mono:** Lida com 0 ou 1 item, fornecendo métodos para trabalhar com pontos de dados únicos ou opcionais. Ele é completado com um sinal onComplete ou onError.<br>
**Flux:** gerencia uma sequência de 0 a N itens, permitindo operações em um fluxo de dados. Assim como o Mono, ele pode terminar com um sinal de conclusão ou de erro.

### Ferramentas de desenvolvimento para WebFlux
Para aproveitar totalmente os recursos do WebFlux, os desenvolvedores podem utilizar várias ferramentas:

**Spring Reactive Web:** Esta dependência é essencial para criar aplicações reativas com WebFlux.<br>
**WebClient:** Para consumir serviços RESTful de forma assíncrona, WebClient fornece uma maneira sem bloqueio de lidar com solicitações HTTP, substituindo o RestTemplate tradicional.<br>
**Router functions:** permite aos desenvolvedores lidar com o roteamento de aplicações de forma declarativa, o que pode ser mais expressivo e flexível em comparação ao roteamento tradicional baseado em controlador.

## Coroutines com Kotlin
As Coroutines são um recurso de destaque para programação assíncrona, oferecendo uma abordagem multitarefa cooperativa sem usar diretamente threads do sistema operacional. Este método reduz significativamente a sobrecarga associada à criação e gerenciamento de threads, melhorando assim o desempenho.

### Principais recursos e implementação

**Contexto e continuação da Coroutine:** As coroutines operam dentro de um CoroutineContext que fornece dados com escopo definido, imutáveis, mas substituíveis, semelhantes aos valores com escopo do Java (JEP 429).
O componente fundamental das coroutines é a interface Continuation, que facilita a continuação da execução após a suspensão.
<br>
**Funções Suspensas:** As funções são marcadas com a palavra-chave suspend, permitindo que sejam pausadas e retomadas posteriormente. A assinatura da função permanece consistente, focando no tipo de retorno.
<br>
**Coroutine Builders and Intrinsics:** launch and asyncsão construtores primários usados ​​para iniciar escopos de coroutine. Eles permitem uma implementação direta, semelhante à escrita de código de bloqueio síncrono.<br>
Operações de baixo nível são tratadas por meio de intrínsecos de rotina como resumeWith, que gerencia o estado de execução pós-suspensão.

### Comparação com Threading Tradicional
Desafios das threads: A thread tradicional é conhecido por consumir muitos recursos e ser desafiador para gerenciar e depurar. Threads são caros e com disponibilidade limitada.
Vantagens das coroutines: As coroutines fornecem uma abordagem mais leve e eficiente para lidar com a simultaneidade e são independentes de plataforma, adaptando-se perfeitamente em JVM, JavaScript e outras plataformas.
Integração e uso prático
A maioria das funcionalidades de coroutines são estendidas por meio de bibliotecas em vez da linguagem principal, com a palavra-chave suspend sendo a principal adição ao Kotlin.
Bibliotecas essenciais como kotlinx.coroutines desempenham um papel crucial na implementação eficiente de tarefas assíncronas.

Considerações de desempenho
Embora as coroutines melhorem o desempenho reduzindo a sobrecarga de thread, há uma sobrecarga de tempo de inicialização aproximada de 50% em comparação com pools de threads tradicionais. No entanto, essa comparação geralmente varia de acordo com o caso de uso e configuração específicos do aplicativo.

A abordagem do Kotlin para programação assíncrona por meio de coroutines não apenas simplifica o processo de desenvolvimento, mas também oferece uma solução robusta e escalonável que se alinha bem às necessidades dos aplicativos modernos. Sua compatibilidade com vários mecanismos de servidor e recursos multiplataforma estabelecem ainda mais as coroutines como uma escolha preferida no ecossistema Kotlin.

Threads virtuais
Threads virtuais é um muito recurso importante da iniciativa Project Loom do OpenJDK, que visa aprimorar a escalabilidade de aplicativos Java, permitindo a criação de muitos threads a um custo baixo. Ao contrário dos threads tradicionais, as threads virtuais são gerenciados por uma biblioteca de tempo de execução ou máquina virtual em vez do sistema operacional, o que reduz a sobrecarga e potencialmente melhora o desempenho. Essas threads fazem parte do Java 19 e são projetados para simplificar o desenvolvimento e a manutenção de aplicativos simultâneos de alto rendimento.

Tipos e características
Existem dois tipos principais de threads neste contexto:

Threads de plataforma: são threads tradicionais que servem como invólucros finos em torno dos threads do sistema operacional.
Threads Virtuais: Não correspondem a um thread específico do sistema operacional e são mais flexíveis e leves.
Threads virtuais são particularmente eficazes para tarefas que geralmente estão bloqueadas ou aguardando a conclusão de operações de E/S. Eles não se destinam a tarefas de longa duração com uso intensivo de CPU devido ao seu design que prioriza a escalabilidade em relação à velocidade de processamento bruta.

Criação e Gestão
A criação de uma thread virtual pode ser realizada usando métodos como Thread.ofVirtual() ou Executors.newVirtualThreadPerTaskExecutor(). Esses métodos destacam a facilidade de integração de threads virtuais em aplicativos Java existentes. Threads virtuais residem como objetos na memória heap Java e são atribuídos a um thread de plataforma apenas durante a execução do trabalho real, o que otimiza a utilização de recursos.

Desempenho e utilização
Threads virtuais são projetados para fornecer escala (maior rendimento) em vez de velocidade (menor latência). Eles têm uma pilha de chamadas superficial, normalmente lidando com tarefas mínimas, como uma única chamada de cliente HTTP ou uma consulta JDBC. A JVM gerencia esses threads de maneira inteligente, estacionando-os quando ociosos e atribuindo recursos de computação apenas quando necessário. Esta abordagem de gerenciamento inclui técnicas como o uso de semáforos para controle de acesso a recursos e a garantia de que threads virtuais sejam sempre threads daemon com prioridade fixa.

Em resumo, as threads virtuais representam uma evolução significativa para o Java e paras linguagens que rodam na jvm, fornecendo uma alternativa mais escalável e eficiente em termos de recursos aos modelos tradicionais de thread.