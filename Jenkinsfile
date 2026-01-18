pipeline {
  agent any

  environment {
    DOCKER_BFLASK_IMAGE = "meera786/my-flask-app:latest"
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
                credentialsId: 'dockerhub-creds',
                usernameVariable: 'DOCKER_USER',
                passwordVariable: 'DOCKER_PASS'
            )
        ]) {
            sh '''
                echo "$DOCKER_PASS" | docker login -u "$DOCKER_USER" --password-stdin
                docker push meera786/my-flask-app:latest
            '''
        }
    }
}


  post {
    always {
      sh 'docker logout'
    }
  }
}
