---
layout: post
title: "Implementando aplicações Rails com Capistrano"
date: 2012-04-06 17:37
comments: true
categories: [Rails]
tags: [capistrano, deploy, gem, Ruby on Rails]
description: "Vamos ver como a dupla sertaneja Rails & Capistrano soam bem na hora de implantar a aplicação"
keywords: "app rubyonrails, criar sistema em rails, configurar capistrano, tutorial para dar deploy em aplicações rails, utilizar o servidor linode, configurar repositório git no capistrano, aplicações rails, configurar mysql em produção, configurar assets pipeline precompile com capistrano, utilizar gem capistrano, instalar capistrano no rubyonrails, criar tarefas rake no capistrano"
---

{% img /images/post-capistrano.jpg 789 %}


<p>
Chegou a hora de implantar a aplicação para os primeiros testes do cliente?<br />
Vamos ver como a dupla sertaneja <a href="http://rubyonrails.org/" title="Framework em Ruby para desenvolvimento de aplicações web">Rails</a> & <a href="http://capistranorb.com/">Capistrano</a> soam bem nesta tarefa.
</p>
<!-- more -->
<p>
O Capistrano é uma gem para implantar (deploy) aplicações web. Inicialmente desenvolvida para Rails, 
tem a finalidade de enviar seu código fonte para um servidor web e permitir o versionamento, ou seja, 
te dando super poderes para voltar a versão anterior do seu código caso algo dê errado na versão atual. 
Com um pouquinho de configuração o Capistrano pode ser utilizado para outros framework/linguagem. 
</p>

<p>
Vamos adimitir que você já tenha conhecimento em Sistemas de Controle de Versão (<a href="http://github.com/" title="Sistema de controle de versão distribuído">Git</a>, <a href="http://subversion.apache.org/" title="Sistema de controle de versão centralizado" >SVN</a>, etc) e que seu 
ambiente de desenvolvimento esteja funfando com a aplicação.
</p>

<em>Hoje foi o meu primeiro contato com o Capistrano, abaixo segue minhas descobertas.</em>

<h2>Requisitos</h2>

<p>
Para executar esta tarefa temos que verificar em nossa maleta de ferramentas se está presente:
</p>
<ul>
	<li><a href="http://www.ruby-lang.org/en/">Ruby</a></li>
	<li>Uma aplicação (no meu caso em Rails)</li>
	<li>Servidor Web com suporte a SSH</li>
	<li>Terminal (bash, sh, etc…)</li>
	<li>Um repositório (No meu caso GIT)</li>
</ul>

<h2>Instalação</h2>
<p>
Como citado, o Capistrano é um gem e com isso você pode instalar do jeito tradicional mesmo. <br />
É, aquele fácil! Fazemos assim:
</p>
{% codeblock bash lang:bash %}
gem install capistrano
{% endcodeblock %}
<p>
O capistrano adiciona alguns utilitários de linha de comando, o <code>cap</code> e <code>capify</code> iremos conhecer mais abaixo.
Também é possível adicionar esta gem no arquivo GemFile do seu projeto.
</p>
{% codeblock ruby lang:ruby %}
gem 'capistrano' 
{% endcodeblock %}
<p>
e depois executar o <code>bundle install</code> em seu terminal.
</p>

<h2>Iniciando</h2>
<p>
Agora utilizamos o <code>capify</code> , nosso amigo de linha de comando para fazer com que o Capistrano monitore nossa work area e nos dê opções para configurá-lo. Primeiramente temos que estar na raiz do diretório do nosso projeto.
</p>
{% codeblock bash lang:bash %}
cd /app_rails/
{% endcodeblock %}
<p>Agora é só executar!</p>
{% codeblock bash lang:bash %}
capify .
{% endcodeblock %}
<p>Com isso ganhamos dois arquivos em nosso projeto, são eles:</p>
<ul>
	<li><b>capifile</b> –> Possui referências as bibliotecas usadas pelo Capistrano e é responsável em carregá-las.</li>
	<li><b>config/deploy.rb</b> –> Este possui todas diretrizes para configurar sua implantação, geralmente precisamos editar apenas este.</li>
</ul>

<h2>Confirgurações</h2>
<p>
O Capistrano precisa de algumas informações para que tudo funcione conforme esperamos e isso fazemos editando o arquivo config/deploy.rb. Estou usando um projeto fictício apenas para exemplo, fique atendo as informações que devem ser substituidas conforme as suas configurações.
<br />Vamos lá então!
</p>
<h3>A) Informações do projeto</h3>
{% codeblock ruby lang:ruby %}
set :application, "app_rails"       # O nome do projeto
set :keep_releases, 5               # Isso guardar os 5 últimos deploys
set :rails_env,     "production"    # O ambiente em que o Rails irá atuar
{% endcodeblock %}
<p>
Toda vez que adicionarmos uma nova funcionalidade ao projeto iremos implementá-la no servidor para que outros tenham acesso e nesse processo, o Capistrano faz o versionamento mantendo seu release anterior. Na linha 2 dizemos que será mantido as 5 últimas atualizações, assim caso algo saia errado neste novo release, podemos usar um comandinho salvador da pátria.
</p>
{% codeblock bash lang:bash %}
cap rollback
{% endcodeblock %}
<p>com isso voltamos ao release que estava funfando.</p>

<h3>B) SCM (source code manager)</h3>
{% codeblock ruby lang:ruby %}
set :scm, 'git'
set :repository,  "git@github.com:LeandroSNunes/app_rails.git"
set :branch, 'master'
set :deploy_via, :remote_cache
{% endcodeblock %}
<p>
No meu caso uso Git como repositório e informo na linha 1. A linha 4 é informado ao Capistrano que ele não precisa pegar todo repositório toda vez que subirmos um novo release, ele somente irá buscar as modificações.
</p>
<h3>C) Informações do servidor</h3>
{% codeblock ruby lang:ruby %}
default_run_options[:pty] = true
ssh_options[:forward_agent] = true
set :user, "leandro"
set :use_sudo, false
server "leandronunes.com.br", :web, :app, :db, :primary => true
set :deploy_to, "/www/app_rails.leandronunes.com.br/#{application}"
{% endcodeblock %}
<p>
Na linha 2 permitimos que o SSH carregue nossa chave para o servidor para que de lá seja acessado o github.<br />
O usuário informado precisa ter permissões de escrita no servidor.<br />
Alguns comandos por default são executados com sudo, porém se tiver usando um servidor compartilhado e que não tiver acesso de superusuário, informe para não utilizar o sudo como na linha 4.<br />
O Capistrano entende que sua aplicação irá fazer muito sucesso e que seja preciso distribuí-la em vários servidores, no meu caso estou enviando tudo para o mesmo lugar então a linha 5 é uma forma otimizada de informar isso.
</p>
<p>Caso precise redirecionar serviços para outros servidores, você pode substituir a linha 5 por:</p>
{% codeblock ruby lang:ruby %}
role :web, 'dominio'
role :app, 'dominio'
role :db,  'dominio', :primary => true
{% endcodeblock %}

<h2>Ops! Executando.</h2>
<p>Agora é a hora! Vamos implantar nossa aplicação no servidor, começamos verificando se tudo está OK.</p>
{% codeblock bash lang:bash %}
cap deploy:check
{% endcodeblock %}
<p>Tudo correndo bem, você será informado. Seguindo, criaremos a estrutura de pastas no servidor.</p>
{% codeblock bash lang:bash %}
cap deploy:setup
{% endcodeblock %}
<p>
O que fizemos? Com isso será criado duas pastas (<em>release</em> e <em>shared</em>) e ainda um link simbólico chamado <em>current</em> apontando para o último release. Na pasta releases é criado uma pasta para o deploy efetuado. A pasta shared serve para armazenar arquivos que serão compartilhados entre os releases, como logs, imagens, arquivos de configuração, etc…
</p>
{% blockquote %}
Quando trabalhamos com Rails, Git e mais de um desenvolvedor é de prática adicionar nosso arquivo database.yml ao .gitignore, fazendo com que este não seja monitorado e conseguentemente não enviado para o repositório, assim configuramos um único arquivo de banco de dados e colocamos na pasta shared para que todos releases compartilhem a mesma configuração. Mais abaixo vamos adicionar uma tarefa pra fazer isso.
{% endblockquote %}

<h3>A) Criando tarefas para o Capistrano</h3>
{% codeblock ruby lang:ruby %}
namespace :deploy do
#   task :start do ; end
#   task :stop do ; end
   task :restart, :roles => :app, :except => { :no_release => true } do
     run "touch #{current_path}/tmp/restart.txt"
	 end
   task :database, :roles => :app do
     run "cp #{deploy_to}/#{shared_dir}/database.yml #{release_path}/config/"
   end
   task :permission, :roles => [:web, :db, :app] do
     run "chmod 755 #{release_path}/public -R" 
   end
end 

namespace :assets do
    task :symlink, :roles => :app do
      assets.create_dir
		  run <<-CMD
		    rm -rf  #{release_path}/public/images/upload &&
		    ln -nfs #{shared_path}/upload #{release_path}/public/images/upload
		  CMD
    end
		task :create_dir, :roles => :app do
		  run "mkdir -p #{shared_path}/upload"
		end
end

after "deploy:assets:symlink", 'deploy:database' 
after "deploy:update_code", 'deploy:permission'
{% endcodeblock %}

<p>
Ao concluir o deploy é necessário reiniciar o servidor para que as novas configurações entrem em vigor, podemos fazer isso simplesmente atualizando o arquivos <em>tmp/restart.txt</em>, percebam na tarefa restart.
</p>
<p>
Acima adicionamos nosso arquivo database.yml em nossa pasta shared então precisamos adicioná-la dentro do release corrente após o deploy, a tarefa <em>database</em> faz exatamente isso.
</p>
<p>
Na tarefa permissions setamos as permissões necessárias para pasta public.
</p>
<p>
Depois, resolvemos o problemas de aquivos de imagens adicionados pelos usuários (podemos supor um cadastro de produtos). As tarefas dentro de <em>assets</em> criam uma pasta upload dentro de shared e um link simbólico em <em>public/imagens</em> apontando para esta pasta.
</p>


<p>Por fim vamos executar o tão esperado deploy.</p>
{% codeblock bash lang:bash %}
cap deploy:cold
cap deploy
{% endcodeblock %}

<p>
Rodamos o <code>cap deploy:cold</code> primeiro para colocar os arquivos no servido, depois efetuamos o deploy.<br />
A partir da segunda implantação não é necessário rodar <code>cap deploy:cold</code><br />
Se precisar migrar seu banco utilize <code>cap deploy:migrations</code><br />
Alguma coisa fugiu do controle? volte ao release anterior com <code>cap deploy:rollback</code><br />
Para mais informações <code>cap -T</code> para visualizar todas as tarefas do Capistrano.
</p>

<h2>Juntando tudo</h2>
{% codeblock ruby lang:ruby %}
# Projeto
set :application, "app_rails"       # O nome do projeto
set :keep_releases, 5               # Isso guardar os 5 últimos deploys
set :rails_env,     "production"    # O ambiente em que o Rails irá atuar
 
# SCM
set :scm, 'git'
set :repository,  "git@github.com:LeandroSNunes/app_rails.git"
set :branch, 'master'
set :deploy_via, :remote_cache
 
# Servidor
default_run_options[:pty] = true
ssh_options[:forward_agent] = true
set :user, "leandro"
set :use_sudo, false
server "leandronunes.com.br", :web, :app, :db, :primary => true
set :deploy_to, "/www/app_rails.leandronunes.com.br/#{application}"
 
#Tarefas
namespace :deploy do
#   task :start do ; end
#   task :stop do ; end
   task :restart, :roles => :app, :except => { :no_release => true } do
     run "touch #{current_path}/tmp/restart.txt"
	 end
   task :database, :roles => :app do
     run "cp #{deploy_to}/#{shared_dir}/database.yml #{release_path}/config/"
   end
   task :permission, :roles => [:web, :db, :app] do
     run "chmod 755 #{release_path}/public -R" 
   end
end 

namespace :assets do
    task :symlink, :roles => :app do
      assets.create_dir
		  run <<-CMD
		    rm -rf  #{release_path}/public/images/upload &&
		    ln -nfs #{shared_path}/upload #{release_path}/public/images/upload
		  CMD
    end
		task :create_dir, :roles => :app do
		  run "mkdir -p #{shared_path}/upload"
		end
end

after "deploy:assets:symlink", 'deploy:database' 
after "deploy:update_code", 'deploy:permission'
{% endcodeblock %}

<h2>Então...</h2>
<p>
Isso foi minha colheita em um dia de deploy aqui na empresa e basicamente quando for efetuar um deploy da sua aplicação você vai estar neste ciclo novamente:
</p>
<ul>
	<li>Adicionar seu último release em um repositório</li>
	<li>Iniciar o monitoramento do Capistrano no diretório de sua aplicação</li>
	<li>Editar o arquivo de configuração do Capistrano</li>
	<li>Fazer um confere das configurações no servidor</li>
	<li>Montar a estrutura de pastas no servidor</li>
	<li>Fazer deploy de sua aplicação</li>
	<li>Configurar o ambiente de produção</li>
	<li>Cruzar os dedos e atualizar seu navegador</li>
</ul>
<p>
Algumas informações tive acesso dando umas googladas e dois blogs que usei de referências foram 
<a href="http://blog.dmitrynix.com/deploy-capistrano/" >Unix and Me</a> e <a href="http://objetiva.co/blog/2008/06/25/capistrano-com-git-tutorial-bsico" >Objetiva</a>, acessem para visualizarem o conteúdo completo.
</p>
<p>
Para mais informações
<a href="https://github.com/capistrano/capistrano/wiki/_pages">https://github.com/capistrano/capistrano/wiki/_pages</a>
</p>

