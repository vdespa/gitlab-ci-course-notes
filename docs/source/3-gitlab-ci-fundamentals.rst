**********************
Gitlab CI Fundamentals
**********************

This section provides an overview of the most essential features available in Gitlab CI.

Overview
--------
- we will go over the most critical features in Gitlab
- we will continue to improve the existing pipeline
    - execution speed
    - general Git workflow
    - add environments
    - add manual steps

- we will go over the fundamentals but not always get into details
- consider reviewing the resources which often point to the official documentation
- assignments will help you get more practice and explore additional feature of Gitlab

YOUR NOTES

.............................................................

.............................................................

.............................................................

.............................................................

Predefined environment variables
--------------------------------

- let's try to add a version to your website so that we know which version is currently deployed
- Gitlab comes with a large list of predefined variables
- Full list at https://docs.gitlab.com/ee/ci/variables/predefined_variables.html
- Try the following in your pipeline to see how variables look like: `echo $CI_COMMIT_SHORT_SHA`
- the dollar sign $ indicates that this is a variable
- we will add a “version” to the website by replacing a marker
- edit the file `src/pages/index.js`
- we will use %%VERSION%% as a marker, but you can use whatever you like
- sed tool - stream editor
    - `sed -i 's/word1/word2/g'` inputfile 
    - s is for substitute
    - use /g at the end for a global replace
    - -i option for edit in place
    - `sed -i "s/%%VERSION%%/$CI_COMMIT_SHORT_SHA/" ./public/index.html`
    - don't forget to use double quotes when using variables, as they won't be replaced
- adapt jobs / tests to reflect the new information
- `curl -s "instazone.surge.sh" | grep "$CI_COMMIT_SHORT_SHA"`

YOUR NOTES

.............................................................

.............................................................

.............................................................

.............................................................


Pipeline triggers / Retrying failed jobs / Pipeline schedules
-------------------------------------------------------------

- sometimes jobs fail for no apparent reason
- especially if your pipeline takes a long time to build and one of the last jobs failed, it might be worth retrying it
- pipelines can be triggered manually without commits
    - manually click on "Run Pipeline"
    - can select the branch
    - define variables
- you can set a schedule when your pipeline should run
    - from your project to go CI/CD > Schedules
    - click on New Schedules
    - you can use one of the predefined options or define your own using the cron syntax
    - you can wait for the pipeline to run or you can manually run it (in case you want to test the configuration)
    - you can run some jobs using the condition `only/except: - schedules`
    - see the full documentation at https://docs.gitlab.com/ee/user/project/pipelines/schedules.html

YOUR NOTES

.............................................................

.............................................................


Using caches to optimize the build speed
----------------------------------------

- you probably have noticed that some of the jobs do need a lot of time to run
- especially the build job which needs to download some dependencies before it can run
- if you are used with other more “traditional” CI servers like Jenkins, this extra time might seem like "forever"
- this behavior occurs because each job is started using a clean environment and only the code within Git is available
- rest assured, there is a solution for this and it is called "cache"
- using caches it is possible to speed up the execution of the job by instructing Gitlab to hold onto some files that we might need
- What to cache?
    - ideal candidates for caching are the external project dependencies that are not stored in Git and that need to be downloaded
    - in our case, the project dependencies are defined in the packages.json file as npm dependencies
    - the folder that npm uses is called `node_modules`
- Usage in .gitlab-ci.yml:

.. code-block:: yaml

    cache:
        key: ${CI_COMMIT_REF_SLUG}
        paths:
            - node_modules/

- the cache can be used locally (on a job level) or globally
- you should notice that each job now has an overhead of downloading the cache (pull) and re-uploading the cache (push)
- troubleshooting: Clearing caches
    - sometimes caches misbehave
    - go from your project to Pipelines
    - click on the button "Clear Runner Caches" to delete the cache
- fine-tuning the cache is a more advanced topic, but for the moment we are good to go

YOUR NOTES

.............................................................

.............................................................

.............................................................

.............................................................

.............................................................

.............................................................

.............................................................

.............................................................

Cache vs Artifacts
------------------

- let's clarify one thing: the difference between cache and artifacts
- they might seem very similar but they are not the same thing and serve different purposes
- artifacts
    - is usually the output from the build process (the package that we want to deploy)
    - an artifact can be partial (if the final package is built across multiple stages)
    - artifacts can be used to pass data between jobs/stages
- cache
    - should not be used for storing artifacts (even if technically possible)
    - should only be used as temporary storage for project dependencies
- read the official documentation: https://docs.gitlab.com/ee/ci/caching/#cache-vs-artifacts

YOUR NOTES

.............................................................

.............................................................

.............................................................

Environments
------------

- currently, we are directly deploying to master (which is not optimal)
- look at the CI/CD diagram we can notice that we are a few systems short
- even if we do Continuous Deployment, we rarely want to deploy to the production system directly
- adding a pre-production or testing stage and running some tests there is a desirable approach
- this allows us to run different kind of tests which require the whole system to respond (usually called integration or acceptance tests)
- it also allows us to test the deployment process before doing this on the production system
- our scenario is very simplistic, but the same idea applies even to large and complex systems
- Gitlab has the concept of environments
- environments allow you to control the continuous deployment of your software
- allows you to track your deployments, so that you know what is currently installed, on which systems and in which version
- environments let you simply tag your jobs and in this way Gitlab knows what you are doing
- you can do this inside a deployment job with:

.. code-block:: yaml

    environment:
    name: staging
    url: http://somedomain.surge.sh


- you can view your environments from your project page by going to Operations > Environments

YOUR NOTES

.............................................................

.............................................................

.............................................................

.............................................................


Defining variables
------------------

- it is not a good idea to duplicate information that can change (for example the domain name)
- you can define variables in the jobs or globally
- you can specify a variable like this:

.. code-block:: yaml

    variables:
      STAGING_DOMAIN: somedomain.surge.sh

- now if you need to change the domain name, you only have to do it in one place


YOUR NOTES

.............................................................

.............................................................


Manual deployments / Manually triggering jobs
---------------------------------------------

- we are revisiting the Continuous Delivery strategy
- when doing Continuous Delivery, we want to have a manual review step before going to production 
- Gitlab offers the possibility of manually triggering jobs
- add `when:manual` to the jobs that need this
- now you will need to view the pipeline and manually click on the "play" button associated with the job
- if there are additional stages after the manual job, they will still be executed
- if this is not desired, additionally configure the manual job with: `allow_failure: false`
- `allow_failure: false` combined with a manual job will set the pipeline in the status "Blocked"

YOUR NOTES

.............................................................

.............................................................

.............................................................


Merge Requests: Using Branches
------------------------------

- right now everything is pushed to master
- while we have in place tests to make sure nothing goes to production that does not work, it still breaks the pipeline
- a broken pipeline means other developers cannot continue working => costs time & money
- so we want to avoid breaking the master, as much as possible
- we could use branches for each new feature, task or bugfix
- once the work was reviewed and the pipeline is successful, the branch can be merged back to the master branch
- this also ensures that the master branch is all the time deployable (which is an essential aspect of CD)
- there are many strategies for dealing with branches
- one of the most known branching models is Gitflow
- you are free to use which model works best for you 
- just avoid working only with the master branch
- we will simply create a new branch for each new change and stop pushing directly to the master branch
- if we create a branch, all the jobs will run as normal
- but we do not want to deploy to staging or production from a branch
- we can set a job policy to run the deploy jobs only for the master branch: 

.. code-block:: yaml

    only:
      - master

YOUR NOTES
.............................................................

.............................................................

.............................................................


Merge requests: Configuring Gitlab
----------------------------------

- to implement our new workflow, we need to do a few settings for your project
- no longer allow pushing to master
    - go to Settings > Repository > Protected branches
    - set Allow to push to "No one"
    - nobody will be able to push a change directly to master
    - all the changes must go through the process of creating a Merge Request
- configuring Merge Requests
    - go to Settings > General > Merge Requests
    - set Merge method to Fast-forward merge
    - under Merge checks, check Pipelines must succeed

YOUR NOTES

.............................................................

.............................................................


Merge requests: Your first merge request
----------------------------------------

- to create a Merge Request (MR) we first need to create a branch
- make a change inside your branch (add some text or something to the website)
- on top of Gitlab you should see Gitlab inviting you to create a Merge Request
- you can also do this from Merge Requests > New merge request
- the Title of the MR will be pre-filled with the commit message you have given
- you can select to delete the branch after the Merge Request was accepted (merged)
- after the branch is merged, the master pipeline will start

YOUR NOTES

.............................................................

.............................................................

.............................................................

Dynamic environments
--------------------

- right now we don't have an environment where we can inspect the merge requests
- we can automatically spin up a dynamic environment for each merge request
- this allows us to review the changes on an actual system
- we can also run more advanced tests if we want to
- this can not only be a good thing for developers but for testers or product owners/project managers and so on
- we can make a dynamic environment by using predefined Gitlab variables
- we can use the following variables
    - $CI_COMMIT_REF_NAME to have the branch name as the environment name
    - $CI_ENVIRONMENT_SLUG for a url-friendly environment name

.. code-block:: yaml

    environment:
        name: review/$CI_COMMIT_REF_NAME
        url: https://instazone-$CI_ENVIRONMENT_SLUG.surge.sh


YOUR NOTES

.............................................................

.............................................................

.............................................................

.............................................................


Destroying environments (Clean-up after the Merge Request)
----------------------------------------------------------

- with so many potential branches, once they are merged, the environments that were created are no longer needed
- we need to tell surge to delete the environments when we don't need them anymore
- surge documentation: https://surge.sh/help/tearing-down-a-project
- by setting the variable GIT_STRATEGY to none inside a job, you will disable git cloning for that job
- this is needed for the "stop review" which needs to run even if the branch was deleted
- if the branch is deleted, it does not make sense to clone the repository and try to open that branch
- "deploy review" needs to have a link to the "stop review" job
- the link is setting on_stop
- "stop review" will be automatically triggered by Gitlab one the branch was merged

YOUR NOTES

.............................................................

.............................................................

.............................................................

.............................................................