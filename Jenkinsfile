pipeline {
  agent any

  stages {
    stage('Checkout') {
      steps {
        checkout scm
      }
    }

    stage('Build Docker Image') {
      steps {
        sh 'docker build . -t flask-app:latest'
      }
    }

    stage('Run Docker Container') {
      steps {
        script {
          // Stop existing container if running
          sh '''
            if [ "$(docker ps -q -f name=flask-app)" ]; then
              docker stop flask-app
              docker rm flask-app
            fi
          '''
          // Run container in background, map port 5000
          sh 'docker run -d --name flask-app -p 5000:5000 flask-app:latest'
        }
      }
    }
  }

  post {
    always {
      echo 'Cleaning up: stopping and removing container'
      sh '''
        docker stop flask-app || true
        docker rm flask-app || true
      '''
    }
  }
}
