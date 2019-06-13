************
Introduction
************

This section provides a short introduction to Gitlab CI. 

Your first pipeline in Gitlab CI
--------------------------------

- In this first video, we are going to create a very simple pipeline in Gitlab
- what is a pipeline anyway?
- analogy to an assembly line needed to build a car
- notice a few characteristics:
    - a series of steps that need to be done on a certain order
    - the steps are connected
    - the output from the previous step is the input for the next step
    - some steps could be done in parallel (wheels)
- many more steps are required to build the final product
- not only the production part is important but also the final testing part before the car goes to the consumer
- the goal is to get the product out of the factory
- you will later see that building and delivering software is, in some regard, quite similar to the step needed to manufacture a car
- let's try to build something similar with Gitlab CI and have two major stages: 
    - build & test
    - Build a car assembly line using Gitlab CI
    - Add chassis
    - Add engine
    - Add wheels
- later in the test phase, we will test it.
- to get started with Gitlab CI you do not need to download any software or anything similar
- create a free account at gitlab.com
- create a new project. This will we a normal Git repository with nothing inside
- new file .gitlab-ci.yaml
- as in the car assembly, in a software pipeline we also have a series of steps or jobs what we need to perform
- always use spaces for indentation, not tabs.
- after commiting, Gitlab will create the pipeline for us
- every time we make a change in the repository, the pipeline will start
- GitLab will execute your scripts with the tool called GitLab Runner, which runs similarly to your terminal
- Gitlab does not save anything unless told so
- we will use artifacts to save the car.txt file
- inspect final artifact
- where is the build folder in the repository? 
- nothing is automatically commmited to the repository

YOUR NOTES:
.............................................................

.............................................................

.............................................................

.............................................................

.............................................................

.............................................................


Gitlab architecture
-------------------

- you need at least one Gitlab Server and one Runner
- the server will provide the interface, store the repositories
- the execution of the pipeline will be delegated to the Gitlab Runner
- so the Gitlab Server does not actually run the jobs
- this allows for a very scalable architecture
- see the Runners under the project Settings > CI/CD > Runners
- we are using Shared Runners provided by Gitlab.com
- you can create your own runners on your own IT infrastructure and still use gitlab.com
- you can assign runners for specific projects


YOUR NOTES:
.............................................................

.............................................................

.............................................................

.............................................................

.............................................................

.............................................................


Why Gitlab / Gitlab CI?
-----------------------

- Gitlab is a modern tool and Gitlab will probably will become one of the market leaders in the next years
- Gitlab offers: 
    - a modern, scalable architecture
    - you can eaisly work with Docker
    - pipeline as a code
    - partially open source
- you need to try it on your own and see if it solves YOUR problems


YOUR NOTES:
.............................................................

.............................................................

.............................................................


How much does Gitlab cost?
--------------------------

- there are two ways to run Gitlab
    - gitlab.com
    - self-hosted on your own architecture
- gitlab.com
    - has a free package
    - 2000 pipeline minutes
    - easy to start and try it out
- self-hosted
    - has a free options as well (Community Edition)
    - you need to take care of running gitlab (installation, updates, infrastructure, backups, ...)
    - have control over your data 
    
YOUR NOTES:
.............................................................

.............................................................

.............................................................

.............................................................