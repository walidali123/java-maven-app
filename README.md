# Jenkins Shared Library Project

This project contains a Jenkins shared library for automating CI/CD pipelines. The library is used to define reusable pipeline components and functions to streamline the development, build, and deployment processes.

## Description

The Jenkins shared library is loaded from a Git repository and includes functions for building a JAR file, building and pushing a Docker image, and deploying an application.

## Usage

To use this shared library in your Jenkins pipeline, follow these steps:

1. Import the library in your pipeline script.
2. Define the stages of your pipeline using the provided functions.

### Importing the Library

```groovy
#!/usr/bin/env groovy

library identifier: 'jenkins-shared-library@master', retriever: modernSCM(
        [$class: 'GitSCMSource',
         remote: 'https://gitlab.com/Walidfadel/jenkins-shared-library.git',
         credentialsId: 'walid-gitlab-credentials'
        ]
)

# Script Functions

- `buildJar()`: Builds a JAR file using Maven.
- `buildImage(String imageName)`: Builds a Docker image with the provided image name.
- `dockerLogin()`: Logs into a Docker registry.
- `dockerPush(String imageName)`: Pushes a Docker image to a registry.
- `deployApp()`: Deploys the application.

## Notes

- Ensure that the necessary credentials are configured in Jenkins for accessing Git and Docker registries.
- Customize the pipeline stages and functions according to your project requirements.

This readme provides detailed instructions on how to use the Jenkins shared library.

