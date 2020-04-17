*******************************************************************************
Using Gitlab CI to build and deploy a Java application to AWS Elastic Beanstalk
*******************************************************************************


Overview
--------

You will learn:
- how build advanced CI pipeline with: codestyle checks, unit tests, API tests, performance tests & security checks
- learn about cloud services and how to use AWS for deployments

- we increase the complexity of the application we are building and deploying
- we will be building and testing a Java application and we will deploy it to Amazon Web Services (AWS)
- the pipeline is more complex, mostly the same GitLab CI principles will be used. 
- this course is focused on building pipelines
- check the Resourced for links to articles and other video tutorials

.............................................................

.............................................................

.............................................................

.............................................................


Introduction to the Java application
------------------------------------

About the project
- simple Java application that represents a simple car fleet management solution.
- use IDE called IntelliJ
- check the Resources for some tutorials that can help you get started
- together we will be building the CI/CD pipeline
- you can work on the .gitlab-ci.yml file without IntelliJ
- to run the project from IntelliJ click on Gradle > cars-api > Tasks > application > bootRun
- the app exposes an API that allows you to add, view, and remove cars from a database.

What is an API?
- an API is a program that does not have a graphical interface, like a website, for example
- an API offers raw data, typicall form a database
- an API can use used by a front-end application to display the data in the browser
- work with the API by using Postman

.............................................................

.............................................................

.............................................................

.............................................................

.............................................................

.............................................................


Running the application in Postman
----------------------------------

- if you have started the application in IntelliJ, you cannot view this in a browser as it does not have a user interface (UI)
- use a software development tool called Postman, which is free to download and install
- you will find the Postman collection attached to this lecture
- can click on "Import" and select both the collection and the environment


.............................................................

.............................................................

.............................................................

.............................................................


Continuous Integration (CI) pipeline overview
---------------------------------------------

- the first step is the CI pipeline.
- the CI pipeline typically has a few stages: build, code quality, test, packaging the application for later use.
- The purpose of the CI pipeline is to ensure that the artifact is releasable

.............................................................

.............................................................

.............................................................


 Build stage: Building a Java application locally
 ------------------------------------------------
- most programs go through a build stage.
- build process will take the source code and transform it into something that can be executed on a computer
- we call this process compilation
- for this project, build process will translate source code into Java bytecode that can be executed on the Java Virtual Machine (JVM)
- the output is a jar file (which is an archive) that contains this bytecode
- to run the build process locally, I will use a tool called Gradle, which is just a build tool
- Gradle helps make the build process much simpler

.............................................................

.............................................................

.............................................................


 Build stage: Building a Java application with Gitlab CI
 -------------------------------------------------------

 Let's do the same thing now in GitLab:
 
- define new pipeline file: .gitlab-ci.yml and add it to Git
- add build stage & job
- publish artifact

.............................................................

.............................................................

.............................................................

Smoke test
----------

- what is a smoke test?
- a straightforward test that checks if something works
- if you were responsible for testing a computer, you will only push the power button and see if something appears on the screen
- we will start the Java application and see if it works by calling the /health endpoint using cURL

.............................................................

.............................................................

.............................................................


CI/CD pipeline recap
--------------------

Now we have an artifact (or a package of software) and we are ready for deployment.

So what to do next?

There are two oppisite directions in which we can go:

- deploy on your own intrastructure (aka server that we control and manage)
- deploy using a cloud provider, like Amazon AWS, Google Cloud, Microsoft Azure and many 

The advantage using a cloud provider is that you only rent the infrastructure for the time you are using it. Let's take this example:

If you build an ecommerce shop and you want to host it, you will need to buy a server, place it somewhere save, make backups, handle software updates, fix any hardware failures and so on.  During peak times (for example before Christmas), you may need to buy an additional server that will stay idle for the rest of the year.

Using a cloud provider, you can focus on actually building and maintaining the application and forget about the hardware and scalability issues.

Which option makes more sense, it is up to you. For some of the reasons mentioned above, cloud services have risen grately in popularity in the last years.

The following lectures will show to use use Amazon AWS to deploy the Java application.

.............................................................

.............................................................

.............................................................


Brief introduction to Amazon Web Services (AWS)
-----------------------------------------------

- Amazon Web Services (AWS) is a cloud platform offering over 170 services available in data centers located all over the world
- AWS offers services that include virtual servers, managed databases, file storage, content delivery, and many others
- while creating an account is free, AWS is a paid service
- a basic usage of many services is free in the first year
- adding a debit/credit card is mandatory

.............................................................

.............................................................

.............................................................


Serverless computing with AWS Elastic Beanstalk
-----------------------------------------------

- with AWS you rent a virtual machine that has a dedicated CPU, memory and disk
- with a virtual server you still need to handle software updates, back-ups, monitoring, and any other aspects that ensure your application is running as expected
- AWS Elastic Beanstalk is a way to deploy an application but let AWS handle the hardware and software needed to run it
- AWS Elastic Beanstalk has a serverless architecture
- serverless architecture: you don't care about the hardware and software needed to run your application
- using a serverless architecture is the easiest way to deploy an application in the cloud

.............................................................

.............................................................

.............................................................



Manually deploying a Java application to AWS Elastic Beanstalk
--------------------------------------------------------------

- use the AWS console interface to upload the jar file
- provide the jar file as a resource

.............................................................

.............................................................

.............................................................


How to deploy to AWS from GitLab CI
-----------------------------------

- the goal is to avoid any manual interaction
- AWS offers a CLI tool that can be used from GitLab CI

.............................................................

.............................................................

.............................................................


Getting started with AWS S3
---------------------------

- AWS S3 is the main storage service for the entire AWS platform
- we will use S3 to upload the artifact

.............................................................

.............................................................

.............................................................


GitLab Group settings
---------------------

- the GitLab group functionality is a way to organize similar projects into group
- having a group allows you to configure environment variables that are available in multiple projects

.............................................................

.............................................................

.............................................................


How to upload a file to AWS S3 from GitLab CI
---------------------------------------------

- create user, set environment variables
  - AWS_ACCESS_KEY_ID
  - AWS_SECRET_ACCESS_KEY
  - S3_BUCKET
- upload the artifact to S3 using the copy command (aws s3 cp)

.............................................................

.............................................................

.............................................................

.............................................................


How to deploy a Java application to AWS Elastic Beanstalk using the AWS CLI
---------------------------------------------------------------------------

- deploying to AWS EB involves two steps
  - creating a new application version referencing the artifact from S3
  - updating the production environment with the latest application version 

.............................................................

.............................................................

.............................................................

.............................................................


Create an application version
-----------------------------

- the Java application allows us to insert the application version in one of the info endpoints
- this allows us to check which version is currently deployed

.............................................................

.............................................................

.............................................................

.............................................................


Verify the application version after deployment
-----------------------------------------------

- we should ensure that the right application version was deployed
- we can do this by checking the /info endpoint which will tell us the current application version

.............................................................

.............................................................

.............................................................

.............................................................


Ensuring coding standards with tools for codestyle checking
-----------------------------------------------------------

- Most projects want to have a consistent code style and to follow some conventions
- automated tools are used to assist with this task
- some do static code analysis, as the inspection performed without actually running the code
- other tools do a dynamic code analysis, which will actually run the code in order to perform the inspection
- one simple one that can be used for Java projects is PMD
- PMD is a static code analysis tool
- PMD can help find unused variables, problematic code blocks and overall to enforce generally accepted best practices
- PMD already has a large set of predefined rules but will also allow you to configure or add new rules, as needed


.............................................................

.............................................................

.............................................................

.............................................................


Introduction to unit testing in CI pipelines
--------------------------------------------

- what is a unit test
- unit tests are responsible for testing only single units of code, typically one class
- the execution time is very fast and give instant feedback
- use JUnit or similar
- for Java projects, JUnit is the most popular framework for writing unit tests

.............................................................

.............................................................

.............................................................

.............................................................


Unit test stage: Run JUnit tests with GitLab CI
-----------------------------------------------

- covers how to add execute JUnit tests
- the JUnit XML report is publish as an artifact and recognized by GitLab CI

.............................................................

.............................................................

.............................................................

.............................................................


How to structure a CI/CD pipeline in GitLab CI?
-----------------------------------------------

- when structuring a pipeline, you need to take into account multiple factors
- what is the logical flow of the pipeline
- which jobs can be grouped together and which not
- run jobs that have a similar execution time in parallel
- put jobs that are easy to run in the beginning (fail fast principle)

.............................................................

.............................................................

.............................................................

.............................................................


API test stage: Run Postman API tests in GitLab CI
--------------------------------------------------

- for this application, API tests are the highest level of tests we can execute
- you can use Postman to write tests
- Newman is the CLI tool from Postman
- Newman allows you to run Postman tests from the CLI

.............................................................

.............................................................

.............................................................

.............................................................


GitLab Pages
------------

- allows you to publish HTML websites directly from a repository
- great option for creating dashboards
- you can publish HTML reports

.............................................................

.............................................................

.............................................................

.............................................................