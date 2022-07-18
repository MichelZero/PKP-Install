
# Parte-01


## Criando a maquina Linux VBox no Windows: 
1. - depois de criada verificar a placa de REDE;
2. - memória, processador, vídeo;

## Como administrador no Power Shell;
3. - ativar VT-x com:
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

