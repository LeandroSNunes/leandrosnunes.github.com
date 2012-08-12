---
layout: post
title: "Scilab, conceitos básicos"
date: 2012-02-21 23:57
comments: true
categories: [Matemática, Softwares]
tags: [scilab, help, ajuda]
description: "Para quem está iniciando na ferramenta, este post servirá para entender alguns conceitos"
keywords: "scilab, comandos, instruções, carregar arquivos, configurar o path, iniciar outras janelas, console, pronpt"
---

<p>
A melhor maneira de se aprender algo é colocando a mão na massa. <br />
Vamos conhecer neste post o Console do <a href="http://www.scilab.org/">Scilab</a> que é o pontapé inicial para entender
 o software. Nele encontra-se o pronpt <code> –> </code> onde podemos executar comandos, instruções, carregar arquivos, configurar o path, iniciar outras janelas, etc…
</p>

<!-- more -->

{% img /images/scilab-conceitos/janela-console.png 789 %}

<h2>Conhecendo o console</h2>
<p>
Vamos conhecer alguns itens do menu que poderão ser úteis e a execução de algumas tarefas. Percorrendo o menu temos:
</p>
{% img /images/scilab-conceitos/menu.png 789 %}

<p>

<ul>
  <li>File:</li>
    <ul>
      <li>Execute: Executa arquivos de scripts e com instruções de comando</li>
      <li>Open a File: Carrega arquivos de textos criados no ambiente do Scilab (.sce e .sci)</li>
      <li>Change Current Directory: Muda o diretório atual do ambiente para um novo local,  comando: <code> –>chdir(‘novaPasta’); </code> no prompt.</li>
      <li>Display Current Directory: Mostra o diretório atual, <code> –>pwd; </code> no prompt.</li>
    </ul>
  <li>Edit</li>
    <ul>
      <li>No menu editar temos as opções padrão que dispensa comentários.</li>
    </ul>
  <li>Preferences</li>
    <ul>
      <li>Clear Console: Limpa o console mais não apaga os dados da memória, podemos executar esta função de outras duas formas: a) Apertando a tecla F2 ou  b) digitando <code> –>clc; </code> no prompt</li>
    </ul>  
  <li>Control</li>
    <ul>
      <li>Resume: Continua a execução após um pause ou um stop</li>
      <li>Abort: Interromp a execução</li>
      <li>Interrupt: Entra no modo de pause, podemos fazer <code> –>Ctrl+C; </code> no prompt</li>
    </ul>
  <li>Applications</li>
    <ul>
      <li>SciNotes: O editor de textos, podemos chamá-lo via prompt com o comando <code> –>editor; </code></li>
    </ul>
  <li>?</li>
    <ul>
      <li>Scilab help: Abre o componente de ajuda.</li>
      <li>Scilab Demonstrations: Exibe a janela de demonstrações de comandos do Scilab</li>
    </ul>
</ul>
</p>

<h2>Digitando comandos</h2>
<p>
Comandos são palavras-chave do Scilab que executam determinadas tarefas e instruções são um conjunto de 
palavras-chave em ordens digitadas no prompt e finalizadas com <code> <Enter> </code>.
</p>
{% codeblock bash lang:bash %}
y=ode(1.e-8*[1;1;1],0,0:0.005:50,'loren');
# O console permite várias instruções na mesma linha 
# separadas por ponto e vírgula.
a = 10; b = 5; c = 4+3
# O console sempre retorna  o valor do último processamento
{% endcodeblock %}

<p>
O uso de <code> ; </code> (ponto e vírgura) no final de cada instrução é opcional, a diferença é que se usarmos a saída será omitida.
</p>

<p>
Muitas vezes quando trabalhamos no editor e executamos um script, percebe-se que não aconteceu nada e aparentemente o 
Scilab está travado, na verdade, o console pode estar esperando uma interação sua para continuar a exibir as 
informações, isso ocorre devido que o retorno de algumas funções ultrapassam a area visível do console.
</p>

{% img /images/scilab-conceitos/interacao.png 789 %}

<p>
então é sempre bom fazer uso do <code> ; </code> (ponto e virgula) no final de cada instrução.<br />

Caso não lembre o nome do comando, você pode utilizar <code> Ctrl+Espaço </code> que aparece uma lista de comando similares.
</p>

{% img /images/scilab-conceitos/auto-complete.jpg 789 %}

<h2>Conhecendo erros</h2>
<p>
Para os comandos não executados corretamente uma mensagem de erro será exibida. Exemplo:
</p>
{% codeblock bash lang:bash %}
!--error 4
Undefined variable: e
{% endcodeblock %}

<p>
Podemos exibir a lista de todos os erros gerados pelo Scilab
</p>
{% codeblock bash lang:bash %}
help error_table
{% endcodeblock %}

<h2>Considerações sobre variáveis</h2>
<p>
<ul>
  <li>O Scilab é case sensitive, isto é, difere maiúscula de minúscula;</li>
  <li>Tem tipagem dinâmica, ou seja, a variável que recebe o tipo de dados numérico poderá posteriormente receber uma string sem problemas;</li>
  <li>Em nomes de variáveis não são permitidos caracteres especiais;</li>
  <li>O nome de uma variável pode ter no máximo 24 caracteres e começar com <code> _ </code> (underline) ou letra;</li>
</ul>
</p>

<h2>Considerações sobre constantes</h2>
<p>
Constantes especiais começam com %
<ul>
  <li>%f – Valor lógico false</li>
  <li>%t – Valor lógico True</li>
  <li>%eps – Precisão da máquina virtual para números reais</li>
  <li>%pi – 3.1415927…</li>
  <li>%e – Base dos logaritmos naturais</li>
  <li>%inf – Infinito</li>
  <li>%nan – not a number (não é número)</li>
  <li>%i – parte imaginária de númeors complexos</li>
</ul>
</p>

<h2>Comandos básicos</h2>

{% codeblock bash lang:bash %}
pwd         #exibe o diretorio atual ,
home        #exibe o diretorio atual
chdir       #muda o diretorio de corrent
mkdir       #cria diretórios
dir         #exibe o nome dos arquivos no diretório corrent
clc         #limpa toda area de trabalho e coloca o cursor no inicio
clc(n)      #limpa n linhas acima da linha atual
clear       #limpa as variáveis da memória
who         #exibe  as constantes especias e as variaveis atuais na memória
whos        #exibe os nomes, os tamanhos e os tipos de dados de todas as variáveis na memória
{% endcodeblock %}

<h2>É isso ai</h2>
<p>
Conhecemos um pouco do ambiente do Scilab, ficar atento ao “;” pode poupar tempo na execução de scripts.<br /> 
Para saber mais sobre o console você pode acessar a documentação <a href="http://help.scilab.org/docs/5.3.2/pt_BR/console.html">clicando aqui!</a>
</p>




