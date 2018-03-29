
# What is Brainlife?

TODO.. Describe mission, what it does not, where we want to go, etc..

# Apps

![dataset](/img/apps.png)

On Brainlife, **apps* are normally small programs that perform specific set of data processing. Although we have a few apps that behaves more like a typical *pipeline* or *workflow* (including pre/post processing, data analysis, reporting, etc..), most Brainlife apps should only do one thing, and one thing well. By joining these small apps together, user can create a *workflow* with a greater flexibility, configurability, and reproducibility. 

From app developer's point of view, this approach allows them to focus on the most idiosyncratic aspect of the algorithm that they are trying to publish. They do not need to worry about how to preprocess the data, or how the generated data is processed afterward. Each app can be written in language or with libraries that developers are most familiar with, by interfacing with other apps through the use of Brainlife `datatypes`.

From computational point of view, each app can be easily parallelized, run on resources that are available at runtime, or on resources that the particular app is most efficient to run on. With a traditional approach of running an entire workflow on a small set of resources, some part of the workflow might not be optimal to run on a given resource. 
 
# Dataset Archive

TODO.. Explain what datasets are, who can access them, etc.. ()

# Data Processing

TODO.. Explain how Brainlife process data, etc..



