---
layout: docs
title:  Installing IBM Streams Runner for Apache Beam on premises
navtitle: Installing
description: Installing IBM® Streams Runner for Apache Beam for an on-premises installation of IBM Streams involves extracting the Streams Runner toolkit and configuring environment variables.
weight:  10
published: true
tag: beam
prev:
  file: beamrunner-2-install
  title: Installing
next:
  file: using
  title: Using
---

Installing IBM® Streams Runner for Apache Beam from an installation of IBM Streams involves extracting the Streams Runner toolkit and configuring environment variables.

## Before you start
The on-premises installation of Streams Runner supports Apache Beam 2.4
applications, so your application must use the [version 2.4 Java
API](https://beam.apache.org/documentation/sdks/javadoc/2.4.0/).

## Before you execute
Ensure the `STREAMS_INSTALL` environment variable is set. If it is not, you
can set it in the `bash` shell by using `cd` to change to the Streams
installation directory and entering the following command:
```bash
. bin/streamsprofile.sh
```

## Installing and configuring Streams Runner

1. Navigate to a directory where you have permission to create files.
1. Extract the toolkit by entering the following command:
```bash
tar -zxvf $STREAMS_INSTALL/etc/beam/com.ibm.streams.beam-x.y.z.tar.gz
```
1. (Optional) Create the Streams Runner environment variables. Although the variables are not required, the documentation refers to them for convenience. If you do not set the environment variables, you must use the full paths when you run the sample applications.
    1. Navigate to the `samples` directory in the expanded toolkit, and set up environment variables for the runner:
    ```bash
    cd com.ibm.streams.beam-x.y.z/samples
    . bin/streams-runner-env.sh
    ```
1. Set up Streams for use with Streams Runner.
    1. Set the `STREAMS_DOMAIN_ID` and `STREAMS_INSTANCE_ID` environment
    variables to the Streams domain and instance you will be using with the
    Streams Runner.
    1. Verify the Streams domain and instance are running:
    ```bash
    streamtool getinstancestate
    ```
    If the `getinstancestate` output includes the error `CDISC5005E` then start the instance:
    ```bash
    streamtool startinstance
    ```
    If the `getinstancestate` includes the error `CDISA5056E` then start
    the domain and instance:
    ```bash
    streamtool startdomain && streamtool startinstance
    ```
    1. Configure your certificates and keystore. Streams Runner uses
    the Streams REST API for Beam metrics and job status, and `java` will
    reject the connection if the certificate is not trusted or does not
    match the host name. Creating and configuring certificates is beyond
    the scope of this installation guide; see security documentation for
    IBM Streams for more information.


## Validating the Streams Runner installation

Streams Runner relies on preserving the `com.ibm.streams.beam` directory structure. To verify an installation, ensure that the `translation` and `sdk` JAR files appear as follows when you enter the following command:
```bash
ls $STREAMS_BEAM_TOOLKIT/lib
```
```
> com.ibm.streams.beam.sdk.jar  com.ibm.streams.beam.translation.jar
```

The Streams Runner directory tree structure:
```
com.ibm.streams.beam-x.y.z/
| - template.vcap
| - README.html
| - README.md
| - com.ibm.streams.beam/
| | - info.xml
| | - toolkit.xml
| | - com.ibm.streams.beam.transforms/
| | - doc/
| | - opt/
| | - impl/
| | | - lib/
| | | | - com.ibm.streams.beam.runtime.jar
| | - lib/
| | | - com.ibm.streams.beam.sdk.jar
| | | - com.ibm.streams.beam.translation.jar
| - samples/
| | - README.html
| | - README.md
| | - pom.xml
| | - src/
| | - target/
| | | - site/
| | - bin/
| | | - streams-runner-env.sh
```
