---
tags:
  - linux
  - comandos
  - bash
  - cli
title: Linux🐧
date: 2024-04-22
---
___
## comando basicos

```bash
$ ip -br -c a #verificar IP com mais precisao
$ cat /etc/*-release
$ cat /etc/*-release | grep PRETTY 
$ lsb_release -a 
$ uname -a 
$ uname -mrs 
$ cat /proc/version
$ usermod -aG wheel user #ADD SUDO 
$ pdftk largepdfile.pdf burst #DIVIDIR PDFS 
$ pdftk 1.pdf 2.pdf 3.pdf cat output 123.pdf #JUNTAS PDFS
$ pkill -KILL -u yourusername #reniciar tty1
$ sudo restart lightdm #reniciar tty1
$ vboxmanage hostonlyif create #VIRYTUALBOX 
$ ifconfig vboxnet0 up         #VIRTUALBOX
$ cd /usr/share/font && fc-cache -f -v #UPDATE FONTS
$ localectl list-keymaps #ALTERAR TECLADO CENTOS
$ localectl set-keymap br-abnt2 #ALTERAR TECLADO CENTOS
$ find ./* -type f -exec grep -l TESTE {} \; #BUSCAR NO DISCO
$ find /opt/rotinas/* -type f -exec grep -l job-carga-resultado-indicador.php {} \;
$ du -sch #TAMANHO DO DIRETORIO
$ cat /proc/cpuinfo |grep 'cpu cores'
$ systemctl status firewalld #VERIFICAR FIREWALL (START/STOP)
$ netstat -tulpn | grep :22 #VIRIFICANDO PORTARS ESCUTANDO EX:22
$ ss -lnt
$ yum list installed
$ stress -c 24 --vm 2 --io 50 --vm-bytes 60G -t 60s #teste estress
$ stress -c 32 --vm 2 --io 50 --vm-bytes 4G -t 30s # -c qtde de processadores e comecar gradativo a qtde de memoria
$ ln -s [caminho diretorio] [nome]
$ yum remove `rpm -q kernel | grep -v 'uname -r'` #remover todos os kernels somente fico o em uso
$ lsblk verificar #tamanho total do disco
$ grep -r palavra [caminho] #BUSCA NO ARQUIVO PALAVRA ESPECIFICA
$ dracut -v -f
$ w # verifica user e cpu etc
$ whatis # saber o q é o comando 
$ wheris # saber onde esta o binario
$ whoami 
$ uptime
$ df -h # disco
$ df -ih # verificar inodes
$ touch # troca timestemp do arquivos
$ tune2fs  #(disco)
$ jobs # comandos em back
$ nohup # para background &
$ vmstat # verificar do sistema como um todo 
$ top
$ htop
$ nmon # monitoramento de hardware
$ iptraf-ng # monitorar placa de rede
$ tcpdump -A -s -i -vv #  dumo do trafico
$ iotop # io do disco
$ iftop # monitora com grafico trafico de rede
$ ansible-playbook playbook.yml --vault-password-file ~/.vault_pass.txt -b #executar playbook ansible
$ ssh-copy-id -i deploy@10.90.1.11 #logar sem senha
$ openssl s_client -showcerts -host host-semhttp -port 443 </dev/null #verificar cadea de certificados 
$ sudo update-alternatives --config editor #configurar vim padrao do sistema ubuntu
```



[[index]]