# Untitled

[SKIP NAVIGATION LINKS](https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/web/servlet/config/annotation/WebMvcConfigurationSupport.html#skip.navbar.top "Skip navigation links")

* [OVERVIEW](https://docs.spring.io/spring-framework/docs/current/javadoc-api/overview-summary.html)
* [PACKAGE](https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/web/servlet/config/annotation/package-summary.html)
* CLASS
* [USE](https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/web/servlet/config/annotation/class-use/WebMvcConfigurationSupport.html)
* [TREE](https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/web/servlet/config/annotation/package-tree.html)
* [DEPRECATED](https://docs.spring.io/spring-framework/docs/current/javadoc-api/deprecated-list.html)
* [INDEX](https://docs.spring.io/spring-framework/docs/current/javadoc-api/index-files/index-1.html)
* [HELP](https://docs.spring.io/spring-framework/docs/current/javadoc-api/help-doc.html)

Spring Framework

* [PREV CLASS](https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/web/servlet/config/annotation/ViewResolverRegistry.html "class in org.springframework.web.servlet.config.annotation")
* [NEXT CLASS](https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/web/servlet/config/annotation/WebMvcConfigurer.html "interface in org.springframework.web.servlet.config.annotation")
* [FRAMES](https://docs.spring.io/spring-framework/docs/current/javadoc-api/index.html?org/springframework/web/servlet/config/annotation/WebMvcConfigurationSupport.html)
* [NO FRAMES](https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/web/servlet/config/annotation/WebMvcConfigurationSupport.html)
* [ALL CLASSES](https://docs.spring.io/spring-framework/docs/current/javadoc-api/allclasses-noframe.html)
* SUMMARY:
* NESTED |
* FIELD |
* [CONSTR](https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/web/servlet/config/annotation/WebMvcConfigurationSupport.html#constructor.summary) |
* [METHOD](https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/web/servlet/config/annotation/WebMvcConfigurationSupport.html#method.summary)
* DETAIL:
* FIELD |
* [CONSTR](https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/web/servlet/config/annotation/WebMvcConfigurationSupport.html#constructor.detail) |
* [METHOD](https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/web/servlet/config/annotation/WebMvcConfigurationSupport.html#method.detail)

org.springframework.web.servlet.config.annotation

## Class WebMvcConfigurationSupport

* [java.lang.Object](https://docs.oracle.com/javase/8/docs/api/java/lang/Object.html?is-external=true "class or interface in java.lang")
*  

  * org.springframework.web.servlet.config.annotation.WebMvcConfigurationSupport
* All Implemented Interfaces:[Aware](https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/beans/factory/Aware.html "interface in org.springframework.beans.factory"), [ApplicationContextAware](https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/context/ApplicationContextAware.html "interface in org.springframework.context"), [ServletContextAware](https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/web/context/ServletContextAware.html "interface in org.springframework.web.context")Direct Known Subclasses:[DelegatingWebMvcConfiguration](https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/web/servlet/config/annotation/DelegatingWebMvcConfiguration.html "class in org.springframework.web.servlet.config.annotation")---

  ```
  public class WebMvcConfigurationSupport
  extends Object
  implements ApplicationContextAware, ServletContextAware
  ```

  This is the main class providing the configuration behind the MVC Java config. It is typically imported by adding [`@EnableWebMvc`]()​ to an application [`@Configuration`]()​ class. An alternative more advanced option is to extend directly from this class and override methods as necessary, remembering to add [`@Configuration`]()​ to the subclass and [`@Bean`]()​ to overridden [`@Bean`]()​ methods. For more details see the javadoc of [`@EnableWebMvc`]()​.This class registers the following [`HandlerMappings`]()​:

  * [`RequestMappingHandlerMapping`]()​ ordered at 0 for mapping requests to annotated controller methods.
  * [`HandlerMapping`]()​ ordered at 1 to map URL paths directly to view names.
  * [`BeanNameUrlHandlerMapping`]()​ ordered at 2 to map URL paths to controller bean names.
  * [`RouterFunctionMapping`]()​ ordered at 3 to map [router functions](https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/web/servlet/function/RouterFunction.html "interface in org.springframework.web.servlet.function").
  * [`HandlerMapping`]()​ ordered at `Integer.MAX_VALUE-1`​ to serve static resource requests.
  * [`HandlerMapping`]()​ ordered at `Integer.MAX_VALUE`​ to forward requests to the default servlet.

  Registers these [`HandlerAdapters`]()​:

  * [`RequestMappingHandlerAdapter`]()​ for processing requests with annotated controller methods.
  * [`HttpRequestHandlerAdapter`]()​ for processing requests with [`HttpRequestHandlers`]()​.
  * [`SimpleControllerHandlerAdapter`]()​ for processing requests with interface-based [`Controllers`]()​.
  * [`HandlerFunctionAdapter`]()​ for processing requests with [router functions](https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/web/servlet/function/RouterFunction.html "interface in org.springframework.web.servlet.function").

  Registers a [`HandlerExceptionResolverComposite`]()​ with this chain of exception resolvers:

  * [`ExceptionHandlerExceptionResolver`]()​ for handling exceptions through [`ExceptionHandler`]()​ methods.
  * [`ResponseStatusExceptionResolver`]()​ for exceptions annotated with [`ResponseStatus`]()​.
  * [`DefaultHandlerExceptionResolver`]()​ for resolving known Spring exception types

  Registers an [`AntPathMatcher`]()​ and a [`UrlPathHelper`]()​ to be used by:

  * the [`RequestMappingHandlerMapping`]()​,
  * the [`HandlerMapping`]()​ for ViewControllers
  * and the [`HandlerMapping`]()​ for serving resources

  Note that those beans can be configured with a [`PathMatchConfigurer`]()​.Both the [`RequestMappingHandlerAdapter`]()​ and the [`ExceptionHandlerExceptionResolver`]()​ are configured with default instances of the following by default:

  * a [`ContentNegotiationManager`]()​
  * a [`DefaultFormattingConversionService`]()​
  * an [`OptionalValidatorFactoryBean`]()​ if a JSR-303 implementation is available on the classpath
  * a range of [`HttpMessageConverters`]()​ depending on the third-party libraries available on the classpath.

  Since:3.1**Author:**Rossen Stoyanchev, Brian Clozel, Sebastien Deleuze**See Also:**​[`EnableWebMvc`]()​, [`WebMvcConfigurer`]()​
*  

  * ### Constructor Summary

    Constructors|Constructor and Description|  
    | -----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|  
    |`<span class="memberNameLink"><a href="https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/web/servlet/config/annotation/WebMvcConfigurationSupport.html#WebMvcConfigurationSupport--">WebMvcConfigurationSupport</a></span>()`​ |
  * ### Method Summary

    All Methods[Instance Methods](javascript:show(2);)​[Concrete Methods](javascript:show(8);)|Modifier and Type|Method and Description|  
    | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------| ------------------------|  
    |`protected void`​|`<span class="memberNameLink"><a href="https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/web/servlet/config/annotation/WebMvcConfigurationSupport.html#addArgumentResolvers-java.util.List-">addArgumentResolvers</a></span>(<a href="https://docs.oracle.com/javase/8/docs/api/java/util/List.html?is-external=true" title="class or interface in java.util">List</a><<a href="https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/web/method/support/HandlerMethodArgumentResolver.html" title="interface in org.springframework.web.method.support">HandlerMethodArgumentResolver</a>> argumentResolvers)`​Add custom [`HandlerMethodArgumentResolvers`]()​ to use in addition to the ones registered by default.|  
    |`protected void`​|`<span class="memberNameLink"><a href="https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/web/servlet/config/annotation/WebMvcConfigurationSupport.html#addCorsMappings-org.springframework.web.servlet.config.annotation.CorsRegistry-">addCorsMappings</a></span>(<a href="https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/web/servlet/config/annotation/CorsRegistry.html" title="class in org.springframework.web.servlet.config.annotation">CorsRegistry</a> registry)`​Override this method to configure cross-origin requests processing.|  
    |`protected void`​|`<span class="memberNameLink"><a href="https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/web/servlet/config/annotation/WebMvcConfigurationSupport.html#addDefaultHandlerExceptionResolvers-java.util.List-org.springframework.web.accept.ContentNegotiationManager-">addDefaultHandlerExceptionResolvers</a></span>(<a href="https://docs.oracle.com/javase/8/docs/api/java/util/List.html?is-external=true" title="class or interface in java.util">List</a><<a href="https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/web/servlet/HandlerExceptionResolver.html" title="interface in org.springframework.web.servlet">HandlerExceptionResolver</a>> exceptionResolvers,<span> </span><a href="https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/web/accept/ContentNegotiationManager.html" title="class in org.springframework.web.accept">ContentNegotiationManager</a> mvcContentNegotiationManager)`​A method available to subclasses for adding default [`HandlerExceptionResolvers`]()​.|  
    |`protected void`​|`<span class="memberNameLink"><a href="https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/web/servlet/config/annotation/WebMvcConfigurationSupport.html#addDefaultHttpMessageConverters-java.util.List-">addDefaultHttpMessageConverters</a></span>(<a href="https://docs.oracle.com/javase/8/docs/api/java/util/List.html?is-external=true" title="class or interface in java.util">List</a><<a href="https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/http/converter/HttpMessageConverter.html" title="interface in org.springframework.http.converter">HttpMessageConverter</a><?>> messageConverters)`​Adds a set of default HttpMessageConverter instances to the given list.|  
    |`protected void`​|`<span class="memberNameLink"><a href="https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/web/servlet/config/annotation/WebMvcConfigurationSupport.html#addFormatters-org.springframework.format.FormatterRegistry-">addFormatters</a></span>(<a href="https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/format/FormatterRegistry.html" title="interface in org.springframework.format">FormatterRegistry</a> registry)`​Override this method to add custom [`Converter`]()​ and/or [`Formatter`]()​ delegates to the common [`FormattingConversionService`]()​.|  
    |`protected void`​|`<span class="memberNameLink"><a href="https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/web/servlet/config/annotation/WebMvcConfigurationSupport.html#addInterceptors-org.springframework.web.servlet.config.annotation.InterceptorRegistry-">addInterceptors</a></span>(<a href="https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/web/servlet/config/annotation/InterceptorRegistry.html" title="class in org.springframework.web.servlet.config.annotation">InterceptorRegistry</a> registry)`​Override this method to add Spring MVC interceptors for pre- and post-processing of controller invocation.|  
    |`protected void`​|`<span class="memberNameLink"><a href="https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/web/servlet/config/annotation/WebMvcConfigurationSupport.html#addResourceHandlers-org.springframework.web.servlet.config.annotation.ResourceHandlerRegistry-">addResourceHandlers</a></span>(<a href="https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/web/servlet/config/annotation/ResourceHandlerRegistry.html" title="class in org.springframework.web.servlet.config.annotation">ResourceHandlerRegistry</a> registry)`​Override this method to add resource handlers for serving static resources.|  
    |`protected void`​|`<span class="memberNameLink"><a href="https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/web/servlet/config/annotation/WebMvcConfigurationSupport.html#addReturnValueHandlers-java.util.List-">addReturnValueHandlers</a></span>(<a href="https://docs.oracle.com/javase/8/docs/api/java/util/List.html?is-external=true" title="class or interface in java.util">List</a><<a href="https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/web/method/support/HandlerMethodReturnValueHandler.html" title="interface in org.springframework.web.method.support">HandlerMethodReturnValueHandler</a>> returnValueHandlers)`​Add custom [`HandlerMethodReturnValueHandlers`]()​ in addition to the ones registered by default.|  
    |`protected void`​|`<span class="memberNameLink"><a href="https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/web/servlet/config/annotation/WebMvcConfigurationSupport.html#addViewControllers-org.springframework.web.servlet.config.annotation.ViewControllerRegistry-">addViewControllers</a></span>(<a href="https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/web/servlet/config/annotation/ViewControllerRegistry.html" title="class in org.springframework.web.servlet.config.annotation">ViewControllerRegistry</a> registry)`​Override this method to add view controllers.|  
    |`<a href="https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/web/servlet/handler/BeanNameUrlHandlerMapping.html" title="class in org.springframework.web.servlet.handler">BeanNameUrlHandlerMapping</a>`​|`<span class="memberNameLink"><a href="https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/web/servlet/config/annotation/WebMvcConfigurationSupport.html#beanNameHandlerMapping-org.springframework.format.support.FormattingConversionService-org.springframework.web.servlet.resource.ResourceUrlProvider-">beanNameHandlerMapping</a></span>(<a href="https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/format/support/FormattingConversionService.html" title="class in org.springframework.format.support">FormattingConversionService</a> conversionService,<span> </span><a href="https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/web/servlet/resource/ResourceUrlProvider.html" title="class in org.springframework.web.servlet.resource">ResourceUrlProvider</a> resourceUrlProvider)`​Return a [`BeanNameUrlHandlerMapping`]()​ ordered at 2 to map URL paths to controller bean names.|  
    |`protected void`​|`<span class="memberNameLink"><a href="https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/web/servlet/config/annotation/WebMvcConfigurationSupport.html#configureAsyncSupport-org.springframework.web.servlet.config.annotation.AsyncSupportConfigurer-">configureAsyncSupport</a></span>(<a href="https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/web/servlet/config/annotation/AsyncSupportConfigurer.html" title="class in org.springframework.web.servlet.config.annotation">AsyncSupportConfigurer</a> configurer)`​Override this method to configure asynchronous request processing options.|  
    |`protected void`​|`<span class="memberNameLink"><a href="https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/web/servlet/config/annotation/WebMvcConfigurationSupport.html#configureContentNegotiation-org.springframework.web.servlet.config.annotation.ContentNegotiationConfigurer-">configureContentNegotiation</a></span>(<a href="https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/web/servlet/config/annotation/ContentNegotiationConfigurer.html" title="class in org.springframework.web.servlet.config.annotation">ContentNegotiationConfigurer</a> configurer)`​Override this method to configure content negotiation.|  
    |`protected void`​|`<span class="memberNameLink"><a href="https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/web/servlet/config/annotation/WebMvcConfigurationSupport.html#configureDefaultServletHandling-org.springframework.web.servlet.config.annotation.DefaultServletHandlerConfigurer-">configureDefaultServletHandling</a></span>(<a href="https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/web/servlet/config/annotation/DefaultServletHandlerConfigurer.html" title="class in org.springframework.web.servlet.config.annotation">DefaultServletHandlerConfigurer</a> configurer)`​Override this method to configure "default" Servlet handling.|  
    |`protected void`​|`<span class="memberNameLink"><a href="https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/web/servlet/config/annotation/WebMvcConfigurationSupport.html#configureHandlerExceptionResolvers-java.util.List-">configureHandlerExceptionResolvers</a></span>(<a href="https://docs.oracle.com/javase/8/docs/api/java/util/List.html?is-external=true" title="class or interface in java.util">List</a><<a href="https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/web/servlet/HandlerExceptionResolver.html" title="interface in org.springframework.web.servlet">HandlerExceptionResolver</a>> exceptionResolvers)`​Override this method to configure the list of [`HandlerExceptionResolvers`]()​ to use.|  
    |`protected void`​|`<span class="memberNameLink"><a href="https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/web/servlet/config/annotation/WebMvcConfigurationSupport.html#configureMessageConverters-java.util.List-">configureMessageConverters</a></span>(<a href="https://docs.oracle.com/javase/8/docs/api/java/util/List.html?is-external=true" title="class or interface in java.util">List</a><<a href="https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/http/converter/HttpMessageConverter.html" title="interface in org.springframework.http.converter">HttpMessageConverter</a><?>> converters)`​Override this method to add custom [`HttpMessageConverters`]()​ to use with the [`RequestMappingHandlerAdapter`]()​ and the [`ExceptionHandlerExceptionResolver`]()​.|  
    |`protected void`​|`<span class="memberNameLink"><a href="https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/web/servlet/config/annotation/WebMvcConfigurationSupport.html#configurePathMatch-org.springframework.web.servlet.config.annotation.PathMatchConfigurer-">configurePathMatch</a></span>(<a href="https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/web/servlet/config/annotation/PathMatchConfigurer.html" title="class in org.springframework.web.servlet.config.annotation">PathMatchConfigurer</a> configurer)`​Override this method to configure path matching options.|  
    |`protected void`​|`<span class="memberNameLink"><a href="https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/web/servlet/config/annotation/WebMvcConfigurationSupport.html#configureViewResolvers-org.springframework.web.servlet.config.annotation.ViewResolverRegistry-">configureViewResolvers</a></span>(<a href="https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/web/servlet/config/annotation/ViewResolverRegistry.html" title="class in org.springframework.web.servlet.config.annotation">ViewResolverRegistry</a> registry)`​Override this method to configure view resolution.|  
    |`protected<span> </span><a href="https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/web/servlet/mvc/method/annotation/ExceptionHandlerExceptionResolver.html" title="class in org.springframework.web.servlet.mvc.method.annotation">ExceptionHandlerExceptionResolver</a>`​|`<span class="memberNameLink"><a href="https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/web/servlet/config/annotation/WebMvcConfigurationSupport.html#createExceptionHandlerExceptionResolver--">createExceptionHandlerExceptionResolver</a></span>()`​Protected method for plugging in a custom subclass of [`ExceptionHandlerExceptionResolver`]()​.|  
    |`protected<span> </span><a href="https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/web/servlet/mvc/method/annotation/RequestMappingHandlerAdapter.html" title="class in org.springframework.web.servlet.mvc.method.annotation">RequestMappingHandlerAdapter</a>`​|`<span class="memberNameLink"><a href="https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/web/servlet/config/annotation/WebMvcConfigurationSupport.html#createRequestMappingHandlerAdapter--">createRequestMappingHandlerAdapter</a></span>()`​Protected method for plugging in a custom subclass of [`RequestMappingHandlerAdapter`]()​.|  
    |`protected<span> </span><a href="https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/web/servlet/mvc/method/annotation/RequestMappingHandlerMapping.html" title="class in org.springframework.web.servlet.mvc.method.annotation">RequestMappingHandlerMapping</a>`​|`<span class="memberNameLink"><a href="https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/web/servlet/config/annotation/WebMvcConfigurationSupport.html#createRequestMappingHandlerMapping--">createRequestMappingHandlerMapping</a></span>()`​Protected method for plugging in a custom subclass of [`RequestMappingHandlerMapping`]()​.|  
    |`<a href="https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/web/servlet/HandlerMapping.html" title="interface in org.springframework.web.servlet">HandlerMapping</a>`​|`<span class="memberNameLink"><a href="https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/web/servlet/config/annotation/WebMvcConfigurationSupport.html#defaultServletHandlerMapping--">defaultServletHandlerMapping</a></span>()`​Return a handler mapping ordered at Integer.MAX_VALUE with a mapped default servlet handler.|  
    |`protected void`​|`<span class="memberNameLink"><a href="https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/web/servlet/config/annotation/WebMvcConfigurationSupport.html#extendHandlerExceptionResolvers-java.util.List-">extendHandlerExceptionResolvers</a></span>(<a href="https://docs.oracle.com/javase/8/docs/api/java/util/List.html?is-external=true" title="class or interface in java.util">List</a><<a href="https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/web/servlet/HandlerExceptionResolver.html" title="interface in org.springframework.web.servlet">HandlerExceptionResolver</a>> exceptionResolvers)`​Override this method to extend or modify the list of [`HandlerExceptionResolvers`]()​ after it has been configured.|  
    |`protected void`​|`<span class="memberNameLink"><a href="https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/web/servlet/config/annotation/WebMvcConfigurationSupport.html#extendMessageConverters-java.util.List-">extendMessageConverters</a></span>(<a href="https://docs.oracle.com/javase/8/docs/api/java/util/List.html?is-external=true" title="class or interface in java.util">List</a><<a href="https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/http/converter/HttpMessageConverter.html" title="interface in org.springframework.http.converter">HttpMessageConverter</a><?>> converters)`​Override this method to extend or modify the list of converters after it has been configured.|  
    |`<a href="https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/web/servlet/FlashMapManager.html" title="interface in org.springframework.web.servlet">FlashMapManager</a>`​|`<span class="memberNameLink"><a href="https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/web/servlet/config/annotation/WebMvcConfigurationSupport.html#flashMapManager--">flashMapManager</a></span>()`​ |  
    |`<a href="https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/context/ApplicationContext.html" title="interface in org.springframework.context">ApplicationContext</a>`​|`<span class="memberNameLink"><a href="https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/web/servlet/config/annotation/WebMvcConfigurationSupport.html#getApplicationContext--">getApplicationContext</a></span>()`​Return the associated Spring [`ApplicationContext`]()​.|  
    |`protected<span> </span><a href="https://docs.oracle.com/javase/8/docs/api/java/util/List.html?is-external=true" title="class or interface in java.util">List</a><<a href="https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/web/method/support/HandlerMethodArgumentResolver.html" title="interface in org.springframework.web.method.support">HandlerMethodArgumentResolver</a>>`​|`<span class="memberNameLink"><a href="https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/web/servlet/config/annotation/WebMvcConfigurationSupport.html#getArgumentResolvers--">getArgumentResolvers</a></span>()`​Provide access to the shared custom argument resolvers used by the [`RequestMappingHandlerAdapter`]()​ and the [`ExceptionHandlerExceptionResolver`]()​.|  
    |`protected<span> </span><a href="https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/web/servlet/config/annotation/AsyncSupportConfigurer.html" title="class in org.springframework.web.servlet.config.annotation">AsyncSupportConfigurer</a>`​|`<span class="memberNameLink"><a href="https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/web/servlet/config/annotation/WebMvcConfigurationSupport.html#getAsyncSupportConfigurer--">getAsyncSupportConfigurer</a></span>()`​Callback for building the [`AsyncSupportConfigurer`]()​.|  
    |`protected<span> </span><a href="https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/web/bind/support/ConfigurableWebBindingInitializer.html" title="class in org.springframework.web.bind.support">ConfigurableWebBindingInitializer</a>`​|`<span class="memberNameLink"><a href="https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/web/servlet/config/annotation/WebMvcConfigurationSupport.html#getConfigurableWebBindingInitializer-org.springframework.format.support.FormattingConversionService-org.springframework.validation.Validator-">getConfigurableWebBindingInitializer</a></span>(<a href="https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/format/support/FormattingConversionService.html" title="class in org.springframework.format.support">FormattingConversionService</a> mvcConversionService,<span> </span><a href="https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/validation/Validator.html" title="interface in org.springframework.validation">Validator</a> mvcValidator)`​Return the [`ConfigurableWebBindingInitializer`]()​ to use for initializing all [`WebDataBinder`]()​ instances.|  
    |`protected<span> </span><a href="https://docs.oracle.com/javase/8/docs/api/java/util/Map.html?is-external=true" title="class or interface in java.util">Map</a><<a href="https://docs.oracle.com/javase/8/docs/api/java/lang/String.html?is-external=true" title="class or interface in java.lang">String</a>,<a href="https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/web/cors/CorsConfiguration.html" title="class in org.springframework.web.cors">CorsConfiguration</a>>`​|`<span class="memberNameLink"><a href="https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/web/servlet/config/annotation/WebMvcConfigurationSupport.html#getCorsConfigurations--">getCorsConfigurations</a></span>()`​Return the registered [`CorsConfiguration`]()​ objects, keyed by path pattern.|  
    |`protected<span> </span><a href="https://docs.oracle.com/javase/8/docs/api/java/util/Map.html?is-external=true" title="class or interface in java.util">Map</a><<a href="https://docs.oracle.com/javase/8/docs/api/java/lang/String.html?is-external=true" title="class or interface in java.lang">String</a>,<a href="https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/http/MediaType.html" title="class in org.springframework.http">MediaType</a>>`​|`<span class="memberNameLink"><a href="https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/web/servlet/config/annotation/WebMvcConfigurationSupport.html#getDefaultMediaTypes--">getDefaultMediaTypes</a></span>()`​ |  
    |`protected<span> </span><a href="https://docs.oracle.com/javase/8/docs/api/java/lang/Object.html?is-external=true" title="class or interface in java.lang">Object</a>[]`​|`<span class="memberNameLink"><a href="https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/web/servlet/config/annotation/WebMvcConfigurationSupport.html#getInterceptors-org.springframework.format.support.FormattingConversionService-org.springframework.web.servlet.resource.ResourceUrlProvider-">getInterceptors</a></span>(<a href="https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/format/support/FormattingConversionService.html" title="class in org.springframework.format.support">FormattingConversionService</a> mvcConversionService,<span> </span><a href="https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/web/servlet/resource/ResourceUrlProvider.html" title="class in org.springframework.web.servlet.resource">ResourceUrlProvider</a> mvcResourceUrlProvider)`​Provide access to the shared handler interceptors used to configure [`HandlerMapping`]()​ instances with.|  
    |`protected<span> </span><a href="https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/validation/MessageCodesResolver.html" title="interface in org.springframework.validation">MessageCodesResolver</a>`​|`<span class="memberNameLink"><a href="https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/web/servlet/config/annotation/WebMvcConfigurationSupport.html#getMessageCodesResolver--">getMessageCodesResolver</a></span>()`​Override this method to provide a custom [`MessageCodesResolver`]()​.|  
    |`protected<span> </span><a href="https://docs.oracle.com/javase/8/docs/api/java/util/List.html?is-external=true" title="class or interface in java.util">List</a><<a href="https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/http/converter/HttpMessageConverter.html" title="interface in org.springframework.http.converter">HttpMessageConverter</a><?>>`​|`<span class="memberNameLink"><a href="https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/web/servlet/config/annotation/WebMvcConfigurationSupport.html#getMessageConverters--">getMessageConverters</a></span>()`​Provides access to the shared [`HttpMessageConverters`]()​ used by the [`RequestMappingHandlerAdapter`]()​ and the [`ExceptionHandlerExceptionResolver`]()​.|  
    |`protected<span> </span><a href="https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/web/servlet/config/annotation/PathMatchConfigurer.html" title="class in org.springframework.web.servlet.config.annotation">PathMatchConfigurer</a>`​|`<span class="memberNameLink"><a href="https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/web/servlet/config/annotation/WebMvcConfigurationSupport.html#getPathMatchConfigurer--">getPathMatchConfigurer</a></span>()`​Callback for building the [`PathMatchConfigurer`]()​.|  
    |`protected<span> </span><a href="https://docs.oracle.com/javase/8/docs/api/java/util/List.html?is-external=true" title="class or interface in java.util">List</a><<a href="https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/web/method/support/HandlerMethodReturnValueHandler.html" title="interface in org.springframework.web.method.support">HandlerMethodReturnValueHandler</a>>`​|`<span class="memberNameLink"><a href="https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/web/servlet/config/annotation/WebMvcConfigurationSupport.html#getReturnValueHandlers--">getReturnValueHandlers</a></span>()`​Provide access to the shared return value handlers used by the [`RequestMappingHandlerAdapter`]()​ and the [`ExceptionHandlerExceptionResolver`]()​.|  
    |`<a href="https://docs.oracle.com/javaee/7/api/javax/servlet/ServletContext.html?is-external=true" title="class or interface in javax.servlet">ServletContext</a>`​|`<span class="memberNameLink"><a href="https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/web/servlet/config/annotation/WebMvcConfigurationSupport.html#getServletContext--">getServletContext</a></span>()`​Return the associated [`ServletContext`]()​.|  
    |`protected<span> </span><a href="https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/validation/Validator.html" title="interface in org.springframework.validation">Validator</a>`​|`<span class="memberNameLink"><a href="https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/web/servlet/config/annotation/WebMvcConfigurationSupport.html#getValidator--">getValidator</a></span>()`​Override this method to provide a custom [`Validator`]()​.|  
    |`<a href="https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/web/servlet/HandlerExceptionResolver.html" title="interface in org.springframework.web.servlet">HandlerExceptionResolver</a>`​|`<span class="memberNameLink"><a href="https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/web/servlet/config/annotation/WebMvcConfigurationSupport.html#handlerExceptionResolver-org.springframework.web.accept.ContentNegotiationManager-">handlerExceptionResolver</a></span>(<a href="https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/web/accept/ContentNegotiationManager.html" title="class in org.springframework.web.accept">ContentNegotiationManager</a> contentNegotiationManager)`​Returns a [`HandlerExceptionResolverComposite`]()​ containing a list of exception resolvers obtained either through [`configureHandlerExceptionResolvers(java.util.List&lt;org.springframework.web.servlet.HandlerExceptionResolver&gt;)`]()​ or through [`addDefaultHandlerExceptionResolvers(java.util.List&lt;org.springframework.web.servlet.HandlerExceptionResolver&gt;, org.springframework.web.accept.ContentNegotiationManager)`]()​.|  
    |`<a href="https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/web/servlet/function/support/HandlerFunctionAdapter.html" title="class in org.springframework.web.servlet.function.support">HandlerFunctionAdapter</a>`​|`<span class="memberNameLink"><a href="https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/web/servlet/config/annotation/WebMvcConfigurationSupport.html#handlerFunctionAdapter--">handlerFunctionAdapter</a></span>()`​Returns a [`HandlerFunctionAdapter`]()​ for processing requests through [handler functions](https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/web/servlet/function/HandlerFunction.html "interface in org.springframework.web.servlet.function").|  
    |`<a href="https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/web/servlet/mvc/HttpRequestHandlerAdapter.html" title="class in org.springframework.web.servlet.mvc">HttpRequestHandlerAdapter</a>`​|`<span class="memberNameLink"><a href="https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/web/servlet/config/annotation/WebMvcConfigurationSupport.html#httpRequestHandlerAdapter--">httpRequestHandlerAdapter</a></span>()`​Returns a [`HttpRequestHandlerAdapter`]()​ for processing requests with [`HttpRequestHandlers`]()​.|  
    |`<a href="https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/web/servlet/LocaleResolver.html" title="interface in org.springframework.web.servlet">LocaleResolver</a>`​|`<span class="memberNameLink"><a href="https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/web/servlet/config/annotation/WebMvcConfigurationSupport.html#localeResolver--">localeResolver</a></span>()`​ |  
    |`<a href="https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/web/accept/ContentNegotiationManager.html" title="class in org.springframework.web.accept">ContentNegotiationManager</a>`​|`<span class="memberNameLink"><a href="https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/web/servlet/config/annotation/WebMvcConfigurationSupport.html#mvcContentNegotiationManager--">mvcContentNegotiationManager</a></span>()`​Return a [`ContentNegotiationManager`]()​ instance to use to determine requested [media types](https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/http/MediaType.html "class in org.springframework.http") in a given request.|  
    |`<a href="https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/format/support/FormattingConversionService.html" title="class in org.springframework.format.support">FormattingConversionService</a>`​|`<span class="memberNameLink"><a href="https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/web/servlet/config/annotation/WebMvcConfigurationSupport.html#mvcConversionService--">mvcConversionService</a></span>()`​Return a [`FormattingConversionService`]()​ for use with annotated controllers.|  
    |`<a href="https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/web/servlet/handler/HandlerMappingIntrospector.html" title="class in org.springframework.web.servlet.handler">HandlerMappingIntrospector</a>`​|`<span class="memberNameLink"><a href="https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/web/servlet/config/annotation/WebMvcConfigurationSupport.html#mvcHandlerMappingIntrospector--">mvcHandlerMappingIntrospector</a></span>()`​ |  
    |`<a href="https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/util/PathMatcher.html" title="interface in org.springframework.util">PathMatcher</a>`​|`<span class="memberNameLink"><a href="https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/web/servlet/config/annotation/WebMvcConfigurationSupport.html#mvcPathMatcher--">mvcPathMatcher</a></span>()`​Return a global [`PathMatcher`]()​ instance which is used for URL path matching with String patterns.|  
    |`<a href="https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/web/util/pattern/PathPatternParser.html" title="class in org.springframework.web.util.pattern">PathPatternParser</a>`​|`<span class="memberNameLink"><a href="https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/web/servlet/config/annotation/WebMvcConfigurationSupport.html#mvcPatternParser--">mvcPatternParser</a></span>()`​Return a global [`PathPatternParser`]()​ instance to use for parsing patterns to match to the [`RequestPath`]()​.|  
    |`<a href="https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/web/servlet/resource/ResourceUrlProvider.html" title="class in org.springframework.web.servlet.resource">ResourceUrlProvider</a>`​|`<span class="memberNameLink"><a href="https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/web/servlet/config/annotation/WebMvcConfigurationSupport.html#mvcResourceUrlProvider--">mvcResourceUrlProvider</a></span>()`​A [`ResourceUrlProvider`]()​ bean for use with the MVC dispatcher.|  
    |`<a href="https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/web/method/support/CompositeUriComponentsContributor.html" title="class in org.springframework.web.method.support">CompositeUriComponentsContributor</a>`​|`<span class="memberNameLink"><a href="https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/web/servlet/config/annotation/WebMvcConfigurationSupport.html#mvcUriComponentsContributor-org.springframework.format.support.FormattingConversionService-org.springframework.web.servlet.mvc.method.annotation.RequestMappingHandlerAdapter-">mvcUriComponentsContributor</a></span>(<a href="https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/format/support/FormattingConversionService.html" title="class in org.springframework.format.support">FormattingConversionService</a> conversionService,<span> </span><a href="https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/web/servlet/mvc/method/annotation/RequestMappingHandlerAdapter.html" title="class in org.springframework.web.servlet.mvc.method.annotation">RequestMappingHandlerAdapter</a> requestMappingHandlerAdapter)`​Return an instance of [`CompositeUriComponentsContributor`]()​ for use with [`MvcUriComponentsBuilder`]()​.|  
    |`<a href="https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/web/util/UrlPathHelper.html" title="class in org.springframework.web.util">UrlPathHelper</a>`​|`<span class="memberNameLink"><a href="https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/web/servlet/config/annotation/WebMvcConfigurationSupport.html#mvcUrlPathHelper--">mvcUrlPathHelper</a></span>()`​Return a global [`UrlPathHelper`]()​ instance which is used to resolve the request mapping path for an application.|  
    |`<a href="https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/validation/Validator.html" title="interface in org.springframework.validation">Validator</a>`​|`<span class="memberNameLink"><a href="https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/web/servlet/config/annotation/WebMvcConfigurationSupport.html#mvcValidator--">mvcValidator</a></span>()`​Return a global [`Validator`]()​ instance for example for validating `@ModelAttribute`​ and `@RequestBody`​ method arguments.|  
    |`<a href="https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/web/servlet/ViewResolver.html" title="interface in org.springframework.web.servlet">ViewResolver</a>`​|`<span class="memberNameLink"><a href="https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/web/servlet/config/annotation/WebMvcConfigurationSupport.html#mvcViewResolver-org.springframework.web.accept.ContentNegotiationManager-">mvcViewResolver</a></span>(<a href="https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/web/accept/ContentNegotiationManager.html" title="class in org.springframework.web.accept">ContentNegotiationManager</a> contentNegotiationManager)`​Register a [`ViewResolverComposite`]()​ that contains a chain of view resolvers to use for view resolution.|  
    |`<a href="https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/web/servlet/mvc/method/annotation/RequestMappingHandlerAdapter.html" title="class in org.springframework.web.servlet.mvc.method.annotation">RequestMappingHandlerAdapter</a>`​|`<span class="memberNameLink"><a href="https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/web/servlet/config/annotation/WebMvcConfigurationSupport.html#requestMappingHandlerAdapter-org.springframework.web.accept.ContentNegotiationManager-org.springframework.format.support.FormattingConversionService-org.springframework.validation.Validator-">requestMappingHandlerAdapter</a></span>(<a href="https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/web/accept/ContentNegotiationManager.html" title="class in org.springframework.web.accept">ContentNegotiationManager</a> contentNegotiationManager,<span> </span><a href="https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/format/support/FormattingConversionService.html" title="class in org.springframework.format.support">FormattingConversionService</a> conversionService,<span> </span><a href="https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/validation/Validator.html" title="interface in org.springframework.validation">Validator</a> validator)`​Returns a [`RequestMappingHandlerAdapter`]()​ for processing requests through annotated controller methods.|  
    |`<a href="https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/web/servlet/mvc/method/annotation/RequestMappingHandlerMapping.html" title="class in org.springframework.web.servlet.mvc.method.annotation">RequestMappingHandlerMapping</a>`​|`<span class="memberNameLink"><a href="https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/web/servlet/config/annotation/WebMvcConfigurationSupport.html#requestMappingHandlerMapping-org.springframework.web.accept.ContentNegotiationManager-org.springframework.format.support.FormattingConversionService-org.springframework.web.servlet.resource.ResourceUrlProvider-">requestMappingHandlerMapping</a></span>(<a href="https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/web/accept/ContentNegotiationManager.html" title="class in org.springframework.web.accept">ContentNegotiationManager</a> contentNegotiationManager,<span> </span><a href="https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/format/support/FormattingConversionService.html" title="class in org.springframework.format.support">FormattingConversionService</a> conversionService,<span> </span><a href="https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/web/servlet/resource/ResourceUrlProvider.html" title="class in org.springframework.web.servlet.resource">ResourceUrlProvider</a> resourceUrlProvider)`​Return a [`RequestMappingHandlerMapping`]()​ ordered at 0 for mapping requests to annotated controllers.|  
    |`<a href="https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/web/servlet/HandlerMapping.html" title="interface in org.springframework.web.servlet">HandlerMapping</a>`​|`<span class="memberNameLink"><a href="https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/web/servlet/config/annotation/WebMvcConfigurationSupport.html#resourceHandlerMapping-org.springframework.web.accept.ContentNegotiationManager-org.springframework.format.support.FormattingConversionService-org.springframework.web.servlet.resource.ResourceUrlProvider-">resourceHandlerMapping</a></span>(<a href="https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/web/accept/ContentNegotiationManager.html" title="class in org.springframework.web.accept">ContentNegotiationManager</a> contentNegotiationManager,<span> </span><a href="https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/format/support/FormattingConversionService.html" title="class in org.springframework.format.support">FormattingConversionService</a> conversionService,<span> </span><a href="https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/web/servlet/resource/ResourceUrlProvider.html" title="class in org.springframework.web.servlet.resource">ResourceUrlProvider</a> resourceUrlProvider)`​Return a handler mapping ordered at Integer.MAX_VALUE-1 with mapped resource handlers.|  
    |`<a href="https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/web/servlet/function/support/RouterFunctionMapping.html" title="class in org.springframework.web.servlet.function.support">RouterFunctionMapping</a>`​|`<span class="memberNameLink"><a href="https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/web/servlet/config/annotation/WebMvcConfigurationSupport.html#routerFunctionMapping-org.springframework.format.support.FormattingConversionService-org.springframework.web.servlet.resource.ResourceUrlProvider-">routerFunctionMapping</a></span>(<a href="https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/format/support/FormattingConversionService.html" title="class in org.springframework.format.support">FormattingConversionService</a> conversionService,<span> </span><a href="https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/web/servlet/resource/ResourceUrlProvider.html" title="class in org.springframework.web.servlet.resource">ResourceUrlProvider</a> resourceUrlProvider)`​Return a [`RouterFunctionMapping`]()​ ordered at 3 to map [router functions](https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/web/servlet/function/RouterFunction.html "interface in org.springframework.web.servlet.function").|  
    |`void`​|`<span class="memberNameLink"><a href="https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/web/servlet/config/annotation/WebMvcConfigurationSupport.html#setApplicationContext-org.springframework.context.ApplicationContext-">setApplicationContext</a></span>(<a href="https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/context/ApplicationContext.html" title="interface in org.springframework.context">ApplicationContext</a> applicationContext)`​Set the Spring [`ApplicationContext`]()​, e.g.|  
    |`void`​|`<span class="memberNameLink"><a href="https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/web/servlet/config/annotation/WebMvcConfigurationSupport.html#setServletContext-javax.servlet.ServletContext-">setServletContext</a></span>(<a href="https://docs.oracle.com/javaee/7/api/javax/servlet/ServletContext.html?is-external=true" title="class or interface in javax.servlet">ServletContext</a> servletContext)`​Set the [`ServletContext`]()​, e.g.|  
    |`<a href="https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/web/servlet/mvc/SimpleControllerHandlerAdapter.html" title="class in org.springframework.web.servlet.mvc">SimpleControllerHandlerAdapter</a>`​|`<span class="memberNameLink"><a href="https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/web/servlet/config/annotation/WebMvcConfigurationSupport.html#simpleControllerHandlerAdapter--">simpleControllerHandlerAdapter</a></span>()`​Returns a [`SimpleControllerHandlerAdapter`]()​ for processing requests with interface-based controllers.|  
    |`<a href="https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/web/servlet/ThemeResolver.html" title="interface in org.springframework.web.servlet">ThemeResolver</a>`​|`<span class="memberNameLink"><a href="https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/web/servlet/config/annotation/WebMvcConfigurationSupport.html#themeResolver--">themeResolver</a></span>()`​ |  
    |`<a href="https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/web/servlet/HandlerMapping.html" title="interface in org.springframework.web.servlet">HandlerMapping</a>`​|`<span class="memberNameLink"><a href="https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/web/servlet/config/annotation/WebMvcConfigurationSupport.html#viewControllerHandlerMapping-org.springframework.format.support.FormattingConversionService-org.springframework.web.servlet.resource.ResourceUrlProvider-">viewControllerHandlerMapping</a></span>(<a href="https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/format/support/FormattingConversionService.html" title="class in org.springframework.format.support">FormattingConversionService</a> conversionService,<span> </span><a href="https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/web/servlet/resource/ResourceUrlProvider.html" title="class in org.springframework.web.servlet.resource">ResourceUrlProvider</a> resourceUrlProvider)`​Return a handler mapping ordered at 1 to map URL paths directly to view names.|  
    |`<a href="https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/web/servlet/RequestToViewNameTranslator.html" title="interface in org.springframework.web.servlet">RequestToViewNameTranslator</a>`​|`<span class="memberNameLink"><a href="https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/web/servlet/config/annotation/WebMvcConfigurationSupport.html#viewNameTranslator--">viewNameTranslator</a></span>()`​ |

    * ### Methods inherited from class java.lang.[Object](https://docs.oracle.com/javase/8/docs/api/java/lang/Object.html?is-external=true "class or interface in java.lang")

      `<a href="https://docs.oracle.com/javase/8/docs/api/java/lang/Object.html?is-external=true#clone--" title="class or interface in java.lang">clone</a>,<span> </span><a href="https://docs.oracle.com/javase/8/docs/api/java/lang/Object.html?is-external=true#equals-java.lang.Object-" title="class or interface in java.lang">equals</a>,<span> </span><a href="https://docs.oracle.com/javase/8/docs/api/java/lang/Object.html?is-external=true#finalize--" title="class or interface in java.lang">finalize</a>,<span> </span><a href="https://docs.oracle.com/javase/8/docs/api/java/lang/Object.html?is-external=true#getClass--" title="class or interface in java.lang">getClass</a>,<span> </span><a href="https://docs.oracle.com/javase/8/docs/api/java/lang/Object.html?is-external=true#hashCode--" title="class or interface in java.lang">hashCode</a>,<span> </span><a href="https://docs.oracle.com/javase/8/docs/api/java/lang/Object.html?is-external=true#notify--" title="class or interface in java.lang">notify</a>,<span> </span><a href="https://docs.oracle.com/javase/8/docs/api/java/lang/Object.html?is-external=true#notifyAll--" title="class or interface in java.lang">notifyAll</a>,<span> </span><a href="https://docs.oracle.com/javase/8/docs/api/java/lang/Object.html?is-external=true#toString--" title="class or interface in java.lang">toString</a>,<span> </span><a href="https://docs.oracle.com/javase/8/docs/api/java/lang/Object.html?is-external=true#wait--" title="class or interface in java.lang">wait</a>,<span> </span><a href="https://docs.oracle.com/javase/8/docs/api/java/lang/Object.html?is-external=true#wait-long-" title="class or interface in java.lang">wait</a>,<span> </span><a href="https://docs.oracle.com/javase/8/docs/api/java/lang/Object.html?is-external=true#wait-long-int-" title="class or interface in java.lang">wait</a>`​
*  

  * ### Constructor Detail

    * #### WebMvcConfigurationSupport

      ```
      public WebMvcConfigurationSupport()
      ```
  * ### Method Detail

    * #### setApplicationContext

      ```
      public void setApplicationContext(@Nullable
                                        ApplicationContext applicationContext)
      ```

      Set the Spring [`ApplicationContext`]()​, e.g. for resource loading.  
      Specified by:`<a href="https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/context/ApplicationContextAware.html#setApplicationContext-org.springframework.context.ApplicationContext-">setApplicationContext</a>`​ in interface `<a href="https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/context/ApplicationContextAware.html" title="interface in org.springframework.context">ApplicationContextAware</a>`​Parameters:`applicationContext`​ - the ApplicationContext object to be used by this object**See Also:**​[`BeanInitializationException`]()​
    * #### getApplicationContext

      <pre><a href="https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/lang/Nullable.html" title="annotation in org.springframework.lang">@Nullable</a>
      public final <a href="https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/context/ApplicationContext.html" title="interface in org.springframework.context">ApplicationContext</a> getApplicationContext()</pre>

      Return the associated Spring [`ApplicationContext`]()​.  
      Since:4.2
    * #### setServletContext

      ```
      public void setServletContext(@Nullable
                                    ServletContext servletContext)
      ```

      Set the [`ServletContext`]()​, e.g. for resource handling, looking up file extensions, etc.  
      Specified by:`<a href="https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/web/context/ServletContextAware.html#setServletContext-javax.servlet.ServletContext-">setServletContext</a>`​ in interface `<a href="https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/web/context/ServletContextAware.html" title="interface in org.springframework.web.context">ServletContextAware</a>`​Parameters:`servletContext`​ - the ServletContext object to be used by this object**See Also:**​[`InitializingBean.afterPropertiesSet()`]()​, [`ApplicationContextAware.setApplicationContext(org.springframework.context.ApplicationContext)`]()​
    * #### getServletContext

      <pre><a href="https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/lang/Nullable.html" title="annotation in org.springframework.lang">@Nullable</a>
      public final <a href="https://docs.oracle.com/javaee/7/api/javax/servlet/ServletContext.html?is-external=true" title="class or interface in javax.servlet">ServletContext</a> getServletContext()</pre>

      Return the associated [`ServletContext`]()​.  
      Since:4.2
    * #### requestMappingHandlerMapping

      <pre><a href="https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/context/annotation/Bean.html" title="annotation in org.springframework.context.annotation">@Bean</a>
      public <a href="https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/web/servlet/mvc/method/annotation/RequestMappingHandlerMapping.html" title="class in org.springframework.web.servlet.mvc.method.annotation">RequestMappingHandlerMapping</a> requestMappingHandlerMapping(<a href="https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/beans/factory/annotation/Qualifier.html" title="annotation in org.springframework.beans.factory.annotation">@Qualifier</a>(<a href="https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/beans/factory/annotation/Qualifier.html#value--">value</a>="mvcContentNegotiationManager")
                                                                             <a href="https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/web/accept/ContentNegotiationManager.html" title="class in org.springframework.web.accept">ContentNegotiationManager</a> contentNegotiationManager,
                                                                             <a href="https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/beans/factory/annotation/Qualifier.html" title="annotation in org.springframework.beans.factory.annotation">@Qualifier</a>(<a href="https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/beans/factory/annotation/Qualifier.html#value--">value</a>="mvcConversionService")
                                                                             <a href="https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/format/support/FormattingConversionService.html" title="class in org.springframework.format.support">FormattingConversionService</a> conversionService,
                                                                             <a href="https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/beans/factory/annotation/Qualifier.html" title="annotation in org.springframework.beans.factory.annotation">@Qualifier</a>(<a href="https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/beans/factory/annotation/Qualifier.html#value--">value</a>="mvcResourceUrlProvider")
                                                                             <a href="https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/web/servlet/resource/ResourceUrlProvider.html" title="class in org.springframework.web.servlet.resource">ResourceUrlProvider</a> resourceUrlProvider)</pre>

      Return a [`RequestMappingHandlerMapping`]()​ ordered at 0 for mapping requests to annotated controllers.
    * #### createRequestMappingHandlerMapping

      ```
      protected RequestMappingHandlerMapping createRequestMappingHandlerMapping()
      ```

      Protected method for plugging in a custom subclass of [`RequestMappingHandlerMapping`]()​.  
      Since:4.0
    * #### getInterceptors

      ```
      protected final Object[] getInterceptors(FormattingConversionService mvcConversionService,
                                               ResourceUrlProvider mvcResourceUrlProvider)
      ```

      Provide access to the shared handler interceptors used to configure [`HandlerMapping`]()​ instances with.This method cannot be overridden; use [`addInterceptors(org.springframework.web.servlet.config.annotation.InterceptorRegistry)`]()​ instead.
    * #### addInterceptors

      ```
      protected void addInterceptors(InterceptorRegistry registry)
      ```

      Override this method to add Spring MVC interceptors for pre- and post-processing of controller invocation.  
      See Also:[`InterceptorRegistry`]()​
    * #### getPathMatchConfigurer

      ```
      protected PathMatchConfigurer getPathMatchConfigurer()
      ```

      Callback for building the [`PathMatchConfigurer`]()​. Delegates to [`configurePathMatch(org.springframework.web.servlet.config.annotation.PathMatchConfigurer)`]()​.  
      Since:4.1
    * #### configurePathMatch

      ```
      protected void configurePathMatch(PathMatchConfigurer configurer)
      ```

      Override this method to configure path matching options.  
      Since:4.0.3**See Also:**​[`PathMatchConfigurer`]()​
    * #### mvcPatternParser

      <pre><a href="https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/context/annotation/Bean.html" title="annotation in org.springframework.context.annotation">@Bean</a>
      public <a href="https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/web/util/pattern/PathPatternParser.html" title="class in org.springframework.web.util.pattern">PathPatternParser</a> mvcPatternParser()</pre>

      Return a global [`PathPatternParser`]()​ instance to use for parsing patterns to match to the [`RequestPath`]()​. The returned instance can be configured using [`configurePathMatch(PathMatchConfigurer)`]()​.  
      Since:5.3.4
    * #### mvcUrlPathHelper

      <pre><a href="https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/context/annotation/Bean.html" title="annotation in org.springframework.context.annotation">@Bean</a>
      public <a href="https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/web/util/UrlPathHelper.html" title="class in org.springframework.web.util">UrlPathHelper</a> mvcUrlPathHelper()</pre>

      Return a global [`UrlPathHelper`]()​ instance which is used to resolve the request mapping path for an application. The instance can be configured via [`configurePathMatch(PathMatchConfigurer)`]()​.**Note:** This is only used when parsed patterns are not [`enabled`]()​.

      Since:4.1
    * #### mvcPathMatcher

      <pre><a href="https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/context/annotation/Bean.html" title="annotation in org.springframework.context.annotation">@Bean</a>
      public <a href="https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/util/PathMatcher.html" title="interface in org.springframework.util">PathMatcher</a> mvcPathMatcher()</pre>

      Return a global [`PathMatcher`]()​ instance which is used for URL path matching with String patterns. The returned instance can be configured using [`configurePathMatch(PathMatchConfigurer)`]()​.**Note:** This is only used when parsed patterns are not [`enabled`]()​.

      Since:4.1
    * #### mvcContentNegotiationManager

      <pre><a href="https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/context/annotation/Bean.html" title="annotation in org.springframework.context.annotation">@Bean</a>
      public <a href="https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/web/accept/ContentNegotiationManager.html" title="class in org.springframework.web.accept">ContentNegotiationManager</a> mvcContentNegotiationManager()</pre>

      Return a [`ContentNegotiationManager`]()​ instance to use to determine requested [media types](https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/http/MediaType.html "class in org.springframework.http") in a given request.
    * #### getDefaultMediaTypes

      ```
      protected Map<String,MediaType> getDefaultMediaTypes()
      ```
    * #### configureContentNegotiation

      ```
      protected void configureContentNegotiation(ContentNegotiationConfigurer configurer)
      ```

      Override this method to configure content negotiation.  
      See Also:[`DefaultServletHandlerConfigurer`]()​
    * #### viewControllerHandlerMapping

      <pre><a href="https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/context/annotation/Bean.html" title="annotation in org.springframework.context.annotation">@Bean</a>
       <a href="https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/lang/Nullable.html" title="annotation in org.springframework.lang">@Nullable</a>
      public <a href="https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/web/servlet/HandlerMapping.html" title="interface in org.springframework.web.servlet">HandlerMapping</a> viewControllerHandlerMapping(<a href="https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/beans/factory/annotation/Qualifier.html" title="annotation in org.springframework.beans.factory.annotation">@Qualifier</a>(<a href="https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/beans/factory/annotation/Qualifier.html#value--">value</a>="mvcConversionService")
                                                                          <a href="https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/format/support/FormattingConversionService.html" title="class in org.springframework.format.support">FormattingConversionService</a> conversionService,
                                                                          <a href="https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/beans/factory/annotation/Qualifier.html" title="annotation in org.springframework.beans.factory.annotation">@Qualifier</a>(<a href="https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/beans/factory/annotation/Qualifier.html#value--">value</a>="mvcResourceUrlProvider")
                                                                          <a href="https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/web/servlet/resource/ResourceUrlProvider.html" title="class in org.springframework.web.servlet.resource">ResourceUrlProvider</a> resourceUrlProvider)</pre>

      Return a handler mapping ordered at 1 to map URL paths directly to view names. To configure view controllers, override [`addViewControllers(org.springframework.web.servlet.config.annotation.ViewControllerRegistry)`]()​.
    * #### addViewControllers

      ```
      protected void addViewControllers(ViewControllerRegistry registry)
      ```

      Override this method to add view controllers.  
      See Also:[`ViewControllerRegistry`]()​
    * #### beanNameHandlerMapping

      <pre><a href="https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/context/annotation/Bean.html" title="annotation in org.springframework.context.annotation">@Bean</a>
      public <a href="https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/web/servlet/handler/BeanNameUrlHandlerMapping.html" title="class in org.springframework.web.servlet.handler">BeanNameUrlHandlerMapping</a> beanNameHandlerMapping(<a href="https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/beans/factory/annotation/Qualifier.html" title="annotation in org.springframework.beans.factory.annotation">@Qualifier</a>(<a href="https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/beans/factory/annotation/Qualifier.html#value--">value</a>="mvcConversionService")
                                                                    <a href="https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/format/support/FormattingConversionService.html" title="class in org.springframework.format.support">FormattingConversionService</a> conversionService,
                                                                    <a href="https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/beans/factory/annotation/Qualifier.html" title="annotation in org.springframework.beans.factory.annotation">@Qualifier</a>(<a href="https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/beans/factory/annotation/Qualifier.html#value--">value</a>="mvcResourceUrlProvider")
                                                                    <a href="https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/web/servlet/resource/ResourceUrlProvider.html" title="class in org.springframework.web.servlet.resource">ResourceUrlProvider</a> resourceUrlProvider)</pre>

      Return a [`BeanNameUrlHandlerMapping`]()​ ordered at 2 to map URL paths to controller bean names.
    * #### routerFunctionMapping

      <pre><a href="https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/context/annotation/Bean.html" title="annotation in org.springframework.context.annotation">@Bean</a>
      public <a href="https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/web/servlet/function/support/RouterFunctionMapping.html" title="class in org.springframework.web.servlet.function.support">RouterFunctionMapping</a> routerFunctionMapping(<a href="https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/beans/factory/annotation/Qualifier.html" title="annotation in org.springframework.beans.factory.annotation">@Qualifier</a>(<a href="https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/beans/factory/annotation/Qualifier.html#value--">value</a>="mvcConversionService")
                                                               <a href="https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/format/support/FormattingConversionService.html" title="class in org.springframework.format.support">FormattingConversionService</a> conversionService,
                                                               <a href="https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/beans/factory/annotation/Qualifier.html" title="annotation in org.springframework.beans.factory.annotation">@Qualifier</a>(<a href="https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/beans/factory/annotation/Qualifier.html#value--">value</a>="mvcResourceUrlProvider")
                                                               <a href="https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/web/servlet/resource/ResourceUrlProvider.html" title="class in org.springframework.web.servlet.resource">ResourceUrlProvider</a> resourceUrlProvider)</pre>

      Return a [`RouterFunctionMapping`]()​ ordered at 3 to map [router functions](https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/web/servlet/function/RouterFunction.html "interface in org.springframework.web.servlet.function"). Consider overriding one of these other more fine-grained methods:* [`addInterceptors(org.springframework.web.servlet.config.annotation.InterceptorRegistry)`]()​ for adding handler interceptors.

      * [`addCorsMappings(org.springframework.web.servlet.config.annotation.CorsRegistry)`]()​ to configure cross origin requests processing.
      * [`configureMessageConverters(java.util.List&lt;org.springframework.http.converter.HttpMessageConverter&lt;?&gt;&gt;)`]()​ for adding custom message converters.
      * [`configurePathMatch(PathMatchConfigurer)`]()​ for customizing the [`PathPatternParser`]()​.

      Since:5.2
    * #### resourceHandlerMapping

      <pre><a href="https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/context/annotation/Bean.html" title="annotation in org.springframework.context.annotation">@Bean</a>
       <a href="https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/lang/Nullable.html" title="annotation in org.springframework.lang">@Nullable</a>
      public <a href="https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/web/servlet/HandlerMapping.html" title="interface in org.springframework.web.servlet">HandlerMapping</a> resourceHandlerMapping(<a href="https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/beans/factory/annotation/Qualifier.html" title="annotation in org.springframework.beans.factory.annotation">@Qualifier</a>(<a href="https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/beans/factory/annotation/Qualifier.html#value--">value</a>="mvcContentNegotiationManager")
                                                                    <a href="https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/web/accept/ContentNegotiationManager.html" title="class in org.springframework.web.accept">ContentNegotiationManager</a> contentNegotiationManager,
                                                                    <a href="https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/beans/factory/annotation/Qualifier.html" title="annotation in org.springframework.beans.factory.annotation">@Qualifier</a>(<a href="https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/beans/factory/annotation/Qualifier.html#value--">value</a>="mvcConversionService")
                                                                    <a href="https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/format/support/FormattingConversionService.html" title="class in org.springframework.format.support">FormattingConversionService</a> conversionService,
                                                                    <a href="https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/beans/factory/annotation/Qualifier.html" title="annotation in org.springframework.beans.factory.annotation">@Qualifier</a>(<a href="https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/beans/factory/annotation/Qualifier.html#value--">value</a>="mvcResourceUrlProvider")
                                                                    <a href="https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/web/servlet/resource/ResourceUrlProvider.html" title="class in org.springframework.web.servlet.resource">ResourceUrlProvider</a> resourceUrlProvider)</pre>

      Return a handler mapping ordered at Integer.MAX_VALUE-1 with mapped resource handlers. To configure resource handling, override [`addResourceHandlers(org.springframework.web.servlet.config.annotation.ResourceHandlerRegistry)`]()​.
    * #### addResourceHandlers

      ```
      protected void addResourceHandlers(ResourceHandlerRegistry registry)
      ```

      Override this method to add resource handlers for serving static resources.  
      See Also:[`ResourceHandlerRegistry`]()​
    * #### mvcResourceUrlProvider

      <pre><a href="https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/context/annotation/Bean.html" title="annotation in org.springframework.context.annotation">@Bean</a>
      public <a href="https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/web/servlet/resource/ResourceUrlProvider.html" title="class in org.springframework.web.servlet.resource">ResourceUrlProvider</a> mvcResourceUrlProvider()</pre>

      A [`ResourceUrlProvider`]()​ bean for use with the MVC dispatcher.  
      Since:4.1
    * #### defaultServletHandlerMapping

      <pre><a href="https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/context/annotation/Bean.html" title="annotation in org.springframework.context.annotation">@Bean</a>
       <a href="https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/lang/Nullable.html" title="annotation in org.springframework.lang">@Nullable</a>
      public <a href="https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/web/servlet/HandlerMapping.html" title="interface in org.springframework.web.servlet">HandlerMapping</a> defaultServletHandlerMapping()</pre>

      Return a handler mapping ordered at Integer.MAX_VALUE with a mapped default servlet handler. To configure "default" Servlet handling, override [`configureDefaultServletHandling(org.springframework.web.servlet.config.annotation.DefaultServletHandlerConfigurer)`]()​.
    * #### configureDefaultServletHandling

      ```
      protected void configureDefaultServletHandling(DefaultServletHandlerConfigurer configurer)
      ```

      Override this method to configure "default" Servlet handling.  
      See Also:[`DefaultServletHandlerConfigurer`]()​
    * #### requestMappingHandlerAdapter

      <pre><a href="https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/context/annotation/Bean.html" title="annotation in org.springframework.context.annotation">@Bean</a>
      public <a href="https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/web/servlet/mvc/method/annotation/RequestMappingHandlerAdapter.html" title="class in org.springframework.web.servlet.mvc.method.annotation">RequestMappingHandlerAdapter</a> requestMappingHandlerAdapter(<a href="https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/beans/factory/annotation/Qualifier.html" title="annotation in org.springframework.beans.factory.annotation">@Qualifier</a>(<a href="https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/beans/factory/annotation/Qualifier.html#value--">value</a>="mvcContentNegotiationManager")
                                                                             <a href="https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/web/accept/ContentNegotiationManager.html" title="class in org.springframework.web.accept">ContentNegotiationManager</a> contentNegotiationManager,
                                                                             <a href="https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/beans/factory/annotation/Qualifier.html" title="annotation in org.springframework.beans.factory.annotation">@Qualifier</a>(<a href="https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/beans/factory/annotation/Qualifier.html#value--">value</a>="mvcConversionService")
                                                                             <a href="https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/format/support/FormattingConversionService.html" title="class in org.springframework.format.support">FormattingConversionService</a> conversionService,
                                                                             <a href="https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/beans/factory/annotation/Qualifier.html" title="annotation in org.springframework.beans.factory.annotation">@Qualifier</a>(<a href="https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/beans/factory/annotation/Qualifier.html#value--">value</a>="mvcValidator")
                                                                             <a href="https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/validation/Validator.html" title="interface in org.springframework.validation">Validator</a> validator)</pre>

      Returns a [`RequestMappingHandlerAdapter`]()​ for processing requests through annotated controller methods. Consider overriding one of these other more fine-grained methods:* [`addArgumentResolvers(java.util.List&lt;org.springframework.web.method.support.HandlerMethodArgumentResolver&gt;)`]()​ for adding custom argument resolvers.

      * [`addReturnValueHandlers(java.util.List&lt;org.springframework.web.method.support.HandlerMethodReturnValueHandler&gt;)`]()​ for adding custom return value handlers.
      * [`configureMessageConverters(java.util.List&lt;org.springframework.http.converter.HttpMessageConverter&lt;?&gt;&gt;)`]()​ for adding custom message converters.
    * #### createRequestMappingHandlerAdapter

      ```
      protected RequestMappingHandlerAdapter createRequestMappingHandlerAdapter()
      ```

      Protected method for plugging in a custom subclass of [`RequestMappingHandlerAdapter`]()​.  
      Since:4.3
    * #### handlerFunctionAdapter

      <pre><a href="https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/context/annotation/Bean.html" title="annotation in org.springframework.context.annotation">@Bean</a>
      public <a href="https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/web/servlet/function/support/HandlerFunctionAdapter.html" title="class in org.springframework.web.servlet.function.support">HandlerFunctionAdapter</a> handlerFunctionAdapter()</pre>

      Returns a [`HandlerFunctionAdapter`]()​ for processing requests through [handler functions](https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/web/servlet/function/HandlerFunction.html "interface in org.springframework.web.servlet.function").  
      Since:5.2
    * #### getConfigurableWebBindingInitializer

      ```
      protected ConfigurableWebBindingInitializer getConfigurableWebBindingInitializer(FormattingConversionService mvcConversionService,
                                                                                       Validator mvcValidator)
      ```

      Return the [`ConfigurableWebBindingInitializer`]()​ to use for initializing all [`WebDataBinder`]()​ instances.
    * #### getMessageCodesResolver

      <pre><a href="https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/lang/Nullable.html" title="annotation in org.springframework.lang">@Nullable</a>
      protected <a href="https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/validation/MessageCodesResolver.html" title="interface in org.springframework.validation">MessageCodesResolver</a> getMessageCodesResolver()</pre>

      Override this method to provide a custom [`MessageCodesResolver`]()​.
    * #### mvcConversionService

      <pre><a href="https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/context/annotation/Bean.html" title="annotation in org.springframework.context.annotation">@Bean</a>
      public <a href="https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/format/support/FormattingConversionService.html" title="class in org.springframework.format.support">FormattingConversionService</a> mvcConversionService()</pre>

      Return a [`FormattingConversionService`]()​ for use with annotated controllers.See [`addFormatters(org.springframework.format.FormatterRegistry)`]()​ as an alternative to overriding this method.
    * #### addFormatters

      ```
      protected void addFormatters(FormatterRegistry registry)
      ```

      Override this method to add custom [`Converter`]()​ and/or [`Formatter`]()​ delegates to the common [`FormattingConversionService`]()​.  
      See Also:[`mvcConversionService()`]()​
    * #### mvcValidator

      <pre><a href="https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/context/annotation/Bean.html" title="annotation in org.springframework.context.annotation">@Bean</a>
      public <a href="https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/validation/Validator.html" title="interface in org.springframework.validation">Validator</a> mvcValidator()</pre>

      Return a global [`Validator`]()​ instance for example for validating `@ModelAttribute`​ and `@RequestBody`​ method arguments. Delegates to [`getValidator()`]()​ first and if that returns `null`​ checks the classpath for the presence of a JSR-303 implementations before creating a `OptionalValidatorFactoryBean`​.If a JSR-303 implementation is not available, a no-op [`Validator`]()​ is returned.
    * #### getValidator

      <pre><a href="https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/lang/Nullable.html" title="annotation in org.springframework.lang">@Nullable</a>
      protected <a href="https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/validation/Validator.html" title="interface in org.springframework.validation">Validator</a> getValidator()</pre>

      Override this method to provide a custom [`Validator`]()​.
    * #### getArgumentResolvers

      ```
      protected final List<HandlerMethodArgumentResolver> getArgumentResolvers()
      ```

      Provide access to the shared custom argument resolvers used by the [`RequestMappingHandlerAdapter`]()​ and the [`ExceptionHandlerExceptionResolver`]()​.This method cannot be overridden; use [`addArgumentResolvers(java.util.List&lt;org.springframework.web.method.support.HandlerMethodArgumentResolver&gt;)`]()​ instead.

      Since:4.3
    * #### addArgumentResolvers

      ```
      protected void addArgumentResolvers(List<HandlerMethodArgumentResolver> argumentResolvers)
      ```

      Add custom [`HandlerMethodArgumentResolvers`]()​ to use in addition to the ones registered by default.Custom argument resolvers are invoked before built-in resolvers except for those that rely on the presence of annotations (e.g. `@RequestParameter`​, `@PathVariable`​, etc). The latter can be customized by configuring the [`RequestMappingHandlerAdapter`]()​ directly.

      Parameters:`argumentResolvers`​ - the list of custom converters (initially an empty list)
    * #### getReturnValueHandlers

      ```
      protected final List<HandlerMethodReturnValueHandler> getReturnValueHandlers()
      ```

      Provide access to the shared return value handlers used by the [`RequestMappingHandlerAdapter`]()​ and the [`ExceptionHandlerExceptionResolver`]()​.This method cannot be overridden; use [`addReturnValueHandlers(java.util.List&lt;org.springframework.web.method.support.HandlerMethodReturnValueHandler&gt;)`]()​ instead.

      Since:4.3
    * #### addReturnValueHandlers

      ```
      protected void addReturnValueHandlers(List<HandlerMethodReturnValueHandler> returnValueHandlers)
      ```

      Add custom [`HandlerMethodReturnValueHandlers`]()​ in addition to the ones registered by default.Custom return value handlers are invoked before built-in ones except for those that rely on the presence of annotations (e.g. `@ResponseBody`​, `@ModelAttribute`​, etc). The latter can be customized by configuring the [`RequestMappingHandlerAdapter`]()​ directly.

      Parameters:`returnValueHandlers`​ - the list of custom handlers (initially an empty list)
    * #### getMessageConverters

      ```
      protected final List<HttpMessageConverter<?>> getMessageConverters()
      ```

      Provides access to the shared [`HttpMessageConverters`]()​ used by the [`RequestMappingHandlerAdapter`]()​ and the [`ExceptionHandlerExceptionResolver`]()​.This method cannot be overridden; use [`configureMessageConverters(java.util.List&lt;org.springframework.http.converter.HttpMessageConverter&lt;?&gt;&gt;)`]()​ instead. Also see [`addDefaultHttpMessageConverters(java.util.List&lt;org.springframework.http.converter.HttpMessageConverter&lt;?&gt;&gt;)`]()​ for adding default message converters.
    * #### configureMessageConverters

      ```
      protected void configureMessageConverters(List<HttpMessageConverter<?>> converters)
      ```

      Override this method to add custom [`HttpMessageConverters`]()​ to use with the [`RequestMappingHandlerAdapter`]()​ and the [`ExceptionHandlerExceptionResolver`]()​.Adding converters to the list turns off the default converters that would otherwise be registered by default. Also see [`addDefaultHttpMessageConverters(java.util.List&lt;org.springframework.http.converter.HttpMessageConverter&lt;?&gt;&gt;)`]()​ for adding default message converters.

      Parameters:`converters`​ - a list to add message converters to (initially an empty list)
    * #### extendMessageConverters

      ```
      protected void extendMessageConverters(List<HttpMessageConverter<?>> converters)
      ```

      Override this method to extend or modify the list of converters after it has been configured. This may be useful for example to allow default converters to be registered and then insert a custom converter through this method.  
      Parameters:`converters`​ - the list of configured converters to extend**Since:**4.1.3
    * #### addDefaultHttpMessageConverters

      ```
      protected final void addDefaultHttpMessageConverters(List<HttpMessageConverter<?>> messageConverters)
      ```

      Adds a set of default HttpMessageConverter instances to the given list. Subclasses can call this method from [`configureMessageConverters(java.util.List&lt;org.springframework.http.converter.HttpMessageConverter&lt;?&gt;&gt;)`]()​.  
      Parameters:`messageConverters`​ - the list to add the default message converters to
    * #### getAsyncSupportConfigurer

      ```
      protected AsyncSupportConfigurer getAsyncSupportConfigurer()
      ```

      Callback for building the [`AsyncSupportConfigurer`]()​. Delegates to [`configureAsyncSupport(AsyncSupportConfigurer)`]()​.  
      Since:5.3.2
    * #### configureAsyncSupport

      ```
      protected void configureAsyncSupport(AsyncSupportConfigurer configurer)
      ```

      Override this method to configure asynchronous request processing options.  
      See Also:[`AsyncSupportConfigurer`]()​
    * #### mvcUriComponentsContributor

      <pre><a href="https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/context/annotation/Bean.html" title="annotation in org.springframework.context.annotation">@Bean</a>
      public <a href="https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/web/method/support/CompositeUriComponentsContributor.html" title="class in org.springframework.web.method.support">CompositeUriComponentsContributor</a> mvcUriComponentsContributor(<a href="https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/beans/factory/annotation/Qualifier.html" title="annotation in org.springframework.beans.factory.annotation">@Qualifier</a>(<a href="https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/beans/factory/annotation/Qualifier.html#value--">value</a>="mvcConversionService")
                                                                                 <a href="https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/format/support/FormattingConversionService.html" title="class in org.springframework.format.support">FormattingConversionService</a> conversionService,
                                                                                 <a href="https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/beans/factory/annotation/Qualifier.html" title="annotation in org.springframework.beans.factory.annotation">@Qualifier</a>(<a href="https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/beans/factory/annotation/Qualifier.html#value--">value</a>="requestMappingHandlerAdapter")
                                                                                 <a href="https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/web/servlet/mvc/method/annotation/RequestMappingHandlerAdapter.html" title="class in org.springframework.web.servlet.mvc.method.annotation">RequestMappingHandlerAdapter</a> requestMappingHandlerAdapter)</pre>

      Return an instance of [`CompositeUriComponentsContributor`]()​ for use with [`MvcUriComponentsBuilder`]()​.  
      Since:4.0
    * #### httpRequestHandlerAdapter

      <pre><a href="https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/context/annotation/Bean.html" title="annotation in org.springframework.context.annotation">@Bean</a>
      public <a href="https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/web/servlet/mvc/HttpRequestHandlerAdapter.html" title="class in org.springframework.web.servlet.mvc">HttpRequestHandlerAdapter</a> httpRequestHandlerAdapter()</pre>

      Returns a [`HttpRequestHandlerAdapter`]()​ for processing requests with [`HttpRequestHandlers`]()​.
    * #### simpleControllerHandlerAdapter

      <pre><a href="https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/context/annotation/Bean.html" title="annotation in org.springframework.context.annotation">@Bean</a>
      public <a href="https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/web/servlet/mvc/SimpleControllerHandlerAdapter.html" title="class in org.springframework.web.servlet.mvc">SimpleControllerHandlerAdapter</a> simpleControllerHandlerAdapter()</pre>

      Returns a [`SimpleControllerHandlerAdapter`]()​ for processing requests with interface-based controllers.
    * #### handlerExceptionResolver

      <pre><a href="https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/context/annotation/Bean.html" title="annotation in org.springframework.context.annotation">@Bean</a>
      public <a href="https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/web/servlet/HandlerExceptionResolver.html" title="interface in org.springframework.web.servlet">HandlerExceptionResolver</a> handlerExceptionResolver(<a href="https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/beans/factory/annotation/Qualifier.html" title="annotation in org.springframework.beans.factory.annotation">@Qualifier</a>(<a href="https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/beans/factory/annotation/Qualifier.html#value--">value</a>="mvcContentNegotiationManager")
                                                                     <a href="https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/web/accept/ContentNegotiationManager.html" title="class in org.springframework.web.accept">ContentNegotiationManager</a> contentNegotiationManager)</pre>

      Returns a [`HandlerExceptionResolverComposite`]()​ containing a list of exception resolvers obtained either through [`configureHandlerExceptionResolvers(java.util.List&lt;org.springframework.web.servlet.HandlerExceptionResolver&gt;)`]()​ or through [`addDefaultHandlerExceptionResolvers(java.util.List&lt;org.springframework.web.servlet.HandlerExceptionResolver&gt;, org.springframework.web.accept.ContentNegotiationManager)`]()​.**Note:** This method cannot be made final due to CGLIB constraints. Rather than overriding it, consider overriding [`configureHandlerExceptionResolvers(java.util.List&lt;org.springframework.web.servlet.HandlerExceptionResolver&gt;)`]()​ which allows for providing a list of resolvers.
    * #### configureHandlerExceptionResolvers

      ```
      protected void configureHandlerExceptionResolvers(List<HandlerExceptionResolver> exceptionResolvers)
      ```

      Override this method to configure the list of [`HandlerExceptionResolvers`]()​ to use.Adding resolvers to the list turns off the default resolvers that would otherwise be registered by default. Also see [`addDefaultHandlerExceptionResolvers(java.util.List&lt;org.springframework.web.servlet.HandlerExceptionResolver&gt;, org.springframework.web.accept.ContentNegotiationManager)`]()​ that can be used to add the default exception resolvers.

      Parameters:`exceptionResolvers`​ - a list to add exception resolvers to (initially an empty list)
    * #### extendHandlerExceptionResolvers

      ```
      protected void extendHandlerExceptionResolvers(List<HandlerExceptionResolver> exceptionResolvers)
      ```

      Override this method to extend or modify the list of [`HandlerExceptionResolvers`]()​ after it has been configured.This may be useful for example to allow default resolvers to be registered and then insert a custom one through this method.

      Parameters:`exceptionResolvers`​ - the list of configured resolvers to extend.**Since:**4.3
    * #### addDefaultHandlerExceptionResolvers

      ```
      protected final void addDefaultHandlerExceptionResolvers(List<HandlerExceptionResolver> exceptionResolvers,
                                                               ContentNegotiationManager mvcContentNegotiationManager)
      ```

      A method available to subclasses for adding default [`HandlerExceptionResolvers`]()​.Adds the following exception resolvers:

      * [`ExceptionHandlerExceptionResolver`]()​ for handling exceptions through [`ExceptionHandler`]()​ methods.
      * [`ResponseStatusExceptionResolver`]()​ for exceptions annotated with [`ResponseStatus`]()​.
      * [`DefaultHandlerExceptionResolver`]()​ for resolving known Spring exception types
    * #### createExceptionHandlerExceptionResolver

      ```
      protected ExceptionHandlerExceptionResolver createExceptionHandlerExceptionResolver()
      ```

      Protected method for plugging in a custom subclass of [`ExceptionHandlerExceptionResolver`]()​.  
      Since:4.3
    * #### mvcViewResolver

      <pre><a href="https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/context/annotation/Bean.html" title="annotation in org.springframework.context.annotation">@Bean</a>
      public <a href="https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/web/servlet/ViewResolver.html" title="interface in org.springframework.web.servlet">ViewResolver</a> mvcViewResolver(<a href="https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/beans/factory/annotation/Qualifier.html" title="annotation in org.springframework.beans.factory.annotation">@Qualifier</a>(<a href="https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/beans/factory/annotation/Qualifier.html#value--">value</a>="mvcContentNegotiationManager")
                                                <a href="https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/web/accept/ContentNegotiationManager.html" title="class in org.springframework.web.accept">ContentNegotiationManager</a> contentNegotiationManager)</pre>

      Register a [`ViewResolverComposite`]()​ that contains a chain of view resolvers to use for view resolution. By default, this resolver is ordered at 0 unless content negotiation view resolution is used in which case the order is raised to [`Ordered.HIGHEST\_PRECEDENCE`]()​.If no other resolvers are configured, [`ViewResolverComposite.resolveViewName(String, Locale)`]()​ returns null in order to allow other potential [`ViewResolver`]()​ beans to resolve views.

      Since:4.1
    * #### configureViewResolvers

      ```
      protected void configureViewResolvers(ViewResolverRegistry registry)
      ```

      Override this method to configure view resolution.  
      See Also:[`ViewResolverRegistry`]()​
    * #### getCorsConfigurations

      ```
      protected final Map<String,CorsConfiguration> getCorsConfigurations()
      ```

      Return the registered [`CorsConfiguration`]()​ objects, keyed by path pattern.  
      Since:4.2
    * #### addCorsMappings

      ```
      protected void addCorsMappings(CorsRegistry registry)
      ```

      Override this method to configure cross-origin requests processing.  
      Since:4.2**See Also:**​[`CorsRegistry`]()​
    * #### mvcHandlerMappingIntrospector

      <pre><a href="https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/context/annotation/Bean.html" title="annotation in org.springframework.context.annotation">@Bean</a>
       <a href="https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/context/annotation/Lazy.html" title="annotation in org.springframework.context.annotation">@Lazy</a>
      public <a href="https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/web/servlet/handler/HandlerMappingIntrospector.html" title="class in org.springframework.web.servlet.handler">HandlerMappingIntrospector</a> mvcHandlerMappingIntrospector()</pre>
    * #### localeResolver

      <pre><a href="https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/context/annotation/Bean.html" title="annotation in org.springframework.context.annotation">@Bean</a>
      public <a href="https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/web/servlet/LocaleResolver.html" title="interface in org.springframework.web.servlet">LocaleResolver</a> localeResolver()</pre>
    * #### themeResolver

      <pre><a href="https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/context/annotation/Bean.html" title="annotation in org.springframework.context.annotation">@Bean</a>
      public <a href="https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/web/servlet/ThemeResolver.html" title="interface in org.springframework.web.servlet">ThemeResolver</a> themeResolver()</pre>
    * #### flashMapManager

      <pre><a href="https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/context/annotation/Bean.html" title="annotation in org.springframework.context.annotation">@Bean</a>
      public <a href="https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/web/servlet/FlashMapManager.html" title="interface in org.springframework.web.servlet">FlashMapManager</a> flashMapManager()</pre>
    * #### viewNameTranslator

      <pre><a href="https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/context/annotation/Bean.html" title="annotation in org.springframework.context.annotation">@Bean</a>
      public <a href="https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/web/servlet/RequestToViewNameTranslator.html" title="interface in org.springframework.web.servlet">RequestToViewNameTranslator</a> viewNameTranslator()</pre>

[SKIP NAVIGATION LINKS](https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/web/servlet/config/annotation/WebMvcConfigurationSupport.html#skip.navbar.bottom "Skip navigation links")

* [OVERVIEW](https://docs.spring.io/spring-framework/docs/current/javadoc-api/overview-summary.html)
* [PACKAGE](https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/web/servlet/config/annotation/package-summary.html)
* CLASS
* [USE](https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/web/servlet/config/annotation/class-use/WebMvcConfigurationSupport.html)
* [TREE](https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/web/servlet/config/annotation/package-tree.html)
* [DEPRECATED](https://docs.spring.io/spring-framework/docs/current/javadoc-api/deprecated-list.html)
* [INDEX](https://docs.spring.io/spring-framework/docs/current/javadoc-api/index-files/index-1.html)
* [HELP](https://docs.spring.io/spring-framework/docs/current/javadoc-api/help-doc.html)

Spring Framework

* [PREV CLASS](https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/web/servlet/config/annotation/ViewResolverRegistry.html "class in org.springframework.web.servlet.config.annotation")
* [NEXT CLASS](https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/web/servlet/config/annotation/WebMvcConfigurer.html "interface in org.springframework.web.servlet.config.annotation")
* [FRAMES](https://docs.spring.io/spring-framework/docs/current/javadoc-api/index.html?org/springframework/web/servlet/config/annotation/WebMvcConfigurationSupport.html)
* [NO FRAMES](https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/web/servlet/config/annotation/WebMvcConfigurationSupport.html)
* [ALL CLASSES](https://docs.spring.io/spring-framework/docs/current/javadoc-api/allclasses-noframe.html)
* SUMMARY:
* NESTED |
* FIELD |
* [CONSTR](https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/web/servlet/config/annotation/WebMvcConfigurationSupport.html#constructor.summary) |
* [METHOD](https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/web/servlet/config/annotation/WebMvcConfigurationSupport.html#method.summary)
* DETAIL:
* FIELD |
* [CONSTR](https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/web/servlet/config/annotation/WebMvcConfigurationSupport.html#constructor.detail) |
* [METHOD](https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/web/servlet/config/annotation/WebMvcConfigurationSupport.html#method.detail)
