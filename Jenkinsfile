 //#!/usr/bin/env groovy

pipeline {
    agent any
    tools {
        maven 'Maven'
    }
    stages {
        /*stage('increment version') {
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
        }*/
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
                    withCredentials([usernamePassword(credentialsId: 'walid-docker-hub-repo', passwordVariable: 'PASS', usernameVariable: 'USER')]) {
                        sh "docker build -t walidali123/my-repo:jma-3.0 ."
                        sh "echo $PASS | docker login -u $USER --password-stdin"
                        sh "docker push walidali123/my-repo:jma-3.0"
                    }
                }
            }
        }
       /* stage('deploy') {
            steps {
                script {
                     //def dockerCmd = 'docker run -p 8080:80 -d walidali123/my-repo:nana '

                   // sshagent(['ec2-server-key']) {
                       // sh "ssh -o StrictHostKeyChecking=no ec2-user@52.90.228.98 ${dockerCmd}"

                    echo 'deploying docker image to EC2...'
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
       
    }*/
}
}

