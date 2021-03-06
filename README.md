# Kinesis S3

[ ![Build Status] [travis-image] ] [travis]
[ ![Release] [release-image] ] [releases] 
[ ![License] [license-image] ] [license]

## Overview

The Kinesis S3 Sink consumes records from an [Amazon Kinesis][kinesis] stream, and writes them to S3.

There are 2 file formats supported:
 * LZO
 * GZip

### LZO

The records are treated as raw byte arrays. [Elephant Bird's][elephant-bird] `BinaryBlockWriter` class is used to serialize them as a [Protocol Buffers][protobufs] array (so it is clear where one record ends and the next begins) before compressing them.

The compression process generates both compressed .lzo files and small .lzo.index files ([splittable LZO][hadoop-lzo]). Each index file contain the byte offsets of the LZO blocks in the corresponding compressed file, meaning that the blocks can be processed in parallel.

### GZip

The records are treated as byte arrays containing UTF-8 encoded strings (whether CSV, JSON or TSV). New lines are used to separate records written to a file. This format can be used with the Snowplow Kinesis Enriched stream, among other streams.

## Quickstart

Assuming git, **[Vagrant] [vagrant-install]** and **[VirtualBox] [virtualbox-install]** installed:

```bash
 host$ git clone https://github.com/snowplow/kinesis-s3.git
 host$ cd kinesis-s3
 host$ vagrant up && vagrant ssh
guest$ cd /vagrant
guest$ sbt test
```

## Prerequisites

You must have `lzop` and `lzop-dev` installed. In Ubuntu, install them like this:

```bash
host$ sudo apt-get install lzop liblzo2-dev
```

NOTE: These are already installed in the Vagrant quickstart environment.

## Command Line Interface

The Kinesis S3 LZO Sink has the following command-line interface:

```
snowplow-kinesis-s3: Version 0.4.0. Copyright (c) 2014-2015, Snowplow Analytics
Ltd.

Usage: snowplow-lzo-s3-sink [OPTIONS]

OPTIONS
--config filename
                   Configuration file.
```

## Running

Create your own config file:

```bash
guest$ cd /vagrant
guest$ cp src/main/resources/config.hocon.sample my.conf
```

You will need to edit all fields in the config.  Consult [this portion][config] of the setup guide on how to fill in the fields.

Next, start the sink, making sure to specify your new config file:

```bash
guest$ sbt "run --config my.conf"
```
    
## Find out more

| Technical Docs              | Setup Guide           | Roadmap & Contributing               |         
|-----------------------------|-----------------------|--------------------------------------|
| ![i1] [techdocs-image]      | ![i2] [setup-image]   | ![i3] [roadmap-image]                |
| [Technical Docs] [techdocs] | [Setup Guide] [setup] | _coming soon_                        |

## Copyright and license

Kinesis S3 is copyright 2014-2015 Snowplow Analytics Ltd.

Licensed under the [Apache License, Version 2.0] [license] (the "License");
you may not use this software except in compliance with the License.

Unless required by applicable law or agreed to in writing, software
distributed under the License is distributed on an "AS IS" BASIS,
WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
See the License for the specific language governing permissions and
limitations under the License.

[travis-image]: https://travis-ci.org/snowplow/kinesis-s3.png?branch=master
[travis]: http://travis-ci.org/snowplow/kinesis-s3

[release-image]: http://img.shields.io/badge/release-0.4.0-blue.svg?style=flat
[releases]: https://github.com/snowplow/kinesis-s3/releases

[license-image]: http://img.shields.io/badge/license-Apache--2-blue.svg?style=flat
[license]: http://www.apache.org/licenses/LICENSE-2.0

[kinesis]: http://aws.amazon.com/kinesis/
[snowplow]: http://snowplowanalytics.com
[hadoop-lzo]: https://github.com/twitter/hadoop-lzo
[protobufs]: https://github.com/google/protobuf/
[elephant-bird]: https://github.com/twitter/elephant-bird/
[s3]: http://aws.amazon.com/s3/
[sbt]: http://typesafe.artifactoryonline.com/typesafe/ivy-releases/org.scala-sbt/sbt-launch/0.13.0/sbt-launch.jar

[vagrant-install]: http://docs.vagrantup.com/v2/installation/index.html
[virtualbox-install]: https://www.virtualbox.org/wiki/Downloads

[setup]: https://github.com/snowplow/snowplow/wiki/kinesis-lzo-s3-sink-setup
[config]: https://github.com/snowplow/snowplow/wiki/kinesis-lzo-s3-sink-setup#configuration
[techdocs]: https://github.com/snowplow/snowplow/wiki/kinesis-lzo-s3-sink

[techdocs-image]: https://d3i6fms1cm1j0i.cloudfront.net/github/images/techdocs.png
[setup-image]: https://d3i6fms1cm1j0i.cloudfront.net/github/images/setup.png
[roadmap-image]: https://d3i6fms1cm1j0i.cloudfront.net/github/images/roadmap.png
