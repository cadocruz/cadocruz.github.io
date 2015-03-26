---
layout: post
title: "Remover Automatic Component Binding no ADF"
description: "Muitas vezes quando estamos desenvolvendo com Oracle ADF no JDeveloper, e estamos criando nossas views, o JDeveloper faz o binding automatico dos componentes da nossa view com nosso Manage Bean, e dependendo da complexidade da view o manage bean vai ficar entulhado de código inútil."
category: adf
tagline: Oracle ADF e JDeveloper
tags: [oracle, java, jdeveloper, auto-binding, adf]
keywords: java, adf, jdeveloper, auto binding, automatic component binding, oracle
comments: true
---


Muitas vezes quando estamos desenvolvendo com Oracle ADF no JDeveloper, e estamos criando nossas views, o JDeveloper faz o binding automatico dos componentes da nossa view com nosso Manage Bean, e dependendo da complexidade da view o manage bean vai ficar entulhado de código inútil.

Mas há 2 formas de desativá-los para a view desejada. 

* Configuração no JDeveloper
* Manual

### Configuração no JDeveloper

Vamos ver como fazer pela própria configuração do JDeveloper.

Abrir a pagina jsf no modo Design.
Ir no Menu Design

![Menu Design]({{ site.BASE_PATH }}/assets/media/jdev_menu_design.png "Menu Design")

Abrir aba Component Binding e desmarcar a opção Auto Bind

![Aba Component Binding]({{ site.BASE_PATH }}/assets/media/jdev_page_properties.png "Aba Component Binding")

Clique OK.

### Manual

O modo manual é muito mais rápido e facil, é só procurar pelo seguinte comentário no fonte da view:

{% highlight jsp %} 
    </af:document>
    <!--oracle-jdev-comment:auto-binding-backing-bean-name:backing_JSF_untitled1-->
</f:view>
{% endhighlight %}

e removê-lo. Geralmente fica no fim do código.

Pronto, removemos o "Automatic Component Binding" do ADF.
