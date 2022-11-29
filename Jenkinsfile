pipeline {
  agent any
  stages {
    stage('Set Variable') {
      steps {
        script {
          IMAGE_NAME = "io-bound-jenkins"
          IMAGE_STORAGE = "ppabew"
        }
      }
    }

    stage('Remove Docker Image') {
            steps {
                script {
                    sh "docker rm -f ${IMAGE_STORAGE}/${IMAGE_NAME};"
                }
            }
    }

    stage('Build Container Image by Maven') {
        steps {
            withMaven(maven: 'M3') {
                      sh "mvn docker build -t ppabew/io-bound-jenkins ."
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
        sh "docker run -e datasource.passwd='mw9129(!@(' -d -p 8090:8090 --name io-bound-worker1 ppabew/io-bound-application;"
        sh "docker ps -a"
      }
    }
  }
}