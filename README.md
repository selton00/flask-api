
# Avaliação DevOps

Esse repositorio é um fork do flask-api mas só irei utilizar o ambiente virtual e o example.py

## Dockerfile flask-api

- Primeiro eu extraio as depedencias com o pipenv

``` 
$ pipenv sync
$ pipenv run pip freeze > requirements.txt
```

Depois foi criado a Dockerfile para o app

- Para buildar a imagem, podemos utilizar:

``` 
$ docker build -t seltonlucas/flask-api:1 .
```

- E para testar:

``` 
$ docker container run --name flask-api --rm -d -p 5000:5000 seltonlucas/flask-api:1
```

## Docker Compose

No arquivo docker-compose.yaml foi configurado o volume persistente e a rede do banco de dados.

Utilizamos a imagem e a versão criadas acima para o app e para o banco de dados foi utilizado a imagem mysql:8.0.

- para rodar o arquivo utilizamos

``` 
$ docker-compose --env-file ./.env up -d 
```

No .env temos todas as váriaveis de ambiente necessários para conectar ao banco.

## Jenkins Container

No repositorio há um diretorio `/jenkins_config` destinado as configurações do Jenkins

O dockerfile do serviço define as configurações para utilizar o Jenkins Configuration as Code(JCasC).

Temos um arquivo que define parametros de segurança, desabilitamos sign up de novos usuários e o login e senha do administrador são definidas como váriaveis.

E outro que define os plugins necessários para construímos nossa pipeline.

- Para buildarmos a imagem do Jenkins utilizamos

``` 
$ docker build -t jenkins:jcasc .
```

- E para iniciarmos 

``` 
$ docker container run --name jenkins -d --rm -v /var/run/docker.sock:/var/run/docker.sock -p 8080:8080 --env JENKINS_ADMIN_ID=admin --env JENKINS_ADMIN_PASSWORD=password jenkins:jcasc
```

Nós definimos aqui o usuário e a senha mas essas váriaveis podem ser definidos por metodos mais seguros.

Para rodar pipelines usando o docker no Jenkins é necessário utilizar o conceito de docker in docker.

## Jenkins Pipeline

A pipeline no jenkins irá utilizar a jenkinsfile configurada na raiz deste repositorio.

- Ao buildar a imagem e enviar para o dockerhub precisamos configurar as credenciais que são definidas na parte de segurança do programa

![Credenciais](imgs/jenkins1.png)

- Logo em seguida iremos criar um job.

![Escolha do repositorio no github](imgs/jenkins2.png)

![Configuração da Pipeline](imgs/jenkins3.png)

Na configuração também podemos definir de quanto em quanto tempo a pipeline será executada.
Podemos por exemplo, definir que a cada push neste repositorio a pipeline irá criar a imagem e enviar para o repositorio remoto

