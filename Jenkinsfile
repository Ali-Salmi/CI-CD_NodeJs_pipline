#!/usr/bin/env groovy
@Library('jenkins-shared-library')_
pipeline {
    agent any
    stages {
        stage('Building Image') {
            steps {
                script{
                    buildImage()
                }
            }
        }
        stage('Pushing to docker hub') {
            steps {
                script{
                    pushDockerhub()
                }
            }
        }
      stage('Deploying to ec2 instance') {
            steps {
                script{
                   echo 'deploying docker image to EC2...'
                   def ec2Instance = "ec2-user@52.59.194.241"
                   def scriptShell="bash ./script.sh alisalmi/jenkins_repo:${BUILD_NUMBER}"
                   sshagent(['ec2-server-key']) {
                     sh "scp -o StrictHostKeyChecking=no docker-compose.yaml ${ec2Instance}:/home/uc2-user"
                     sh "scp -o StrictHostKeyChecking=no script.sh ${ec2Instance}:/home/uc2-user"
                     sh "ssh -o StrictHostKeyChecking=no ${ec2Instance} ${scriptShell}"
                   }
                }
            }
        }
    }
}
