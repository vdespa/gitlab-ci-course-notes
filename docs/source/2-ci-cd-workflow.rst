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

