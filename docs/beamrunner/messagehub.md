---
layout: docs
title:  io sample application for IBM Streams Runner for Apache Beam
navtitle: io sample app
description:  You can use a simple application called `io` to learn how to use IBM Cloud Message Hub for file input and output.
weight:  10
published: true
tag: beam
prev:
  file: objstor
  title: FileStreamSample sample app
next:
  file: monitor
  title: Monitoring
---

You can use the IBM® Streams Runner for Apache Beam `io` sample application to learn how to use IBM Cloud Message Hub for file input and output. A Message Hub example application is provided in `$STREAMS_RUNNER_HOME/examples/io`.

## Before you start

Before you run the Apache Beam 2.4 `io` sample application, you must configure and run the following services on IBM Cloud®:

- Streaming Analytics. For more information, see [Creating a Streaming Analytics service on IBM Cloud](../../../beamrunner-2b-sas/#creating-a-streaming-analytics-service-on-bluemix).
- IBM Message Hub.
   - Create the service if you don't already have one. For more information, see [Creating an IBM Cloud Object Storage service](../io/#creating-an-ibm-cloud-object-storage-service).
   - Set up credentials for the service. **Remember**: Make sure the environment variables are configured. For more information, see [Set up credentials for the service](../io/#setting-up-credentials-for-the-service).

**Important**: If you want to compile your application on IBM Cloud, you must unset the `STREAMS_INSTALL` variable before you submit the application to the Streaming Analytics service.

## Running the sample application

1. Go to the `$STREAMS_RUNNER_HOME/examples/io` directory.
2. Compile io examples into a uber jar by running `mvn package`. Then the uber jar `beam-examples-io-x.y.z.jar` should be generated in the `$STREAMS_RUNNER_HOME/examples/io/target` folder.
3. Start the producer by running the following command:

  ```
  java -cp $STREAMS_BEAM_TOOLKIT/lib/com.ibm.streams.beam.translation.jar:$STREAMS_INSTALL/lib/com.ibm.streams.operator.samples.jar:target/beam-examples-io-x.y.z.jar \
        com.ibm.streams.beam.examples.io.mh.Producer\
        --runner=StreamsRunner \
        --contextType=DISTRIBUTED \
        --jarsToStage=target/beam-examples-io-x.y.z.jar \
        --topic=<your message hub topic> \
        --cred=<path to the message hub credential file>
  ```
4. Start the consumer by running the following command:
  ```
  java -cp $STREAMS_BEAM_TOOLKIT/lib/com.ibm.streams.beam.translation.jar:$STREAMS_INSTALL/lib/com.ibm.streams.operator.samples.jar:target/beam-examples-io-x.y.z.jar \
        com.ibm.streams.beam.examples.io.mh.Consumer\
        --runner=StreamsRunner \
        --contextType=DISTRIBUTED \
        --jarsToStage=target/beam-examples-io-x.y.z.jar \
        --topic=<your message hub topic> \
        --cred=<path to the message hub credential file>
  ```
5. If both the producer and the consumer are running correctly, you should see the consumer printing
numbers to `System.out`, one integer per line.
