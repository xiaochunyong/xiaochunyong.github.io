# 
requestmatchers authorizerequests 区别

Spring Security 源码解答

1. 请求如果失败(认证不通过) 抛出 AccessDeniedException, 则 ExceptionTranslationFilter 会在处理异常中将当前的Request 缓存到Session中, 以留着下次使用

例如: 在登录前访问了一个受保护的资源, 会重定向到登录页, 登录成功后, 会自动跳到之前访问的受保护资源    



```
[requestMatchers=[Ant [pattern='/oauth/token'], Ant [pattern='/oauth/token_key'], Ant [pattern='/oauth/check_token']]],
[
 org.springframework.security.web.context.request.async.WebAsyncManagerIntegrationFilter@709c6768,
 org.springframework.security.web.context.SecurityContextPersistenceFilter@6e140753,
 org.springframework.security.web.header.HeaderWriterFilter@23d0f6be,
 org.springframework.security.web.authentication.logout.LogoutFilter@20bf7206,
 org.springframework.security.oauth2.provider.client.ClientCredentialsTokenEndpointFilter@77e24d3c,
 org.springframework.security.web.authentication.www.BasicAuthenticationFilter@73f0b216,
 org.springframework.security.web.savedrequest.RequestCacheAwareFilter@1e834a5b,
 org.springframework.security.web.servletapi.SecurityContextHolderAwareRequestFilter@5d1907fb,
 org.springframework.security.web.authentication.AnonymousAuthenticationFilter@3e356ed6,
 org.springframework.security.web.session.SessionManagementFilter@45c10678,
 org.springframework.security.web.access.ExceptionTranslationFilter@37f75ebf,
 org.springframework.security.web.access.intercept.FilterSecurityInterceptor@1722ede1
]


[requestMatchers=[Ant [pattern='/api/**']]], 
[
 org.springframework.security.web.context.request.async.WebAsyncManagerIntegrationFilter@c680819,
 org.springframework.security.web.context.SecurityContextPersistenceFilter@2e2a2b07,
 org.springframework.security.web.header.HeaderWriterFilter@78d49939,
 org.springframework.security.web.authentication.logout.LogoutFilter@3c00da24,
 org.springframework.security.oauth2.provider.authentication.OAuth2AuthenticationProcessingFilter@769c80d1,
 org.springframework.security.web.savedrequest.RequestCacheAwareFilter@52dc71b2,
 org.springframework.security.web.servletapi.SecurityContextHolderAwareRequestFilter@e1d2781,
 org.springframework.security.web.authentication.AnonymousAuthenticationFilter@52cda4bd,
 org.springframework.security.web.session.SessionManagementFilter@21f27bb5,
 org.springframework.security.web.access.ExceptionTranslationFilter@67e37d25,
 org.springframework.security.web.access.intercept.FilterSecurityInterceptor@1e60890c
]



[requestMatchers=[Ant [pattern='/oauth/**'], Ant [pattern='/login/**'], Ant [pattern='/logout/**']]], 
[
 org.springframework.security.web.context.request.async.WebAsyncManagerIntegrationFilter@2898c70d,
 org.springframework.security.web.context.SecurityContextPersistenceFilter@56afbace,
 org.springframework.security.web.header.HeaderWriterFilter@5bd191c2,
 org.springframework.security.web.authentication.logout.LogoutFilter@2ac7c683,
 org.springframework.security.web.authentication.UsernamePasswordAuthenticationFilter@60660d21,
 org.springframework.security.web.authentication.ui.DefaultLoginPageGeneratingFilter@19565b5,
 org.springframework.security.web.authentication.ui.DefaultLogoutPageGeneratingFilter@ec67be1,
 org.springframework.security.web.savedrequest.RequestCacheAwareFilter@40dcbf7,
 org.springframework.security.web.servletapi.SecurityContextHolderAwareRequestFilter@53850535,
 org.springframework.security.web.authentication.AnonymousAuthenticationFilter@64a00fe0,
 org.springframework.security.web.session.SessionManagementFilter@51806858,
 org.springframework.security.web.access.ExceptionTranslationFilter@2bef33e7,
 org.springframework.security.web.access.intercept.FilterSecurityInterceptor@41c56930
]

```



### CAS

1. 验证Token

```
org.jasig.cas.client.validation.AbstractUrlBasedTicketValidator.validate
```