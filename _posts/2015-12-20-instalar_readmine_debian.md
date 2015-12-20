---
layout: post
title: "Instalando o Redmine no Debian"
description: "Uma dúvida comum de quem vai instalar o redmine pela primeira vez no Debian é, qual tutorial seguir, tendo em vista que na internet existem vários tutoriais. Mas qual usar? Pensando nisso, fiz um passo a passo rápido e facil, para instalar o Redmine no Debian."
category: linux
tagline: 
tags: [redmine, debian, linux]
keywords: redmine, debian, linux
comments: true
fullview: true
---

Uma dúvida comum de quem vai instalar o redmine pela primeira vez no Debian é, qual tutorial seguir, tendo em vista que na internet existem vários tutoriais. Mas qual usar? Pensando nisso, fiz um passo a passo rápido e facil, para instalar o Redmine no Debian.

Primeiro de tudo, vamos atualizar a lista de aplicativos no repositório com apt-get update.

{% highlight bash %} 
sudo apt-get update
{% endhighlight %}

Após isso, vamos instalar o MySQL (pode usar o PostgreSQL se preferir)

### Instalar o MySQL

{% highlight bash %} 
sudo apt-get install mysql-server
{% endhighlight %}

Ao instalar o MySQL, será pedido uma senha de root

![MySQL]({{ site.BASE_PATH }}/assets/media/redmine/Debian_20_12_2015_18_24_54.png "MySQL")


### Instalar Apache2 e Redmine

{% highlight bash %} 
sudo apt-get install apache2 libapache2-mod-passenger
sudo apt-get install redmine redmine-mysql
{% endhighlight %}

Após isso, será perguntado se você quer configurar um banco de dados ao redmine, escolha "Yes"

![Configurar Banco de Dados do Redmine]({{ site.BASE_PATH }}/assets/media/redmine/Debian_20_12_2015_18_36_27.png "Configurar Banco de Dados do Redmine")

Agora escolha o banco de dados, no nosso caso será o MySQL.

![Escolher Banco de Dados]({{ site.BASE_PATH }}/assets/media/redmine/Debian_20_12_2015_18_36_53.png "Escolher Banco de Dados")

Digite a senha do usuário de root do MySQL que criamos anteriormente

![Senha Banco de Dados]({{ site.BASE_PATH }}/assets/media/redmine/Debian_20_12_2015_18_41_54.png "Senha Banco de Dados")

digite a senha da instancia do redmine e depois repita a senha.

![Senha Instancia Redmine]({{ site.BASE_PATH }}/assets/media/redmine/Debian_20_12_2015_18_44_18.png "Senha Instancia Redmine")

Agora vamos configurar os arquivos para colocar o Redmine no ar.

#### Editar o arquivo /etc/apache2/mods-available/passenger.conf 

{% highlight bash %} 
sudo vi /etc/apache2/mods-available/passenger.conf 
{% endhighlight %}

adicionar `PassengerDefaultUser www-data`

deixar igual abaixo:

{% highlight bash %} 
  PassengerDefaultUser www-data
  PassengerRoot /usr/lib/ruby/vendor_ruby/phusion_passenger/locations.ini
  PassengerDefaultRuby /usr/bin/ruby
{% endhighlight %}

#### Criar um link no diretorio /var/www/html
{% highlight bash %} 
sudo ln -s /usr/share/redmine/public /var/www/html/redmine
{% endhighlight %}

#### instalar o Bundler
{% highlight bash %} 
gem install bundler
{% endhighlight %}

#### Editar o arquivo /etc/apache2/sites-available/000-default.conf
{% highlight bash %} 
sudo vi /etc/apache2/sites-available/000-default.conf
{% endhighlight %}

adicionar:

{% highlight bash %} 
<Directory /var/www/html/redmine>
    RailsBaseURI /redmine
    PassengerResolveSymlinksInDocumentRoot on
</Directory>
{% endhighlight %}

#### Adicionar o passenger ao apache
{% highlight bash %} 
sudo a2enmod passenger
{% endhighlight %}

#### Reiniciar apache2
{% highlight bash %} 
sudo service apache2 restart
{% endhighlight %}

Agora é só acessar `http://localhost/redmine`, o usuário e senha default é: `admin/admin`

