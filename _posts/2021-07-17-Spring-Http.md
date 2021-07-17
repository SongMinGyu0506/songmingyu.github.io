---
layout: post
title:  "HttpEntity & Template"
date:   2021-06-27 +0900
categories: Java Spring
---

# 서론
Java Springboot JPA 관련으로 공부중에 테스트 소스코드에 ResponseEntity, restTemplate... 등 처음 보는 클래스들이 등장하였는데 해당 부분을 기억하기 쉽게 기록한다.

# HttpEntity
Spring Framework에서 제공하는 클래스, 하위 클래스로는 ResponseEntity, RequestEntity...등이 있다.  
HttpEntity 클래스는 HttpHeader와 HttpBody를 포함하는 클래스.  
즉, HttpEntity 클래스는 HTTP 프로토콜에서 지정하는 헤더와 몸체부분, 예를 들자면 통신 상태가 어떤지(404, 200 ...) HTTP 프로토콜의 내용을 가지는 객체라고 이해하면 될 것 같다.    
그 상속받는 RequestEntity와 ResponseEntity 클래스 또한 상태, 헤더, 몸체부를 포함한다. 엔티티가 가지는 메소드 또한 ```getHeaders() getBody() getStatusCode()``` 등으로 구성되어 있다.  

# RestTemplate
Spring Framework에서 제공하는 Rest 엔드포인트 호출 방식, HTTP에서 주로 사용하는 CRUD(GET POST PUT DELETE)를 사용하기 쉽도록 만들어짐

## 메소드 설명
|메소드명|파라미터|설명|  
|------|:----:|------|  
|getForObject|String url,Class<T> responseType,Object... uriVariables|객체로 GET 결과 반환|  
|getForEntity|String url,Class<T> responseType,Object... uriVariables|ResponseEntity로 Get 결과 반환|  
|postForLocation|String url,@Nullable Object request,Class<T> responseType,Object... uriVariables|헤더에 저장된 URL을 결과로 반환|  
|postForObject|String url,@Nullable Object request,Class<T> responseType,Object... uriVariables|객체로 POST 요청 결과 반환|  
|postForEntity|String url,@Nullable Object request,Class<T> responseType,Object... uriVariables|ResponseEntity로 POST 요청 결과 반환|  



