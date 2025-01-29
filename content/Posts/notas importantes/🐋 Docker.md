---
tags:
  - docker
  - linux
  - cli
  - comandos
title: 🐋 Docker
date: 2024-04-22
---

----

[[🐧Linux| ]][[index| ]] 
## comandos iniciais

```bash
$ docker ps -a #LISTA AS IMAGENS
$ docker start nome da imagem/container
$ docker exec -it nome do container bash
$ docker system prune
$ docker exec -it container-go bash
$ docker run -d -p 8000:8000 -p 9443:9443 --restart=always -v /var/run/docker.sock:/var/run/docker.sock -v portainer_data:/data portainer/portainer-ce:latest
$ docker run -it --name teste alpine sh #CONTAINER TESTE
$ docker run -it --name az-cli mcr.microsoft.com/azure-cli sh #subir container azure cli

```

```bash
#add hosts do container com docker-compose
--add-host="endereco.io:0.0.0.0" \
--add-host="endereco2.io:1.1.1.1." \
```

```bash
#comando para executar me shell script (remover containers)
docker ps | grep 'Up 1 week' | awk '{print $1}'
docker ps | grep 'Up 3 hours' | awk '{print $1}' | | xargs --no-run-if-empty docker stop
docker system prune --all -f
docker container ls -a --size --format "table {{.ID}}\t{{.Names}}\t{{.Image}}\t{{.Size}}"
```

## comandos basiado curso youtube

```bash
$ docker system info #verificar estado do docker
$ docker search #procura images ex: docker search debian
$ docker image pull alpine #baixar somente a imagem
$ docker container run -dit --name alpine --hostname teste alpine #criando container
$ docker container run -dit -p 22:22 --name debian-ssh --hostname debian-ssh ssh #subir container para teste ssh/buildar a imagem antes
$ echo 'teste' > /tmp/teste.log && docker container cp /tmp/teste.log alpine:tmp # copiando arquivo para dentro do container
$ docker container exec alpine ls -l /tmp # conferindo o arquivo
$ docker container run --rm -it hello-world #rm remove quando terminar
$ docker ps -a #temos tambem container/volume/image ls -a
$ docker ps -aq #somente ID
$ docker rm -f ID #remove o container forcando
$ docker rm $(docker ps -aq) #remove todos containers com subshell 
$ ssh ip172-18-0-165-c9apb9433d5g00d0fqog@direct.labs.play-with-docker.com #comando para acessar do seu terminal com ssh o play with docker
$ docker container run -it -p 8080:8080 pengbai/docker-supermario #rodar super mario no play with docker
$ docker container run --rm -it echo-container #container é removido quando sai
$ docker login -u douglastos #logar ho docker hub, e usar o logout para sair
$ docker history debian #verificar todas camadas criadas (historico)
$ docker inspect debian #verificar inspeciona mais profundo as camadas
$ docker container commit debian_container debian-imagem-http #cria imagem a partir do container
$ docker image save debian-imagem-http -o debian-imagem-http.tar #criar backup da imagem
$ docker image load -i debian-imagem-http.tar #restore da imagem
$ docker image build -t echo-container . #buildar uma imagem a partir do arquivo Dockerfile
$ docker image build --no-cache -t echo-container . #buildar uma imagem a partir do arquivo Dockerfile sem cache
$ docker image ls | head -n1 && docker images | grep debian #fitro para comparacao de images (purto linux)
$ docker image ls | egrep "REPO|debian" #fitro para comparacao de images (purto linux)

```

## COMANDOS NO DOCKERFILE

```dockerfile
FROM        #-> diz qual é a origem da imagem (imagem base)
COPY        #-> copia arquivos de path para o container
RUN         #-> executa comandos dentro do container
ADD         #-> Quase igual copy, mais aceita URL e alterar permissoes 
EXPOSE      #-> Expoe uma porta (só convenincia no Dockerfile)
ENTRYPOINT  #-> ponto de entrada do container (o que mantem o container vivo)
CMD         #-> Argumentos para o entreypoint

# DICA 1 : A ORDEM IMPORTA PARA O CACHE
# DICA 2 : COPY mas especifico para limitar a quebra de cache
# DICA 3 : identifique as instruçoes que podem ser AGRUPADAS.
# DICA 4 : remova dependecias desnecessarias ex usar "--no-install-recommends"
# DICA 5 : remover cache do gerenciador de pacotes
# DICA 6 : usar imagem oficial
# DICA 7 : utilize tags mais especificas
# DICA 8 : procure por falvors minimos (procurar por slim "debian GNU->libc" ou alpine musl->libc)
```


## volumes

```dockerfile
VOLUME # verificar qual o volume para usar com docker system info

alterar o drive storage # /etc/docker/daemon.json

#criando um volume bind -> -v origem:destino
$ docker container run -dit --name server -v /srv:/srv debian

#ciando volume anonimo -> -v destino (nesse modo ele cria um volume com um hash)
$ docker container run -dit --name server -v /volume debian

#criando volume nomeado -> -v (sem PATH, somente o nome)
$ docker container run -dit --name server -v volume:/volume debian

#criando volume com mount:
$ docker container run -dit --name server2 --mount source=volume2,target=/volume2 debian

#copiando arquivo com comando docker container cp
$ docker container cp ~/dockerfiles webserver:webdata

#trazendo conteudo de um container rodando
$ docker container run -dit --volumes-from webserver --name volumetest debian
```


## netowrks

```bash

#recursos de redes utilizados pelo docker

$ # veth -> Virtual ethernet 
$ # bridge -> roteamento do host pra o container
$ # iptables -> firewall linux
$ #dokcer por padrao nao publica portas para o mundo
$ #comando para publicar portas
$ docker container run -dit --name web -p 7070:80 nginx
$ #  -p / --publish ORIGEM:DESTINO -> host:container ex: 7070:80 o acesso sera localhost:7070
$ # -p 7070:80 -> PEGUE TODA CONEXAO NA PORTA 7070 DO HOST E DIRECIONE PARA PORTA 80 DO CONTAINER
$ # DRIVERS DE REDE \
$ #  - bridge \
$ #  - host \
$ #  - overlay \
$ #  - macvlan \
$ #  - none \
$ #  - plugins de drivers de rede
$ # REDE BRIDGE -> resolucao DNS + conecte e desconecte containers
$ docker container run -dit --name web --hostname webteste --network bridge -p 8080:80 nginx #comando para setar rede bridge
$ # comando para verificar como funciona esse comando na rede
$ curl localhost:8080 # verifica o que esta respondendo na porta 8080
$ sudo ss -ntpl | grep 80 
$ sudo iptables -nL 
$ docker container run -dit --name webhost --hostname webteste --network host nginx #comando para subir container com rede host (sem roteamento usando Ip da maquina local)
$ docker container run -dit --name semrede --network none alpine ash #comando para subir container com rede none entrepoynt ash
$ docker container exec semrede ip link show #comando para verificar so tem o loopback
$ # REDE BRIDGE DEFAULT NAO RESOLVE NOMEcd .
$ # USER-DEFINED BRIDGE NETWORK (RESOLVE DNS) 
$ # BENEFICIO DE USAR (USER-DEFINED BRIDGE)
$ # - DNS AUTOMATICO 
$ # - MELHOR ISOLAMENTO 
$ # - CONECTAR E DESCONECTAR ON-THE-FLY
$ # - CONFUGRACAO PERSONALIZADAS
$ docker network create --driver bridge --subnet 172.20.0.0/16 dca-lan #COMANDO PARA CRIAR REDE USER DEFINED
$ docker container run -dit --name servidor-dcalan -h servidor-dcalan --network dca-lan debian #criar container para testar a rede criada
$ docker exec servidor-dcalan ping -c 4 client-dcalan #testetano dns da rede
PING client-dcalan (172.20.0.3) 56(84) bytes of data.
64 bytes from client-dcalan.dca-lan (172.20.0.3): icmp_seq=1 ttl=64 time=0.129 ms
64 bytes from client-dcalan.dca-lan (172.20.0.3): icmp_seq=2 ttl=64 time=0.147 ms
64 bytes from client-dcalan.dca-lan (172.20.0.3): icmp_seq=3 ttl=64 time=0.139 ms
64 bytes from client-dcalan.dca-lan (172.20.0.3): icmp_seq=4 ttl=64 time=0.149 ms
$ docker network disconnect dca-lan client-dcalan #comando para desconectar o cabo de rede
$ docker network connect --ip 172.20.0.200 dca-lan client-dcalan #reconectando o container a rede com IP de preferencia.
$ docker exec -it servidor-dcalan ping -c 4 client-dcalan
PING client-dcalan (172.20.0.200) 56(84) bytes of data.
64 bytes from client-dcalan.dca-lan (172.20.0.200): icmp_seq=1 ttl=64 time=0.161 ms
64 bytes from client-dcalan.dca-lan (172.20.0.200): icmp_seq=2 ttl=64 time=0.118 ms
64 bytes from client-dcalan.dca-lan (172.20.0.200): icmp_seq=3 ttl=64 time=0.119 ms
64 bytes from client-dcalan.dca-lan (172.20.0.200): icmp_seq=4 ttl=64 time=0.699 ms

--- client-dcalan ping statistics ---
4 packets transmitted, 4 received, 0% packet loss, time 3006ms
rtt min/avg/max/mdev = 0.118/0.274/0.699/0.245 ms
$
```

## DOCKER SWARM

``` bash
# COMPOSICAO DO CLUSTER

# MASTER -> MANAGER
# NO1 -> WORKER
# NO2 -: WORKER
# RAFT CONSENSUS -> UTILIZAR NO MAXIMO ATE 7 NOS MANAGER
# [1] -> 3 -> 5 -> [7]
# start no no1 comando
$ docker swarm init --advertise-addr (IP) #nó atual
Swarm initialized: current node (v7pffbu3tgltd16k6gjhguzrm) is now a manager.

To add a worker to this swarm, run the following command:

    docker swarm join --token SWMTKN-1-2fv3z869jsh4rh8y8d7ho7y7mqkuxdc8ym6k1hztb0sqaw61g9-7cikh9rauhlzxw5djgmbb34xz 192.168.0.8:2377

To add a manager to this swarm, run 'docker swarm join-token manager' and follow the instructions.

$ docker node ls
ID                            HOSTNAME   STATUS    AVAILABILITY   MANAGER STATUS   ENGINE VERSION
v7pffbu3tgltd16k6gjhguzrm *   node1      Ready     Active         Leader           20.10.0
n8xy83vb3a8cikfwi4tzfmk8q     node2      Ready     Active                          20.10.0
2g3n77ow9deebvtcg28vvosj0     node3      Ready     Active                          20.10.0

$ docker node promote node3 #promovendo o node3 para manager
Node node3 promoted to a manager in the swarm.

$ docker node ls
ID                            HOSTNAME   STATUS    AVAILABILITY   MANAGER STATUS   ENGINE VERSION
v7pffbu3tgltd16k6gjhguzrm *   node1      Ready     Active         Leader           20.10.0
n8xy83vb3a8cikfwi4tzfmk8q     node2      Ready     Active                          20.10.0
2g3n77ow9deebvtcg28vvosj0     node3      Ready     Active         Reachable        20.10.0

$ docker node demote node3 #promovendo o node3 a worker
Manager node3 demoted in the swarm.
$ 

#CONCEITOS 
# SERVICES -> SERVIÇO -> ESTADO DESEJADO
# TASKS -> TAREFA

# SERVICE             -> TASK    -> CONTAINER (RESULTADO)
#  3 REPLICAS NGINX   -> nginx.1 -> nginx:latest
#                     -> nginx.2 -> nginx:latest
#                     -> nginx.3 -> nginx:latest
# SERVICOS REPLICADOS X GLOBAIS
# GLOBAL -. RODA EM TODOS OS NOS
# REPLICADO ->  RODA EM UMA QUANTIDADE DETERMINADA

# PROXIMO PASSO COLOCAR MAQUINA REGISTER
#subir 4 nos e executar esse comando em cada um deles 
$ echo '{ "insecure-registries" : ["registry.docker-dca.example:5000"] }' | sudo tee /etc/docker/daemon.json ; sudo systemctl restart docker
#comando é para que cada aceita maquina registry como segura
$ docker container run -dit --name registry -p 5000:5000 registry:2
$ docker pull alpine
$ docker image tag alpine registry.docker-dca.example:5000/alpine
$ docker push registry.docker-dca.example:5000/alpine
# SUBINDO A PRIMEIRA TASK
$ docker service create --name webserver registry.docker-dca.example:5000/nginx
$ docker service ps webserver
ID             NAME          IMAGE                                           NODE                        DESIRED STATE   CURRENT STATE            ERROR     PORTS
75qgrmfaonco   webserver.1   registry.docker-dca.example:5000/nginx:latest   node02.docker-dca.example   Running         Running 38 seconds ago
$ docker service update --publish-add 80 webserver # add uma porta ao servico
$ docker service ps webserver
ID             NAME              IMAGE                                           NODE                        DESIRED STATE   CURRENT STATE                 ERROR     PORTS
s2p653283zkq   webserver.1       registry.docker-dca.example:5000/nginx:latest   node01.docker-dca.example   Running         Running about a minute ago
75qgrmfaonco    \_ webserver.1   registry.docker-dca.example:5000/nginx:latest   node02.docker-dca.example   Shutdown        Shutdown about a minute ago
$ docker service ls
ID             NAME        MODE         REPLICAS   IMAGE                                           PORTS
6oqd0gcg9h1n   webserver   replicated   1/1        registry.docker-dca.example:5000/nginx:latest   *:30000->80/tcp
$  docker service update --replicas 10 pingtest
```

referencia:
curso caio delgado https://www.youtube.com/watch?v=U-GGoWq26C4&list=PL4ESbIHXST_TJ4TvoXezA0UssP1hYbP9_


<script src="https://giscus.app/client.js" data-repo="douglastos/douglastos.github.io" data-repo-id="R_kgDOLvf9iw"
data-category="General" data-category-id="DIC_kwDOLixoLc4CeGqc" data-mapping="title"data-strict="1"data-reactions-enabled="1"data-emit-metadata="0"data-input-position="bottom"data-theme="dark"data-lang="pt"crossorigin="anonymous"async>
</script>