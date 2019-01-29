#Brainlife Architecture

## Overview

Brainlife consists of various independent services. Currently, some of these services are grouped into two major service groups; Warehouse, and Amaretti. Warehouse provides most of Brainlife specific functionalities and mainly used by Brainlife web UI and CLI (command-line-interface). It provides functionalities such as organizing datasets under project, orchestrating pipeline rules, and moving data between the various data archives and computing resources. Amaretti is another major group of services responsible for "meta"-orchestration of jobs and workflows requested by Warehouse.

![diagram](https://docs.google.com/drawings/d/e/2PACX-1vSbxpvxhckYT5rUJReexZdbaL4xZpMDiebDP-yQAxrcy1VwKCAHYQQTWE8mMQ4lBgQg9qpcZcZmaEr1/pub?w=960&amp;h=551)

There are other smaller services such as authentication, event, profile, and various database services that are used by both Warehouse and Amaretti. For example, the authentication service allows user to signup / signin and issue JWT token which can be statelessly verified by other services. The event service allows web UI and CLI to subscribe to a time event stream that users are authorized to receive through a standard websocket.

These services are distributed across multiple docker hosts and various special purpose VMs. They mainly communicate through REST APIs and/or AMQP message bus and most web traffics are proxied through a single nginx instance in order to minimize IP address and ssh renegotiations. Our service architecture also includes monitoring and test infrastructure and various deployment services which are not included in the above diagram.

## UI

Brainlife web UI is written using VueJS; a popular single page front end framework. We use webpack and other standard post-processing tools to "compile" our UI code and deliver them to the user's browser through our nginx interface. To provide scheme.org descriptors and promote discoverability of our public assets (such as App, Publications, Projects, etc), we do some server side rendering to deliver necessary metadata to various search engines.


