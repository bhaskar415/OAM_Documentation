---
layout: default
title: High Availability
permalink: /high-availability/
sitemap:
    priority: 0.7
    lastmod: 2016-03-10T00:00:00-00:00
---

# <i class="fa fa-sitemap"></i> High Availability

## Summary

1. [Multi Datacenter Failover](#mdc_testing)
2. [APS Sync](#aps_sync)


## <a name="mdc_testing"></a> Multi Datacenter Failover

The first question JHipster will ask you is the kind of application you want to generate. You have in fact the choice between two architecture styles:

- A "monolithic" architecture uses a single, one-size-fits-all application, which contains both the front-end AngularJS code, and the back-end Spring Boot code.
- A "microservices" architecture splits the front-end and the back-end, so that it's easier for your application to scale and survive infrastructure issues.

A "monolithic" application is much easier to work on, so if you don't have any specific requirements, this is the option we recommend, and our default option.

_The rest of this guide is only for people interested in doing a microservices architecture._

## <a name="aps_sync"></a> APS Sync

The JHipster microservices architecture works in the following way:

 * A [gateway](#gateway) is a JHipster-generated application (using application type `microservice gateway` when you generate it) that handles Web traffic, and serves an AngularJS application. There can be several different gateways, if you want to follow the [Backends for Frontends pattern](https://www.thoughtworks.com/insights/blog/bff-soundcloud), but that's not mandatory.
 * The [JHipster Registry](#registry) is a runtime application, using the usual JHipster structure, on which all applications registers and get their configuration from.
 * Microservices are JHipster-generated applications (using application type `microservice application` when you generate them), that handle REST requests. They are stateless, and several instances of them can be launched in parallel to handle heavy loads.
 * The [JHipster Console](https://github.com/jhipster/jhipster-console) is a monitoring & alerting console, based on the ELK stack.

In the diagram below, the green components are specific to your application and the blue components provide its underlying infrastructure.

![Diagram]({{ site.url }}/images/microservices_architecture_1.png)
