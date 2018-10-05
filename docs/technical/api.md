# Brainlife API

This document describes some of Brainlife's microservices in case you might be interested in directly interfacing with them through APIs.

## Warehouse

Warehouse is the main application responsible for bulk of Brainlife platform UI.

* [Warehouse Github](https://github.com/brain-life/warehouse)
* [Warehouse API Doc](https://brain-life.github.io/warehouse/apidoc)

!!! note
    Brainlife CLI interacts with Warehouse API to import / export datasets, query task status, and among other things. At the moment, we have a very limited CLI support, but please try using our CLI tool if you just want to interface with Brainlife Warehouse via commandline [CLI Github](https://github.com/brain-life/cli)

## Amaretti

`Amaretti` is responsible for submitting, monitoring, and interfacing with apps running on various resources that you have access to. Please see Amaretti technical doc for more information.

* [Amaretti Doc](https://brain-life.github.io/amaretti/)
* [Amaretti Github](https://github.com/brain-life/amaretti)
* [Amaretti API Doc](https://brain-life.github.io/amaretti/apidoc)

## Authentication Service

* [Auth Github](https://github.com/soichih/auth)
* [Auth API Doc](http://soichi.us/auth/apidoc/)

## Event Service

* [Event Github](https://github.com/soichih/event)
* [Event API Doc](http://soichi.us/event/apidoc/)

## Profile Service

* [Profile Github](https://github.com/soichih/profile)
* [Profile API Doc](http://soichi.us/profile/apidoc/)
