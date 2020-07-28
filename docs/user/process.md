# Processes

> Hey there! If you have not read the [data processing section](/docs/user/tutorial/#data-processing) of the introductory tutorial yet, please do that first.

Now that we are `Projects` experts, we can cover `Processes`. In any Project you create, you will see a Processes tab (like in the image below). You perform all of your data analysis in the Processes section.

![new processes](/docs/img/new.processes.png)

Each Process is a logical grouping of data analysis and processing tasks that share input and output datasets. After you submit a task, brainlife.io will automatically assign your task to a computing resource you have access to. Our task orchestration service (called [Amaretti](https://github.com/brain-life/amaretti)) takes care of data transfer and monitoring of your tasks for you!

Processing data begins with staging your datset. You can stage a dataset from any project and subject you have read access to (but remember that it may be helpful to create a separate Process for each subject). If you are not sure how to stage data, simply follow our step-by-step [data processing tutorial](https://brainlife.io/docs/user/started/#data-processing). 

## Monitoring Tasks

Once you submit a task, you will notice that brainlife.io keeps you updated on the task is "Running" and when it is "Finished." You can also see the log content by opening the `Raw Output` section of the task and selecting any log files you want to take a closer look at.

![rawoutput](/docs/img/task.raw.output.png)

!!! note
    The `Raw Output` section will not be available for tasks that are not yet assigned to a resource.

If you want to download a file, you can simply click the (:fa-download:) icon to download individual files and entire directories.

## Task Status

Let's quickly break down all of the task statuses you may encounter while processing data:

* Requested

    When you first submit your task, brainlife.io will place it under the `Requested` state as it waits to be assigend a resource to run on. If there are a lot tasks being processed on the platform at the same time, it may take a few minutes for your task to be picked up.

* Running

    Once brainlife.io selects the appropraite resource, your task will be queued up and begin running shortly. You should note that most resources have their own local batch scheduling systems so brainlife.io's status might show the task is `Running` while it waits in the remote queue.

* Finished

    Success! `Finished` obviously means the task was completed successfully. You can visualize output datasets by clicking :fa-eye: buttons next to each output dataset (Hint: This is covered in the [data processing tutorial](https://brainlife.io/docs/user/started/#data-processing) we keep talking about). If you requested to auto-archive the output datasets, datasets will be copied to the Project's `Archive`.

* Failed

    Boo, your task `Failed` -- it will terminate with a non-0 exit code. You can try to examine the output for the cause. If you are really stuck, you can always ask for help in the #general channel on brainlife.io's Slack. You can also contact the App developer or submit a Github issue.

* Removed

    Most resources use a **scratch space** to stage the task's work directory. Normally, scratch spaces have a time limit on how long the data files remain in the system. When brainlife.io detects the task directory no longer exists, it will let you know with a `Removed` tag.

    !!! note
        brainlife.io tries to clean up old task directories within 25 days of the successful completion of the task to provide consistent behavior and reduce disk space across resources. If you have an output dataset you would like to keep, please archive it, or submit the task with the auto-archiving box checked.

        If you archive your output, you will see a list of datasets archived from this output.

        ![status](/docs/img/task.archived.png)


## Submitting Apps

To submit an App, simply click the `Submit App` button in your `Processes` tab under the process. brainlife.io selects Apps that you can execute based on the available datasets within your process and required input datasets for each App.

![submit app](/docs/img/submit app.png)

The more datasets you stage or create, the more Apps you can submit. If you do not find the App you are looking for, head over to the `Apps` page. You can either go back to the `Processes` page and generate or stage the required dataset, or you can execute the App directly from the App page by selecting the `Execute` tab under the App.

![app.execute](/docs/img/app.execute.png)

When you submit an App through the Execute tab, brainlife.io will create a new process under the selected project, stage all of the input files you selected, and submit your App in a single step. 

!!! tip
    If you are looking for a sample dataset, try the [O3D project](https://doi.org/10.25663/bl.p.3) which contains a lot of common data derivatives.
