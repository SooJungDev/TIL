## MessageSource 설정하기

~~~ java
@Bean
public MessageSource messageSource(){
    ResourceBundelMessageSource messageSource = new ResourceBundleMessageSource();
    
    //해당 폴더에서 properties를 찾는다.
    messageSource.setBasename("classpath:/messages/messages");
    messageSource.setDefaultEncoding("UTF-8");
    return messageSource;
}

@Bean
public LocaleChangeInterceptor localeChangeInterceptor(){
    LocaleChangeInterceptor localeChangeInterceptor = new LocaleChangeInterceptor();
    //request로 넘어오는 language parameter를 받아서 locale로 설정한다.
    localeChangeInterceptor.setParamName("language");
    return localChangeInterceptor;
}

@Bean
public LocaleResolver localeResolver(){
   //세션 기준으로 로케일을 설정한다.
   SessionLocaleResolver localResolver = new SessionLocaleResolver();
   
   //쿠키 기준(세션이 끊겨도 브라우저에 설정된 쿠키기준으로)
   //CookieLocaleResolver localeResolver= new CookieLocaleResolver();
   
   //기본 로케일을 강제로 설정가능
   localeResolver.setDefaultLocale(new Locale("en_US"));
   return localeResolver;

}

public void addInterceptors(InterceptorRegistry registry){
    //interceptor를 추가한다.
    registry.addInterceptor(localeChangeInterceptor());
}
~~~


## 참고사이트
- http://lahuman.jabsiri.co.kr/87