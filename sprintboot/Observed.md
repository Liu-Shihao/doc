要在 Spring Boot 3.1.x 中使用 `micrometer-tracing-bridge-otel` 和 `exporter-zipkin` 以及 `@Observed` 注解实现方法级别的分布式追踪，并将数据导出到 Jaeger，请按照以下步骤操作：

### 1. 添加依赖

在 `pom.xml` 文件中添加相关依赖：

```xml
<dependency>
    <groupId>io.micrometer</groupId>
    <artifactId>micrometer-tracing-bridge-otel</artifactId>
</dependency>
<dependency>
    <groupId>io.opentelemetry</groupId>
    <artifactId>opentelemetry-exporter-zipkin</artifactId>
</dependency>
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-actuator</artifactId>
</dependency>
<dependency>
    <groupId>io.opentelemetry</groupId>
    <artifactId>opentelemetry-sdk-extension-autoconfigure</artifactId>
    <version>1.15.0</version>
</dependency>
```

### 2. 配置 OpenTelemetry 和 Zipkin

在 `application.yml` 文件中配置 OpenTelemetry 和 Zipkin：

```yaml
management:
  tracing:
    enabled: true

otel:
  traces:
    exporter:
      zipkin:
        endpoint: http://your-jaeger-server:9411/api/v2/spans
```

### 3. 使用 `@Observed` 注解

在需要追踪的方法上使用 `@Observed` 注解：

```java
import io.micrometer.observation.annotation.Observed;
import org.springframework.stereotype.Service;

@Service
public class YourService {

    @Observed(name = "yourMethod")
    public void yourMethod() {
        // 方法实现
    }
}
```

### 4. 配置 OpenTelemetry 和 Micrometer Tracing

创建一个配置类以设置 OpenTelemetry 和 Micrometer Tracing：

```java
import io.micrometer.tracing.Tracer;
import io.micrometer.tracing.otel.bridge.OtelTracer;
import io.opentelemetry.api.OpenTelemetry;
import io.opentelemetry.api.trace.TracerProvider;
import io.opentelemetry.exporter.zipkin.ZipkinSpanExporter;
import io.opentelemetry.sdk.OpenTelemetrySdk;
import io.opentelemetry.sdk.trace.SdkTracerProvider;
import io.opentelemetry.sdk.trace.export.BatchSpanProcessor;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

@Configuration
public class TracingConfig {

    @Bean
    public Tracer tracer(OpenTelemetry openTelemetry) {
        return new OtelTracer(openTelemetry.getTracer("your-tracer"));
    }

    @Bean
    public OpenTelemetry openTelemetry() {
        ZipkinSpanExporter zipkinExporter = ZipkinSpanExporter.builder()
                .setEndpoint("http://your-jaeger-server:9411/api/v2/spans")
                .build();
        SdkTracerProvider tracerProvider = SdkTracerProvider.builder()
                .addSpanProcessor(BatchSpanProcessor.builder(zipkinExporter).build())
                .build();
        return OpenTelemetrySdk.builder()
                .setTracerProvider(tracerProvider)
                .build();
    }
}
```

### 5. 验证和测试

确保应用程序运行并正确配置后，调用标注了 `@Observed` 注解的方法，检查 Jaeger UI 以查看追踪信息是否正确记录和显示。

通过这些步骤，您可以在 Spring Boot 3.1.x 中使用 Micrometer Tracing 和 OpenTelemetry 将方法级别的追踪数据导出到 Jaeger。

在 Spring Boot 3.2.0 中，`management.zipkin` 配置已被合并到 `management.tracing` 和 `otel.traces.exporter.zipkin` 配置中。以下是完整的 `application.yaml` 配置示例，展示如何将追踪数据发送到 Jaeger（通过 Zipkin 格式）：

### Spring Boot 3.2.0 配置示例

```yaml
management:
  tracing:
    enabled: true
    sampling:
      probability: 1.0  # 采样率设置

otel:
  traces:
    exporter:
      zipkin:
        endpoint: http://your-jaeger-server:9411/api/v2/spans  # Zipkin 端点
    sampler:
      probability: 1.0  # OpenTelemetry 采样率设置
    resource:
      attributes:
        service.name: your-service-name  # 设置服务名称
```

### 解释

- **`management.tracing.enabled`**: 启用 Spring Boot Actuator 的追踪功能。
- **`management.tracing.sampling.probability`**: 设置采样率，1.0 表示全部采样。
- **`otel.traces.exporter.zipkin.endpoint`**: 配置 Zipkin 端点，指向 Jaeger 服务器的 API。
- **`otel.traces.sampler.probability`**: 设置 OpenTelemetry 的采样率。
- **`otel.traces.resource.attributes.service.name`**: 设置服务名称，便于在 Jaeger 中识别。

通过这些配置，Spring Boot 应用将使用 OpenTelemetry 进行追踪，并通过 Zipkin 格式将追踪数据导出到 Jaeger。