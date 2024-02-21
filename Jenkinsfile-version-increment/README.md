# Jenkins Pipeline Script

This pipeline script defines a Jenkins pipeline for incrementing application version, building the application, building a Docker image, deploying the image, and committing version updates.

## Pipeline Overview

The pipeline consists of the following stages:

1. **increment version**: Increments the application version based on the Maven project version.
2. **build app**: Builds the application using Maven.
3. **build image**: Builds a Docker image using the application version and pushes it to a Docker registry.
4. **deploy**: Deploys the Docker image to EC2.
5. **commit version update**: Commits the updated version to the Git repository.

## Script Functions

- `increment version`: Increments the application version and sets it in the Maven project.
- `build app`: Builds the application using Maven.
- `build image`: Builds a Docker image and pushes it to a Docker registry.
- `deploy`: Deploys the Docker image to EC2.
- `commit version update`: Commits the updated version to the Git repository.

## Notes

- Ensure that the necessary credentials are configured in Jenkins for accessing Git, Docker registries, and EC2.
- Customize the pipeline script according to your project requirements.

```groovy
#!/usr/bin/env groovy

pipeline {
    agent any
    tools {
        maven 'Maven'
    }
    stages {
        stage('increment version') {
            steps {
                script {
                    echo 'incrementing app version...'
                    sh 'mvn build-helper:parse-version versions:set \
                        -DnewVersion=\\\${parsedVersion.majorVersion}.\\\${parsedVersion.minorVersion}.\\\${parsedVersion.nextIncrementalVersion} \
                        versions:commit'
                    def matcher = readFile('pom.xml') =~ '<version>(.+)</version>'
                    def version = matcher[0][1]
                    env.IMAGE_NAME = "$version-$BUILD_NUMBER"
                }
            }
        }
        stage('build app') {
            steps {
                script {
                    echo "building the application..."
                    sh 'mvn clean package'
                }
            }
        }
        stage('build image') {
            steps {
                script {
                    echo "building the docker image..."
                    withCredentials([usernamePassword(credentialsId: 'docker-hub-repo', passwordVariable: 'PASS', usernameVariable: 'USER')]) {
                        sh "docker build -t walidali123/my-repo:${IMAGE_NAME} ."
                        sh "echo $PASS | docker login -u $USER --password-stdin"
                        sh "docker push walidali123/my-repo:${IMAGE_NAME}"
                    }
                }
            }
        }
        stage('deploy') {
            steps {
                script {
                    echo 'deploying docker image to EC2...'
                }
            }
        }
        stage('commit version update') {
            steps {
                script {
                    withCredentials([usernamePassword(credentialsId: 'gitlab-credentials', passwordVariable: 'PASS', usernameVariable: 'USER')]) {
                        // git config here for the first time run
                        sh 'git config --global user.email "jenkins@example.com"'
                        sh 'git config --global user.name "jenkins"'

                        sh "git remote set-url origin https://${USER}:${PASS}@gitlab.com/Walidfadel/java-maven-app.git"
                        sh 'git add .'
                        sh 'git commit -m "ci: version bump"'
                        sh 'git push origin HEAD:jenkins-jobs'
                    }
                }
            }
        }
    }
}
