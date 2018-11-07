# custom auth security

swagger will show all APIs details in spring project.

## Installation
pom.xml we need to include following dependencies.

```xml
<dependency>
	<groupId>io.springfox</groupId>
	<artifactId>springfox-swagger2</artifactId>
	<version>2.9.2</version>
</dependency>
<dependency>
	<groupId>io.springfox</groupId>
	<artifactId>springfox-swagger-ui</artifactId>
	<version>2.9.2</version>
</dependency>
<dependency>
	<groupId>io.springfox</groupId>
	<artifactId>springfox-bean-validators</artifactId>
	<version>2.9.2</version>
</dependency>
```

## Configuration
We need to include the following "SpringFoxConfig.java" file.   

```java
import java.util.Arrays;
import java.util.Collections;

import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.context.annotation.Import;

import springfox.bean.validators.configuration.BeanValidatorPluginsConfiguration;
import springfox.documentation.builders.ParameterBuilder;
import springfox.documentation.builders.PathSelectors;
import springfox.documentation.builders.RequestHandlerSelectors;
import springfox.documentation.schema.ModelRef;
import springfox.documentation.service.ApiInfo;
import springfox.documentation.service.Contact;
import springfox.documentation.spi.DocumentationType;
import springfox.documentation.spring.web.plugins.Docket;
import springfox.documentation.swagger2.annotations.EnableSwagger2;

@Configuration
@EnableSwagger2
@Import(BeanValidatorPluginsConfiguration.class)
public class SpringFoxConfig {
   
    @Bean
    public Docket apiDocket() {
        return new Docket(DocumentationType.SWAGGER_2)
                .select()
                .apis(RequestHandlerSelectors.basePackage("com.haroob"))
                .paths(PathSelectors.ant("/api/v1/**"))
                .build()
                .apiInfo(getApiInfo())
                .globalOperationParameters(
                        Arrays.asList(new ParameterBuilder()
                            .name("DeviceId")
                            .description("DeviceId security auth header.")
                            .modelRef(new ModelRef("string"))
                            .parameterType("header")
                            .required(true)
                            .build(),
                            new ParameterBuilder()
                            .name("SessionId")
                            .description("SessionId security auth header.")
                            .modelRef(new ModelRef("string"))
                            .parameterType("header")
                            .required(true)
                            .build()));
    }

    private ApiInfo getApiInfo() {
        return new ApiInfo(
                "Noor-Payment API",
                "Payment gateway for business.",
                "V1",
                "TERMS OF SERVICE URL",
                new Contact("NAME","URL","EMAIL"),
                "LICENSE",
                "LICENSE URL",
                Collections.emptyList()
        );
    }

}
```

## Conclusion
End of this integration you may find swagger-ui in result
1. Demo [http://localhost:8080/swagger-ui.html](http://localhost:8080/swagger-ui.html).
2. Production [http://localhost:8080/noor-payments/swagger-ui.html](http://localhost:8080/noor-payments/swagger-ui.html).

## References
To make edit this document please use [edit readme.md](https://www.makeareadme.com/#rendered-1).
