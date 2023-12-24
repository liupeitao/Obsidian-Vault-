# Springboot 整合Junit4

By default, `@SpringBootTest`​ will not start a server. You can use the `webEnvironment`​ attribute of `@SpringBootTest`​ to further refine how your tests run:

* ​`MOCK`​(Default) : Loads a web `ApplicationContext`​ and provides a mock web environment. Embedded servers are not started when using this annotation. If a web environment is not available on your classpath, this mode transparently falls back to creating a regular non-web `ApplicationContext`​. It can be used in conjunction with [`@AutoConfigureMockMvc`]()​[ or ](https://docs.spring.io/spring-boot/docs/2.3.0.RELEASE/reference/html/spring-boot-features.html#boot-features-testing-spring-boot-applications-testing-with-mock-environment)​[`@AutoConfigureWebTestClient`]()​ for mock-based testing of your web application.

‍
