---
layout: post
title: "Padrões para desenvolvimento com Rails"
date: 2013-02-11 18:31
comments: true
categories: [Rails]
tags: [YAGNI, KISS,YARD, Controllers, Models]
description: "Algumas regras para um desenvolvimento de aplicações Rails mais produtivo e menos custoso de manter."
keywords: "aplicação de princípios YAGNI, aplicação do princípios KISS, padrão de arquivos em aplicações rails, padrões para desenvolvimento de aplicações rails, código auto-explicativo, comentários YARD, princípios de SOLID, uso de find_by_, operações CRUD, higienização fe parâmetros no rails, princípios de Domain Driven Development"
---
Todos nós gostamos (ou não) de codar com clareza e organização, dessa forma podemos dar possibilidades à terceiros e a nós mesmos 
de efetuar manutenções no programa que desenvolvemos. Quando se trata de equipe de desenvolvimento, algumas regras
deveriam ser explícita e revisadas cotidianamente, pois a curva de entendimento para um novo dev no projeto pode ser
alta e em alguns casos, ficar com um a menos no time resulta na entrega de software mais rápido que ambientar um novo dev.

<!-- more -->
Pensando nesses conceitos, encontrei o artigo [*Rails 3.2 Development Standards Guide*](https://github.com/hopsoft/rails_standards/tree/rails-3-2) 
do [Nathan Hopkins](https://github.com/hopsoft) que baseando-se em princípios como o
[YAGNI](http://en.wikipedia.org/wiki/You_ain't_gonna_need_it) e  [KISS](http://en.wikipedia.org/wiki/KISS_principle ) elaborou alguns
padrões a seguir quando se desenvolve em Rails, achei bastante interessante e resolvir compartilhar. Vejamos:


##Introdução
Aplicar os princípios [YAGNI](http://en.wikipedia.org/wiki/You_ain't_gonna_need_it) e 
[KISS](http://en.wikipedia.org/wiki/KISS_principle ) para:.

* Arquitetura Geral
* Produtos e Recursos da API
* Implementações Específicas


##Arquivos
* Deve ser usado espaços e não tabs
* Tabs devem ser iguais a dois espaços
* Finais de linha no padrão Unix (\n)
* Usar UTF-8 encoding


##Documentação
Faça um esforço para o código ser auto-explicativo.

* Prefira nomes descritivos em seu código. Por exemplo `user_count` é um nome melhor do que `len`.
* Use comentários [YARD](http://yardoc.org/) quando a documentação do código for considerado necessária.
* Evite comentários no método quando ele for muito complexo; _refatoração_ é melhor.


##Diretrizes Gerais
Essas diretrizes são baseadas nas regras de programação de [Sandi Metz](http://sandimetz.com/) introduzidas no Ruby Rogues.
As regras são propositalmente agressiva e são projetadas para dar-lhe uma pausa para que o seu app não corra solto.
Espera-se que você vai quebrá-las por razões pragmáticas ... muito. Veja a nota na YAGNI e KISS.

* Classes não podem ter mais de 100 linhas de código.
* Métodos não podem ser maior do que cinco linhas de código.
* Os métodos podem ter no máximo 4 parâmetros.
* Controllers só devem instanciar um objeto.
* Views só devem ter acesso a uma variável de instância.
* Nunca diretamente referêncie uma outra classe/módulo de dentro de uma classe. Estas referências devem ser passado por parâmetros.

*Seja atencioso ao aplicar estas regras. Se você está lutando contra o quadro (no caso de Scrum, Kaban, etc..), é hora de ser um pouco mais pragmático.*


##Models
* Nunca use finders dinâmicos. por exemplo `find_by_ …`
* Seja atencioso sobre o uso de callbacks e observers que podem levar ao acoplamento indesejado.

Todos os modelos devem ser organizadas usando o seguinte formato:


{% codeblock ruby lang:ruby %}
class MyModel < ActiveRecord::Base
  # extends ...................................................................
  # includes ..................................................................
  # security (i.e. attr_accessible) ...........................................
  # relationships .............................................................
  # validations ...............................................................
  # callbacks .................................................................
  # scopes ....................................................................
  # additional config .........................................................
  # class methods .............................................................
  # public instance methods ...................................................
  # protected instance methods ................................................
  # private instance methods ..................................................
end
{% endcodeblock %}

*OBS: Os comentários listados acima deve existir no arquivo para servir como um lembrete visual do formato.*

##Implementação de Models
É geralmente uma boa idéia isolar diferentes obrigações em módulos separados. 
Recomendamos o uso de Concerns como descrito neste [post do blog](http://37signals.com/svn/posts/3372-put-chubby-models-on-a-diet-with-concerns).


{% codeblock Project %}
|-project
  |-app
    |-assets
    |-controllers
    |-helpers
    |-mailers
    |-models
      |-concerns <-----
    |-views
{% endcodeblock %}


###Orientações

* Operações CRUD que estão limitados a um único modelo deve ser implementada no modelo. Por exemplo, um método `full_name` 
que concatena `first_name` e `last_name`
* Operações CRUD que ultrapassam este modelo devem ser implementadas como uma Concern. Por exemplo, um método `status` que 
precisa olhar para vários outros modelos.
* Operações simples não CRUD, devem ser implementadas como uma Concern.
* Importante! Concerns devem ser isolados e independentes. Eles não devem fazer suposições sobre como o receiver é composto 
em tempo de execução. É inaceitável que uma concern invoque métodos definidos em outras concerns, no entanto, invocando 
métodos definidos no receiver pretendido é permitido.
* Operações complexas multi-step devem ser implementadas como um processo. Veja abaixo.

##Controllers
Controladores devem higienizar parâmetros antes de realizar qualquer outra lógica. A solução preferida é inspirada por esta 
[essência do DHH](https://gist.github.com/dhh/1975644).

Aqui está um exemplo de higienização de parâmetros.

{% codeblock ruby lang:ruby %}
class ExampleController < ActionController::Base
  def create
    Example.create(sanitized_params)
  end

  def update
    Example.find(params[:id]).update_attributes!(sanitized_params)
  end

  protected

  def sanitized_params
    params[:example].slice(:expected_param, :another_expected_param)
  end
end
{% endcodeblock %}

##Processos
Um processo é definido como uma operação multi-step, que inclui qualquer um dos seguintes itens.

* Uma transação com uma tarefa complexa está sendo realizada.
* Uma chamada feita para um serviço externo.
* Qualquer interação no nível do sistema operacional é executada.
* O envio de e-mails, a exportação de arquivos, etc ..

Em uma tentativa de gerir melhor os processos, nós seguimos vagamente alguns princípios de Domain Driven Development (DDD). 
Ou seja, nós adicionamos um diretório `processes` em `app` para realizar implementações de nossos processos.

{% codeblock Project %}
|-project
  |-app
    |-assets
    |-controllers
    |-helpers
    |-mailers
    |-models
    |-processes <-----
    |-views
{% endcodeblock %}

Recomendamos o uso de uma ferramenta como o [Hero](https://github.com/hopsoft/hero) para ajudar 
a modelar esses processos.

**Importante:** Não use model ou controller callbacks para invocar um processo. Em vez disso, invocar processos diretamente 
do controlador.

##Logging
Nós usamos a gem [Yell](https://github.com/rudionrails/yell) para registro. Aqui está um exemplo de configuração.

{% codeblock ruby lang:ruby %}
# example/config/application.rb
module Example
  class Application < Rails::Application
    log_levels = [:debug, :info, :warn, :error, :fatal]

    # %m : The message to be logged
    # %d : The ISO8601 Timestamp
    # %L : The log level, e.g INFO, WARN
    # %l : The log level (short), e.g. I, W
    # %p : The PID of the process from where the log event occured
    # %t : The Thread ID from where the log event occured
    # %h : The hostname of the machine from where the log event occured
    # %f : The filename from where the log event occured
    # %n : The line number of the file from where the log event occured
    # %F : The filename with path from where the log event occured
    # %M : The method where the log event occured
    log_format = Yell.format( "[%d] [%L] [%h][%p][%t] [%F:%n:%M] %m")

    config.logger = Yell.new do |logger|
      logger.adapter STDOUT, :level => log_levels, :format => log_format
    end
  end
end
{% endcodeblock %}

##Extensions & Monkey Patches
* Seja pensativo sobre monkey patching e procure primeiro soluções alternativas.
* Use um inicializador para carregar extensions e monkey patches.

Todas as extensions e monkey patches deve estar em um diretório de extensões em lib.

{% codeblock Project %}
|-project
  |-app
  |-config
  |-db
  |-lib
    |-extensions <-----
{% endcodeblock %}

Use módulos para estender objetos ou adicionar monkey patches. Isto fornece alguma consistência quando você precisa 
para rastrear bugs.

Aqui está um exemplo:

{% codeblock ruby lang:ruby %}
module CowboyString
  def downcase
    self.upcase
  end
end
::String.send(:include, CowboyString)
String.ancestors # => [String, CowboyString, Enumerable, Comparable, Object, Kernel]
{% endcodeblock %}

##Dependências de Gems
Dependências de gems deve ser fixadas antes de implantar no aplicativo de produção. Isto vai garantir a estabilidade da aplicação.

Recomendamos o uso de [gerenciadores de dependência](http://gembundler.com/v1.2/gemfile.html). Ao utilizar um gerenciador de
dependência, certifique-se de especificar pelo menos as versões maiores ou menores. Aqui está um exemplo.

{% codeblock ruby lang:ruby %}
# Gemfile
gem 'rails', '3.2.11' # GOOD: exact
gem 'pg', '~>0.9'     # GOOD: tilde
gem 'yell', '>=1.2'   # BAD: unspecific
gem 'nokogiri'        # BAD: unversioned
{% endcodeblock %}

Bundler's Gemfile.lock resolve o mesmo problema, mas nós descobrimos que as atualizações indevidas do pacote podem causar problemas. 
É muito melhor ser explícito no Gemfile para garantir a estabilidade do aplicativo.

**This will cause your project to slowy drift away from the bleeding edge**. A estratégia deve ser empregada para garantir que o
projeto não se desvie muito do comportamento da versão da gem. Por exemplo, atualizar as gems regularmente (a cada 3-4 meses) e 
ser vigilantes sobre os patches de segurança. Serviços como [Gemnasium](https://gemnasium.com/) pode ajudar com isso.

##Uma nota sobre Frameworks frontend
Coisas interessantes estão acontecendo no mundo dos frameworks frontend.

* [Backbone](http://backbonejs.org/)
* [Ember](http://emberjs.com/)
* [Angular](http://angularjs.org/)
* [Knockout](http://knockoutjs.com/)
* e muitos outros ...

Ser cuidadoso sobre a decisão de usar um framework frontend. Pergunte-se se existe complexidade em manter e se a decisão é certa. 
Muitas vezes existem soluções melhores e mais simples.

Leia os seguintes artigos antes de decidir. No final, você deve ser capaz de expressar por que sua decisão é a certa.

[How Basecamp Next got to be so damn fast without using much client-side UI](http://37signals.com/svn/posts/3112-how-basecamp-next-got-to-be-so-damn-fast-without-using-much-client-side-ui)
[Rails in Realtime](http://layervault.tumblr.com/post/30932219739/rails-in-realtime)
[Rails in Realtime, Part 2](http://layervault.tumblr.com/post/31462727280/rails-in-realtime-part-2)
Em ambos os casos estar atento a "layout thrashing", [conforme descrito aqui](http://kellegous.com/j/2013/01/26/layout-performance/).
