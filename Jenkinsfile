pipeline {
  agent any
  stages {
    stage('Set Variable') {
      steps {
        script {
          IMAGE_NAME = "io-bound-jenkins"
          IMAGE_STORAGE = "ppabew"
          IMAGE_STORAGE_CREDENTIAL = "ppabew"
          SSH_CONNECTION = "root@172.27.0.89"
          SSH_CONNECTION_CREDENTIAL = "ppabew"
        }
      }
    }

     stage('Remove Docker Image') {
            steps {
                sh "docker rm -f ${IMAGE_STORAGE}/${IMAGE_NAME}"
            }
    }


    stage('Build Maven') {
        steps {
            withMaven(maven: 'M3') {
                      sh 'mvn clean install'
            }
        }
    }

    stage('Build Container Image') {
      steps {
        script {
          image = docker.build("${IMAGE_STORAGE}/${IMAGE_NAME}")
        }
      }
    }

    stage('Push Container Image') {
      steps {
        script {
          sh "docker push ${IMAGE_STORAGE}/${IMAGE_NAME}"
        }

      }
    }

    stage('Server Run') {
      steps {
        sh "docker run -e datasource.passwd='mw9129(!@(' -d -p 8090:8090 --name io-bound-worker1 ${IMAGE_STORAGE}/${IMAGE_NAME};"
        sh "docker ps -a"
      }
    }

  }
}