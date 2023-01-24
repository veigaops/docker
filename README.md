# docker

# Instalar o docker

yum install yum-utils
sudo yum-config-manager     --add-repo     https://download.docker.com/linux/centos/docker-ce.repo
sudo yum install docker-ce docker-ce-cli containerd.io docker-compose-plugin --allowerasing

# Comandos para verificar os containers
docker container ps

#Executar uma imagem
docker container run -ti hello-world
docker run -it ubuntu

#Listar os containers
docker container ls
docker container ls -a

#para sair do Container
CTRL+D --> mata o container
CTRL+P+Q -->sai sem matar o container

#Para se conectar ao container
docker container attach <container_id>

#Para que o container possa ser executado como daemon
docker container run -d nginx

#Para acessar o container
docker container exec -ti eb0f5b29780e bash
docker container exec -ti eb0f5b29780e ls /usr/share nginx/html

#parar/reiniciar um container
docker container stop <container_ID>
docker container start <container_ID>
docker container restart <container_ID>

#Verificar informações do container
docker container inspect <container_ID>

#Pausar/despausar os containers
docker container pause <container_ID>
docker container unpause <container_ID>

#Verificar Logs
docker container logs -f <container_ID>

#Remover container
docker container rm -f  <container_ID>
docker container rm -f  xxx

#Verificar o status de cpu/mem
docker container stats <container_ID>

#limitar utilização de memória no container
docker container run -d -m 128M nginx

#limitar utilização de cpu no container
docker container run -d -m 128M --cpus 0.2 nginx

#Update
docker container update --cpus 0.8 <container_ID>