#!groovy
pipeline {
  agent none
  stages {
    stage('sonar-scanner') {
      agent {
        docker {
          image 'sonarsource/sonar-scanner-cli'
        }
      }
      steps {
        sh 'sonar-scanner -Dsonar.token=squ_78b56e97db2eaa0d9ec39a67e7765a50032bb8d8 -Dsonar.host.url=http://host.docker.internal/sonarqube'
      }
    }
    stage('Docker Build') {
      agent any
      steps {
        sh 'docker build -t localhost:5000/hello-world-nodejs .'
      }
    }
    stage('Docker Push') {
      agent any
      steps {
        withCredentials([usernamePassword(credentialsId: 'dockerHub', passwordVariable: 'dockerHubPassword', usernameVariable: 'dockerHubUser')]) {
          // sh "docker login -u ${env.dockerHubUser} -p ${env.dockerHubPassword}"
          sh "docker login http://localhost:5000/v2/ -u ${env.dockerHubUser} -p ${env.dockerHubPassword}"
          sh 'docker push localhost:5000/hello-world-nodejs:latest'
        }
      }
    }
  }
}