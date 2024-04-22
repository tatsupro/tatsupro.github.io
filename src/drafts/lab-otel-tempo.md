# LAB - Tích hợp OpenTelemetry và Tempo local

## Cách thức tích hợp

**1. Setup Spring Boot App**

Dùng [Spring Initializr](https://start.spring.io/) hoặc lệnh sau:

```bash
mkdir lab-otel-tempo && cd lab-otel-tempo
```

```bash
curl https://start.spring.io/starter.tgz \
    -d type=maven-project \
    -d language=java \
    -d platformVersion=3.1.5 \
    -d packaging=jar \
    -d jvmVersion=21 \
    -d groupId=com.stanleykubrick \
    -d artifactId=observability \
    -d name=observability \
    -d description="A Stanley Kubrick Production" \
    -d packageName=com.stanleykubrick.observability \
    -d dependencies=web,actuator | tar -xzvf -
```

Thêm `opentelemetry-collector` vào `pom.xml`:

```xml
<project>
  <dependencyManagement>
    <dependencies>
      <dependency>
        <groupId>io.opentelemetry</groupId>
        <artifactId>opentelemetry-bom</artifactId>
        <version>1.37.0</version>
        <type>pom</type>
        <scope>import</scope>
      </dependency>
    </dependencies>
  </dependencyManagement>
  <dependencies>
    <dependency>
      <groupId>io.opentelemetry</groupId>
      <artifactId>opentelemetry-api</artifactId>
    </dependency>
  </dependencies>
</project>
```



[github](https://github.com/tatsupro/lab-otel-tempo/commit/3126c2caee485926474c5cde998bc029afbf4e74)

## Liên kết ngoài

- [OpenTelemetry - Java](https://opentelemetry.io/docs/languages/java)
- [Medium - Comprehensive Observability in Spring Boot using OpenTelemetry, Prometheus, Grafana, Tempo, and Loki](https://medium.com/@ahmadalammar/comprehensive-observability-in-spring-boot-using-opentelemetry-prometheus-grafana-tempo-and-067196eee539)
