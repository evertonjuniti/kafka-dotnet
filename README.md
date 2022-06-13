# kafka-dotnet
PoC de Kafka com resiliência no consumo de mensagens

## Apresentação

Este repositório tem como objetivo apresentar o mecanismo de resiliência após consumo de uma mensagem no Confluent Kafka.

Ao invés de utilizar outras tecnologias como filas para criar uma resiliência no processamento pós consumo de mensagens, abordo aqui o mecanismo de avanço do offset apenas ao final do processamento com sucesso, caso haja qualquer problema o offset não é avançado, permitindo processar novamente a mesma mensagem em consumo posterior oportuno da mensagem.

Para isto divido esta PoC em algumas etapas, sendo:

1. Subir um cluster Kafka simulando 3 nós no Docker, definindo ao menos 2 partições para cada nó. Para isto é necessário que você tenha o Docker instalado
2. Criação de mensagens através de um aplicativo console em .Net, específico para atuar como producer
3. Leitura de mensagens através de um aplicativo console em .Net, específico para atuar como consumer

### Subida do cluster Kafka

Utilize a definição do script Docker Compose presente na pasta "kafka-cluster" e o seguinte comando para subir o cluster Kafka com 3 nós (lembre-se de executar este comando dentro da pasta "kafka-cluster"):

`docker compose up -d`

Se você não alterou nada do que está no docker-compose.yml, o cluster deverá subir com os seguintes containers:

- zookeeper1
- zookeeper2
- kafka1
- kafka2
- kafka3
- kafka-ui

Todos esses containers estarão na mesma rede, chamada "kafka-network". Isso é necessário para que os containers "se conversem".

### Producer

`docker build -t evertonjuniti/kafka-producer:latest .`

`docker run --name kafka-producer --network kafka-network -p 5000:80 -d evertonjuniti/kafka-producer:latest`

### Consumer

`docker build -t evertonjuniti/kafka-consumer:latest .`

`docker compose up -d`