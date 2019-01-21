#Brainlife Architecture

Brainlife consists of various independent services. Currently, some of these services are grouped into 2 major service groups; Warehouse, and Amaretti. Warehouse provides most of Brainlife specific functionalities and mainly used by Brainlife web UI and CLI (command-line-interface). It provides functionalities such as organizing datasets under project, orchestrating pipeline rules, and moving data between the various data archives and computing resources. Amaretti is another major group of services responsible for "meta"-orchestration of jobs and workflows requested by Warehouse.

![diagram](https://docs.google.com/drawings/d/e/2PACX-1vSbxpvxhckYT5rUJReexZdbaL4xZpMDiebDP-yQAxrcy1VwKCAHYQQTWE8mMQ4lBgQg9qpcZcZmaEr1/pub?w=960&amp;h=551)

These services are distributed across multiple docker hosts and various special purpose VMs. They mainly communicate through REST APIs and/or AMQP message bus and most web traffics are routed through a single nginx instance. Our service architecture also includes monitoring and test infrastructure and various deployment services which are not included in the above diagram.
