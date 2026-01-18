pipeline {
    agent any

    environment {
        DOCKER_IMAGE = "meera786/my-flask-app:latest"
    }

    stages {
        stage('Build') {
            steps {
                sh 'docker build -t my-flask-app .'
                sh 'docker tag my-flask-app $DOCKER_IMAGE'
            }
        }

        stage('Test') {
            steps {
                sh 'docker run my-flask-app python -m pytest app/tests/'
            }
        }

        stage('Deploy') {
            steps {
                withCredentials([
                    usernamePassword(
                        credentialsId: 'dockerhub-creds',
                        usernameVariable: 'DOCKER_USERNAME',
                        passwordVariable: 'DOCKER_PASSWORD'
                    )
                ]) {
                    sh '''
                        echo "$DOCKER_PASSWORD" | docker login -u "$DOCKER_USERNAME" --password-stdin
                        docker push $DOCKER_IMAGE
                    '''
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

