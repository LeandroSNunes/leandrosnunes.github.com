---
layout: post
title: "Habilitando Apache com Virtual Host no Mac OS X 10.8"
date: 2013-01-20 00:24
comments: true
categories: [Apache]
tags: [Apache, Apple, Mountain Lion, OSX, Virtual Hosts]
description: "Na versão do Mac OS X 10.8 foi removido a opção 'Web Sharing' do painel de controle, assim, vamos configurar o Apache de outra maneira."
keywords: "configuração do Apache, Apache web server, criando virtual hosts, desenvolvimento com wordpress, configurando Apache no mac, mac web sharing, servirdor apache mac, configurando domínios no mac"
---
Na versão do Mac OS X 10.8 (Mountain Lion) foi [removido](http://support.apple.com/kb/HT5230?viewlocale=pt_BR) a opção "Web Sharing" do painel de controle "Sharing" em System Preferences, dessa forma a configuração se dá
de outra forma, basicamente usando o terminal com `sudo` ou usuário `root` e um editor de texto que pode ser o [Vim](http://www.vim.org). Vamos as configurações.
<!-- more -->
##Habilitando o Apache
Para iniciar:

{% codeblock bash lang:bash %}
sudo apachectl start
{% endcodeblock %}

Para parar:

{% codeblock bash lang:bash %}
sudo apachectl stop
{% endcodeblock %}

Para reiniciar:

{% codeblock bash lang:bash %}
sudo apachectl restart
{% endcodeblock %}

##Adicionando seu domínio
É necessário adicionar seu domínio no arquivo `/etc/hosts`, inclua a seguinte linha:
{% codeblock bash lang:bash %}
127.0.0.1 leandronunes.com
{% endcodeblock %}
 O domínio leandronunes.com é só como exemplo, no lugar pode ser o seu ;-P

##Arquivo httpd.com
Agora temos de fazer algumas configurações no próprio Apache.

Para habilitar o PHP, descomente a linha 117
{% codeblock bash lang:bash %}
LoadModule php5_module libexec/apache2/libphp5.so
{% endcodeblock %}

Configure seu document root na linha 169
{% codeblock bash lang:bash %}
DocumentRoot "myPath"
{% endcodeblock %}

e na linha 196
{% codeblock bash lang:bash %}
<Directory "myPath">
{% endcodeblock %}

Informe os arquivos default que o Apache irá ler na linha 231
{% codeblock bash lang:bash %}
DirectoryIndex index.html index.php
{% endcodeblock %}

Agora, descomente a linha 477 para incluir o arquivo de hosts virtuais
{% codeblock bash lang:bash %}
Include /private/etc/apache2/extra/httpd-vhosts.conf
{% endcodeblock %}


##Criando um host virtual
Vamos adicionar nosso host no arquivo `/private/etc/apache2/extra/httpd-vhosts.conf`, ficando dessa seguinte forma:

{% codeblock bash lang:bash %}
NameVirtualHost *:80

<VirtualHost *:80>
  ServerAdmin seuemail@email.com
  DocumentRoot "/Users/leandronunes/"
  ErrorLog "/private/var/log/apache2/error_log"
  CustomLog "/private/var/log/apache2/access_log" common
</VirtualHost>

<VirtualHost *:80>
  ServerAdmin seuemail@email.com
  DocumentRoot "/Users/leandronunes/blog"
  ServerName leandronunes.com
  ServerAlias www.leandronunes.com
  ErrorLog "/Users/leandronunes/blog/log/error_log"
  CustomLog "/Users/leandronunes/blog/log/access_log" common
</VirtualHost>

{% endcodeblock %}

##Testando
Primeiro, dê um restart no Apache, isso já foi mostrado mais acima.
Adicione um arquivo `index.php` na raíz de seu projeto com o conteúdo.
{% codeblock bash lang:php %}
<?php phpinfo(); ?>
{% endcodeblock %}

e acesse pelo browser `http://leandronunes.com`

Pronto, acredito que apareceu alguma coisa nesse momento. :-P

##Conclusão
Dessa forma simulamos os paths reais da aplicação em produção e eliminamos muitas dores de cabeça, principalmente para quem cria blogs em wordpress. ;-/
