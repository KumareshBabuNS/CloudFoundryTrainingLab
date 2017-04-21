This training is targeted at developers with basic hands-on Cloud Foundry experience, some experience deploying web-based applications, comfortable using the command line and SSH and those who have an interest in deploying innovative, microservice-based systems into the cloud.

### You will need:

- 	Code editor of choice like Atom

- 	cURL (for windows): https://curl.haxx.se/download.html#Win64

- 	Pivotal Web Services account or ability to create one

... If you don’t have an account, sign up now at: try.run.pivotal.io/homepage

- 	Cloud Foundry command line interface (CLI): console.run.pivotal.io/tools

... If you already have the CLI, make sure you have the most recent version

```$cf --version
cf.exe version 6.26.0+9c9a261.2017-04-06
```

... If your version is not more recent that this one, please install the latest.
... Make sure you have correctly installed the CLI. From a terminal window/command prompt:

` $cf `

... You should see the self documenting help text. This will be very useful when you go through the class.

###  CF Help

... You can run cf help at any time to see a list of commands. 

... You can also run cf <SOME_COMMAND> --help to see the details for a specific command.

### Login & Target

... Use cf login to target and login to Pivotal Web Services.

... If you are new to PWS, you will notice you are automatically directed to your org and the ‘development’ space.

... You should see output similar to:

``` API endpoint:   https://api.run.pivotal.io (API version: 2.56.0)

User:           sgreenberg@pivotal.io

Org:            Pivotal-Enablement

Space:          development
```

... Alternatively, you can check where you are logged in and targeted at anytime using cf target.

