# Kafka 201
Recursos y pasos importantes para el curso

## Antes del curso
Es importante que tener un recordatorio de los siguientes temas:
* [Setup | Instalación](https://kafka.apache.org/quickstart) (Para la instalación es importante ver la sección **Antes de empezar**)
* [What is Kafka?](https://www.confluent.io/what-is-apache-kafka/) | [What is Apache Kafka?](https://www.geeksforgeeks.org/what-is-apache-kafka-and-how-does-it-work/?ref=rp)
* [Topics, partitions and offsets](https://medium.com/event-driven-utopia/understanding-kafka-topic-partitions-ae40f80552e8) 

# :computer:  Actividades

## Antes de empezar :exclamation:
Para realizar este curso es importante tener instalado los siguientes programas:

- Java > 8
- Kafka (https://kafka.apache.org/quickstart) < v2.2+ (en adelante)
- Tener un IDE instalado como [Visual Studio Code](https://code.visualstudio.com/download) o [IntelliJ](https://www.jetbrains.com/idea/download) 
- Maven

Para la instalación en Windows seguir el siguiente video:

* https://youtu.be/aKDWWICgfA0 (en)
* https://youtu.be/u5THXLlW0tU (es)


## Kafka CLI (Command Line Interface) 201
Una vez que Kafka fue descargado e instalado, procederemos a validar que este bien instalado para comenzar las pruebas

### Iniciar ZooKeeper service y Kafka Server
Abrimos una terminal y nos movemos al directorio donde se descargo Kafka 
`(ej. C:/Downloads/kafka_2.13-3.2.1)`

``` bash
# Iniciamos Zookeeper (Zookeeper sera nuestra herramienta aunada a Kafka para mantener los logs/mensajes guardados)
# Mac/Linuz
bin/zookeeper-server-start.sh config/zookeeper.properties 

# Windows  
.\bin\windows\zookeeper-server-start.bat .\config\zookeeper.properties

# si todo funciona bien, nos saldra un log parecido a 
# INFO binding to port 0.0.0.0/0.0.0.0:2181 (org.apache.zookeeper...)
# En caso de falla, usualmente es porque el puerto 2181 esta ocupado y debemos cambiar el puerto editando el archivo de 
# zookeeper.properties y modificando el puerto
```

Luego abrimos una segunda terminal, y nos movemos de nuevo al directorio de kafka
```bash
# Inicializamos el server de kafka con el comando
# Mac/Linux
bin/kafka-server-start.sh config/server.properties

# para Windows
.\bin\windows\kafka-server-start.bat .\config\server.properties
# si se inicializa bien veremos el log 
# INFO [KafkaServer id=0] started (kafka.server....)
``` 

### Crear un TOPIC para guardar los eventos
Actualmente tenemos corriendo dos servicios: 
- Zookeeper que trabaja de la mano de Kafka para guardar los logs (como si fuera nuestra Base de datos) 
- El server/broker de Kafka

Ahora en una tercera terminal hacemos lo siguiente
```bash
bin/kafka-topics.sh --create --topic <topic-name> --bootstrap-server localhost:<kafka-server-port>
# el puerto base para el server de kafka suele ser el 9092

# para Windows
.\bin\windows\kafka-topics.bat --create --topic <topic-name> --bootstrap-server localhost:<kafka-server-port>
# .\bin\windows\kafka-topics.bat --create --topic topic_demo --bootstrap-server localhost:9092
```

Al crear el TOPIC nos saldra un log similar a:
`Created topic <topic-name>`

Para validar que se creo el TOPIC podemos utilizar el comando `--describe` que nos indica como esta creado el topico o el comando `--list` que nos muestra la lista de topicos creados en Kafka

```bash
# Describe
# Mac/Linux
bin/kafka-topics.sh --describe --topic <topic-name> --bootstrap-server localhost:<kafka-server-port>

#Windows
.\bin\windows\kafka-topics.bat --describe --topic <topic-name> --bootstrap-server localhost:<kafka-server-port>

# list
# Mac/Linux
bin/kafka-topics.sh --list --bootstrap-server localhost:<kafka-server-port>

# Windows
.\bin\windows\kafka-topics.bat --list --bootstrap-server localhost:<kafka-server-port>
```

Para crear un TOPIC con `particiones` en especifico + `replication factor`
```bash
# Mac/Linux
bin/kafka-topics.sh --create --topic <topic-name> --partitions <#_particiones> --bootstrap-server localhost:<kafka-server-port>

# Windows
.\bin\windows\kafka-topics.bat --create --topic <topic-name> --partitions <#_particiones> --bootstrap-server localhost:<kafka-server-port> --replication-factor <#_replicacion>
```

### Configurar Retencion por Tamaño y Tiempo
```bash
# Mac/Linux
bin/kafka-configs.sh --bootstrap-server localhost:<port> --alter --entity-type topics --entity-name configured-topic --add-config retention.ms=-1,retention.bytes=524288000

# Windows
.\bin\windows\kafka-configs.bat --bootstrap-server localhost:<port> --alter --entity-type topics --entity-name configured-topic --add-config retention.ms=-1,retention.bytes=524288000
```

### Configurar o modificar factor de replicación (in-sync replicas)
```
# Mac/Linux
bin/kafka-configs.sh --bootstrap-server localhost:9092 --alter --entity-type topics --entity-name <topic_name> --add-config min.insync.replicas=<#_replicas>

# Windows
.\bin\windows\kafka-configs.bat --bootstrap-server localhost:9092 --alter --entity-type topics --entity-name <topic_name> --add-config min.insync.replicas=<#_replicas>
```

### Escribir eventos en el TOPIC [Opcional]

```bash
bin/kafka-console-producer.sh --topic <topic-name> --bootstrap-server localhost:<kafka-server-port> 
> <type-data>
> <type-data>
> <type-data>

# para Windows
.\bin\windows\kafka-console-producer.bat --topic <topic-name> --bootstrap-server localhost:<kafka-server-port> 
> <data>
> <data>
> <data>
```


### Leer los eventos del TOPIC [Opcional]

```bash
bin/kafka-console-consumer.sh --topic <topic-name> --bootstrap-server localhost:<kafka-server-port> 

# para Windows
.\bin\windows\kafka-console-consumer.bat --topic <topic-name> --bootstrap-server localhost:<kafka-server-port> 
```

Añadir la flag `--from-beginning` despues del nombre del topic
```bash
.\bin\windows\kafka-console-consumer.bat --topic <topic-name> --from-beginning --bootstrap-server localhost:<kafka-server-port> 
```

![Alt text](./img/kafka_zookeeper_diagram.png "Kafka & Zookeeper Diagram")


### Practica
La practica y ejercicios las podemos encontrar en el directorio de practica


# :books: Para aprender mas
* Kafka: The Definite Guide, 2nd Edition | O'Reilly Media
* https://kafka.apache.org/
* https://developer.confluent.io/learn-kafka/
* https://www.pluralsight.com/courses/apache-kafka-getting-started
* [Algoritmo Paxos y Raft](https://medium.com/@juan.baranowa/algor%C3%ADtmos-de-consenso-raft-y-paxos-b252e51e911a)
* [Aprendiendo Apache Kafka Pt 1](https://www.enmilocalfunciona.io/aprendiendo-apache-kafka-parte-1/)
* [Aprendiendo Apache Kafka Pt 2](https://www.enmilocalfunciona.io/aprendiendo-apache-kafka-parte-2-2/)
* [Aprendiendo Apache Kafka Pt 4 - Instalación](https://www.enmilocalfunciona.io/aprendiendo-apache-kafka-parte-4/)
