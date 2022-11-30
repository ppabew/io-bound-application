pipeline {
  agent any
  stages {
    stage('Set Variable') {
      steps {
        script {
          IMAGE_NAME = "io-bound-jenkins"
          IMAGE_STORAGE = "ppabew"
          IMAGE_STORAGE_CREDENTIAL = "docker-hub"
        }
      }
    }

    stage('Server Run') {
      steps {
        sh "docker rm -f io-bound-jenkins-2;"
        sh "sleep 10;"
        sh "docker run -e datasource.passwd='mw9129(!@(' -d -p 9093:8090 --name ${IMAGE_NAME}-2 ${IMAGE_STORAGE}/${IMAGE_NAME}:latest;"
        sh "docker ps -a"
      }
    }

  }
}