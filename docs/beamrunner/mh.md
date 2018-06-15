---
layout: docs
title:  Using the IBM Cloud Message Hub service with IBM Streams Runner for Apache Beam
navtitle: Using IBM Cloud Message Hub
description:  Beam applications can produce/consume messages to/from IBM Cloud Message Hub using Beam's native KafkaIO.
weight:  10
published: true
tag: beam
prev:
  file: objstor
  title: Using IBM Cloud Object Storage
next:
  file: performance
  title: Performance considerations
---

Beam applications can produce/consume messages to/from IBM Cloud Message Hub
using Beam's native [KafkaIO](https://beam.apache.org/documentation/sdks/javadoc/2.4.0/org/apache/beam/sdk/io/kafka/KafkaIO.html).
IBM Cloud Message Hub is a scalable, distributed, high throughput messaging
service that enables applications and services to communicate easily and
reliably. For more information, see [Getting started with Message Hub](https://console.bluemix.net/docs/services/MessageHub/index.html).

## Creating a Message Hub service on IBM Cloud

If you have not already done so, you must create a Message Hub service on IBM Cloud.

1. Navigate to IBM Cloud [Catalog page](https://console.bluemix.net/catalog/), and search for **Message Hub**.
2. Click the **Message Hub** service.
3. For **Pricing Plan**, choose standard.
4. Click Create. IBM Cloud returns to the manage page of the Message Hub service.
5. On the manage page, navigate to **Topic** tab, click the plus button (**create**), provide a topic name, and then click **Create Topic**. You will provide this topic name to the producer and consumer in subsequent steps.

## Setting up credentials for the service

To communicate with Message Hub from Beam applications, you must specify the
IBM Cloud service credentials.

1. From the Message Hub manage page, click **Service credentials** on the left navigation bar.
2. If necessary, create a credential by clicking **New credential**. Use the default information and click Add.
3. Click **View credentials**.
4. Copy the credentials JSON content to a file (_e.g._, `mh.cred`) for future uses.

## Running an example application

This release provides a Message Hub example in `$STREAMS_RUNNER_HOME/examples/io`.

1. Navigate to the `$STREAMS_RUNNER_HOME/examples/io` directory.
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
