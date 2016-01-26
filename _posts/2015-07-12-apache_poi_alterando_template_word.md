---
layout: post
title: "Apache POI - Alterando templates no Word!"
description: "Leitura de template do word com o apache poi"
category: java
tagline: 
tags: [java, apache-poi, word, template]
keywords: java, apache-poi, word, template, docx
comments: true
fullview: true
---

Uma tarefa muito comum, porém chata é a leitura de **templates** do **Microsoft Word** e a alteração de palavras chaves personalizadas por um valor em específico. Há diversas formas de se fazer isso, hoje vou mostrar uma dessas formas, utilizando o [Apache POI](https://poi.apache.org/).
Este código foi testado com as versões 3.10-FINAL e 3.12 do **Apache POI**. Devido a um [Bug](https://bz.apache.org/bugzilla/show_bug.cgi?id=57963) na versão 3.12 é necessário adicionar a biblioteca `poi-scratchpad` as dependências, caso contrário você receberá o erro:

{% highlight bash %}
error: cannot access Paragraph
            List<XWPFRun> runs = paragraph.getRuns();
                                          ^
  class file for org.apache.poi.wp.usermodel.Paragraph not found
{% endhighlight %}

Nas dependências do gradle adicione as seguintes bibliotecas:

{% highlight bash %}compile group: 'org.apache.poi', name: 'poi', version: '3.12'
compile group: 'org.apache.poi', name: 'poi-ooxml', version: '3.12'
compile group: 'org.apache.poi', name: 'poi-scratchpad', version: '3.12'{% endhighlight %}


### O modelo

Vamos utilizar um modelo simples (o código do exemplo é para arquivos **.docx**), mas bastante comum. Um texto com uma tabela estática, para efeito de exemplo coloquei apenas duas variáveis no documento, `#valor_parcela#` e uma variável na tabela `#valor_total#`.
Veja a imagem do modelo, abaixo: 

![Modelo]({{ site.BASE_PATH }}/assets/media/modelo_documento.png "Modelo")

### E o código?

Vamos lá, chega de enrolação e vamos ao código.
Criei um método `extractTemplate(InputStream stream, Map<String, String> properties)` que recebe um stream do documento e um Map<String, String> com as chaves/valores que iremos substituir.

{% gist cadocruz/0dbe40c1a9df3bdf1a60 %}

Você pode fazer o download do código fonte do exemplo [aqui](https://github.com/cadocruz/exemplo-apache-poi). (https://github.com/cadocruz/exemplo-apache-poi)

Abaixo dois métodos extras, um para configurar o tamanho da tabela e outro para configurar um paragrafo.
{% gist cadocruz/fe12e43e668ca84cfc61 %}