pipeline {
  agent any

  environment {
    DOCKER_BFLASK_IMAGE = "yourdockerhubusername/my-flask-app:latest"
    DOCKER_REGISTRY_CREDS = "dockerhub"
  }

  stages {

    stage('Build') {
      steps {
        sh 'docker build -t my-flask-app .'
        sh 'docker tag my-flask-app $DOCKER_BFLASK_IMAGE'
      }
    }

    stage('Test') {
      steps {
        sh 'docker run my-flask-app python -m pytest tests/'
      }
    }

    stage('Deploy') {
      steps {
        withCredentials([
          usernamePassword(
            credentialsId: "${DOCKER_REGISTRY_CREDS}",
            usernameVariable: 'DOCKER_USERNAME',
            passwordVariable: 'DOCKER_PASSWORD'
          )
        ]) {
          sh 'echo $DOCKER_PASSWORD | docker login -u $DOCKER_USERNAME --password-stdin'
          sh 'docker push $DOCKER_BFLASK_IMAGE'
        }
      }
    }
  }

  post {
    always {
      sh 'docker logout'
    }
  }
}
