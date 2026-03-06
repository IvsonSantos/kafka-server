# Kafka Server Setup and Usage

Guia rápido para configurar e operar o Kafka em modo KRaft.

## Single Server Setup

Para iniciar um servidor único:

```bash
./bin/kafka-server-start.sh config/kraft/server.properties
```

## Multiple Servers Setup

### 1. Gerar um Cluster ID
```bash
./bin/kafka-storage.sh random-uuid
```

### 2. Formatar os diretórios de armazenamento
Use o UUID gerado no passo anterior para formatar os servidores 1, 2 e 3:

```bash
./bin/kafka-storage.sh format -t r2SZ2jBDRGKSx0b2Aie03Q -c config/kraft/server-1.properties
./bin/kafka-storage.sh format -t r2SZ2jBDRGKSx0b2Aie03Q -c config/kraft/server-2.properties
./bin/kafka-storage.sh format -t r2SZ2jBDRGKSx0b2Aie03Q -c config/kraft/server-3.properties
```

### 3. Iniciar os servidores
Execute cada comando em um terminal diferente:

```bash
./bin/kafka-server-start.sh config/kraft/server-1.properties
./bin/kafka-server-start.sh config/kraft/server-2.properties
./bin/kafka-server-start.sh config/kraft/server-3.properties
```

## Topic Management

Comandos executados a partir do diretório `./bin`:

### Criar um tópico
```bash
./kafka-topics.sh --create --topic price-lists-approved --partitions 3 --replication-factor 3 --bootstrap-server localhost:9092,localhost:9094
```

### Listar e descrever tópicos
```bash
./kafka-topics.sh --list --bootstrap-server localhost:9092
./kafka-topics.sh --describe --bootstrap-server localhost:9092
```

### Deletar um tópico
```bash
./kafka-topics.sh --delete --topic price-lists-approved --bootstrap-server localhost:9092
```

## Messaging Operations

### Produzir mensagens
Criar o tópico se não existir e enviar mensagens simples:
```bash
./kafka-console-producer.sh --bootstrap-server localhost:9092,localhost:9094 --topic mytopic
```

Enviar mensagens com chave-valor:
```bash
./kafka-console-producer.sh --bootstrap-server localhost:9092,localhost:9094 --topic mytopic --property "parse.key=true" --property "key.separator=:"
```

### Consumir mensagens
Consumir desde o início:
```bash
./kafka-console-consumer.sh --topic mytopic --from-beginning --bootstrap-server localhost:9092
```

Consumir mensagens chave-valor:
```bash
./kafka-console-consumer.sh --topic mytopic --from-beginning --bootstrap-server localhost:9092 --property print.key=true
```