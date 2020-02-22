---
title: "Spring Cloud Netflix"
date: 2020-01-22T14:00:00+09:00
lastmod: 2020-01-22T14:00:00+09:00
draft: true
show_in_homepage: true
license: ''

tags: ['Spring', 'MSA', 'DevOps']
categories: ['DevOps', 'MSA', 'Spring']

featured_image: ''
featured_image_preview: ''

comment: true
toc: true
autoCollapseToc: true
math: true
---

Pattern
- Eureka: Service discovery
    - 유레카 인스턴스가 등록된다
    - 클라이언츠는 스프링 Beans를 사용해 인스턴스를 발견한다
    - 선언적 자바 구성으로 임베디드 유레카 서버는 생성된다.
    - [SAMPLE](https://github.com/spring-cloud-samples/eureka)

- Hystrix: Circuit Breaker
    - Hystrix 클라이언트는 간단한 어노테이션으로 구축된다.
    - 선언적 자바 구성이 포함된 내장 Hystrix 대시보드

- Feign: Declarative REST Client
    - Feign은 JAX-RS 또는 Spring MVC 주석으로 장식 된 인터페이스의 동적 구현을 ​​만듭니다.

- Ribbon: Client Side Load Balancer

- Archaius: External Configuration
    - Spring Boot 규칙을 사용하여 Netflix 구성 요소의 기본 구성 사용 가능

- Zuul: Router and Filter
    - Zuul 필터의 자동 등록 및 프록시 생성을 되 돌리는 구성 방법에 대한 간단한 규칙
    - Filter
        1. PRE Filter - 라우팅전에 실행되며 필터이다. 주로 logging, 인증등이 pre Filter에서 이루어 진다.
        2. ROUTING Filter - 요청에 대한 라우팅을 다루는 필터이다. Apache httpclient를 사용하여 정해진 Url로 보낼수 있고, Neflix Ribbon을 사용하여 동적으로 라우팅 할 수도 있다.
        3. POST Filter - 라우팅 후에 실행되는 필터이다. response에 HTTP header를 추가하거나, response에 대한 응답속도, Status Code, 등 응답에 대한 statistics and metrics을 수집한다.
        4. ERROR Filter - 에러 발생시 실행되는 필터이다.


