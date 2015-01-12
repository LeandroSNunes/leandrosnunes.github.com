---
layout: post
title: "Scilab Help, torne-se amigo dele!"
date: 2012-02-20 22:27
comments: true
categories: [Matemática, Softwares]
tags: [scilab, help, ajuda]
description: "O Help do Scilab é um dos componentes mais importante para o início do aprendizado da ferramenta, para consulta de sintaxe e pesquisa de funções. Vamos conhecê-lo? "
keywords: "utilizando a ajuda do scilab, buscar comando no help do scilab, como utilizar a ajuda do scilab, aprender a criar gráficos no scilab, executar comandos no console do scilab, pesquisar sobre funções, utilizar o menu"
---
<p>
O <a href="http://www.scilab.org/" title="Site oficial do Scilab para download da ferramenta e tutoriais">Scilab</a> possui um componente de help (ajuda) que pode ser utilizado como um contato inicial e sempre que houver dúvidas de sintaxe 
de comandos, nomes de funções, etc… Quem está começando a manipular o Scilab, é válido dar uma navegada no help para conhecer seu conteúdo e 
ter uma ideia de onde procurar o que necessitar.
</p>

<!-- more -->

<h2>Meios de acesso ao help</h2>
<p>
Podemos acessar o help de 4 formas:<br /><br />

<b>1)</b> Pelo menu principal:
</p>
{% img /images/scilab-help/menu-help.jpg 789 'Acessando o help do Scilab pelo menu principal' %}

<p>
2) Pela tecla F1
</p>
<p>
3) Pelo ícone na barra de ferramentas:
</p>
{% img /images/scilab-help/barra-ferramentas.jpg 789 'Acessando o help do Scilab pelo ícone localizado na barra de ferramenta' %}

<p>
4) Pelo comando help executado no prompt
</p>
{% codeblock bash lang:bash %}
help
{% endcodeblock %}
<p>
O comando help aceita um termo para pesquisar como parâmetro
</p>
{% codeblock bash lang:bash %}
help plot2d
{% endcodeblock %}

<h2>CONHECENDO A INTERFACE</h2>
<p>
Ao abrir a janela de help podemos na coluna da esquerda navegar pela relação de todos os grupos dos elementos, começando com o 
manual da ferramenta e a classificação por assunto dos itens. A cada grupo, o sistema mostra a relação de itens que o compôem.
</p>
<p>
Também temos uma aba para buscas.
</p>
{% img /images/scilab-help/help.png 789 "Janela do help do Scilab para consulta" %}

<p>
Do lado direito temos a descrição do item selecionado a esquerda.<br />
Reparem na parte dos exemplos, existe dos botões muito úteis.
</p>
{% img /images/scilab-help/botoes-exemplos.jpg 789 "Visualizando os exemplos e executando com o botôes de atalho" %}

<p>
O da esquerda executa o código do exemplo direto no console.<br />
O da direitra abre o código no editor (SciNotes)
</p>

<h2>Concluíndo</h2>
<p>
Este post foi uma passada rápida por um dos componentes mais utilizado. ;P
</p>

