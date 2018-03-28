Brainlife platform consists of various microservices which enables various functionalities available to our end-users through our web UI. This document describes some of these services in case you might be interested in knowing how Brainlife works or directly interfacing with these services through its APIs.

![diagram](https://docs.google.com/drawings/d/e/2PACX-1vSbxpvxhckYT5rUJReexZdbaL4xZpMDiebDP-yQAxrcy1VwKCAHYQQTWE8mMQ4lBgQg9qpcZcZmaEr1/pub?w=960&amp;h=551)

# Warehouse

Warehouse is the main application responsible for bulk of Brainlife platform UI.

* [Warehouse API Doc](https://brain-life.github.io/warehouse/apidoc)
* [Warehouse Github](https://github.com/brain-life/warehouse)

# CLI

You can use Brainlife CLI to import / export datasets, among other things. At the moment, we have a very limited CLI support. CLI is simply a wrapper around Brainlife APIs for various services listed in this page. You can use other APIs to perform necessary action if necessary.

* [CLI Github](https://github.com/brain-life/cli)

# Amaretti

`Amaretti` is responsible for submitting, monitoring, and interfacing with apps running on various resources that you have access to. Please see Amaretti technical doc for more information.

* [Amaretti Technical Doc](https://brain-life.github.io/amaretti/)
* [AMaretti Github](https://github.com/brain-life/amaretti)
* [Amaretti API Doc](https://brain-life.github.io/amaretti/apidoc)

# Other APIs

Brainlife also uses the following other services. 


## Authentication Service

* [Auth Github](https://github.com/soichih/auth)
* [Auth API Doc](http://soichi.us/auth/apidoc/)

## Event Service

* [Event Github](https://github.com/soichih/event)
* [Event API Doc](http://soichi.us/event/apidoc/)

## Profile Service

* [Profile Github](https://github.com/soichih/profile)
* [Profile API Doc](http://soichi.us/profile/apidoc/)
