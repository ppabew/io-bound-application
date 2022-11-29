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
          docker.withRegistry("https://${IMAGE_STORAGE}", IMAGE_STORAGE_CREDENTIAL) {
            image.push("${env.BUILD_NUMBER}")
            image.push("latest")
            image
          }
        }

      }
    }

    stage('Server Run') {
      steps {
        sshagent(credentials: [SSH_CONNECTION_CREDENTIAL]) {
          sh "ssh -o StrictHostKeyChecking=no ${SSH_CONNECTION} 'docker rm -f ${IMAGE_NAME}'"
          sh "ssh -o StrictHostKeyChecking=no ${SSH_CONNECTION} 'docker rmi -f ${IMAGE_STORAGE}/${IMAGE_NAME}:latest'"
          sh "ssh -o StrictHostKeyChecking=no ${SSH_CONNECTION} 'docker pull ${IMAGE_STORAGE}/${IMAGE_NAME}:latest'"
          sh "ssh -o StrictHostKeyChecking=no ${SSH_CONNECTION} 'docker images'"
          sh "ssh -o StrictHostKeyChecking=no ${SSH_CONNECTION} 'docker run -d --name ${IMAGE_NAME} -p 8080:8080 ${IMAGE_STORAGE}/${IMAGE_NAME}:latest'"
          sh "ssh -o StrictHostKeyChecking=no ${SSH_CONNECTION} 'docker ps'"
        }

      }
    }

  }
}