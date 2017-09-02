[DESCOMPLICANDO O DOCKER](https://www.youtube.com/watch?v=0xxHiOSJVe8&list=PLf-O3X2-mxDkiUH0r_BadgtELJ_qyrFJ_)(LinuxTips)
---

## DESCOMPLICANDO O DOCKER V1 - 06 - Administrando Containers Docker

* docker create PARAMETROS IMAGEM
   
   * cria um container, mas não o inicializa
   
   parametros:
    
    * **--name** : define um nome pro container. Não é o nome do host
   
   imagem: nome da imagem 
 
***Exemplo:***
```bash
docker create ubuntu --name renato
1cd67c909efcdb6203185675c6d55b76bc8ba8aba79c56117eba3b66739b201d
docker ps -a
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS               NAMES
9bc13bc535aa        ubuntu              "/bin/bash"         7 seconds ago       Created                                 renato
```    


* docker run PARAMETROS IMAGEM COMANDO

imagem:
  
Uma imagem local, caso não tenha a imagem local. Será feito o download  

parametros: 

   * **-t**: terminal
     Conecta em um terminal no container 
   * **-i**: interativo
     Modo interativo

Exemplo:

```bash

docker run -it ubuntu /bin/bash

```

Para sair de um container **ctrl +d** . O container será encerrado 

Para sair de um container sem encerrá-lo **ctrl +p +q**

* Vizualizar containeres parados


```bash
docker ps -a

CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS                         PORTS               NAMES
36eae871c067        ubuntu              "/bin/bash"         26 seconds ago      Exited (0) 12 seconds ago                          unruffled_dijkstra
6fe711691772        ubuntu              "/bin/bash"         39 seconds ago      Up 37 seconds                                      silly_bardeen
f96324d24209        ubuntu              "/bin/bash"         4 minutes ago       Exited (0) 4 minutes ago                           keen_beaver
21842bd1fb0e        ubuntu              "/bin/bash"         31 minutes ago      Created                                            quizzical_pike
f34ff4a581d1        centos:7            "/bin/bash"         39 minutes ago      Exited (0) 31 minutes ago                          agitated_pasteur
f8a176c9d761        ubuntu              "/bin/bash"         42 minutes ago      Exited (130) 41 minutes ago                        serene_banach
619fcf1fbf6c        hello-world         "/hello"            About an hour ago   Exited (0) About an hour ago                       sad_mcclintock



```

* Visualizar containeres em execução


```bash
docker ps

CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS               NAMES
6fe711691772        ubuntu              "/bin/bash"         12 minutes ago      Up 12 minutes                           silly_bardeen

```

* Voltar para o shell de um containeres no modo interativo

   * docker attach CONTAINER ID
     
     CONTAINER ID: 

      *Para descobrir o código do container rode **docker ps** 


```bash
docker attach f34ff4a581d1

[root@f34ff4a581d1 /]#
```

* Para parar um container

docker stop CONTAINER ID

```bash
docker stop f34ff4a581d1

docker ps -a

CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS 
f34ff4a581d1        centos:7            "/bin/bash"         About an hour ago   Exited (137) 13 seconds ago                                                    

PORTS               NAMES
                    agitated_pasteur
```

* Para inicalizar um container

docker start CONTAINER ID

```bash
docker start f34ff4a581d1

docker ps

CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS               NAMES
f34ff4a581d1        centos:7            "/bin/bash"         About an hour ago   Up 6 seconds                            agitated_pasteur

```

* Para pausar um container

docker pause CONTAINER ID

```bash
docker pause f34ff4a581d1

docker ps
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS                           PORTS               NAMES

docker ps -a
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS                           PORTS               NAMES
f34ff4a581d1        centos:7            "/bin/bash"         About an hour ago   Up 2 minutes (Paused)                                agitated_pasteur
```

* Para parar um container

docker upause CONTAINER ID

```bash
docker upause f34ff4a581d1

docker ps

CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS               NAMES
f34ff4a581d1        centos:7            "/bin/bash"         About an hour ago   Up 6 minutes                            agitated_pasteur

```

* Para destruir um container

docker rm CONTAINER ID

   * **-f**: Força de destruição de um container em execução

```bash
docker rm f34ff4a581d1

docker ps 
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS               NAMES


docker ps -a

CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS                           PORTS               NAMES
36eae871c067        ubuntu              "/bin/bash"         About an hour ago   Exited (0) About an hour ago                         unruffled_dijkstra
f96324d24209        ubuntu              "/bin/bash"         About an hour ago   Exited (0) About an hour ago                         keen_beaver
21842bd1fb0e        ubuntu              "/bin/bash"         About an hour ago   Created                                              quizzical_pike
f8a176c9d761        ubuntu              "/bin/bash"         About an hour ago   Exited (130) About an hour ago                       serene_banach
619fcf1fbf6c        hello-world         "/hello"            2 hours ago         Exited (0) 2 hours ago                               sad_mcclintock
```
   * o container some da lista


* Visualizar recursos consumidos pelo container

docker stats CONTAINER ID

```bash
docker stats 36eae871c067

CONTAINER           CPU %               MEM USAGE / LIMIT   MEM %               NET I/O             BLOCK I/O           PIDS
36eae871c067        0.00%               568KiB / 15.58GiB   0.00%               5.13kB / 90B        0B / 0B             1
```

* Visualizar processos do container

docker top CONTAINER ID

```bash
docker top fec534563d74

UID                 PID                 PPID                C                   STIME               TTY                 TIME                CMD
root                18145               18127               0                   23:03               pts/0               00:00:00            /bin/bash
root                18285               18145               0                   23:04               pts/0               00:00:00            sleep 900
```

* Inspecionar comando realizados no container

```bash
docker logs fec534563d74
root@fec534563d74:/# sleep 900
```

## DESCOMPLICANDO O DOCKER V1 - 07 - Limitando CPU e MEM dos containers e o Docker Update

   ***Nota*** Quando você cria um container. Ele não tem limites de **CPU** e **Memória RAM**

* docker inspect: Visualiza informações sobre um determinado container

docker inspect CONTAINER ID

***Exemplo:***
```bash
docker inspect 9bc13bc535aa | grep -i memory
"Memory": 0,
"KernelMemory": 0,
"MemoryReservation": 0,
"MemorySwap": 0,
"MemorySwappiness": -1,
```

Para definir os limites de memoria de um container basta passar o parâmetro **--memory/-m** TAMANHO(MB,GB ...) com o comando **run** ou **create**

docker run/create --memory/-m NOVO TAMANHO CONTAINER ID

***Exemplo***

```bash
docker run --memory 512mb --name renato512mb -ti ubuntu
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS                     PORTS               NAMES
af74d84dbbb1        ubuntu              "/bin/bash"         19 seconds ago      Exited (0) 5 seconds ago                       renato512mb

docker inspect af74d84dbbb1 | grep -i memory
"Memory": 536870912,
"KernelMemory": 0,
"MemoryReservation": 0,
"MemorySwap": 1073741824,
"MemorySwappiness": -1,
```

Para definir os limites de cpu de um container basta passar o parâmetro **--cpus-shares/-c** PRIORIDADE(1-1024) com o comando **run** ou **create**
 
*Para balancear o uso da CPU pelos containers, utilizamos especificação de pesos para cada container, quanto menor o peso, menor sua prioridade no uso. Os pesos podem oscilar de 1 a 1024.*

Para entendimento, vamos imaginar que três containers foram colocados em execução. Um deles tem o peso padrão 1024 e dois têm o peso 512. Caso os três processos demandem toda CPU o tempo de uso deles será dividido da seguinte maneira:

    O processo com peso 1024 usará 50% do tempo de processamento
    Os dois processos com peso 512 usarão 25% do tempo de processamento, cada.


docker run/create --cpu-shares/-c PRIORIDADE CONTAINER ID

***Exemplo***

```bash
docker run -c 1024 --name renato -ti debian
docker inspect renato | grep -i shares
            "CpuShares": 1024,
```

Para alterar os limites de cpu de um container deve-se usar o comando **update**

docker update --cpu-shares/-c PRIORIDADE CONTAINER ID 

***Exemplo***

```bash
docker update -c 512 af74d84dbbb1
docker inspect renato | grep -i shares
            "CpuShares": 512,
```
