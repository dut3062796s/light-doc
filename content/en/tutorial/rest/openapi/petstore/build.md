---
title: "Build and Start"
date: 2017-11-08T10:55:22-05:00
description: ""
categories: []
keywords: []
slug: ""
weight: 50
aliases: []
toc: false
draft: false
---

In the previous step, we've generated a project into light-example-4j/rest/openapi/petstore 
folder and it is ready to be built and started. The default build tool generated is Maven and
we are planning to support Gradle soon so that users can choose their favorite tool from
config.json for the light-codegen


To build and start the generated server, use the following command:

```
cd ~/networknt/light-example-4j/rest/openapi/petstore
mvn install exec:exec
```

Now the server is started and listens to port 8443 on HTTPS with HTTP 2.0 as default. 

Services built on top of the light platform can consist of numerous middleware handlers that are
responsible for all cross-cutting concerns. It is a [plugin architecture][] and each plugin
has its own configuration file. 

The following is the default server.yml to control how the server is started. 

```yaml
# Server configuration
---
# This is the default binding address if the service is dockerized.
ip: 0.0.0.0

# Http port if enableHttp is true. It will be ignored if dynamicPort is true.
httpPort:  8080

# Enable HTTP should be false by default. It should be only used for testing with clients or tools
# that don't support https or very hard to import the certificate. Otherwise, https should be used.
# When enableHttp, you must set enableHttps to false, otherwise, this flag will be ignored. There is
# only one protocol will be used for the server at anytime. If both http and https are true, only
# https listener will be created and the server will bind to https port only.
enableHttp: false

# Https port if enableHttps is true. It will be ignored if dynamicPort is true.
httpsPort:  8443

# Enable HTTPS should be true on official environment and most dev environments.
enableHttps: true

# Http/2 is enabled by default for better performance and it works with the client module
enableHttp2: true

# Keystore file name in config folder. KeystorePass is in secret.yml to access it.
keystoreName: tls/server.keystore

# Flag that indicate if two way TLS is enabled. Not recommended in docker container.
enableTwoWayTls: false

# Truststore file name in config folder. TruststorePass is in secret.yml to access it.
truststoreName: tls/server.truststore

# Unique service identifier. Used in service registration and discovery etc.
serviceId: com.networknt.petstore-1.0.1

# Flag to enable self service registration. This should be turned on on official test and production. And
# dyanmicPort should be enabled if any orchestration tool is used like Kubernetes.
enableRegistry: false

# Dynamic port is used in situation that multiple services will be deployed on the same host and normally
# you will have enableRegistry set to true so that other services can find the dynamic port service. When
# deployed to Kubernetes cluster, the Pod must be annotated as hostNetwork: true
dynamicPort: false

# Minimum port range. This define a range for the dynamic allocated ports so that it is easier to setup
# firewall rule to enable this range. Default 2400 to 2500 block has 100 port numbers and should be
# enough for most cases unless you are using a big bare metal box as Kubernetes node that can run 1000s pods
minPort: 2400

# Maximum port rang. The range can be customized to adopt your network security policy and can be increased or
# reduced to ease firewall rules.
maxPort: 2500

# environment tag that will be registered on consul to support multiple instances per env for testing.
# https://github.com/networknt/light-doc/blob/master/docs/content/design/env-segregation.md
# This tag should only be set for testing env, not production. The production certification process will enforce it.
# environment: test1
```

Now the server is up and running, let's [test][] it to ensure it is working. 
 
[plugin architecture]: /architecture/plugin/
[test]: /tutorial/rest/openapi/petstore/test/