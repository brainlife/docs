---
title: "Using Brainlife with Amazon Web Services"
---

# Using Brainlife with Amazon Web Services

## Introduction

This tutorial guides you through accessing and using Brainlife datasets with various AWS services. Brainlife datasets are hosted on Amazon S3 and can be seamlessly integrated with SageMaker, Lambda, EC2, and Batch for neuroimaging analysis and processing.

## Accessing Brainlife Datasets

Brainlife datasets are stored in an S3 bucket at `s3://brainlife/`. Each dataset includes a `config.json` file containing essential metadata.

To list available datasets using the AWS CLI:

```
aws s3 ls s3://brainlife/
```

## Using Datasets with AWS Services

### SageMaker

In a SageMaker notebook, you can import Brainlife datasets using:

!!! info
    Replace `project_id` and `dataset_id` with Brainlife-specific identifiers

```
!aws s3 cp s3://brainlife/project_id/dataset_id/ . --recursive
```

### Lambda

For serverless applications, reference Brainlife datasets in your Lambda function by specifying the S3 path and using the AWS SDK to access the data.

### EC2 & Batch

For compute-intensive tasks, you can use EC2 instances or AWS Batch with Brainlife datasets. Access the datasets directly from S3 using the AWS SDK or CLI commands in your processing scripts. For neuroimaging workflows, you can either develop custom applications or leverage Brainlife's pre-built pipelines that are compatible with cloud-based execution environments.

## OME-Zarr on S3

OME-Zarr is a format for storing bioimaging data, often used in microscopy workflows. If you’re storing an OME-Zarr dataset in S3 (including Brainlife’s S3 buckets), you’ll find a directory structure containing various `.z*` files that specify metadata, multiscale images, and associated attributes.

Here’s an example of what an OME-Zarr layout might look like:

```
OME-Zarr/
├── .zattrs               # Global attributes (JSON)
├── .zgroup               # Group metadata (JSON)
├── 0                     # Multiscale level 0 (highest resolution)
│   ├── .zarray           # Zarr array metadata (JSON)
│   ├── 0                 # Chunk file(s) containing raw data
│   ├── 1
│   └── 2
└── 1                     # Multiscale level 1
    ├── .zarray
    ├── 0
    ├── 1
    └── 2
```

- **`.zattrs`** and **`.zgroup`** store high-level metadata about the dataset.
- The numbered directories (`0`, `1`, etc.) represent different resolutions or scales of the image data.
- Each `.zarray` contains metadata for that specific resolution level.
- The numbered files within each resolution directory (for example, `0`, `1`, `2`) contain the actual chunked pixel data.

When working with OME-Zarr data on S3, you can use tools such as [zarr-python](https://zarr.readthedocs.io/en/stable/) or [ome-zarr-py](https://github.com/ome/ome-zarr-py) to read and write image data directly from the bucket.
