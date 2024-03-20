#Brainlife Architecture

## Overview

brainlife.io is composed of a collection of [microservices](https://microservices.io/){target=_blank}; Two major groups of microservices exist; the Warehouse, and Amaretti. 

![diagram](https://docs.google.com/drawings/d/e/2PACX-1vSbxpvxhckYT5rUJReexZdbaL4xZpMDiebDP-yQAxrcy1VwKCAHYQQTWE8mMQ4lBgQg9qpcZcZmaEr1/pub?w=960&amp;h=551)

These services are distributed across multiple docker hosts and various VMs on [Jetstream-Cloud](https://jetstream-cloud.org/){target=_blank}. They mainly communicate through REST APIs and/or AMQP message bus. All external web traffic is proxied through our API proxy (nginx). Our infrastructure also includes monitoring and operational services (not included in the diagram).

### Amaretti

[Amaretti](https://brainlife.github.io/amaretti/){target=_blank} is another major service group that handles the orchestration of tasks(jobs) and workflows on various computing resources. Amaretti is what we call a "meta" orchestration service, meaning it sits on top of various local batch scheduling systems such as PBS, CondorHT, and Slurm to handle the actual job executions on the remote computing resources. Amaretti simply orchestrates and monitors these heterogeneous computing resources and abstracts various common operations requested by our users.

### Authentication

We have developed a simple authentication service that maintains user information and allows them to signup / signin and issue JWT token that can be [statelessly authenticated](https://www.jbspeakr.cc/purpose-jwt-stateless-authentication/){target=_blank} by other services.

### AMQP / Event

[The event service](https://github.com/soichih/event){target=_blank} allows web UI and CLI to subscribe to an event stream via websocket. Various services publish events such as task/instance status or dataset detail updates. Other services can then subscribe to these events with an appropriate access token to receive in real time, or queue these events to be handled by various event handlers.

### Warehouse 

[Warehouse](https://github.com/brainlife/warehouse){target=_blank} provides a bulk of Brainlife-specific functionalities. Brainlife web UI and CLI (command-line interface) primarily interface with warehouse APIs to fulfill most user requests. Warehouse provides functionalities such as organizing datasets under projects, orchestrating pipeline rules, and requesting Amaretti to submit jobs or move datasets between various computing resources and staging and archiving datasets to and from the Warehouse archive. All new datasets are currently, archived on XSEDE Wrangler and optionally copied to IU SDA tape archive system as a backup for published datasets.

Brainlife's web UI is written using [VueJS](https://vuejs.org/){target=_blank}; a popular Javascript frontend framework. Standard post-processing tools such as Webpack and Babel compile our UI code before being delivered to the user's browser. A small amount of server-side rendering is performed periodically to provide scheme.org descriptors for our public assets; such as Apps, Publications, and Projects. This also promotes the discoverability of those assets by various search engines.

#### Modular Processing

Brainlife platform allows App developers to register [datatypes](/docs/user/datatypes/). Each App exchanges data through registered datatypes and this allows App to be written in a modular fashion; each App is only responsible for specific data processing (alignment, tracking, statistical analysis, etc..) which increases the amount of code reuse, and App developer can focus on a truly unique aspect of their App. For example, a statistician writing a statistical method does not have to be concerned with how the input data is preprocessed, or a developer writing a fiber tracking App does not have to write code that analyzes generated fibers which is commonly handled by tract profile analysis Apps. From a user's point of view, module design allows them to more easily compose their workflow using interchangeable Apps that fit their needs. User can also more easily experiment with newer versions or different algorithms only on a part of their workflow.

Allowing users to compose different App increases the chance of incompatibility between Apps. For example, although an App might take anat/t1w input, it might only work properly with *acpc aligned* anat/t1 input. To reduce the risk of incompatibility between the Apps, Brainlife allows each dataset to be tagged by ["datatype tags"](/docs/user/datatypes/#datatype-tags). App developers can then specify certain datatype tags required by the App input. This prevents users from submitting Apps with incompatible input datasets.

Brainlife Apps are designed to process one subject at a time. If a user wants to run an App across multiple subjects, the same App must be submitted as many times as there are subjects. This allows Amaretti to submit Apps as individual jobs on HPC / high-throughput-computing systems, and even across multiple resources if necessary and/or when those resources are available. Eventually, this approach would enable us to submit jobs on truly D-HTC systems for a large "big data" processing. Submitting individual jobs for each subject also allows us to better handle error that occurs on certain subjects without impacting the overall workflows. As the number of processed subjects increases, it is more likely that some subjects will never complete successfully (such as due to data issues).

#### Bulk Processing

As we modularized our data processing, it created issues for managing our data processing. For example, if a user wants to submit a workflow consisting of 10 individual Apps across 200 subjects, it will require 2000(10x200) individual jobs to be submitted and monitored. 

To manage this, we have developed a mechanism, the [pipeline](/docs/user/pipeline/). The pipeline allows ["stream processing"](https://medium.com/@gowthamy/big-data-battle-batch-processing-vs-stream-processing-5d94600d8103){target=_blank} of Brainlife archived datasets and it can be configured by creating a series of "rules" through a simple user interface. Each pipeline rule is responsible for submitting a specific App whenever it detects a new input dataset and corresponding (yet to be generated) output datasets. Brainlife's pipeline rules are evaluated continuously (until deactivated) and can be set up one rule at a time while executing earlier rules. We believe that our method of handling large workflows through stream processing method has several advantages over more conventional workflow management systems such as PegasusWMS, Nipype, and condor DAGMan, where users would imperatively define the entire workflow on various programming languages or in a proprietary syntax and execute (submit) the entire workflow.

The advantages are ...

* More error-resilient and, allows users to handle/recover from issues not anticipated when the workflow is initially conceived. When a job fails, the failed job can be handled individually, or set up another set of rules to process a subset of subjects to work around the problem while other rules are still active.
* Whenever new datasets become available (either uploaded by users, generated by another process, or by other rules), existing rules will automatically detect those new datasets and submit new jobs if necessary. Pipeline rules can be configured once, and left alone for as long as necessary (nothing needs to be "resubmitted" to handle new datasets.)
* Can be configured through GUI (no programming / special syntax is needed). It is easier for novice users as complexity can be *added* rather than having to think through everything before submitting the full workflow.

The disadvantages are ...

* When there are many rules involved it is difficult to understand the entire workflow (a better visualization might help)
* Each rule is applied to all *subjects*. It is difficult/impossible to define rules that operate on multiple subjects simultaneously; like to aggregate outputs from all subjects (we have a different mechanism to accomplish this)
* Could lead to an infinite loop as our system is not *DAG* (hasn't happened yet)

### CLI

[Brainlife CLI](https://github.com/brainlife/cli){target=_blank} is primarily used to upload/download datasets to/from user's computers. CLI also provides some capabilities available through the Web UI such as submitting/monitoring a task and querying various assets available to the user. Brainlife CLI is currently written in node js and distributed through npm; a popular cross-platform software packaging library. Unlike the Web UI, users must manually install npm and the brainlife CLI tool, and keep it up-to-date.

### Visualization

Brainlife provides several ways to visualize stored data objects. The first way is through web-based UI tools such as [BrainBrowsers](https://brainbrowser.cbrain.mcgill.ca/){target=_blank} and [TractView](https://github.com/brainlife/ui-tractview){target=_blank}. These UIs will run on user's web browser and download necessary data-objects via Brainlife's web API. Some of these App requires GPUs on user's machines.

Another way is to launch native UIs such as Freeview, fsleyes, and fibernavigator on Brainlife's GPU-enabled visualization VMs and stream to user's browser using a web-based VNC client.  This approach allows our users to entertain familiar UIs and visualize their data objects without installing these UIs and downloading data objects to be visualized. The only component required by the user is a web browser. 

![viewers](../img/viewers.png)
