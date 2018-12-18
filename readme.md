## Steps

1. Create maven project, add dependencies and build plugin in pom.xml.

```xml
<dependency>
    <groupId>mysql</groupId>
    <artifactId>mysql-connector-java</artifactId>
    <scope>runtime</scope>
</dependency>
<dependency>
    <groupId>org.liquibase</groupId>
    <artifactId>liquibase-core</artifactId>
</dependency>
<dependency>
    <groupId>org.liquibase</groupId>
    <artifactId>liquibase-maven-plugin</artifactId>
    <version>3.6.2</version>
</dependency>
```

```xml
<plugin>
    <groupId>org.liquibase</groupId>
    <artifactId>liquibase-maven-plugin</artifactId>
    <version>3.6.2</version>
    <configuration>
        <propertyFile>src/main/resources/liquibase.properties</propertyFile>
    </configuration>
</plugin>
```

2. Create file `src/main/resources/liquibase.properties` and complete db connect configuration.

Note that you should replace <db-user> and <db-password> to your db account.

```properties
url=jdbc:mysql://localhost:3306/test_db
username=<db-user>
password=<db-password>
changeLogFile=src/main/resources/db/db.changelog-master.xml
outputChangeLogFile=src/main/resources/liquibase-outputChangeLog.xml
```

3. Create empty database `test_db`.

4. Add changeset for adding a table.

Create file `src/main/resources/db/changelog/20180108153042135_create_user_table.xml` and input content:

```xml
<changeSet id="20180108153042135" author="woody">
    <createTable tableName="user">
        <column name="id" type="int" autoIncrement="true">
            <constraints primaryKey="true" nullable="false"/>
        </column>
        <column name="firstname" type="varchar(50)"/>
        <column name="lastname" type="varchar(50)"/>
        <column name="state" type="char(2)"/>
    </createTable>
</changeSet>
```

Then include this file in `src/main/resources/db/db.changelog-master.xml`:

```xml
<include file="changelog/20180108153042135_create_user_table.xml" relativeToChangelogFile="true"/>
```

5. Execute `liquibase:update` command to apply changes to database. 

```sh
mvn liquibase:update
```

6. Repeat #4 and #5 to add more changesets.