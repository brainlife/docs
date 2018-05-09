# product.json standard

Your app can generate a file named `product.json` to send information back to Brainlife UI. You can think of it as the opposite of `config.json`. `product.json` will be picked up by Brainlife once your App finishes successfully and loaded on to Brainlife's internal database (mongodb).

`product.json` can be used to do the following.

1. Display messages, statuses, graphs(plotly) on Brainlife UI.
2. Store small amount of data that you can later be used to aggregate across multiple output datasets without having to stage all output datasets.

product.json should be very small (like a few kilobytes). Larger data should be stored as a regular output data, and product.json can contain reference (paths) to that datafile. Any data referenced needs to be staged out of archive for user to actually access them. Having data on product.json allows quick (~10msec) access it is stored in mongoDB. To aggregate data from more than dozen dataset, the data should be pre-aggregated and stored in product.json (raw data can be stored as regular output dataset).

## Aggregation Visualizer / App 

When a App runs, it currently stages all input tasks which could be giga-bytes each. This is not sustainable for Apps that simply wants to aggregate basic information from multiple datasets. By storing all necessary information in product.json, a developer can create a Javascript based visualizer that can simply query and consume information to visualize. User can then dynamically update query and play around with the data.   

For developer who doesn't want to code in Javascript, we could provide more standard "App" way of accomplishing the same thing by staging product.json from each input dataset (rather than the actual dataset) and let it do the aggregation and generate some static images / graphs (or product.json / visjs!) 

Deverloper can also use Brainlife API to query for datasets and its product.json and run it via command line. Such App can live completely outside Brainlife UI.


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

### plotly graphs

```json
{
	"brainlife": [
        {
		"type": "plotly",
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

### Messages (error / warnings)

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

### Others

Any unstructured data can be just stored anywhere inside the product.json as usual.

```json
{
    "rmse": 123,
}
```


