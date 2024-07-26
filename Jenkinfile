pipeline {
  agent any

  environment {
    DOCKERHUB_CREDENTIALS = credentials('dockerhub')
  }

  stages {
    stage('Build') {
      steps {
        script {
          dockerImage = docker.build("myorg/mywebsite:${env.BUILD_ID}")
        }
      }
    }
    stage('Push') {
      steps {
        script {
          docker.withRegistry('https://index.docker.io/v1/', DOCKERHUB_CREDENTIALS) {
            dockerImage.push()
          }
        }
      }
    }
    stage('Deploy') {
      steps {
        script {
          sh 'kubectl apply -f k8s/deployment.yaml'
          sh 'kubectl apply -f k8s/service.yaml'
        }
      }
    }
  }
}
