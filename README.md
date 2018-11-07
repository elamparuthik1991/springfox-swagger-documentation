# custom auth security

swagger will show all APIs details in spring project.

## 1.Installation
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

## 2.Configuration
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
## 3 Brief- How to integrate.
This part show how to write API Description and Hide/Show APIs.

### 3.1.1 Swagger in Controller Implementation
This @Api need to include in begining of the controller.
```java
@Api("Transaction related operations are handled here.")
@RestController
@RequestMapping("/api/v1/")
public class TransactionController {
```
Or
```java
@Api(value = "/api/v1/", description = "Registration related operations are handled here.")
@RestController
@RequestMapping("/api/v1/")
public class TransactionController {
```
### 3.1.2 Remove Controller.
@ApiIgnore this annotation will help to remove controller.
```java
@ApiIgnore
@Api(value = "/api/v1/", description = "Registration related operations are handled here.")
@RestController
@RequestMapping("/api/v1/")
public class TransactionController {
```
### 3.1.3 Swagger in Controller-method Implementation
1. @ApiImplicitParams annotation need to include in begining of the controller method. It will display custom request in swagger.
2. @ApiOperation annotation need to include in begining of the controller method. It will have error code and demo response. eg.400.
3. @ApiIgnore to remove model object like AppRegistration and to remove unwanted models.
```java
@ApiImplicitParams({
	@ApiImplicitParam(paramType = "request", name = "request", value = "{\"noorAccountNumber\": \"AS-2342-324-2342\", \"mobileNumber\": \"234234323\"}", required = true, dataType = "body") })
@ApiResponses(value = {
	@ApiResponse(code = 400, message = "error message-> status:error, message:error message here.", response = Result.class) })
@ApiOperation("generate otp for mobile app.")
@PostMapping("/requestRegistration")
public ResponseEntity<Result> getRequestRegistration(@ApiIgnore @Valid @RequestBody AppRegistration appRegistration,
	@ApiIgnore Errors errors) {
```

### 3.2.1 Swagger in Model.
@ApiModel will help to describe the model.
```java
@Entity
@Table(name = "app_registration")
@ApiModel(description = "Class representing a AppRegistration tracked by the application..")
```
### 3.2.2 Swagger in Model.
@ApiModelProperty will help to describe the model parameter.
1. example- will be the example value.
2. request- will intimate * in swagger.
3. value- will be field value.
```java
@ApiModelProperty(value="noorAccountNumber", notes = "noor account number should not be empty.", example = "SA-3242-234-2342", required = true, position = 0)
@NotEmpty(message="noorAccountNumber.error.empty")//noor account number should not be empty.
private String noorAccountNumber;
```

## Conclusion
End of this integration you may find swagger-ui in result
1. Demo [http://localhost:8080/swagger-ui.html](http://localhost:8080/swagger-ui.html).
2. Production [http://localhost:8080/noor-payments/swagger-ui.html](http://localhost:8080/noor-payments/swagger-ui.html).

## References
To make edit this document please use [edit readme.md](https://www.makeareadme.com/#rendered-1).
