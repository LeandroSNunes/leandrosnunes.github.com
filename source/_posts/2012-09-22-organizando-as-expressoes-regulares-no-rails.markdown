---
layout: post
title: "Organizando as Expressões Regulares no Rails"
date: 2012-09-22 14:55
comments: true
categories: [Rails, Ruby]
tags: [Expressões Regulares, Patterns]
description: "No dia a dia sempre necessitamos dos super poderes das Expressões Regulares para validações de formulários e organizá-las é uma boa idéia. Neste post mostro uma solução que estou usando no Rails"
keywords: "expressões regulares, expressôes regulares, er, ers, patterns, regex, regexp, utilizando ers ruby, regexp no ruby, validação com  expressão regular, validar email com expressão regular, atributo pattern do html5, validação de formulário com er, criando validação de formulario, text_field + rails, validando dados na tag input, regexp ruby-doc.org"
---


{% img /images/post-organizando-ers.jpg 789 "Banner do post Organizando as Expressões Regulares no Rails" %}


<p>
No dia a dia sempre necessitamos dos super poderes das Expressões Regulares para validações de formulários, replaces em 
textos e tantas outras coisas mais, alguns patterns raramente mudam de um projeto para outro, o pattern para validar e-mails 
é um exemplo.
</p>
<!-- more -->

<p>
Pensando nisso e aproveitando a estrutura do Rails que já possui a pasta <code> /lib </code>  para armazenar nossos códigos customizados, 
criei um module "ER" para ir colecionando os patterns rotineiros. 
</p>
{% blockquote %}
Neste post estou mostrando uma solução que encontrei pois ainda desconheço se o Rails possui alguma convenção para essa tarefa.
{% endblockquote %}
<h2>Vamos ver a ideia!</h2>

<p>
Na pasta <code> /lib </code> criei um arquivo <code> er.rb </code>  que será nosso "repositório de ERs".
</p>
{% codeblock ruby lang:ruby %}
module ER
  # Pattern para validação de e-mail
  EMAIL = /^[^@][\w.-]+@[\w.-]+[.][a-z]{2,4}$/i 
  # Pattern para validação de data no padrão 99/99/9999
  DATE = /^(([012][0-9])|(3[01]))\/(0[1-9]|1[012])\/\d{4}$/ 
  # Pattern para validação de data no padrão 9999-99-99
  DATE_DB = /^\d{4}-(0[1-9]|1[012])-(([012][0-9])|(3[01]))$/
  # Pattern para validação de horas sem os segungos no padrão 99:99
  TIME_H_M = /^(([01]\d)|(2[0-3])):([0-5]\d)$/
  # Pattern para validação de urls, permitido os protocolos http e https
  URL = /^(http|https):\/\/[a-z0-9]+([\-\.]{1}[a-z0-9]+)*\.[a-z]{2,5}(([0-9]{1,5})?\/.*)?$/ix
end
{% endcodeblock %}

<h2>Utilizando</h2>
<p>
Para exemplificar, vamos validar um model User.
</p>
{% codeblock ruby lang:ruby %}
require 'er'
class User < ActiveRecord::Base
  attr_accessible :email, :full_name
  validates :email, presence:true,  format:{with: ER::EMAIL}
end
{% endcodeblock %}
Perceberam a chamada do pattern? :P

<p>
Para validação em front-end utilizando o atributo <code> pattern </code> do HTML5, podemos reaproveitar nossos patterns, só que 
precisamos de um passo a mais devido o padrão ser ER crua sem estar contida em "//" (barras). 
</p>

<p>
Criei então um Help para fazer essa tarefa e as View continuarem fazendo apenas seu papel. 
No arquivo <code> app/helpers/application_helper.rb </code>  incluir:
</p>
{% codeblock ruby lang:ruby %}
def er_for_html(er)
  begin
    ER.const_get(er.upcase.to_sym).source
  rescue NameError => exc
    "A expressao solicitada nao existe"
  end
end
{% endcodeblock %}


<p>
Notem que usei <code> Module#const_get </code>  para pegar a referência da constante informado e no fim o 
<code> Regexp#source </code>  que retorna a string original que está envolvida por "//" <br />
Ficando no formulário:
</p>
{% codeblock ruby lang:ruby %}
<%= f.text_field(:email, :class => :span3, :pattern => er_for_html("email"), :type => :email, :required => true, :title => "E-mail" ) %>

{% endcodeblock %}

<h2>Concluíndo</h2>

<p>
É isso ai, a intenção é só para mostar uma possibilidade de organizar as coisas, claro que deve possuir outras, dessa forma, 
aceito sugestões e dicas. :)
</p>

<p>
Para uma consulta rápida sobre metacaracteres o Aurélio disponibiliza um guia rápido <a href="http://piazinho.com.br/download/expressoes-regulares-3-tabelas.pdf" title="Ir para outra página">http://piazinho.com.br/download/expressoes-regulares-3-tabelas.pdf</a>.<br />
Para se aprofundar, leia o livro <a href="http://piazinho.com.br/" title="Ir para página do livro">Expressões Regulares - Uma abordagem divertida</a>.
</p>




