startar 1 instancia
./bin/kafka-server-start.sh config/kraft/server.properties

startar 3 
./bin/kafka-server-start.sh config/kraft/server-1.properties
./bin/kafka-server-start.sh config/kraft/server-2.properties
./bin/kafka-server-start.sh config/kraft/server-3.properties