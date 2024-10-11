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
        bearer:
          #Set whether to enable bearer token authentication. Default value true.
          enabled: true
          #Sets the prefix to use for the auth token. Default value Bearer.
          prefix: Bearer
          #Sets the header name to use. Default value Authorization.
          header-name: 
        jwt:
          #Sets whether JWT security is enabled. Default value (true)
          enabled: true
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

## Authentication Strategy
"By default, Micronaut requires just one Authentication Provider to return a successful authentication response. You can set `micronaut.security.authentication-provider-strategy: ALL` to require all AuthenticationProviders to return a successful authentication response."

## Login Handler
Login Handler defines how to handle login attempts. There are various default implementation as well as we could have custom implementation.
Default implementation can be used by setting config values of `micronaut.security.authentication` as below

| Config Value | Required Dependency module | Bean Registered                |
|:------------:|:--------------------------:|:-------------------------------|
|    cookie    |       micronaut-jwt        | JwtCookieLoginHandler          |
|    bearer    |       micronaut-jwt        | AccessRefreshTokenLoginHandler |
|   idtoken    |      micronaut-oauth       | IdTokenLoginHandler            |
|   session    |     micronaut-session      | SessionLoginHandler            |

## Redirect configuration
```yaml
micronaut:
  security:
    redirect:
      enabled: true
      #Where the user is redirected to after a successful login. Default value ("/").
      login-success: /
      login-failure: /
      logout: /
      # where to redirect in case of un-authorized response
      unauthorized:
        url: /
        enabled: true
      # where to redirect in case of forbidden response  
      forbidden:
        url: /
        enabled: true
```

## Authentication Provider
 > Note : For reactive use `HttpRequestReactiveAuthenticationProvider`

### Built-in Authentication Providers
 * LdapAuthenticationProvider
 * OauthPasswordAuthenticationProvider
 * OpenIdPasswordAuthenticationProvider

## Security Rules

```yaml
micronaut:
  security:
    ip-patterns:
      - 127.0.0.1
      - 192.168.1.*
```

Using expression to restrict access
`@Secured("#{ user?.attributes?.get('email') == 'sherlock@micronaut.example' }")`

Using intercept map

```yaml
micronaut:
  security:
    intercept-url-map:
      -
        pattern: /images/*
        http-method: GET
        access:
          - isAnonymous()
      -
        pattern: /books
        access:
          - isAuthenticated()
      -
        pattern: /books/grails
        http-method: POST
        access:
          - ROLE_GRAILS
          - ROLE_GROOVY
      -
        pattern: /books/grails
        http-method: PUT
        access:
          - ROLE_ADMIN
```

## References
* https://guides.micronaut.io/latest/micronaut-security-jwt-gradle-java.html
* https://micronaut-projects.github.io/micronaut-security/latest/guide/#jwt