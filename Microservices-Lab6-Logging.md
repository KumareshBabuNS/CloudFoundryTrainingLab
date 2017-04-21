In this exercise, you will view logs for the browser application as well as instrumentation for distributed tracing in another demo application.

>Notice that Cloud Foundry automatically treats your application logs as streams and makes them easily accessible: http://12factor.net/logs

### CLI Logs

Cloud Foundry aggregates logs related to your application. These include logs for Cloud Foundry components as well as calls to your application.
To help with tracing of microservice calls, the browser app has been instrumented using [Spring Cloud Sleuth](http://cloud.spring.io/spring-cloud-static/spring-cloud-sleuth/1.0.9.RELEASE/). This adds tracing information to your logs.

#### Tailing Logs

- Use ` cf logs` to tail logs for your browser application.
- Issue a few requests through the web interface for the browser application.

You should see logs from the Router component of CF

` 2016-05-20T12:50:59.07-0600 [RTR/5] <-- RTR is the Router.`

and also your application.

```
2016-05-20T12:52:11.07-0600 [APP/0]      OUT 2016-05-20 18:52:11.068  INFO [browser,92ca8a46b9903cdc,92ca8a46b9903cdc,true]
```

The section [` browser,92ca8a46b9903cdc,92ca8a46b9903cdc,true`] is added by Spring Cloud Sleuth to display tracing information. 
This corresponds to [app-name from configuration, spanID, traceID,…].

This info is also available in the browser UI.
![](Lab6-1.png)

If you want to see just the application logs, you can run:
` cf logs | grep APP`

### Working with Zipkin

We have pushed four microservice applications that use Spring Cloud Slueth with an external [Zipkin](http://zipkin.io/) service.

The source for these applications can be found at: https://github.com/mikegehard/DistributedTracingDemo_Velocity2016

The Zipkin UI can be found at http://zipkin-server-receivable-plasmolysis.cfapps.io/.

In the previous section you saw span and trace ids added to the application logs. Now lets see how zipkin works with this information to provide visiblity into your microservice application.

If you want to add a new trace, you can curl any one of the microservices using:

```
curl http://acme-financial-ui-transferrable-conversazione.cfapps.io/start
curl http://acme-financial-account-microservice-parlourish-pumicer.cfapps.io/action
curl http://acme-financial-back-office-microservice-spoutless-nonfreeman.cfapps.io//action
curl http://acme-financial-customer-microservice-overeager-browser.cfapps.io//action
```

#### Understanding the trace hierarchy

In your browser Navigate to the [dependencies](http://zipkin-server-receivable-plasmolysis.cfapps.io/dependency) to view the microservice hierarchy.

- Which microservices get called from the UI service?
- Which microservices are at the end of the call chain?

#### Viewing trace timings

In your browser Navigate [here](http://zipkin-server-receivable-plasmolysis.cfapps.io/) to access the find trace form.

Clicking on the Find Traces button will bring up a list of existing traces.

Click on one of the traces and let’s dive into the details.

- How many services are invoved in this trace?
- How long does the total trace take?
- What HTTP endpoints get called for each span?
- What controller methods get called for each span?
- How long is spent in each span for the trace?
- Which leaf service takes the longest?
