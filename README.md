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
