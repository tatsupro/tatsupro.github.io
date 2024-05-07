# OpenTelemetry

## Triển khai tích hợp

### Java Agent (Manual Instrumentation)

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
