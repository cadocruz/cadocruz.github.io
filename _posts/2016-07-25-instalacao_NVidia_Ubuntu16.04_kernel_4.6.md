---
layout: post
title: "Instalação do driver NVidia no Ubuntu 16.04 com kernel 4.6.*"
description: "Como fazer a instalação do driver 340.96 da NVidia no Ubuntu se você estiver usando o kernel 4.6.4"
category: linux
tagline:
tags: [ubuntu, linux, nvidia, kernel4.6]
keywords: ubuntu, linux, nvidia, kernel
comments: true
fullview: true
---

Estava tendo problemas com a versão 340.96 do driver da NVidia quando compilava o kernel 4.6.4, no começo pensei ser problema de configuração no kernel, pois fazia anos que não fazia isso :), mas depois de algum tempo de pesquisa descobri que era um bug no driver da NVidia mesmo.

Então vamos lá:
(Vou mostrar como fazer a instalação do zero, mas se você já tinha o driver funcionando, mas só quer saber como faze-lo funcionar no novo kernel, pode pular esta parte.)

Antes de tudo, remova tudo que você tinha instalado da nvidia:
{% highlight bash %}
sudo apt-get remove nvidia*
sudo apt-get autoremove
{% endhighlight %}
Atualize o repositório:
{% highlight bash %}
sudo apt-get update
{% endhighlight %}
Instale os pacotes necessários para a compilação do módulo da NVidia:
{% highlight bash %}
sudo apt-get install dkms build-essential linux-headers-generic
{% endhighlight %}
Adicione o modulo do nouveau na blacklist:
{% highlight bash %}
sudo vi /etc/modprobe.d/blacklist-nouveau.conf
{% endhighlight %}

adicione estas linhas:

```
blacklist nouveau
blacklist lbm-nouveau
options nouveau modeset=0
alias nouveau off
alias lbm-nouveau off
```
Desabilite o modulo nouveau do kernel com o comando abaixo:

{% highlight bash %}
echo options nouveau modeset=0 | sudo tee -a /etc/modprobe.d/nouveau-kms.conf
sudo update-initramfs -u
{% endhighlight %}

baixe o patch do driver da nvidia: https://git.archlinux.org/svntogit/packages.git/plain/trunk/linux-4.6.patch?h=packages/nvidia-340xx

salve em algum diretório como linux-4.6.patch

Reinicie o computador, na tela de login digite ``Ctrl+Alt+F1``, vai entrar no terminal do linux, faça o login, entre no diretorio onde se encontra o driver e digite:

{% highlight bash %}
sudo chmod +x NVIDIA-Linux-x86_64-340.96.run
{% endhighlight %}

Pare o X-Server
{% highlight bash %}
sudo service lightdm stop
{% endhighlight %}

Agora vamos começar a brincadeira com a versão do driver 340.96 da NVidia.
Como falei, tem um bug nele que impede de compilar no 4.6.*

Extraia o driver da NVidia
{% highlight bash %}
sudo ./NVIDIA-Linux-x86_64-340.96.run -x
{% endhighlight %}

Entre no diretorio que extraimos:
{% highlight bash %}
cd NVIDIA-Linux-x86_64-340.96
{% endhighlight %}

copie o arquivo linux-4.6.patch que baixamos para o diretório NVIDIA-Linux-x86_64-340.96
Aplique o patch:

{% highlight bash %}
patch -p1 < linux-4.6.patch
{% endhighlight %}

entre no diretório kernel (dentro de NVIDIA-Linux-x86_64-340.96) e digite:


{% highlight bash %}
make clean
make install
{% endhighlight %}
Após a compilação volte um nível (NVIDIA-Linux-x86_64-340.96) e digite:

{% highlight bash %}
./nvidia-installer
{% endhighlight %}

Para mim, a opção para registrar o modulo DKMS não funcionou, marquei como 'No'
Terminado a instalação, reiniciei o computador e o driver estava configurado.
Foi nítida a diferença de qualidade de gráfico do driver nouveau para o driver proprietário da NVidia.

É isso aí...
