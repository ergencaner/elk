
# ELK Stack (ElasticSearch, Logstash, Kibana)

Ready to use ELK Stack on Docker with docker-compose.
Best used with microservices.



## Installation

Deploy on Docker with docker-compose

```bash
docker compose up -d
```
    
## Usage/Examples


* Logs will be created with index pattern 
```javascript
logstash-%{springAppName}-%{host}-%{+YYYY.MM.dd}
```
* Logstash is configured to listen to inputs on port **5044**

### Sample logback-spring.xml to use with Spring Boot

* **INFO** level logs are sent to Logstash.
* **DEBUG** level logs are sent to console.
* **springAppName** is also included in order to log microservice name.

```xml
<?xml version="1.0" encoding="UTF-8"?>
<configuration>
    <include resource="org/springframework/boot/logging/logback/defaults.xml"/>
    <include resource="org/springframework/boot/logging/logback/console-appender.xml"/>
    <springProperty scope="context" name="springAppName" source="spring.application.name"/>
    <springProperty scope="context" name="springAppVersion" source="spring.application.version"/>
    <contextName>${springAppName}</contextName>

    <appender name="logstashNetworkAppender" class="net.logstash.logback.appender.LogstashTcpSocketAppender">
        <destination>localhost:5044</destination>
        <encoder class="net.logstash.logback.encoder.LogstashEncoder">
        </encoder>
        <keepAliveDuration>5 minutes</keepAliveDuration>
        <filter class="ch.qos.logback.classic.filter.ThresholdFilter">
            <level>INFO</level>
        </filter>
    </appender>

    <root level="DEBUG">
        <appender-ref ref="logstashNetworkAppender"/>
        <appender-ref ref="CONSOLE"/>
    </root>
</configuration>
```


## License

Distributed under the MIT License. See LICENSE.txt for more information.
[MIT](https://choosealicense.com/licenses/mit/)
