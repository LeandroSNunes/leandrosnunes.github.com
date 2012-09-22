---
layout: post
title: "Instalando o Scilab no Windows"
date: 2012-02-22 20:40
comments: true
categories: [Matemática, Softwares]
tags: [Scilab, Windows, Instalação]
description: "Baixando e instalando o Scilab no Windows"
keywords: "instalar o scilab, variáveis de console, utilizar o pronpt, sistemas operacionais que rodam o scilab, instalar scilabian no windows, instalação e configuração, install scilab, download do scilab, baixar o scilab, conhecer o software,  baixar versão 32 bits, baixar versão 64 bits"
---
{% img /images/scilab-windows/banner.jpg 789 "Banner informativo para download no site scilab.org" %}
<p>
Instalar o Scilab no Windows é super simple, acesse o site oficial <a href="http://www.scilab.org" title="Site oficial do Scilab para download da ferramenta e tutoriais">www.scilab.org</a> para baixar a última versão.
</p>

<!-- more -->

<p>
Clique em download para baixar a versão de 32bits ou selecione Other Systems para baixar para a versão de 64bits ou para outro OS.
</p>

<h3>Instalação</h3>
<p>
1) Ao terminar o download, execute o arquivo para iniciar a instalação. Na tela que se abre, clique em Run.
</p>

{% img /images/scilab-windows/step2.jpg 789 %}

<p>
2) Agora escolha o idioma que será utilizado durante a instalação.
</p>
{% img /images/scilab-windows/step3.jpg 789 %}

<p>
3) Chegamos no assistente, então, clique em avançar.
</p>
{% img /images/scilab-windows/step4.jpg 789 %}

<p>
Daqui pra frente, segue-se o modo de instalação padrão do windows: avançar, avançar e avançar. <br />

4) Após a instação o Scilab abrirá o seu Console:
</p>

{% img /images/scilab-windows/console.jpg 789 %}

{% blockquote %}
Este iremos estudar no próximo post.
{% endblockquote %}


<h3>Finalizando</h3>
<p>
Como de costume após uma instação, vamos imprimir a famosa frase “Alô Mundo” e testar se está tudo ok, para isso, no pronpt do 
console <code> –> </code> digite o seguinte comando:
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

<a href="http://leandronunes.com/blog/2012/02/22/instalando-o-scilab-no-linux/">Para saber como instalar o Scilab em ambiente windows clique aqui.</a>
No site  do Scilab existe versões de instalação para MacOSX, Linux e Windows ambas de distribuição <a href="http://www.gnu.org/licenses/license-list.html" title="Ir para outro site" target="_blank">free</a>, 
lembrando que seu uso está sobre a licença <a href="http://www.cecill.info/" title="Ir para outro site" target="_blank">CeCILL (GPL compatible)</a>. Você pode usar, modificar e / ou redistribuir o software sob os termos da licença.


