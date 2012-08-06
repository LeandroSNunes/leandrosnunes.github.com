---
layout: post
title: "Instalando Arduino no Debian squeeze"
date: 2012-05-09 21:39
comments: true
categories: [Arduino]
tags: [Processing, Arduino, Debian, Arduino-core]
description: "Como usuário do Debian, percebi que instruções sobre instalação em livros não diz muita coisa mais garimpando na web, concluir minha instalação."
keywords: "Arduino, Debian, Linux"
---
{% img /images/arduino/banner.jpg 789 %}
<p>
Ta nóis aqui em pleno carnaval 2012 atrás do bloco dos aficionados por tecnologia com uma boa xícara de café e estudando Arduino. Como usuário do Debian, percebi que instruções sobre instalação em livros não diz muita coisa mais garimpando na web, concluir minha instalação.
</p>
<!-- more -->
<p>
O Arduino esta dividido em dois pacotes:
</p>
<ul>
	<li>Arduino: Que é a IDE em Java para desenvolvimento e contém algumas biblioteca.</li>
	<li>Arquino-Core:  Ferramenta mínima para interagir via linha de comando sem dependências do Java, usando Makefile Martin Oldfield.</li>
</ul>

<p>
Alternativamente se seu arquivo  <em>/etc/apt/sources.list</em>  estiver atualizado você pode faze a instalação dos pacotes executando:
</p>
{% codeblock bash lang:bash %}
sudo apt-get install arduino
{% endcodeblock %}
ou
{% codeblock bash lang:bash %}
sudo apt-get install arduino-core
{% endcodeblock %}
<p>
Atualize seu arquivo lendo o post Tornando ninja o arquivo sources.list do Debian
</p>
{% blockquote %}
Como eu não tinha certeza que a última versão está disponível nos repositórios do Debian e não queria testar, 
fiz a instalação baixando o pacote no site oficial <a href="http://arduino.cc/en/Main/Software" >http://arduino.cc/en/Main/Software.</a>
{% endblockquote %}


<h2>Dependencias</h2>
<p>
Usuários do arduino Uno precisam instalar o pacote <em>brxtx-java versão 2.2pre2-3</em> ou superior  Então edite seu arquivo  <em>/etc /apt /sources.list</em> (ou use seu gerenciador de pacotes favorito gui) e adicione:
</p>

<p>
<em>deb http://ftp.us.debian.org/debian/ squeeze main contrib non-free</em><br /> 
Feito isso, execute:
</p>
{% codeblock bash lang:bash %}
sudo apt-get install librxtx-java
{% endcodeblock %}
<p>
Agora instale todas as dependências
</p>
{% codeblock bash lang:bash %}
sudo apt-get install gcc-avr avrdude gcc avr-libc libantlr-java libecj-java libjna-java liboro-java openjdk-6-jdk
{% endcodeblock %}
<p>
Caso queira instalar somentes pacotes instáveis, retire a linha do arquivo source.list antes de executar este último comando.
</p>

<h2>Fazendo o download</h2>
<p>
Baixe o binário para linux de 32 bits (ou 64 bits, se for preciso) clicando no link <a href="http://arduino.cc/en/Main/Software">http://arduino.cc/en/Main/Software</a> e extraia o arquivo .tgz para a pasta de sua preferência.<br />
Vou baixar a versão para 64bits na pasta onde irei efetuar a instalação via linha de comando.
</p>

{% codeblock bash lang:bash %}
cd /opt/
sudo wget http://arduino.googlecode.com/files/arduino-1.0-linux64.tgz
sudo tar zxvf arduino-1.0-linux64.tgz
sudo rm arduino-1.0-linux64.tgz
{% endcodeblock %}

<h2>Executando</h2>
<p>
Agora você já pode executar a IDE dando dois clicks no arquivo <em>/opt/arduino-1.0/arduino</em>  ou via linha de comando
</p>

{% codeblock bash lang:bash %}
cd arduino-1.0
sh arduino
{% endcodeblock %}

<p>
Caso queira adicionar o Arduino no menu de programas do Debian, leia este post <a href="http://leandronunes.com/blog/2012/05/05/adicionar-programa-no-menu-do-debian/">“Adicionar programa no menu do Debian”.</a>
</p>

<h2>Dando permissão</h2>
<p>
Você vai notar que não pode selecionar uma porta serial no menu ferramentas. Isto é provavelmente porque você precisa adicionar o usuário para alguns grupos. Você também tem que conectar o Arduino UNO para poder selecionar uma porta serial.
</p>

<p>
Para um teste rápido, você pode executar arduino como root com: sudo sh arduino. Você deve ser capaz de selecionar uma porta serial agora.
</p>

<p>
Claro que você não quer estar executando o IDE Arduino como root, então adicionar seu usuário ao grupo <code>tty</code> e <code>dialout</code>   assim:
</p>
{% codeblock bash lang:bash %}
sudo usermod -a -G tty yourUserName
sudo usermod -a -G dialout yourUserName
{% endcodeblock %}
<p>
Faça logoff e logon novamente para que as alterações tenham efeito!
</p>

<h2>Então..</h2>
<p>
Isso é o começo para iniciar os teste com arduino e impressionar os vizinhos.<br />
Para mais detalhes sobre instalação em outros ambiente visite o <a href="http://renatoaloi.blogspot.com/2011/10/instalando-arduino-guia-completo.html">blog do Renato Aloi</a>, uma ótima referência de estudos também!
Mais informações de instação leia o site oficial <a href="http://arduino.cc/playground/Linux/Debian">http://arduino.cc/playground/Linux/Debian</a>
</p>
