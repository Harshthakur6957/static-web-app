pipeline {
  agent any
  environment {
    DOCKER_CRED = credentials('DockerHub')
  }
  stages {
    stage('fetch-code') {
      steps {
        git branch: 'main', url: 'https://github.com/Harshthakur6957/static-web-app.git'
      }
    }
    stage('build-image') {
      steps {
        sh 'docker build -t cheaterkook/static-web-app:${BUILD_NUMBER} .'
      }
    }
    stage('push-image') {
      steps {
        sh ' echo $DOCKER_CRED_PSW | docker login -u $DOCKER_CRED_USR --password-stdin'
        sh 'docker push cheaterkook/static-web-app:${BUILD_NUMBER}'
      }
    }
    stage('update-minikube') {
      steps {
        sh 'kubectl set image deployment/d4 static-web-app=cheaterkook/static-web-app:${BUILD_NUMBER}'
      }
    }
  }
}
