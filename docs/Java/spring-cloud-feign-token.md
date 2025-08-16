# Feign接口调用传递token

```java
@Configuration
public class FeignOAuthRequestInterceptor implements RequestInterceptor {

    private final String AUTHORIZATION_HEADER = "Authorization";

    @Override
    public void apply(RequestTemplate requestTemplate) {
        SecurityContext securityContext = SecurityContextHolder.getContext();
        Object details = securityContext.getAuthentication().getDetails();
        RequestAttributes requestAttributes = RequestContextHolder.currentRequestAttributes();
        if (details != null && details instanceof OAuth2AuthenticationDetails) {
            OAuth2AuthenticationDetails authenticationDetails = (OAuth2AuthenticationDetails) details;
            requestTemplate.header(AUTHORIZATION_HEADER, String.format("Bearer %s", authenticationDetails.getTokenValue()));
        } else if (requestAttributes != null && requestAttributes instanceof ServletRequestAttributes) {
        HttpServletRequest request = ((ServletRequestAttributes) requestAttributes).getRequest();
        String token = request.getHeader(AUTHORIZATION_HEADER);
        requestTemplate.header(AUTHORIZATION_HEADER, token);
        }
    }
}
```