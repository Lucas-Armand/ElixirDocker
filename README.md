# ElixirDocker
Objetivo dese projeto é criar um serviço de chat cloud native, capaz de escalar horizontalmente e que possa rodar de maneira única em mais de um servidor. Um requisito é que o servidor seja feitos em Elixir. Como esse projeto será o meu primeiro projeto em Elixir, tentarei montar esse documente em um formato passo-a-passo e construir uma lista de links relevaantes para usar como referencia em projetos futuros. Esse trabalho também será uma resposta a um desafio então tentarei realiza-lo numa design sprint de quatro dias.

# Testando o projeto: (ATÉ 07/11)

Para facilitar a vizualização do projeto e os testes futuros eu coloquei todo o código para rodar em um repositório em uma maquina virtual. 

### "Hello World" App Cloud Elixir/Docker:

A seguir temos o um "HelloWord" feito Elixir/Docker podem ser acessados a partir de: 

http://165.227.27.41:8080 (RESULTADO PARA UM SÓ NODE)

Para testar com dois servidores usando o "swarm" do Docker, só foi possível rodando localmente. Então, é necessário acessar ssh:

```
ssh root@138.197.222.53
password:targetso
```

E então faça o acesso a porta 8080:


```
curl http://localhost:8080
```

Se você acessar multiplas vezes verá que ID do "nó" alterna entre dois valores, como na imagem abaixo: 

![]()

### Chat Elixir (2 Nodes):

E o projeto de servidor chat no Elixir em multiplos "nodes" pode ser acessado por: 

http://165.227.27.41:8081

http://165.227.27.41:8082

Caso deseje acessar a maquina virtual:


```
ssh root@165.227.27.41
password:targetso
```

# Executando o projeto na própria maquina:

Primeiro é necessário baixar o repositória na sua maquina:

```
https://github.com/Lucas-Armand/ElixirDocker.git
```

# Criando um "Hello World" Cloud Nativo no Elixir/Docker:

## Criando uma imagem do app no docker:

```
cd elixir-on-docker/
docker-compose run --rm www mix deps.get
```
( Nesse ponto é possível fazer um teste :  ```docker-compose up --build ``` )

## Inciando o swarm:

```
docker swarm init < coloque aqui seu IP (Ex.: 165.227.27.41)>
```
( O resultado desse comando gera um token que será necessário mais adiante!)

## Criando virtual machines no docker:

Para facilitar a implementação e reproduzibilidade docódigo usaremos as chamdas "docker-machine" que são maquinas virtuais geradas pelo próprio docker. Então:

```
docker-machine create --driver virtualbox myvm1
```

## Conectando demais nodes no swarm:

Agora, usando o token do nosso manager node, devemos iniciar os nós nas virtual machines:
```
docker-machine ssh myvm1 "docker swarm join --token SWMTKN-1-299w4q8qco8zfede9n622zskmttmgs7zast89vbg7mncsi4vfa-ao4za4snopb11g7a3z1zbt3od 165.227.27.41:2377"
```
Nesse ponto se executarmos o comando ``` docker node ls ``` teremos um resultado como :
```
ID                            HOSTNAME                     STATUS              AVAILABILITY        MANAGER STATUS
p2hnk9awi5oqp1zd3fimymiyy *   docker-s-2vcpu-4gb-sfo2-03   Ready               Active              Leader
awzsljg0ya0s9v0jfrzytcvbg     myvm1                        Ready               Active                         
```
Por fim, se rodarmos o comando:

```
docker stack deploy -c docker-compose.yml www
```
o código começa rodar e podemos fazer o acesso:


```
curl http://localhost:8080
```


# Referências:
[Elixir first Project](https://elixir-lang.org/getting-started/mix-otp/introduction-to-mix.html#our-first-project)

[Elixir first web aplication](https://codewords.recurse.com/issues/five/building-a-web-framework-from-scratch-in-elixir)

[Elixir hello word](https://angelika.me/2016/08/14/hello-world-web-app-in-elixir-part-3-phoenix/)

[Elixir+vim](https://github.com/elixir-editors/vim-elixir)

[Elixir Chat Cloud Aplication](https://www.youtube.com/watch?v=JEXT-qaeeyg)

[what's pg2?](https://speakerdeck.com/antipax/pg2-and-you-getting-distributed-with-elixir?slide=12)

[Building a distributed chatroom with Raxx.Kit](http://crowdhailer.me/2018-05-01/building-a-distributed-chatroom-with-raxx-kit/)

[Elixir on Docker](https://github.com/CrowdHailer/elixir-on-docker)

[Elixir Meetup Londom](https://skillsmatter.com/skillscasts/11072-elixir-london-october) 

[Tutorial Docker - Part 3 (swarm)](https://docs.docker.com/get-started/part3/#take-down-the-app-and-the-swarm)


```

###  Instalando Elixir em Server Linux:

```
wget https://packages.erlang-solutions.com/erlang-solutions_1.0_all.deb && sudo dpkg -i erlang-solutions_1.0_all.deb
sudo apt-get update
sudo apt-get install esl-erlang
sudo apt-get install elixir
