In the last section, we lost all our data when we restarted our app. In this section, we will fix that.

Creating a MySQL instance

We will create an instance of mysql and bind it to our app, thereby removing state from memory.
Use cf marketplace to view the available services and plans.
Use cf create-service to create a MySQL service instance cleardb and select the free plan spark.

Checking Your Work
You should be able to see your service instance:
cf services
...

name           service   plan    bound apps   last operation   
people-mysql   cleardb   spark                create succeeded   

Binding to Your App
You need to bind your service instance to your application so that is can be used.
Use cf bind-service to bind your service instance to your application.
Restage your app so that it uses the new service: cf restage.

Checking Your Work
You should be able to see your service instance bound to your app:
cf services

...
name         service   plan    bound apps   last operation   
people-mysql cleardb   spark   people       create succeeded

Testing Statelessness
At this point, you should be able to put data into your service that lands in the external mysql service.
curl -X POST -H "Content-Type:application/json" -d '{"firstName":"Steve", "lastName":"Greenberg", "company":"Pivotal"}' http://people-<RANDOM_ROUTE>.cfapps.io/people

Restart your app.
You should still see the data:
curl http://people-<RANDOM_ROUTE>.cfapps.io/people
...

{
  "_embedded" : {
    "people" : [ {
      "firstName" : "Steve",
      "lastName" : "Greenberg",
      "company" : "Pivotal",
      "_links" : {
        "self" : {
          "href" : "http://people-<RANDOM_ROUTE>.cfapps.io/people/2"
        },
        "person" : {
          "href" : "http://people-<RANDOM_ROUTE>.cfapps.io/people/2"
        }
      }
    } ]
  },
  "_links" : {
    "self" : {
      "href" : "http://people-<RANDOM_ROUTE>.cfapps.io/people"
    },
    "profile" : {
      "href" : "http://people-<RANDOM_ROUTE>.cfapps.io/profile/people"
    },
    "search" : {
      "href" : "http://people-<RANDOM_ROUTE>.cfapps.io/people/search"
    }
  },
  "page" : {
    "size" : 20,
    "totalElements" : 1,
    "totalPages" : 1,
    "number" : 0
  }
}
Congrats! You now have a stateless app: 12factor.net/processes

How does it work?
Run the following:
cf env people

This will print the environment variables for your application. Look for a System-Provided variable called VCAP_SERVICES. You should see the service credentials for your mysql service. Note:
Cloud Foundry leverage the environment variables: 12factor.net/config
Cloud Foundry treats services as attached resources: 12factor.net/backing-services

Scale Out
By moving the state for your application into an external service, you can now scale out your application horizontally.
Use cf scale to scale your app to 2 instances.
Notice that you can scale by adding instances: 12factor.net/concurrency

Checking your Work
You should see 2 instances:
cf app people
...

0   running   2016-05-17 09:53:40 AM   0.1%     376.8M of 750M   153.7M of 1G      
1   running   2016-05-17 10:01:35 AM   0.0%     232.1M of 750M   153.7M of 1G      

Scale Down
Use cf scale to reduce your app back to 1 instance.
Notice that you can start quickly and dispose of unneeded instances gracefully: 12factor.net/disposability
