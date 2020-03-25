---
title: Springboot Config Documentation, Two Ways With IntelliJ IDEA
description: 'IDE integrated options to assist the writing of configuration files.'
tags: spring boot, java
image: 'https://dzone.com/storage/temp/12792889-road-sign-pointing-in-two-directions.jpeg'
datePublished: 2019-12-03
lastModified: 2020-03-25
---

In modern software development, the applications are driven by configuration.

If configurable properties are large in numbers, then it is difficult to remember the purpose, structure and type of each property. It becomes a necessity to invest in the documentation of these properties.

_This article explores IDE integrated options to assist in writing Spring Boot YAML based configuration._

---

_Originally published at [Dzone](https://dzone.com/articles/two-ways-configuration-documentation-with-springnb)_

---

## Spring Configuration Processor¹

Spring Boot provides an out of box mechanism for configuration documentation: include a jar named `spring-boot-configuration-processor` and trigger build.

```gradle
annotationProcessor 'org.springframework.boot:spring-boot-configuration-processor'
```

### ⚙ How it works

1. You trigger build.
2. It scans `@ConfigurationProperties` and constructs a JSON file which describes overall properties.

```json
{
  "groups": [
    {
      "name": "db",
      "type": "mighty.config.Database",
      "sourceType": "mighty.config.Database"
    }
  ],
  "properties": [
    {
      "name": "db.hostname",
      "type": "java.lang.String",
      "sourceType": "mighty.config.Database"
    },
    {
      "name": "db.password",
      "type": "java.lang.Character[]",
      "sourceType": "mighty.config.Database"
    },
    {
      "name": "db.port",
      "type": "java.lang.Integer",
      "sourceType": "mighty.config.Database",
      "defaultValue": 0
    },
    {
      "name": "db.username",
      "type": "java.lang.String",
      "sourceType": "mighty.config.Database"
    }
  ],
  "hints": []
}
```

3. IDE intercepts generated file and helps you write configuration.

![](https://miro.medium.com/max/6720/1*T3SzqgOdqOs5UuxQzlVxAA.png)

IntelliJ IDEA — Ultimate Edition detects the spring meta processor on the classpath and provides hints based upon metadata file generated.

However, there is no support on IntelliJ IDEA — Community Edition, Spring Tools 4 (Eclipse with Spring tools), Visual Code (with Spring tools).

## Yaml Schema

YAML schema is newer, still evolving, and powerful option. In reality, YAML schema is written in [JSON format²](https://json-schema.org/understanding-json-schema) _(strange!)_.

At the time of writing, [Draft#8 or 2019–09](https://json-schema.org/specification.html) is released — tools like Visual Studio Code (with [YAML extension by Redhat](https://marketplace.visualstudio.com/items?itemName=redhat.vscode-yaml)), IntelliJ IDEA 2019.x support Draft#7.

### ⚙ How it works

1. You write schema by hand.

```json
{
  "$schema": "http://json-schema.org/draft-07/schema#",
  "$id": "mighty/db",

  "title": "Database Configuration",
  "description": "Applicable database properties",
  "type": "object",
  "properties": {
    "db": {
      "type": "object",
      "description": "DB configuration",
      "properties": {
        "hostname": {
          "type": "string",
          "description": "Hostname of db"
        },
        "port": {
          "type": "integer",
          "default": 0,
          "description": "port of db"
        },
        "serviceName": {
          "type": "string"
        },
        "username": {
          "type": "string",
          "description": "DB user"
        },
        "password": {
          "type": "string",
          "description": "DB Password"
        }
      },
      "required": ["hostname", "port"]
    }
  }
}
```

2. You create schema-mapping: mapping of files against schema governance.

![](https://miro.medium.com/max/6480/1*qeBViqhJOKRRofYhrIBFeQ.png)

<figcaption>Mapping Definition | IntelliJ IDEA Community Edition</figcaption>

3. IDE provides hints based upon schema provided to helps you write schema efficiently.

![](https://miro.medium.com/max/5600/1*Qsr3CONNTPfD_S7ZaeSFrA.png)

## Comparison

Now we have seen both ways, Let us compare:

### Case #1: Static Properties

Consider configuration which accepts some fixed set of properties related to database connection.

```yaml
db:
  hostname:
  port:
  username:
  password:
```

Here is IDE performance on both style:
![](https://miro.medium.com/max/5000/1*78nH6B-DFIthEp1Zjoj0gg.png)

The types of properties are not shown in suggestions in the case of YAML schema, yet it suggests the type while validating a property. With Spring Boot Configuration Metadata, Java exception message _Type Mismatch_ is shown.

### Case #2: Dynamic Properties

Let’s extend the previous case configuration. Consider the extension of configuration as below:

```yaml
db:
  hostname:
  port:
  type: # type of db
  .
  # DB specific properties
  .
----
# in case of mysql
db:
  hostname: localhost
  port: 9991
  type: mysql
  dumpQueriesOnException: true
  callableStmtCacheSize: 10
---
# in case of oracle
db:
  hostname: localhost
  port: 9992
  type: oracle
  batchPerformanceWorkaround: true
  connectionRetryDelay: 5
```

Based on the type; additional properties can be varied. Consider, two types of DB supported: Oracle and MySQL.

Oracle-specific properties if `type` is `oracle` or MySQL-specific properties if `type` is `mysql`.

#### UML Class diagram for a possible solution

![](https://miro.medium.com/max/1160/1*YmJ_9Vz5wEn6AZF58H7FKQ.png)

IDE is unable to distinguish Oracle properties with Mysql Properties in case of Spring Configuration Metadata. The generated spring metadata JSON file does not describe or command conditional nature of properties. Also, Spring Configuration metadata specification does not specify any conditional construct.

![](https://miro.medium.com/max/1280/1*H1E1WKmlXsFFVdgETgBxCQ.gif)

<figcaption> With Spring Configuration Metadata</figcaption>

IDE is able to intercept the conditional properties in case of Yaml Schema.

![](https://miro.medium.com/max/1280/1*y9XJLZK1LD3wBH9D0-5KVA.gif)

<figcaption>with Yaml Schema</figcaption>

## 💭 Wrapping Up

![](https://miro.medium.com/max/11300/0*hGQqWQA5f867CaCR)

<figcaption>Photo by Pablo García Saldaña on Unsplash</figcaption>

Spring Configuration Metadata programmer is equivalently good as YAML Schema for static configurable properties moreover it is automatically generated; however, this is supported by the paid edition of IntelliJ IDEA.

Yaml provides excellent conditional schema support which makes it powerful; however, writing such complex schema is not an easy task.

YAML schema is not limited to spring configuration files `application.yml` or `bootstrap.yml` but any YAML file. It enables a broad scope of applicability; **it is a better choice for applications requiring external configuration.**

### What could be better ?

Spring developers should include conditional properties construct, or they can adopt YAML/ JSON schema, which would be future standard and would be understood by a broad audience (even with different language background!). Also, It would simplify IDE implementation.

## References

1. Spring Configuration Meta  
   https://docs.spring.io/spring-boot/docs/2.1.7.RELEASE/reference/html/configuration-metadata.html
2. JSON Schema  
   https://json-schema.org/understanding-json-schema/
3. Polymorphic Spring Configuration  
   https://stackoverflow.com/questions/53962547/polymorphic-configuration-properties-in-spring-boot
4. Oracle Driver
   https://docs.oracle.com/cd/E13222_01/wls/docs81/jdbc_drivers/oracle.html
