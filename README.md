# Docker-compose com PostgreSQL e Worpress
O passo a passo sobre alguns aspectos de uma arquitetura em docker

## Entendo os aspectos da arquitetura:
Para iniciar com a utilização do docker em um terminal devemos instalar o docker e os pacotes necessarios para que ele execulte corretamente.
O Linux escolhido foi o centos 8 da Oracle lixus;
Para baixar o docker no centos 8 utilizaremos os seguinte comando:

```bash
sudo dnf check-update
sudo dnf upgrade
sudo dnf config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo
sudo dnf install docker-ce docker-ce-cli containerd.io
sudo systemctl start docker
sudo systemctl enable docker
sudo docker --version
```
## Fazer a Img do docker Postgresql:
Comecei a criar a imagem do postegresql no arquivo yaml, no docker utiliza a arquivo docker-compose.yml para achar um img, você pode renomear de outra forma usando um parâmetro, mas utilizei da forma convencional para a criação do meu arquivo. Na linha de comando utilizei:

```bash
vi docker-compose.yml
```

``` ymal
version: '3.5'

volumes:
  data:

services:
  database:
    image: postgres:14
    ports:
      - "5432:5432"
    environment:
      - POSTGRES_PASSWORD=12345678
    volumes:
      - data:/var/lib/postgresql/data
```

Dei o comando:

```bash
docker-compose up -d
```
Recebi o seguinte erro:

ERROR:
        Can't find a suitable configuration file in this directory or any
        parent. Are you in the right directory?

        Supported filenames: docker-compose.yml, docker-compose.yaml, compose.yml, compose.yaml


Para resolver esse problema tive que instalar alguns pacotes novo para instalar o docker-compose.

```bash
python --version
sudo dnf install python3
python -m pip --version
python -m ensurepip --default-pip
python -m pip install --upgrade pip
pip install bcrypt
python3 -m pip install bcrypt
sudo dnf install python3-pip
python3 -m pip --version
pip3 install docker-compose

```
Então dei novamente o comando e a imagem foi criada. Para acessar o DB utilizei o pgAdmin.

![image](https://github.com/steffanyperfil/Docker-compose/assets/62122415/f1da4049-f9dc-40d2-93b0-a17003955ca9)

## Instalando o Wordpress:

Para a instalalção do Wordpress criei uma nova VM e fiz a instalação do docker seguindo a documentação oficial do link abaixo:

https://docs.docker.com/engine/install/centos/#uninstall-docker-engine

Utlizei a seguinte imagem do Wordpress:

``` bash
$ docker compose up -d

Creating network "my_wordpress_default" with the default driver
Pulling db (mysql:5.7)...
5.7: Pulling from library/mysql
efd26ecc9548: Pull complete
a3ed95caeb02: Pull complete
<...>
Digest: sha256:34a0aca88e85f2efa5edff1cea77cf5d3147ad93545dbec99cfe705b03c520de
Status: Downloaded newer image for mysql:5.7
Pulling wordpress (wordpress:latest)...
latest: Pulling from library/wordpress
efd26ecc9548: Already exists
a3ed95caeb02: Pull complete
589a9d9a7c64: Pull complete
<...>
Digest: sha256:ed28506ae44d5def89075fd5c01456610cd6c64006addfe5210b8c675881aff6
Status: Downloaded newer image for wordpress:latest
Creating my_wordpress_db_1
Creating my_wordpress_wordpress_1

```

Utilizei o modo brigde que me alocou um ip e com isso conseguir acessar o wordpress.

![image](https://github.com/steffanyperfil/Docker-compose/assets/62122415/665cae52-668a-458a-a9ec-a444d9ff98b4)

Para a persistência dos dados na img utilizer o volumes para guardar os arquivos.






