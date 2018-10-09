# simple-java-maven-app

This is a simple jenkins build tutorial.

Following steps

1. Fork sample repository on GitHub 
2. Clone git project locally to obtain the simple "Hello world!" Java application from GitHub

## What is in the repository
The repository contains a simple Java application which outputs the string
"Hello world!" and is accompanied by a couple of unit tests to check that the
main application works as expected. The results of these tests are saved to a
JUnit XML report.

## What is in the jenkins directory
The `jenkins` directory contains the completed `Jenkinsfile` (i.e. Pipeline) for this tutorial and 
the `scripts` subdirectory contains a shell script with commands that are executed when Jenkins processes
the "Deliver" stage of your Pipeline.


## Create your Pipeline project in Jenkins

1. log in to Jenkins and click create new jobs or click New Item at the top left.

![Jenkins image](images/jenkins-01)

2. In the Enter an item name field, specify the name for your new Pipeline project (e.g. simple-java-maven-app).

3. Scroll down and click Pipeline, then click OK at the end of the page.

4. Specify a brief description for your Pipeline (e.g. An entry-level Pipeline demonstrating how to use Jenkins to build a simple Java application with Maven.)

5. Click the Pipeline tab at the top of the page to scroll down to the Pipeline section.

6. From the Definition field, choose the **Pipeline script from SCM option**. 
This option instructs Jenkins to obtain your Pipeline from Source Control Management (SCM), which will be your locally cloned Git repository.

7. From the SCM field, choose **Git.**

    In the Repository URL field, specify the directory path of your locally cloned repository above, which is from your user account/home directory on your host machine, mapped to the /home directory of the Jenkins container - i.e.

8. For example "/Users/rfeggins@us.ibm.com/projects-oct-2018/simple-java-maven-app"

9. Click **Save** to save your new Pipeline project. 

