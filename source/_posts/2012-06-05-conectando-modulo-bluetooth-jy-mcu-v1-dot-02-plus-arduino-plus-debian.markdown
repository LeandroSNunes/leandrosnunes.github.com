---
layout: post
title: "Conectando módulo Bluetooth JY-MCU V1.02 + Arduino + Debian"
date: 2012-06-05 21:54
comments: true
categories: [Linux,Debian,Arduino]
tags: [Debian, Arduino, Linux, Bluetooth, Blueman, CuteCom]
description: "Neste post vamos ver como fazer a conexão do módulo de Bluetooth JY-MCU V1.02 com o Arduino no Debian 6"
keywords: "Debian, Linux, apt-get, Bluetooth, Blueman, CuteCom, JY-MCU, gnome-bluetooth, Package, BT_BOARD"
---

<p>
Tempo curtissímo neste final de período de faculdade, mais hoje, não pude deixar de compartilhar algumas informações com os que estão junto comigo, descobrindo 
o Arduino. Vamos desenrrolar o assunto...
</p>
<p>
Recentemente comprei um módulo de Bluetooth para Arduino, o módulo adquirido foi o "JY-MCU V1.02" 
<a href="http://dx.com/p/jy-mcu-arduino-bluetooth-wireless-serial-port-module-104299?item=1">Confira aqui!!!</a>, então, fui à luta 
para fazer funfar no Debian (lembrando que sou novato no Debian e Arduino também) ;) , a briga foi boa!!! <br />
Gostaria de agradecer nosso
amigo/professor Marcelo Brunoro por ter mostrado que a placa realmente funciona (utilizando outro OS), antes que eu desistisse heheheh.
</p>
<!-- more -->

<h2>Requisitos para o Debian</h2>
<p>
O Debian 6 é instalado com o pacode de ferramentas <a href="http://packages.debian.org/pt/sid/gnome-bluetooth">gnome-bluetooth</a> para 
manipular dispositivos Bluetooth utilizado inteface gráfica, porém, esse pacote não possibilita o mapeamento de uma porta serial para o
dispositivo, e a comunição com o Arduino se dá via porta serial que, neste caso, utilizaremos como exemplo a biblioteca <a href="http://arduino.cc/hu/Reference/SoftwareSerial">SoftwareSerial</a>.
É preciso então a instalação do pacote <a href="http://blueman-project.org/">Blueman</a>, vamos lá então...
</p>

{% codeblock bash lang:bash %}
sudo apt-get install blueman
{% endcodeblock %}


<p>
Depois da instalação o programa se encontra em <em>System -> Preferences -> Bluetooth Manager</em>
</p>
{% img /images/arduino_bluetooth/blueman.png 789 %}

<p>
Antes de plugar o módulo, é preciso instalar mais um software, o <a href="http://packages.debian.org/unstable/comm/cutecom">"CuteCom"</a>, para monitoramento da porta serial, ele será útil para enviar e receber dados da porta destinada ao
módulo de Bluetooth.
</p>

{% codeblock bash lang:bash %}
sudo apt-get install cutecom
{% endcodeblock %}

<p>
Para finalizar, é preciso que sua IDE do Arduino seja a 1.0.1 ou superior, caso contrário a biblioteca SoftwareSerial irá bugar, para fazer esse trabalho de atualização 
é só seguir os passos neste post <a href="http://leandronunes.com/blog/2012/05/09/instalando-arduino-no-debian-squeeze/">Instalando Arduino No Debian Squeeze</a> trocando
a versão da IDE.
</p>

<h2>Ligando os componentes</h2>
<p>
O módulo JY-MCU V1.02 possui 4 pinos (RX, TX, GND E VCC) e já vem com um cabo.
</p>
{% img /images/arduino_bluetooth/modulo.jpg 789 %}

<p>
A ligação no Arduino se dá da seguinte forma:
</p>
<table width="200px">
	<thead>
		<tr><th>JY-MCU</td><td>Arduino</th></tr>
	</thead>
	<tbody>
		<tr><td>RX</td><td>PINO 3</td></tr>
		<tr><td>TX</td><td>PINO 2</td></tr>
		<tr><td>GND</td><td>GND</td></tr>
		<tr><td>VCC</td><td>5V</td></tr>
	</tbody>
</table>
<br />
<p>
Agora é só plugar o cabo USB no Arduino e no PC, pronto, tudo conectado
</p>

{% img /images/arduino_bluetooth/arduino.jpg 789 %}


<h2>Codando nosso exemplo</h2>
<p>
Na IDE do Arduino existe vário exemplos de códigos para iniciar projetos, vamos utilizar um para testar nossa conexão, vá em <em>File -> Exemples -> SoftwareSerial -> SoftwareSerialExemple </em>
</p>
{% img /images/arduino_bluetooth/exemplos_arduino.jpg 789 %}

<p>
Com algumas modificações o código ficou assim:
</p>

{% codeblock Processing lang:c %}

#include <SoftwareSerial.h>

SoftwareSerial mySerial(2, 3); // RX, TX

void setup()  
{
 // Open serial communications and wait for port to open:
  Serial.begin(9600);
   while (!Serial) {
    ; // wait for serial port to connect. Needed for Leonardo only
  }

  
  Serial.println("Goodnight moon!");

  // set the data rate for the SoftwareSerial port
  mySerial.begin(9600);
  mySerial.println("Hello, world?");
}

void loop() // run over and over
{
  if (mySerial.available())
    Serial.write(mySerial.read());
  if (Serial.available())
    mySerial.write(Serial.read());
}
{% endcodeblock %}

<h3>O que foi mudado</h3>
<p>
<ul>
	<li>Os pinos utilizados <code>SoftwareSerial mySerial(2, 3);</code></li>
	<li>A taxa de rate na linha <code>Serial.begin(9600);</code></li>
	<li>A taxa rate do módulo<code>mySerial.begin(9600);</code></li>
</ul>
</p>

<p>
Vá em frente e faça o upload para o Arduino :)
</p>


<h2>Detectando o módulo de Bluetooth no PC</h2>
<p>
Agora abra o Bluetooth Manager, clique em Search e o módulo será listado como "linvor", clicando com o botão direito, selecione "add Device"
</p>
{% img /images/arduino_bluetooth/add_device.png 789 %}

<p>
Novamente como botão direito clique em "Pair", será pedido o PIN, no nosso caso digite "1234". <br />
Novamente como botão direito clique em "Dev B", ufa! chega de botão direito, já estar conectado.
</p>
{% img /images/arduino_bluetooth/conected.png 789 %}

<em>
Em baixo na tarja amarela, visualizamos a porta que está sendo utilizada "/dev/rfcomm0"
</em>

<h3>Monitorando</h3>
<p>
Abra o CuteCom e no campo "Device:", clique e digite o caminho da porta, depois é so clicar em "Open device"
</p>
{% img /images/arduino_bluetooth/cutecom.png 789 %}

<p>
Voltamos na IDE do Arduino e abrimos o Serial Monitor, a mensagem "Goodnight moon!" será exibida
</p>
{% img /images/arduino_bluetooth/serial_monitor.png 789 %}

<p>
No CuteCom será exibido "Hello, world?"
</p>
{% img /images/arduino_bluetooth/cutecom_mensagem.png 789 %} 
<p>
Pronto, agora o que voce digitar no campo "input" do CuteCom será exibido no Serial Monitor e vice-versa, neste caso os dispositivos estão pareados.
</p>
{% img /images/arduino_bluetooth/mensagem_ok.png 789 %}

<h2>Então...</h2>
<p>
Ufa! O módulo de Bluetooth JY-MCU V1.02 está conectado e conversando com o Arduino.<br />
O módulo de Bluetooth não estando pareado ele aceita uma série de comando como descrito por <a href="http://byron76.blogspot.com.br/2011/09/one-board-several-firmwares.html">Byron</a>,
vale a pena dar uma conferida, vou deixar para outro post. <br />
É isso ai, espero ter fornecido informações importantes para a continuação dos estudos. Até a proxima!!!!!
</p>
