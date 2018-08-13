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

Brainlife uses Apps to analyze data. **Apps** are small programs, small modules or compute units, that can process data individually or be made part of a larger series of steps in a full data analysis workflow. Brainlife Apps are meant to do small but meaningful steps in a longer analysis pipeline. Apps are modules and the brainlife.io platform. The platform allows users to develop, use, combine, and reuse Apps to build complex pipelines for customized brain data analyses. Most Apps indeed do only one thing, they process data in a specific way and are meant to perform a small set of operations and handle small sets of data; they do one thing, they do it well.

Apps can be developed and published on the Brainlife platform by anyone. App developers can be computational neuroscientists, cognitive neuroscientists, but also computer scientists or engineers. Apps are snippets of code implementing algorithms or analyses. By following a few easy steps developers can publish their code for brain data analysis on the Brainlife platform as an App. Publishing code as Apps allows scientists to use the code in combination with the data and computational resources available through Brainlife. Apps published on Brainlife can be used privately or shared publicly with the platform users community.

# Brainlife Datatypes

Brainlife Apps communicate via Brainlife “Datatypes.” A Datatype defines the expected list of filenames or the directory structure that an Apps can use as input or generate as output. The same Datatype is ideally used by multiple Apps, this allows Apps to communicate by their input and output data sets and reuse the data to generate more data derivatives, useful for other Apps. Datatypes, in addition to allowing the various Apps developed by independent developers to communicate on the brainlife.io platform, also allow Apps to be joined together to form a pipeline or workflows. The Brainlife Datatypes are maintained by individual developers participating in a specific Datatype, and discussed and maintained at https://github.com/brain-life/datatypes, conveniently using the full versioning and management features of github.com.

# Brainlife Clouds (Compute Resource)

Brainlife allows orchestration of data and computing across mix systems of Clouds and high-performance computing clusters (HPC). We refer to both HCP and Clouds as Brainlife Clouds, technically not precise terminology, but simple. Less is more. Brainlife orchestration allows users and compute resource providers to register a compute resource to make it available publicly to the full Brainlife users community or privately to a subset of users. Brainlife has smart mechanisms that allow Apps to run on different resources, privately or publicly. With a traditional approach of running an entire workflow on a small set of resources, some part of the workflow might not be optimal to run on a given resource, Brainlife allows App Developers to identify resources that best fit their Apps and score the compute resources available on the Brainlife platform depending on how well they work with the App they develop.

# Brainlife Viz (Cloud Visualization)

Brainlife has mechanisms to run brain data visualization on Brainlife Clouds. Data visualization is meant to provide users with an agile way to get feedback on the quality of the results generatd by Apps and pipelines. Visualization is impleelemnted with smart cloud-side methods, so that data are not moved from the Clould to the users computer. THis increases seecurity and improves management. Brainlife Viz allows running major software for data visualization familiar to the neuroscience community (e.g., FreeView, FSLview, MRview), and running GPU rendered visualizations. Visualization Apps can be openly contributed by developers tot hee Brainlife platform. 





