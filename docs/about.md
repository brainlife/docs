<!--- 

Brainlife Style and Conventions

* Brainlife is a platform.
* Brainlife Apps are simply Apps.
* Brainlife Datatypes are simply Datatypes.
* Brainlife Data sets are Datasets
* Brainlife Apps and Datasets together are Research Assets or simply Assets

-->

# What is Brainlife?

A modern platform that uses both cloud and high-performance computing systems to support reproducible analyses, data management and visualization. Brainlife also provide unique mechanisms to publish all research assets associated with a scientific project (data and analyses) embedded in a cloud computing environment and referenced by a single digital-object-identifier (DOI). The platform is unique because of its focus on supporting scientific reproducibility beyond open code and open data, by providing fundamental smart mechanisms for what we refer to as “Open Services.” 

# Brainlife Apps

Brainlife uses Apps to analyze data. **Apps** are small programs, small modules or compute units, that can b made part of a larger series of steps in a full data analysis workflow for a publication. Brainlife Apps are meant to do a small but meaningful step in a longer analysis pipeline. Apps are modules and the platform allows users to develop, use, combine, and reuse Apps to simply build complex pipelines for customized brain data analyses. Most Apps indeed do only one thing, they process data in a specific way and are meant to perform a small set of operations and handle small sets of data; they do one thing, they do it well.

Apps can be developed and published on the Brainlife platform by anyone. App developers can be computational neuroscientists, cognitive neuroscientists, but also computer scientists or engineers. Apps are snippets of code implementing algorithms or analyses. By follow a few easy steps code to publish the code on the Brainlife platform as an App. Publishing code as an App allows scientists to use the data and computational resources available through Brainlife. Apps published on Brainlife can be used privately or shared publicly with the platform users community. 

# Brainlife Datatypes

Brainlife Apps communicate via Brainlife “Datatypes.” A Datatype defines expected list of file names / directory structure that Apps that uses that datatype to generate as output data. It can also be used as input dataset by other App that expects the same datatype. Datatypes allow multiple apps developed by independent developers to be joined together to form a *workflows*. The Brainlife Datatypes are similar to BIDS in concept, except they are maintained by individual developers participating in a specific datatype, and they mainly concerns *data derivatives*. Brainlife allows user to download datasets in BIDS structure.

# Brainlife Clouds (Compute Resource)

Brainlife allows orchestration of data and computing across mix systems of Clouds and high-performance computing clusters (HPC). We refer to both HCP and Clouds as Brainlife Clouds, technically not precise terminology, but simple. Less is more. Brainlife orchestration allows users and compute resource providers to register a compute resource to make it available publicly to the full Brainlife users community or privately to a subset of users. Brainlife has smart mechanisms that allow Apps to run on different resources, privately or publicly. With a traditional approach of running an entire workflow on a small set of resources, some part of the workflow might not be optimal to run on a given resource, Brainlife allows App Developers to identify resources that best fit their Apps and score the compute resources available on the Brainlife platform depending on how well they work with the App they develop.

# Brainlife Viz (Cloud Visualization)

Brainlife has mechanisms to run brain data visualization on Brainlife Clouds. Data visualization is meant to provide users with an agile way to get feedback on the quality of the results generatd by Apps and pipelines. Visualization is impleelemnted with smart cloud-side methods, so that data are not moved from the Clould to the users computer. THis increases seecurity and improves management. Brainlife Viz allows running major software for data visualization familiar to the neuroscience community (e.g., FreeView, FSLview, MRview), and running GPU rendered visualizations. Visualization Apps can be openly contributed by developers tot hee Brainlife platform. 





