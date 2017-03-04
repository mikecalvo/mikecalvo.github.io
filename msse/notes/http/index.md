---
title: Web Application Development
layout: default
---

## Consuming REST Services

### Marc Kapke

#### kapkema@gmail.com

---

# Monolith

- Most, if not all, of system on single server
- DB Access
- Some RPC (SOAP,etc)

# Microservices

- System domain distributed across several servers
- Very dependent on Server to Server communication
- Some DB Access - but most data fetched from other Domain services

---

# Rise of REST
- Existing protocols (SOAP, etc) were to heavy-weight
    - Bloated messages
    - Message processing overhead
    - Lots of configuration

---

# Why REST took off
- Standard HTTP
- Cachable
- Fast and efficient
    - No additional message processing
    - Smaller message formats

---

# Spring RestTemplate

- `RestTemplate` is Spring's HTTP Client
    - Central class for synchronous http access
    - Supports all major REST Verbs
        - (GET, PUT, POST, DELETE, HEAD, OPTIONS)
    - Simply provide a Url
    - Returns an Http Response with message body

---

# Serialization
- Response can be retrieved as JSON String
    - `String jsonString = restTemplate.getForObject("http://myurl.com", String)`
- Response can be serialized as POJO
    - `MyDomainClass responseBody = restTemplate.getForEntity("http://myurl.com", MyDomainClass)`

---

# Response types
- <verb>ForEntity methods return ResponseEntity
    - `ResponseEntity myEntity = restTemplate.getForEntity("http://myurl.com", Map)`
- <verb>ForObject methods return Object only
    - `Map myResponse = restTemplate.getForObject("http://myurl.com", Map)`

---

# Configurable and Injectable
- Add RestTemplate bean to Autowire
- Add `ClientHttpRequestFactory` when building `RestTemplate` to configure

``` groovy
@Bean
RestTemplate restTemplate(RestTemplateBuilder builder) {
    builder.requestFactory(getClientHttpRequestFactory())
    return builder.build()
}


private static ClientHttpRequestFactory getClientHttpRequestFactory() {
    HttpComponentsClientHttpRequestFactory clientHttpRequestFactory = new HttpComponentsClientHttpRequestFactory()
    clientHttpRequestFactory.setConnectTimeout(200)
    clientHttpRequestFactory.setConnectionRequestTimeout(100)
    clientHttpRequestFactory.setReadTimeout(3000)
    return clientHttpRequestFactory
}
```

---

# Example GET Request

``` groovy
  @Autowired
  RestTemplate restTemplate

  ResponseEntity getArticles(String category, Long time) {
    try {
      restTemplate.getForEntity("https://api.nytimes.com/svc/mostpopular/v2/mostviewed/${category}/${time}.json?api-key=<key>&offset=0", Map)
    }catch (HttpClientErrorException ex){
      return new ResponseEntity([error: ex.message], ex.statusCode)
    }
  }
```

---

# Testing 3rd party dependencies
- Not reliable
    - Could be down
    - May not be accessible from all test environments
    - Can't trigger error states

---

# Mock 3rd party dependencies
- Consistent responses
- Reliably trigger request/response scenarios
    - Bad request
    - Internal Server Error
    - etc...

---

# Http layer is tricky to mock
- Want to mock at lowest level possible
- Spring provides MockRestServiceServer to handle this
    - Replaces RestTemplate at runtime with Mock implementation
    - Setup Mock responses to return during test execution
    - Returned just like actual response

# Example Test

``` groovy
@Autowired
  RestTemplate restTemplate

  @Autowired
  TestRestTemplate testRestTemplate

  MockRestServiceServer mockServer

  def "get articles"() {
    setup:
    mockServer = MockRestServiceServer.createServer(restTemplate)

    mockServer.expect(requestTo(new StringContains("/svc/mostpopular/v2/mostviewed/food/1.json"))).andExpect(method(HttpMethod.GET))
              .andRespond(withSuccess("{}", MediaType.APPLICATION_JSON_UTF8))

    when:
    def responseEntity = testRestTemplate.getForEntity("/articles/food/1", Map)

    then:
    mockServer.verify()


    responseEntity.body == [:]
    responseEntity.statusCode == HttpStatus.OK
  }

```
