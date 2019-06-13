***********************************
Basic CI/CD workflow with Gitlab CI
***********************************

This section provides a short introduction to the CI/CD workflow

What is CI / CD?
----------------

- CI stands for Continuous Integration
    - CI is a practice
    - code is integrated with other developers
    - most commonly the way to integrate code is to check if the build step is still working
    - common practice is too also check if unit tests still work
    - coding guidelines should also be considered (can be mostly automated)
    - work is integrated all the time, multiple times per day
    - most of the time the goal of the CI pipeline is to build a package that can be deployed

- CD can stand for Continuous Delivery
    - CD is done after CI
    - you cannot do CD without first doing CI
    - the goal of CD is to take the package created by the CI pipeline and to test it further
        - making sure it can be installed (by actually instaling the package on a system similar to production)
        - run additional tests to check if the package integrates with other systems
    - after a manual check / decision, the package can be installed on a production system as well

- CD can also stand for Continuous Deployment
    - CD goes a step further and automatically installes every package to production
    - the package must first go through all previous stages successfully
    - no manual intervention is required

YOUR NOTES:
.............................................................

.............................................................

.............................................................

.............................................................

.............................................................

.............................................................

- the advantages of CI
    - detecting errors early in the development process
    - reduces integration errors
    - allow developers to work faster

- the advantages of CD
    - ensure that every change is releasable
    - reduces the risk of a new deployment
    - delivers value much faster (changes are released more often)


Short introduction to Node.js and npm
-------------------------------------

- Node.js is a runtime environment for executing JavaScript
- initially JavaScript was only executed within a browser
- Node.js opened the posibility of running JavaScript without a browser.
- we will need Node.js for running some tools
- npm is the Node Package Manager
- npm is used to install new tools / libraries

YOUR NOTES:
.............................................................

.............................................................

.............................................................


Creating a new project
----------------------

- to build a simple static website we will use a tool called Gatsby
- Gatsby runs on Node.js and we need to use npm to install it
- first check that Node.js / npm are installed. 
- open a terminal and run `node --version` and `npm --version`
- in order to install simply gatsby, run `npm install -g gatsby-cli`
- `-g` instructs npm to globally install the tool on your computer (so that you can use it from any folder)
- to create a new website run `gatsby new static-website`
- instead of static-website you can use any name you like
- inspect the website by starting the local development server: `gatsby develop`
- open the website from the address http://localhost:8000


YOUR NOTES:
.............................................................

.............................................................

.............................................................

.............................................................

.............................................................

.............................................................

Building the project locally
----------------------------

- `gatsby develop` can only be used for local development
- we need to create a production-ready version of the website
- to create a build from gatsby we use: `gatsby build`
- the output is inside the public folder

YOUR NOTES:
.............................................................

.............................................................

Short introduction to images and Docker
---------------------------------------

- Docker is a tool that allows virtualization
- there is a large collection of images that have many software configurations "pre-installed"
- docker works with images
- an image is a file with a set of instructions on how to package code or tools and all the dependencies
- once an image is executed, it becomes a container
- a container has some similarities with a virtual machine (but it isn't one)
- traditional CI server require to install all the tools / dependencies on the server
- Gitlab breakes this pattern (see the architecture lecture as well) and works with Docker images which contain all the dependencies 
- the Gitlab CI Runner with be using the specified Docker images

YOUR NOTES:
.............................................................

.............................................................


Building the project using Gitlab CI
------------------------------------

- I recommend you use a code editor to edit the project files
- I am using Visual Studio Code which is free to use and can be downloaded from https://code.visualstudio.com/
- we now need to replicate the steps that we have done on our computer in Gitlab
- we first need to create a new file for defining the pipeline: .gitlab-ci.yml
- we use `npm install` to install all the project dependencies
- we also need to install Gatsby: `npm install -g gatsby-cli`
- with `gatsby build` we will build the website on Gitlab
- we prefer to use a Docker image instead of manually installing node & npm
- the reason for using a Docker image with pre-installed node & npm is speed and a simpler pipeline configuration file
- use Docker Hub (https://hub.docker.com/) to search for images 
    - make sure you are using official images 
    - make sure the image that you plan to use is updated regulary
    - usually a large number of downloads in a good indicative that the image is popular and up-to-date
    - we will use the "node" image without specifing a version
    - if the job fails, try using node in a specific version: 
- due to the architecture of Gitlab, executing the jobs may seem very slow (compared to other CI servers) 
- we will improve the speed of the jobs during the course
- the output from the job is not saved anywhere, so we need to define an artifact
- the artifact contains only the public folder (no other files)

YOUR NOTES:
.............................................................

.............................................................

.............................................................

.............................................................

Adding a test stage
-------------------

- why do jobs fail?
    - all command have an exit code (between 0 and 255) that is returned
    - 0 means successfully executed
    - >0 means that is failed 
- we will add a simple test
- the test will check if a string is found inside the index.html file (the start page of the website)
- we use the command `grep` like this: `grep "Gatsby" index.html`
- use `echo $?` will give you the last exit code (on Unix-like machines)
- it is important to find a way to test your assertions, to make sure that the pipeline actually fails

YOUR NOTES
.............................................................

.............................................................

.............................................................

.............................................................


Running jobs in parallel
------------------------

- we can use the alpine Docker image to optimize our build speed
- alpine images are only 5MB in size and are faster to download and start
- we can use Gatsby to start a server from the production build (the public folder)
- when we start a server with the website, we can run other kinds of tests
- we use `gatsby server` to start a local server
- we want to start a server with our website and check if it works
- we can use curl to download a copy of the website using HTTP (similar to what a normal browser does)
- pipes (|) are used to use the output from one command as the input for another
- assigning two jobs to the same stage makes them run in parallel
- when planning to run parallel jobs, you need to make sure there are no dependencies between them

YOUR NOTES

.............................................................

.............................................................

.............................................................

.............................................................

Running jobs in the background
------------------------------

- gatsby serve is a never-ending process; it will run forever
- such comamnds will block the pipeline as the runner will wait for them to complete (which they don't)
- all jobs have a default timeout (most commonly 1h)
- if a job is not done in 1 hour, Gitlab will terminate it and the pipeline will be marked as failed
- adding & after a command will release the terminal and run the command in the background
- with the sleep command you can add a delay between comamnds
- using sleep is considered rather a bad practice, but in some cases it is acceptable (especially if we are talking about a few seconds)
- if you are adding sleeps that exceed 30 seconds, it might be a sign that you are doing something working
- pipelines can run in parallel (if the previous pipeline did not finish before the next pipeline is triggered)
- if you want to stop a running job, you can simply open the job and click on Cancel
- we have used the tac command to fix an error that has occured as a result of using curl

YOUR NOTES

.............................................................

.............................................................

.............................................................

.............................................................

Deployment using surge.sh
-------------------------

- surge is a cloud platform for hosting static websites
- surge is easy to use & configure
- install surge on your computer using `npm install --global surge`
- to create an account / project simply run `surge` and follow the instructions

YOUR NOTES

.............................................................

.............................................................

Using environment variables for managing secrets
------------------------------------------------

- do not store any credentials (username, passwords, tokens) in your pipeline OR project files
- with Gitlab you can define environment variables that contain secrets
- environment variables will be available when running the pipeline
- in order to deploy with surge from Gitlab, we need to generate a token: `surge token`
- we do not want to give Gitlab our username (email) and password, so a token is a better alternative
- to create an environment variable from your project to Settings > CI/CD
- the variables that we will use are SURGE_LOGIN and SURGE_TOKEN
- the name of the variables is given by Surge, as Surge will automaticaly detect them

YOUR NOTES

.............................................................

.............................................................

.............................................................

.............................................................

Deploying the project using Gitlab CI
-------------------------------------

- everytime you want to use a tool, you need to make sure it is installed
- the node Docker image that we use does not include surge, so we use npm to install surge
- alternatively we could use a Docker image that already has surge installed 
- to deploy a project run: `surge --project ./public --domain SOMENAME.surge.sh`

YOUR NOTES

.............................................................

.............................................................

.............................................................

.............................................................