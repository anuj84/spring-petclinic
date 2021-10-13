
########################################################
# Adding build to petclinic

## Change Log
* Update to pom.xml to use JCenter
* Added Dockerfile to containerize the app
* Added Jenkinsfile to build artifact and container

## Run pipeline
* Create a multiline pipeline in Jenkins
* Update the Branch Sources section with the repository URL
```
https://github.com/anuj84/spring-petclinic.git
```
* Select the branch petclinic-ci
* Select if unit tests need to run
* Run the pipeline

## Run Docker Image

* Successfull execution of the pipeline creates image 'petclinic' which can run the application using the following command
    ```
    docker run -p 8080:8080 petclinic
    ```
* Runnable docker image is available on docker hub and can be downloaded using 
    ```
    docker pull anuj1537/registry:petclinic
    ```

#########################################################