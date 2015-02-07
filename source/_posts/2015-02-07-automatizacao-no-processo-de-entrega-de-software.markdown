---
layout: post
title: "Automatização no Processo de Entrega de Software"
date: 2015-02-07 12:48
comments: true
categories: "Entrega Contínua"
tags: [Pipeline de Entrega, Provisionamento, Integração Contínua, XP, Scrum, Kanban]
description: "Como permitir aumentar a capacidade de liberar versões do software sob demanda, de gerar releases bem-sucedidos e implantar o sistema em qualquer ambiente simplesmente apertando um botão"
keywords: "Implantar o pipeline de entrega, Automatização dos processo de entrega de software, Aplicação de metodologias ágeis de desenvolvimento, Automação da entrega de software"
---

Um grande problema que afeta a qualidade do desenvolvimento é a realização de tarefas importante para a entrega de software que são executadas
manualmente. Este artigo tem o objetivo de mostrar boas práticas de desenvolvimento que vem sendo estudadas ha várias décadas para fornecer ao mundo
uma alternativa às metodologias pesadas e altamente dirigidas por documentação, permitindo aumentar a capacidade de liberar versões do sistema sob
demanda, de gerar releases bem-sucedidos e implantar o sistema em qualquer ambiente simplesmente apertando um botão, sem precisar se preocupar se
funcionará ou não.

<!-- more -->

> Este artigo foi escrito como requisito da matéria Seminário de Computação e Informática do curso de Ciência da Computação. [Clique aqui para ver o
> original](https://www.scribd.com/doc/255008188/Automatizac-a-o-no-Processo-de-Entrega-de-Software)


## 1. Introdução 
Com a globalização e o aumento ao acesso a banda larga, o mundo vive uma interação em tempo real. Ideias, tendências, necessidades, informações,
dentre outros, navegam de continente à continente em uma velocidade jamais imaginada, tendo o software como parte fundamental desse processo.
Desenvolver software nesse cenário requer um processo onde uma ideia na mente do cliente vire código funcional e esteja disponível para o usuário
final de maneira rápida e confiável. O tempo para efetuar esse processo, denominado  como tempo de ciclo, pode determinar o rumo dos negócios junto a
concorrência.

Desde 1980, autores como Kent Beck, Martin Fowler, Paul Duvall, Ron Jeffries e Dave Thomas, estudam melhores práticas de desenvolvimento com o
objetivo de poder fornecer ao mundo uma alternativa às metodologias pesadas e altamente dirigidas por documentação que estão em uso até hoje
[\[5\]](#livro5). Conforme os dos 12 princípios do Manifesto Ágil [\[6\]](#livro6), o software em funcionamento tem maior valor do que uma
documentação abrangente e software funcional é a medida primária do progresso. Software não gera lucro ou valor até que esteja nas mão de seus
usuários [\[4\]](#livro4).

Este artigo demonstra boas práticas das metodologias ágil de desenvolvimento para efetuar entrega contínua de software de valor ao cliente, descreve
os passos de como detectar erros mais cedo através da prática da integração contínua, permitindo que se desenvolva software coeso mais rapidamente
[\[2\]](#livro2). Demonstra o padrão pipeline de entrega, capaz de permitir automatização desde o desenvolvimento do software até sua entrega final.
Conforme Duval [\[1\]](#livro1), o pipeline de entrega é um processo no qual diversos tipos de tarefas são executadas com base no sucesso da tarefa anterior.

Com a automatização dos processos, a entrega de software se torna confiável, previsível, com riscos quantificáveis e bem entendidos, garantindo que
quando for preciso fazer alguma modificação, o tempo para realizá-las, colocá-las em produção e em uso, seja o menor possível e, que problemas sejam
encontrados cedo o bastante para que sejam fáceis de corrigir.

O pipeline envolve atividades de todos os interessados pela entrega de software, amplia as taxas de implantação e fomenta práticas do movimento
DevOps, criando um relacionamento colaborativo entre as equipes de desenvolvimento, qualidade e de operações. Automatizar os processos de forma que
todos os envolvidos possam executar tarefas (que até então são manuais) de forma assíncrona, melhora produtividade e a quantidade de entregas de valor
dentro de tempo de ciclo, permitindo implantar o sistema para qualquer ambiente instantaneamente, refletindo as mudanças de uma forma eficiente e com
baixo custo.

## 2. Metodologias Ágeis de Desenvolvimento 
Os processos das metodologias tradicionais não acompanharam a evolução do tráfico de informações. Conforme [\[4\]](#livro4), o principal problema
enfrentado pelos profissionais da área de desenvolvimento de software é como fazer para transformar uma boa ideia em um sistema e entregá-lo aos
usuários o quanto antes.

Em busca de valorizar a importância da interação entre os envolvidos e a entrega de software de valor ao cliente a curto prazo, em 2001, 17 pensadores
de desenvolvimento de software se reuniram e concordaram que, em suas experiências prévias, um pequeno conjunto de princípios sempre parecia ter sido
respeitado quando os projetos davam certo. Esses princípios foram reunidos no Manifesto Ágil [\[6\]](#livro6).

Kent Beck definiu um conjunto de valores, princípios e práticas que resultou em um trabalho denominado Extreme Programming (XP). Segundo Sato
[\[9\]](#livro9), a XP foi uma das primeiras metodologias ágeis que revolucionou a forma como os softwares eram desenvolvidos e, além de se basear em
valores para guiar o desenvolvimento, tais como a comunicação clara entre os envolvidos, a simplicidade em fazer o suficiente para atender as
necessidades, o feedback para direcionar o produto e a coragem de efetuar mudanças, trazem uma serie de práticas, como a integração contínua (IC), o
desenvolvimento guiado por testes e a refatoração. Práticas que quando aplicadas, contribuem para uma entrega de qualidade e eficiente de software.

Ken Schwaber definiu o Scrum, um conjunto de práticas como o objetivo de manter o gerenciamento do projeto visível aos usuários. Uma metodologia ágil 
para gestão e planejamento de projetos de software, onde o desenvolvimento é dividido em iterações com período de duas a seis semanas, chamadas de Sprints.

Outra metodologia ágil que vem sendo utilizada para dar apoio ao Scrum é o Kanban. Kanban é um termo de origem japonesa e significa literalmente “cartão”, 
criado por Taiichi Ohno para indicar o andamento do fluxo de produção em empresas de fabricação em série. Tipicamente usa-se um quadro em branco com 
post-its (Quadro Kanban) para mapear o fluxo de valor das atividades relacionadas ao desenvolvimento de software.

A implementação de metodologias ágil proporciona o desenvolvimento cooperativo, onde baseiam-se mais nas pessoas e suas iterações
em vez de grandes esforços de planejamento e processos rígidos. Segundo Pressman [\[7\]](#livro7), em essência, os métodos ágeis
foram desenvolvidos em um esforço para vencer as fraquezas percebidas e reais da engenharia de software convencional.

## 3. Pippeline de Implantação
O processo de levar a funcionalidade da mente do cliente até o usuário final envolve várias etapas, dentre elas, a etapa de desenvolvimento. O
desenvolvimento é composto de vários processo além da codificação e podem ser mapeados através da modelagem do mapa de fluxo de valor. O mapa de fluxo
de valor possibilita uma visão mais holística, de ponta a ponta do processo de entrega. O objetivo do fluxo de valor é mapear uma solicitação do
cliente, do momento em que ela chega até que ela esteja disponível em produção [\[8\]](#livro8).

No desenvolvimento, é necessário juntar o código produzido ao código principal, testar esse código para certificar que não foram adicionados defeitos
ao projeto, configurar ambientes para instalação do software e efetuar a implantação nesses ambientes, proporcionando demonstrações, testes
exploratório e a disponibilização para o usuário final. O pipeline de implantação modela esse processo, e sua inversão em uma ferramenta de integração
contínua, de gerência de versões e a utilização da prática de desenvolvimento guiado por teste como o TDD1, é o que permite que uma equipe veja e
controle o processo de cada mudança à medida que ela se move em direção a entrega.

Para Humble e Farley [\[4\]](#livro4) o pipeline de implantação é uma manifestação automatizada do processo de levar o software do controle de versão até os
usuários. Cada mudança passa de forma consistente no percurso de entrega através da automatização. A automatização torna passos complexos e
suscetíveis a erros em passos repetíveis e confiáveis. Fowler [\[3\]](#livro3) acrescenta que o pipeline é para detectar quaisquer mudanças que levem os
problemas para produção e para permitir a colaboração entre os envolvidos, possibilitando a visibilidade de todo o fluxo de mudanças juntamente com
uma trilha para uma auditoria completa.

## 4. Entrega de software automatizada
Metodologia ágeis valorizam a entrega de software de valor a curto prazo. Com elas, tem-se um conjunto de boas práticas para o desenvolvimento de
software incremental (ou iterativo), onde o sistema começa a ser implantado logo no início do projeto e vai ganhando novas funcionalidades ao longo do
tempo. Isso exige que os processos de entrega sejam executados diversas vezes no decorrer de uma Sprint.

Para a definição de um pipeline de entrega, é necessário ter uma visão de todos os processos. O quadro Kanban auxilia nessa tarefa. A Figura 01
demonstra um exemplo do fluxo de entrega. Modelar um pipeline de implantação exige entender seus processos de entrega e balancear o nível de feedback
que você quer receber [\[9\]](#livro9). Projetos anteriores podem servir como base para um novo projeto.

Após a definição das funcionalidades, elas são adicionadas na coluna “Solicitações” e ficam aguardando o planejamento para a próxima Sprint. No
planejamento, as solicitações que irão ser desenvolvidas na Sprint, são movidas para a coluna “A Fazer”. Ao iniciar a codificação da funcionalidade, o
desenvolvedor atualiza o status da funcionalidade movendo-a para a coluna “Fazendo”, até que seja concluída. Até esse momento, o processo se deu de
forma manual.

![Movimentação das funcionalidades no quandro Kanban](/images/Quadro-Kanban.png)

_Figure 01. Movimentação das funcionalidades no quandro Kanban._

A entrada do pipeline inicia-se com o check-in no sistema de controle de versão. Um dos sistemas de controle de versão mais utilizado é o
[Git](http://git-scm.com). Nessa etapa do processo, uma ferramenta de IC como o [Jenkis](http://jenkis-ci.org), o [Go](http://go.thoughworks.com), ou
até um serviço de integração contínua nas nuvens como o [TravisCI](http://travis-ci.org), pode ser configurada para monitorar o repositório a cada
alteração e mover a funcionalidade nas três etapas seguintes.

De acordo com a tecnologia utilizada pela aplicação, é possível fazer uso de ferramentas como o [RSpec](http://rspec.info) e o [JUnit](http://junit.org)
para escrever os testes automatizados. A alteração que tenha sido validada com sucesso vira uma nova versão do software (uma versão candidata a ir
para produção). Cada estágio de teste avalia a versão candidata de uma perspectiva diferente e a cada teste que ela passa, aumenta a confiança em sua
implementação. O objetivo desse processo é eliminar as versões candidatas que não estejam prontas para produção o quanto antes e obter feedback sobre
a falha o mais rápido possível [\[4\]](#livro4).

Quando a versão candidata passa pelo estágio de teste de aceitação automatizados, ela se torna algo útil e interessante, não sendo mais prioridade da
equipe de desenvolvimento. É preferível que os estágios de implantação para os ambientes de aceitação e produção não executem automaticamente. Os
testadores devem ser capazes de ver quais versões candidatas passaram com sucesso e implantar o sistema em um ambientes configurado com um simples
apertar de botão. Para a automatização do processo de implantação, ferramentas como a [Capistrano](http://capistranorb.com) e a
[Mina](http://nadarei.co/mina), podem ser utilizadas.

Com a adoção dessa abordagem, não é permitido efetuar implantação do sistema em produção sem que a versão seja apropriadamente testada. Regressões são
evitadas ao se fazer correções, elas passam pelo mesmo processo que quaisquer mudança. Um aumento da automação de processos leva a uma maior
eficiência e amplia as taxas de implementação com relação as metodologias tradicionais, conforme a Figura 02.

Para a criação dos ambientes de aceitação e de produção, existem uma série de ferramentas de provisionamento no mercado, delas destacam-se:
[Ansible](http://www.ansibleworks.com/tech/), [Chef](http://www.opscode.com/chef), [Puppet](http://puppetlabs.com/puppet) e
[Salt](http://saltstack.com/). O processo de provisionamento é um conjunto de passos executáveis que podem ser aplicados em uma imagem inicial do
sistema operacional para ter tudo configurado corretamente [\[10\]](#livro10).

![Movimentação das funcionalidades no quandro Kanban](/images/cascataxagil.png)

_Figure 02. Ciclo de entregas de software_


## 5. Conclusão

Este trabalho teve como foco demonstrar conjunto de boas práticas de desenvolvimento de software, testadas e validadas por diversos autores. O
objetivo está em permitir efetuar a entrega de software com maior qualidade em um curto período e promover a comunicação com um ciclo de feedback
constante entre todos os interessados.

Com isso, muda-se o conceito de “pronto”, fazendo com que uma funcionalidade só esteja pronta quando ela está em produção, fazendo o que tem que fazer
e entregando valor aos seus usuários. Em Startups, esse ciclo de entrega contínua é constantemente utilizado, geralmente se tem pouco recurso e é
necessário pensar em automatização desde o início. É preciso entregar valor ao usuário para continuar vivo e, esperar a próxima madrugada para subir a
nova funcionalidade pode causar um grande impacto nos negócios.

A qualidade da aplicação está relacionada também com o ambiente onde ela está implantada. Deve-se tratar as configurações da mesma forma que o código
fonte. Ao seguir a política de que nada é modificado em um ambiente a menos que esteja em um script com versão, automatizado e faça parte de um
caminho único para a produção (o pipeline de entrega), é possível determinar melhor a causa raiz dos erros mais rapidamente, acelerando a correção e
diminuindo a probabilidade de que pequenos erros se transformem em grandes dores de cabeça no futuro.

## 6. Referências

[\[1\]](id:livro1) [DUVAL, Paul. (2012) "Agile DevOps: O Achatamento do Processo de Reliase de Software"](http://www.ibm.com/developerworks/br/library/a-devops1/)

[\[2\]](id:livro2) [FOWLER, Martin. (2006) “Continuous Integration”](http://martinfowler.com/articles/continuousIntegration.html)

[\[3\]](id:livro3) [FOWLER, Martin. (2013) “Deployment Pipeline”](http://martinfowler.com/bliki/DeploymentPipeline.html)

[\[4\]](id:livro4) [HUMBLE, J.; FARLEY, D. (2014) “Entrega Contínua: Como Entregar Software de Forma Rápida e Confiável”. Porto Alegre: Bookman](http://www.grupoa.com.br/livros/engenharia-de-software-e-metodos-ageis/entrega-continua/9788582601037)

[\[5\]](id:livro5) [LOPES, Camilo. (2012) “TDD na Prática. Rio de Janeiro: Ciência Moderna](http://www.lcm.com.br/site/#/livros/detalhesLivro/tdd---test-driven-development-na-pratica.html)

[\[6\]](id:livro6) [MANIFESTO ÁGIL. (2001) "Manifesto Para o Desenvolvimento Ágil de Software"](http://manifestoagil.com.br/)

[\[7\]](id:livro7) [PRESSMAN, R. S. (2006) “Engenharia de Software”. São Paulo: McGraw-Hill](http://www.grupoa.com.br/livros/engenharia-de-software-e-metodos-ageis/engenharia-de-software/9788563308337)

[\[8\]](id:livro8) [POPPENDIECK, Mary; POPPENDIECK, Tom. (2011) “Implementando o Desenvolvimento Lean de Software: Do Conceito ao Dinheiro”. Porto Alegre: Bookman](http://www.grupoa.com.br/livros/engenharia-de-software-e-metodos-ageis/implementando-o-desenvolvimento-lean-de-software/9788577807567)

[\[9\]](id:livro9) [SATO, Danilo. (2013) “DevOps na Prática: Entrega de Software Confiável e Automatizada”. São Paulo: Casa do Código](http://www.casadocodigo.com.br/products/livro-devops)

[\[10\]](id:livro10) [TAVARES, B. (2013)” Puppet e Vagrant: Como provisionar máquinas para seu projeto](http://www.thoughtworks.com/pt/insights/blog/puppet-and-vagrant-how-provision-machines-your-project)

[\[11\]](id:livro11) [TELES, Vinícius M. (2004) “Extreme Programming. São Paulo: Novatec Editora Ltda](http://novatec.com.br/livros/extreme/)

















