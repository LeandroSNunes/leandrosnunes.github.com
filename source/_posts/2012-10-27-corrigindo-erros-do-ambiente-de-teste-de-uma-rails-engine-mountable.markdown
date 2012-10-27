---
layout: post
title: "Corrigindo bugs do ambiente de teste de uma Rails Engine Mountable"
date: 2012-10-27 10:12
comments: true
categories: [Rails, Ruby]
tags: [engine, tests, bugs]
description: 
keywords:
---




Ao tentar executar a suite de testes em uma [Rails Engine](http://edgeapi.rubyonrails.org/classes/Rails/Engine.html) fui surpreendido com alguns erros, então partir para campo afim de 
descobrir o porquê das coisas não funcionarem convencionalmente como se esperava. Na página [Issues](https://github.com/rails/rails/issues?labels=engines&state=open)
 do Rails no [GitHub](https://github.com/) vi que se tratava de bugs do Rails mesmo, no meu caso a versão 3.2.8.
 
Como não era somente um bug para corrigir e não achei um post relacionando todos, depois da garimpada na net, resolvi juntar 
tudo e postar aqui.

Vou fazer um exemplo de execução de testes em uma Engine para exemplificar melhor. Nossa mega Engine vai se chamar 
Blog (nesse momento estou inspirado), vamos lá então.

<!-- more -->

>Não vou entrar em detalhes sobre Rails Engine, se você não tem noção nenhuma do que seria isso pode começar aqui: [http://www.akitaonrails.com/2010/05/10/rails-3-introducao-a-engines#.UIqn32lUMzE](http://www.akitaonrails.com/2010/05/10/rails-3-introducao-a-engines#.UIqn32lUMzE)  -  (Sto. @AkitaOnRails).

>Futuramente (quando a faculdade deixar) pretendo fazer um exemplo para documentar também, ai coloco o link aqui ;)

##Criando uma Rails Engine


Vamos criar nossa app Blog.

{% codeblock bash lang:bash %}
rails plugin new Blog --mountable
cd Blog
{% endcodeblock %}

*Vimos que criamos uma Engine isolada, dessa forma nossas classes serão englobadas no namespace Blog e criadas dentro de pastas nomeadas 
pelo namespace.*

![Estrutura de pastas](/images/bug_engine/estrutura.png)

Agora vamos gerar um Scaffold para ter o que testar ;P
{% codeblock bash lang:bash %}
rails g scaffold Post title body:text
{% endcodeblock %}

Em uma Engine, temos tarefas rake específicas prefixadas com "app", as que nos interessam nesse momento são as relativas ao banco de dados

{% codeblock bash lang:bash %}
rake -T db

rake app:db:create # Create the database from config/database.yml for the current Rails.env (use db:create:all to create all dbs in the config)
rake app:db:drop # Drops the database for the current Rails.env (use db:drop:all to drop all databases)
rake app:db:fixtures:load # Load fixtures into the current environment's database.
rake app:db:migrate # Migrate the database (options: VERSION=x, VERBOSE=false).
rake app:db:migrate:status # Display status of migrations
rake app:db:rollback # Rolls the schema back to the previous version (specify steps w/ STEP=n).
rake app:db:schema:dump # Create a db/schema.rb file that can be portably used against any DB supported by AR
rake app:db:schema:load # Load a schema.rb file into the database
rake app:db:seed # Load the seed data from db/seeds.rb
rake app:db:setup # Create the database, load the schema, and initialize with the seed data (use db:reset to also drop the db first)
rake app:db:structure:dump # Dump the database structure to db/structure.sql. Specify another file with DB_STRUCTURE=db/my_structure.sql
rake app:db:version # Retrieves the current schema version number
rake db:create # Create the database from config/database.yml for the current Rails.env (use db:create:all to create all dbs in the config)
rake db:drop # Drops the database for the current Rails.env (use db:drop:all to drop all databases)
rake db:fixtures:load # Load fixtures into the current environment's database.
rake db:migrate # Migrate the database (options: VERSION=x, VERBOSE=false).
rake db:migrate:status # Display status of migrations
rake db:rollback # Rolls the schema back to the previous version (specify steps w/ STEP=n).
rake db:schema:dump # Create a db/schema.rb file that can be portably used against any DB supported by AR
rake db:schema:load # Load a schema.rb file into the database
rake db:seed # Load the seed data from db/seeds.rb
rake db:setup # Create the database, load the schema, and initialize with the seed data (use db:reset to also drop the db first)
rake db:structure:dump # Dump the database structure to an SQL file
rake db:version # Retrieves the current schema version number
{% endcodeblock %}

Vamos criar nosso DB e executar as migrações

{% codeblock bash lang:bash %}
rake app:db:create
rake app:db:migrate

== CreateBlogPosts: migrating ================================================
-- create_table(:blog_posts)
-> 0.0015s
== CreateBlogPosts: migrated (0.0016s) =======================================
{% endcodeblock %}

>Observe que a tabela criada é prefixada pelo nome da Engine.

>Com o scaffold criamos toda a estrutura para os testes.

![Estrutura de pastas nos testes](/images/bug_engine/teste.png)

Enfim, vamos aos erros.


ERROS
-----

O scaffold já cria os testes funcionais para CRUD, veja o arquivo `test/functional/blog/posts_controller_test.rb`, 
dessa forma já podemos executar os testes e ver se está tudo funfando.


{% codeblock bash lang:bash %}
rake test
{% endcodeblock %}

>**Ops! deu pau ;(**

### 1) NoMethodError: undefined method `posts' for #<Blog::PostsControllerTest:0x007f942c045ed0>

Esse error acontece devido nosso controller tentar carregar as fixtures do post que não foram carregadas/criadas, veja:

{% codeblock ruby lang:ruby %}
module Blog
    class PostsControllerTest < ActionController::TestCase
        setup do
            @post = posts(:one)
        end
     end
end
{% endcodeblock %}

Você precisa explicitar isso para o [ActiveSupport::TestCase](http://api.rubyonrails.org/classes/ActiveSupport/TestCase.html), 
abra o arquivo `test/test_helper.rb` e adicione: `ActiveSupport::TestCase.fixtures :all` no contexto onde as fixtures são carregadas.


{% codeblock ruby lang:ruby %}
# Load fixtures from the engine
if ActiveSupport::TestCase.method_defined?(:fixture_path=)
    ActiveSupport::TestCase.fixture_path = File.expand_path("../fixtures", __FILE__)
    ActiveSupport::TestCase.fixtures :all
end
{% endcodeblock %}

Agora rode os testes;

{% codeblock bash lang:bash %}
rake test
{% endcodeblock %}

>**Ops! deu pau ;(**

###2) ActiveRecord::StatementInvalid: Could not find table 'blog_posts' 

Esse error é devido as convenções não funcionarem aqui, se você reparou o path das fixtures está setado para raiz da pasta fixtures, então vamos alterar.

{% codeblock ruby lang:ruby %}
# Load fixtures from the engine
if ActiveSupport::TestCase.method_defined?(:fixture_path=)
    ActiveSupport::TestCase.fixture_path = File.expand_path("../fixtures/blog", __FILE__)
    ActiveSupport::TestCase.fixtures :all
end
{% endcodeblock %}

Agora rode os testes;

{% codeblock bash lang:bash %}
rake test
{% endcodeblock %}

>**Ops! deu pau ;(**

###3) ActiveRecord::StatementInvalid: SQLite3::SQLException: no such table: posts: DELETE FROM "posts"

Como convenção, a fixture post tenta utilizar a tabela post, mais como vimos, estamos "namespaceados" pelo nome da engine, 
então precisamos novamente explicitar para o ActiveSupport::TestCase que o objeto utilizado pela fixture post é `Blog::Post`, 
dessa forma o [ActiveRecord](http://api.rubyonrails.org/classes/ActiveRecord/Base.html) referencia a tabela correta. 

Então adicione mais uma linha no seu test_helper.rb

{% codeblock ruby lang:ruby %}
# Load fixtures from the engine
if ActiveSupport::TestCase.method_defined?(:fixture_path=)
    ActiveSupport::TestCase.fixture_path = File.expand_path("../fixtures/blog", __FILE__)
    ActiveSupport::TestCase.fixtures :all
    ActiveSupport::TestCase.set_fixture_class :posts => Blog::Post
end
{% endcodeblock %}

Agora rode os testes;

{% codeblock bash lang:bash %}
rake test
{% endcodeblock %}

>**Ops! deu pau ;(**

###4) ActionController::RoutingError: No route matches {:id=>"980190962", :post=>{:body=>"MyText", :title=>"MyString"}, :controller=>"blog/posts", :action=>"update"}

Vamos observar o arquivo de rotas, `config/routes.rb`

{% codeblock ruby lang:ruby %}
BlogTest::Engine.routes.draw do
   resources :posts
end
{% endcodeblock %}

Realmente não temos a rota 'blog/posts' criada e não devemos criar, pois quando montamos nossa Engine em uma App mãe, 
essa rota será criada devido nossa Engine ser isolada (--mountable), então a App precisa diferenciar a requisição ao 
controller post da Engine da requisição do controller post dela mesma (caso tenha). 

Não sei se expliquei bem, mais é só para justificar o porquê de não alterar esse arquivo.

Para resolver isso, vamos disponibilizar diretamente ao controller_test as rotas. 
Em `test/functional/blog/posts_controller_test.rb` adicione:

{% codeblock bash lang:bash %}
setup do
    @post = posts(:one)
    @routes = Engine.routes
end
{% endcodeblock %}

Agora rode os testes;

{% codeblock bash lang:bash %}
rake test
{% endcodeblock %}

>**Congratulation!**

Agora nossa suite de teste esta rodando, já podemos trabalhar. :P


Tentei explicar as correções de forma que quem esteja começando no mundo Rails possa entender o que está acontecendo e não 
simplesmente copiar e colar o código para correção. Lembrando que também sou um aspirante Rails e estou aberto para correções 
neste post caso cometi alguma gafe.


##Referencias

- [https://github.com/rails/rails/issues/4971](https://github.com/rails/rails/issues/4971)
- [https://github.com/rails/rails/issues/6573](https://github.com/rails/rails/issues/6573)

##Mais sobre Engine
- [http://edgeguides.rubyonrails.org/engines.html](http://edgeguides.rubyonrails.org/engines.html)




