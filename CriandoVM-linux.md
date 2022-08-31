
# Parte-01


## Criando a maquina Linux VBox no Windows:
https://www.virtualbox.org/
1. - depois de criada verificar a placa de REDE, use a Placa em modo Bridge
2. - memória, processador, vídeo;

## Como administrador no Power Shell;
3. - ativar VT-x: exemplo https://youtu.be/iBNytZ3GZWM
- [x]  vá em: cd 'C:\Arquivos de Programas\Oracle\VirtualBox\'
- [x]  execute: ./vboxManage.exe modifyvm "nome-maquina" --nested-hw-virt on

## Instalando o Debian:
1. - escolha a ISO de instalação (baixada do site);
2. - faça a instalação minima do servidor;
- [x] - escolha apenas: utilitário de sistema padrão

VM pronta para baixar: https://1drv.ms/u/s!AjncOwJSKLqhgmCR7a0n_G2KPgA1?e=yQTefV
- atualizada em 16/07/2022

Vídeo da criação da VM:: https://youtu.be/GohsfmLnNnY

# Parte-02


## Preparando o usuário para administrar:
1. - logar com o usuário comum (aluno);
2. - com o **"su -"** abra o terminal de root;

3. - instalar o pacote SUDO e o SSH;

         apt install sudo
         apt install ssh

4. - adicione o seu usuário ao grupo SUDO;
- [x] - faça o logoff do root e use o **"su -"** novamente;

         adduser aluno sudo
         
- [x] - faça o logoff do root;
- [x] - peque o IP com **ip a** anote; aqui ficou 192.168.0.16
- [x] - faça logoff do usário aluno e dai para frente só use o usário Aluno via SSH;

5. - no terminal do power shell do windows, faça login com SSH;

         ssh aluno@192.168.0.16
       
- [x] - aceite a chave de criptografia com yes;


## Criando os sites no servidor:
1. - se não tiver instalado os pacotes de serviço web;
- [x] - Instalando o Apache 2;
     
         sudo apt install apache2 apache2-utils libexpat1 ssl-cert

2. - abra um navegador e acesse o IP, deve abrir a pagina "Apache2 It works!";

3. - Criar a estrutura de diretório (use -p para não gerar erro):
         
         sudo mkdir -p /var/www/revistas/public_html
         sudo mkdir -p /var/www/monografias/public_html

4. - Conceder permissões:
- [x] - para que nosso usuário atual consiga modificar os arquivos;
   
         sudo chown -R $USER:$USER /var/www/revistas/public_html
         sudo chown -R $USER:$USER /var/www/monografias/public_html
         
- [x] - garantir que o acesso de leitura seja concedido ao diretório Web geral;
         
         sudo chmod -R 755 /var/www

5. - Criar páginas de teste para cada host virtual:
- [x] - criando arquivo index.html em cada site;
   
         nano /var/www/revistas/public_html/index.html
         
- [x] - use o HTML abaixo:

        <html>
          <head>
            <title>Bem vindo ao portal ded revistas!</title>
          </head>
          <body>
            <h1>Portal de revista esta online!</h1>
          </body>
        </html>

  
         nano /var/www/monografias/public_html/index.html
   
- [x] - use o HTML abaixo:

        <html>
          <head>
            <title>Bem vindo ao portal ded monografias!</title>
          </head>
          <body>
            <h1>Portal de monografias esta online!</h1>
          </body>
        </html>

6 - Criar arquivos do host virtual:
- OSB: O Apache vem com um arquivo de host virtual padrão chamado 000-default.conf
- podemos copiar ou criar do zero os nossos, vamos copiar.
 
         sudo cp /etc/apache2/sites-available/000-default.conf /etc/apache2/sites-available/revistas.com.conf
         
- [x] - edite o arquivo revistas.com.conf;
         
         sudo nano /etc/apache2/sites-available/revistas.com.conf
  
- [x] copie o exemplo abaixo:


         <VirtualHost *:80>
          ServerAdmin seu.email@email.com
          ServerName revistas.com
          ServerAlias www.revistas.com
          DocumentRoot /var/www/revistas/public_html
          ErrorLog ${APACHE_LOG_DIR}/error.log
          CustomLog ${APACHE_LOG_DIR}/access.log combined
         </VirtualHost>
         

         sudo cp /etc/apache2/sites-available/revistas.conf /etc/apache2/sites-available/monografias.com.conf
         sudo nano /etc/apache2/sites-available/monografias.com.conf
  
- [x] - copie o exemplo abaixo:


         <VirtualHost *:80>
          ServerAdmin seu.email@email.com
          ServerName monografias.com
          ServerAlias www.monografias.com
          DocumentRoot /var/www/monografias/public_html
          ErrorLog ${APACHE_LOG_DIR}/error.log
          CustomLog ${APACHE_LOG_DIR}/access.log combined
         </VirtualHost>

7 - Habilitar os novos arquivos de host virtual:
         
         sudo a2ensite revistas.com.conf
         sudo a2ensite monografias.com.conf
         
- [x] - desabilite o site padrão definido em 000-default.conf;
         
         sudo a2dissite 000-default.conf

8 - Configurar o arquivo de hosts locais (opcional que recomendo):
 
         sudo nano /etc/hosts
- [x] - exemplo abaixo:

         127.0.0.1       localhost
         127.0.1.1       portal.rede     portal
         #192.168.0.16 revistas.com
         192.168.0.16 revistas.com
         192.168.0.16 monografias.com

         # The following lines are desirable for IPv6 capable hosts
         ::1     localhost ip6-localhost ip6-loopback
         ff02::1 ip6-allnodes
         ff02::2 ip6-allrouters

9 - reinicie o servidor ou o apache2

         sudo service apache2 restart
- [x] - ou
  
         sudo init 6

OBS: para testar os site no modo texto use o navegador w3m

VM pronta para baixar: https://1drv.ms/u/s!AjncOwJSKLqhgmHgj3tV7RTB8hHr?e=BAaCgD
- atualizada em 16/07/2022


# Parte-03

instalando o portal de revistas

Após atualizar, faça a atualização do repositório com
          sudo apt update
          sudo apt upgrade


############################################################
Instalando o MySQL Server (mariaDB) e Client:

logar via ssh com aluno@IP-DO-SERVIDOR
1 - 
          sudo apt install mariadb-server mariadb-client libmariadb-dev

Instalando o PHP7.4 e bibliotecas necessárias:
2 - 
          sudo apt install libapache2-mod-php7.4 php7.4 php7.4-common php7.4-curl php7.4-dev php7.4-gd php7.4-imagick php7.4-mysql php7.4-ps php7.4-xsl php7.4-mbstring php7.4-xml php7.4-cli php7.4-soap php7.4-zip php7.4-intl php7.4-curl

Instalando o PHPMyAdmin:
3 - 
          sudo apt install phpmyadmin

reinicie o servidor:
          sudo init 6

criando o arquivo index.php na pasta /var/www/revistas/public_html :
          sudo touch index.php

editar o arquivo:
          sudo nano index.php

adicione a função PHP:
<?php phpinfo()?>

Teste se o PHP no navegador (o IP pode mudar):
http://192.168.0.17/index.php

3 - estando tudo ok, remova o index.html e index.php:
            sudo rm *

entre no mariaDB cliente:
4 - 
            sudo mysql -u root

 a seguir, substituindo o valor do login e senha pelos valores que desejar:
 CREATE USER 'login-aqui'@'localhost' IDENTIFIED BY 'senha-aqui';
4.1 - 
            CREATE USER 'aluno'@'localhost' IDENTIFIED BY 'senha';
 
Agora vamos conceder as permissões necessárias para este novo usuário. Não se esqueça de utilizar aqui o mesmo login utilizado no comando anterior:


GRANT ALL PRIVILEGES ON * . * TO 'login-aqui'@'localhost';
4.2 - 
            GRANT ALL PRIVILEGES ON * . * TO 'aluno'@'localhost';

Agora, é necessário salvar as alterações, executando o comando abaixo:
4.3 - 
            FLUSH PRIVILEGES;
4.4 - 
            quit;

crie um banco com o nome do portal da revista no phpmyadmin:
4.5 - revistas

5 - baixando e instalando o OJS:
https://pkp.sfu.ca/ojs/ojs_download/
 navegue com seu navegador na página de download. Com o mouse sobre o link de download da versão mais recente (formato tar.gz), ao invés de clicar para download, clique com o botão direito do mouse, onde encontrará a opção “Copiar endereço do link”. Esta opção vai salvar o endereço do link na área de transferência.
# caso o wget não esteja instalado: sudo apt install wget
5.1 - 
            sudo wget https://pkp.sfu.ca/ojs/download/ojs-3.3.0-11.tar.gz


O próximo passo é descompactar o arquivo:
5.2 - 
            sudo tar -xzvf ojs-3.3.0-11.tar.gz

Agora, vamos mover os arquivos para a pasta /var/www/revistas/public_html/ com o comando a seguir:
5.3 - 
            sudo mv -f /var/www/revistas/ojs-3.3.0-11/* /var/www/revistas/public_html/

removendo o arquivo e pasta ojs-3....
5.4 - 
            sudo rm ojs-3.3.0-11.tar.gz
5.5 - 
            sudo rm -rf ojs-3.3.0-11/

5.6 - OBS: Torne os seguintes arquivos e diretórios (e seu conteúdo) graváveis ​​(ou seja, alterando o proprietário ou as permissões com chown ou chmod):
 - config.inc.php (opcional — se não for gravável, você será solicitado a substituir manualmente este arquivo durante a instalação)
 - public
 - cache
 - cache/t_cache
 - cache/t_config
 - cache/t_compile
 - cache/_db
5.7 - 
            sudo chown -R www-data config.inc.php public/ cache/ 

# Com os arquivos na pasta correta, precisamos agora conceder permissão de leitura e escrita na pasta do periódico, pois o OJS realiza cache do HTML renderizado, onde é possível otimizar a performance. Esta ação é necessária, pois o OJS3 não funciona sem este recurso.
# É altamente recomendável que esse diretório seja colocado em um local não acessível pela Web para garantir um ambiente seguro (ou protegido de acesso direto, como por meio de regras .htaccess)
# crie o diretorio:
5.8 - 
            sudo mkdir /var/www/revistas/ojs_files
5.9 - 
            sudo chmod 777 -R /var/www/revistas/ojs_files
5.10 - 
            sudo chown -R $USER:$USER /var/www/revistas/ojs_files


# Feito isso, já conseguiremos visualizar a tela de configuração do OJS no navegador. Acesse através do navegador o IP do seu servidor, exemplo:

http://IP-DO-SERVIDOR

na pagina de instalação: 
# veja se todas as condições estão ok

6 - informe os dados do admin do portal de revista:
nome: aluno
senha: senha
email: aluno@email.com

7 - Locale Settings:
7 - verificar se o servidor suporta mbstring: yes

8 - Primary locale:
escolha português (brasil)

Additional locales:
English
Español (España)
português (brasil)

9 - Client character set:
UTF-8

10 - Connection character set:
UTF-8

11 - File Settings
Directory for uploads:
/var/www/revistas/ojs_files

12 - Database Settings
Database driver:
# esse dados foi usado na preparação do servidor, ver acima;
MySQL
Host: localhost
username: aluno
password: senha
database name: revistas

OAI Settings:
Repository IDENTIFIer:
revistas.192.168.0.17 (o IP pode ser diferente)

13 - Install Open Journal systems

14 - você deve receber a mensagem abaixo:
14.1 - Installation of OJS has completed successfully.

15 - click em login e crie as revistas

VM para baixar: https://1drv.ms/u/s!AjncOwJSKLqhgmKQ2QyqZZBra24Z?e=jRW5Of

