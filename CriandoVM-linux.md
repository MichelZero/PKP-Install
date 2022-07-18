
# Parte-01


## Criando a maquina Linux VBox no Windows: 
1. - depois de criada verificar a placa de REDE;
2. - memória, processador, vídeo;

## Como administrador no Power Shell;
3. - ativar VT-x: exemplo https://youtu.be/iBNytZ3GZWM
- [x] - vá em: cd 'C:\Arquivos de Programas\Oracle\VirtualBox\'
- [x] - execute: ./vboxManage.exe modifyvm "nome-maquina" --nested-hw-virt on

## Instalando o Debian:
1. - escolha a ISO de instalação (baixada do site);
2. - faça a instalação minima do servidor;
- [x] - escolha apenas: utilitário de sistema padrão

VM pronta para baixar: https://1drv.ms/u/s!AjncOwJSKLqhgmCR7a0n_G2KPgA1?e=yQTefV
- atualizada em 16/07/2022


# Parte-02


## Preparando o usuário para administrar:
1. - logar com o usuário comum (aluno);
2. - com o "SU -" abra o terminal de root;

3. - instalar o pacote SUDO e o SSH:
- [x] - apt install sudo
- [x] - apt install ssh

4. - adicione o seu usuário ao grupo SUDO;
- [x] - faça o logoff do root e use o "SU -" novamente;
- [x] - adduser aluno sudo
- [x] - faça o logoff do root;
- [x] - peque o IP com "ip a" anote; aqui ficou 192.168.0.16
- [x] - faça logoff do usário aluno e dai para frente só use o usário Aluno via SSH;

5. - no terminal do power shell do windows, faça login com SSH;
- [x] - ssh aluno@192.168.0.16
- [x] - aceite a chave de criptografia com yes;


## Criando os sites no servidor:
1. - se não tiver instalado os pacotes de serviço web;
- [x] - Instalando o Apache 2;
- [x] - sudo apt install apache2 apache2-utils libexpat1 ssl-cert

2. - abra um navegador e acesse o IP, deve abrir a pagina "Apache2 It works!";

3. - Criar a estrutura de diretório (use -p para não gerar erro):
- [x] - sudo mkdir -p /var/www/revistas/public_html
- [x] - sudo mkdir -p /var/www/monografias/public_html

4. - Conceder permissões:
- [x] - para que nosso usuário atual consiga modificar os arquivos;
- [x] - sudo chown -R $USER:$USER /var/www/revistas/public_html
- [x] - sudo chown -R $USER:$USER /var/www/monografias/public_html
- [x] - garantir que o acesso de leitura seja concedido ao diretório Web geral;
- [x] - sudo chmod -R 755 /var/www

5. - Criar páginas de teste para cada host virtual:
- [x] - criando arquivo index.html em cada site;
- [x] - nano /var/www/revistas/public_html/index.html
- [x] - use o HRML abaixo:

      <html>
        <head>
          <title>Bem vindo ao portal ded revistas!</title>
        </head>
        <body>
          <h1>Portal de revista esta online!</h1>
        </body>
      </html>
      
- [x] - nano /var/www/monografias/public_html/index.html
- [x] - use o HRML abaixo:

      <html>
        <head>
          <title>Bem vindo ao portal ded monografias!</title>
        </head>
        <body>
          <h1>Portal de monografias esta online!</h1>
        </body>
      </html>

6 - Criar arquivos do host virtual:
OSB: O Apache vem com um arquivo de host virtual padrão chamado 000-default.conf
podemos copiar ou criar do zero os nossos, vamos copiar.
6.1 - sudo cp /etc/apache2/sites-available/000-default.conf /etc/apache2/sites-available/revistas.com.conf
6.2 - edite o arquivo revistas.com.conf;
6.3 sudo nano /etc/apache2/sites-available/revistas.com.conf
6.4 copie o exemplo abaixo:
<VirtualHost *:80>
    ServerAdmin seu.email@email.com
    ServerName revistas.com
    ServerAlias www.revistas.com
    DocumentRoot /var/www/revistas/public_html
    ErrorLog ${APACHE_LOG_DIR}/error.log
    CustomLog ${APACHE_LOG_DIR}/access.log combined
</VirtualHost>
6.5 - sudo cp /etc/apache2/sites-available/revistas.conf /etc/apache2/sites-available/monografias.com.conf
6.6 - sudo nano /etc/apache2/sites-available/monografias.com.conf
6.7 - copie o exemplo abaixo:
<VirtualHost *:80>
    ServerAdmin seu.email@email.com
    ServerName monografias.com
    ServerAlias www.monografias.com
    DocumentRoot /var/www/monografias/public_html
    ErrorLog ${APACHE_LOG_DIR}/error.log
    CustomLog ${APACHE_LOG_DIR}/access.log combined
</VirtualHost>

7 - Habilitar os novos arquivos de host virtual:
7.1 - sudo a2ensite revistas.com.conf
7.2 - sudo a2ensite monografias.com.conf
7.3 - desabilite o site padrão definido em 000-default.conf;
7.4 - sudo a2dissite 000-default.conf

8 - Configurar o arquivo de hosts locais (opcional que recomendo)
8.1 - sudo nano /etc/hosts
8.2 - exemplo abaixo:
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
9.1 - sudo service apache2 restart
ou
9.2 - sudo init 6

OBS: para testar os site no modo texto use o w3m

VM pronta para baixar: https://1drv.ms/u/s!AjncOwJSKLqhgmHgj3tV7RTB8hHr?e=BAaCgD
- atualizada em 16/07/2022
