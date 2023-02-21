---
title: "AWS ParallelCluster UI"
date: 2019-01-24T09:05:54Z
weight: 1
---

![AWS ParallelCluster UI Logo](parallelcluster-ui.svg)

[AWS ParallelCluster UI](https://docs.aws.amazon.com/parallelcluster/latest/ug/pcui-using-v3.html) is a web UI built for AWS ParallelCluster that makes it easy to create, update, and access HPC clusters. It gives you a quick way to connect to clusters via shell [SSM](https://aws.amazon.com/blogs/aws/new-session-manager/) or remote desktop [DCV](https://aws.amazon.com/hpc/dcv/). The UI is built using the [AWS ParallelCluster API](https://docs.aws.amazon.com/parallelcluster/latest/ug/api-reference-v3.html), making it fully compatible with any cluster 3.X or greater regardless of if you create the cluster through the API, CLI or Web UI.

#### Want to request a new feature?

1. First checkout the [Github Issues](https://github.com/aws/aws-parallelcluster-ui/issues)
2. If you don't already see your feature, [open a feature request](https://github.com/aws/aws-parallelcluster-ui/issues/new)

#### AWS ParallelCluster UI Architecture

AWS ParallelCluster UI is built on a fully serverless architecture, in most cases it's [completely free](https://docs.aws.amazon.com/parallelcluster/latest/ug/install-pcui-v3.html#install-pcui-costs-v3) to run, you just pay for the clusters themselves.

![pc_arch](architecture.png)

#### Costs

AWS ParallelCluster UI is built on a serverless architecture and falls into the free-tier for most uses. I've detailed the dependency services and their free-tier limits below:

| Service       | Free Tier                                                        |
|---------------|------------------------------------------------------------------|
| Cognito       | 50,000 Monthly Active Users                                      |
| API Gateway   | 1M Rest API Calls                                                |
| Lambda        | 1M free requests / month & 400,000 GB-seconds of compute / month |
| Image Builder | No-Cost except EC2                                               |
| EC2           | ~15 mins one-time to build Container Image                       |

Typical usage will likely cost < $1 / month.

#### Get Started

You can get started with your first cluster in as little as ~15 minutes following [**1 - Setup**](01-getting-started.html)
