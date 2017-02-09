# Constructing a system base image to a Java Application

The image adrianofonseca/wildfly-app is basically the Wildfly server customized with Postgres Driver Module and a Datasource AppDS. This image to run requires a Postgres Server named postgres running in the default port 5432.

The connection configuration in the container is as follows:

```xml
	       <datasource jndi-name="java:jboss/datasources/AppDS" pool-name="AppDS" enabled="true" use-java-context="true">
                    <connection-url>jdbc:postgresql://postgres:5432/app</connection-url>
                    <driver>postgresql-driver</driver>
                    <transaction-isolation>TRANSACTION_READ_COMMITTED</transaction-isolation>
                    <pool>
                        <min-pool-size>10</min-pool-size>
                        <max-pool-size>100</max-pool-size>
                        <prefill>true</prefill>
                    </pool>
                    <security>
                        <user-name>app</user-name>
                        <password>app</password>
                    </security>
                    <statement>
                        <prepared-statement-cache-size>32</prepared-statement-cache-size>
                        <share-prepared-statements>true</share-prepared-statements>
                    </statement>
                </datasource>
                 <drivers>
                    <driver name="postgresql-driver" module="org.postgresql"/>
                </drivers>
```

To construct the project just check out PlayWithJEE in https://github.com/adriano-fonseca/PlayWithJEE and build it using the follow Maven command to package the application:

```shell
mvn package -Dmaven.test.skip=true

```

After that you must find the target folder PlayJavaEE.war, just copy the file to your Build context folder and run the command:

```shell
docker image build -t app-image .

```

In order to run the image that you have built, you need to have  a Postgres Server running. You can get one through the adrianofonseca/postgres:9.5 image. To run the postgres container:

```shell
docker container run --name db --rm -p 5432:5432 -v /home/adr-fonseca/docker/docker-postgres/postgresql/:/var/lib/postgresql -e DB_USER=app -e DB_PASS=app -e DB_NAME=app adrianofonseca/postgres:9.5

```

Having Postgres running you can run the app Image with

```shell
docker container run --name app --rm -p 8080:8080 -p 9990:9990 --link db:postgres app-image

```
 
You can get the docker compose file to run this conceptual application easily and also a docker compose file version 3 to deploy as stack in Swarm Cluster [here](https://github.com/adriano-fonseca/docker-stack). 
