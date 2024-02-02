# Lab 1 - Docker

## Criando a instancia


## Pre-reqs

Usaremos uma máquina virtual no EC2 com a imagem oficial `Amazon Linux` para rodar o Docker:

- Uma maquina virtual com `Amazon Linux`. Seguir os passos do lab [lab 01 - EC2](/shift/multicloud/lab01-iaas-ec2.md). 

We will be using the Docker image `josecastillolema/api` hosted on [Docker Hub](https://hub.docker.com/r/josecastillolema/api) during this class.

The image `josecastillolema/api` cointains the [following files](lab01-docker):
   - [**`api.py`**](lab01-docker/api.py): Uma simples API escrita em Python que usa a biblioteca [Flask](https://flask.palletsprojects.com/en/1.1.x/).
   - [**`requirements.txt`**](lab01-docker/requirements.txt): As dependências da aplicação. Podem ser instaladas usando `pip`, o gestor de dependências do Python.
   - [**`Dockerfile`**](lab01-docker/Dockerfile): O arquivo usado por `docker para criar a imagem
 
## Instalação do Docker
