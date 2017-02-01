# Starting from a base image contruct an App Image

The image adrianofonseca/wildfly-app is basically the Wildfly server costumised with Postgres Driver Module and a Datasource AppDS that searchs for a Postgres Server named postgres running in the default port 5432.

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

To construct the project just check out PlayWithJEE in https://github.com/adriano-fonseca/PlayWithJEE and build it using the follow Maven command:

```shell
mvn package -Dmaven.test.skip=true

```
After that you must find the target folder PlayJavaEE.war
