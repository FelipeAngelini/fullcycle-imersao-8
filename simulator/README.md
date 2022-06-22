# Imersão Full Stack & FullCycle - Codelivery

## Descrição

Repositório do simulador de rotas feito com Golang (Backend).
Simulador envia e recebe mensagens do Kafka para simular o veiculo em transito.

## Configurar /etc/hosts

A comunicação entre as aplicações se dá de forma direta através da rede da máquina.
Para isto é necessário configurar um endereços que todos os containers Docker consigam acessar.

Acrescente no seu /etc/hosts (para Windows o caminho é C:\Windows\system32\drivers\etc\hosts):
```
127.0.0.1 host.docker.internal
```
Em todos os sistemas operacionais é necessário abrir o programa para editar o *hosts* como Administrator da máquina ou root.

## Rodar a aplicação

Será necessário abrir 3 terminais app/producer/consumer

No primeiro terminal executar(app):
```
docker-compose up -d
docker exec -it simulator bash
go run main.go
```


No segundo terminal executar(producer): 
```
cd .docker/kafka 
docker-compose up -d
docker exec -it kafka_kafka_1 bash
kafka-console-producer --bootstrap-server=localhost:9092 --topic=route.new-direction
```

No terceiro terminal executar(consumer): 
```
cd .docker/kafka 
docker exec -it kafka_kafka_1 bash
kafka-console-consumer --bootstrap-server=localhost:9092 --topic=route.new-position --group=terminal
```


----

No producer colocar o seguinte JSON para iniciar
```
{"clientId":"1","routeId":"1"}
```