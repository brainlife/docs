#Brainlife Architecture

## Overview

Brainlife is constructed as a collection of independent [microservices](https://microservices.io/). Currently, there are two major groups for these services; Warehouse, and Amaretti. 

![diagram](https://docs.google.com/drawings/d/e/2PACX-1vSbxpvxhckYT5rUJReexZdbaL4xZpMDiebDP-yQAxrcy1VwKCAHYQQTWE8mMQ4lBgQg9qpcZcZmaEr1/pub?w=960&amp;h=551)

These services are distributed across multiple docker hosts and various VMs on [Jetstream-Cloud](https://jetstream-cloud.org/). They mainly communicate through REST APIs and/or AMQP message bus. All external web traffics are proxied through our API proxy (nginx). Our infrastructure also includes monitoring and operational services which are not included in the above diagram.

### Warehouse

Warehouse provides a bulk of Brainlife specific functionalities. Brainlife web UI and CLI (command-line-interface) primarily interface with these services through the REST API. Warehouse provides functionalities such as organizing datasets under project, orchestrating pipeline rules, and requesting Amaretti to move datasets between various computing resources and the Warehouse archive. All new datasets are currently archived on XSEDE Wrangler and optionally copied to IU SDA tape archive system as a backup.

### Amaretti

Amaretti is another major service group that handles orchestration of tasks(jobs) and workflows on various computing resources. It is primary used by Warehouse to fulfill various user requests. Amaretti is what we call "meta" orchestration service, meaning it simply interfaces with local batch scheduling systems such as PBS, CondorHT, and slurm to actually handle job executions on the remove computing resources.

### Authentication

Our authentication service allows user to signup / signin and issue JWT token that can be [statelessly authenticated](https://www.jbspeakr.cc/purpose-jwt-stateless-authentication/) by other services.

### Event / AMQP

The event service allows web UI and CLI to subscribe to an event stream via websocket. Various services publish events such as task/instance status or dataset detail updates. Other services can then subscribe to these events with an appropriate access token to receive in realtime, or queue these events to be handled by various event handlers.

### Web UI

Brainlife web UI is written using [VueJS](https://vuejs.org/); a popular Javascript frontend framework. Standard post-processing tools such as webpack and babel are used to compile our UI code before delivered to the user's browser. A small amount of server-side rendering is performed periodically to provide scheme.org descriptors for our public assets; such as Apps, Publications, Projects. This also promotes discoverability of those assets by various search engines.

### CLI

[Brainlife CLI](https://github.com/brain-life/cli) is primarily used to upload/download datasets to/from user's computers. CLI also provides some capabilities available through the Web UI such as submitting/monitoring a task, and querying various assets that user has access to. Brainlife CLI is currently written in nodejs and distributed through npm; a popular cross-platform software packaging library. Unlike the Web UI, user must manually install npm anb brainlife CLI tool, and keep it up-to-date.

### Visualization

Brainlife provides 2 ways to visualize datasets stored. The first way is through web based UI tools such as [BrainBrowsers](https://brainbrowser.cbrain.mcgill.ca/) and [TractView](https://github.com/brain-life/ui-tractview). These UIs will run on user's web browser and download necessary dataset via Brainlife's web API. Some of these App requires GPUs on user's machines.

Another way is to launch native UIs such as Freeview, fsleyes, and fibernavigator on Brainlife's GPU-enabled visualization VMs and streamed to user's browser using web-based VNC client.  This approach allows our users to entertain familar UIs and visualize their dataset without having to install these UIs and downloading datasets to be visualized. The only component required by the user is a web browser. 

![viewers](/docs/img/viewers.png)