# `product.json`

Your App can optionally generate a file named `product.json` to send information back to Brainlife. You can think of it as the counterpart for `config.json`. As `config.json` is used to pass configuration parameters from the user to an App, `product.json` can be used to send information from your App back to Brainlife upon successfully completion. The information stored in  `product.json` can be used relay information back to the App submitter, or used to perform data aggregation quickly across multiple subjects.

!!! warning
    You should not store more than a few kilobytes of information on `product.json`.

You can use `product.json` for the following purposes.

1. Display messages, statuses, graphs(plotly) on Brainlife UI.
2. Store small amount of unstructured data that you can later use to quickly aggregate across multiple output datasets.
3. Specify user tags (or datatype_tags) to be added to the output dataset.

We will explain each use cases below.

## 1. Displaying messages, graphs on Brainlife UI

<!--

### Images

```json
{
	"brainlife": [
		{
			"type": "image",
			"label": "xyz-image",
			"path": "./images/myimage.png"
		}
    ]
}
```
-->

Although `product.json` can store any data, we've defined a special key(`"brainlife"`) that you could use to render graphical information back to Brainlife UI.

### Messages (error / warnings)

You can display error / warning message on Brainlife process UI by storing them in following format.

```json
{
	"brainlife": [
		{"type": "error", "msg": "Some tracts have less than 20 streamlines. Check quality of data!"},
       ]
}
```

Above message will be displayed under the Output section of the process UI as well as dataset detail page, like the following.

![messages](/docs/img/product.messages.png)

You can also display following message types.

```json
{
	"brainlife": [
		{"type": "error", "msg": "here is my error message"},
		{"type": "danger", "msg": "here is my error message"},
		{"type": "info", "msg": "here is my info message"},
		{"type": "warning", "msg": "here is my warning message"},
		{"type": "success", "msg": "here is my warning message"},
       ]
}
```

### Graphs (plotly)

You can also display plotly graph.


```json
{
	"brainlife": [
        {
		"type": "plotly",
		"name": "my plotly test",
		"data": [ 
			{"x": '2014-06-11', "y": 10}, 
			{"x": '2014-06-12', "y": 25}, 
			{"x": '2014-06-13', "y": 30},
			{"x": '2014-06-14', "y": 10}, 
			{"x": '2014-06-15', "y": 15}, 
			{"x": '2014-06-16', "y": 30} 
		],
		"layout": { start: '2014-06-10', end: '2014-06-18' },
		}	
	]
}
```

You can embed images by first base64 encoding the image and storing it on product.json


```json
{
    "brainlife": [
        { 
            "type": "image/png", 
            "name": "My image title",
            "base64": "............base64 encoded png............"
        }
    ]
}
```

## 2. Storing unstructured data

Any unstructured data can be just stored anywhere inside the `product.json`. 

Like..

```json
{
    "rmse": 123,
    "labels: ["apple", "orange"]
}
```

You might have App that could produce multi-gigabytes of output datasets. If you need to run aggregation process across many subjects using such outputs, you might end up needing to stage hundreds of gigabytes of output datasets which may or may not be possible, or desirable to do so. Instead, you could produce pre-aggregated data and store it inside `product.json`. You can then query all datasets and download `product.json` contents from each dataset without having to download the actual output datasets. Please take a look at [Brainlife CLI](https://github.com/brain-life/cli) for more information on querying datasets.

<!--
By storing all pre-aggregated information in `product.json`, you can create a Javascript based visualizer that can simply query and consume information to visualize. User can then dynamically update query and play around with the data.   

For developer who doesn't want to code in Javascript, we could provide more standard "App" way of accomplishing the same thing by staging product.json from each input dataset (rather than the actual dataset) and let it do the aggregation and generate some static images / graphs (or product.json / visjs!) 

Deverloper can also use Brainlife API to query for datasets and its product.json and run it via command line. Such App can live completely outside Brainlife UI.
-->


## 3. Specifying dataset (user) tags

Your App can set "tags" key to specify dataset tags that you'd like to add to the dataset output. 

```json
{
    "tags": [ "tags_from_app" ]
}
```

!!! note
    If you have multiple output datasets, specified tags will be applied to all output datasets. If you'd like to specify different tags
    for each output dataset, please read the section below ("Multiple output datsets")

You can also set(/override) dataset metadata

```json
{
    "meta": {
        "subject": "12345",
        "SliceTiming": [0, 1, 2, 3]
    }
}
```

Although not recommended, you can also override datatype tag of the output dataset.

```json
{
    "datatype_tags": [ "acpc_aligned" ],
}
```

!!! warning 
    Brainlife UI relies on each App to have predetermined datatype and datatype tags. Overriding datatype tags could cause your workflow
    to be inconsistent. You should also make sure that any datatype tags you are using is valid and [registered](https://brainlife.io/datatypes) for your output datatype.

## Multiple output datasets

Most Apps output more than 1 datasets. By default, all information stored on `product.json` will be copied to all output datasets. If you'd like to store different information for each output dataset, you can do so by organizing your `product.json` with App's registerd output IDs as keys on the root of `product.json`

For example, let's assume you have an App with 2 output datasets; t1, and mask, and you have used "t1" and "mask" as output IDs when you registered your App.

You could then output something like the following `product.json`

```
{
    "t1": {
        "val1": 123,
        "val2": 456
    },
    "mask": {
        "val1": 567,
        "val2": 890
    }
}

```

When this `product.json` is imported, brainlife will pick "t1" as the root of product.json for t1 output, and "mask" as mask output. You can still store items on the root of the `product.json` which will be merged on to output specific data. This is useful if you have information that is common across all outputs, but wants to be able to specify output specific information also.
