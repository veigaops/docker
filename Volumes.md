#montar um volume qdo já temos um diretório

#Volume do tipo Bind

docker container run -ti --mount type=bind,src=/opt/giropops,dst=/giropops debian

#Volume somente leitura

docker container run -ti --mount type=bind,src=/opt/giropops,dst=/giropops,ro debian

#Comando Docker volume

#Criar um volume
docker volume create giropops

#Listar Volumes

docker volume ls

#inspecionar
docker volume inspect giropops
[
    {
        "CreatedAt": "2023-01-23T11:57:51-05:00",
        "Driver": "local",
        "Labels": {},
        "Mountpoint": "/var/lib/docker/volumes/giropops/_data",
        "Name": "giropops",
        "Options": {},
        "Scope": "local"
    }
]


#Criar 2 diretórios
 cd /var/lib/docker/volumes/giropops/_data
 docker volume ls

 #adicionar o volume à imagem
 docker container run -ti --mount type=volume,src=giropops,dst=/giropops debian

# Remover volume
 docker volume rm giropops
Error response from daemon: remove giropops: volume is in use - [5009c4580fce1a3f02e9318c037a1044906b39a423cf7e14be11b8d4904f3f21]

# Ir apagando todos os container que fazem referência ao volume
docker container rm 5009c4580fce

# Apagar todo volume que não estiver sendo utilizado por container
docker volume prune

# Remover todos os containers que estiverem parados
docker container prune

# Criar um container com volume (Antigamente) dataonly container
docker container create -v /data --name dbdados centos
docker run -d -p 5432:5432 --name pgsql1 --volumes-from dbdados -e POSTGRESQL_USER=docler -e POSTGRESQL_PASS=docker -e POSTGRESQL_DB=docker kamui/postgresql

docker run -d -p 5433:5432 --name pgsql2 --volumes-from dbdados -e POSTGRESQL_USER=docler -e POSTGRESQL_PASS=docker -e POSTGRESQL_DB=docker kamui/postgresql

# Debug 
docker logs -f ffb0e2db0bff
2023-01-23 17:45:45 UTC FATAL:  data directory "/data" has wrong ownership
2023-01-23 17:45:45 UTC HINT:  The server must be started by the user that owns the data directory.


# Criar um volume da volumes e subir 2 serviços do Postgres
docker volume create dbdados
 docker volume ls
DRIVER    VOLUME NAME
local     02d932c4985f2f85bb2dd2962d66be73d4b0ad4e8f9040407e963f2b69683da2
local     67d70b5800cfb9227a472fa2f0763fabe6064f46430419e70c07604ac27915ac
local     b357821e9d3c16d3263ca9abee97a3ca072af1106bf725eef8e114880a25cc26
local     dbdados
local     giropops

docker run -d -p 5432:5432 --name pgsql1 --mount type=volume,src=dbdados,dst=/data -e POSTGRESQL_USER=docker -e POSTGRESQL_PASS=docker -e POSTGRESQL_DB=docker kamui/postgresql

docker run -d -p 5433:5432 --name pgsql2 --mount type=volume,src=dbdados,dst=/data -e POSTGRESQL_USER=docker -e POSTGRESQL_PASS=docker -e POSTGRESQL_DB=docker kamui/postgresql

docker volume inspect dbdados

# Fazendo Backup do ambiente
mkdir /opt/backup
docker container run -ti --mount type=volume,src=dbdados,dst=/data --mount type=bind,src=/opt/backup,dst=/backup debian tar -cvf /backup/bkp-banco.tar /data