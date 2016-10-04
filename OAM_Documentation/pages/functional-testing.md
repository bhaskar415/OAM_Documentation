---
layout: default
title: Functional Testing
permalink: /functional-testing/
sitemap:
    priority: 0.7
    lastmod: 2016-03-10T00:00:00-00:00
---

# <i class="fa fa-sitemap"></i> Functional Testing

## Summary

1. [SSO Features](#sso_features)
2. [Websso](#websso_features)
3. [B2B Websso](#b2b_websso)
4. [Webaccess](#webaccess)
5. [Integrated Applications](#application_Integrated)


## <a name="sso_features"></a> SSO Features

The first question JHipster will ask you is the kind of application you want to generate. You have in fact the choice between two architecture styles:

- A "monolithic" architecture uses a single, one-size-fits-all application, which contains both the front-end AngularJS code, and the back-end Spring Boot code.
- A "microservices" architecture splits the front-end and the back-end, so that it's easier for your application to scale and survive infrastructure issues.

A "monolithic" application is much easier to work on, so if you don't have any specific requirements, this is the option we recommend, and our default option.

_The rest of this guide is only for people interested in doing a microservices architecture._

## <a name="websso_features"></a> Websso

The JHipster microservices architecture works in the following way:

 * A [gateway](#gateway) is a JHipster-generated application (using application type `microservice gateway` when you generate it) that handles Web traffic, and serves an AngularJS application. There can be several different gateways, if you want to follow the [Backends for Frontends pattern](https://www.thoughtworks.com/insights/blog/bff-soundcloud), but that's not mandatory.
 * The [JHipster Registry](#registry) is a runtime application, using the usual JHipster structure, on which all applications registers and get their configuration from.
 * Microservices are JHipster-generated applications (using application type `microservice application` when you generate them), that handle REST requests. They are stateless, and several instances of them can be launched in parallel to handle heavy loads.
 * The [JHipster Console](https://github.com/jhipster/jhipster-console) is a monitoring & alerting console, based on the ELK stack.

In the diagram below, the green components are specific to your application and the blue components provide its underlying infrastructure.

![Diagram]({{ site.url }}/images/microservices_architecture_1.png)

## <a name="b2b_websso"></a> B2B Websso

JHipster allows use to generate an API gateway. This gateway is a normal JHipster application, so you can use the usual JHipster options and development workflows on that project, but it also acts as the entrance to your microservices. More specifically, it provides HTTP routing and load balancing, quality of service, security and API documentation for all microservices.

### <a name="webaccess"></a> Webaccess

When the gateways and the microservices are launched, they will register themselves in the registry (using the `eureka.client.serviceUrl.defaultZone` key in the `src/main/resources/config/application.yml` file).

The gateway will automatically proxy all requests to the microservices, using their application name: for example, when microservices `app1` is registered, it is available on the gateway on the `/app1` URL.

For example, if your gateway is running on `localhost:8080`, you could point to [http://localhost:8080/app1/rest/foos](http://localhost:8080/app1/rest/foos) to
get the `foos` resource served by microservice `app1`. If you're trying to do this with your Web browser, don't forget REST resources are secured by default in JHipster, so you need to send the correct JWT header (see the point on security below), or remove the security on those URLs in the microservice's `MicroserviceSecurityConfiguration` class.

If there are several instances of the same service running, the gateway will get those instances from the JHipster Registry, and will:

- Load balance HTTP requests using [Netflix Ribbon](https://github.com/Netflix/ribbon).
- Provide a circuit breaker using [Netflix Hystrix](https://github.com/Netflix/hystrix), so that failed instances are quickly and safely removed.

Each gateway as a specific "admin > gateway" menu, where opened HTTP routes and microservices instances can be monitored.

### <a name="application_Integrated"></a> Integrated Applications
