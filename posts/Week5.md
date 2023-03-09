# Week 5 [30012023 - 03022023:]

## ==== Concept of Monitoring Metrics, Prometheus and Grafana ====

Modern DevOps is becoming more and more complex. DevOps engineers are building complex infrastructures that are becoming difficult to handle/maintain and monitor manually. It, therefore, needs more automation such as Prometheus monitoring. 

<strong>Prometheus</strong> is an open-source automated monitoring and alerting system. It has become a widely accepted tool for monitoring highly dynamic container environments such as Kubernetes. It can also monitor traditional non-container infrastructures such as Linux/Apache/Windows servers, single applications, and Database services.

When running Prometheus to monitor Kubernetes, it scrapes metrics data frim the Kubernetes Cluster for monitoring the system. These metrics are CPU status, memory/disk space storage, request duration, number of running processes, server resources, and node status. The actual monitoring work of Prometheus can be broken down into threee components:

<ul>
	<li>Data Retrieval Worker: This component scrapes and pulls the metrics data from the application. It then transforms the metrics data into time-series data.</li>
	<li>Time Series Database: This component stores the metrics data in time-series format.</li>
	<li>HTTP Server: This component accepts queries fro the stored time-series data and displays the data in a web UI or dashboard. It can use the inbuilt/default Prometheus Web UI or a third-party platform (example: Grafana).</li>
</ul>

When you use Prometheus to monitor a Kubernetes cluster, it automatically manages all the microservices. It can then easily detect any failures in the processes running the microservices. It also provides quicker debugging of the Kubernetes cluster to identify errors and failures. When prometheus detects an error in one of the microservices, it uses the Alert Manager component to notify the system administrator. The system administrator will then take the necessary measures to resolve the error. 

Summarization of Prometheus:

<ul>
	<li>Constantly monitoring the microservices running in the Kubernetes cluster.</li>
	<li>Automatically alert the administrator when a microservice crashes.</li>
	<li>It reports on the cluster's health and performance.</li>
    <li>Displays the metric data on the Prometheus Web UI or a third party platform.</li>
</ul>

<strong>Grafana</strong> is a multi-platform solution that gets data from a data source such as Prometheus and transform it into visualization charts. 

## ================== Helm / Helm Charts ==================

Helm is popularly known as the package manager for Kubernetes. Helm bundles a Kubernetes application into a package known as a Helm Chart, a collection of YAML files, helping us define, install, upgrade and integrate even the most complex applciations on Kubernetes using a few Helm commands. 

```bash
	$ helm repo add <repo name from artifact hub etc.>
    $ helm repo update
    $ helm repo list
```
<img src="http://35.240.250.152/wp-content/uploads/2023/03/image42.png" alt="Helm3.0" width="100%">

```bash
	$ helm create <chart name>
```

<ul>
	<li>Chart.yaml: This is the main file that contains the description of our chart</li>
	<li>values.yaml: this is the file that contains the default values for our chart</li>
	<li>templates: This is the directory where Kubernetes resources are defined as templates</li>
    <li>charts: This is an optional directory that may contain sub-charts</li>
</ul>

The most important piece of the puzzle is the templates/ directory. This is where Helm finds the YAML definitions for your Kubernetes objects. If you already have definitions for your application, all you need to do is replace the generated YAML files for your own. What you end up with is a working chart that can be deployed using the helm install command.

https://helm.sh/docs/

If you already have definitions for your application, all you need to do is replace the generated YAML files for your own. What you end up with is a working chart that can be deployed using the helm install command. We can do a dry-run of a helm install and enable debug to inspect the generated definitions:

        - helm install --dry-run --debug <deployment name> <chart folder>

When you run the ‘helm install’ command:

<ul>
	<li>Kubernetes reads the configuration YAML files in the Helm Chart. </li>
	<li>It then sends the Deployments, Services, ConfigMaps, and Secrets configuration YAML files to the Kubernetes API server.</li>
	<li>Kubernetes then takes these files and creates the application inside the Minikube Kubernetes Cluster. </li>
</ul>

The .Values object is a key element of Helm charts, used to expose configuration that can be set at the time of deployment. The defaults for this object are defined in the values.yaml file. 

Note: If a user of your chart wanted to change the default configuration, they could provide overrides directly on the command-line:

```bash
	$ helm install example mychart --debug --set service.internalPort=8080
```
Note: a running instance of a chart with a specific config is called a release

```bash
	$ helm delete <release name>
```
We typically install charts from a tar package. We can use helm package to create this tar package:

```bash
	$ helm package <chart folder>
```

Helm will create a tar package in our working directory, using the name and version from the metadata defined in the Chart.yaml file. A user can install from this package instead of a local directory by passing the package as the parameter to helm install.

```bash
	$ helm install <deployment name> <tgz file name> <optional specifications, e.g. --set service.type=NodePort>
```

https://getbetterdevops.io/helm-quickstart-tutorial/
https://tanzu.vmware.com/developer/guides/create-helm-chart/

## == Monitoring Metrics using Prometheus & Grafana on local cluster ==

https://sweetcode.io/how-to-integrate-prometheus-and-grafana-on-kubernetes-with-helm/

## ============= Dashboards-as-code via GitOps =============

<ol>
	<li>Generate Grafana-compatible JSON containing dashboard objects.</li>
	<li>Add that build to CI.</li>
	<li>Configure Grafana to pick up files from that directory.</li>
</ol>

Advantages of not creating dashboards visually through the Grafana UI:

<ul>
	<li>Codifying dashboards therefore leads to long-term consistency, yet making large changes easy. If you tell people to visually create dashboards instead of dashboard-as-code, after a few months you will see a bloat of outdated, awful, non-informative and unreviewed dashboards, with lots of them probably unused.</li>
	<li>Coded dashboards improve visability as intended. Everyone looks at a dashboard and see the same things, read the same things, and understand the dashboard exactly the way you understand it. </li>
</ul>

https://andidog.de/blog/2022-04-21-grafana-dashboards-best-practices-dashboards-as-code
https://grafana.com/blog/2020/02/26/how-to-configure-grafana-as-code/

## ================== GraphQL vs REST ==================

### ----------------------------------- RESTful APIs -----------------------------------

REST stands for Representational State Transfer. It (RESTful API) is an API design paradigm meant to standardize web services via a series of constraints. In particular, RESTful APIs are <strong>stateless</strong> (neither server nor client stores data from each other) and was built so that any compliant web service can interact with textual resource representations. 

In other words, using REST when building a web service makes it easier for clients and servers to understand each other even when one does not have knowledge of the other.

We use HTTP requests as follows with regards to CRUD operations:

<ul>
	<li>GET; fetches a resource from the server</li>
	<li>POST; requests a resource to be created on the server</li>
    <li>PUT; requests for a resource to be updated</li>
    <li>DELETE; requests for a resource to be deleted</li>
    <li>PATCH: requests for a resource to be updated. While PUT replaces the entire resource with a new resource, PATCH alters only the specified data.</li>
</ul>

Note HTTP Status codes; 200 -> Success, 400 -> client-side errors, 500 -> server-side errors.

Payload refers to data that has been transferred through the RESTful API (be it from client or server). Aside from this payload (when looking at HTTP responses), there is typically a message embedded within a response body along with its response code.

The 5 core components of a HTTP request:

<ul>
	<li>Method; or the verb -> aforementioned operations GET, POST, etc.</li>
	<li>URI; typically using the URL, the URI identifies the resource.</li>
    <li>HTTP version; indicates the HTTP version used (typically 1.1 or 2.0).</li>
    <li>Request Header; contains metadata -> message format, cache settings, etc.</li>
    <li>Request Body; contains content or data being sent.</li>
</ul>

The 4 core components of a HTTP response:

<ul>
	<li>Status Code; provides information of success or failure of a request.</li>
	<li>HTTP version; indicates the HTTP version used.</li>
    <li>Response Header; contains the content length, type, date, etc.</li>
    <li>Response Body (Note: the request body of a request does not always exist, but there will always exist a response body for a response); contains the return data.</li>
</ul>

An idempotent method yields the same response regardless of how many times a request is sent. Example; the nth press of a button on a webpage that sends a GET request to a backend RESTful API will still yield the same response as that of its first press. Note: Remember that the RESTful API architecture is designed on the principle that the clients' requests should not be depended on previous API calls (hence stateless -> methods are indempotent).

Considerations when building RESTful APIs:

<ul>
	<li>Supporting JSON data transfer</li>
	<li>Using proper status codes</li>
    <li>Using URI hierachy to represent relationship of resources</li>
    <li>Using idempotent HTTP methods</li>
    <li>Using Caching</li>
    <li>Incorprating Security Measures</li>
</ul>

### ----------------------------- Caching with RESTful APIs -----------------------------

When data is cached, we reduce load on servers, reduce server costs (lesser requests made to our backend) and clients can receive their data faster. 

<ul>
	<li>GET requests should be cachable by default – until a special condition arises. Usually, browsers treat all GET requests as cacheable.</li>
	<li>POST requests are not cacheable by default but can be made cacheable if either an Expires header or a Cache-Control header with a directive, to explicitly allows caching, is added to the response.</li>
    <li>Responses to PUT and DELETE requests are not cacheable at all.</li>
</ul>

When a client makes an API request to a server for the first time, the server returns a response with one of these configurations in its header: ETag, Cache-Control, or Last-Modified. Based on these configurations, the client decides how to cache the response data.

For instance, if you set the value of Cache-Control in the API response header to max-age=60, the browser will cache the response data for sixty seconds. When the browser makes a request to the API within the sixty-second timeframe, the browser will use the cached data instead of sending a request to the server via the API.

https://stellate.co/blog/deep-dive-into-caching-rest-apis

### ------------------- Token-Based Authentication with RESTful APIs -------------------

<img src="http://35.240.250.152/wp-content/uploads/2023/03/image39.png" alt="TokenAuth" width="100%">

<img src="http://35.240.250.152/wp-content/uploads/2023/03/image40.png" alt="Auth2.0" width="100%">

### ----------------------------------- GraphQL APIs -----------------------------------

GraphQL is a relatively new API query language that was developed and open-sourced by Facebook in 2012. It’s designed to handle the complex, nested data dependencies found in modern applications. Specifically, it enables declarative data fetching to allow clients to specify exactly what data is needed from an API.

<img src="http://35.240.250.152/wp-content/uploads/2023/03/image2.png" alt="RESTvsGraphQL" width="100%">

GraphQL is a relatively new API query language that was developed and open-sourced by Facebook in 2012. It’s designed to handle the complex, nested data dependencies found in modern applications. Specifically, it enables declarative data fetching to allow clients to specify exactly what data is needed from an API.

GraphQL has three distinct features that developers must consider before working with it. These are:

<ul>
	<li>Different handling of endpoints: In REST, the endpoint is tied to the identity of a resource. That means lots of endpoints with specific shapes. In contrast, GraphQL separates them. That means one endpoint regardless of the query’s complexity.</li>
	<li>Different handling of resources: In REST, the server determines the shape of the resource. When a client asks for a resource, it can result in the return of too much or too little information (called over-fetching or under-fetching). In GraphQL, the server declares what resources exist and the client asks for what it needs. That makes it easier to receive more specific data from the server.</li>
    <li>Different handling of caching: REST can handle caching using the GET method. However, GraphQL uses POST for fetching data, and entities are not identified by URLs – which means GraphQL lacks native caching functionality. Some libraries exist that can help developers get around this, or caching needs can be handled on the client’s end. GraphQL generally posits that caching should be handled by clients.</li>
</ul>

Use REST when:

<ul>
	<li>A simpler web service is involved: REST was developed when web services were still relatively simple (stateless, entire data is transfered). It’s perfectly fine to use it for projects that don’t need a hyper-efficient way to handle endpoints or resources.</li>
	<li>Maximum compatibility is desired: REST is more established and enjoys widespread support. As a result, it’s easier to find more tools, integration, and infrastructure for a more specialized project.</li>
</ul>

In contrast, use GraphQL when:

<ul>
	<li>Account for increased mobile usage: GraphQL creates more efficient data loading by minimizing the data that must be transferred over a network (and arguably, increases security). That helps underpowered devices or those on poor-quality networks access web services with minimal disruption.</li>
	<li>Call many nested endpoints in one request: GraphQL solves the problem of over- or under-fetching data and can retrieve multiple resources with a single query. It can call many related functions without making multiple trips, which makes the entire process more straightforward.</li>
</ul>

https://www.cosmicjs.com/blog/graphql-vs-rest-a-quick-guide
https://www.howtographql.com/basics/1-graphql-is-the-better-rest/