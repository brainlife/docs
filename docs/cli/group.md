!!! warning
    This is a draft. Comments are welcomed!

Once you are able to generate large amount of data derivatives on brainlife.io, your next step might involve performing data aggregation for group or statistical analysis. You can download all data derivatives to your computer and conduct any post-processing on your own computer, however, downloading all derivatives might take a long time, and consume a lot of disk space. If you are wanting to quickly explore results of your analysis, this might not be ideal.

Brainlife allows Apps to generate condensed (textual) version of output dataset and store them on brailife.io database. Similar to dataset metadata, you can query and/or download this information without having to download the raw data derivatives. This data is called `product.json`. For example, "Tract Profile Analysis" App (https://brainlife.io/app/59ca5c03c27f5b0770add9cf) generates the following `product.json`.

```json
{
    "profiles": {
        "forcepsMinor": {
            "ad": {
                "profile": [
                    1.3266,
                    1.318,
                    ...
                    1.3188,
                    1.3251
                ],
                "mean": 1.4605,
                "sd": 0.1479
            },
            "fa": {
                "profile": [
                    0.3648,
                    0.3619,
                    ...
                    0.3291,
                    0.3347
                ],
                "mean": 0.4918,
                "sd": 0.1284
            },
            "md": {
                "profile": [
                    0.9368,
                    0.9326,
                    ...
                    0.9588,
                    0.9582
                ],
                "mean": 0.9152,
                "sd": 0.0676
            },
            "rd": {
                "profile": [
                    0.742,
                    0.7399,
                    ...
                    0.7788,
                    0.7747
                ],
                "mean": 0.6426,
                "sd": 0.1203
            },
            "icvf": { ... },
            "isovf": { ...  }
        },
        "forcepsMajor": { ...  },
        "leftTPC": { ...  },
        "leftOccipitoCerebellar": { ...  }
    }
}
```

You can download the content of `product.json` with brainlife.io REST API. 

## Matlab Example

!!! note
    Please run `bl login` before running this example. You will also have to adjust the project / datatype IDs.

```matlab

%define query
find = struct;
find.removed = false;
find.project = '5a74d1e26ed91402ce400cca'; %project id from brainlife.io
find.datatype = '5965467cb09297d8d81bdbcd'; %tractprofile datatype (find in datatypes page)
find.tags = {'wNODDI', '21600'};

%you can filter by subject
find.meta_dot_subject = struct('x_in',["103", "105"]);

%load brainlife access token
jwt = fileread('~/.config/brainlife.io/.jwt'); %use bl login to generate it
options = weboptions()
options.HeaderFields = {'Authorization', ['Bearer ', jwt]};
options.Timeout = 10; %could take a long time to load with a larger product..

%load datasets
disp('Querying brainlife for dataset product...')
disp(find)
url = 'https://brainlife.io/api/warehouse/dataset';

%I have to do some matlab > mongo query conversion
find_json = strrep(jsonencode(find), 'x_', '$');
find_json = strrep(find_json, '_dot_', '.');
data = webread(url, 'find', find_json, 'select', 'product', 'limit', '1000', options);

%TODO ... do things with data.datasets
disp('loaded! - enjoy with data')
disp(data)

 
```
