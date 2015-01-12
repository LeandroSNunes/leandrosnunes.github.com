---
layout: post
title: "adicionar programa no menu do debian"
date: 2012-05-05 21:07
comments: true
categories: [Linux, Debian]
tags: [Debian, menu, atalho, shortcut]
description: "Vamos adicionar um programa ao menu de aplicativos do Debian para facilitar a utilização e não ficar passando path para starta via shell."
keywords: "instalar programas no debian, instalar programas no Linux, adicionar programa no menu do debian, utilizar o shell para executar programas, adicionar software menu debian, selecionar icones para programas, personalizar o menu"
---
<p>
Hoje instalando um software via terminal do Debian senti a necessidade de adicioná-lo ao menu de aplicativos para facilitar a utilização e não  ficar passando path para starta via shell.
</p>

<p>
Nesse tutorial vamos instalar o <a href="http://www.scilab.org/">Scilab</a>. Para outros aplicativos somente precisamos trocar o path do executável e do ícone.
</p>
<!-- more -->

<h2>Adicionando…</h2>
<p>
<b>1)</b> Clique com o botão direito na barra de menu e escolha a opção <em>Edit menus</em>
</p>
{% img /images/menu-debian/step1.jpg 789 %}

<p>
<b>2)</b> Na tela que se abre, ecolha na aba Menus no lado esquerdo onde irá inserir o programa e depois clique em <em>New Item</em>.
</p>
{% img /images/menu-debian/step2.jpg 789 %}
<p>
<b>3)</b> Agora vamos inserir as informações necessárias
</p>
<ul>
	<li>Type:          Application</li>
	<li>Name:          Scilab</li>
	<li>Command:  <code>/opt/scilab-5.3.3/bin/scilab</code></li>
	<li>Comment:  Qualquer coisa</li>
</ul>
{% img /images/menu-debian/step3.jpg 789 %}
<p>
<b>4)</b> Para trocar o ícone, clique nele e selecione uma imagem em  /opt/scilab-5.3.3/share/scilab/icons
{% blockquote %}
Por default o Debian abre na paste de ícones padrão.
{% endblockquote %}
</p>
{% img /images/menu-debian/step4.jpg 789 %}
<p>
<b>5)</b> Pronto! está lá nosso atalho para o Scilab inserido no menu. Acesse e confira pois o Debian não deixou eu dar um print screen com o menu aberto. :P
</p>

