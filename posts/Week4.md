# Week 4 [25012023 (Wednesday) - 27012023:]

## ======= Concept of Logging, why log, key considerations =======

- <strong>High Data Throughput</strong>; To debug issues successfully, engineering teams need a high count of logs per second and low-latency log processing. To avoid business-disrupting outages or failures, engineers need to get critical log data quickly, and this is where log collectors with high data throughput are preferable.

- <strong>Reliability</strong>; Log collectors should ensure the high integrity of processed data. Even under increased data throughput, the integrity of data should be preserved. The frequency and amount of data loss by log collectors should be bounded and ideally avoided under any conditions.

- <strong>Scalability</strong>; There are several strategies that enable log collectors to handle large volumes of data. Filtering the logs that aren’t a high priority, parsing, or compressing complex logs are just a few of them. Also, it’s important to consider that such data processing may incur increased CPU and memory resource utilization as the price to pay for the capability to scale. Also, when the data rate increases, there is a higher resource consumption and possible backpressure which also affects the scaling capability of log collectors.

- <strong>Handling a variety of data formats</strong>; Logs come in many different formats from various elements of cloud applications and infrastructure. Containers and microservices are developed in different programming languages or frameworks, with varying approaches to logging formats. Avoiding the need to use more than one log collector to handle the number of formats reduces the overall complexity.

- <strong>Support for various data sources and destintions</strong>; Log collectors source data from a variety of cloud environments. The log data sources may also include message queueing and streaming platforms such as Kafka, Redis, and RabbitMQ. The log collectors send the data to different destinations, such as log management tools and storage archives. The ability of log collectors to handle various sources and destinations adds to their flexibility and usability.

- <strong>Security</strong>; The capacity to handle sensitive information, such as anonymizing or excluding confidential fields and sending logs to storage backends in a secure manner, should be considered when assessing any log collector. 

## === ELK stack (Logstash) vs EFK stack (Fluentd vs/& FluentBit): ===

Elasticsearch is simply the "database" and Kibana is the visualization tool. (Recap: Grafana is for computer metrics, while Kibana is for logs. Also, Prometheus is like Logstash for metrics).

<ul>
	<li>Fluentd has a smaller memory footprint to Logstash.</li>
	<li>Fluentd has vendor neutrality (a CNCF project), better support for Kubernetes.</li>
	<li>Fluentd is traditionally meant for logging JSON data.</li>
	<li>Logstash has greater capabilities for log processings/ filtering the logs into certain wanted information compared to Fluentd and FluentBit.</li>
</ul>

Logstash is a reliable log collector with many options for processing log data, other log collectors described in this blog may be better if a small memory footprint is a critical requirement. Because Logstash is written in Java, it requires JVM support. If you’re planning to collect logs from embedded devices and IoT applications, it‘s not the best choice (too heavy).

<ul>
	<li>Fluentd is heavier than FluentBit (40MB vs 450KB).</li>
	<li>Fluentd is written in Ruby (mostly) and C, while FluentBit is wholly written in C.</li>
	<li>Better community support with Fluentd (around 650 plugins) to FluentBit (around 35).</li>
	<li>But according to Mayur, FluentBit has more or less most of the capabilities of Fluentd.</li>
</ul>

FluentBit acts as a collector and forwarder and was designed with performance in mind, as described above. Fluentd was designed to handle heavy throughput — aggregating from multiple inputs, processing data and routing to different outputs. FluentBit is not as pluggable and flexible as Fluentd, which can be integrated with a much larger amount of input and output sources.

In a way, FluentBit is to Fluentd, what Beats are to Logstash — a lightweight shipper that can be installed as agents on edge hosts or devices in a distributed architecture. In Kubernetes for example, FluentBit would be deployed per node as a daemonset, collecting and forwarding data to a Fluentd instance deployed per cluster and acting as an aggregator — processing the data and routing it to different sources based on tags.

The difference between Fluentd and FluentBit can therefore be summed up simply to the difference between log forwarders and log aggregators. The former are installed on edge hosts to receive local events. Once received, the event is forwarded to the log aggregators. The latter are daemons that receive streams of events from the log forwarders, buffer them and periodically upload the data to a data store of some sorts.

https://www.fluentd.org/
https://fluentbit.io/
https://www.elastic.co/logstash/
https://era.co/blog/choose-open-source-log-collector
https://sematext.com/blog/logstash-alternatives/#logstash-disadvantages
https://logz.io/blog/fluentd-vs-fluent-bit/

## ==== Logging using a sidecar container in a Multi-Container Pod ====

DaemonSet vs Sidecar as your containerization pattern:

<ul>
	<li>Lowered the resource usage when using the DaemonSet pattern.</li>
	<li>Upgrading of applications and infrastructure agents are easier when using DaemonSets.</li>
	<li>Latency between containers are optimized when using DaemonSets.</li>
</ul>

https://wecode.wepay.com/posts/scds-battle-of-containerization

```bash
	$ docker run -it -v /home/wongkanghong/dockerStuff:/dockerStuff cr.fluentbit.io/fluent/fluent-bit:2.0 /fluent-bit/bin/fluent-bit -c /dockerStuff/example.conf
```

https://docs.fluentbit.io/manual/pipeline/inputs/tail
https://www.creativesoftware.com/tale-fluent-bit-input-tail
https://www.golinuxcloud.com/kubernetes-sidecar-container/
https://docs.fluentbit.io/manual/installation/kubernetes
https://matthewpalmer.net/kubernetes-app-developer/articles/configmap-example-yaml.html#:~:text=The%20simplest%20way%20to%20create,necessary%20for%20your%20programming%20language.
https://stackoverflow.com/questions/68712337/fluentbit-not-reading-logs-from-shared-pvc

## ==== Logging using EF(Fluentbit)K on local Aptos Fullnode ====

https://aptos.dev/nodes/full-node/fullnode-source-code-or-docker/
https://itnext.io/docker-in-docker-521958d34efd
https://docs.aws.amazon.com/eks/latest/userguide/storage-classes.html
https://github.com/aptos-labs/aptos-core/tree/main/terraform/helm

https://devopscube.com/setup-efk-stack-on-kubernetes/#:~:text=Conclusion-,What%20is%20EFK%20Stack%3F,large%20volumes%20of%20log%20data
https://blog.opstree.com/2023/01/24/protected-efk-stack-setup-for-kubernetes/
https://stackoverflow.com/questions/61044914/how-to-expose-minikube-in-gcp-vm
https://stackoverflow.com/questions/66088041/how-to-expose-minikube-cluster-to-internet
https://gist.github.com/gbates101/9a513aa0e417de0c4f5d30453bce60d3

GitHub repo to reference logging configMaps and parsers:

https://github.com/fluent/fluent-bit-kubernetes-logging/tree/master/output/elasticsearch

## ================== What is Karpenter ? ==================

To get started with Karpenter in any Kubernetes cluster, ensure there is some compute capacity available, and install it using the Helm charts provided in the public repository. Karpenter also requires permissions to provision compute resources on the provider of your choice. When Karpenter is installed in your cluster:

Karpenter observes the aggregate resource requests of unscheduled pods and makes decisions to launch new nodes and terminate them to reduce scheduling latencies and infrastructure costs. Karpenter does this by observing events within the Kubernetes cluster and then sending commands to the underlying cloud provider’s compute service, such as Amazon EC2. Simply put, Karpenter uses a proprietary bin-packing algorithm under the hood to help bin-pack your applications into your kubernetes nodes. 

<img src="http://35.240.250.152/wp-content/uploads/2023/03/image12.png" alt="Karpenter" width="100%">

Accelerated Computing: Karpenter works with all kinds of Kubernetes applications, but it performs particularly well for use cases that require rapid provisioning and deprovisioning large numbers of diverse compute resources quickly. For example, this includes batch jobs to train machine learning models, run simulations, or perform complex financial calculations. You can leverage custom resources of nvidia.com/gpu, amd.com/gpu, and aws.amazon.com/neuron for use cases that require accelerated EC2 instances.

Provisioners Compatibility: Kapenter provisioners are designed to work alongside static capacity management solutions like Amazon EKS managed node groups and EC2 Auto Scaling groups. You may choose to manage the entirety of your capacity using provisioners, a mixed model with both dynamic and statically managed capacity, or a fully static approach. We recommend not using Kubernetes Cluster Autoscaler at the same time as Karpenter because both systems scale up nodes in response to unschedulable pods. If configured together, both systems will race to launch or terminate instances for these pods.

https://aws.amazon.com/blogs/aws/introducing-karpenter-an-open-source-high-performance-kubernetes-cluster-autoscaler/