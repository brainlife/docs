# Brainlife API

This document describes some of Brainlife's microservices in case you might be interested in directly interfacing with them through APIs.

## Warehouse

Warehouse is the main application responsible for bulk of Brainlife platform UI.

* [Warehouse Github](https://github.com/brainlife/warehouse)
* [Warehouse API Doc](https://brainlife.github.io/warehouse/apidoc)

!!! note
    Brainlife CLI interacts with Warehouse API to import / export datasets, query task status, and among other things. At the moment, we have a very limited CLI support, but please try using our CLI tool if you just want to interface with Brainlife Warehouse via commandline [CLI Github](https://github.com/brainlife/cli)

## Amaretti

`Amaretti` is responsible for submitting, monitoring, and interfacing with apps running on various resources that you have access to. Please see Amaretti technical doc for more information.

* [Amaretti Doc](https://brainlife.github.io/amaretti/)
* [Amaretti Github](https://github.com/brainlife/amaretti)
* [Amaretti API Doc](https://brainlife.github.io/amaretti/apidoc)

## Authentication/Profile Service

Our authentication service is hosted under a private github repo. Please contact us if you'd like to access
this information.

* [Auth Github](https://github.com/soichih/auth) 

## Event Service

Event service streams information from brainlife event bus to various subscribers (UI, backend service, etc..). It uses authentication service authorization token to provide user access control.

* [Event Github](https://github.com/soichih/event)
* [Event API Doc](http://soichi.us/event/apidoc/)

## Others

Brainlife consists of various other microservices. They are not meant to be interfaced directly via 3rd party application, so please contact us to find more more about it.

