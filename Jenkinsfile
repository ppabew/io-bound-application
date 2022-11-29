pipeline {
  agent any
  stages {
    stage('Set Variable') {
      steps {
        script {
          IMAGE_NAME = "io-bound-jenkins"
          IMAGE_STORAGE = "ppabew"
          IMAGE_STORAGE_CREDENTIAL = "docker-hub"
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
          docker.withRegistry("https://registry.hub.docker.com", IMAGE_STORAGE_CREDENTIAL) {
            image.push("${env.BUILD_NUMBER}")
            image.push("latest")
            image
          }
        }
      }
    }

    stage('Server Run') {
      steps {
        sh "docker rm -f io-bound-worker1;"
        sh "sleep 10;"
        sh "docker run -e datasource.passwd='mw9129(!@(' -d -p 9091:8090 --name ${IMAGE_NAME} ${IMAGE_STORAGE}/${IMAGE_NAME}:latest;"
        sh "docker ps -a"
      }
    }

  }
}