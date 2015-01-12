---
layout: post
title: "Instalando o Scilab no Linux"
date: 2012-02-22 20:01
comments: true
categories: [Matemática, Softwares]
tags: [Scilab, Linux, Debian, Instalação]
description: "Baixando e instalando o Scilab no Linux usando o terminal"
keywords: "instalar o scilab, variáveis de console,utilizar o pronpt, instalar no linux o scilab, sistemas operacionais que rodam o scilab, instalar scilabian no debian, instalação e configuração, install scilab, download do scilab, baixar o scilab, conhecer o software, baixar pelo apt-get, baixar pelo aptitude, pesquisar scilab com apt-cache, baixar versão 32 bits, baixar versão 64 bits"
---
Para instalar o Scilab no Linux vamos fazer uso do nosso terminal, assim fica bem simples. <br />
A minha instalação foi efetuada no Debian 6 squeeze 64bits.

<!-- more -->

<h3>Instalação</h3>

Temos versões do Scilab para 32bits e 64bits. A instalação é igual mudando apenas o link para o download. Fazemos assim:

<b>1)</b> Acessando a pasta onde queremos efetuar a instalação, no meu caso  <code> /opt </code>
{% codeblock bash lang:bash %}
cd /opt
{% endcodeblock %}

<b>2)</b> Baixando o binário na versão do nosso sistema.
{% codeblock bash lang:bash %}
# 32 bits
wget http://www.scilab.org/download/5.3.3/scilab-5.3.3.bin.linux-i686.tar.gz
 
#64 bits
wget http://www.scilab.org/download/5.3.3/scilab-5.3.3.bin.linux-x86_64.tar.gz
{% endcodeblock %}

{% blockquote %}
Vou seguir utilizando a verão para 64 bits.
{% endblockquote %}

<b>3)</b> Vamos descompactá-lo então
{% codeblock bash lang:bash %}
tar fzxv scilab-5.3.3.bin.linux-x86_64.tar.gz
{% endcodeblock %}

<p>
Pronto! Scilab instalado.
</p>

<h3>Utilizando</h3>
<p>
Para executá-lo é so digitar
</p>
{% codeblock bash lang:bash %}
/opt/scilab-5.3.3/bin/scilab
{% endcodeblock %}

<p>
Para facilitar, podemos adicionar o Scilab no menu do Debian, é só dar uma olhada <a href="http://goo.gl/bmTol" title="Ir para o post">aqui</a> para saber como.
<br />
Com o Scilab em execução, vamos imprimir a famosa frase “Alô Mundo” e testar se está tudo ok, para isso, no pronpt do console do Scilab  <code> –> </code> digite o seguinte comando:
</p>
{% codeblock bash lang:bash %}
disp("Alô Mundo");
{% endcodeblock %}

<p>
Agora para encerrar e aguardarmos o próximo post digite:
</p>

{% codeblock bash lang:bash %}
exit
{% endcodeblock %}

<h3>Informações Úteis</h3>

<a href="http://leandronunes.com/blog/2012/02/22/instalando-o-scilab-no-windows/">Para saber como instalar o Scilab em ambiente windows clique aqui.</a>
No site  do Scilab existe versões de instalação para MacOSX, Linux e Windows ambas de distribuição <a href="http://www.gnu.org/licenses/license-list.html" title="Ir para outro site" target="_blank">free</a>, 
lembrando que seu uso está sobre a licença <a href="http://www.cecill.info/" title="Ir para outro site" target="_blank">CeCILL (GPL compatible)</a>. Você pode usar, modificar e / ou redistribuir o software sob os termos da licença.





