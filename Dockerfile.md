# Criação de um Dockerfile

vim Dockerfile

FROM debian
LABEL app="Giropops"
ENV JEFERSON="LINDO"
RUN apt-get update && apt-get install -y stress & apt-get clean

CMD stress --cpu 1 --vm-bytes 64M --vm 1

#Criar uma imagem
docker image build -t toskeira:1.0 .

# Listar uma imagem
docker image ls

#Rodar a imagem criada
docker container run -d toskeira:1.0