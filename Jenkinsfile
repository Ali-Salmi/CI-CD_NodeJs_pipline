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
                   def ec2Instance = "ec2-user@3.72.109.42"
                  def dockerRun = "docker run -d -p3000:80 alisalmi/jenkins_repo:${BUILD_NUMBER}"
                   sshagent(['ec2-server-key']) {
                     sh "ssh -o StrictHostKeyChecking=no ${ec2Instance} ${dockerRun}"
                   }
                }
            }
        }
    }
}
