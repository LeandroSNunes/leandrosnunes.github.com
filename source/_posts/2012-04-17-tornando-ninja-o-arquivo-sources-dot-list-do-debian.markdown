---
layout: post
title: "Tornando ninja o arquivo sources.list do Debian"
date: 2012-04-17 22:30
comments: true
categories: [Linux,Debian]
tags: [Debian, aptitude, apt-get, Linux, repositório]
description: "Vamos então adicionar mais repositórios no arquivo sources.list arquivo para que nossos amigos (APT e APTITUDE) achem o que precisamos com maior facilidade."
keywords: "Debian, Linux, apt-get, aptitude, sources.list, repositório, update, atualização"
---
{% img /images/source-list/banner.jpg 789 %}
<p>
Para que o uso do computador faça sentido, precisamos instalar softwares que satisfaçam nossas necessidades. Para lidar com esse problema, o Debian possui ferramentas para instalar e atualizar pacotes e suas dependências de maneira rápida e prática. Nossos amigos são o APT e o APTITUDE.
</p>
<!-- more -->
<p>
Quando utilizamos essas ferramentas para instalar pacotes, elas fazem uma consulta no arquivo <em>sources.list</em> que geralmente estar em <em>/etc/apt/</em> e este arquivo possui uma lista de repositórios onde irá ser efetuada a busca atrás do pacote solicitado.
</p>

<p>
Vamos então adicionar mais repositórios neste arquivo para que nossos amigos (APT e APTITUDE) achem o que precisamos com maior facilidade.
</p>

<p>
O Jonhnatha Trigueiro mantem o site <a href="http://goo.gl/L0H76" title="Ir para o site">Debian Sources List</a> Generator que é uma mão na roda para esta tarefa. Vamos ver como tudo funciona.
</p>

<h2>Selecionado repositórios</h2>
<p>
<a href="http://goo.gl/L0H76">Acessem o site clicando aqui..</a> Na página inicial possuímos 5 passos para gerar um arquivo <em>sources.list</em> customizado, vamos seguílos.
</p>

<h3>1 – Select your country</h3>
{% img /images/source-list/step1.png 789 %}
<p>
Escolhemos de qual país serão nossos repositórios.
</p>

<h3>2 – Select your release</h3>
{% img /images/source-list/step2.png 789 %}
<p>
Para qual versão do Debian.
</p>

<h3>3 – Debian Branches</h3>
{% img /images/source-list/step3.png 789 %}
<p>
Repositórios disponíveis. Temos duas opções para cada um sendo que a que possui Sources Repository armazena código fonte para desenvolvedores.
</p>

<h3>4 – Debian Updates</h3>
{% img /images/source-list/step4.png 789 %}
<p>
O títudo é bem intuitivo!
</p>

<h3>5 – 3rd Parties Repos</h3>
{% img /images/source-list/step5.png 789 %}
<p>
Repositórios adicionados por terceiros. Esta lista é bem maior e varia de acordo com o release escolhido.
</p>

<h2>Gerando o arquivo</h2>
<p>
Depois das escolhas, clique em <strong>Generate List</strong>
Uma lista com todos os repositórios escolhidos é gerada, então abra o arquivo sources.list e substitua seu conteúdo.
</p>
{% codeblock bash lang:bash %}
sudo vim /etc/apt/sources.list
{% endcodeblock %}
<p>
Para os repositórios de terceiro é necessário adicionar a chave publica ssh para que seja efetuada a busca, observe a lista que o comando para esta tarefa já está incluído.
</p>
{% img /images/source-list/step6.png 789 %}
<p>
É só copiar e executar no terminal!
</p>


<h2>Atualizando a distro!</h2>
{% codeblock bash lang:bash %}
sudo apt-get update
sudo apt-get upgrade
sudo aptitude update
{% endcodeblock %}

<h2>Então...</h2>
<p>
Espero que estas informações sejam úteis, principalmente para os que como eu, estão começando.
Dêem uma navegada em <a href="http://goo.gl/L0H76" title="Ir para o site">Debian Sources List</a> , temos a possibilidade de adicionar repositórios também e contribuir para a comunidade open source.
</p>
