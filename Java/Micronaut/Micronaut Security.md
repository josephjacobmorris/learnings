# Micronaut Security

## Micronaut Security JWT

### Step 1 : Create Project With Dependencies
```bash
mn create-app example.micronaut.micronautguide \
    --features=security-jwt,data-jdbc,reactor,graalvm,validation \
    --build=gradle \
    --lang=java \
    --test=junit
```

### Step 2 : Set Properties
```yaml
micronaut:
  security:
#Set authentication to bearer to receive a JSON response from the login endpoint.
    authentication: bearer
#used for generating JWT 
    token:
        jwt:
          signatures:
            secret:
              generator:
                secret: "${JWT_GENERATOR_SIGNATURE_SECRET:pleaseChangeThisSecretForANewOne}"
```
> Note : Keep the secret in a secure manner , usually done using some key vault

### Step 3: Provide the Authentication Provider to verify access before creating JWT
Create an implementaion of `HttpRequestAuthenticationProvider`
```java
package example.micronaut;

import io.micronaut.core.annotation.NonNull;
import io.micronaut.core.annotation.Nullable;
import io.micronaut.http.HttpRequest;
import io.micronaut.security.authentication.AuthenticationFailureReason;
import io.micronaut.security.authentication.AuthenticationRequest;
import io.micronaut.security.authentication.AuthenticationResponse;
import io.micronaut.security.authentication.provider.HttpRequestAuthenticationProvider;
import jakarta.inject.Singleton;

@Singleton 
class AuthenticationProviderUserPassword<B> implements HttpRequestAuthenticationProvider<B> { 

    @Override
    public AuthenticationResponse authenticate(
            @Nullable HttpRequest<B> httpRequest,
            @NonNull AuthenticationRequest<String, String> authenticationRequest
    ) {
        return authenticationRequest.getIdentity().equals("sherlock") && authenticationRequest.getSecret().equals("password")
                ? AuthenticationResponse.success(authenticationRequest.getIdentity())
                : AuthenticationResponse.failure(AuthenticationFailureReason.CREDENTIALS_DO_NOT_MATCH);
    }
}
```
### Step 4: Annotate your controllers with appropriate `@Secured` annonatation
Valid values include:
* `@Secured(SecurityRule.IS_AUTHENTICATED)`
* `@Secured(SecurityRule.IS_ANYONYMOUS)`

### Step 5: Define REST Client to use (from Consumer perspective & Testing Perspective)
```java
package example.micronaut;

import io.micronaut.http.annotation.Body;
import io.micronaut.http.annotation.Consumes;
import io.micronaut.http.annotation.Get;
import io.micronaut.http.annotation.Header;
import io.micronaut.http.annotation.Post;
import io.micronaut.http.client.annotation.Client;
import io.micronaut.security.authentication.UsernamePasswordCredentials;
import io.micronaut.security.token.render.BearerAccessRefreshToken;

import static io.micronaut.http.HttpHeaders.AUTHORIZATION;
import static io.micronaut.http.MediaType.TEXT_PLAIN;

@Client("/")
public interface AppClient {

    @Post("/login")
    BearerAccessRefreshToken login(@Body UsernamePasswordCredentials credentials);

    @Consumes(TEXT_PLAIN)
    @Get
    String home(@Header(AUTHORIZATION) String authorization);
}
```
## References
* https://guides.micronaut.io/latest/micronaut-security-jwt-gradle-java.html