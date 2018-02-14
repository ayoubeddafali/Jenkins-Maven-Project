# Devops Training

#### This is a simple *Continuous Integration* environment for Java based projects.


# Files:

 - **Jenkinsfile** : where the stages are defined.
 - **pom.xml** : where the actual phases are defined.
 - **src/** : source code directory.

---

In the Jenkins file, we have 7 stages that are :
 - Checkout from scm stage.
 - Build stage.
 - Unit tests stage.
 - Packaging stage.
 - Deploy to snapshots repo stage.
 - Deploy to releases repo stage.
 - Functional tests stage.

### First Stage : Checking source code for SCM

 Jenkins by default trigger a builtin stage that checkout the code for us. But for better control, we need to perform a manual checkout, so we deactivate the default behavior of jenkins by skiping the default checkout in this way :

 ```groovy
 options {
    skipDefaultCheckout: true
 }
 ```

We specify also that `agent none` which means that we must specify an agent in each stage we define. This is a good practice when you rely on slave nodes to do some work, or just for testing purposes.
The first 6 will be executed in the `master` node, and the last one (Functional test) will be executed on a slave node. We indicate the node with this block of code :

```groovy
 agent {
    label 'Slave1'
 }
```

We use the checkout function to perform a checkout from an scm specifying the `url`, `branches`, and the `credentialsId` :

![images/checkout.png]()

### Second Stage : Build

Here, we're compiling the java classes to bytecode using `maven`:

![images/build.png]()

### Third Stage : Unit Tests

Here, We are using the `surefire` plugin to perform the tests suite, and generate a report for the current build.

![images/unit_tests.png]()

### Fourth Stage : Packaging

Here, we are compiling, packaging the code, and install the artifacts locally, and archiving them.

![images/packaging.png]()

### Fifth Stage : Deploying to Snapshots Repo

Here we are deploying to the snapshots repository , indicated in the `distributionManagment` tag in **pom.xml** file using the `deploy` phase.

![images/snapshot.png]()

Here we are deploying to the releases repository, indicated in the `distributionManagmenet` tag in **pom.xml** file using the release plugin.

### Sixth Stage : Deploying to releases Repo.

![images/release.png]()

### Seventh Stage : Functional Testing

Here we are downloading our artifact, and testing it on a slave node.

![images/functional.png]()


# Maintainer

> Ayoub Ed-dafali








