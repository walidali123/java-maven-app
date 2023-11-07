#!/usr/bin/env groovy

/*library identifier: 'jenkins-shared-library@master', retriever: modernSCM(
    [$class: 'GitSCMSource',
     remote: 'https://gitlab.com/nanuchi/jenkins-shared-library.git',
     credentialsId: 'gitlab-credentials'
    ]
)*/

pipeline {
    agent any
    tools {
        maven 'maven'
    }
    environment {
        IMAGE_NAME = 'walidali123/my-repo:java-maven-2.0'
    }
    stages {
        stage('build app') {
            steps {
               script {
                  echo 'building application jar...'
                  buildJar()
               }
            }
        }
        stage('build image') {
            steps {
                script {
                   echo 'building docker image...'
                   buildImage(env.IMAGE_NAME)
                   dockerLogin()
                   dockerPush(env.IMAGE_NAME)
                }
            }
        }
        stage('deploy') {
            steps {
                script {
                   echo 'deploying docker image to EC2...'

                   //def shellCmd = "bash ./server-cmds.sh ${IMAGE_NAME}"
                   //def ec2Instance = "ec2-user@35.180.251.121"
                   def dockerComposeCmd = "docker-compose -f docker-compose.yaml up --detach"
                   sshagent(['ec2-server-key']) {
                       sh "scp docker-compose.yaml ec2-user@52.91.79.89:/home/ec2-user"
                       sh "ssh -o StrictHostKeyChecking=no ec2-user@52.91.79.89 ${dockerComposeCmd} "
                       //sh "scp -o StrictHostKeyChecking=no server-cmds.sh ${ec2Instance}:/home/ec2-user"
                       //sh "scp -o StrictHostKeyChecking=no docker-compose.yaml ${ec2Instance}:/home/ec2-user"
                       //sh "ssh -o StrictHostKeyChecking=no ${ec2Instance} ${shellCmd}"
                   }
                }
            }
        }
    }
}
