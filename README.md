<h1 align="center">Vagrantfile Gerencianet - PHP</h1>

![Curso de integração Gerencianet](https://media-exp1.licdn.com/dms/image/C4D1BAQH9taNIaZyh_Q/company-background_10000/0/1603126623964?e=2159024400&v=beta&t=coQC_AK70vTYL3NdvbeIaeYts8nKumNHjvvIGCmq5XA)

<p align="center">
  <span><b>Português</b></span> |
  <a href="https://github.com/gerencianet/gn-vagrant-php/blob/master/README-en.md">Inglês</a>
</p>

---

Este repositório fornece um modelo Vagrantfile para facilitar a criação de uma máquina virtual usando um hipervisor. Pense nisso algo como XAMPP/WAMP que não depende do seu sistema operacional. Após a conclusão da configuração, você terá uma máquina virtual com Ubuntu, preparada para execução de projetos PHP.


## Sobre o Vagrant

O [Vagrant](https://www.vagrantup.com/) é uma ótima ferramenta para criar e configurar ambientes de máquina virtual leves, reproduzíveis e portáteis.

Os ambientes de desenvolvimento gerenciados pelo Vagrant podem ser executados em plataformas virtualizadas locais (providers), como VirtualBox, VMware e outros. Aqui detalhamos como instalar utilizando o VirtualBox.

O Vagrant fornece a estrutura e o formato de configuração para criar e gerenciar ambientes de desenvolvimento completos. Esses ambientes de desenvolvimento funcionam em seu computador ou na nuvem e são portáteis entre Windows, Mac OS X e Linux.

## Como funciona?
Quando executamos o comando para o Vagrant subir uma VM, ele lê o arquivo [Vagrantfile](https://www.vagrantup.com/docs/vagrantfile). Nele estão todas as configurações e definições da VM em questão. O Vagrant inicia uma box no provider, definida no arquivo de configuração. Como existem mais instruções expressas, ele as executa, deixando uma máquina disponível totalmente configurada para desenvolvimento.

## Configuração simplificada
1. Instale dependências de acordo com seu sistema operacional
    
* Acesse a [página de download](https://www.virtualbox.org/wiki/Downloads) do [VirtualBox](https://www.virtualbox.org/), escolha seu sistema operacional, baixe e instale o software.
* Acesse a [página de download](https://www.vagrantup.com/downloads) do [Vagrant](https://www.vagrantup.com/), escolha o seu sistema operacional, baixe e instale o software.

2. Clone este projeto e acesse o diretório

    Crie um diretório para a sua VM (Ex: ``C:\Users\%userprofile%\vagrant-boxes``), acesse-a e execute os seguintes comandos:
    
    ```
    $ git clone https://github.com/gerencianet/gn-vagrant-php.git
    $ cd gn-vagrant-php
    ```

3. Inicialização

    Para iniciar sua máquina, execute o seguinte comando no diretório raiz do Vagrantfile:

    ```
    # Aciona o vagrant para baixar a imagem do Ubuntu (se necessário) e re/iniciar uma instância.
    $ vagrant up
    ```
    
    **E sobre o VirtualBox?**

    Parece que não usamos o VirtualBox para nada. Mas, abrindo o aplicativo VirtualBox você verá que a VM está executando.

    Como você pode ver, o VirtualBox está nos informando que o Ubuntu está funcionando corretamente.

 4. Acesso SSH   

    Agora temos esse sistema operacional configurado em nosso computador, mas como o acessamos? Assim como você acessaria qualquer servidor Linux remoto por meio da linha de comando, você fará o mesmo com o Vagrant. Execute o seguinte comando no diretório raiz do Vagrantfile para entrar com segurança na máquina virtual.
   
    ```
    # Conecta você à máquina virtual. A configuração é armazenada no diretório para que você sempre possa retornar a esta máquina.
    $ vagrant ssh
    ```

## Compartilhamento de pasta e direcionamento de porta

Há configuração de pasta compartilhada definida no Vagrantfile, criando no diretório do vagrant a pasta ``www_gn``. Diretório este, que é mapeado a pasta raiz do Apache, permitindo implementar seu projeto nesta pasta.

O direcionamento de portas do Vagrant, permitem que você acesse uma porta em sua máquina host e tenha todos os dados encaminhados para uma porta na máquina convidada, seja por TCP ou UDP. Com esta configuração, você pode então abrir seu navegador http://localhost:8080 e navegar no seu proejeto.
``` 
config.vm.network "forwarded_port", guest: 80, host: 8080
config.vm.synced_folder ".", "/vagrant", type: "virtualbox", owner: "www-data", group: "www-data"
```

## VM Setup e ferramentas

O seguinte setup e ferramentas serão configurados automaticamente ao iniciar a VM.
* Ubuntu v20.04
* 1024mb de memória RAM
* Apache
* PHP v8.0
* Nano
* Git
* Composer

## Comandos úteis
Para executar os comandos na VM, você deve estar no diretório onde o Vagrantfile em questão está localizado.

```
# Reinicia a máquina virtual.
# Equivalente a um reboot. Útil principalmente quando há mudanças no Vagrantfile.
$ vagrant reload

# Desliga a máquina virtual.
# Equivalente a um shutdown.
$ vagrant halt

# Suspende a execução da máquina virtual salvando seu estado.
# Ideal para o dia-a-dia quando está desenvolvendo, e for dar um pause.
$ vagrant suspend

# Retoma uma máquina virtual previamente suspensa.
$ vagrant resume

# Destrói a máquina virtual. 
# Use quando quiser começar do zero.
$ vagrant destroy
```
